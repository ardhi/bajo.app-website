---
weight: 2230
title: "Environment Support"
description: ""
icon: "article"
date: "2025-11-01T07:12:15+07:00"
lastmod: "2025-11-01T07:12:15+07:00"
draft: false
toc: true
---

Configuration file support for different environments is also available. All you need to do is create a ```{ns}-{env}.json``` file in your ```{dataDir}/config```, where:

- ```{ns}```: the namespace/plugin name
- ```{env}```: your desired environment (```dev``` or ```prod```)
- App-wide settings with ```bajo-{env}.json``` are also possible.

Bajo is smart enough to select which config file will be used based on the following order of precedence:

1. Use ```{ns}-{env}.json``` if the file exists.
2. If not, use ```{ns}.json```.
3. If that also doesn't exist, then use the plugin's default config values.

