---
layout: page
comments: false
title: Test the API with a Unit Test
permalink: fm-invoices-unit-test-api.html
---

To test our API and APIResponse classes, we're going to write some unit tests using the Roboelectric library.

>Robolectric is a unit test framework that de-fangs the Android SDK jar so you can test-drive the development of your Android app.

We're using this framework because unit tests are supposed to run in isolation. That means, we shouldn't have to run the entire Android app just to test one class. However, since our code has dependencies on the Android SDK, the unit test will fail when it can't find the required Android methods and objects. For the unit test to work, we need to "mock" the external dependencies.

>Mock is a method/object that simulates the behavior of a real method/object in controlled ways.
>
> [Source](http://stackoverflow.com/questions/2665812/what-is-mocking)

Roboelectric takes care of mocking the Android methods and objects by referring our calls to shadow objects that simulate them. 

#### Add Robolectric

1. Open the `/app/build/build.gradle` file

2. Add the following line `testCompile "org.robolectric:robolectric:3.2.2"`

3. Click on *Sync Now*

![Add Roboelectric](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_01_add_roboelectric.png)

#### Rename the ExampleUnitTest

Android provides a default package to store the unit tests. When you create a new project, Android Studio creates the test package with a sample unit test. We're going to rename the sample file and write our own tests in it.

1. Right click on the `ExampleUnitTest` file under the (test) package.
2. Click on the *Refactor* menu
3. Click on the *Rename* function 

![Rename the sample unit test](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_02_rename_example_unit_text.png)

Rename it to `APIUnitTest` and click on *Refactor*.

![Rename the sample unit test](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_03_rename.png)

#### Copy the starter code

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
Notice that our class "extends" `Roboelectric`. That means that our class is a subclass of `Roboelectric`. We will learn more about subclassing later when we start writing Android code. But for now, all we need to know is that the `Roboelectric` class will work behind the scenes to create the shadow Android objects and let us run our code as if the Android app was running.

#### Our first test: getCustomers()

The first test will simply get all the customer records from our API, and it will check if the HTTP status code equals 200 (OK). The test will pass if the server response was succesfull. 

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

First, we declare it as `public` because it is required by the test framework. 

Second, we define the return type as `void` because the method doesn't return anything. Test methods (annotated with @Test) are supposed to either pass or fail according to an assertion (something that is true or false). They don't need to return anything.

Third, we create an `APIResponse` object. The name of our `APIResponse` object is `apiResponse`. Remember, `APIResponse` is the blueprint, and `apiResponse` is the object. 

Fourth, we set the `apiResponse` object to the result of the `getCustomer()` method from our `API` class. Since `getCustomer()` is a `static` method, we don't need to create an instance of `API`. We just call it directly from the `API` class. We pass 0 as an argument because we want all the customer records.

Finally, we use an `assertTrue` statement to evaluate if the `responseCode` from the server, now stored in the `apiResponse` object, and accesible via the `getResponse()` getter method, equals 200.

Let's run the test and find out.

1. Right click on the `APIUnitTest` file.
2. Click on *Run APIUnitTest*

![Run the first test](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_04_run_first_test.png)

#### If the test passes

If the test passes, you should see a lot of green, like this.

![Test passes](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_05_test_passed.png)

#### If the test fails

If the test fails, you will see a lot of red, and you will to need to debug the issue. First, add the following lines before the assertion.

```java
System.out.println("Response Code: " + apiResponse.getResponseCode());
System.out.println("Response Text: " + apiResponse.getResponseText());
```

These lines will print the `responseCode` and the `responseText` to the console so you can get more information on the issue. Run the test again and examine the result.

For example, if you get a 403 (Forbidden), that probably means you have the wrong API key, or a password mismatch between the RESTfm.ini.php file and your FileMaker account.
![Error 403](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_06_test_failed_403.png)

If you get a 404 (Not Found), that probably means your URL syntax is wrong, or you have a typo.
![Error 404](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_07_test_failed_404.png)


#### Getting the result as JSON

Now that we can successfully query our API, let's convert the text result to a JSON object. Here is the next test:

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

Let's go over it. It starts just like the previous test. But this time, we will use the `responseCode` and `responseText` multiple times, so we set them in variables.

Then, we write an `If` statement, so if the `responseCode` does not equal 200, the test fails. If it equals 200, it proceeds.

We execute the next part inside a try/catch block. The try/catch block is required because both the `JSONObject(String json)` constructor and the `getJSONArray()` method throw a `JSONException`. 

If a constructor or method throws an exception we have to catch it. Otherwise, the program does not run and the compiler complaints about the uncaught exception. But don't worry, Android Studio will warn you about uncauugh exceptions before you even run the program.

 ![Uncaught Exception](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_08_unhandled_exception.png)

You can also consult the Android Developers documentation for more information on the type of exceptions that a constructor or method throws, if any.

For example: [https://developer.android.com/reference/org/json/JSONObject.html#JSONObject()](https://developer.android.com/reference/org/json/JSONObject.html#JSONObject())

 ![Consult the documentation](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_09_documentation.png)

After creating a `JSONObject` from the `API` response text (jsonObject), we run the `getJSONArray()` method on it to get the "data" portion of the response and convert it to a `JSONArray` object. Our test will pass if the resulting `jsonArray` object has a length over 0.

Let's run the test. Instead of running it by right clicking on the `APIUnitTest` file (which would run all tests in the class), let's run it by clicking on the test itself. By doing so, we get the option to only run only that test.

 ![Run the second test](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_10_run_second_test.png)


#### Exploring the JSON 

Now that we have successfully created a `JSONArray` from the `API` response, it's time to explore the data. Let's run the test again, but this time, we will run it with the *Debugger* and use the *Evaluate Expression* window to further explore the data. These two features should be familiar, they are similar to the *Script Debugger* and the *Data Viewer* in FileMaker.

First, let's set a breakpoint on the assertion, to make the execution of the code pause when it reaches it. 

![Set a breakpoint](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_11_set_breakpoint.png)

Then, let's right click on the test and click on "Debug getCustomersJSONArray()" to run the test in debug mode.

![Run the test with the debugger](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_12_debug_test.png)

The test will start running and pause at the breakpoint. You should see the following output in the *Debugger* pane. Take some time to explore the variables, and then click on the *Evaluate Expression* button.

![The debug pange](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_13_debug_pane.png)

The *Evaluate Expression* window will pop up. This is very similar to the *Data Viewer* in FileMaker. Type "jsonArray" in the expression field and click on *Evaluate*.

![Evaluate Expression](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_14_evaluate_expression.png)

Take some time to explore the `jsonArray`. Click on records and the nodes so you can see the data from your FileMaker file.

#### Parsing the JSONArray

To get a record from the array you can to use the `getJSONObject(int position)` method by passing the position of the object that you want to get. This is similar to using the getValue() function in FileMaker. The main difference is that in Java, the first position of an array is 0, not 1.

So let's get the first customer record, type `jsonArray.getJSONObject(0);` and click on *Evaluate*.

![getJsonObject](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_15_getjsonobject.png)

Now, let's get some specific values from the customer object by using a `get` method according to the type of value that we are getting. Technically, all the values in our JSON are String. But we can get them as `int` or `Date` or any other type if we need to. 

For example, we know the "CUSTOMER ID MATCH FIELD" value is a number, so we get it using the `getInt()` method. Like this:

```java
jsonArray.getJSONObject(0).getInt("CUSTOMER ID MATCH FIELD")
```

![getInt](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_16_get_id.png)

And we can get the "Company" value with the `getString()` method. Like this:

```java
jsonArray.getJSONObject(0).getString("Company)
```

![getCompany](http://throw.rocks/fm-invoices/09_unit_test_api/unit_test_api_17_get_company.png)

Now that we know how to parse the JSON, let's write a test that validates some of the fields.

```java
    @Test
    public void validateFirstCustomerRecord() {
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
                JSONObject customerRecord = jsonArray.getJSONObject(0);
                int id = customerRecord.getInt("CUSTOMER ID MATCH FIELD");
                String company = customerRecord.getString("Company");
                assertFalse(id != 0);
                assertFalse(company.isEmpty());
            } catch (JSONException e) {
                assertFalse(true);
            }
        }
    }
```

Notice we're creating a `JSONObject` named `customerRecord` from the first record in our array (0), and then we are setting the `id` and the `company` variables using the corresponding get methods (`getInt()` and `getString()`). Then, we write the assertions to make our test pass if the id does not equal 0 and if the company name is not empty.

Let's run the test. If you get any assertion errors then use the *Debugger* and the *Evaluate Expression* window to examine the data.

#### Loop through the JSONArray

Finally, let's write a test that loops through the array. This is crucial because once we receive the JSON from FilMaker we will need to loop through it to get the individual records. The following test introduces a `for` loop to perform the assertions on all the customer records.

```java
    @Test
    public void validateAllCustomerRecords() {
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
                int countRecords = jsonArray.length();
                assertTrue(countRecords > 0);
                for (int i = 0; i < countRecords; i++) {
                    JSONObject customerRecord = jsonArray.getJSONObject(0);
                    int id = customerRecord.getInt("CUSTOMER ID MATCH FIELD");
                    String company = customerRecord.getString("Company");
                    assertFalse(id != 0);
                    assertFalse(company.isEmpty());
                }
            } catch (JSONException e) {
                assertFalse(true);
            }
        }
    }
```

Let's go over it. This time we are setting the `countRecords` variable to the result of `jsonArray.length()`. You will use this variable in a loop to know when to exit. This is similar to how you set a $count and $counter variables in FileMaker to exit a Loop using the `Exit Loop If[]` statement. We add an assertion to make sure that our count is over 0. Then we go into the `for` loop to evaluate the values for each individual record. For your reference, below is the basic structure of a [for loop](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/for.html):

```java
for (initialization; termination; increment) {
    statement(s)
}
```

This is how we will use it:

```java
 for (int i = 0; i < countRecords; i++) {
     // Our statements
}
```

We start by declaring the `i` (counter) variable as 0. Then, as long as `i` is less than `countRecords`, we will execute the statements. Finally, we will increment the `i` variable by 1, using the `++` operator, which is the same as writing `i = i + 1`.

Let's run the test now. If you get get any assertion errors, that means that one of your records either doesn't have an id, or the company name is missing.

#### Congratulations!

You learned how to write unit tests, and how to call your `API` class and parse the JSON response. Soon we will soon use the code in these tests to call the `API` from our Android app and populate a list of customers. But first, let's create an emulator so we can test the customer list layout when we start building it.

<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-create-emulator.html">Create an Emulator</a>