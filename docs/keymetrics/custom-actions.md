---
layout: docs
title: Trigger functions remotely
description: Custom actions
permalink: /docs/pages/custom-actions/
---

## Trigger functions remotely

Interact with your application by exposing triggerable function from your code. For example it is particularly usefull to change variables values on-the-fly, to switch an application in maintenance mode or any actions to change your software behavior without restart. 

Once you exposed a function, a button named with your function will be added to the dashboard:

<img src="/images/custom-actions.png" alt="Custom actions"/>

## Cross Actions

If you have an application hosted on a fleet of servers, you can now perform a custom action through all the different servers where the app is hosted simultaneously. You must filter the dashboard to see the global activity of the app and click the button of the desired custom action. You will then see the progress of the action on each server and will be able to check the logs. 

<img src="http://i.imgur.com/bSsXaHL.jpg" alt="Custom actions"/>

## Usage

You will need to [install pmx](/docs/usage/install-pmx/) then can use either simple actions for instant results or scoped actions to monitor the progress of an action.

### Simple actions

Simple action allows to trigger a function from Keymetrics. The function takes a function as a parameter (reply here) and need to be called once the job is finished.

Example:

```javascript
var pmx = require('pmx');

pmx.action('db:clean', function(reply) {
  clean.db(function() {
    /**
     * reply() must be called at the end of the action
     */
     reply({success : true});
  });
});
```

#### Passing a parameter

To pass a parameter to the remote function, just add the `param` attribute to the callback:

```javascript
var pmx = require('pmx');

pmx.action('db:clean', function(param, reply) {
  console.log(param);
  reply({success : true});
});
```

When triggering the remote function on Keymetrics, a popup will open asking you to enter the parameter you want to pass to the function.

### Scoped actions

Scoped Actions are advanced remote actions that can be also triggered from Keymetrics.

Two arguments are passed to the function, data (optionnal data sent from Keymetrics) and res that allows to emit log data and to end the scoped action.

Example:

```javascript
pmx.scopedAction('long running lsof', function(data, res) {
  var child = spawn('lsof', []);

  child.stdout.on('data', function(chunk) {
    chunk.toString().split('\n').forEach(function(line) {
      res.send(line); // This send log to Keymetrics to be saved (for tracking)
    });
  });

  child.stdout.on('end', function(chunk) {
    res.end('end'); // This end the scoped action
  });

  child.on('error', function(e) {
    res.error(e);  // This report an error to Keymetrics
  });

});
```

## Examples

### Clean database

```javascript
var pmx = require('pmx');

pmx.action('db:clean', function(reply) {
  clean.db();
  reply({success : true});
});
```

### Change variable value on-the-fly

```javascript
var pmx = require('pmx');

pmx.action('debug:on', function(reply) {
  DEBUG="*"
  reply({DEBUG : DEBUG});
});
```

### Query a database and get the result

```javascript
var pmx = require('pmx');

pmx.action('get:active_user', function(reply) {
  User.find({realtime_connected : true }, function(err, users) {
    if (err)
      return reply({success : false, err : err});
    return reply({users : users});
  });
});
```
