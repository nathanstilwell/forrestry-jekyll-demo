---
default: 'true'
title: Integration
position: 1

---
# Shopify

Flow offers a suite of services to enable the best shopping experience for global visitors of your store. As part of the integration you will localize pricing so that users can shop with a familiar currency and use a localized cart and checkout hosted by Flow. This allows you to maintain a single store that can support multiple currencies where you would otherwise have to create multiple stores using default Shopify settings. At the end of the `Simple Integration` steps you will have done the minimum amount of work to start accepting orders from customers.

We recommend completing the `Simple Integration` immediately so that end-to-end testing of the backend integration can begin and not slow down overall development time. Afterwards, you can circle back and add customizations beyond the basic integration. Before beginning, create an account at [console.flow.io](https://console.flow.io). You will be invited to your sandbox organization, where you will create and configure worldwide experiences and settings.

When the Simple Integration steps are completed, see the `Other Enhancements` section for guides that cover common scenarios encountered by Flow clients.

## Simple Integration

Here we will cover the easiest option for end-to-end integration. After completing these steps, your Shopify store will have synced the product catalog with Flow and be able to display localized prices. International users will be able to add items to cart and check out using Flow's Checkout UI.

### 1. Install the Flow Connect and Flow Checkout apps ([Guide](forrestry-jekyll-demo/shopify/guide/shopify-app-install))

This will connect your store to Flow, allowing products to be synced. For each item, duties and taxes will be calculated and localized versions of the items will be created for each Experience defined in Flow. After all of the items are localized, Flow's service will push updates to Shopify as metafields on each product variant. The metafield will will contain pricing for all defined experiences.

### 2. Add `flow.js` to your Shopify theme ([Guide](/shopify/guide/shopify-flow-js-install))

`flow.js` is the JavaScript library required for the front end of your store. It will enable localization, cart, and fraud functionality. On each page load, we will geolocate the user and pick the best experience (currency, usually) for the user based on where they are shopping from. With this added to the Shopify theme you can proceed with localizing the content and get the first test order placed. Also of note, flow.js has Beacon analytics built in, so there is no need to go through any additional installation process to take advantage of Beacon's API.

### 3. Use metafields to display localized pricing ([Guide](/shopify/guide/server-side-rendering))

Flow recommends rendering localized pricing information in liquid templates directly, ensuring great performance and a smooth user experience.

### 4. Utilize Flow's Cart and Checkout ([Guide](/shopify/guide/cart-checkout-setup))

Since Flow does not have access to Shopify data, it is necessary to separately manage carts and orders. We recommend maintaining only the Flow cart for global shoppers. This prevents making duplicate calls to both the Shopify and Flow cart APIs. When a user proceeds to checkout, they will need to be redirected to Flow Checkout UI.

## Other Enhancements

After completing the `Simple Integration` steps above, clients often need to make further customizations to the shopping experience. Below are guides for commons scenarios encountered in the past.

### Using URL Parameters to Control Flow.js ([Guide](/shopify/guide/url-parameters))

An overview of the URL parameters supported by flow.js. Useful during the development phase to manipulate the flow session or force visitors into a specific experience for marketing purposes.

### Sending Localized Emails with Shopify ([Guide](/shopify/guide/localized-email))

This guide will walk you through how to enable localization of order data in emails sent via Shopify Liquid templates. Localized price data from Flow will be added to orders in Shopify as [metafields](https://help.shopify.com/themes/liquid/objects/metafield) and cart [line item properties](https://help.shopify.com/themes/liquid/objects/line_item#line_item-properties).

### Custom Content in Cart and Checkout ([Guide](/shopify/guide/cart-checkout-content))

A guide on how to add custom content to Flow's Hosted Cart and Checkout.

### Country Picker ([Guide](/shopify/guide/shopify-country-picker))

The Flow country picker is an included module in flowjs. It exposes the function `window.flow.countryPicker.createCountryPicker` which takes in an options object that will define its behaviour.

### Utilizing Events from flow.js ([Guide](/shopify/guide/events))

This guide provides examples of how to use events from `flow.js` to help with integration points such as analytics, cart, and other 3rd party integrations.

### Duty and Tax Messaging ([Guide](/shopify/guide/duty-vat-display))

Flow allows you to control the inclusion of duty and tax in item prices. Messaging can be added to each item in the browsing experience of your website. If tax and/or duty is included in the price for an item you can display a provided 'label' like "Duty and VAT Included".

### Associate flow_consumer_id with a customer session ([Guide](/shopify/guide/flow-consumer-id))

Include a tag in a Shopify template to associate a customer with a `flow_consumer_id`.

### Redirecting to checkout without using carts ([Guide](/shopify/guide/checkout-without-cart))

In this guide, we provide an example of how to drive users to checkout with a single click. Common use cases include 'Buy Now' functionality on product detail pages or via affiliate partnerships.

### Dynamically insert text based on the country or experience ([Guide](/shopify/guide/features))

String Features can be created to localize text based on experience or geolocation