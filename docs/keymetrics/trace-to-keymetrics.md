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

## What is required to use Keymetrics ?

## Supercharge Node.js with PM2

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

## Monitor and Diagnose with Keymetrics

Now let's get this Node.js application monitored by Keymetrics. Make sure [you've created an account](https://app.keymetrics.io/#/) and just copy the `pm2 link <secret> <public>` displayed in the popup and paste it into your terminal. 

![https://i.imgur.com/3bg5Wrg.png](https://i.imgur.com/3bg5Wrg.png)

Instantly, you will see the Keymetrics Dashboard revealed and you can monitor some of the Key Metrics of your application.

## Feature comparison between Trace and Keymetrics

### Memory & CPU profiling

Trace allows you to investigate memory & cpu dumps to find leaks & bottlenecks in production easily
 
### Distributed Tracing

Finding issues in a distributed system or microservices can be challenging. We collect, group and visualize service calls with issues, so you don't have to waste your time with spending hours to dig in your logs.
 
### Metrics
 
Our metrics page allows you to keep track of what is currently happening in your application. It makes it easy to spot potential errors during runtime.
 
### Topology
 
On the service map you can check how your services communicate with each other, what are the average response times and how many requests each service makes. See the communication with databases and 3rd party APIs, and get live hints when your services are getting slower.
 
Alerting: 
You simply don't have enough time watching the metrics page and trying to figure out problems with your infrastructure. This feature allows you to set an alert for your future self, or your team when something bad occurs based on the collected metrics.
 
Errors: 
 
You can find and filter errors in your code and see their stack traces and occurrence data.


## Keymetrics specific features

- Server monitoring
- Real time Dashboard 
- Custom Metrics
- Trigger Function from Dashboard
- Plugins system (DB & Server monitoring)
- User ACL management

## What is next

Webinars
 
