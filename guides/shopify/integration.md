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

### 1. Install the Flow Connect and Flow Checkout apps ([Guide](/forrestry-jekyll-demo/guides/shopify/shopify-app-install "Guide"))

This will connect your store to Flow, allowing products to be synced. For each item, duties and taxes will be calculated and localized versions of the items will be created for each Experience defined in Flow. After all of the items are localized, Flow's service will push updates to Shopify as metafields on each product variant. The metafield will will contain pricing for all defined experiences.

### 2. Add `flow.js` to your Shopify theme ([Guide](/forrestry-jekyll-demo/guides/shopify/flow-js))

`flow.js` is the JavaScript library required for the front end of your store. It will enable localization, cart, and fraud functionality. On each page load, we will geolocate the user and pick the best experience (currency, usually) for the user based on where they are shopping from. With this added to the Shopify theme you can proceed with localizing the content and get the first test order placed. Also of note, flow.js has Beacon analytics built in, so there is no need to go through any additional installation process to take advantage of Beacon's API.

### 3. Use metafields to display localized pricing ([Guide](/forrestry-jekyll-demo/guides/shopify/server-side-rendering))

Flow recommends rendering localized pricing information in liquid templates directly, ensuring great performance and a smooth user experience.

_etc, etc, etc_