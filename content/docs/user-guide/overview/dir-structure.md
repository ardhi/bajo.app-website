---
weight: 2120
title: "Directory Structure"
description: ""
icon: "article"
date: "2025-11-01T08:52:25+07:00"
lastmod: "2025-11-01T08:52:25+07:00"
draft: false
toc: true
---

Your typical Bajo app directory structure should look like this:

```
|- {app-dir}
|  |- {data-dir}
|  |  |- config
|  |  |  |- .plugin
|  |  |  |- bajo.json
|  |  |  |- main.json
|  |  |  |- ...
|  |- main
|  |  |- extend
|  |  |- index.js
|  |  |  ...
|  |- package.json
|  |- index.js
|  |  ...
```

- You can move ```{data-dir}``` out of ```{app-dir}``` if you want, but you need to tell Bajo where to find it. For more on this, please follow along.
- ```{data-dir}``` should be the only place Bajo **writes** anything. Bajo and its plugins should **never** be allowed to write anything outside ```{data-dir}``` on their own.
- ```config``` is a special directory within ```{data-dir}``` where your configuration files should reside. Inside this directory, you should find:
  - a special file named ```.plugins``` that tells Bajo which plugins should be loaded
  - a file named ```bajo.json``` to configure global settings
  - all plugin-specific config files, named after their namespace
- The ```main``` directory, or ```{mainNs}``` namespace, is the special plugin where you put your application code. And yes, it is actually a normal Bajo plugin! This means everything in there will be handled just like a regular plugin—it has the ability to extend other plugins, has its own config file, and more — with a few differences:
  - it's always available and can't be disabled
  - it's always the last one to start
  - if this directory is missing, it will be created automatically on startup
  - if the plugin's factory function is missing (```index.js```), it will be created dynamically
- ```index.js``` is the main entry point for your app.

To set your ```{data-dir}``` somewhere else, you need to tell Bajo where to find it by using an argument switch.

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
