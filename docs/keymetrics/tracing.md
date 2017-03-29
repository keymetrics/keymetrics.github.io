---
layout: docs
title: Transaction tracing
description: Transaction tracing
permalink: /docs/pages/tracing/
---

## Transaction tracing

This feature allows you to record and aggregate the database and external calls that your application make on every http request.
You can use transaction traces to troubleshoot performance issues and to get detailed low-level insight into how your app is working.

<img src="/images/tracing.png" alt="Transaction Interface"/>

## Usage

You will need to have at least pm2 `2.4.x` to use it, then you have two ways to enable it :
 - pmx using a options :

```javascript
require('pmx').init({
  transactions : true
});
```
 - or using pm2 with the `--trace` options, ex :`pm2 start ecosystem.json --trace`
 
 On the other hand, you can disable it with `--disable-trace` option when restarting your process.

## Explaination

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

When received by PM2, transactions are aggregated depending on their path (so without the query), for example :
- `/api/users/1` and `/api/users/2` will be aggregated together because PM2 detected the `1` and `2` has identifier
- `/api/users/search` and `/api/users/1` will not be aggregated together because `search` isnt a identifier

PM2 detect identifier with multiples regex :
- UUID v1/v4 with or without dash (`/[0-9a-f]{8}-[0-9a-f]{4}-[14][0-9a-f]{3}-[0-9a-f]{4}-[0-9a-f]{12}|[0-9a-f]{12}[14][0-9a-f]{19}/`)
- any number (`/\d+/`)
- suit of number and letter (`/[0-9]+[a-z]+|[a-z]+[0-9]+/`) : this one is used by mongo for document id 

Don't hesitate to open an issue [here](https://github.com/keymetrics/keymetrics-support) if you think we should add another type of identifier.

## Things to know
- PM2 will wait 10 minutes after you started your application to send data (to aggregate enough value for them to be relevant).
- You can find the dashboard showing the context of a transaction (your source code), we retrieve it using the V8 API and sometimes (when module using too much async code) we can loose the code that start this request, especially for mongodb.
- There isnt any AVERAGE computed, only MEDIAN value.
- The dataset used to compute percentiles (so median and p95) are all values aggregated over one hour (see [implementation](https://github.com/keymetrics/pmx/blob/master/lib/utils/probes/Histogram.js))

## Dashboard explanation

PM2 sends aggregated data with an interval of 1min (the document is really heavy), so you'll get realtime update every minute only.

Keymetrics will store it and show you some graphic representing value over time (currently the median, p95, p99 and median of specific type like redis/mongo etc) :

<img src="/images/tracing-graph.png" alt="Transaction Interface"/>


## Incompatibilities

This feature has some known problems with other modules :
* `node-newrelic`: This module do the same thing as we do, so you might encounter problems with it.
* `request-promise`: This module clears the node cache and requires a new clean version of the `http` module. To solve this require `http` again after requiring `request-promise` to get the correctly wrapped `http` module.
