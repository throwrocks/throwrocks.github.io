---
layout: page
comments: false
title: Customer Adapter
permalink: fm-invoices-customer-adapter
---

In FileMaker, we display data using Layouts in different View Modes; Form View, List View, and Table View. By setting a Layout to Form View, we get access to the current record in the foundset, and by setting it to List View, we get access to all the records in the foundset. To display data from the records, we insert fields or merge fields that reference the fields from the underlying table occurrence. We can visualize it like this:

![FileMaker Views](http://throw.rocks/fm-invoices/11_customer_adapter/filemaker_views.png)

In Android, we have to code the data-binding ourselves. To show a list of items, the most efficient way to implement data-binding is using an Adapter. 

#### Android Adapters

>An Adapter object acts as a bridge between an AdapterView and the underlying data for that view. The Adapter provides access to the data items. The Adapter is also responsible for making a View for each item in the data set.
>
>[Source](https://developer.android.com/reference/android/widget/Adapter.html)

Notice that an Adapter does two things. It provides access to the data items (in our case, the customer records), and is responsible for making a view for each item in the data set. We already created the "item_customer" layout. Now we have to make the Adapter create one of those layouts for each customer and populate it with the corresponding data. Just like FileMaker, when in Browse Mode, creates one Body part for each record in the foundset and populates the fields and merge fields with data from the table.

Let's visualize how the Adapter will work in our app. I included the API class calling the FileMaker Server so you can see it in context. There's still some missing pieces but we will add them later when we work in our Loader.

![Source](http://throw.rocks/fm-invoices/11_customer_adapter/customer_adapter_android_adapter.png)

#### The RecyclerView

Android has different types of views to present lists of items. For this project, we will use the `RecyclerView`.

>The RecyclerView widget is a more advanced and flexible version of ListView. This widget is a container for displaying large data sets that can be scrolled very efficiently by maintaining a limited number of views.
>
>[Source](https://developer.android.com/training/material/lists-cards.html#RecyclerView)

To use the `RecyclerView`, we need to add a few dependencies to our app. Add the following lines to your `app/build.gradle` file and then click on *Sync Now*.

```java
    compile 'com.android.support:support-v4:25.0.0'
    compile 'com.android.support:recyclerview-v7:25.0.0'
```

#### Implement the RecyclerView in `MainActivity`

In the previous section, we added several <include> tags in our `activity_main.xml` so we could test the `item_customer` layouts. We're going to remove those <include> elements and replace them with a `RecyclerView`. 

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

#### Create the CustomerAdapter class

First, create a new package to store our adapters.

Then, create a new Java class inside the new package and name it CustomerAdapter.

#### Create a ViewHolder inner class that extends RecyclerView.ViewHolder

Now we're going to add the ViewHolder inner class to our CustomerAdapter. This class will extend the RecyclerView.ViewHolder class and we will use it to hold the views from the "item_customer" layout so we can reuse (or recycle) them as the user scrolls through our list.

>A ViewHolder describes an item view and metadata about its place within the RecyclerView.
> RecyclerView.Adapter implementations should subclass ViewHolder and add fields for caching potentially expensive findViewById(int) results.
>
>[Source](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)

The ViewHolder originally was a technique developed by Android developers to improve the performance of `ListView`. The concept is to store a single reference to the individual views of your item layout, and then reuse them as the user scrolls up and down the list. The Android platform later incorporated the ViewHolder pattern into the `RecyclerView`.

Let's create the ViewHolder inner class. 

```java
public class CustomerAdapter {

    public class ViewHolder extends RecyclerView.ViewHolder {
        // TODO: Declare views
        public ViewHolder(View view) {
            super(view);
            // TODO: Initialize views
        }
    }
}
```

Later on, we will declare all the individual views of our item customer layout, that is, the name, company, and city views. And then inside the constructor, we will initialize the views by finding them on the layout by their ids.

#### Extend RecyclerView.Adapter

Now that we've implemented the ViewHolder inner class, let's our CustomerAdapter become a subclass of `RecyclerView.Adapter`.

```java
public class CustomerAdapter extends RecyclerView.Adapter<CustomerAdapter.ViewHolder> {
    //...
}
```

Now you should see some red in your class, that's Android Studio warning you that you are missing some required methods to be able to extend `RecyclerView.Adapter`. Let me show you how to figure out what methods are needed and how to include them.

#### Create the required Override methods

#### Create the constructor

Add the field mCustomers field.

```java
    private JSONArray mCustomers;

    public CustomerAdapter(JSONArray mCustomers) {
        this.mCustomers = mCustomers;
    }
```

#### The CustomerAdapter template

```java
public class CustomerAdapter extends RecyclerView.Adapter<CustomerAdapter.ViewHolder> {
 private ArrayList<Customer> mCustomers;

    public CustomerAdapter(ArrayList<Customer> mCustomers) {
        this.mCustomers = mCustomers;
    }

    public class ViewHolder extends RecyclerView.ViewHolder {
        // TODO: Declare views
        public ViewHolder(View view) {
            super(view);
            // TODO: Initialize views
        }
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {

    }

    @Override
    public int getItemCount() {
        return 0;
    }

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        return null;
    }
}
```

#### Complete the ViewHolder inner class

```java
    public class ViewHolder extends RecyclerView.ViewHolder {
        TextView companyView;
        TextView nameView;
        TextView cityView;
        JSONObject customerRecord;

        public ViewHolder(View view) {
            super(view);
            companyView = (TextView) view.findViewById(R.id.item_customer_company);
            nameView = (TextView) view.findViewById(R.id.item_customer_name);
            cityView = (TextView) view.findViewById(R.id.item_customer_city);
        }
    }


```

#### Complete onCreateViewHolder

```java
    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.item_customer, parent, false);
        return new ViewHolder(view);
    }
```

#### Complete onBindViewHolder

```java
    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        try {
            holder.customerRecord = mCustomers.getJSONObject(position);
            String company = holder.customerRecord.getString("Company");
            String firstName = holder.customerRecord.getString("First");
            String lastName = holder.customerRecord.getString("Last");
            String city = holder.customerRecord.getString("City");
            holder.companyView.setText(company);
            holder.nameView.setText(firstName + " " + lastName);
            holder.cityView.setText(city);
        } catch (JSONException e) {
            Log.e(CustomerAdapter.class.getName(), "Error: " + e);
        }
    }
```

```java
    @Override
    public int getItemCount() {
        if (mCustomers == null) {
            return 0;
        } else {
            return mCustomers.length();
        }
    }
```

#### The complete CustomerAdapter
```java
package rocks.athrow.fm_invoices.adapter;

import android.support.v7.widget.RecyclerView;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import org.json.JSONArray;
import org.json.JSONException;
import org.json.JSONObject;

import rocks.athrow.fm_invoices.R;
import rocks.athrow.fm_invoices.data.Customer;

/**
 * Created by jose on 1/22/17.
 */

public class CustomerAdapter extends RecyclerView.Adapter<CustomerAdapter.ViewHolder> {
    private JSONArray mCustomers;

    public CustomerAdapter(JSONArray mCustomers) {
        this.mCustomers = mCustomers;
    }

    public class ViewHolder extends RecyclerView.ViewHolder {
        TextView companyView;
        TextView nameView;
        TextView cityView;
        JSONObject customerRecord;

        public ViewHolder(View view) {
            super(view);
            companyView = (TextView) view.findViewById(R.id.item_customer_company);
            nameView = (TextView) view.findViewById(R.id.item_customer_name);
            cityView = (TextView) view.findViewById(R.id.item_customer_city);
        }
    }

    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        try {
            holder.customerRecord = mCustomers.getJSONObject(position);
            String company = holder.customerRecord.getString("Company");
            String firstName = holder.customerRecord.getString("First");
            String lastName = holder.customerRecord.getString("Last");
            String city = holder.customerRecord.getString("City");
            holder.companyView.setText(company);
            holder.nameView.setText(firstName + " " + lastName);
            holder.cityView.setText(city);
        } catch (JSONException e) {
            Log.e(CustomerAdapter.class.getName(), "Error: " + e);
        }
    }

    @Override
    public int getItemCount() {
        if (mCustomers == null) {
            return 0;
        } else {
            return mCustomers.length();
        }
    }

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.item_customer, parent, false);
        return new ViewHolder(view);
    }
}
```