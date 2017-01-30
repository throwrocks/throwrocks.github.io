---
layout: page
comments: false
title: The Customer Details
permalink: fm-invoices-customer-details
---

![FileMaker Customer Details 1](http://throw.rocks/fm-invoices/15_customer_details/customer_details_01_filemaker.png)


![FileMaker Customer Details 2](http://throw.rocks/fm-invoices/15_customer_details/customer_details_02_filemaker.png)


[![Android Customer Details][3]][3]

[3]: http://throw.rocks/fm-invoices/15_customer_details/customer_details_03_android_design.png

#### Download the icons

1. email
2. phone
3. web
4. location

#### Develop the layout

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="16dp">

        <TextView
            android:id="@+id/customer_detail_header_company"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Company Name"
            android:textColor="#212121"
            android:textSize="20sp" />

        <TextView
            android:id="@+id/customer_detail_header_customer"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Customer Name"
            android:textColor="#212121"
            android:textSize="14sp" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:orientation="horizontal">

            <ImageButton
                android:id="@+id/customer_detail_button_email"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:src="@drawable/ic_email_black_24dp"
                android:tint="#424242" />

            <ImageButton
                android:id="@+id/customer_detail_button_phone"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:src="@drawable/ic_phone_black_24dp"
                android:tint="#424242" />

            <ImageButton
                android:id="@+id/customer_detail_button_web"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:src="@drawable/ic_web_black_24dp"
                android:tint="#424242" />

            <ImageButton
                android:id="@+id/customer_detail_button_location"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:src="@drawable/ic_location_on_black_24dp"
                android:tint="#424242" />
        </LinearLayout>

        <View
            android:layout_width="match_parent"
            android:layout_height="1dp"
            android:layout_marginBottom="8dp"
            android:layout_marginTop="8dp"
            android:background="#E0E0E0" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="Company" />

            <TextView
                android:id="@+id/customer_detail_company"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="Job Title" />

            <TextView
                android:id="@+id/customer_detail_job_title"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="Website" />

            <TextView
                android:id="@+id/customer_detail_website"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <View
            android:layout_width="match_parent"
            android:layout_height="1dp"
            android:layout_marginTop="8dp"
            android:background="#E0E0E0" />

        <LinearLayout
            android:id="@+id/customer_detail_button_view_invoices"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginBottom="8dp"
            android:layout_marginTop="8dp"
            android:orientation="horizontal">

            <LinearLayout
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_gravity="start"
                android:layout_weight="5"
                android:orientation="vertical">

                <TextView
                    android:id="@+id/customer_detail_count_invoices"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginStart="8dp"
                    android:gravity="center"
                    android:text="0"
                    android:textColor="#2196F3"
                    android:textSize="20sp" />

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginStart="8dp"
                    android:text="Invoices" />
            </LinearLayout>

            <ImageView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_gravity="center"
                android:layout_weight="1"
                android:src="@drawable/ic_keyboard_arrow_right_black_24dp" />
        </LinearLayout>

        <View
            android:layout_width="match_parent"
            android:layout_height="1dp"
            android:layout_marginBottom="8dp"
            android:background="#E0E0E0" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="Office Phone" />

            <TextView
                android:id="@+id/customer_detail_phone_office"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="Mobile Phone" />

            <TextView
                android:id="@+id/customer_detail_phone_mobile"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="Fax" />

            <TextView
                android:id="@+id/customer_detail_fax"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <View
            android:layout_width="match_parent"
            android:layout_height="1dp"
            android:layout_marginBottom="8dp"
            android:layout_marginTop="8dp"
            android:background="#E0E0E0" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="Office Email" />

            <TextView
                android:id="@+id/customer_detail_office_email"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="Home Email" />

            <TextView
                android:id="@+id/customer_home_email"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <View
            android:layout_width="match_parent"
            android:layout_height="1dp"
            android:layout_marginBottom="8dp"
            android:layout_marginTop="8dp"
            android:background="#E0E0E0" />

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal">

            <Button
                android:id="@+id/customer_detail_button_billing_address"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="Billing Address"
                android:textColor="#212121"
                android:textSize="12sp" />

            <Button
                android:id="@+id/customer_detail_button_shipping_address"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:text="Shipping Address"
                android:textColor="#9E9E9E"
                android:textSize="12sp" />
        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="Address 1" />

            <TextView
                android:id="@+id/customer_detail_address1"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="Address 2" />

            <TextView
                android:id="@+id/customer_detail_address2"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="City" />

            <TextView
                android:id="@+id/customer_detail_city"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="State" />

            <TextView
                android:id="@+id/customer_detail_state"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="Postal Code" />

            <TextView
                android:id="@+id/customer_detail_postal_code"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />

        </LinearLayout>

        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            android:orientation="horizontal">

            <TextView
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_weight="1.5"
                android:text="Country" />

            <TextView
                android:id="@+id/customer_detail_country"
                android:layout_width="0dp"
                android:layout_height="wrap_content"
                android:layout_marginStart="8dp"
                android:layout_weight="4"
                android:text="data"
                android:textColor="#212121" />
        </LinearLayout>
    </LinearLayout>
</ScrollView>
```

[![Test the Layout][4]][4]

[4]: http://throw.rocks/fm-invoices/15_customer_details/customer_details_04_test_layout.png


#### Congratulations!

<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-intent-extras.html">Setting the Customer Details via Intent</a>