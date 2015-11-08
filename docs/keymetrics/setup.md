---
layout: docs
title: Setup Keymetrics and PM2
description: Setup Keymetrics and PM2
permalink: /docs/usage/setup/
---

Getting started with Keymetrics is seamless, smart and straightforward:

## 1- Create an account

First create an account in Keymetrics: [https://app.keymetrics.io/#/register](https://app.keymetrics.io/#/register)

![register](/images/tutorial/register.png)

## 2- Create a Bucket

Then you will land to the Bucket page.

**Bucket**: A bucket is a way to organize multiple servers in the same context. One bucket can used for one your customer or one bucket could represent an environment like *staging*, *production*, *test*.

Click on the button create a new bucket:

![create a new bucket](/images/tutorial/create-bucket.png)

Give a name to your bucket and an optional comment:

![post bucket](/images/tutorial/post-bucket.png)

## 3- Link your server to Keymetrics

Congratulation, you have succesfully created you very first bucket in Keymetrics! You benefits now from a public and secret key that will identify and secure the connectivity between your server and Keymetrics,

You will land up to the following page telling you how to link PM2 to Keymetrics:

![popup link to pm2](/images/tutorial/popup-link.png)

Just copy paste (or click on the green button):

```bash
$ wget -qO- http://install.keymetrics.io/install.sh | SECRET_ID=1k706942ti26lb7 PUBLIC_ID=0m0bbf0zymvsrrh bash
```

This will look for a local Node.js binary else it will install it, then will install PM2 and will directly link your server with Keymetrics.

You can see the result in realtime, or else refresh the page to see your server being monitored.

![installation success](/images/tutorial/installation-succesful.png)

Now you are ready to start an application with PM2 and supervize it on Keymetrics!

## 4- Start an application

Now that you are ready to go, let's start an application to see the result in Keymetrics.

If you have a Node.js application at hand, start it with:

```bash
$ pm2 start my-app.js
```

Else use our sample application:

```bash
$ git clone https://github.com/keymetrics/app-playground.git
$ cd app-playground
$ pm2 start app.js --watch
```

![sample app](/images/tutorial/sample-app.png)
