---
layout: page
comments: false
title: Data Loader
permalink: fm-invoices-data-loader
---

To implement the `Adapter`, we need to fetch the data from our FileMaker API like we did in the Unit tests. However, we can't simply call `API.getCustomers()` from `MainActivity`.

>By default, Android 3.0 (API level 11) and higher requires you to perform network operations on a thread other than the main UI thread; if you don't, a NetworkOnMainThreadException is thrown.
>
>[Source](https://developer.android.com/training/basics/network-ops/connecting.html)

##### What is the UI thread?

> ... the Android system starts a new Linux process for the application with a single thread of execution. By default, all components of the same application run in the same process and thread (called the "main" thread). 
>
>[Source](https://developer.android.com/guide/components/processes-and-threads.html#Processes)

This is similar to how FileMaker works, where we only have one thread for all the applications running in FileMaker. We notice it when we run a script or perform a complex find, because the application does not respond until the operation is complete. With the new FileMaker *Perform Script on Server* step, we can perform scripts in the server, giving us an alternative for running long operations and not blocking the "UI thread". But technically, the server script is running on the server, not on the client in a separate thread.

However, in Android, we have the option to run operations in a separate thread inside the same app, which allows us to keep the UI responsive. When the background operation is complete, it can communicate back to the UI thread by passing objects and other information. To get the data from our FileMaker API and pass it to the adapter, we're going to implement a `DataLoader` class by extending `AyncTaskLoader`, which is an Android class designed to facilitiate performing operations in a background thread, and publishing the results to the UI thread.

#### The Data Loader Process

Let's visualize how the process will work. When `MainActivity` runs, it will initialize our `DataLoader`, which will execute in a background thread. The `DataLoader` will then call our `API.getCustomers()` method, and pass the results as a `JSONArray` object back to `MainActivity`. Finally, `MainActivity` will create an `Adapter` object, pass the `JSONArray` to it, and set the `Adapter` on our `RecyclerView`, which will then populate our Customer List layout with the data.

[![Data Loader Process][1]][1]

[1]: http://throw.rocks/fm-invoices/13_data_loader/data_loader_01_process.png


#### Create the DataLoader Class

Create a new class inside the data package and name it `DataLoader`. Edit the class to make it "extend" `AsyncTaskLoader<>`. Inside the angle brackets, type `JSONArray`, which is the type of object that our `DataLoader` will return.

```java
public class DataLoader extends AsyncTaskLoader<JSONArray> {

}
```

Now you should see Android Studio underlining the class declaration in red, and warning that you have to implement the `loadInBackground()` method. Use the *Generate* function to add it. It will look like this:

```java
    @Override
    public JSONArray loadInBackground() {
        return null;
    }
}
```

Finally, we need to add a constructor because we will need to instantiate (create an object of) our `DataLoader`.  Let's use the *Generate* function. It will look like this:

```java
    public DataLoader(Context context) {
        super(context);
    }
```

#### Complete the loadInBackground() method

This is where all the action happens.

> #### LoaderManager()
> 
>Called on a worker thread to perform the actual load and to return the result of the load operation.
>
>[Source](AsyncTaskLoader.html#loadInBackground())

Fortunately, when we wrote the Unit tests we wrote the code that we need to implement here. Let's walk through it.

1. Create an `APIResponse` object to hold the API result, and a `JSONArray` object to hold and return the converted result.

2. Set the `APIResponse` object to the result of `APIGetCustomers(0)`. 

3. Create and initialize the `responseText` variable with the result of `apiResponse.getResponseText()`, and create and initialize the `responseCode` variable with the result of `apiResponse.getResponseCode()`.

4. If the responseCode equals 200 (OK), we `try` to create a `JSONObject` from our `responseText`, and then set the `JSONArray` object from the "data" node.

5. Finally, we return the `JSONArray` object. If there was an error, the object will be null. 

```java
    @Override
    public JSONArray loadInBackground() {
        APIResponse apiResponse;
        JSONArray jsonArray = null;
        apiResponse = API.getCustomers(0);
        int responseCode = apiResponse.getResponseCode();
        String responseText = apiResponse.getResponseText();
        if (responseCode == 200) {
            try {
                JSONObject jsonObject = new JSONObject(responseText);
                jsonArray = jsonObject.getJSONArray("data");
            } catch (JSONException e) {
                Log.e(ContactsContract.Data.class.getName(), "Error: " + e);
            }
        }
        return jsonArray;
    }
```

#### Implement LoaderManager

```java
public class MainActivity extends AppCompatActivity
        implements LoaderManager.LoaderCallbacks<List<String>>{
//..
}
```

```java
  @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        getLoaderManager().initLoader(1, null, MainActivity.this).forceLoad();
    }
```

```java
    @Override
    public Loader<JSONArray> onCreateLoader(int id, Bundle args) {
        return new DataLoader(this);
    }
```

```java
    @Override
    public void onLoadFinished(Loader<JSONArray> loader, JSONArray data) {
        mAdapter = new CustomerAdapter(data);
        RecyclerView customerList = (RecyclerView) findViewById(R.id.customers_list);
        customerList.setAdapter(mAdapter);
    }
```



#### MainActivity RecyclerView

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    tools:context="rocks.athrow.fm_invoices.MainActivity">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/customers_list"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layoutManager="LinearLayoutManager" />
</LinearLayout>
```

#### Congratulations!


<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-new-activity-via-intent.html">New Activity via Intent</a>