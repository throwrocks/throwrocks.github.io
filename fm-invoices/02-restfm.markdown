---
layout: page
comments: false
title: Turn FileMaker Server into a JSON API with RESTfm
permalink: fm-invoices-restfm.html
---

We're going to use RESTfm because it can transform the XML output from the FileMaker Custom Web Publishing Engine into JSON, which is very easy to consume in Java. Also, because it allows us to create API Keys for our users, so our Android app doesn't have to pass the actual username and password of our FileMaker database file.

#### What is RESTfm?

>RESTfm is PHP code that turns your FileMaker Server into a RESTful Web Service, so you can access your FileMaker Server databases via HTTP using a common REST architecture with easy to understand API calls.
>
>[http://www.restfm.com/](http://www.restfm.com/)

RESTfm is developed by [Goya Pty Ltd](http://www.goya.com.au/), a consulting and development company specializing in FileMaker Pro. RESTfm is Open Source and distributed under the [MIT License](https://github.com/GoyaPtyLtd/RESTfm/blob/master/LICENSE).

#### Install RESTfm

Please follow the instructions to install RESTfm in your platform.

[Install on Windows](http://www.restfm.com/restfm-manual/install/quick-install-microsoft-windows)

[Install on MacOSx](http://www.restfm.com/restfm-manual/install/quick-install-apple-os-x)

Continue once you complete the installation and the RESTfm report page indicates no errors.

#### Create an API Key

Now let's create an API Key for your Android app. Open the `RESTfm.ini.php` file and locate the following section around line 108.

```php
/*
 * List of API keys associated with a username and password.
 */
$config['keys'] = array (
    //'EXAMPLEKEY' => array ('exampleuser', 'examplepass'),
);
```

Uncomment the line inside the array and set your information. You can make up your API Key, or you can use an application to create one for you. For example, you can use this free [API Key Generator](https://codepen.io/corenominal/pen/rxOmMJ). 

You will also need to set the FileMaker username and password associated to the API Key. Let's place all the information here first and then we'll create the account in FileMaker.

Your keys configuration should look something like this:

```php
$config['keys'] = array (
    '71c717c4-d8e3-485f-a815-f5928f1f7a3e' => array ('Jose', 'myPassword'),
);
```
#### Congratulations!

You have successfully installed and configured RESTfm. Let's now host the FileMaker Server Sample file.


<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-host-filemaker-server-sample-data.html">Host the FileMaker Server Sample File</a>