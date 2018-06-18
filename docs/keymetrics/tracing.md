---
layout: docs
title: Transaction tracing
description: Transaction tracing
permalink: /docs/pages/tracing/
---

## Transaction tracing

This feature allows you to record and aggregate the database and external calls that your application make on every http request.
You can use transaction traces to troubleshoot performance issues and to get detailed low-level insight into how your app is working.

<img src="http://i.imgur.com/bgPDwbF.png" alt="Transaction Interface"/>

## Usage

You will need to have at least pm2 `2.4.4` to use it, then start/reload the application in tracing mode with the `--trace` option:

```bash
$ pm2 start ecosystem.json --trace
# Or a specific application
$ pm2 reload API --trace
# Or a specific process id or instance
$ pm2 reload 3 --trace
```

### Via [Process File](http://pm2.keymetrics.io/docs/usage/application-declaration/)

Just add the attribute `trace: true` to enable tracing for the target application:

```javascript
module.exports = {
  apps : [{
    name        : "API",
    script      : "./api.js",
    trace       : true
  }]
}
```

### Tracing Status

You can see which application has the Tracing activated by noticing the Clock next to the application name:

<img src="http://i.imgur.com/v8ZiWtg.png" alt="Tracing PM2"/>

### Disable Tracing

To disable the transaction tracing:

```bash
$ pm2 reload <APP> --disable-trace
```

## Advanced Options

To customize some Transaction Tracing feature, first install pmx and require it into your code (at the really beginning of your main file):

**WARNING**: If you want the options to use options with the transaction, you need to disable `--trace` because this option will enable it with default settings. You need to only use the programmatic API. 

```javascript
require('pmx').init({
  transactions : true
});
```
 On the other hand, you can disable it with `--disable-trace` option when restarting your process.
 For the configuration side, you need to define them programmaticly when requiring pmx like above, here are the available options : 
 
 ```javascript
 {
    // Log levels: 0-disabled,1-error,2-warn,3-info,4-debug
    logLevel: 1,

    // Ignore request based on matching string/regex for each field
    // Only one value need to match for the request to be ignored.
    // Example :
    // ignoreFilter: { path: [/v1/, '/'], ip: [/127.0.0.1/, '::1'] } 
    // will ignore request that contains v1 in their path or the index
    // it will ignore request that has been made by localhost
    ignoreFilter: {
      'path': [],
      'method': [],
      'ip': []
    },

    // An upper bound on the number of traces to gather each second. If set to 0,
    // sampling is disabled and all transactions are recorded. Sampling rates greater
    // than 1000 are not supported and will result in at most 1000 samples per
    // second.
    samplingRate: 0
 }
 ```
 

## Dashboard walkthrough

PM2 sends aggregated data with an interval of 1min (the document is really heavy), so you'll get realtime update every minute.


### Latency Graph

Keymetrics stores the data and shows you some graphs representing value over time (currently the median, p95, p99 and median of specific type like redis/mongo etc) for the selected application:

<img src="/images/tracing-graph.png" alt="Transaction Interface"/>

Under the graph you can select which values you want drawn on the graph:
* The [median](https://en.wikipedia.org/wiki/Median) of the application tells you what an ordinary user can expect as a response time.
* P95 and P99 curves lets you explore the evolution of the slowest latencies of your application.
* The database latencies shows you how much time they consume in a standard request.

### Transaction list

<img src="/images/tracing-list.png" alt="Transaction Listing"/>

This represents the list of recorded paths: every time the application answers for the first time to a path, PM2 logs it and tries to aggregate it. If no previous record is found, it is appended to the aggregate. You can list the transactions via:

* Most time consuming: Total time spent in this route for the whole application
* Slowest routes: Which routes take the most time
* Number of calls: how many calls are made to every route

When clicking on a route, you access to the Transaction details

### Transaction details and Variances

<img src="/images/tracing-details.png" alt="Transaction Detail"/>

Some transactions have the same path but respond differently: a forbidden access on a route can return a 403 and be executed differently than usual. We call those **variances**: for each path we log up 5 most used variances that you can examine here.

Let's examine a specific variance: 
* median, slowest and fastest call response time
* Metadata about the call
* List of registered subcalls. If no call to an external [entity](http://docs.keymetrics.io/docs/pages/tracing/#under-the-hood) is made, nothing will appear here. The call display and information depends on the stack logged. For databases, you will for example see the database call made.

You can then click on another **variance** to examine why and how the behaviour was different.

## Under the hood

PMX will wrap below modules if they exist in your application : 
 - `express` version 4
 - `hapi` versions 8 - 13
 - `restify` versions 3 - 4
 - `koa` version v1.x
 - Outbound HTTP requests using `http` core module
 - `mongodb-core` version 1 (used by mongoose)
 - `redis` versions 0.12 - 2
 - `mysql` version ^2.9
 - `pg` version ^6.x

Then record all requests made or received by them then sended to PM2 to be aggregated. 
The impact on performance should be low since there is no heavy logic done in your process except wrap modules and sending data. 

## Things to know
- PM2 will wait 10 minutes after you started your application to send data (to aggregate enough value for them to be relevant).

- You can find the dashboard showing the context of a transaction (your source code), we retrieve it using the V8 API and sometimes (when module using too much async code) we can loose the code that start this request, especially for mongodb.

- There isnt any AVERAGE computed, only [MEDIAN](https://en.wikipedia.org/wiki/Median) value.

- The dataset used to compute percentiles (so median and p95) are all values aggregated over the last hour (see [implementation](https://github.com/keymetrics/pmx/blob/master/lib/utils/probes/Histogram.js))


- When received by PM2, transactions are aggregated depending on their path (so without the query), for example :
  - `/api/users/1` and `/api/users/2` will be aggregated together because PM2 detected the `1` and `2` has identifier
  - `/api/users/search` and `/api/users/1` will not be aggregated together because `search` isnt a identifier

- PM2 detect identifier with multiples regex :
  - UUID v1/v4 with or without dash (`/[0-9a-f]{8}-[0-9a-f]{4}-[14][0-9a-f]{3}-[0-9a-f]{4}-[0-9a-f]{12}|[0-9a-f]{12}[14][0-9a-f]{19}/`)
  - any number (`/\d+/`)
  - suit of number and letter (`/[0-9]+[a-z]+|[a-z]+[0-9]+/`) : this one is used by mongo for document id
  - most SEO optimized webpages (articles, blog posts...): `/((?:[0-9a-zA-Z]+[@\-_.][0-9a-zA-Z]+|[0-9a-zA-Z]+[@\-_.]|[@\-_.][0-9a-zA-Z]+)+)/`

Don't hesitate to open an issue [here](https://github.com/keymetrics/keymetrics-support) if you think we should add another type of identifier or correct one.

## Troubleshooting

It's possible that the agent inside your application is enabled but the tracing isn't succesful. Few things to check :
- If you used `--trace`, disable it with `--disable-trace` then add the pmx snippet at the first line of your application :
```
require('pmx').init({
  transactions : true
});
```
- If still not working, please double-check that you correctly added the snippet at the first line of your application (before any other `require`)
- If still not working, you may have a dependency that breaks our agent, see the [next chapter](http://docs.keymetrics.io/docs/pages/tracing/#incompatibilities).
- If you don't have any dependency listed as incompatible, please contact our support either on zendesk, contact@keymetrics.io or https://github.com/keymetrics/keymetrics-support

## Incompatibilities

This feature has some known problems with other modules :
* `node-newrelic`: This module do the same thing as we do, so you might encounter problems with it.
* `request-promise`: This module clears the node cache and requires a new clean version of the `http` module. To solve this require `http` again after requiring `request-promise` to get the correctly wrapped `http` module.
