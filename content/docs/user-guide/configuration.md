---
weight: 2300
title: "Configuration"
description: "Dealing with configuration files"
icon: "article"
date: "2025-11-10T09:33:40+07:00"
lastmod: "2025-11-10T09:33:40+07:00"
draft: false
toc: true
---

## General

- All configuration files must be placed in the ```{dataDir}/config``` subfolder.
- Config files must be named after their plugin namespace.
- The file format should be either ```.json``` or ```.js```. If a ```.js``` file is used, it must be in ES6 format and should export either a plain JavaScript object or a function. Both synchronous and asynchronous functions are supported. If it returns a function, this function will be called within its plugin scope and should return a plain JS object.
- Other formats (```.yml```, ```.yaml``` and ```.toml```) can also be used by installing & loading [bajoConfig](https://github.com/ardhi/bajo-config) plugin
- The order of precedence is ```.js``` > ```.json``` > ```.yml``` > ```.yaml``` > ```.toml```. This means that if a .js file exists, it will be used instead of a .json file or any other format.

Example:

{{< tabs tabTotal=4 >}}
{{% tab tabName="bajo.js" %}}
```javascript {linenos=table,anchorlinenos=true}
async function config () {
  const env = 'prod'
  const log = {
    pretty: true,
    timeTaken: true
  }
  const lang = 'id'
  return { env, log, lang }
}
```
{{% /tab %}}
{{% tab tabName="bajo.json" %}}
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
{{% /tab %}}
{{% tab tabName="bajo.yml/bajo.yaml" %}}
```yaml {linenos=table,anchorlinenos=true}
env: prod
log:
  pretty: true
  timeTaken: true
lang: id
```
{{% /tab %}}
{{% tab tabName="bajo.toml" %}}
```toml {linenos=table,anchorlinenos=true}
env = "prod"
lang = "id"

[log]
pretty = true
timeTaken = true
```
{{% /tab %}}
{{< /tabs >}}



## Plugins

Plugins are what make the Bajo Framework so great and flexible: they extend app features and functionalities!

To use plugins, follow these steps:

1. Install it with ```npm install {pkgName}```, where ```{pkgName}``` is the plugin's package name. You can install as many plugins as you want; for a complete list of plugins, please [click here](/docs/ecosystem.md).
2. Optionally, create ```{dataDir}/config/{ns}.json``` to customize the plugin's settings, where ```{ns}``` is the namespace or plugin name.
3. Open or create ```{dataDir}/config/.plugins``` and list the plugin's ```{pkgName}``` name in it, one per line.

For example, the text below will load ```bajo-config```, ```bajo-extra```, and ```bajo-template```:

```text {linenos=table,anchorlinenos=true}
# .plugin file
bajo-config
bajo-extra
bajo-template
```

If you later decide to disable one or more plugins, you just need to remove them from the ```.plugins``` file or place a ```#``` hash mark in front of the package name and restart your app.

{{% alert context="warning" %}}
**Warning**: Please do not confuse ```{pkgName}``` and ```{ns}```. The plugin package is the name of the JS package listed on npm, while ```{ns}``` is the namespace or plugin name, which is basically the camel-cased version of the plugin's package name.
{{% /alert %}}

## Environment

Configuration file support for different environments is also available. All you need to do is create a ```{ns}-{env}.json``` file in your ```{dataDir}/config```, where:

- ```{ns}```: the namespace/plugin name
- ```{env}```: your desired environment (```dev``` or ```prod```)
- App-wide settings with ```bajo-{env}.json``` are also possible.

Bajo is smart enough to select which config file will be used based on the following order of precedence:

1. Use ```{ns}-{env}.json``` if the file exists.
2. If not, use ```{ns}.json```.
3. If that also doesn't exist, then use the plugin's default config values.

## Config Override

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

```bash {linenos=table,anchorlinenos=true}
$ node index.js --env=prod --log-pretty --log-timeTaken --lang=id
```
