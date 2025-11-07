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

Using dotenv:

1. Create or open ```{appDir}/.env```
2. Use ```__``` (double underscores) as replacement for dots in an object.
3. ```DIR__DATA```: Sets the ```{dataDir}``` data directory.
4. ```DIR__TMP```: Sets ```{tmpDir}``` temporary directory.
5. For every key in ```{ns|bajo}.json```, use its snake-cased, upper-cased version. For example:
   - ```env: 'prod'``` → ```ENV=prod```
   - ```log.dateFormat: 'YYYY-MM-DD'``` → ```LOG__DATE_FORMAT=YYYY-MM-DD```
   - ```exitHandler: true``` → ```EXIT_HANDLER=true```
6. To override a plugin's config, prepend every key in the plugin's config with the snake-cased, upper-cased version of the namespace followed by a dot. For example:
   - ```key``` in ```myPlugin``` → ```MY_PLUGIN.KEY=...```
   - ```key.subKey.subSubKey``` in ```myPlugin``` → ```MY_PLUGIN.KEY__SUB_KEY__SUB_SUB_KEY=...```

Example:
```text {linenos=table,anchorlinenos=true}
# .env file
ENV=prod
LOG__PRETTY=true
LOG__TIME_TAKEN=true
LANG=id
```

### Argument Switches

Using argument switches:

1. Use switches, for example: ```node index.js --xxx=one --yyy=two```
2. Use ```-``` as the replacement for dots in an object.
3. ```--dir-data```: Sets the ```{dataDir}``` data directory.
4. ```--dir-tmp```: Sets the ```{tmpDir}``` temporary directory.
5. For every key in ```{ns|bajo}.json```, add ```--``` prefix. For example:
   - ```env: 'prod'``` → ```--env=prod```
   - ```log.dateFormat: 'YYYY-MM-DD'``` → ```--log-dateFormat=YYYY-MM-DD```
   - ```exitHandler: true``` → ```--exitHandler```
6. To override a plugin's config, prepend every key in the plugin's config with the plugin name followed by a colon ```:```. For example:
   - ```key``` in ```myPlugin``` → ```--myPlugin:key=...```
   - ```key.subKey.subSubKey``` in ```myPlugin``` → ```--myPlugin:key-subKey-subSubKey=...```

Example:
```bash
$ node index.js --env=prod --log-pretty --log-timeTaken --lang=id
```
