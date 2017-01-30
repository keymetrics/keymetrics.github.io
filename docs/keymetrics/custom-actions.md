---
layout: docs
title: Trigger functions remotely
description: Custom actions
permalink: /docs/pages/custom-actions/
---

## Trigger functions remotely

Interact with your applications by exposing triggerable functions from your code. For example it is particularly useful to change variables values on-the-fly, to switch an application in maintenance mode or to trigger any actions to change your software behavior without having to restart. 

Once you exposed a function, a button named with your function will be added to the dashboard:

<img src="/images/custom-actions.png" alt="Custom actions"/>

## Cross Actions

If you have an application hosted on a fleet of servers, you can perform a custom action through all the different servers where the app is hosted simultaneously. You must filter the dashboard to see the global activity of the app and click the button of the desired custom action. You will then see the progress of the action on each server and you will also be able to check the logs. 

<img src="http://i.imgur.com/bSsXaHL.jpg" alt="Custom actions"/>

## Usage

You will need to [install pmx](/docs/usage/install-pmx/) to then be able to use either simple actions for instant results or scoped actions to monitor the progress of a specific action.

### Simple actions

Simple actions allow you to trigger a function from Keymetrics. The function takes a function as a parameter (reply here) and need to be called once the job is finished.

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

Triggering a remote function on Keymetrics will open a popup that will ask you to enter the parameter you want to pass to the function.

### Scoped actions

Scoped Actions are advanced remote actions that can also be triggered from Keymetrics.

Two arguments are passed to the function, data (optionnal data sent from Keymetrics) and res that will emit the log data and end the scoped action.

Example:

```javascript
pmx.scopedAction('long running lsof', function(data, res) {
  var child = spawn('lsof', []);

  child.stdout.on('data', function(chunk) {
    chunk.toString().split('\n').forEach(function(line) {
      res.send(line); // This sends log to Keymetrics to be saved (for tracking)
    });
  });

  child.stdout.on('end', function(chunk) {
    res.end('end'); // This ends the scoped action
  });

  child.on('error', function(e) {
    res.error(e);  // This reports an error to Keymetrics
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
