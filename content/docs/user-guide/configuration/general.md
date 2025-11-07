---
weight: 2210
title: "General Rules"
description: ""
icon: "article"
date: "2025-11-01T07:11:28+07:00"
lastmod: "2025-11-01T07:11:28+07:00"
draft: false
toc: true
---

- All configuration files must be placed in the ```{dataDir}/config``` subfolder.
- Config files must be named after their plugin namespace.
- The file format should be either ```.json``` or ```.js```. If a ```.js``` file is used, it must be in ES6 format and should export either a plain JavaScript object or a function. Both synchronous and asynchronous functions are supported. If it returns a function, this function will be called within its plugin scope and should return a plain JS object.
- Other formats (```.yml```, ```.yaml``` and ```.toml```) can also be used by installing & loading [bajoConfig](https://github.com/ardhi/bajo-config) plugin
- The order of precedence is ```.js``` > ```.json``` > ```.yml``` > ```.yaml``` > ```.toml```. This means that if a .js file exists, it will be used instead of a .json file or any other format.

Example: bajo.json
```json {linenos=table,anchorlinenos=true}
{
  "env": "prod",
  "log": {
    "pretty": true,
    "timeTaken": true
  },
  "lang": "id"
}
```
