---
title: Localized Pricing
date: 2019-01-07 20:57:33 +0000

---
# Displaying localized prices

One of the first things you'll want to do with Flow is display localized prices for global users browsing the website. When you installed the [Shopify App](/shopify/guide/shopify-app-install), metafields were added to all of the variants containing localized pricing for each experience defined in Flow.

The goal of this guide is to show you how to expose the metafield data to `flow.js` so that it can index the data and, with the help of some HTML attributes, use it to swap out domestic prices with localized versions.

## Exposing the metafield data in the DOM

To add the metafield data onto the page, store the metafield value JSON in an element. Any element is allowed, as long as the `flow-localized-variant` attribute exists.

```html
{% for localizedVariant in current_variant.metafields.flow_variant %}
<data
  flow-localized-variant
  value="{{ localizedVariant | last | escape }}"></data>
{% endfor %}
```

You can get the metafields from the product using `{% for localizedVariant in product.selected_or_first_available_variant.metafields.flow_variant %}`

flow.js will locate all `flow-localized-variant` elements on the page and cache the data so that it can be used to localize the rest of the elements on the page.

## Add HTML attributes to price elements

To print out a localized value for a variant, use `flow-variant` to target the variant by ID. Use `flow-selector` to declare which field to display as the innerHTML of the element. Add the `flow-default` attribute to have flow.js default to a value if no data exists for the path provided by the `flow-selector` attribute.

```html
<span
  flow-variant="{{ current_variant.id }}"
  flow-selector="prices.item.label"
  flow-default="{{ product.price | money }}"></span>
```

**Tip:** For compare at price elements, use the `prices.compare_at.label` selector instead.

## Testing localized prices

Any page with the correct data tags and price tags will automatically be localized based on which country the user is browsing from.

To test this, update your session to be from a country that is not domestic. Use a country that is currently mapped to an experience in the Flow Console. You can update the session one of two ways:

- Using the `?flow_country=<3_letter_country_code>` e.g, `?flow_country=CAN` for Canada.
- Using the flow.js API in the JavaScript console: `Flow.setCountry('CAN');`

After updating the session, refresh the page and you should see localized prices from Flow. If you do not see localized prices, double check the following:

- Make sure the `data` tags mentioned above are showing up in the HTML document with data in the values.
- Check to see that you have been assigned an experience by Flow using `Flow.getExperience()` in the JavaScript console.
  - Make sure experiences are defined at [console.flow.io](https://console.flow.io)
  - Choose a country in use by one of the experiences defined for the organization