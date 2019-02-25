# Google Ads Conversion Tracking

This guide will walk you through configuring a Google Ads Gtag conversion for Shopify stores. This Conversion code will be configured to send revenue and the Order ID with each conversion.

---------------------------------------------------

## Overview

Setting up an AdWords conversion code requires a base Gtag to be installed on all pages, as well as an event snippet on the checkout success page. The following information is necessary before this setup can be completed:

  - Google Ads Conversion ID (Account specific)
  - Google Ads Conversion Label (Conversion Specific)

The conversion ID can be found within the base Gtag snippet for the Google Ads account. The Event Snippet for the specific conversion contains all of the required information. The following snippet shows where to find both of these values in an Event Snippet (Labeled `CONVERSION_ID` and `CONVERSION_LABEL` respectively):

```html
<script>
  gtag('event', 'conversion', {
      'send_to': 'AW-CONVERSION_ID/CONVERSION_LABEL',
      'value': 0,
      'currency': 'USD',
      'transaction_id': ''
  });
</script>
```


### Step 1 - Installing the Gtag Base Code

To begin tracking conversions on a Shopify store, we must first configure a global site tag for our Google Ads account. If a Base Code was provided in the ticket, use that. You can also generate a new one with the template below, replacing the `CONVERSION_ID` value wherever it appears:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=AW-CONVERSION_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'AW-CONVERSION_ID');
</script>
```

Once you have the Global Site Tag base code prepared you can install it by following these steps:

1. Navigate to the **Online Store** section of the store
2. Click **Actions** > **Edit Code**
3. Open the `theme.liquid` file if it was not opened by default
4. Paste the base code within the `<head></head>` sections of the template file
5. Click **Save**

### Step 2 - Installing the Event Snippet

Shopify does not use the `theme.liquid` file on the checkout pages, so we will need to add the base Gtag snippet a second time during this step. The Event Snippet will require some customization before it can be installed, so it will be easiest to extract the Conversion ID and Label from the event snippet provided. Once you have these values use them to populate the following template, replacing `CONVERSION_ID` and `CONVERSION_LABEL` wherever they appear:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=AW-CONVERSION_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'AW-CONVERSION_ID');
</script>
{% if first_time_accessed %}
<script>
  gtag('event', 'conversion', {
      'send_to': 'AW-CONVERSION_ID/CONVERSION_LABEL',
      'value': {{ subtotal_price | money_without_currency }},
      'currency': 'USD',
      'transaction_id': '{{ order_number }}'
  });
</script>
{% endif %}
```

Once you have this template prepared, install it by following these steps:

1. Click **Settings** in the Shopify store
2. Click **Checkout**
3. Scroll down the page until you find a text box labeled **Additional Scripts**
4. Paste the populated template in the **Additional Scripts** section
5. Click **Save**

---------------------------------------------------

## Summary

This concludes the setup for Google Ads conversion tracking on Shopify stores. While we can't test purchase based conversions, you *can* test the frontend of the site to ensure that the Gtag base code was added to all pages using Tag Assistant.

#### References:
[Google Ads Conversion Tracking - Shopify Guide](https://help.shopify.com/en/manual/promoting-marketing/analyze-marketing/tracking-adwords-conversions)