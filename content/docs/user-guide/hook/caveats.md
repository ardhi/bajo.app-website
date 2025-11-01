---
weight: 2330
title: "Caveats"
description: ""
icon: "article"
date: "2025-11-01T07:25:44+07:00"
lastmod: "2025-11-01T07:25:44+07:00"
draft: false
toc: true
---

Hooks give you a lot of flexibility and freedom, but you need to be aware of the following caveats:

- You need to use an **asynchronous** function. Even if your function is synchronous, it will be called as an asynchronous one—and as you know, there is a performance degradation when using asynchronous operations
- **Stay away** from using ```runHook``` inside a hook! Even though it's possible, your code will become unreadable and messy pretty soon.
- It's hard to trace errors in a hook. Because of its sequential nature, if a handler that's called earlier than yours throws an error, your hook won't be called at all.
- If you use so many plugins that use the hook system so extensively with so many files, your app's boot time can take much longer than it's supposed to.

My advice is to **use it wisely**. Don't use hooks unless necessary; this will make your app or plugin clean and easy to understand.

