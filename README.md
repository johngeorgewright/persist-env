[![Build Status](https://img.shields.io/travis/johngeorgewright/persist-env/master.svg?style=flat-square)](https://travis-ci.org/johngeorgewright/persist-env)
[![NPM Version](https://img.shields.io/npm/v/persist-env.svg?style=flat-square)](https://www.npmjs.com/package/persist-env)
[![Dependencies](https://img.shields.io/gemnasium/johngeorgewright/persist-env.svg?style=flat-square)](https://gemnasium.com/github.com/johngeorgewright/persist-env)
[![Greenkeeper badge](https://badges.greenkeeper.io/johngeorgewright/persist-env.svg)](https://greenkeeper.io/)
[![License](https://img.shields.io/npm/l/persist-env.svg?style=flat-square)](https://github.com/johngeorgewright/persist-env/blob/master/LICENSE)

# persist-env

> Persist NPM config between scripts

## Problem

Running `npm run ...blah` resets any configuration variables (environment
variables starting with `$npm_package_config_`). Therefore a script like so:

```
npm_package_config_foo=bar; npm test
```

... means that during `npm test` the config variable `foo`
(`process.env.npm_package_config_foo`) not be `bar` but infact it's previous
value.

Prefixing your commands with this script replaces all of your `npm run ...blah`
with the script referenced in your package file.

### Example

Image your package looks something like:

```json
{
  "name": "my-project",
  "version": "1.0.0",
  "config": {
    "db_name": "my_db"
  },
  "scripts": {
    "get-db": "echo $npm_package_config_db_name",
    "test": "npm_package_config_db_name=\"${npm_package_config_db_name}_test\"; npm run -s get-db && echo done"
  }
}
```

Then running the test command will produce the following:

```
$> npm test

> my-project@1.0.0 test /some/path
> npm_package_config_db_name="${npm_package_config_db_name}_test"; npm run -s get-db && echo done

my_db
done
```

Notice the config variable (`npm_package_config_db_name`) wasn't carried over
to `get-db`. To achieve this all I now need to do is a add `prst` infront of
`npm run -s get-db`:

```
npm_package_config_db_name="${npm_package_config_db_name}_test"; prst npm run -s get-db && echo done
```

Now running the script will produce the desired result:

```
$> npm test

> my-project@1.0.0 test /some/path
> npm_package_config_db_name="${npm_package_config_db_name}_test"; npm run -s get-db && echo done

my_db_test
done
```

## Installation

```
npm i -D persist-env
```
