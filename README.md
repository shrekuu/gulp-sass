[![Build Status](https://travis-ci.org/dlmanning/gulp-sass.svg?branch=master)](https://travis-ci.org/dlmanning/gulp-sass)


#Attention: Read this before posting an issue

At the moment gulp-sass will not work with node 0.12 or io.js. gulp-sass is just a wrapper around node-sass, which implements node bindings to libsass. The maintainers of node-sass are doing their best to finish version 2.0, which will include support for node 0.12 and io.js. In the meantime, there is nothing I can do to make gulp-sass work on on anything other that node 0.10.xx. If you need to run gulp-sass, don't upgrade node until node-sass 2.0 is finalized.

Thanks

gulp-sass
=========

Sass plugin for [gulp](https://github.com/gulpjs/gulp).

#Install

```
npm install gulp-sass
```

#Basic Usage

Something like this:

```javascript
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function () {
	gulp.src('./scss/*.scss')
		.pipe(sass())
		.pipe(gulp.dest('./css'));
});
```

Options passed as a hash into `sass()` will be passed along to [`node-sass`](https://github.com/sass/node-sass).

If you want to use the indented syntax (`.sass`) as the top level file, use `sass({indentedSyntax: true})`.

## gulp-sass specific options

#### `errLogToConsole: true`

If you pass `errLogToConsole: true` into the options hash, sass errors will be logged to the console instead of generating a `gutil.PluginError` object. Use this option with `gulp.watch` to keep gulp from stopping every time you mess up your sass.

#### `onSuccess: callback`

Pass in your own callback to be called upon successful compilation by node-sass. The callback has the form `callback(css)`, and is passed the compiled css as a string. Note: This *does not* prevent gulp-sass's default behavior of writing the output css file.

#### `onError: callback`

Pass in your own callback to be called upon a sass error from node-sass. The callback has the form `callback(err)`, where err is the error string generated by libsass. Note: this *does* prevent an actual `gulpPluginError` object from being created.

#### `sync: true`

If you pass `sync: true` into the options hash, sass.renderSync will be called, instead of sass.render. This should help when memory and/or cpu usage is getting very high when rendering many and/or big files.

## Source Maps

gulp-sass can be used in tandem with [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) to generate source maps for the SASS to CSS compilation. You will need to initialize [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) prior to running the gulp-sass compiler and write the source maps after.

```javascript
var sourcemaps = require('gulp-sourcemaps');

gulp.src('./scss/*.scss')
  .pipe(sourcemaps.init())
    .pipe(sass())
  .pipe(sourcemaps.write())
  .pipe(gulp.dest('./css');

// will write the source maps inline in the compiled CSS files
```

By default, [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps) writes the source maps inline in the compiled CSS files. To write them to a separate file, specify a relative file path in the `sourcemaps.write()` function.

```javascript
var sourcemaps = require('gulp-sourcemaps');

gulp.src('./scss/*.scss')
  .pipe(sourcemaps.init())
    .pipe(sass())
  .pipe(sourcemaps.write('./maps'))
  .pipe(gulp.dest('./css'));

// will write the source maps to ./dest/css/maps
```

#Imports and Partials

gulp-sass now automatically passes along the directory of every scss file it parses as an include path for node-sass. This means that as long as you specify your includes relative to path of your scss file, everything will just work.

scss/includes/_settings.scss:

```scss
$blue: #3bbfce;
$margin: 16px;
```

scss/style.scss:

```scss
@import "includes/settings";

.content-navigation {
  border-color: $blue;
  color:
    darken($blue, 9%);
}

.border {
  padding: $margin / 2;
  margin: $margin / 2;
  border-color: $blue;
}
```

#Issues

Before submitting an issue, please understand that gulp-sass is only a wrapper for [node-sass](https://github.com/sass/node-sass), which in turn is a node front end for [libsass](https://github.com/sass/libsass). Missing sass features and errors should not be reported here.
