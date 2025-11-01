---
weight: 2220
title: "Using Plugins"
description: ""
icon: "article"
date: "2025-11-01T07:11:49+07:00"
lastmod: "2025-11-01T07:11:49+07:00"
draft: false
toc: true
---

Plugins are what make the Bajo Framework so great and flexible: they extend app features and functionalities!

To use plugins, follow these steps:

1. Install it with ```npm install {package}```, where ```{package}``` is the plugin's package name. You can install as many plugins as you want; for a complete list of plugins, please [click here](ecosystem.md).
2. Optionally, create ```{data-dir}/config/{ns}.json``` to customize the plugin's settings, where ```{ns}``` is the namespace or plugin name.
3. Open or create ```{data-dir}/config/.plugins``` and list the plugin's ```{package}``` name in it, one per line.

For example, the text below will load ```bajo-config```, ```bajo-extra```, and ```bajo-template```:

```text
# .plugin file
bajo-config
bajo-extra
bajo-template
```

If you later decide to disable one or more plugins, you just need to remove them from the ```.plugins``` file or place a ```#``` hash mark in front of the package name and restart your app.

> **Warning**: Please do not confuse ```{package}``` and ```{ns}```. The plugin package is the name of the JS package listed on npm, while ```{ns}``` is the namespace or plugin name, which is basically the camel-cased version of the plugin's package name.
