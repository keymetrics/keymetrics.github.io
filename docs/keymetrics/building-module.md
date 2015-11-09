---
layout: docs
title: Module System
description: PM2 module
permalink: /docs/usage/building-module/
---

<a href="/images/racks/mongodb-rack.png" title="Keymetrics interface explanation"><img src="/images/racks/mongodb-rack.png"/></a>
<a href="/images/racks/redis-rack.png" title="Keymetrics interface explanation"><img src="/images/racks/redis-rack.png"/></a>

## What is a module?

A module is a simple Node.js application that will connect to a technology (database, utility) to both retrieve metrics (Memory, Entries...) and expose actions (restart, flush...).

A module is published on NPM.

## Getting started: Installing a module

1- Install PM2

```bash
$ npm install pm2 -g
```

2- [Go to Keymetrics](https://app.keymetrics.io/#/) and create a new account

3- Create a bucket and then follow the instruction to link your local pm2 to Keymetrics

4- Install a simple module

```bash
# Install a module that will monitor your current machine
$ pm2 install pm2-server-monit
```

Now you can see in the Keymetrics interface the module interface you just installed.

The source code of this pm2-server-monit module is [here](https://github.com/pm2-hive/pm2-server-monit)

Here are some other modules:

- [pm2-redis](https://github.com/pm2-hive/pm2-redis.git)
- [pm2-mongodb](https://github.com/pm2-hive/pm2-mongodb)
- [pm2-elasticsearch](https://github.com/pm2-hive/pm2-elasticsearch)

## Getting started: Creating your own module

Creating your own module is straightforward.

To generate a sample module:

```bash
$ pm2 module:generate <module-name>
```

Now let's run this module with PM2:

```
$ cd <module-name>
$ pm2 install .
```

You can now edit the source, once you change something, pm2 will automatically restart the module.

To remove the module:

```
$ pm2 uninstall <module-name>
```

## Advanced

### Package.json: Declare options, widget aspect and module behavior

A package.json must be present with some extra fields like `config` for configuration variables and `apps` to declare the [behavior of this module](https://github.com/Unitech/PM2/blob/master/ADVANCED_README.md#options-1):

```javascript
{
  "name": "pm2-logrotate",  // Used as the module name
  "version": "1.0.0",       // Used as the module version
  "description": "my desc", // Used as the module comment
  "dependencies": {
    "pm2": "latest",
    "pmx": "latest"         // Common dependencies to communiate with Keymetrics
  },
  "config": {              // Default configuration value
                           // These values can be modified via Keymetrics or PM2 configuration system

     "days_interval" : 7,  // -> this value is returned once you call pmx.initModule()
     "max_size" : 5242880
  },
  "apps" : [{              // Application configuration
    "merge_logs"         : true,
    "max_memory_restart" : "200M",
    "script"             : "index.js"
  }],
  "author": "Keymetrics Inc.", // Optional
  "license": "AGPL-3.0"        // Optional
}
```

### Module entry point

This is the index.js file (declared in the package.json in the apps section):
The pmx.initModule takes a range of options to configure the module display in Keymetrics or to override the PID monitored by PM2:

```javascript
var pmx     = require('pmx');

// Initialize the module
var conf    = pmx.initModule({

    // Override PID to be monitored (for CPU and Memory blocks)
    pid              : pmx.resolvePidPaths(['/var/run/redis.pid', '/var/run/redis/redis-server.pid']),

    widget : {

      // Module display type. Currently only 'generic' is available
      type : 'generic',

      // Logo to be displayed on the top left block
      // Must be https
      logo : 'https://image.url',

      // 0 = main element
      // 1 = secondary
      // 2 = main border
      // 3 = secondary border
      // 4 = text color (not implemented yet)
      theme : ['#9F1414', '#591313', 'white', 'white'],

      // Toggle horizontal blocks above main widget
      el : {
        probes : false,
        actions: false
      },

      block : {
        // Display remote action block
        actions : true,

        // Display CPU / Memory
        cpu     : true,
        mem     : true,

        // Issues count display
        issues  : true,

        // Display meta block
        meta    : true,

        // Display metadata about the probe (restart nb, interpreter...)
        meta_block : true,

        // Name of custom metrics to be displayed as a "major metrics"
        main_probes : ['Processes']
      },
    },
}, function(err, conf) {
  // The module has been initiated
  // Now you can require all files
  console.log(conf);
});
```

### Module configuration

In the package.json you can declare default options accessible in the Module under the attribute `config`. These values can be overriden by PM2 or Keymetrics.

#### Default values

Add default configuration values in package.json under the "config" attribute:

```json
{
 [...]
 "config": {             // Default configuration value
    "days_interval" : 7,  // -> returned from var ret = pmx.initModule()
    "max_size" : 5242880  // -> e.g. ret.max_size
 }
 [...]
}
```

Then these values are accessible via the data returned by pmx.initModule().

Example:

```javascript
var conf = pmx.initModule({[...]});

// Now we can read these values
console.log(conf.days_interval);
```

### Override configuration values

#### With PM2

With PM2 this is straightforward. Just call this command:

```bash
$ pm2 set module_name:option_name <new_value>
```

Example:

```bash
$ pm2 set server-monitoring:days_interval 2
```

- **NOTE** These variables are written in `~/.pm2/module_conf.json`, so if you prefer, you can directly edit these variables in this file.
- **NOTE2** You can display configuration variable via `pm2 conf [module-name]`
- **NOTE3** When you set a new value the target module is restarted
- **NOTE4** You have to typecast yourself values, it is always strings!

#### With Keymetrics

In the main Keymetrics Dashboard, the module will have a button called "Configure". Once you click on it you will be able to access / modify all configurations variable exposed on the package.json!
