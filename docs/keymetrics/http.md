---
layout: docs
title: HTTP analysis
description: HTTP analysis page
permalink: /docs/pages/http/
---

## HTTP analysis

The HTTP analysis page allow you to track the average HTTP latency of your application and slow routes. A slow route is a route that takes more than 200ms to finish or that has a response greater or equal to 500.

<img src="/images/latency.png" alt="HTTP latency Interface"/>

## Usage

You will need to [install pmx](/docs/usage/install-pmx/) and enable the http flag when requiring pmx and note that you need to require it **before** requiring any http related modules.

```javascript
require('pmx').init({
  http : true
});

var http = require('http');
[...]
```

## Possible issues with modules:

To retrieve the http latency, pmx [wraps](https://github.com/keymetrics/pmx/blob/master/lib/wrapper/simple_http.js) the `http` module. If you require any module modifying the `http` module, the wrapper could be removed.
* `request-promise`: This module clears the node cache and requires a new clean version of the `http` module. To solve this require `http` again after requiring `request-promise` to get the correctly wrapped `http` module.
