# Analytics

A pluggable analytics library designed to work with any third party analytics tool.

Connect with your favorite analytic providers, trigger custom logic based on user activity, or easily provide opt out mechanisms for visitors who wish to turn off analytics entirely.

## Table of Contents
<!-- AUTO-GENERATED-CONTENT:START (TOC:collapse=true&collapseText=Click to expand) -->
<details>
<summary>Click to expand</summary>

- [Features](#features)
- [Why](#why)
- [Philosophy](#philosophy)
- [Install](#install)
- [Usage](#usage)
- [Demo](#demo)
- [API](#api)
  * [analytics.identify](#analyticsidentify)
  * [analytics.track](#analyticstrack)
  * [analytics.page](#analyticspage)
  * [analytics.getState](#analyticsgetstate)
  * [analytics.reset](#analyticsreset)
  * [analytics.dispatch](#analyticsdispatch)
  * [analytics.storage](#analyticsstorage)
  * [analytics.storage.getItem](#analyticsstoragegetitem)
  * [analytics.storage.setItem](#analyticsstoragesetitem)
  * [analytics.storage.removeItem](#analyticsstorageremoveitem)
  * [analytics.user](#analyticsuser)
  * [analytics.ready](#ready)
  * [analytics.on](#analyticson)
  * [analytics.once](#analyticsonce)
  * [analytics.enablePlugin](#analyticsenableplugin)
  * [analytics.disablePlugin](#analyticsdisableplugin)
  * [analytics.loadPlugin](#analyticsloadplugin)
  * [EVENTS](#events)
  * [CONSTANTS](#constants)
- [Analytic plugins](#analytic-plugins)
- [Creating analytics plugins](#creating-analytics-plugins)
  * [React to any event](#react-to-any-event)
  * [(optionally) Use middleware](#optionally-use-middleware)
  * [Opt out example plugin](#opt-out-example-plugin)
- [Plugin Naming Conventions](#plugin-naming-conventions)
- [CONTRIBUTING](#contributing)

</details>
<!-- AUTO-GENERATED-CONTENT:END -->

## Features

- [x] [Pluggable](#analytic-plugins) - Bring your own third party tool
- [x] Works on client & server-side
- [x] Test & Debug analytics integrations with time travel & offline mode.
- [ ] (WIP) In client, works offline. Queues events to send when connection resumes

##  Why

Companies frequently change their analytics requirements and add/remove services to their sites and applications. This can be a time consuming process integrating over and over again with N number of third party tools.

This library solves that.

##  Philosophy

> You should never be locked into a tool.

To add or remove an analytics provider adjust the `plugins` you load into `analytics`.

## Install

```bash
npm install analytics --save
```

## Usage

```js
import Analytics from 'analytics'
import googleAnalyticsPlugin from 'analytics-plugin-ga'
import customerIOPlugin from 'analytics-plugin-customerio'

const analytics = Analytics({
  app: 'my-app-name',
  version: 100,
  plugins: [
    googleAnalyticsPlugin({
      trackingId: 'UA-121991291',
    }),
    customerIOPlugin({
      siteId: '123-xyz'
    })
  ]
})

// page tracking
analytics.page()
// event tracking
analytics.track('userPurchase', {
  price: 20
})
// identifying users
analytics.identify('user-id-xyz', {
  firstName: 'bill',
  lastName: 'murray',
  email: 'da-coolest@aol.com'
})
//...
```

## Demo

See [Analytics Demo](https://analytics-demo.netlify.com/) for a site example.

## API

<!-- AUTO-GENERATED-CONTENT:START (API_DOCS) -->
### analytics.identify

Identify a user. This will trigger `identify` calls in any installed plugins and will set user data in localStorage

**Arguments**

- **userId** <code>String</code> - Unique ID of user
- **traits** <code>Object</code> - Object of user traits
- **options** <code>Object</code> - Options to pass to indentify call
- **callback** <code>Function</code> - Optional callback function after identify completes

**Example**

```js
identify('xyz-123', {
  name: 'steve',
  company: 'hello-clicky'
})
```

### analytics.track

Track an analytics event. This will trigger `track` calls in any installed plugins

**Arguments**

- **eventName** <code>String</code> - Event name
- **payload** <code>Object</code> - Event payload
- **options** <code>Object</code> - Event options
- **callback** <code>Function</code> - Callback to fire after tracking completes

**Example**

```js
analytics.track('buttonClick')
```

### analytics.page

Trigger page view. This will trigger `page` calls in any installed plugins

**Arguments**

- **data** <code>String</code> - (optional) page data
- **options** <code>Object</code> - Event options
- **callback** <code>Function</code> - Callback to fire after page view call completes

**Example**

```js
analytics.page()
```

### analytics.getState

Get data about user, activity, or context. You can access sub-keys of state with `dot.prop` syntax.

**Arguments**

- **key** <code>String</code> - (optional) dotprop sub value of state

**Example**

```js
// Get the current state of analytics
analytics.getState()

// Get a subpath of state
analytics.getState('context.offline')
```

### analytics.reset

Clear all information about the visitor & reset analytic state.

**Arguments**

- **callback** <code>Function</code> - Handler to run after reset


### analytics.dispatch

Emit events for other plugins or middleware to react to.

**Arguments**

- **action** <code>Object</code> [description]


### analytics.storage

Storage utilities for persisting data. These methods will allow you to save data in localStorage, cookies, or to the window.


### analytics.storage.getItem

Get value from storage

**Arguments**

- **key** <code>String</code> - storage key
- **options** <code>Object</code> - storage options

**Example**

```js
analytics.storage.getItem('storage_key')
```

### analytics.storage.setItem

Set storage value

**Arguments**

- **key** <code>String</code> - storage key
- **value** <a href="Any.html">Any</a> - storage value
- **options** <code>Object</code> - storage options

**Example**

```js
analytics.storage.setItem('storage_key', 'value')
```

### analytics.storage.removeItem

Remove storage value

**Arguments**

- **key** <code>String</code> - storage key
- **options** <code>Object</code> - storage options

**Example**

```js
analytics.storage.removeItem('storage_key')
```

### analytics.user

Get user data

**Arguments**

- **key** <code>String</code> - dot.prop subpath of user data

**Example**

```js
// get all user data
const userData = analytics.user()

// get user company name
const companyName = analytics.user('company.name')
```

### analytics.ready

Fire callback on analytics ready event

**Arguments**

- **callback** <code>Function</code> - function to trigger when all providers have loaded

**Example**

```js
analytics.ready((action, instance) => {
  console.log('all integrations have loaded')
})
```

### analytics.on

Attach an event handler function for one or more events to the selected elements.

**Arguments**

- **name** <code>String</code> - Name of event to listen to
- **callback** <code>Function</code> - function to fire on event

**Example**

```js
analytics.on('track', (action, instance) => {
  console.log('track call just happened. Do stuff')
})
```

### analytics.once

Attach a handler function to an event and only trigger it only once.

**Arguments**

- **name** <code>String</code> - Name of event to listen to
- **callback** <code>Function</code> - function to fire on event

**Example**

```js
analytics.once('track', (action, instance) => {
  console.log('This will only triggered once')
})
```

### analytics.enablePlugin

Enable analytics plugin

**Arguments**

- **name** <code>String</code>|<code>Array</code> - name of integration(s) to disable
- **callback** <code>Function</code> - callback after enable runs

**Example**

```js
analytics.enablePlugin('google')

// enable multiple integrations at once
analytics.enablePlugin(['google', 'segment'])
```

### analytics.disablePlugin

Disable analytics plugin

**Arguments**

- **name** <code>string</code>|<code>array</code> - name of integration(s) to disable
- **callback** <code>Function</code> - callback after disable runs

**Example**

```js
analytics.disablePlugin('google')

analytics.disablePlugin(['google', 'segment'])
```

### analytics.loadPlugin

Load registered analytic providers.

**Arguments**

- **namespace** <code>String</code> - integration namespace

**Example**

```js
analytics.loadPlugin('segment')
```

### EVENTS

Core Analytic events. These are exposed for third party plugins & listeners
Use these magic strings to attach functions to event names.


### CONSTANTS

Core Analytic constants. These are exposed for third party plugins & listeners
<!-- AUTO-GENERATED-CONTENT:END -->

## Analytic plugins

- [Google Analytics](https://www.npmjs.com/package/analytics-plugin-ga)
- [Customer.io](https://www.npmjs.com/package/analytics-plugin-customerio)
- [Original Source Plugin](https://www.npmjs.com/package/analytics-plugin-original-source)
- Add yours! 👇

## Creating analytics plugins

The library is designed to work with any third party analytics tool.

Plugins are just plain javascript objects that expose methods for `analytics` core to register and call.

Here is a quick example of a plugin. This is a 'vanilla' plugin example for connecting a third party analytics tool

```js
// vanilla-example.js
export default function googleAnalytics(userConfig) {
  // return object for analytics to use
  return {
    // All plugins require a namespace
    NAMESPACE: 'google-analytics',
    config: {
      whatEver: userConfig.fooBar,
      googleAnalyticsId: userConfig.id
    },
    initialize: function() {
      // load provider script to page
    },
    page: function() {
      // call provider specific page tracking
    },
    track: function() {
      // call provider specific event tracking
    },
    identify: function() {
      // call provider specific user identify method
    },
    loaded: () => {
      // return boolean so analytics knows when it can send data to third party
      return !!window.gaplugins
    }
  }
}
```

To use this plugin above, import it and pass it into the `plugins` array when you bootstrap analytics.

```js
import Analytics from 'analytics'
import vanillaExample from './vanilla-example.js'

const analytics = Analytics({
  app: 'my-app-name',
  plugins: [
    vanillaExample({ id: 'xyz-123' }),
    ...otherPlugins
  ]
})
```

### React to any event

Plugins can react to any event flowing through `analytics`. For example, if you wanted to trigger custom logic when `analytics` bootstraps you can attach a function handler to `initialize`.

For a full list of core events, checkout `events.js`.

```js
// plugin.js
export default function firstSource(userConfig) {
  return {
    NAMESPACE: 'first-source',
    // Run function on `initialize` event
    initialize: (action, instance) => {

      // Do whatever

      // (optionally) Dispatch additional events for other plugins to react to
      instance.dispatch({
        type: 'setOriginalSource',
        originalSource: getOriginalSource(userConfig),
        originalLandingPage: getOriginalLandingPage(userConfig)
      })
    }
  }
}
```

Using this plugin is the same as any other.

```js
import Analytics from 'analytics'
import myPlugin from './plugin.js'

const analytics = Analytics({
  app: 'my-app-name',
  plugins: [
    myPlugin(),
    ...otherPlugins
  ]
})
```

### (optionally) Use middleware

Alternatively, you can also add whatever middleware functionality you'd like from the redux ecosystem.

```js
// logger-plugin.js redux middleware
const logger = store => next => action => {
  if (action.type) {
    console.log(`>> dispatching ${action.type}`, JSON.stringify(action))
  }
  let result = next(action)

  return result
}

export default logger
```

Using this plugin is the same as any other.

```js
import Analytics from 'analytics'
import loggerPlugin from './logger-plugin.js'

const analytics = Analytics({
  app: 'my-app-name',
  plugins: [
    ...otherPlugins,
    loggerPlugin
  ]
})
```

### Opt out example plugin

This is a vanilla redux middleware that will opt out users from tracking. There are **many** ways to implement this type of functionality using `analytics`

```js
const optOutPlugin = store => next => action => {
  const { type } = action
  if (type === 'trackStart' || type === 'pageStart' || type === 'trackStart') {
    // Check cookie/localStorage/Whatever to see if visitor opts out

    // Here I am checking user traits persisted to localStorage
    const { user } = store.getState()
    // user has optOut trait cancel action
    if (user && user.traits.optOut) {
      return next({
        ...action,
        ...{
          abort: true,
          reason: 'User opted out'
        },
      })
    }
  }
  return next(finalAction)
}

export default optOutPlugin
```

##  Plugin Naming Conventions

Plugins should follow a naming convention before being published to NPM

```
analytics-plugin-{your-plugin-name}

npm install analytics-plugin-awesome-thing
```

# CONTRIBUTING

Contributions are always welcome, no matter how large or small. Before contributing,
please read the [code of conduct](CODE_OF_CONDUCT.md).
