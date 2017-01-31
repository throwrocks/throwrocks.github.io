---
layout: page
comments: false
title: Common Intents
permalink: fm-invoices-common-intents
---

#### Declare the buttton variables

```java
        // Declare the button variables
        ImageButton buttonEmail = (ImageButton) findViewById(R.id.customer_detail_button_email);
        ImageButton buttonPhone = (ImageButton) findViewById(R.id.customer_detail_button_phone);
        ImageButton buttonWeb = (ImageButton) findViewById(R.id.customer_detail_button_web);
        ImageButton buttonLocation = (ImageButton) findViewById(R.id.customer_detail_button_location);
        // TODO: Set the click listeners
```
#### Create the Listener Methods

##### Send Email


https://developer.android.com/guide/components/intents-common.html#Email

```java
   public void composeEmail(String[] addresses) {
        Intent intent = new Intent(Intent.ACTION_SEND);
        intent.setType("*/*");
        intent.putExtra(Intent.EXTRA_EMAIL, addresses);
        if (intent.resolveActivity(getPackageManager()) != null) {
            startActivity(intent);
        }
    }
```

```java
        buttonEmail.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String[] to = new String[1];
                to[0] = officeEmail;
                composeEmail(to);
            }
        });
```

##### Dial Phone Number

https://developer.android.com/guide/components/intents-common.html#Phone

```java
    public void dialPhoneNumber(String phoneNumber) {
        Intent intent = new Intent(Intent.ACTION_DIAL);
        intent.setData(Uri.parse("tel:" + phoneNumber));
        if (intent.resolveActivity(getPackageManager()) != null) {
            startActivity(intent);
        }
    }
```

```java
        buttonPhone.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                dialPhoneNumber(officePhone);
            }
        });
```

##### Open Web page

https://developer.android.com/guide/components/intents-common.html#Browser

```java
    public void openWebPage(String url) {
        Uri webpage = Uri.parse(url);
        Intent intent = new Intent(Intent.ACTION_VIEW);
        intent.setData(webpage);
        if (intent.resolveActivity(getPackageManager()) != null) {
            startActivity(intent);
        }
    }
```

```java
        buttonWeb.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                openWebPage(website);
            }
        });
```

##### Show Map


https://developer.android.com/guide/components/intents-common.html#Maps

```java
    public void showMap(String location) {
        Uri geoLocation = Uri.parse("geo:0,0?q="+location);
        Intent intent = new Intent(Intent.ACTION_VIEW);
        intent.setData(geoLocation);
        if (intent.resolveActivity(getPackageManager()) != null) {
            startActivity(intent);
        }
    }
```

```java
        buttonLocation.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                showMap(Uri.encode(address1 + ", " + city + ", " + state + ", " + postalCode));
            }
        });
```


#### Congratulations!

<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-address-toggle.html">The Address Toggle</a>