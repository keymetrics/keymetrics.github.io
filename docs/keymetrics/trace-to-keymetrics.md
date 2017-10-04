---
layout: docs
title: Migrating From RisingStack Trace To Keymetrics
description: Migrating From RisingStack Trace To Keymetrics
permalink: /docs/usage/from-trace-to-keymetrics/
---

![https://i.imgur.com/17Wa4fu.png](https://i.imgur.com/17Wa4fu.png)

## From Trace to Keymetrics

As you might have seen, Trace and Keymetrics had partnered up. Trace was the tool built by the engineers of RisingStack, a monitoring SaaS which use an agent to provide insights into Node.js services. 

The partnership of RisingStack & Keymetrics includes merging Trace and its userbase into Keymetrics, and providing technical help to build a superior Node.js monitoring tool.

This piece of documentation is for former Trace users and will help them understand how do properly migrate from Trace to Keymetrics

## What is required to use Keymetrics?

It requires a very short amount of time to use Keymetrics and PM2 in production.
Basically you will just need to use pm2 as a process manager / node.js runtime and use one to command to link it to Keymetrics.

## 1/ Start your application with PM2

<br/>
<center>
 <img src="https://raw.githubusercontent.com/Unitech/pm2/master/pres/pm2-v3.png" width="500"/>
</center>
<br/><br/>

Keymetrics is built on top of PM2, a well known, [Open Source](https://github.com/Unitech/pm2) process manager that improves performance and reliability of your Node.js applications.

Getting started with PM2 is as easy as:

```bash
# Install PM2 globally
$ npm install pm2 -g
# Start your Node.js application
$ pm2 start app.js
```

Your app is now daemonized, monitored and kept alive forever. 

Some of the key features are:
- [Automatic clustering and Zero Downtime Reload](http://pm2.keymetrics.io/docs/usage/cluster-mode/)
- [Process Configuration file](http://pm2.keymetrics.io/docs/usage/application-declaration/)
- [Docker integration](http://pm2.keymetrics.io/docs/usage/docker-pm2-nodejs/)
- Auto Source Map resolving, Log management and much ore

Discover all PM2 features via our [official documentation](http://pm2.keymetrics.io/).

## 2/ Link PM2 to Keymetrics

Now let's get this Node.js application monitored by Keymetrics. Make sure [you've created an account](https://app.keymetrics.io/#/) and just copy the `pm2 link <secret> <public>` displayed in the popup and paste it into your terminal. 

![https://i.imgur.com/3bg5Wrg.png](https://i.imgur.com/3bg5Wrg.png)

## 3/ Monitor

Instantly, you will see the Keymetrics Dashboard revealed and you can monitor some of the Key Metrics of your application.

![https://keymetrics.io/assets/images/homepage-tour/screen1.jpeg](https://keymetrics.io/assets/images/homepage-tour/screen1.jpeg)

## Feature comparison between Trace and Keymetrics

### Transaction Tracing

As Trace, Keymetrics provides a Transaction Tracing system that allows you to identify which HTTP calls are slow and why they are slow.

![https://keymetrics.io/assets/images/homepage-tour/screen6.jpeg](https://keymetrics.io/assets/images/homepage-tour/screen6.jpeg)

[Learn more about Transaction Tracing](http://docs.keymetrics.io/docs/pages/tracing/)

### Memory & CPU profiling

The Trace tracing feature is similar to what can be found under the "Profiling" tab of Keymetrics App:

![https://keymetrics.io/assets/images/tour/screen4@2x.jpg?v=d02a2e7337](https://keymetrics.io/assets/images/tour/screen4@2x.jpg?v=d02a2e7337)

For CPU profiling, it can be started directly from the Keymetrics interface and stopped any time you want. You will then get a visualisation of the stack and you will still have the ability to download the CPU profiling file.

About Memory Profiling, you have the exact same ability to heapdump memory any time you want.

[Learn more about Memory & CPU Profiling](http://docs.keymetrics.io/docs/pages/profiling/)

### Errors & Exceptions
 
You can find and filter errors in your code and see their stack traces and occurrence data. We also display line code number and logs before exceptions

![https://keymetrics.io/assets/images/homepage-tour/screen5.jpeg](https://keymetrics.io/assets/images/homepage-tour/screen5.jpeg)

[Learn more about Exceptions Tracking](http://docs.keymetrics.io/docs/pages/issues/)

### Metrics
 
With Keymetrics you can also monitor criticals metrics and also have data retention of 14 days. This will be extended to 60 days in the next month.

![https://keymetrics.io/assets/images/homepage-tour/screen3.jpeg](https://keymetrics.io/assets/images/homepage-tour/screen3.jpeg)

[Learn more about Metrics Monitoring](http://docs.keymetrics.io/docs/pages/monitoring/)

### Features in development
 
The Trace Uptime Webchecks and Topology overview will be developped in the following month in partnership with RisingStack team. 

### Some Nifty Keymetrics Features

- [Custom metrics](http://docs.keymetrics.io/docs/pages/custom-metrics/) allows you to track variable value over time
- [Remote Functions Trigger](http://docs.keymetrics.io/docs/pages/custom-actions/) allows you to trigger code function straight fronm the dashboard

## More about Keymetrics

[Fly over here](http://docs.keymetrics.io/)

## What is next

Webinars
 
