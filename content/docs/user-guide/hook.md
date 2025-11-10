---
weight: 2300
title: "Hook"
description: "Understand and effectively utilize system hooks"
icon: "article"
date: "2025-11-10T09:26:06+07:00"
lastmod: "2025-11-10T09:26:06+07:00"
draft: false
toc: true
---

A **hook** refers to a mechanism that allows you to inject a custom function to extend Bajo's functionality at specific points. These points are typically predefined by the framework, providing opportunities to execute code before, during, or after a particular operation.

## Usage

In Bajo, hooks can be created anywhere very easily. Simply call the ```runHook``` method followed by the parameters you want to pass.

The hook name is always in the form of [TNsPathPairs](https://ardhi.github.io/bajo/global.html#TNsPathPairs), while its parameters are a rest parameter. This means you can pass any number of parameters to the function, or none at all.

Example:

1. In your JavaScript file, add the following code snippet:

   ```javascript {linenos=table,anchorlinenos=true}
   const { runHook } = this.app.bajo
   await runHook('main:sayHello', 'Don', 'Meri', { movie: 'Jumbo', year: 2025 })
   ````
2. Go to directory ```{appDir}/main/extend/bajo/hook```. Create one if it doesn't exist yet.
3. Create file ```main@say-hello.js``` in the directory above.
4. Enter these lines:
   ```javascript {linenos=table,anchorlinenos=true}
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

## Anatomy

Many times, there are more than one handler listening for a particular hook name. Especially in a framework that uses plugins extensively like Bajo, many plugins can listen to one hook at the same time. This creates a problem with call order.

To overcome this problem, Bajo gives you the opportunity to set a ```level```. Functions with a lower level will be called earlier. Functions with no level will be assigned level 999 by default.

Now, change your ```main@say-hello.js``` file above to export an object instead of a function:

```javascript {linenos=table,anchorlinenos=true}
const sayHello = {
  level: 10, // <-- will get called early
  handler: async function (...params) {
    const [mainChar, friend, payload] = params
    console.log(mainChar, friend, payload) // output: Don, Meri, { movie: 'Jumbo', year: 2025 }
  }
}
```

## Caveats

Hooks give you a lot of flexibility and freedom, but you need to be aware of the following caveats:

1. You need to use an **asynchronous** function. Even if your function is synchronous, it will be called as an asynchronous one—and as you know, there is a performance degradation when using asynchronous operations
2. **Stay away** from using ```runHook``` inside a hook! Even though it's possible, your code will become unreadable and messy pretty soon.
3. It's hard to trace errors in a hook. Because of its sequential nature, if a handler that's called earlier than yours throws an error, your hook won't be called at all.
4. If you use so many plugins that use the hook system so extensively with so many files, your app's boot time can take much longer than it's supposed to.

My advice is to **use it wisely**. Don't use hooks unless necessary; this will make your app or plugin clean and easy to understand.
