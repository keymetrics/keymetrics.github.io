---
layout: docs
title: Keymetrics security concerns
description: Keymetrics security concerns
permalink: /docs/usage/security/
---

Security is a main concern at Keymetrics.

## Between PM2 and Keymetrics

When you create a *Bucket*, you may have noticed that we provide two unique keys. One private and one public.

These keys are used to identify your PM2 toward Keymetrics and also to cipher all data transfered (push or reverse data) between PM2 and Keymetrics. The data is ciphered in AES256.

### Remote actions lock'in

As you may know, Keymetrics provide a very simple way to interact with your servers, from triggering in-code functions as well as triggering PM2 actions.

Concerning PM2 actions, a strict list of allowed remote functions are hard-coded into PM2 itself, forbidding any critical actions to be called. This strict list of allowed remote functions are splitted in two packs.

The first one, non-offensive functions like `pm2 restart | reload | gracefulReload` that cannot change deeply the state of your software. Then the second one, sensitive functions like `pm2 stop | updateDeep | pm2 set [...]`. These sensitive functions can be called from Keymetrics but asks to the one who wants to trigger the function, a password, previously set at PM2 level.

This password that you configured at the PM2 level via the command `pm2 set pm2:passwd <password>` is not known from Keymetrics. Only you know it, and this password is only transfered by Keymetrics, making the system very secure.

## Between Keymetrics and your browser

We use the HTTPS protocol to secure all data transfered from Keymetrics to your browser.
