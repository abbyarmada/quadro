# Quadro framework

[![Code Climate](https://codeclimate.com/github/WisePricer/quadro/badges/gpa.svg)](https://codeclimate.com/github/WisePricer/quadro)
[![CircleCI](https://circleci.com/gh/WisePricer/quadro.svg?style=shield)](https://circleci.com/gh/WisePricer/quadro)

TOC:

<!-- TOC depthFrom:1 depthTo:6 withLinks:1 updateOnSave:1 orderedList:0 -->

- [Quadro framework](#quadro-framework)
	- [Requirements](#requirements)
	- [Installation](#installation)
	- [Configuration manager](#configuration-manager)
	- [Tests runner](#tests-runner)
	- [HTTP Server](#http-server)
		- [HTTP port](#http-port)

<!-- /TOC -->

## Requirements

- NodeJS v7.6.0+

## Installation

Create a new app and add the following to package.json:

```json
"dependencies": {
  "quadro": ""
}
```

In `app.js` add:

```js
const Quadro = require('quadro')
Quadro()
```

You're done! Run with:

```sh
node app.js [--watch]
```

Local configuration:
You can maintain your local-machine specific configuration config/local.
For git to ignore those changes use:

```sh
git update-index --skip-worktree **/config/local/*
```

Test with:

```sh
node app.js test [--watch]
```

## Configuration manager

Config files should be placed in `ROOT/config/` dir.

To get value of `key.nestedKey` in configuration file named `config/someConfig.yml`:

```js
app.config.someConfig.key.nestedKey
```

For safe configuration fetch, use `config.get`:

```js
app.config.get('someConfig.key.nestedKey')
```

To get value of `key.nestedKey` in `config/someConfig.yml|yaml|js|json` and
defaults to `3000` if such key doesn't exist

```js
app.config.get('someConfig.key.nestedKey', 3000)
```


### Environment-specific configuration

Quadro will load configuration from these paths (in the following order):

- config/
- config/$NODE_ENV
- config/local

Note: $NODE_ENV - is the NODE_ENV environment variable value

Configurations are merged while loading. In the following example:

```
|-- config/quadro.yml
  |- dev/quadro.yml
  |- local/quadro.yml
```

Configuration keys from `local/quadro.yml` will override keys from `dev/quadro.yml`
which in turn will override keys from `config/quadro.yml`.

It is highly recommended that any config values that are tweaked during debugging/development
will be added to `local/` configs. This way you can add `**/config/local/` to `.gitignore`
and be able to keep your dev environment clean during commits/pushes.

## Tests runner
To run tests use:

```sh
node app.js test
```

To make tests re-run on file changes use:

```sh
node app.js test --watch
```

## HTTP Server

### HTTP port

Use config value: `quadro.http.port`
Default: `3000`


## Logger

Use app.log.trace|debug|info|warn|error

Set logging level through config `quadro.logger.level`. Default is `info`.

# Application instance

## Key attributes

- `.log` - Logger
- `.env` - Runtime environment (dev|test|production), comes from NODE_ENV
- `.name` - Application name. Taken from `package.json`
- `.config` - Application configuration manager

## Additional attributes

- `.cliParams` - Parsed CLI parameters
- `.appDir` - Application
- `.quadroDir` - Location of quadro package
- `.http` - HTTP Server reference
- `.koaApp` - KOA Application
