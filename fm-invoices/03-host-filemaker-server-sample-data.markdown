---
layout: page
comments: false
title: Host the FileMaker Server Sample File
permalink: fm-invoices-host-filemaker-server-sample-data.html
---

The Sample File is a copy of the FileMaker Pro 14 Invoices Starter Solution. I already populated the file with customers and products. You can download it here [here]().

If you need help hosting the file, you can consult the FileMaker Server documentation on [Uploading database files to FileMaker Server](http://help.filemaker.com/app/answers/detail/a_id/11957/~/uploading-database-files-to-filemaker-server).

Open the file after you finish hosting it.

![Customers List](http://throw.rocks/fm-invoices/03_host_sample_data/host_sample_data_01_filemaker_starter_solution.png)

### Create the API layouts 

When using FileMaker Custom Web Publishing, you always have to specify a layout in your API calls. The result of your API call will contain all the records in the foundset and all the fields placed on the specified layout. The layout is required whether you are creating, updating, finding or deleting records, or even when running a script. For this reason, I like to create layouts in the FileMaker file dedicated to the API. By doing so, I don't have to add unnecesary fields on the user Layouts, and I don't have to worry about removing an API field by accident.

Let's open the Layout Manager and create a folder for the API layouts, and then create one layout for each table that our Android app will use. 

![Create the API layouts](http://throw.rocks/fm-invoices/03_host_sample_data/host_sample_data_02_create_api_layouts.png)

#### The individual API layouts 

Each layout should have all the fields that your API will return. Let's start with the api_customers layout. Add the following fields:

* CUSTOMER ID MATCH FIELD
* First
* Last
* Company
* Initial
* Job Title
* Website
* Customer Name
* Customer Info
* Office Phone
* Mobile Phone
* Fax
* Office Email
* Home Email
* Address 1
* City
* State
* Postal Code
* Country
* Billing Address
* Billing Address Short
* Address 1 Shipping
* City Shipping
* State Shipping
* Postal Code Shipping
* Country Shipping
* Shipping Address
* Shipping Address Short

![The Customers API layout](http://throw.rocks/fm-invoices/03_host_sample_data/host_sample_data_03_api_customers_layout.png)

#### Create the Account 

Open the FileMaker Security settings and create a new account with the [Data Entry Only] Privilege Set. The account name and password has to match the values you set in the RESTfm.ini.php file. 

```
$config['keys'] = array (
    '71c717c4-d8e3-485f-a815-f5928f1f7a3e' => array ('Jose', 'myPassword'),
);
```

![FileMaker Account](http://throw.rocks/fm-invoices/03_host_sample_data/host_sample_data_05_user.png)

Notice that my account name is Jose and my password is MyPassword. So I create the account with the same name and password. By doing so, RESTfm will know that the API Key "71c717c4-d8e3-485f-a815-f5928f1f7a3e" belongs to Jose. And it will handle authenticating the user without having to provide the user name and password.

#### Add the fmphp Extended Privilege

We're almost ready to test our API. Just one more step. We need to add the fmphp *Extended Privilege* to the Privilege Set of the API user that we set in the $config['keys'] variable of our RESTfm.ini.php file.

In my case, the user Jose has the [Data Entry Only] Privilege Set. So I'm adding the fmphp Extended Privilege to that Privilege Set.

![fmphp Extended Privilege](http://throw.rocks/fm-invoices/03_host_sample_data/host_sample_data_04_php_access.png)


#### Testing the API from the browser

Great, now we will query the API from the browser to get the first record of the customers table.

Build the URL using this format:

`http://myserver/RESTfm/sample_data/layout/api_customers.json?RFMmax=1&RFMkey=myAPIKey`

For example:

`http://throw.rocks/RESTfm/sample_date/layout/api_customers.json?RFMmax=1&RFMkey=71c717c4-d8e3-485f-a815-f5928f1f7a3e`

And your browser should respond with the following output:

```json
{  
   "metaField":[  
      {  
         "name":"CUSTOMER ID MATCH FIELD",
         "autoEntered":1,
         "global":0,
         "maxRepeat":1,
         "resultType":"number"
      },
      {  
         "name":"First",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Last",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Company",
         "autoEntered":1,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Initial",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Job Title",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Website",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Customer Name",
         "autoEntered":1,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Customer Info",
         "autoEntered":1,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Office Phone",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Mobile Phone",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Fax",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Office Email",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Home Email",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Address 1",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"City",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"State",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Postal Code",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Country",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Billing Address",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Billing Address Short",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Address 1 Shipping",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"City Shipping",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"State Shipping",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Postal Code Shipping",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Country Shipping",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Shipping Address",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      },
      {  
         "name":"Shipping Address Short",
         "autoEntered":0,
         "global":0,
         "maxRepeat":1,
         "resultType":"text"
      }
   ],
   "meta":[  
      {  
         "recordID":"2",
         "href":"\/RESTfm\/sample_data\/layout\/api_customers\/2.json"
      }
   ],
   "data":[  
      {  
         "CUSTOMER ID MATCH FIELD":"2",
         "First":"James",
         "Last":"Butt",
         "Company":"Benton, John B Jr",
         "Initial":"B",
         "Job Title":"",
         "Website":"http:\/\/www.bentonjohnbjr.com",
         "Customer Name":"Butt, James",
         "Customer Info":"James Butt",
         "Office Phone":"504-621-8927",
         "Mobile Phone":"504-845-1427",
         "Fax":"",
         "Office Email":"jbutt@gmail.com",
         "Home Email":"",
         "Address 1":"6649 N Blue Gum St",
         "City":"New Orleans",
         "State":"LA",
         "Postal Code":"70116",
         "Country":"USA",
         "Billing Address":"6649 N Blue Gum St\nNew Orleans, LA 70116\nUSA",
         "Billing Address Short":"New Orleans, LA 70116 USA",
         "Address 1 Shipping":"6649 N Blue Gum St",
         "City Shipping":"New Orleans",
         "State Shipping":"LA",
         "Postal Code Shipping":"70116",
         "Country Shipping":"USA",
         "Shipping Address":"6649 N Blue Gum St\nNew Orleans, LA 70116\nUSA",
         "Shipping Address Short":"New Orleans, LA 70116 USA"
      }
   ],
   "info":{  
      "tableRecordCount":"502",
      "foundSetCount":"502",
      "fetchCount":"1",
      "skip":0,
      "X-RESTfm-Version":"4.0.1\/20160322-b72e584",
      "X-RESTfm-Protocol":"5",
      "X-RESTfm-Status":200,
      "X-RESTfm-Reason":"OK",
      "X-RESTfm-Method":"GET",
      "X-RESTfm-PHP-memory_limit":"128M",
      "X-RESTfm-PHP-post_max_size":"8M"
   },
   "nav":[  
      {  
         "name":"start",
         "href":"\/RESTfm\/sample_data\/layout\/api_customers.json?RFMmax=1&RFMkey=71c717c4-d8e3-485f-a815-f5928f1f7a3e"
      },
      {  
         "name":"next",
         "href":"\/RESTfm\/sample_data\/layout\/api_customers.json?RFMmax=1&RFMkey=71c717c4-d8e3-485f-a815-f5928f1f7a3e&RFMskip=1"
      },
      {  
         "name":"end",
         "href":"\/RESTfm\/sample_data\/layout\/api_customers.json?RFMmax=1&RFMkey=71c717c4-d8e3-485f-a815-f5928f1f7a3e&RFMskip=501"
      }
   ]
}
```

There's a lot of information in the response. But don't worry about it. We'll parse it out later. Let's install Android Studio first so we can start developing the Android app.

### Congratulations!

You are now ready to install Android Studio.

<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-install-android-studio.html">Install Android Studio</a>