---
title: Flow js
date: 2019-01-07 20:36:58 +0000

---
# flow.js

Flow.js is a JavaScript library that supplements an integration to Flow by making it easy to localize your existing shopping experience. It's designed to work in the browser along side your existing implementation.

With flow.js you'll be able to:

- Manage the Flow experience for a user's session
- Localize Prices and text
- Manage the Flow cart

## Setup and Initialization

See below for the script that needs to be added to your site. Be sure to replace `{flow_organization}` with the appropriate organization from Flow and `{myshopify_domain}` with the internal Shopify domain for your shop.

```html
<script>
(function (f, l, o, w, i, n, g) {
  f[i] = f[i] || {};
  f[i].set = f[i].set || function () {
    (f[i].q = f[i].q || []).push(arguments);
  };
  f[i].initialized = 0;
  f[i].fraud = 'enabled';
  n = l.createElement(o);
  n.src = w;
  n.async = 1;
  g = l.getElementsByTagName(o)[0];
  g.parentNode.insertBefore(n, g);
})(window, document, 'script', 'https://shopify-cdn.flow.io/{flow_organization}/js/v0/flow.js?shop={myshopify_domain}', 'Flow');
</script>
```

Currently only Shopify integrations are supported.

When initialized, the following things will happen:

1. Check to see that a session exists
  1. If not, it will create one via an API call and store the resulting data in a cookie and localStorage.
1. Localize any prices on the page (any HTML tag with the `flow-price` attribute)
1. Localize any text on the page (any HTML tag with the `flow-text` attribute)
1. Trigger the `ready` event

Flow.js exposes itself in the global namespace as: `Flow`.

### Fraud

Fraud monitoring is enabled by default. To disable it, remove the following line from the JavaScript tag.

```javascript
f[i].fraud = 'enabled';
```

### Manual Initialization

By default flow.js will attempt to auto initialize itself. Sometimes there are exceptional situations, like race conditions with other JavaScript code, that make this not desirable. To manually initialize flow.js, add the following line immediately after the main script tag. This will allow you to call `Flow.init()` when you want to (see below for more details on how to call init).

```javascript
Flow.manualInit = true;
```

`Flow.init()` won't exist in the browser's JavaScript environment until flow.js has fully loaded on the page. An event is fired when flow.js is loaded and `Flow.init` is accessible. To subscribe to the event, do the following:

```JavaScript
Flow.set('on', 'loaded', () => Flow.init({ force: true }));
```

It's recommended that you run this towards the bottom of your HTML document, before the closing `body` tag.

### Flow Initialization

Immediately after the script above is executed, events can be subscribed to from flow.js. The most useful event is `ready`, which means flow.js is fully initialized and the functions defined in later sections of this document are available for use. This is achieved via `Flow.set`:

```javascript
Flow.set('on', 'ready', () => {
  if (Flow.getExperience()) {
    // User is global and should use Flow for localization and cart.
  } else {
    // User is domestic and should use the native shopping experience.
  }
});
```

### Optin Target
By default when loading optin messages from the server, the messages will be appended to the body of the HTML document. To specify where in the DOM consent messages render add

```javascript
Flow.optinTarget = ELEMENT_CSS_SELECTOR;
```

at the end of, or immediately after, the flow.js tag. You can read about how to format the selector text [**here**](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelectorAll).

## Session

The session is how Flow keeps track of a visitor to your store over time. The cookie is long lasting and makes it possible to keep track of a user's cart between visits.

See [Organization Session](/type/organization-session) for documentation on the session model.

## Reference

See below for further documentation on the capabilities of flow.js.

- [Content](/shopify/guide/server-side-rendering) - Everything related to localizing the content of a page
- [Events](/shopify/events) - All of the events flow.js emits
- [Cart](/shopify/cart) - Cart updates and utilities
- [Variants](/shopify/variants) - Localized variant helpers
- [Orders](/shopify/orders) - Localized variant helpers
- [Cache](/shopify/variants) - Localized data cache
- [Format](/shopify/format) - Formatting utilities
- [Session](/shopify/session) - Session data utilities

## options

Many of the functions listed above will take an `options` object. It contains the callback functions for any API calls that are made behind the scenes. The `success` and `error` props will be used to return the data from the API. The `status` parameter may be null if the data was cached and no API call was made.

```javascript
Flow.cart.getCart(options) {
  success: function (status, cart) {
    // status will usually be a 2xx value.
    // cart: actual data returned from the API
  },
  error: function (status, error) {
    // status will usually be a 422 or 5xx value
    // An Error object describing what went wrong.
  }
}
```