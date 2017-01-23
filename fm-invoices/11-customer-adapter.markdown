---
layout: page
comments: false
title: Customer Adapter
permalink: fm-invoices-customer-adapter
---

#### Add the support library

```java
    compile 'com.android.support:support-v4:25.0.0'
    compile 'com.android.support:recyclerview-v7:25.0.0'
```

#### Create the CustomerAdapter class

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
#### Extend RecyclerView.Adapter

```java
public class CustomerAdapter extends RecyclerView.Adapter<CustomerAdapter.ViewHolder> {
    //...
}
```

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

#### The ViewHolder class

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

#### onCreateViewHolder

```java
    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext())
                .inflate(R.layout.item_customer, parent, false);
        return new ViewHolder(view);
    }
```
#### onBindViewHolder
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