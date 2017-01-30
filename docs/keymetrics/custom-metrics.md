---
layout: docs
title: Monitor custom metrics
description: Custom metrics
permalink: /docs/pages/custom-metrics/
---

## Collect and monitor custom metrics

By plugging custom metrics onto your code, you will be able to monitor values in realtime and overtime straight from the dashboard.

Make sure you added [pmx](/docs/usage/install-pmx/) into your package.json. You can monitor values as follows:

```javascript
var probe = require('pmx').probe();

var counter = 0;

var metric = probe.metric({
  name    : 'Realtime user',
  value   : function() {
    return counter;
  }
});

setInterval(function() {
  counter++;
}, 100);
```

You will then be able to see this metric into the application widget:

<img src="/images/custom-metric.png" alt="Custom metrics"/>

A premium account will let you monitor these values over the time in the Monitoring page:

<img src="/images/custom-metric-2.png" alt="Custom metrics over time"/>

## Usage

You will need to [install pmx](/docs/usage/install-pmx/) first.

### Custom Metrics

You can then program your very own metrics to track important and relevant information. 4 differents probes are available:

- **Simple metrics**: Values that can be read instantly
    - eg. Monitor variable value
- **Counter**: Things that increment or decrement
    - eg. Downloads being processed, user connected
- **Meter**: Things that are measured as events / interval
    - eg. Request per minute for a http server
- **Histogram**: Keeps a resevoir of statistically relevant values biased towards the last 5 minutes to explore their distribution
    - eg. Monitor the mean of execution of a query into database

#### Simple Metric: Simple value reporting

This allows to expose values that can be read instantly.

```javascript
var probe = pmx.probe();

// Here the value function will be called every second to get the monitored value
var metric = probe.metric({
  name    : 'Realtime user',
  value   : function() {
    return Object.keys(users).length;
  }
});

// Here we are going to call valvar.set() to set the new value
var valvar = probe.metric({
  name    : 'Realtime Value'
});

valvar.set(23);
```

#### Counter: Sequential value change

Things that increment or decrement.

```javascript
var probe = pmx.probe();

// The counter will start at 0
var counter = probe.counter({
  name : 'Current req processed'
});

http.createServer(function(req, res) {
  // Increment the counter, counter will eq 1
  counter.inc();
  req.on('end', function() {
    // Decrement the counter, counter will eq 0
    counter.dec();
  });
});
```

#### Meter: Average calculated values

Things that are measured as events / interval.

```javascript
var probe = pmx.probe();

var meter = probe.meter({
  name      : 'req/min',
  samples   : 1,
  timeframe : 60
});

http.createServer(function(req, res) {
  meter.mark();
  res.end({success:true});
});
```

##### Options

**samples** option is the rate unit. Defaults to **1** sec.
**timeframe** option is the timeframe over which events will be analyzed. Defaults to **60** sec.

#### Histogram

Keeps a resevoir of statistically relevant values biased towards the last 5 minutes to explore their distribution.

```javascript
var probe = pmx.probe();

var histogram = probe.histogram({
  name        : 'latency',
  measurement : 'mean'
});

var latency = 0;

setInterval(function() {
  latency = Math.round(Math.random() * 100);
  histogram.update(latency);
}, 100);
```

### Common Custom Metrics options

- `name` : The probe name as it will be displayed on the **Keymetrics** dashboard
- `agg_type` : This param is optionnal, it can be `sum`, `max`, `min`, `avg` (default) or `none`. It will impact the way the probe data are aggregated within the **Keymetrics** backend. Use `none` if this is irrelevant (eg: constant or string value).
- `alert` : For `Meter` and `Counter` probes. This param is optionnal. Creates an alert object (see below).

## Notification System

A notification system allows you to trigger (email, slack, webhook...) an alert when a monitored value crosses a threshold.
When setting a threshold value, the color on the dashboard changes from green to red depending on the alert level!

You can both programmatically parameter this limit or on the dashboard by clicking on the button "alert".

Example for a `cpu_usage` var:

```javascript
var metric = probe.metric({
  name  : 'CPU usage',
  value : function() {
    return cpu_usage;
  },
  alert : {
    mode  : 'threshold',
    value : 95,
    msg   : 'Detected over 95% CPU usage', // optional
    action: function() { //optional
      console.error('Detected over 95% CPU usage');
    },
    cmp   : function(value, threshold) { //optional
      return (parseFloat(value) > threshold); // default check
    }
  }
});
```

### Options

- `mode` : `threshold`, `threshold-avg`, `smart`.
- `value` : Value that will be used for the exception check.
- `msg` : String used for the exception.
- `action` :  **optional**. Function triggered when the exception is reached.
- `cmp` : **optional**. Function used for exception check taking 2 arguments.
- `interval` : **optional**, `threshold-avg` mode. Sample length for monitored value (180 seconds default).
- `timeout` : **optional**, `threshold-avg` mode. Time after which mean comparison starts (30 000 milliseconds default).

### Builtin custom Metrics

When initializing pmx, you can set to true some options:

- `network` option will display inbound and outbound traffic
- `ports` will display ports your application is using

```javascript
var pmx = require('pmx').init({
  network       : true,  // Network monitoring at the application level
  ports         : true,  // Shows which ports your app is listening on (default: false)
});
```
