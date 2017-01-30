---
layout: page
comments: false
title: Data Loader
permalink: fm-invoices-data-loader
---

#### The Data Loader Process

[![Data Loader Process][1]][1]

[1]: http://throw.rocks/fm-invoices/13_data_loader/data_loader_01_process.png


#### Create the DataLoader Class


```java
public class DataLoader extends AsyncTaskLoader<JSONArray> {
    public DataLoader(Context context) {
        super(context);
    }

    @Override
    public JSONArray loadInBackground() {
        return null;
    }
}
```

#### loadInBackground

```java
    @Override
    public JSONArray loadInBackground() {
        APIResponse apiResponse;
        apiResponse = API.getCustomers(0);
        JSONArray jsonArray = null;
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