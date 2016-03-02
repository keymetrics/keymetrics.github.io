---
layout: docs
title: Monitoring page
description: Monitoring page description
permalink: /docs/pages/monitoring/
---

## Monitoring page

This page allow you to track vital application signs (CPU/Memory) and other custom metrics over the time.

<img title="monitoring interface" src="/images/monitoring-interface.png"/>

### Metrics monitored

- Memory usage
- CPU usage
- Process events (restart, code update, reload, stop, start)
- Custom metrics value over the time

### Trend line

A trend line is now generated when you use the global filters (it only appears when you select one specific server).
This trend line is a linear regression over the values displayed. It helps you understand how a metric is evolving over the time.

<img title="monitoring interface trend line" src="/images/monitoring-trend-line.png"/>

### Graphics label

<center>
<img title="monitoring interface" src="/images/bar-label.png"/>
</center>

### Custom metrics histogram

Once you configure [some custom metrics](http://docs.keymetrics.io/docs/usage/pmx-keymetrics-library/#expose-metrics-measure-anything) you can track their values over the time.
