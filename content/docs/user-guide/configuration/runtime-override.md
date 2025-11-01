---
weight: 2240
title: "Runtime Override"
description: ""
icon: "article"
date: "2025-11-01T07:15:02+07:00"
lastmod: "2025-11-01T07:15:02+07:00"
draft: false
toc: true
---

You can easily override ANY key-value pair setting with environment variables and program argument switches. Bajo also supports [dotenv](https://github.com/motdotla/dotenv) with a ```.env``` file.

The order of precedence is: environment variable > argument switches > config files > default, built-in values.

All values (whether they come from environment variables, argument switches, or config files) will be parsed using [dotenv-parse-variables](https://github.com/ladjs/dotenv-parse-variables), so please make sure you visit the repository to fully understand how it works.

### dotenv

- Create or open ```{app-dir}/.env```
- Use ```__``` (double underscores) as replacement for dots in an object.
- ```DIR__DATA```: Sets the ```{data-dir}``` data directory.
- ```DIR__TMP```: Sets ```{tmp-dir}``` temporary directory.
- For every key in ```{ns|bajo}.json```, use its snake-cased, upper-cased version. For example:
  - ```env: 'prod'``` → ```ENV=prod```
  - ```log.dateFormat: 'YYYY-MM-DD'``` → ```LOG__DATE_FORMAT=YYYY-MM-DD```
  - ```exitHandler: true``` → ```EXIT_HANDLER=true```
- To override a plugin's config, prepend every key in the plugin's config with the snake-cased, upper-cased version of the namespace followed by a dot. For example:
  - ```key``` in ```myPlugin``` → ```MY_PLUGIN.KEY=...```
  - ```key.subKey.subSubKey``` in ```myPlugin``` → ```MY_PLUGIN.KEY__SUB_KEY__SUB_SUB_KEY=...```

Example:
```text
# .env file
ENV=prod
LOG__PRETTY=true
LOG__TIME_TAKEN=true
LANG=id
```

### Argument Switches

- Use switches, for example: ```node index.js --xxx=one --yyy=two```
- Use ```-``` as the replacement for dots in an object.
- ```--dir-data```: Sets the ```{data-dir}``` data directory.
- ```--dir-tmp```: Sets the ```{tmp-dir}``` temporary directory.
- For every key in ```{ns|bajo}.json```, add ```--``` prefix. For example:
  - ```env: 'prod'``` → ```--env=prod```
  - ```log.dateFormat: 'YYYY-MM-DD'``` → ```--log-dateFormat=YYYY-MM-DD```
  - ```exitHandler: true``` → ```--exitHandler```
- To override a plugin's config, prepend every key in the plugin's config with the plugin name followed by a colon ```:```. For example:
  - ```key``` in ```myPlugin``` → ```--myPlugin:key=...```
  - ```key.subKey.subSubKey``` in ```myPlugin``` → ```--myPlugin:key-subKey-subSubKey=...```

Example:
```bash
$ node index.js --env=prod --log-pretty --log-timeTaken --lang=id
```
