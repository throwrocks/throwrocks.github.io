---
layout: page
comments: true
title: Enable Auto Import
permalink: fm-invoices-create-new-project.html
---

#### Why Auto Import?

The Android platform provides numerous classes that we can use for free.  To use these classes, we must import their corresponding packages into our classes.  Since you will be copying the code from this tutorial, you will save a lot of time if you let Android Studio automatically import the required packages.

#### How to enable Auto Import?

From the menu, click on File and then Settings.

![Welcome to Android Studio](http://throw.rocks/fm-invoices/02_settings/settings_01_open.png)

On the settings window, use the search box to find "Auto Import." Once you get the results on the right panel, check the box next to "Add unambiguous imports on the fly."

![Enable Auto Imports](http://throw.rocks/fm-invoices/02_settings/settings_02_enable_auto_import.png)

What will happen then is that when you copy some code that uses classes from packages that you haven't imported, Android Studio will automatically add the import statements to your class.

For example, later on, you will work on implementing an `API` class, and you will have to copy a method that uses the `URL` class.  Specifically 

```
URL url = new URL(builtUri.toString());
```

As soon as you copy that line of code, Android Studio will add the following import statement to the top of your class.

```
import java.net.URL;
```


By using Auto Import, it will be a lot easier for you to copy code from this tutorial. 

#### What happens if I don't have a necessary import statement?

If you try to use the `URL` class without the import statement, Android Studio will turn it red and give you a warning.

![Broken import statement](http://throw.rocks/fm-invoices/02_settings/settings_03_broken_import.png)

But don't worry, these errors are easy to catch because your app will not even run.

#### Quick warning on copying code

Make sure you don't copy my package declaration. 

```
package rocks.athrow.fm_invoices;
```

Since you package name will probably be different, if you paste my package declaration your code will not compile.

#### Congratulations!

You are now ready to start developing an Android app that connects to FileMaker Server.



