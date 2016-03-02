---
layout: docs
title: Event logging
description: Event logging
permalink: /docs/pages/events/
---

## Events

The event mechanism allows you to track important things happening in your code. You can also subscribe to an event so you will receive an email every time the event is happening.

For example:

- A new user has registered
- A new mail has been sent
- A robot has finished its jobs and you want to know what happened

<img src="/images/event-interface.png" alt="Event Interface"/>

## Usage

You will need to [install pmx](/docs/usage/install-pmx/), then the prototype is simple:

```javascript
pmx.emit(EVENT_NAME, DATA)
```

EVENT_NAME must be a string
DATA can be an object or a string

## Example #1: User registration

```javascript
var pmx = require('pmx');

pmx.emit('user:register', {
  user : 'Alex registered',
  email : 'thorustor@gmail.com'
});
```

### Example #2: Content creation

```javascript
var pmx = require('pmx');

pmx.emit('content:page:created', 'A new page has been created');
```

## Subscribe to an event by email

To subscribe to an event just click on "Subscribe by mail to this event":

<center>
<img src="/images/subscribe-event.png" alt="Subscribe Event Interface"/>
</center>
