---
layout: page
comments: false
title: Customers List
permalink: fm-invoices-customer-list.html
---

To make our design choices simple so we can focus on developing the Android app, we will use the FileMaker Invoices `Customers | iPhone` layout as our guide. The main difference between creating layouts in FileMaker vs Android, is that Android layouts are XML files. Android Studio has a visual editor, but let's write our layouts in XML so we can understand each component and their attributes. 

The first layot we'll create is the "item_customer" layout. Which is basically the Body part of the FileMaker `Customers | iPhone` layout. To achieve the same design in Android, we can use a combination of three different views (or widgets). 

![Customer Item Design](http://throw.rocks/fm-invoices/11_customer_list/customer_list_01_design.png)

**LinearLayout**
>A Layout that arranges its children in a single column or a single row. 
>
>[Source](https://developer.android.com/reference/android/widget/LinearLayout.html)

**TextView**

>Displays text to the user and optionally allows them to edit it.
>
>[Source](https://developer.android.com/reference/android/widget/TextView.html)

**ImageView**

>Displays an arbitrary image, such as an icon.
>
>[Source](https://developer.android.com/reference/android/widget/ImageView.html)

Notice that our parent layout is a horizontal `LinearLayout` with three children positioned next to each other.

1. A vertical `LinearLayout` with the "Company Name" and the "Customer Name" inside of it.
2. The City `TextView`
3. The arrow icon `ImageView`

#### Download The arrow icon 

Download the icon: https://material.io/icons/#ic_keyboard_arrow_right

[![Download the icon][10]][10]

[10]: http://throw.rocks/fm-invoices/11_customer_list/customer_list_10_download_arrow_icon.png


[![Copy the drawable folders][12]][12]

[12]: http://throw.rocks/fm-invoices/11_customer_list/customer_list_11_copy_the_drawable_folders.png

[![res/drawable][11]][11]

[11]: http://throw.rocks/fm-invoices/11_customer_list/customer_list_11_res_drawables.png


#### Create the "item_customer.xml" file 

In Android, we store layout files in the `app/main/res/layout` folder. Let's create a new file there:

![Create Customer Item Layout](http://throw.rocks/fm-invoices/11_customer_list/customer_list_02_create_layout.png)

Name it "item_customer" and click *Ok*.

![Create Customer Item Layout](http://throw.rocks/fm-invoices/11_customer_list/customer_list_03_name_layout.png)

Now the change the layout view from *Design* mode to *Text* mode. We will design our layout in XML so we can learn how it works under the hood.

[![Change to Text mode][13]][132]

[13]: http://throw.rocks/fm-invoices/11_customer_list/customer_list_12_text_mode.png

Now copy the code below in your new layout. I added color, padding and margin so we can see how the individual views are rendered on the screen.

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

#### Incluce the item_customer in activity_main

For testing purposes, let's include the new layout in activity_main, so when we run the app we can see it in the emulator. Delete the "Hello World" `TextView` and replace it with an include element that references our new layout.

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

</LinearLayout>
```

#### Run the app

Let's run the app on the emulator. Click on the "Play" button on the Android Studio **toolbar**.

![Run the app](http://throw.rocks/fm-invoices/11_customer_list/customer_list_04_run_app.png)

#### LinearLayout Weight

If we look at the emulator we will see that our views are positioned correctly, but their widths are not right. 

![Run the app](http://throw.rocks/fm-invoices/11_customer_list/customer_list_05_first_run.png)

We want our views to fill the screen. However, we don't to hard-code specific dimension values because if we run the app in devices with different resolutions and densities it will not look good. In FileMaker, we can use anchors to make our views stick, float, or grow to fill the layout. In Android, when using `LinearLayout` we can use the `weight` attribute to make our layout responsive.

>LinearLayout also supports assigning a weight to individual children with the android:layout_weight attribute. This attribute assigns an "importance" value to a view in terms of how much space it should occupy on the screen. A larger weight value allows it to expand to fill any remaining space in the parent view.
>
>[Source](https://developer.android.com/guide/topics/ui/layout/linear.html)

Let's assign some weight values the views inside our parent `LinearLayout` and set their widths to 0dp. By setting the widths to 0, the width will be determined by the weight value.

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

Let's run the app again:

![Run the app again](http://throw.rocks/fm-invoices/11_customer_list/customer_list_06_second_run.png)

Now let's rotate the device to see how it behaves. Use the rotate buttons on the floating bar next to your emulator window.

![Rotate the device](http://throw.rocks/fm-invoices/11_customer_list/customer_list_07_rotate_device.png)

![Rotate the device](http://throw.rocks/fm-invoices/11_customer_list/customer_list_08_landscape.png)

#### Clean up and style the item_customer layout

Now let's remove the background colors and the padding we used to visualize our views, and let's style our text views so they look similar to the FileMaker `Customers | iPhone` layout.

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
#### Final test

As a final test, let's add multiple includes to the `activity_main` layout so we can see how our list will look like:

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

![Run the app](http://throw.rocks/fm-invoices/11_customer_list/customer_list_09_final.png)

#### Congratulations!


<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-customer-adapter.html">The Customer Adapter</a>