---
layout: docs
title: Alert system
description: Email notifications
permalink: /docs/pages/alert-system/
---

## Notifications page

Different alerting preferences are available in the Notifications page.

<img src="/images/alert-page.png" alt="Alert page"/>

### Exceptions alerting

Whenever a new exception or issue is received by Keymetrics, you can be alerted by email by sliding the switch button to **On**.

### Server offline / Connectivity issue alerting

If your server does not send monitoring data to Keymetrics for more than 2 minutes, you can be alerted by email by sliding the switch button to **On**.

## Custom metrics alert

Once you have set <a href="/docs/pages/custom-metrics/" title="custom metrics link">custom metrics</a> to monitor critical metrics on your code, you can then set alert thresholds on these values.

<img src="/images/metrics-alert.png" alt="Alert page"/>

In the main dashboard page, click on the **Alert** button to be able to configure alerts for each custom metric.

Alert mode available are:

- **Threshold**: trigger an email directly when the value is above or below the threshold
- **Threshold average**: trigger an email when the value is above or below the threshold for *X* seconds
- **Smart** (bêta): trigger an email automatically when the value is unusual
