---
layout: page
comments: false
title: The API class
permalink: fm-invoices-the-api-class.html
---


#### Create the API class
```java
package rocks.athrow.fm_invoices.data;

/**
 * Created by jose on 1/18/17.
 */

public final class API {
    private static final String API_HOST = "";
    private static final String API_KEY = "";

    private API() {
        throw new AssertionError("No API instances for you!");
    }
}
```

```java
    private static final String API_HOST = "http://myhost.com/RESTfm/sample_data/";
    private static final String API_KEY = "?RFMkey=MyAPIKey";
```

#### Add the httpConnect method

Add the following method to your API class. If you set up the Auto Import correctly, you will see.

```java
    /**
     * httpConnect
     *
     * @param queryURL the query URL
     * @return an APIResponse object
     */
    private static APIResponse httpConnect(String queryURL) {
        APIResponse apiResponse = new APIResponse();
        HttpURLConnection urlConnection = null;
        BufferedReader reader = null;
        try {
            Uri builtUri = Uri.parse(queryURL).buildUpon().build();
            URL url = new URL(builtUri.toString());
            urlConnection = (HttpURLConnection) url.openConnection();
            urlConnection.setRequestMethod("GET");
            urlConnection.connect();
            apiResponse.setResponseCode(urlConnection.getResponseCode());
            InputStream inputStream = urlConnection.getInputStream();
            StringBuilder buffer = new StringBuilder();
            if (inputStream == null) {
                return apiResponse;
            }
            reader = new BufferedReader(new InputStreamReader(inputStream));
            String line;
            while ((line = reader.readLine()) != null) {
                buffer.append(line);
            }
            if (buffer.length() == 0) {
                return apiResponse;
            }
            apiResponse.setResponseText(buffer.toString());
        } catch (IOException v) {
            apiResponse.setResponseText(v.toString());
            return apiResponse;
        } finally {
            if (urlConnection != null) {
                urlConnection.disconnect();
            }
            if (reader != null) {
                try {
                    reader.close();
                } catch (final IOException e) {
                    e.printStackTrace();
                }
            }
        }
        return apiResponse;
    }
```

#### Create your first API method

The URI will look like this:

`http://myhost.com/RESTfm/sample_data/layout/customers.json?RFMkey=MyAPIKey&RFMmax=0`

http://www.restfm.com/restfm-manual/web-api-reference-documentation/uri-restfmdatabaselayoutlayout/read-get


```java
    public static APIResponse getCustomers(int maxResults) {
        String url = API_HOST + "layout/customers.json" + API_KEY + "&RFMmax=" + maxResults;
        return httpConnect(url);
    }
```




