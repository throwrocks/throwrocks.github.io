---
layout: page
comments: false
title: Setting the Customer Details via Intent
permalink: fm-invoices-intent-extras
---

#### Adapter 

```java
    @Override
    public void onBindViewHolder(ViewHolder holder, int position) {
        try {
            holder.customerRecord = mCustomers.getJSONObject(position);
            final String company = holder.customerRecord.getString("Company");
            final String firstName = holder.customerRecord.getString("First");
            final String lastName = holder.customerRecord.getString("Last");
            final String fullName = firstName + " " + lastName;
            final String city = holder.customerRecord.getString("City");
            final String jobTitle = holder.customerRecord.getString("Job Title");
            final String website = holder.customerRecord.getString("Website");
            final String officePhone = holder.customerRecord.getString("Office Phone");
            final String mobilePhone = holder.customerRecord.getString("Mobile Phone");
            final String officeEmail = holder.customerRecord.getString("Office Email");
            final String homeEmail = holder.customerRecord.getString("Home Email");
            final String fax = holder.customerRecord.getString("Fax");
            final String address1 = holder.customerRecord.getString("Address 1");
            final String state = holder.customerRecord.getString("State");
            final String postalCode = holder.customerRecord.getString("Postal Code");
            final String country = holder.customerRecord.getString("Country");
            final String shippingAddress1 = holder.customerRecord.getString("Address 1 Shipping");
            final String shippingCity = holder.customerRecord.getString("City Shipping");
            final String shippingState = holder.customerRecord.getString("State Shipping");
            final String shippingPostalCode = holder.customerRecord.getString("Postal Code Shipping");
            final String shippingCountry = holder.customerRecord.getString("Country Shipping");
            holder.companyView.setText(company);
            holder.nameView.setText(fullName);
            holder.cityView.setText(city);
            holder.customerRow.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    Intent intent = new Intent(mContext, CustomerDetailsActivity.class);
                    intent.setFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                    intent.putExtra("Company", company);
                    intent.putExtra("Customer Name", fullName);
                    intent.putExtra("City", city);
                    intent.putExtra("Job Title", jobTitle);
                    intent.putExtra("Website", website);
                    intent.putExtra("Office Phone", officePhone);
                    intent.putExtra("Mobile Phone", mobilePhone);
                    intent.putExtra("Office Email", officeEmail);
                    intent.putExtra("Home Email", homeEmail);
                    intent.putExtra("Fax", fax);
                    intent.putExtra("Address 1", address1);
                    intent.putExtra("State", state);
                    intent.putExtra("Postal Code", postalCode);
                    intent.putExtra("Country", country);
                    intent.putExtra("Address 1 Shipping", shippingAddress1);
                    intent.putExtra("City Shipping", shippingCity);
                    intent.putExtra("State Shipping", shippingState);
                    intent.putExtra("Postal Code Shipping", shippingPostalCode);
                    intent.putExtra("Country Shipping", shippingCountry);
                    mContext.startActivity(intent);
                }
            });
        } catch (JSONException e) {
            Log.e(CustomerAdapter.class.getName(), "Error: " + e);
        }
    }

```

[![Debug the intent extras][1]][1]

[1]: http://throw.rocks/fm-invoices/16_intent/intent_01_test_intent_extras.png

#### Get the intent extras

```java
@Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_customer_details);
        Intent intent = getIntent();
        final String company = intent.getStringExtra("Company");
        final String fullName = intent.getStringExtra("Customer Name");
        final String city = intent.getStringExtra("City");
        final String jobTitle = intent.getStringExtra("Job Title");
        final String website = intent.getStringExtra("Website");
        final String officePhone = intent.getStringExtra("Office Phone");
        final String mobilePhone = intent.getStringExtra("Mobile Phone");
        final String fax = intent.getStringExtra("Fax");
        final String officeEmail = intent.getStringExtra("Office Email");
        final String address1 = intent.getStringExtra("Address 1");
        final String state = intent.getStringExtra("State");
        final String postalCode = intent.getStringExtra("Postal Code");
        final String country = intent.getStringExtra("Country");
        final String shippingAddress1 = intent.getStringExtra("Address 1 Shipping");
        final String shippingCity = intent.getStringExtra("City Shipping");
        final String shippingState = intent.getStringExtra("State Shipping");
        final String shippingPostalCode = intent.getStringExtra("Postal Code Shipping");
        final String shippingCountry = intent.getStringExtra("Country Shipping");
    }
```

#### Set the views with the intent extras

```java
    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_customer_details);
        Intent intent = getIntent();
        final String company = intent.getStringExtra("Company");
        final String fullName = intent.getStringExtra("Customer Name");
        final String city = intent.getStringExtra("City");
        final String jobTitle = intent.getStringExtra("Job Title");
        final String website = intent.getStringExtra("Website");
        final String officePhone = intent.getStringExtra("Office Phone");
        final String mobilePhone = intent.getStringExtra("Mobile Phone");
        final String fax = intent.getStringExtra("Fax");
        final String officeEmail = intent.getStringExtra("Office Email");
        final String address1 = intent.getStringExtra("Address 1");
        final String state = intent.getStringExtra("State");
        final String postalCode = intent.getStringExtra("Postal Code");
        final String country = intent.getStringExtra("Country");
        final String shippingAddress1 = intent.getStringExtra("Address 1 Shipping");
        final String shippingCity = intent.getStringExtra("City Shipping");
        final String shippingState = intent.getStringExtra("State Shipping");
        final String shippingPostalCode = intent.getStringExtra("Postal Code Shipping");
        final String shippingCountry = intent.getStringExtra("Country Shipping");
        TextView headerCompanyView = (TextView) findViewById(R.id.customer_detail_header_company);
        TextView headerCustomerView = (TextView) findViewById(R.id.customer_detail_header_customer);
        TextView companyView = (TextView) findViewById(R.id.customer_detail_company);
        TextView jobTitleView = (TextView) findViewById(R.id.customer_detail_job_title);
        TextView websiteView = (TextView) findViewById(R.id.customer_detail_website);
        TextView countInvoicesView = (TextView) findViewById(R.id.customer_detail_count_invoices);
        TextView officePhoneView = (TextView) findViewById(R.id.customer_detail_phone_office);
        TextView mobilePhoneView = (TextView) findViewById(R.id.customer_detail_phone_mobile);
        TextView faxView = (TextView) findViewById(R.id.customer_detail_fax);
        TextView officeEmailView = (TextView) findViewById(R.id.customer_detail_office_email);
        TextView homeEmailView = (TextView) findViewById(R.id.customer_home_email);
        TextView address1View = (TextView) findViewById(R.id.customer_detail_address1);
        TextView address2View = (TextView) findViewById(R.id.customer_detail_address2);
        TextView cityView = (TextView) findViewById(R.id.customer_detail_city);
        TextView stateView = (TextView) findViewById(R.id.customer_detail_state);
        TextView postalCodeView = (TextView) findViewById(R.id.customer_detail_postal_code);
        TextView countryView = (TextView) findViewById(R.id.customer_detail_country);
        headerCompanyView.setText(company);
        headerCustomerView.setText(fullName);
        companyView.setText(company);
        jobTitleView.setText(jobTitle);
        websiteView.setText(website);
        countInvoicesView.setText("0");
        officePhoneView.setText(officePhone);
        mobilePhoneView.setText(mobilePhone);
        faxView.setText(fax);
        officeEmailView.setText(officeEmail);
        homeEmailView.setText(homeEmail);
        address1View.setText(address1);
        address2View.setText("");
        cityView.setText(city);
        stateView.setText(state);
        postalCodeView.setText(postalCode);
        countryView.setText(country);
    }
}
```

[![Set the customer detail views][2]][2]

[2]: http://throw.rocks/fm-invoices/16_intent/intent_02_set_customer_details_data.png

#### Congratulations!

<br/>
<hr/>
<br/>

Next: <a href="/fm-invoices-common-intents.html">Common Intents: Send Email, Dial Phone Number, Open Website, and Show Maps</a>