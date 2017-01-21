---
layout: page
comments: false
title: Turn FileMaker Server into a JSON API with RESTFm
permalink: fm-invoices-restfm.html
---

To turn our FileMaker Server into a JSON API we will use RESTFm. 

#### What is RESTFm?

>RESTfm is PHP code that turns your FileMaker Server into a RESTful Web Service, so you can access your FileMaker Server databases via HTTP using a common REST architecture with easy to understand API calls.

RESTfm is developed by [Goya Pty Ltd](http://www.goya.com.au/), a consulting and development company specialising in FileMaker Pro. RESTfm is now Open Source and distributed under the [MIT License](https://github.com/GoyaPtyLtd/RESTfm/blob/master/LICENSE).

The reason we're going to use RESTfm is because it can transform the XML output from the FileMaker Custom Web Publishing Engine into JSON, which is very easy to consume in Java. I also like that you can create API Keys in RESTfm, so your client applications don't have to pass the actual username and password for your FileMaker solution.