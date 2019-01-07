---
title: Server Side Rendering
date: 2019-01-07 21:25:48 +0000

---
# Localizing Prices Using Metafields
{% raw %}

This guide will walk you through how to leverage variant metafields from Flow to render localized prices in Liquid templates, the recommended way to integrate Flow pricing into Shopify stores.

## Previous localization approaches

Our integration is constantly evolving. The current iteration is what we call Server Side Rendering 2.0. This update increases the efficiency of synchronizing product variant data, offering one metafield containing all experience data for a single variant. If you are currently using a previous version of localization, please refer to the deprecated documentation below.

- [Client Side Localization](#)
- [Server Side Rendering 1.0](#)

## Server Side Rendering

We set an attribute on the Shopify Cart named `flow_metafield_key` containing the namespace for variant metafields. This attribute is accessible in all Liquid templates of a theme. The namespace is a mapping to an experience defined in Flow. For each variant a `flow_detail` metafield will exist containing all of the information needed to display the correct price based on the `flow_metafield_key` cart attribute. Using the snippets provided by Flow, the pricing information can be extracted and utilized within the theme.

## How It Works

### Sessions and Flow.js in the Browser

When a customer visits your website, Flow.js will establish a [session](https://docs.flow.io/type/session) with Flow. If Flow is enabled, the customer is assigned an [experience](https://docs.flow.io/type/experience). 

To support server side rendering, Flow.js will detect the `flow_metafield_key` attribute on the session and set the _same attribute_ on the Shopify cart. This allows a mapping to the experience defined in Flow and is then used in Liquid templates.

### Server Side

All of the information from Flow is stored in a single String metafield. The data is compressed into a single value to ensure the best performance when syncing information between Flow and Shopify. There are rate limits on how quickly data can be provided via the Shopify API and having a single metafield per variant guarantees a deterministic amount of time to sync pricing information.

The raw data is stored as a single metafield in the `flow_detail` namespace. Here is an example of how to get the data and its structure:

Retrieve the _first_ metafield in the `flow_detail` namespace and assign the value to `flow_metafields_payload`.

```liquid
{%- assign flow_metafields_payload = variant.metafields['flow_detail'] | first -%}
```

The value of the metafield:

```
1||flow_ivxxpbremn;flow_c17phumisl;flow_q3azvr4jyh;flow_ug5whsyxom;flow_gb3b497r6b;flow_u1vfevsl7n;flow_p6xa9fkqcv;flow_akyorqtxmn;flow_zew5ybvzpj;flow_fwkkjwppio;flow_ghqlur8m2m;flow_lbvk2vbzlg;flow_rk0ijzexzl;flow_xh7fv20xzm;flow_xj9zleh3tr|ARS;7.270,50 ARS;Includes VAT;9.825,00 ARS;;;2.101,45 ARS;i;|ARS;7.265,31 ARS;Includes VAT;9.818,00 ARS;;;;i;|AUD;A$256.78;Includes VAT and duty;A$347.00;;;;i;|BRL;R$579,42;;R$783,00;;;;i;|XOF;CFA88,823;Includes VAT;CFA120,031;;;;i;|CAD;CA$231.17;Includes VAT;CA$314.99;;;CA$36.42;i;|CNY;CN¥1,185.48;Includes VAT;CN¥1,602.00;;;;i;|EUR;166,75 €;Includes VAT;226,95 €;;;;i;|EUR;146,48 €;;197,95 €;32,81 €;VAT;17,58 €;i;|EUR;165,33 €;Includes VAT;224,95 €;;;;i;|JPY;¥27,700;Includes VAT;;;;;i;|EUR;168,18 €;Includes VAT;228,95 €;;;;i;|PHP;8,140.00 PHP;;11,000.00 PHP;;;;i;|GBP;£118.39;;£159.99;£26.52;VAT;;i;|USD;US$148.00;;US$200.00;US$0.00;ST;US$0.00;i;
```

The value is pipe (`|`) and semicolon (`;`) separated and contains all of the pricing information for all experiences defined in Flow.

## Rendering Localized Pricing

When rendering localized pricing, you must account for all of the settings that can be managed in [console](https://console.flow.io). Simply rendering the item price from the metafield is not enough. Tax and duty display settings must also be accounted for. To remove any friction surrounding these settings, Flow has created utility Liquid template snippets that encompass all of this logic.

- [flow_metafields_prices.liquid](http://cdn.flow.io/docs/shopify/flow-utility-snippets/flow_metafields_prices.liquid)
- [flow_localized_price.liquid](http://cdn.flow.io/docs/shopify/flow-utility-snippets/flow_localized_price.liquid)

This is intended to be used in place of the actual price you would render without Flow.

**Without Flow**
```liquid
<span class="money">{{ variant.price }}</span>
```

**With Flow**

Replace span above with:

```liquid
{% include 'flow_localized_price', flow_tag_display: 'item_price' %}
```
## First time visitors

First time visitors will not have a Flow session established server side. To account for this, the `flow_localized_price.liquid` snippet will add standard `flow-*` attributes. You can rely on flow.js to fetch the correct pricing from the API and localize the price elements client side. This will have some latency and the price may flicker, but will only happen the first time a user visits the website.

## Verification

Server side rendering of Flow pricing should now be working. To verify that it is actually happening server side, check for the presence of the `flow-localized` on the price elements. View the source of a page that is rendering prices (use 'view page source' option, as flow.js will add it client side if the class is missing). Search for the `flow-localized` class that is added to price elements by the Liquid snippet. If you are seeing this class in the source of the page, it means it was localized server side.

If you do not see the `flow-localized` class in the HTML document try the following:

1. Make sure the attribute `flow_metafield_key` exists in the shopify cart.
2. Make sure metafields with a namespace of the value of `flow_detail` exists on the variant. You can see the raw data for a variant by looking at it in the admin: https://{shop_id}.myshopify.com/admin/products/{product_id}/variants/{variant_id}/metafields.json

If the cart attribute or metafields seem incorrect contact the Tech Lead at Flow.

## Snippets Explained

### Installation

To install and utilize the snippets, download each by clicking the links below.

- [flow_metafields_prices.liquid](http://cdn.flow.io/docs/shopify/flow-utility-snippets/flow_metafields_prices.liquid)
- [flow_localized_price.liquid](http://cdn.flow.io/docs/shopify/flow-utility-snippets/flow_localized_price.liquid)

Then simply add both files to the snippet folder in your theme. Once that is done, you can now include them wherever needed.

## flow_localized_price snippet ([Snippet](http://cdn.flow.io/docs/shopify/flow-utility-snippets/flow_localized_price.liquid))

The `flow_localized_price` snippet can be leveraged to render Flow formatted markup for localization data.

```liquid
  {% include 'flow_localized_price', flow_tag_display: 'item_price' %}
```

would render

```html
<span class="flow-price flow-price__item flow-localized " flow-variant="32353637185" flow-selector="prices.item.label" flow-default="$148.00">7.270,50 ARS</span>
```

### Usage

```
{% include 'flow_localized_price' with [variant], flow_tag_display: [string], classnames: [string] %}
```

### with [variant]

If no variant is provided to `flow_localized_price`, the snippet will use [`product.selected_or_first_available_variant`](https://help.shopify.com/en/themes/liquid/objects/product#product-selected_or_first_available_variant). If another variant should be used, it can be passed using `with`.

**Example**

```
{% include 'flow_localized_price' with selectedVariant %}
```

## variables

### `flow_tag_display`

`flow_localized_price` can be provided with a varaible named `flow_tag_display` as a string parameter indicating which piece of localization data to render. Valid values for `flow_tag_display` are,
+ item_price - the localized item price of the variant
+ price_includes - included cost in the item price (tax and or duty)
+ compare_at - the localized compare_at price of the variant
+ tax - the localized tax for a variant including tax amount and tax name
+ duty - the localized duty for a variant including the amount and duty name

If no `flow_tag_display` is provided, the snippet will default to `item_price`.

**Examples**

`item_price`

liquid:
```liquid
  {% include 'flow_localized_price', flow_tag_display: 'item_price' %}
```
rendered markup:
```html
<span class="flow-price flow-price__item flow-localized " flow-variant="32353637185" flow-selector="prices.item.label" flow-default="$148.00">
  7.270,50 ARS
</span>
```

`price_includes`

Liquid:
```liquid
  {% include 'flow_localized_price', flow_tag_display: 'price_includes' %}
```
rendered markup:
```html
<span class="flow-price flow-price__includes flow-localized " flow-variant="32353637185" flow-selector="prices.includes.label">
  Includes VAT and duty
</span>
```

`compare_at`

Liquid:
```liquid
  {% include 'flow_localized_price', flow_tag_display: 'compare_at' %}
```
rendered markup:
```html
<span class="flow-price flow-price__compare-at flow-localized " flow-variant="32353637185" flow-selector="prices.compare_at.label" flow-default="$200.00">
  A$347.00
</span>
```

`tax`

Liquid:
```liquid
  {% include 'flow_localized_price', flow_tag_display: 'tax' %}
```
rendered markup:
```html
<span>
  <span class="flow-price flow-price__vat flow-localized " flow-variant="32353637185" flow-selector="prices.vat.label">
    25,00 €
  </span>
  <span class="flow-price flow-price__vat-name flow-localized " flow-variant="32353637185" flow-selector="prices.vat.name">
    VAT
  </span>
</span>
```

`duty`

Liquid:
```liquid
  {% include 'flow_localized_price', flow_tag_display: 'duty' %}
```
rendered markup:
```html
  <span>
    <span class="flow-price flow-price__duty flow-localized" flow-variant="32353637185" flow-selector="prices.duty.label">
      CA$40.00
    </span>
    <span class="flow-price flow-price__duty-name flow-localized " flow-variant="32353637185" flow-selector="prices.duty.name">
      Duty
    </span>
  </span>
```

### `classnames`

`classnames` can be provided to `flow_localized_snippet` if additional class names need to be applied to the rendered markup output from the snippet. For example,

```liquid
  {% include 'flow_localized_price', classnames: 'foo bar baz' %}
```

would render

```html
  <span class="flow-price flow-price__item flow-localized foo bar baz" flow-variant="32353637185" flow-selector="prices.item.label" flow-default="$148.00">
    7.270,50 ARS
  </span>
```

## flow_metafields_prices snippet ([Snippet](http://cdn.flow.io/docs/shopify/flow-utility-snippets/flow_metafields_prices.liquid))

The `flow_metafields_prices` snippet can be leveraged to parse through the new metafields and provide variables of this data to use within your own Liquid markup.

```liquid
  {% include 'flow_metafields_prices' with ShopifyVariantObject %}
```

Use `with` to assign a variable to use within the snippet. It's expected that the value is the variant to localize.

Once this is done, the snippet will provide the following Liquid variables

Variable  | Description
--------- | ---------------
flow_localized_currency | The localized currency of the item. Will be the 3 letter currency code
flow_localize_price | The localized price label. Will include the currency symbol or country code as configured in console
flow_localized_price_includes | If the price is configured to have tax or duty included, this value will be the localized text. ex: `Includes Duty` or `Includes HST and Duty`
flow_localized_compare_at | The localized compare_at price label. Will include the currency symbol or country code as configured in console
flow_localized_tax | The localized tax amount label. Will include the currency symbol or country code as configured in console. ex: `CA$24.95`
flow_localized_tax_name | The localized tax name. ex: `HST`
flow_localized_duty | The localized duty amount. Will include the currency symbol or country code as configured in console. ex: `CA$1.35`
flow_localized_status | The status of the item. Will be one of the [subcatalog_item_status](/type/subcatalog-item-status) values.
flow_localized_inventory_status | The inventory status of the variant. Will be one of the [experience_inventory_item_status](/type/experience-inventory-item-status) values.
{% endraw %}
