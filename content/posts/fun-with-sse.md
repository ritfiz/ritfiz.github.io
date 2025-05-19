---
date: '2025-05-19T14:48:45+05:30'
draft: false
title: 'Fun with Server Sent Events'
---

## Introduction

As the title goes, I was working on a project where I was trying to have fun with Server Sent Events.

If you already know what it is, its a way for the server to send data to the frontend. Something similar to websocket but unidirectional in nature. 

While we were building an app for Freight operators. We came across a very unique problem about which I will talk in today's blog.

### The app

A dashboard with a list of trucks that the operator would like to track and for every truck, a panel showing insights about the transfer was shown.

### The problem

We got some people complaining to us about jankiness and unresposive pages. 

### Steps taken to track down the root cause

Checking memory snapshots and profiling pages was a nobrainer for such an issue, but we did not find any specific issue with them. No memory leaks. No CPU hog.

We got on a call with one of our clients and asked them to show us the issue on their system. Interestigly, they were using the app in a different manner. Since, all operators had more than one trucks being operated, the user had multiple tabs opened with different truck information shown on each page.

## The culprit

We were using Server sent events to send updates to the UI. Now, when a browser has to receive data from the SSE, it basically needs to keep a connection alive. When users opened multiple tabs, they were also keeping multiple connections open to the same url. 

Every browser has a limit on the number of connections it can have with a server url at a time. For chrome it is around 6. Once the connection limit is reached, any kind of request be it assets etc were getting dropped.

## The solution

We offloaded the notification system to web workers as a first step. But that meant it will just process the request in the background, The connections would still be alive. 

So we moved to SharedWorkers. A single worker which was shared across tabs and only one connection was established at a time.

## The code

TODO: Will update this section soon
