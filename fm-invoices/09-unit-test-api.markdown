---
layout: page
comments: false
title: Test the API with a Unit Test
permalink: fm-invoices-unit-test-api.html
---

It's time to test our API and APIResponse classes. To do this, we're going to write some Unit Tests using the Roboelectric library.

>Robolectric is a unit test framework that de-fangs the Android SDK jar so you can test-drive the development of your Android app.

We're using this framework because Unit Tests are meant to run in isolation. That means we shouldn't have to run the entire Android app just to test one class. However, since our code has dependencies on the Android SDK, the unit test will fail when they can't find the required objects. For the unit test to work, we would need to "mock" the external dependencies.

>Mock is a method/object that simulates the behavior of a real method/object in controlled ways.
>
> [Source](http://stackoverflow.com/questions/2665812/what-is-mocking)

Roboelectric takes care of mocking the Android objects by referring our calls to shadow objects that simulate them. 

### Add Robolectric

1. Open the /app/build/build.gradle file

2. Add the following line `testCompile "org.robolectric:robolectric:3.2.2"`

3. Click on *Sync Now*

![Add Roboelectric](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_01_add_roboelectric.png)

### Rename the ExampleUnitTest

Android provides a default location to store the unit tests. When you create a new project, Android Studio creates this location and a sample unit test. We're going to rename the sample file and write our own tests. 


1. Right click on the ExampleUnitTest under the (test) package.
2. Click on the *Refactor* menu
3. Click on *Rename* function 

![Rename the sample unit test](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_02_rename_example_unit_text.png)

Rename it to `APIUnitTest` and click on *Refactor*.

![Rename the sample unit test](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_03_rename.png)

### Copy the starter code

Now replace the contents of the example unit test with the following empty class:

```java
package rocks.athrow.fm_invoices;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.robolectric.Robolectric;
import org.robolectric.RobolectricTestRunner;
import org.robolectric.annotation.Config;

import rocks.athrow.fm_invoices.data.API;
import rocks.athrow.fm_invoices.data.APIResponse;

import static org.junit.Assert.*;

@Config(manifest = Config.NONE)
@RunWith(RobolectricTestRunner.class)
public class APIUnitTest extends Robolectric {
  // Write your tests below this line
}
```
Notice that our class "extends" `Roboelectric`. That means that our class is a subclass of `Roboelectric`. We will learn more about subclassing later when we start writing Android code. But for now, all we need to know is that `Roboelectric` class will be doing a lot of work behind the scenes to allow us to run our tests.

#### Our first test: getCustomers()

The first test will simply get all the customer records from our API, and it will check if that HTTP status code equals 200 (OK). The test will pass if the server response was succesfull. 

Add the following method to your class:

```java
  @Test
    public void getCustomers() throws Exception {
        APIResponse apiResponse;
        apiResponse = API.getCustomers(0);
        assertTrue(apiResponse.getResponseCode() == 200);
    }
```

Let's go over the test. 

First, we declare it as public because it is required by the test framework. 

Second, we define the return type as void, because the method doesn't return anything. Test methods (annotated with @Test) are supposed to either pass or fail according to an assertion (something is true or false).They don't need to return anything.

Third, we create an APIResponse object. The name of our APIResponse object is apiResponse. Remember, APIResponse is the blueprint, and apiResponse is the object. 

Fourth, we set the apiResponse object to the result of the getCustomer() method from our API class. Since getCustomer() is a static method, we don't need to create an instance of API. We just call it directly from the class. We pass 0 because we want all customer records.

Finally, we use an assertTrue statement to evaluate if the responseCode from the server, now stored in the apiResponse object, and accesible via the getResponse() getter method, equals 200.

Let's run the test and find out.

1. Right click on the APIUnitTest file.
2. Click on *Run APIUnitTest*

![Rename the sample unit test](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_04_run_first_test.png)

### If the test passes

If the test passed, you should see something like this.

![Rename the sample unit test](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_05_test_passed.png)

### If the test fails

If the test fails, you will need to debug the issue. First, add the following lines before the assertion.

```java
System.out.println("Response Code: " + apiResponse.getResponseCode());
System.out.println("Response Text: " + apiResponse.getResponseText());
```

Those lines will print the responseCode and the responseText to the console so you can get more information on the issue. Run the test again and examine the result.

For example, if you get a 403 (Forbidden), that probably means you have a wrong API key, or a password mismatch between the RESTfm.ini.php file and your FileMaker account.
![Error 403](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_06_test_failed_403.png)

If you get a 404 (Not Found), that probably means your URL syntax is wrong, or you have a typo.
![Error 404](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_07_test_failed_404.png)


#### Getting the result as JSON


Here is the next test:

```java
    @Test
    public void getCustomersJSONArray() {
        APIResponse apiResponse;
        apiResponse = API.getCustomers(0);
        int responseCode = apiResponse.getResponseCode();
        String responseText = apiResponse.getResponseText();
        if (responseCode != 200) {
            System.out.println(responseCode);
            System.out.println(responseText);
            assertFalse(true);
        } else {
            try {
                JSONObject jsonObject = new JSONObject(responseText);
                JSONArray jsonArray = jsonObject.getJSONArray("data");
                assertTrue(jsonArray.length() > 0);
            } catch (JSONException e) {
                assertFalse(true);
            }
        }
    }
```

Let's go over it. It starts just like the previous test. But this time, we'll use the responseCode and responseText multiple times, so we're setting them in variables. 

If the responseCode does not equal 200, the test will fail. If it equals 200, it will proceed. 

We execute the next part of the test inside a try/catch block. The try/catch block is required because both the `JSONObject(String json)` constructor and the `getJSONArray()` method throw a `JSONException`. 

If a constructor or method throws an exception we have to catch it. Otherwise, the program will not run because of an uncaught exception. But don't worry, Android Studio will warn you before you even run the program.

 ![Uncaught Exception](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_08_unhandled_exception.png)

You can also consult the Android Developers documentation for more information. 

For example: [https://developer.android.com/reference/org/json/JSONObject.html#JSONObject()](https://developer.android.com/reference/org/json/JSONObject.html#JSONObject())

 ![Consult the documentation](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_09_documentation.png)

After creating a JSONObject from the API response text, we run the getJSONArray() method on it to get the "data" portion of the response and convert it to a `JSONArray` object. Our test will pass if the resulting jsonArray object has a length over 0.

Let's run the test. Instead of running it by right clicking on the APIUnitTest file, let's run it clicking it on the test itself. By doing so, we get the option to run only that test. If we run it from the file, it will run all tests. 

 ![Run the second test](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_10_run_second_test.png)


#### Exploring the JSON 

Now that we have successfully created a JSONArray from the API response, it's time to explore the data. 