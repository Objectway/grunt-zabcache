# grunt-zabcache

> Grunt task for generating an HTML5 AppCache manifest from the specified list of files. Forked from https://github.com/marcozabo/grunt-zabcache with headcomment support. The headcomment allow to add freetext comment in the first line of the manifest

## Getting Started
This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

```shell
npm install grunt-zabcache --save-dev
```

Once the plugin has been installed, it may be enabled inside your Gruntfile with this line of JavaScript:

```js
grunt.loadNpmTasks('grunt-zabcache');
```

## The "zabcache" task

### Overview
In your project's Gruntfile, add a section named `zabcache` to the data object passed into `grunt.initConfig()`.

```js
grunt.initConfig({
  zabcache: {
    options: {
      // Task-specific options go here.
    },
    your_target: {
      // Target-specific file lists and/or options go here.
    },
  },
})
```

### Options

#### options.basePath
Type: `String`
Default value: `process.cwd()`

The absolute or relative path to the directory to consider as the root of the application for which to generate the cache manifest.

#### options.headcomment
Type: `String`
Default value: ``

Custom string for the comment section

#### options.baseUrl
Type: `String`
Default value: `undefined`

The base URL to prepand to all expanded cache entries.

#### options.ignoreManifest
Type: `Boolean`
Default value: `true`

Specifies if to ignore the cache manifest itself from the list of files to insert in the "CACHE:" section.

#### options.preferOnline
Type: `Boolean`
Default value: `false`

Specifies whether to write the "prefer-online" entry in the "SETTINGS:" section or not. 

### Target fields

#### dest

`String` indicating the output path for the AppCache manifest.

#### cache

A descriptor for the "CACHE:" entries. A cache descriptor can be either a `String` or an `Array` of `String`s, in the format accepted by the `patterns` argument to [grunt.file.expand](http://gruntjs.com/api/grunt.file#grunt.file.expand).
Alternatively, this argument can be an `Object` containing the optional properties `patterns` (a cache descriptor, as defined earlier) and `literals` (`String` or `Array` of `String`s to insert as is in the "CACHE:" section).

#### ignored

Files to be ignored and excluded from the "CACHE:" entries. This parameter has been deprecated since v0.1.4, when proper support for the `!` prefix to the glob patterns was added (this serves the same purpose while being more concise).

#### network

`String` or `Array` of `String`s to insert as is in the "NETWORK:" section.

#### fallback

`String` or `Array` of `String`s to insert as is in the "FALLBACK:" section.

### Usage Examples

In this example, the module is set to generate an AppCache manifest from the contents of the `static` directory, placing the resulting manifest in `static/manifest.appcache`. The `basePath` option allows the module to generate paths relative to the `static` directory, instead of the directory where the gruntfile resides.

```js
grunt.initConfig({
  zabcache: {
    options: {
      basePath: 'static'
    },
    all: {
      dest: 'static/manifest.appcache',
      cache: 'static/**/*',
      network: '*',
      fallback: '/ /offline.html'
    }
  }
})
```

The next example uses the extended syntax to the `cache` parameter and add the version number taken from your package.json:

```js
grunt.initConfig({
pkg: grunt.file.readJSON('package.json'),
  zabcache: {
    options: {
      basePath: 'static',
      headcomment: '<%= pkg.name %> version: <%= pkg.version %>'
    },
    all: {
      dest: 'static/manifest.appcache',
      cache: {
        patterns: [
          'static/**/*',         // all the resources in 'static/'
          '!static/ignored/**/*' // except the 'static/ignored/' subtree
        ],
        literals: '/'            // insert '/' as is in the "CACHE:" section
      }
    }
  }
})
```
The last example uses headcomment to add free text in the first commented line:

```js
grunt.initConfig({
pkg: grunt.file.readJSON('package.json'),
  zabcache: {
    options: {
      basePath: 'static',
      headcomment: "MyWebApp 1.0.0"
    },
    all: {
      dest: 'static/manifest.appcache',
      cache: {
        patterns: [
          'static/**/*',         // all the resources in 'static/'
          '!static/ignored/**/*' // except the 'static/ignored/' subtree
        ],
        literals: '/'            // insert '/' as is in the "CACHE:" section
      }
    }
  }
})
```
