# global-dotenv

Global-dotenv creates the shared persistent `global.env` file that is accessible from all running apps on your machine and allows them to read variables from it and append variables to it. It enables the apps "know about each other" and share information between each other without building a dedicated message system.

## Install

```bash
npm install global-dotenv
```

## Usage

To read the global `global.env` file as JSON, require and parse global-dotenv:

```javascript
const global_dotenv = require('global-dotenv');

var global_dotenv_as_json = global_dotenv.parseSync();
// { FOO: 'BAR', MAY: 'The 4th' }

global_dotenv_as_json['FOO'];
// 'BAR'
```
If the global file doesn't yet exist or is removed, an empty object `{}` is returned. As the file is shared by all running apps, and can be altered by them at any moment, it's recommended to parse it every time right before you want to read the values from it.

To append variables into the global `global.env` file, pass them into appendSync function in the form of `key, value`:

```javascript
const global_dotenv = require('global-dotenv');

global_dotenv.appendSync('FOO', 'BAR');
global_dotenv.appendSync('NUMBER', 1024);
```

Both `key` and `value` will be converted into strings before appending. If typeof `value` is "object", it's converted into a string with JSON.stringify():

```javascript
const global_dotenv = require('global-dotenv');

global_dotenv.appendSync('OBJ', {foo: 'bar'});
// { OBJ: '{"foo":"bar"}' }
```

## Config

The default path to the global file is `/home/global.env` for Linux and `%HOMEDRIVE%:\Users\global.env` for Windows. You can change it by using a custom global file. Do this by passing an options object with a `path` key into `parseSync` and `appendSync` functions: 

```javascript
const global_dotenv = require('global-dotenv');

var global_dotenv_as_json = global_dotenv.parseSync({path: '/path/to/the/file'});

global_dotenv.appendSync('FOO', 'BAR', {path: '/path/to/the/file'});
```
If the file doesn't exist, it will be automatically created. 

## Acessibility of the global.env file

The whole point of this module is that the global file can be accessed by any app on the machine. It's only possible if the apps are running under a user that has access to the global file. **Make sure to start the apps under a user that has access to the global file.** For example if the app uses the default `/home/global.env` file, it either must be started by `root` user or with `sudo` command, or you must manually create `/home/global.env` file and set it's permissions accordingly. If the app that tries to use the global file under a user that has no acces to the global file, `parseSync` and `appendSync` functions will throw an exception. You'll need to try-catch it yourself.

The safe option is to run apps under the same non-root `some_user` user and have a custom global file in `/home/some_user/global.env`.
