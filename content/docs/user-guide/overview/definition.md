---
weight: 2110
title: "Definition"
description: ""
icon: "article"
date: "2025-11-01T08:52:03+07:00"
lastmod: "2025-11-01T08:52:03+07:00"
draft: false
toc: true
---

Before we go any further, here are some of the terminologies I use throughout this documentation:

- ```{app-dir}```: The app directory is where you write all your code (your project directory).
- ```{data-dir}```: The data directory defaults to ```{app-dir}/data``` if not specifically stated. Bajo also creates this directory automatically if it doesn't already exist.
- ```{tmp-dir}```: The temporary directory defaults to the OS temporary directory.
- ```{pkgName}```: The plugin's package name, as it normally appears on an npm listing.
- ```{ns}```: The plugin name or namespace, which is the camel-cased version of the package name.
- ```{mainNs}```: The main namespace, a special plugin and directory named ```main``` located inside your ```{app-dir}``` where you should write all your code.
