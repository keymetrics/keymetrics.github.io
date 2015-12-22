---
layout: docs
title: Versioning management
description: Custom actions
permalink: /docs/pages/versioning-management/
---


```
{
  "name": "application-name",
  "version": "0.0.1",
  "private": true,
  "scripts": {
  "start": "node app"
  },
  "dependencies": {
  "pmx" : "latest"
  },
  "apps" : [{
  "script" : "app.js",
  "name" : "keymetrics tuto",
        "post_update" : ["npm install"]
  }]
  }
```
