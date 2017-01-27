---
layout: docs
title: Keymetrics security concerns
description: Keymetrics security concerns
permalink: /docs/usage/security/
---

At Keymetrics, security is our main concern.

## Between PM2 and Keymetrics

When you create a *Bucket*, you may have noticed that we provide two unique keys. One private and one public.

These keys are used to identify your PM2 toward Keymetrics and also to cipher all data transfered (push or reverse data) between PM2 and Keymetrics. The data is ciphered in AES256.

## Data stored in Keymetrics

> What information about our services / servers / applications is collected and sent to your systems?

**Server**: Hostname, IP, Load average, Memory, CPU infos, distribution type
**Applications**: CPU, Memory, PM2 metadata (exec mode, id, restart nb, unstable restart nb, probes, actions name), Git metadata
**Services** (via module system): Custom metrics, Custom actions name (no storage of results)

> How do you protect that data?

Data is ciphered while transfered into network (HTTPS and AES256). Data stored in database is normalized but each bucket has his own database (with database name ciphered).

> Do you adhere to any industry security standards, such as ISO or PCI?

We use [Stripe](https://stripe.com/) as our payment system, we never store any informations about credit cards used on Keymetrics. 

### Remote actions lock'in

As you may know, Keymetrics provides a very simple way to interact with your servers, from triggering in-code functions as well as triggering PM2 actions.

Concerning PM2 actions, a strict list of allowed remote functions are hard-coded into PM2 itself, forbidding any critical actions to be called. This strict list of allowed remote functions are splitted in two packs.

The first one, non-offensive functions like `pm2 restart | reload | gracefulReload` that cannot change deeply the state of your software. 
Then the second one, sensitive functions like `pm2 stop | updateDeep | pm2 set [...]`. These sensitive functions can be called from Keymetrics but asks to the one who wants to trigger the function, a password, previously set at PM2 level.

This password, that you configured at the PM2 level via the command `pm2 set pm2:passwd <password>`, is not shared with Keymetrics. Only you know it, and this password is only transferred by Keymetrics, making the system very secure.

## Between Keymetrics and your browser

We use the HTTPS protocol to secure all data transferred from Keymetrics to your browser.

## User permissions

You can add as many users as you want to your buckets. There are four different roles: User, Developer, Admin and Owner.
Only the Owner can change a user's role.

1. User
  * Has read-only permission on the bucket
  * This is the default role when you add a user

2. Developer
  * This should be assigned to your tech team members as they can do the following:
  * Trigger remote actions (e.g: restart, custom actions)
  * Delete exceptions
  * Delete HTTP monitoring data
  * Subscribe to new events
  * Update alert settings

3. Admin
  * Has the same rights as developer
  * Can add a user to the bucket
  * Can delete a server

4. Owner
  * Cannot be changed
  * Update bucket metadata (name, description...)
  * Can remove users from the bucket
  * Delete the bucket
  * Change user permissions
  * Upgrade the bucket to a premium plan
