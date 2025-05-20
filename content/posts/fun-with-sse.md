---
date: '2025-05-19T14:48:45+05:30'
draft: false
title: 'Fun with Server Sent Events'
---

## Introduction

I was working on a project to experiment with Server‑Sent Events (SSE), which allow the server to send data to the frontend in a unidirectional stream.
While developing an app for freight operators, we encountered a unique problem that I will discuss in this blog

### The app

The application featured a dashboard that listed the trucks an operator wanted to track, and for each truck, a panel displayed insights about its transfer

### The problem

Some users reported that the pages felt janky and unresponsive

### Steps taken to track down the root cause

We captured memory snapshots and profiled the pages, standard steps when diagnosing performance issues, but we found no memory leaks or CPU bottlenecks.

Next, we asked one of our clients to reproduce the issue on their system. Interestingly, they used the app by opening multiple tabs, each showing data for a different truck — because each operator managed more than one vehicle.

### The culprit

We employed Server‑Sent Events to push UI updates, which requires maintaining an open connection from the browser to the SSE endpoint. With multiple tabs open, each tab kept its own connection to the same URL. 

Browsers limit the number of concurrent connections per origin—Chrome, for example, allows roughly six simultaneous connections to one URL. 

After reaching this limit, any additional requests (including asset requests) were dropped, leading to slowed or frozen pages

### The solution

First, we offloaded our notification logic to a web worker so that updates would process in the background, though the open connections still remained

To ensure only a single connection per origin, we switched to a SharedWorker. This setup allowed all tabs to share one worker instance and establish only one SSE connection at a time 

### The code

TODO: Will update this section soon
