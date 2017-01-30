---
layout: docs
title: Integrating Keymetrics with 3rd party services
description: Keymetrics integrations
permalink: /docs/pages/integrations/
---

## Slack integration

The Slack integration allows you to receive exceptions and event notifications straight into a selected Slack channel.

First you need to get the Slack URL and to setup an incoming Webhook. More details on how to set this up can be found here: [https://my.slack.com/services/new/incoming-webhook/](https://my.slack.com/services/new/incoming-webhook/) or [https://api.slack.com/incoming-webhooks](https://api.slack.com/incoming-webhooks).

Then go to the Notifications page of your bucket:

<img src="/images/slack.png" alt="slack integration"/>

Insert the webhook into the Slack integration textfield and click on the checkbox active. Then click on update.

You will receive a notification into your configured slack channel confirming that it has been configured.

## Custom integration

You can also set a custom webhook that will simply POST data to any URL when you receive a notifications.
To do so you will need to setup a http server that will listen for calls from Keymetrics, then you will just have to insert the URL into Keymetrics interface: 

<img src="/images/webhooks.png" alt="webhook integration"/>

I just setup a https://requestb.in URL (don't hesitate to use our own to test the different functionalities, it's free!). This will log all http requests that have been made to this URL. Example of an error that has been thrown:

```
 {
   "event":"event:new_exception",
   "data":{
      "process":{
         "pm_id":9,
         "name":"pm2-elasticsearch",
         "rev":"ac77098c5e1b10d74360b113da6e717fab8fe427",
         "server":"pm2-module-testing"
      },
      "data":{
         "message":"Bad argument",
         "stack":"TypeError: Bad argument\n    at TypeError (native)\n    at ChildProcess.spawn (internal/child_process.js:274:26)\n    at exports.spawn (child_process.js:362:9)\n    at Object.exports.execFile (child_process.js:151:15)\n    at exports.exec (child_process.js:111:18)\n    at /home/node/pm2-elasticsearch/lib/actions.js:25:5\n    at process.<anonymous> (/home/node/pm2-elasticsearch/node_modules/pmx/lib/actions.js:64:14)\n    at emitTwo (events.js:92:20)\n    at process.emit (events.js:172:7)\n    at handleMessage (internal/child_process.js:695:10)"
      },
      "at":1472651928925,
      "created_at":1472651928925,
      "updated_at":1472651928925,
      "count":1,
      "identifier":"ad6f8650dabfec83f183633b0bba7d97",
      "infected_servers":[
         "pm2-module-testing"
      ],
      "timestamps":[
         1472651928925
      ],
      "commits":[
         "ac77098c5e1b10d74360b113da6e717fab8fe427"
      ],
      "bucket_url":"https://app.keymetrics.io/#/bucket/YOUR_BUCKET_ID/exceptions"
   }
}
```
 
This will let you easily setup a little express server that can receive webhooks, automatically send SMS or use any integration you want.
