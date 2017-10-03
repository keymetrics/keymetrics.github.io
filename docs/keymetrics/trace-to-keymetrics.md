---
layout: docs
title: Migrating From RisingStack Trace To Keymetrics
description: Migrating From RisingStack Trace To Keymetrics
permalink: /docs/usage/from-trace-to-keymetrics/
---

# Intro

As you might have seen, Trace and Keymetrics had partnered up. Trace was the tool built by the engineers of RisingStack, a monitoring tool which use to provide actionable insights into Node.js services & microservices architectures. 
The partnership of RisingStack & Keymetrics includes merging Trace and its userbase into Keymetrics, and providing technical help to build a superior Node.js monitoring tool.

This piece of documentation is for former Trace users and will help them understand how do properly migrate from Trace to Keymetrics

# What is required to use Keymetrics ?

Keymetrics is built on top of PM2, a well known process manager that improves performance and reliability of your Node.js applications.
Getting started with PM2 is as easy as:

```bash
# Install PM2 globally
$ npm install pm2 -g
# Start your Node.js application
$ pm2 start app.js
```

Your app is now daemonized, monitored and kept alive forever. Discover all PM2 features via our official documentation and don't forget to checkout unique PM2 features like cluster mode, process configuration files or the official Docker integration!

## Linking PM2 to Keymetrics

Now let's get this Node.js application monitored by Keymetrics. Make sure you've created an account and just copy the `pm2 link <secret> <public>` displayed in the popup and paste it into your terminal. Instantly, you will see the Keymetrics Dashboard revealed and you can monitor some of the Key Metrics of your application.




# How to use Trace features in KM


Memory & CPU profiling

Trace allows you to investigate memory & cpu dumps to find leaks & bottlenecks in production easily
 
Distributed Tracing: 

Finding issues in a distributed system or microservices can be challenging. We collect, group and visualize service calls with issues, so you don't have to waste your time with spending hours to dig in your logs.
 
Metrics: 
 
Our metrics page allows you to keep track of what is currently happening in your application. It makes it easy to spot potential errors during runtime.
 
Topology
 
On the service map you can check how your services communicate with each other, what are the average response times and how many requests each service makes. See the communication with databases and 3rd party APIs, and get live hints when your services are getting slower.
 
Alerting: 
You simply don't have enough time watching the metrics page and trying to figure out problems with your infrastructure. This feature allows you to set an alert for your future self, or your team when something bad occurs based on the collected metrics.
 
Errors: 
 
You can find and filter errors in your code and see their stack traces and occurrence data.


# Keymetrics specific features

Real time Dashboard 
Custom Metrics
Trigger Function from Dashboard
Plugins system (DB & Server monitoring)
User ACL management


# what is next

Webinars
 
