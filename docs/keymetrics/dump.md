---
layout: docs
title: Memory/CPU profiling
description: Profiling your code
permalink: /docs/pages/profiling/
---

## Memory/CPU Profiling

The profiling section allow you to take snapshot of memory and CPU straight from your production servers. You will then get a file that can be inspected with the chrome developer tool.

## Usage

You will need to [install pmx](/docs/usage/install-pmx/) and add the v8-profiler dependecy to your project:

```bash
$ npm install v8-profiler --save
```

Once the module installed, restart your application and you will be able to see buttons on Profiling pages:

<img src="/images/heapdump.png" alt="Heapdump"/>

Now click on the button to take a heapdump, it may take some time depending on the weight of the heap file. Once the heapdump file is ready you will have a button "Download":

<img src="/images/heap2.png" alt="Heapdump"/>

Click on the Download button. Once downloaded, open up the Google Chrome developer tool (Ctrl+Shift+i), go to the Profiles tab and select the option **Load**:

<img src="/images/heap3.png" alt="Heapdump"/>

Select the heapdump file just downloaded and you will have an overview of your memory.

## Tracking memory leaks

To track memory leak you will need multiple heapdump file and compare them to see which element is increasing over time.

To know more about memory analysis please [follow this link](https://developer.chrome.com/devtools/docs/heap-profiling)
