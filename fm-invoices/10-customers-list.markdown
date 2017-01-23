---
layout: page
comments: false
title: Customers List
permalink: fm-invoices-customer-list.html
---

https://material.io/icons/#ic_keyboard_arrow_right

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="#4DD0E1"
    android:orientation="horizontal"
    android:padding="4dp">

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:background="#FDD835"
        android:orientation="vertical"
        android:padding="4dp">

        <TextView
            android:id="@+id/item_customer_company"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="#FB8C00"
            android:text="My Company" />

        <TextView
            android:id="@+id/item_customer_name"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:background="#FB8C00"
            android:text="Jose Lopez" />

    </LinearLayout>

    <TextView
        android:id="@+id/item_customer_city"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_marginEnd="4dp"
        android:layout_marginStart="4dp"
        android:background="#FB8C00"
        android:gravity="end|center"
        android:text="Dallas" />

    <ImageView
        android:id="@+id/item_customer_right_arrow"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:background="#7CB342"
        android:src="@drawable/ic_keyboard_arrow_right_black_24dp" />
</LinearLayout>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="#4DD0E1"
    android:orientation="horizontal"
    android:padding="4dp">

    <LinearLayout
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="8"
        android:background="#FDD835"
        android:orientation="vertical"
        android:padding="4dp">

        <TextView
            android:id="@+id/item_customer_company"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:background="#FB8C00"
            android:text="My Company" />

        <TextView
            android:id="@+id/item_customer_name"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:background="#FB8C00"
            android:text="Jose Lopez" />

    </LinearLayout>

    <TextView
        android:id="@+id/item_customer_city"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_marginEnd="4dp"
        android:layout_marginStart="4dp"
        android:layout_weight="4"
        android:background="#FB8C00"
        android:gravity="end|center"
        android:text="Dallas" />

    <ImageView
        android:id="@+id/item_customer_right_arrow"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"
        android:background="#7CB342"
        android:src="@drawable/ic_keyboard_arrow_right_black_24dp" />
</LinearLayout>
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    tools:context="rocks.athrow.fm_invoices.MainActivity">

    <include layout="@layout/item_customer" />

    <include layout="@layout/item_customer" />

    <include layout="@layout/item_customer" />

    <include layout="@layout/item_customer" />

    <include layout="@layout/item_customer" />

    <include layout="@layout/item_customer" />
</LinearLayout>
```


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="horizontal"
    android:padding="16dp">

    <LinearLayout
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_weight="8"
        android:orientation="vertical">

        <TextView
            android:id="@+id/item_customer_company"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="My Company"
            android:textColor="#212121"
            android:textSize="18sp" />

        <TextView
            android:id="@+id/item_customer_name"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_marginTop="4dp"
            android:text="Jose Lopez" />

    </LinearLayout>

    <TextView
        android:id="@+id/item_customer_city"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="4"
        android:gravity="end|center"
        android:text="Dallas"
        android:textColor="#212121" />

    <ImageView
        android:id="@+id/item_customer_right_arrow"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"
        android:src="@drawable/ic_keyboard_arrow_right_black_24dp"
        android:tint="#9E9E9E" />
</LinearLayout>
```