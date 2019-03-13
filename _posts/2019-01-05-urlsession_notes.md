---
title: 'URLSession - Notes'
date: 2019-01-05
permalink: /posts/urlsession
tags:
  - iOS
  - URLSession
  - iOS Networking
---

The following is a condensed notes from Apple Developer documentation and other sites from internet.

* URL Loading is performed asynchronously
* One URLSession can be used repeatedly to create tasks. But separate sessions can be created for different purposes. For example, a new session can be created for each tab in a browser.
* Each session is associated with a delegate to get periodic updates.
* Session can be configured to run in background so that any downloads keep continuing even if the app is quit/suspended and is notified of download completion when the app is active again.
* Data tasks can be used for server requests. They are not supported in background sessions
* Upload and Download tasks are supported in all sessions

