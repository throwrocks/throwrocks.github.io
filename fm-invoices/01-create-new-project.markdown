---
layout: page
comments: true
title: Create a New Project
permalink: fm-invoices-create-new-project.html
---

#### Create a new Project

When you open Android Studio for the first time you will be presented with this splash screen.  Click on "Start a new Android project" to get started.

![Welcome to Android Studio](http://throw.rocks/fm-invoices/01_create_project/create_new_project_01_welcome.png)

On the next window, you are going to name your application. You will also need to set your company name. The combination of your company domain and your application name creates the app's package name.

For example, I'm naming this app FM-Invoices and my company domain is throw.rocks. The resulting package name is `rocks.athrow.fm_invoices`.

In my case, Android Studio adds an "a" before throw because throw is a reserved Java word. Feel free to use your own Appliocation name and Company domain for this project.

![Start a new project](http://throw.rocks/fm-invoices/01_create_project/create_new_project_02_project_name.png)

Next, Android Studio will ask you what is the minimum API that your app will target. This is a decision that you will have to make based on your targe audience.  Since this is an app for internal use and I know that all my users are on Android 5.0+, I'm going to select Lolipop. If I were to distribute my app to the world, then it would be compaitble with 40% of all Android devices out there.

Keep in mind that you can always change this setting later. If you decice to downgrade the APi later, you may have used classes and are methods that are not available in earlier APIs. So Android Studio will warn you at that point and you will have to find alternatives to make your app compatible with the older devices.

![Target Device](http://throw.rocks/fm-invoices/01_create_project/create_new_project_03_target_device.png)

Now it's time to chose a template. This will basically help you start with some boilerplate code to support certain design patterns and functionalities. For this project, we will just create a Blank Activity and write everything from scratch. Once you get more experience with Android code you will be able to use a starter solution and modify it accordingly.

![Choose Template](http://throw.rocks/fm-invoices/01_create_project/create_new_project_04_template.png)

Finally, Android Studio will help you set up some details about your MainActivity. The MainActivity by default will be the first Activity that your app will launch. It's like the first screen and the startup script that you run when opening a FileMaker solution.

![Create MainActivity](http://throw.rocks/fm-invoices/01_create_project/create_new_project_05_main_activity.png)

Congratulations, you have created your first Android project! This is how your screen should look now. 

![Congratulations](http://throw.rocks/fm-invoices/01_create_project/create_new_project_06_congratulations.png)

