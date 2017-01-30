---
layout: page
comments: false
title: New Activity via Intent
permalink: fm-invoices-new-activity-via-intent
---

#### Create the activity Package

#### Rename MainActivity to CustomersListActivity

[![Rename MainActivity][1]][1]

[1]: http://throw.rocks/fm-invoices/14_new_activity/new_activity_01_rename_mainactivity_file.png

[![Rename file][2]][2]

[2]: http://throw.rocks/fm-invoices/14_new_activity/new_activity_02_rename_file.png

[![Rename class][3]][3]

[3]: http://throw.rocks/fm-invoices/14_new_activity/new_activity_03_rename_class.png

[![Rename in manifest][4]][4]

[4]: http://throw.rocks/fm-invoices/14_new_activity/new_activity_04_rename_in_manifest.png

#### Move CustomersListActivity to the activity Package

#### Create the CustomerDetailsActivity

```java
public class CustomerDetailsActivity extends AppCompatActivity {
   
}
```

#### Add CustomerDetailsActivity to AndroidManifest.xml

```xml
   <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity android:name=".activity.CustomersListActivity">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
        <activity
            android:name=".activity.CustomerDetailsActivity"
            android:label="Customer Details"
            android:parentActivityName=".activity.CustomersListActivity" />
    </application>
```

#### Rename activity_main.xml to activity_customers_list.xml

[![Rename activity_main][5]][5]

[5]: http://throw.rocks/fm-invoices/14_new_activity/new_activity_05_rename_customers_list.png

#### Add an id to the customer item

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/item_customer_row"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal"
    android:padding="16dp">
    ...
```

#### Create the activity_customer_details.xml layout

#### Generate the onCreate() method on the CustomerDetailsActivity

>##### setContentView (View view)
>
>Set the activity content to an explicit view. This view is placed directly into the activity's view hierarchy. It can itself be a complex view hierarchy. 
>
>[Source](https://developer.android.com/reference/android/app/Activity.html#setContentView(android.view.View))



```java
public class CustomerDetailsActivity extends AppCompatActivity {
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_customer_details);
    }
}
```

#### Starting new activities via intent



##### Adding context to the Adapter constructor

We need context to start a new activity.

>#####Context
>
>Interface to global information about an application environment. This is an abstract class whose implementation is provided by the Android system. It allows access to application-specific resources and classes, as well as up-calls for application-level operations such as launching activities, broadcasting and receiving intents, etc.
>
>[Source](https://developer.android.com/reference/android/content/Context.html)

Add to the constructor.

```java
    private Context mContext;
    private JSONArray mCustomers;

    public CustomerAdapter(Context mContext, JSONArray mCustomers) {
        this.mContext = mContext;
        this.mCustomers = mCustomers;
    }
```

Modify usage of the constructor.

```java
    @Override
    public void onLoadFinished(Loader<JSONArray> loader, JSONArray data) {
        mAdapter = new CustomerAdapter(getApplicationContext(), data);
        RecyclerView customerList = (RecyclerView) findViewById(R.id.customers_list);
        customerList.setAdapter(mAdapter);
    }
```

#### Add the customer row the viewholder

```java
  public class ViewHolder extends RecyclerView.ViewHolder {
        LinearLayout customerRow;
        TextView companyView;
        TextView nameView;
        TextView cityView;
        JSONObject customerRecord;

        public ViewHolder(View view) {
            super(view);
            customerRow = (LinearLayout) view.findViewById(R.id.item_customer_row);
            companyView = (TextView) view.findViewById(R.id.item_customer_company);
            nameView = (TextView) view.findViewById(R.id.item_customer_name);
            cityView = (TextView) view.findViewById(R.id.item_customer_city);
        }
    }
```
#### Set a onClick method on the customer row


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
            holder.customerRow.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Intent intent = new Intent(mContext, CustomerDetailsActivity.class);
                    mContext.startActivity(intent);
                }
            });
        } catch (JSONException e) {
            Log.e(CustomerAdapter.class.getName(), "Error: " + e);
        }
    }
```

#### Test on the emulator

![Test Customer Details](http://throw.rocks/fm-invoices/14_new_activity/new_activity_06_test_customer_details.png)


#### SingleTop

```xml
 <activity
            android:name=".activity.CustomersListActivity"
            android:label="Customers List"
            android:launchMode="singleInstance">
```

#### Congratulations!


<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-customer-details.html">The Customer Details Layout</a>