---
layout: docs
title: Install PMX
description: Installing PMX for advanced interactions
permalink: /docs/usage/install-pmx/
---

# PMX

In order to use Keymetrics in a more advanced way, you will need to install PMX. You just need to add it as a dependency to your project:

```bash
$ npm install pmx --save
```

Then to get the PMX library in your code, just require it **at the very beginning of your code**:

```javascript
var pmx = require('pmx').init({
  http          : true, // HTTP routes logging (default: true)
  ignore_routes : [/socket\.io/, /notFound/], // Ignore http routes with this pattern (Default: [])
  errors        : true, // Exceptions logging (default: true)
  custom_probes : true, // Auto expose JS Loop Latency and HTTP req/s as custom metrics
  network       : true, // Network monitoring at the application level
  ports         : true  // Shows which ports your app is listening on (default: false)
});
```
