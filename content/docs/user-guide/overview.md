---
weight: 2100
title: "Overview"
description: "Terminology, directory structure, and supported runtime"
icon: "article"
date: "2025-11-10T09:31:21+07:00"
lastmod: "2025-11-10T09:31:21+07:00"
draft: false
toc: true
---

{{% alert context="info" %}}
All examples in this chapter utilize **Node.js**. If you prefer **Bun**, substitute ```node``` with ```bun``` in the commands.
{{% /alert %}}

## Terminology

Before proceeding, familiarize yourself with these terminologies used throughout this documentation:

1. ```{appDir}```: The application's project directory where all your code residese.
2. ```{dataDir}```: The directory for all data; defaults to ```{appDir}/data``` and is created automatically if missing.
3. ```{tmpDir}```: The temporary directory, defaults to the OS temporary directory.
4. ```{pkgName}```: The plugin's package name, typically matching its npm listing.
5. ```{ns}```: The plugin name or namespace, which is the camel-cased version of the package name.
6. ```{mainNs}```: The special main namespace and directory named main within your ```{appDir}```; this is where all application code should be written.

## Directory Structure

Your typical Bajo app directory structure should look like this:

```
.
└── {appDir}
    ├── {dataDir}
    │   ├── config
    │   │   ├── .plugin
    │   │   ├── bajo.json
    │   │   ├── main.json
    │   │   └── ...
    │   └── plugins
    │       └── ...
    ├── main
    │   ├── extend
    │   │   └── ...
    │   ├── index.js
    │   └── ...
    ├── package.json
    ├── index.js
    └── ...
```

1. You can move ```{dataDir}``` out of ```{appDir}``` if you want, but you need to tell Bajo where to find it. For more on this, please follow along.
2. ```{dataDir}``` should be the only place Bajo **writes** anything. Bajo and its plugins should **never** be allowed to write anything outside ```{dataDir}``` on their own.
3. ```config``` is a special directory within ```{dataDir}``` where your configuration files should reside. Inside this directory, you should find:
   - a special file named ```.plugins``` that tells Bajo which plugins should be loaded
   - a file named ```bajo.json``` to configure global settings
   - all plugin-specific config files, named after their namespace
4. The ```main``` directory, or ```{mainNs}``` namespace, is the special plugin where you put your application code. And yes, it is actually a normal Bajo plugin! This means everything in there will be handled just like a regular plugin—it has the ability to extend other plugins, has its own config file, and more — with a few differences:
   - it's always available and can't be disabled
   - it's always the last one to start
   - if this directory is missing, it will be created automatically on startup
   - if the plugin's factory function is missing (```index.js```), it will be created dynamically
5. ```index.js``` is the main entry point for your app.

To set your ```{dataDir}``` somewhere else, you need to tell Bajo where to find it by using an argument switch.

Assuming your data directory is ```my-data-dir``` at the same level as your app directory, run your app like this:

```bash
$ node index.js --dir-data=../my-data-dir
```

If using program arguments seems a bit like a hassle for you, just use Bajo's [dotenv](https://github.com/motdotla/dotenv) support. Create a ```.env``` file in your app directory and put this inside:

```text
DIR__DATA=../my-data-dir              # double underscores!!!
```

From now on, you can start the app just by typing:

```bash
$ node index.js
```

## Runtime

Bajo should run perfectly fine on Node.js version 20 or higher. Using the latest stable runtime is recommended. Bajo-based apps are also known to run with **Bun** without any problems. But Bajo **cannot** run on Deno due to its heavy reliance on Node.js-specific libraries and environments.

Bajo is a pure ES6 framework that utilizes dynamic imports **a lot**. Running on a system with a fast disk (e.g., SSD) and enough RAM is highly recommended, especially when you load a lot of plugins.
