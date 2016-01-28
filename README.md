# grunt-zabcache

> The simplest way to build your own appcache.manifest file with Grunt. Forked from https://github.com/canvace/grunt-appcache with headcomment support and other changes. For further informations about HTML 5 appcache please refer to HTML 5 documentation.

## Requirements
This plugin requires Grunt `~0.4.1`

If you haven't used [Grunt](http://gruntjs.com/) before, be sure to check out the [Getting Started](http://gruntjs.com/getting-started) guide, as it explains how to create a [Gruntfile](http://gruntjs.com/sample-gruntfile) as well as install and use Grunt plugins. Once you're familiar with that process, you may install this plugin with this command:

###Installation
```shell
npm install grunt-zabcache --save-dev
```

Once installed, add the following line to your Gruntfile:

```js
grunt.loadNpmTasks('grunt-zabcache');
```

## The "zabcache" task

### Your Gruntfile
In your project's Gruntfile, add a section named `zabcache` into `grunt.initConfig()`.

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

#### basePath
Type: `String`
Default value: `process.cwd()`

The absolute or relative path to the directory to consider as the root of the application for which to generate the cache manifest.

#### options.headcomment
Type: `String`
Default value: ``

Free text to be used as comment in the first line of the appcache.manifest file

#### options.adddate
Type: `Boolean`
Default value: true

adds a timestamp in the first (commented) line of the appcache.manifest file

#### options.addpkgname
Type: `Boolean`
Default value: false

adds the name ofyour project in the first (commented) line of the appcache.manifest file

#### options.addpkgversion
Type: `Boolean`
Default value: false

adds the version ofyour project in the first (commented) line of the appcache.manifest file

#### options.usebowerjson
Type: `Boolean`
Default value: false

grab project name and version from bower.json (default: from package.json)

#### options.indexfile
Type: `String`
Default value: ``
Optional: true

path of your index.html main file. If set, a reference to the manifest will be created

#### options.baseUrl
Type: `String`
Default value: `undefined`

The base URL to prepand to all expanded cache entries.

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
      basePath: 'static',
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

The next example uses the extended syntax to the `cache` parameter and add a custom string in the comment line 

```js
grunt.initConfig({

  zabcache: {
    options: {
      basePath: 'static',
      headcomment: 'Free text here',
      adddate: false
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
The last example shows how to add the name and version of your project in the manifest file 

```js
grunt.initConfig({
pkg: grunt.file.readJSON('package.json'),
  zabcache: {
    options: {
      basePath: 'static',
      adddate: true,
      addpkgname: true,
      addpkgversion: true
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
