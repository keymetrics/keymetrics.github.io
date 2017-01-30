---
layout: docs
title: Using Keymetrics on PaaS
description: Keymetrics Heroku, Azure, Google App Engine
permalink: /docs/usage/paas-heroku-azure/
---

Thanks to the PM2 API, using the Keymetrics/PM2 combo is straightforward and configurable:
Now you can run micro-service style applications very easily. Don't forget to have a look on how to declare [multiple applications in a JSON style with PM2](http://pm2.keymetrics.io/docs/usage/application-declaration/#declaration-via-js-json-or-json5-file).

Change the following snippet with your private and public key and you are ready to go:

```javascript
// Make sure you added pm2 as a dependency in your package.json
// Then in your Procfile, do a simple `node bootstrap.js`

var pm2 = require('pm2');

var MACHINE_NAME = 'hk1';
var PRIVATE_KEY  = 'z1ormi9vomgq66';
var PUBLIC_KEY   = 'oa0m7nuhdfibi16';

var instances = process.env.WEB_CONCURRENCY || -1;
var maxMemory = process.env.WEB_MEMORY || 512;

pm2.connect(function() {
  pm2.start({
    script    : 'app.js',
    name      : 'production-app',        // These attributes are optional
    exec_mode : 'cluster',               // http://pm2.keymetrics.io/docs/usage/application-declaration/#options
    instances : instances,
    max_memory_restart : maxMemory + 'M',   // Auto restart if process taking more than XXmo
    env: {                            // If needed declare some environment variables
       "NODE_ENV": "production",
       "AWESOME_SERVICE_API_TOKEN": "xxx"
    },
    post_update: ["npm install"]       // Commands to execute once we do a pull from Keymetrics
  }, function() {
    pm2.interact(PRIVATE_KEY, PUBLIC_KEY, MACHINE_NAME, function() {
     // Display logs in standard output
     pm2.launchBus(function(err, bus) {
       console.log('[PM2] Log streaming started');

       bus.on('log:out', function(packet) {
         console.log('[App:%s] %s', packet.process.name, packet.data);
       });

       bus.on('log:err', function(packet) {
         console.error('[App:%s][Err] %s', packet.process.name, packet.data);
       });
      });
    });
  });
});
```
