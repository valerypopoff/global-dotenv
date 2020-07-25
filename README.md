# global-dotenv

Dotenv creates the shared global `global.env` file that is accessible from all running apps on your machine and allows them to read variables from it and append variables to it.

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

The default path to the global file is `/home/global.env`. You can change it by passing an options object with a `path` key into parseSync and appendSync functions: 

```javascript
const global_dotenv = require('global-dotenv');

var global_dotenv_as_json = global_dotenv.parseSync({path: '/path/to/the/file'});

global_dotenv.appendSync('FOO', 'BAR', {path: '/path/to/the/file'});
```
If the file doesn't exist, it will be automatically created. 
