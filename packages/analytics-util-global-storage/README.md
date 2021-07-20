<!--
title: Javascript Global Storage Utilities
pageTitle: GlobalStorage Utils
description: Utility library for managing global values
-->

# Global Storage Utilities

[Tiny](https://bundlephobia.com/result?p=@analytics/global-storage-utils) Global Storage utilities library for [analytics](https://npmjs.com/package/analytics) & whatever else 🌈

This will work with [analytics](https://getanalytics.io) or as a standalone import in your code.

[See live demo](https://utils-global-storage.netlify.app/).

## How to install

Install `@analytics/global-storage-utils` from [npm](https://www.npmjs.com/package/@analytics/globalstorage-utils).

```bash
npm install @analytics/global-storage-utils
```

## API

Below is the api for `@analytics/global-storage-utils`. These utilities are tree-shakable.

```js
import { get, set, remove } from '@analytics/global-storage-utils'

set('key', 'value')

const val = get('key')
console.log(val) // 'value'

remove('key')
```
