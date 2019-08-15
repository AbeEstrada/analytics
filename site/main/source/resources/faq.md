---
title: Frequently Asked Questions
description: Common questions asked about analytics
---

## Do I need to use a plugin?

No, analytics will work out of the box without any plugins attached. However the data from `page`, `track`, and `identify` calls won't send information anywhere.

You can attach your own data collection method via an inline plugin

```js
{
  NAMESPACE: 'custom-analytics-plugin',
  page: ({ payload }) => {
    const { properties, meta, anonymousId, userId } = payload
    // Send data to custom collection endpoint
    fetch('https://your-custom-collection-endpoint.com', {
      method: 'POST',
      body: JSON.stringify({
        date: meta.timestamp
        title: properties.title
        anonymousId: anonymousId,
        userId: userId,
      })
    })
  }
}
```

## How much bundle size will this add?

A major goal of this project is to keep things as lean as possible. You can view the size of analytics on [bundlephobia](https://bundlephobia.com/result?p=analytics@0.1.19).

If you're already using `redux` in your application code, the bundle will be even smaller 🎉.

## Does this work in client & on server?

Yes, analytics was designed to work in both browsers and servers.

## Other questions

Tweet Questions [@DavidWells](https://twitter.com/davidwells)
