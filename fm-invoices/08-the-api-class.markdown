---
layout: page
comments: false
title: Create the API class
permalink: fm-invoices-the-api-class.html
---

#### Create the data package

![Create a new package](http://throw.rocks/fm-invoices/08_data/data_01_create_package.png)

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

![Create a new class](http://throw.rocks/fm-invoices/08_data/data_04_create_new_class_api)

Name the class APIResponse.

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


#### Create the API class

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

```java
    private static final String API_HOST = "http://myhost.com/RESTfm/sample_data/";
    private static final String API_KEY = "?RFMkey=MyAPIKey";
```

#### Add the httpConnect method

Add the following method to your API class. If you set up the Auto Import correctly, you will see.

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

#### Create your first API method

The URI will look like this:

`http://myhost.com/RESTfm/sample_data/layout/customers.json?RFMkey=MyAPIKey&RFMmax=0`

http://www.restfm.com/restfm-manual/web-api-reference-documentation/uri-restfmdatabaselayoutlayout/read-get


```java
    public static APIResponse getCustomers(int maxResults) {
        String url = API_HOST + "layout/customers.json" + API_KEY + "&RFMmax=" + maxResults;
        return httpConnect(url);
    }
```




