# Import Once
This is a fork of [node-sass-import-once](https://github.com/at-import/node-sass-import-once) library.

## Difference with the original library:
- Added ability to load dependencies from a node_modules directory (via ~ in path), so you can use it with webpack. Thanx @AndreyUtka
- Also, you have an ability to load dependencies from a custom node_modules directory via a `createImporterWithCustomNodeModules` function

## Usage

```javascript
var sass = require('node-sass');
    importOnce = require('node-sass-import-once');

// general usage
sass.render({
  file: scss_filename,
  importer: importOnce,
  importOnce: {
    index: false,
    css: false,
    bower: false
  }
});

// usage with a custom node_modules directory
var customNodeModulesPath = "/my/custom/path/to/node_modules";
sass.render({
  file: scss_filename,
  importer: importOnce.createImporterWithCustomNodeModules(customNodeModulesPath),
  importOnce: {
    index: false,
    css: false,
    bower: false
  }
});
```

If you are using Gulp or Grunt to compile (or similiar), require `node-sass-import-once` as normal and pass it as the `importer` option in to your task.

## Features

Node Sass Import Once will ensure that each file import is only imported once. In addition to that, the following options are available:

* **index** - Allows folders to contain an index file (`index.scss`, `index.sass`, `_index.scss`, `_index.sass`) that will automatically be imported if just the folder name is imported. For instance, `@import 'partials';` will try and import `(_)partials.s(c|a)ss` first, then `partials/(_)index.s(c|a)ss`.
* **css** - Allows a CSS file to be imported directly into your Sass files. Simply don't include a file extension, and if it's available as a CSS file it'll be imported!
* **bower** - Automatically parses your [Bower](http://bower.io/) directory for the files you're requested. Will look for the file in `bower_components` (or specified directory from `.bowerrc`), `bower_components/{module-name}/stylesheets`, `bower_components/{module-name}-sass/stylesheets`, `bower_components/sass-{module-name}/stylesheets`, `bower_components/{module-name}/sass`, `bower_components/{module-name}-sass/sass`, `bower_components/sass-{module-name}/sass`. `{module-name}` is the first section of your import path, so in `@import 'breakpoint'` **breakpoint** would be your module name and in `@import 'toolkit/kickstart` **toolkit** would be your module name. If you also have **css** enabled, it will look inside of `bower_components/{module-name}/dist` and `bower_components/{module-name}/dist/css`.

If you import a file ending in `.js`, `.json`, `.yml`, or `.yaml`, the importer will attempt to transform your JSON or YAML into a Sass map, with the variable name being the name of the file.
