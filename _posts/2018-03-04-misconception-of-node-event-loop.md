---
title:  "Common Misconception of NodeJs Event Loop"
categories: 
  - Javascript
tags:
  - NodeJs
comments: true
---

Node.js is an event-driven programming environment. This means that everything that happens in Node is the reaction to an event. 
A transaction passing through Node traverses a cascade of callbacks. Abstracted away from the developer, this is all handled by a 
library called libuv which provides a mechanism called an event loop.

This event loop is maybe the most misunderstood concept of the platform. I interviewed many people, most of the people have these kind of
misconceptions. So some of the misconceptions are here..

## Misconception 1: The event loop runs in a separate thread than the user code

### Misconception:

There is a main thread where the JavaScript code of the user (userland code) runs in and another one that runs the event loop. Every time an asynchronous operation takes place, the main thread will hand over the work to the event loop thread and once it is done, the event loop thread will ping the main thread to execute a callback.

### Reality:

There is only one thread that executes JavaScript code and this is the thread where the event loop is running. The execution of callbacks (know that every userland code in a running Node.js application is a callback) is done by the event loop. We will cover that in depth a bit later.

## Misconception 2: Everything that’s asynchronous is handled by a thread pool

### Misconception:

Asynchronous operations, like working with the filesystems, doing outbound HTTP requests or talking to databases are always loaded off to a thread pool provided by libuv.

### Reality:

Libuv by default creates a thread pool with four threads to offload asynchronous work to. Today’s operating systems already provide asynchronous interfaces for many I/O tasks (e.g. AIO on Linux). Whenever possible, libuv will use those asynchronous interfaces, avoiding usage of the thread pool. The same applies to third party subsystems like databases. Here the authors of the driver will rather use the asynchronous interface than utilizing a thread pool. In short: Only if there is no other way, the thread pool will be used for asynchronous I/O.

## Misconception 3: The event loop is something like a stack or queue

### Misconception:

The event loop continuously traverses a FIFO of asynchronous tasks and executes the callback when a task is completed.

### Reality:

While there are queue-like structures involved, the event loop does not run through and process a stack. The event loop as a process is a set of phases with specific tasks that are processed in a round-robin manner.


Philip Roberts has explained [here](https://www.youtube.com/watch?v=8aGhZQkoFbQ) very clearly about what is the event loop and how does it work in run time with clean simulation of the engine.
