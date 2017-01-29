---
layout: page
comments: false
title: The Customer Adapter
permalink: fm-invoices-customer-adapter
---

In FileMaker, we display data using Layouts in different View Modes; Form View, List View, and Table View. By setting a Layout to Form View, we get access to the current record in the foundset, and by setting it to List View, we get access to all the records in the foundset. To display data from the records, we insert fields or merge fields that reference the fields from the underlying table occurrence. We can visualize it like this:

[![FileMaker Views][1]][1]

[1]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_01_filemaker_views.png

In Android, we have to code the data-binding ourselves. To show a list of items, the most efficient way to implement data-binding is using an Adapter. 

#### Android Adapters

>An Adapter object acts as a bridge between an AdapterView and the underlying data for that view. The Adapter provides access to the data items. The Adapter is also responsible for making a View for each item in the data set.
>
>[Source](https://developer.android.com/reference/android/widget/Adapter.html)

Notice that an Adapter does two things. It provides access to the data items (in our case, the customer records), and is responsible for making a view for each item in the data set. We already created the "item_customer" layout. Now we have to make the Adapter create one of those layouts for each customer and populate it with the corresponding data. Just like FileMaker, when in Browse Mode, creates one Body part for each record in the foundset and populates the fields and merge fields with data from the table.

Let's visualize how the Adapter will work in our app. I included the API class calling the FileMaker Server so you can see it in context. There's still some missing pieces but we will add them later when we work in our Loader.

[![Android Adapter][2]][2]

[2]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_02_android_adapter.png

#### The RecyclerView

Android has different types of views to present lists of items. For this project, we will use the `RecyclerView`.

>The RecyclerView widget is a more advanced and flexible version of ListView. This widget is a container for displaying large data sets that can be scrolled very efficiently by maintaining a limited number of views.
>
>[Source](https://developer.android.com/training/material/lists-cards.html#RecyclerView)

To use the `RecyclerView`, we need to add a few dependencies to our app. Add the following lines to your `app/build.gradle` file and then click on *Sync Now*.

```groovy
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

First, create a new package to store the adapters.

[![Create new package][3]][3]

[3]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_03_create_new_package.png

Name it `adapter`.

[![Name package][4]][4]

[4]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_04_name_package.png

Then, create a new Java class inside the `data` package and name it `CustomerAdapter`.

[![New Adapter class][5]][5]

[5]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_05_new_class.png

#### Create a ViewHolder inner class that extends RecyclerView.ViewHolder

Now we're going to add the `ViewHolder` inner class to our `CustomerAdapter`. This class will extend the `RecyclerView.ViewHolder` class and we will use it to hold the views from the "item_customer" layout so we can reuse (or recycle) them as the user scrolls through our list.

>A ViewHolder describes an item view and metadata about its place within the RecyclerView.
> RecyclerView.Adapter implementations should subclass ViewHolder and add fields for caching potentially expensive findViewById(int) results.
>
>[Source](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html)

Originally, Android developers created the ViewHolder design pattern to improve the performance of `ListView`. The idea is to store a single reference to the individual views of your layout, and then reuse them as the user scrolls up and down the list instead of creating them again. The Android platform later incorporated the `ViewHolder` pattern into the `RecyclerView`.

Let's create the stub for the `ViewHolder` inner class. 

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

We will fill in the TODOs shortly. First, let's set up the entire Adapter class.

#### Extend RecyclerView.Adapter

Now that we've implemented the `ViewHolder inner` class, we can extend the `CustomerAdapter` to become a subclass of `RecyclerView.Adapter`.

```java
public class CustomerAdapter extends RecyclerView.Adapter<CustomerAdapter.ViewHolder> {
    //...
}
```

Now you should see some red in your class, that's Android Studio warning you that you are missing some required methods to be able to extend `RecyclerView.Adapter`. Let me show you how to figure out what methods are needed and how to include them.

#### Create the required Override methods

Because we are subclassing `RecyclerView.Adapter`, we need to implemenet certain required methods.

[![Required OnBindViewHolder][6]][6]
    
[6]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_06_required_onbindviewholder.png


[![Generate][7]][7]

[7]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_07_generate.png


[![Generate OnBindViewHolder][8]][8]

[8]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_08_override_methods.png


[![Required GetItemCount][9]][9](
    
[9]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_10_required_getitemcount.png

[![Generate GetItemCount][10]][10]

[10]: http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_11_generate_getitemcount.png.png

![Required OnCreateViewHolder](http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_12_required_oncreateviewholder.png)

![Generate OnCreateViewHolder](http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_13_generate_oncreateviewholder.png.png)

![All Required Methods](http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_14_all_required_methods.png.png)

#### Create the constructor

Add the field mCustomers field.

![Generate Constructor](http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_15_constructor.png)

```java
    public CustomerAdapter(JSONArray mCustomers) {
        this.mCustomers = mCustomers;
    }
```

#### The CustomerAdapter template

Now we have all the components that we need to implement the adapter. Your class should look like this.

```java
public class CustomerAdapter extends RecyclerView.Adapter<CustomerAdapter.ViewHolder> {
   private JSONArray mCustomers;

   public CustomerAdapter(JSONArray customers) {
        this.mCustomers = customers;
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

Now let's flesh out `ViewHolder` class and the required methods and explain what their purpose is.

#### Complete the ViewHolder inner class

First, we declare the class fields. We need fields for our views, and a field for the data. The three views that we need to bind the data with are `TextView`, as defined in our `item_customer.xml` layout. So we declare those first. Then, we declare a `JSONObject` field to hold a customer record.


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

Finally, we initialize the views in the constructor. Notice that the constructor accepts a `View` object. That `View` object will be an instance of our `item_customer` layout. To initialize our view variables, we use the `findviewById()` method on the `View` object. 

>##### findViewById()
>
>Look for a child view with the given id. If this view has the given id, return this view.
>
>[Source](https://developer.android.com/reference/android/view/View.html#findViewById(int))

Also, notice the usage of `R.id`.

>##### R.id
>
>When your application is compiled, aapt generates the R class, which contains resource IDs for all the resources in your res/ directory.
>
>[Source](https://developer.android.com/guide/topics/resources/accessing-resources.html)

In other words, all the resource ids, like your view ids, are stored in a generated class called `R`. You can reference those ids through this class.

Now, let's look at the `ViewHolder` and the `item_customer` layout side by side so you can see the relationship.

![ViewHolder references our Layout](http://throw.rocks/fm-invoices/12_customer_adapter/customer_adapter_16_data_binding.png)

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

#### Congratulations!


<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-data-loader.html">The Customer Data Loader</a>