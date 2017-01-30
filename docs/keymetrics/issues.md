---
layout: docs
title: Keymetrics issues tutorial
description: Introduction to Keymetrics issues
permalink: /docs/pages/issues/
---

Once your bucket is correctly linked to your server, you should have a similar view in your dashboard:

<img src="/images/tutorial/issues/first.png"/>

The errors count is 0, this means your application didn't have any exception yet ! Lets see how that works.

## Exceptions monitoring

As many of Keymetrics features, the issues reporting requires PMX [to be installed on your project](/docs/usage/install-pmx/).

There are two ways to get errors reported in Keymetrics, catching uncaught exceptions and report custom ones. Let's review them.

### Catch uncaught exceptions

Let's consider the following snippet somewhere in your code:

```javascript
throw new Error("error message")
```

When `pmx` is correctly setup, it catches it and here is what happens:

It sends a notification to keymetrics, the error is then saved and increases the error count displayed on the dashboard:

<img src="/images/tutorial/issues/second.png"/>

A red counter also appears on the left-side menu when expanded:

<img src="/images/tutorial/issues/third.png"/>

The error is described in the `issues` tab:

<img src="/images/tutorial/issues/fourth.png"/>

If the email alerts are `on` for the exceptions alert (under the "Alerts" tab), an email is sent to you when the exception occurs for the first time. Otherwise it just increments the counters.

### Custom issues

You can also emit your own issues using `pmx.notify()`. Given the following snippets:

```javascript
pmx.notify(new Error('This is an error'));
pmx.notify({ success : false });
```

Here is what you will see in your "Issues" tab when both happen:

<img src="/images/tutorial/issues/fifth.png"/>

### Bonus: Express middleware

`pmx` also comes with its own express middleware. All you need to do is to add the following line:

```javascript
app.use(pmx.expressErrorHandler());
```

after the routes declaration.

## Source map support

To enable the source map support you need to upgrade PM2 (http://pm2.keymetrics.io/docs/usage/source-map-support/).
