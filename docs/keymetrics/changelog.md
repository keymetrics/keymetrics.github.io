---
layout: docs
title: Keymetrics application Changelog
description: Keymetrics changelog
permalink: /docs/usage/keymetrics-changelog/
---

## February 2016

- Feature: Offline system available again
- Feature: Better support for auto scaling systems (via auto server deletion on offline)

## January 2016

- Enhancement: Realtime system refactored with Primus
- Enhancement: 40% back end more stable (leak fixed)

## December 2015

- Feature: CPU profile page now display D3.js charts and analyze cpu profile
- Enhancement: Realtime system refactored from socket.io 0.9.x to raw websockets
- Enhancement: System is now much more stable and realtime connection infinite
- Enhancement: CPU profiling system and Heapdump is now much more stable
- Feature: Custom metrics can now be configured to have a threshold (@todoc)
- Enhancement: Log display now preprend timestamp to each line
- Enhancement: Log display now supports chalk (or any kind of colored output)
- Enhancement: Log display now display all messages
- Enhancement: In monitoring page do not switch trendline and graph value in charts
- Enhancement: Online servers are listed first in dashboard page
- Enhancement: Now widgets in the dashboard are pixel perfect
- Enhancement: Some enhancements has been done in modal popup
- Fix: Invert module listing
- Fix: Don't display wrong events when app filtered in monitoring page
- Fix: Deletion of meta servers easier

## November 2015

- When filtering by app, in the dashboard there is an Orchestration box available in https://cl3.keymetrics.io
- Hide server info when filtering by server
- Better monitoring preview page
- Refactor trenline generation
- Add trendline in memory chart
- Add regression line in custom metrics charts
- Values in chart tooltips are now normalized
- Slack integration
