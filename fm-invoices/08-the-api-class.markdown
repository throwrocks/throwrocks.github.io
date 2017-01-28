---
layout: page
comments: false
title: Create the API class
permalink: fm-invoices-the-api-class.html
---

In this tutorial we will create a data package that will contain all our data related classes. We will also create the APIResponse and API classes that will allow us to connect to our FileMaker Server and return a response.

#### What is a package?

>A package is a grouping of related types providing access protection and name space management. 
>
>[Source](https://docs.oracle.com/javase/tutorial/java/package/packages.html)

It's a good practice to organize related classes in their own package. It makes your code easier to navigate and to use. 

#### Create the data package

Right click on our main package, then click on *New*, and then on *Package*.

![Create a new package](http://throw.rocks/fm-invoices/08_data/data_01_create_package.png)

Name it `data` and click *Ok*.

![Name the package](http://throw.rocks/fm-invoices/08_data/data_02_create_package_name.png)

#### Create the APIResponse Class

We will use the APIResponse class to store the response from our FileMaker Server. The response will consist of two elements:

1. An HTTP status code
2. A text result

When the API call is successful, we get the standard 200 HTTP status code.

> 200 OK: Standard response for successful HTTP requests. The actual response will depend on the request method used. In a GET request, the response will contain an entity corresponding to the requested resource. In a POST request, the response will contain an entity describing or containing the result of the action.
>[Source](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#2xx_Success)

In the text result, we get the JSON response that we saw in [Host the FileMaker Server Sample File](/fm-invoices-host-filemaker-server-sample-data.html). 

When the call is unsuccessful, we get a different status code, most likely 500:

>500 Internal Server Error
A generic error message, given when an unexpected condition was encountered and no more specific message is suitable
>[Source](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes#5xx_Server_Error)

In the text result, we will get either a JSON response from RESTfm containing an error message, or some other web server message.

So every time we call the API, we will get both a status code and a text result. We will use the APIResponse class to create an object that stores these two pieces of information. The APIResponse will allow us to evaluate the response efficiently so we can proceed accordingly. It will also allow use to pass the entire response between classes. Similar to how to use the "Exit Script[]" step in FileMaker to pass Get ( ScriptResult ) to the calling activity.

Let's get started. Create a new class inside the data package.

![Create a new class](http://throw.rocks/fm-invoices/08_data/data_04_create_new_class_api.png)

Name the class `APIResponse`.

![Name APIResponse class](http://throw.rocks/fm-invoices/08_data/data_05_create_api_response.png)

The structure of this class is identical to the customer class we created in [A primer on Java classes](/fm-invoices-java-classes-primer.html). 

#### Create the fields

First, let's create the fields for this class. As I explained above, the API will return a code and a text result. You should be familiar with this by now.

```java
public final class APIResponse {
    private String responseText;
    private int responseCode;
}
```

#### Create  the Constructor

Now let's create an empty constructor. But this time, I will show you how Android Studio can help you automate this.

Click anywhere on the editor window, where the code for your APIResponse class is. Then click on the *Generate* function.

![Generate](http://throw.rocks/fm-invoices/08_data/data_06_generate.png)

Then click on Constructor.

![Generate constructor](http://throw.rocks/fm-invoices/08_data/data_07_generate_constructor.png)

And finally, click on *Select None*.

![Select constructor fields](http://throw.rocks/fm-invoices/08_data/data_08_generate_constructor_select_fields.png)

Your class will now look like this:

```java
public final class APIResponse {
    private String responseText;
    private int responseCode;

    public APIResponse() {
    }
}
```

#### Create the Getter and Setter methods

Now let's create the getter and setter methods using the *Generate* function. This time click on *Getter and Setter*.

![Generate Getter and Setter](http://throw.rocks/fm-invoices/08_data/data_09_generate_getters_setters.png)

Then select both fields and click on *Ok*.

![Select for Getter and Setter fields](http://throw.rocks/fm-invoices/08_data/data_10_getters_setters_select_fields.png)

Your class will now look like this:

```java
package rocks.athrow.fm_invoices.data;

public final class APIResponse {
    private String responseText;
    private int responseCode;

    public APIResponse() {
    }

    public String getResponseText() {
        return responseText;
    }

    public void setResponseText(String responseText) {
        this.responseText = responseText;
    }

    public int getResponseCode() {
        return responseCode;
    }

    public void setResponseCode(int responseCode) {
        this.responseCode = responseCode;
    }
}

```
This is all we need. Let's now create the API class. 

#### Create the API class

The API class is a different type of class. You can think of it as a Utility class. We will not use it to create objects. We will use it to hold our different API queries, to establish a connection to the FileMaker Server, and to create and return an APIResponse object that holds the server response.

First, declare the class. Then, create two fields: API_HOST and API_KEY. Finally, create a constructor that throws an AssertionError. Since this is utility class, we don't allow creating objects it.


```java
package rocks.athrow.fm_invoices.data;

public final class API {
    private static final String API_HOST = "";
    private static final String API_KEY = "";

    private API() {
        throw new AssertionError("No API instances for you!");
    }
}
```
Fill in the values for your host and key. For example:

```java
    private static final String API_HOST = "http://myhost.com/RESTfm/sample_data/";
    private static final String API_KEY = "?RFMkey=MyAPIKey";
```

#### Add the httpConnect method

Now, let's add a method that handles connecting to our host and returning the response. 

First, declare it as `private static`. Private, because only the API class will use it. And static, because it belongs to the class, not to an object of the class. Next, declare the return type as `APIResponse`. Which means that this method will return an `APIResponse` object that holds the server response. Then, name the method `httpConnect`. And finally, declare a `String` argument `queryUrl` which will be the complete URL that we will use to query our API (for example, http://throw.rocks/RESTfm/sample_date/layout/api_customers.json?RFMmax=1&RFMkey=71c717c4-d8e3-485f-a815-f5928f1f7a3e)

```java

    private static APIResponse httpConnect(String queryURL) {
        APIResponse apiResponse = new APIResponse();
        HttpURLConnection urlConnection = null;
        BufferedReader reader = null;
        try {
            Uri builtUri = Uri.parse(queryURL).buildUpon().build();
            URL url = new URL(builtUri.toString());
            urlConnection = (HttpURLConnection) url.openConnection();
            urlConnection.setRequestMethod("GET");
            urlConnection.connect();
            apiResponse.setResponseCode(urlConnection.getResponseCode());
            InputStream inputStream = urlConnection.getInputStream();
            StringBuilder buffer = new StringBuilder();
            if (inputStream == null) {
                return apiResponse;
            }
            reader = new BufferedReader(new InputStreamReader(inputStream));
            String line;
            while ((line = reader.readLine()) != null) {
                buffer.append(line);
            }
            if (buffer.length() == 0) {
                return apiResponse;
            }
            apiResponse.setResponseText(buffer.toString());
        } catch (IOException v) {
            apiResponse.setResponseText(v.toString());
            return apiResponse;
        } finally {
            if (urlConnection != null) {
                urlConnection.disconnect();
            }
            if (reader != null) {
                try {
                    reader.close();
                } catch (final IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return apiResponse;
    }
```

Don't worry about the nuts and bolts of it. This is a generic method use to query an HTTP host.

#### getCustomers()

Now let's create the first method that will query our API. This method will get our customer records. It will basically do three things:

1. Build the URL (`http://myhost.com/RESTfm/sample_data/layout/customers.json?RFMkey=MyAPIKey&RFMmax=0`)
2. Pass it to httpConnect
3. Return the APIResponse object from httpConnect

```java
    public static APIResponse getCustomers(int maxResults) {
        String url = API_HOST + "layout/customers.json" + API_KEY + "&RFMmax=" + maxResults;
        return httpConnect(url);
    }
```

#### Congratulations!

We now have a class with methods to query our FileMaker Server and return the response. Let's test it.

<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-unit-test-api.html">Test the API with a Unit test</a>