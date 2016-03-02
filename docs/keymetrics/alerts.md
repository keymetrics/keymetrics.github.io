---
layout: docs
title: Alert system
description: Email notifications
permalink: /docs/pages/alert-system/
---

## Alert page

Some alerting systems are available in the Alert page.

<img src="/images/alert-page.png" alt="Alert page"/>

### Exceptions alerting

Whenever a new exception or issue is received by Keymetrics, you can be alerted by email by sliding the switch button to **On**.

### Server offline / Connectivity issue alerting

If your server does not send monitoring data to Keymetrics for more than 2 minutes, you can be alerted by email by sliding the switch button to **On**.

## Custom metrics alert

Once you have set <a href="/docs/pages/custom-metrics/" title="custom metrics link">custom metrics</a> to monitor critical metrics on your code, you can then set alerts threshold on these values.

<img src="/images/metrics-alert.png" alt="Alert page"/>

Just to go to the main dashboard page, click on the **Alert** button and you will be able to configure alerts on each custom metrics.

Alert mode available are:

- **Threshold**: trigger an email directly when value is above or below threshold
- **Threshold average**: trigger an email when value is above or below threshold for *X* seconds
- **Smart** (bêta): trigger an email automatically when value is weird
