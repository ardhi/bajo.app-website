---
weight: 2310
title: "Usage"
description: ""
icon: "article"
date: "2025-11-01T07:25:12+07:00"
lastmod: "2025-11-01T07:25:12+07:00"
draft: false
toc: true
---

A **hook** refers to a mechanism that allows you to inject a custom function to extend Bajo's functionality at specific points. These points are typically predefined by the framework, providing opportunities to execute code before, during, or after a particular operation.

In Bajo, hooks can be created anywhere very easily. Simply call the ```runHook``` method followed by the parameters you want to pass.

The hook name is always in the form of [TNsPairs](global.html#TNsPathPairs), while its parameters are a rest parameter. This means you can pass any number of parameters to the function, or none at all.

Example:

- In your JavaScript file, add the following code snippet:

  ```javascript
  const { runHook } = this.app.bajo
  await runHook('main:sayHello', 'Don', 'Meri', { movie: 'Jumbo', year: 2025 })
  ````
- Go to directory ```{app-dir}/main/extend/bajo/hook```. Create one if it doesn't exist yet.
- Create file ```main@say-hello.js``` in the directory above.
- Enter these lines:
  ```javascript
  async function sayHello (...params) {
    const [mainChar, friend, payload] = params
    console.log(mainChar, friend, payload) // output: Don, Meri, { movie: 'Jumbo', year: 2025 }
  }

  export default sayHello
  ```

Note the hook name and its associated file name:

```main:sayHello``` → ```main@say-hello.js```

Because a colon (```:```) is prohibited in a file name, Bajo replaces it with the ```@``` symbol.

During the boot process, Bajo will scan for hook files and load them into the hook list. When your ```runHook``` is executed, Bajo will find its related object from the list. If such a hook exists, its function handler will be called.
