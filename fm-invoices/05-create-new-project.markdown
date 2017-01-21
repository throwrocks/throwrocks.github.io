---
layout: page
comments: false
title: Create a New Project
permalink: fm-invoices-create-new-project.html
---

#### Create a new Project

When you open Android Studio for the first time, it will bring up this splash screen.  Click on "Start a new Android project" to get started.

![Welcome to Android Studio](http://throw.rocks/fm-invoices/05_create_project/create_new_project_01_welcome.png)

On the next window, you are going to name your application, and you will also need to enter your company domain name. The combination of your application and company domain names creates the app's package name.

For example, I'm calling this app FM-Invoices, and my company domain is throw.rocks. The resulting package name is `rocks.athrow.fm_invoices`.

In my case, Android Studio adds an "a" before throw because 
"throw" is a reserved Java word. Feel free to name your application and company domain names however you like.

![Start a new project](http://throw.rocks/fm-invoices/05_create_project/create_new_project_02_project_name.png)

Next, Android Studio will ask you what the minimum API that your app will target. You have to decide the minimum API based on your target audience.  Since this is an app for internal use and I know that all my users are on Android 5.0+, I'm going to select Lolipop. If I later want to distribute my app to the world, it would be compatible with 40% of all Android devices in the wild.

Keep in mind that you can always change the minimum API later. Although you may have used classes and are methods that are not available in previous APIs, Android Studio will warn you about them and at that point, and you will have to find alternatives to make your app compatible with the older devices.

![Target Device](http://throw.rocks/fm-invoices/05_create_project/create_new_project_03_target_device.png)

Now it's time to chose a template. These are boilerplate code to support certain design patterns and functionalities. For this project, we will create a Blank Activity because we will write everything from scratch. Once you get more experience with Android code, you will be able to use a template and modify it accordingly.

![Choose Template](http://throw.rocks/fm-invoices/05_create_project/create_new_project_04_template.png)

Finally, Android Studio will help you set up some details about your `MainActivity`. The `MainActivity` is by default the first Activity that your app will launch. It's like the first screen and the startup script that runs when opening a FileMaker application.

![Create MainActivity](http://throw.rocks/fm-invoices/05_create_project/create_new_project_05_main_activity.png)

Congratulations, you have created your first Android project! Your screen should look like this now. 
![Congratulations](http://throw.rocks/fm-invoices/05_create_project/create_new_project_06_congratulations.png)

