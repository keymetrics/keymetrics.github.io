---
layout: docs
title: Memory/CPU profiling
description: Profiling your code
permalink: /docs/pages/profiling/
---

## Memory/CPU Profiling

The profiling section allows you to take memory and CPU snapshots straight from your production servers. You will then get a file that can be inspected with the chrome developer tool.

## Installation

### PM2 v2.3 and more

You can now install the [V8 profiler](https://www.npmjs.com/package/v8-profiler) via pm2, and have it available everyhwere on your server. Just use:

```bash
$ pm2 install profiler
# or
$ pm2 install v8-profiler
```

### Alternative installation for older PM2

You will need to [install pmx](/docs/usage/install-pmx/) and to add the v8-profiler dependency to your project:

```bash
$ npm install v8-profiler --save
```

If you don't see the profiling buttons appearing after restarting your app, consider upgrading pm2 to 2.3+.

## Usage

Once the module is installed, restart your application and different buttons will appea on the Profiling pages:

<img src="/images/heapdump.png" alt="Heapdump"/>

Now click on the button to take a heapdump, it may take some time depending on the weight of the heap file. Once the heapdump file is ready the "Download" button will appear:

<img src="/images/heap2.png" alt="Heapdump"/>

Click on the "Download" button. Once the download is complete, open up the Google Chrome developer tool (Ctrl+Shift+i), go to the Profiles tab and select the option **Load**:

<img src="/images/heap3.png" alt="Heapdump"/>

Select the heapdump file you just downloaded and you will have an overview of your memory.

## Tracking memory leaks

To track memory leak you will need multiple heapdump files and compare them to see which element is increasing over time.

To know more about memory analysis please [follow this link](https://developer.chrome.com/devtools/docs/heap-profiling).
