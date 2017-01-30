---
layout: page
comments: false
title: The Address Toggle, Wrapping Up
permalink: fm-invoices-address-toggle-wrapping-up
---

#### The Address toggle

Declare the buttons. Set the initial state.

```java
        final Button buttonBillingAddress = (Button) findViewById(R.id.customer_detail_button_billing_address);
        final Button buttonShippingAddress = (Button) findViewById(R.id.customer_detail_button_shipping_address);
        buttonBillingAddress.setEnabled(false);
        buttonBillingAddress.setTextColor(Color.parseColor("#9E9E9E"));
        buttonShippingAddress.setOnClickListener(new View.OnClickListener() {
```

```java
        buttonShippingAddress.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                address1View.setText(shippingAddress1);
                address2View.setText("");
                cityView.setText(shippingCity);
                stateView.setText(shippingState);
                postalCodeView.setText(shippingPostalCode);
                countryView.setText(shippingCountry);
                buttonShippingAddress.setEnabled(false);
                buttonShippingAddress.setTextColor(Color.parseColor("#9E9E9E"));
                buttonBillingAddress.setEnabled(true);
                buttonBillingAddress.setTextColor(Color.parseColor("#212121"));
            }
        });
        buttonBillingAddress.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                address1View.setText(address1);
                address2View.setText("");
                cityView.setText(city);
                stateView.setText(state);
                postalCodeView.setText(postalCode);
                countryView.setText(country);
                buttonBillingAddress.setEnabled(false);
                buttonBillingAddress.setTextColor(Color.parseColor("#9E9E9E"));
                buttonShippingAddress.setEnabled(true);
                buttonShippingAddress.setTextColor(Color.parseColor("#212121"));
            }
        });
```