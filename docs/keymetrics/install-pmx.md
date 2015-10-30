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
var pmx = require('pmx').init();
```

To know more about PMX: [click here](/docs/usage/install-pmx)
