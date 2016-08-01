---
layout: docs
title: Integrating Keymetrics with 3rd party services
description: Keymetrics integrations
permalink: /docs/pages/integrations/
---

## Slack integration

The Slack integration allow you to receive exceptions and event notifications straight into a Slack channel.

First you need to get the Slack URL, you need to setup an Incoming Webhook. More details on how to set this up can be found here: [https://my.slack.com/services/new/incoming-webhook/](https://my.slack.com/services/new/incoming-webhook/) or [https://api.slack.com/incoming-webhooks](https://api.slack.com/incoming-webhooks)

Then go to the alert page of your bucket:

<img src="/images/slack.png" alt="slack integration"/>

Put the webhook into the Slack integration textfield and click on the checkbox active. Then click on update.

You will receive a notification into your configured slack chanel telling you that it has been configured.
