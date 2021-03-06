# Terminus Secrets Plugin

[![Terminus v1.x Compatible](https://img.shields.io/badge/terminus-v1.x-green.svg)](https://github.com/pantheon-systems/terminus-secrets-plugin/tree/1.x)
[![Terminus v0.x Compatible](https://img.shields.io/badge/terminus-v0.x-green.svg)](https://github.com/pantheon-systems/terminus-secrets-plugin/tree/0.x)

Terminus Plugin that allows for manipulation of a simple 'secrets.json' file for use with Quicksilver on [Pantheon](https://www.pantheon.io) sites.

Adds a command 'secrets' to Terminus 0.x which you can use to add, fetch, remove and update. For a version that works with Terminus 1.x, see the [1.x branch](https://github.com/pantheon-systems/terminus-secrets-plugin/tree/1.x).

Use as directed by Quicksilver examples.

Be aware that since this manages a simple json file in the network attached storage, filesystem synchronization operations will affect the secrets. You should not use this method if your use-case is to have different secrets in different environments. For that, or for secrets that are sensitive, we recommend [Lockr](https://github.com/lockr/lockr-terminus).

## Configuration

This plugin requires no configuration to use.

## Examples
Write "value" into "key" in the "test" environment of "sitename".
```
terminus secrets set --site=sitename --env=test key value
```

Remove the secret "key" in the "test" environment of "sitename".
```
terminus secrets delete --site=sitename --env=test key
```

Show current value of "key" in the "test" environment of "sitename".
```
terminus secrets show --site=sitename --env=test key
```

Show all available keys in the "test" environment of "sitename"
```
terminus secrets show --site=sitename --env=test
```

Learn more about Terminus and Terminus Plugins at:
[https://github.com/pantheon-systems/cli/wiki/Plugins](https://github.com/pantheon-systems/cli/wiki/Plugins)

## Installation
For help installing, see [Terminus's Wiki](https://github.com/pantheon-systems/terminus/wiki/Plugins)
```
mkdir -p ~/terminus/plugins
composer create-project -d ~/terminus/plugins pantheon-systems/terminus-secrets-plugin:~0
```

## Internals

This plugin writes entries into the file `private/secrets.json`.  This file is, naturally enough, a json file containing multiple keys.  The `terminus secrets` script will fetch this file, modify is as requested, and then write it back to the Pantheon site.

Note that the `private` directory is located one level above the local working copy of your git repository on your Pantheon application server. This directory is **not** copied to `test` and `live` during deployments; you must therefore individually set secrets on each environment where you would like them to be available.

Also, be aware that your secrets may be overwritten by filesystem sync operations. For instance, if you check the "pull files and database from Live" option when deploying to Test, that will overrite the Test env with secrets (or a lack thereof) in Live. If you intend to use secrets.json for production, make sure you set the same file in all environments to avoid confusion.

## Help
Run `terminus help secrets` for help.

### Note on Bug in Help

Terminus has a bug which requires positional arguments to be described in help as 'email', as any other alternative results in the argument failing validation.

