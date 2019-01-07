---
title: page title
date: 2019-01-07 20:09:27 +0000
published: false

---
# Installing Shopify Applications

This guide will cover how to install the Shopify applications required for a successful integration with Flow. It relies on 3 applications:

- **Flow Connect** - A Shopify App that serves as the official connection between Flow and the store. First, the app enables syncing of products between Flow and Shopify so that they can be localized (tax, duty, and configured price rules). In addition to that we use the Shopify API to submit orders after a user completes the checkout process in Checkout UI.
- **Flow Checkout** - This is a Proxy Shopify App that creates a proxy page in Shopify to Flow's Checkout UI at checkout.flow.io. It makes it so that users can checkout on your store's main URL instead of the Flow one. The added bonus is that Hosted Cart and Checkout are embedded into the Shopify theme which means that it can be customized with the same CSS and JavaScript used in the rest of the shopping experience.
- **Private App** - A Private App allows Flow to take advantage of Shopify Plus APIs, such as Pricing Rules. In addition to that it improves the performance of the data exchange between Flow and Shopify. There are strict limits on the number of API calls that can be made concurrently.

## Flow Connect

To install the **Flow Connect Shopify App** visit the following URL, replacing `[shopify-store]` with your Shopify store domain:

```text
https://[shopify-store].myshopify.com/admin/oauth/authorize?client_id=5e24a86297ad509aa7fd1b1f4a86b829&scope=read_themes,write_themes,read_products,write_products,read_orders,write_orders,read_script_tags,write_script_tags,read_fulfillments,write_fulfillments,read_shipping,write_shipping,read_analytics&redirect_uri=https://shopify-connect.flow.io/app/auth/shopify/callback
```

Then, log in to [http://console.flow.io](http://console.flow.io) and select the Flow organization to sync.  The production Shopify shop should be associated with the Flow production organization and a development/sandbox Shopify shop, if applicable, should be associated with the Flow sandbox organization.  A message will display that the shop has been successfully connected.

Flow adds [metafields](https://help.shopify.com/themes/liquid/objects/metafield) to your shop's product variants with all of the localized information for each experience. This data enables Flow to localize your shop's content.

## Flow Checkout

To install the **Flow Checkout Shopify App** visit the following URL, replacing `[shopify-store]` with your Shopify store domain:

```text
https://[shopify-store].myshopify.com/admin/oauth/authorize?client_id=1183411b268b839edfbd62674afbe2df&scope=read_themes,write_themes,read_products,write_products,read_orders,write_orders,read_script_tags,write_script_tags,read_fulfillments,write_fulfillments,read_shipping,write_shipping,read_analytics&redirect_uri=https://checkout.flow.io/app/auth/shopify/callback
```

Once installed, no other setup is required.

## Private App

To create a private app:

1. Log in to your Shopify admin.
2. Create a new private app (URL: `https://[shopify-store].myshopify.com/admin/apps/private`)
3. In the **Description** section, enter **Flow**.
4. In the **Permissions** section, select:
    1. **Products, variants and collections** (_Read and Write_) - improve performance when syncing Shopify variant metafields
    2. **Orders, transactions and fulfillments** (_Read and Write_) - improve performance when syncing Shopify order metafields
    3. **Price rules** (_Read and Write_) - enable Shopify price Rules/discount capabilities within Flow
    4. **Fulfillment services** (_Read and Write_) - enables Flow to more efficiently work with fulfillments
    5. **Shipping rates, countries and provinces** (_Read and Write_) - enables Flow to more efficiently work with shipping
    6. **Customer details and customer groups** (_Read and Write_) - enables Flow to enhance customer metafields
5. When you're done, click **Save**. The API key and password are displayed.
6. Save Private App credentials in Flow - enables Flow to interact with Shopify APIs on behalf of your shop
    1. Navigate to [https://js.flow.io/shopify/private-app-form](https://js.flow.io/shopify/private-app-form) and you will be presented with a simple form.
    2. Enter organization id from Flow.  The organization id can be seen in the URL when selecting an organization (sandbox for development) in [Console](https://console.flow.io/) or retrieved via the [Organizations API](https://docs.flow.io/module/localization/resource/organizations#get-organizations).
    3. Enter Flow API Key.  Your API Key can be found or a new one created in Flow console [https://console.flow.io/:organization/organization/api-keys](https://console.flow.io/:organization/organization/api-keys) (Replace `:organization` with the Flow organization id).
    4. Enter Shopify Private App API Key as it was displayed in step #7.
    5. Enter Shopify Private App password as it was displayed in step #7.
    6. Click **Submit**.  If an error message is displayed, please verify information is correct.  If issues persist, please contact Flow for assistance.
