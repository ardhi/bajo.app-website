---
weight: 2320
title: "Anatomy"
description: ""
icon: "article"
date: "2025-11-01T07:25:35+07:00"
lastmod: "2025-11-01T07:25:35+07:00"
draft: false
toc: true
---

Many times, there are more than one handler listening for a particular hook name. Especially in a framework that uses plugins extensively like Bajo, many plugins can listen to one hook at the same time. This creates a problem with call order.

To overcome this problem, Bajo gives you the opportunity to set a ```level```. Functions with a lower level will be called earlier. Functions with no level will be assigned level 999 by default.

Now, change your ```main@say-hello.js``` file above to export an object instead of a function:

```javascript
const sayHello = {
  level: 10, // <-- will get called early
  handler: async function (...params) {
    const [mainChar, friend, payload] = params
    console.log(mainChar, friend, payload) // output: Don, Meri, { movie: 'Jumbo', year: 2025 }
  }
}
```
