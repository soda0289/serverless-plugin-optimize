Serverless Optimize Plugin
=============================
[![serverless](http://public.serverless.com/badges/v3.svg)](http://www.serverless.com) 
[![npm version](https://badge.fury.io/js/serverless-plugin-optimize.svg)](https://badge.fury.io/js/serverless-plugin-optimize)
[![dependencies](https://img.shields.io/david/FidelLimited/serverless-plugin-optimize.svg)](https://www.npmjs.com/package/serverless-plugin-optimize)
[![license](https://img.shields.io/npm/l/serverless-plugin-optimize.svg)](https://raw.githubusercontent.com/FidelLimited/serverless-plugin-optimize/master/LICENSE)

Bundle with Browserify, transpile with Babel to ES5 and minify with Uglify your Serverless functions.

This plugin is a child of the great [serverless-optimizer-plugin](https://github.com/serverless/serverless-optimizer-plugin). Kudos!

**Note:** Requires Serverless *v1.2.x* or higher.

## Setup

 Install via npm in the root of your Serverless service:
```
npm install serverless-plugin-optimize babel-preset-es2015 --save-dev
```

* Add the plugin to the `plugins` array in your Serverless `serverless.yml`:

```yml
plugins:
  - serverless-plugin-optimize
```

* Set your packages to be built individually to have smaller packages:

```yml
package:
  individually: true
```

* All done!

## Options

Configuration options can be set globally in `custom` property and inside each function in `optimize` property. Function options overwrite global options.

#### Global

* **debug** (default `false`) - When debug is set to `true` it won't remove `prefix` folder and will generate debug output at the end of package creation.
* **exclude** (default `['aws-sdk']`) - Array of modules or paths that will be excluded.
* **extensions** (default `['.js', '.json']`) - Array of optional extra extensions modules that will be included.
* **global** (default `false`) - When global is set to `true` transforms will run inside `node_modules`.
* **ignore** - Array of modules or paths that won't be transformed with Babelify and Uglify.
* **includePaths** - Array of file paths that will be included in the bundle package. Read [here](#includepaths-files) how to call these files.
* **minify** (default `true`) - When minify is set to `false` Uglify transform won't run.
* **plugins** - Array of Babel plugins.
* **prefix** (default `_optimize`) - Folder to output bundle.
* **presets** (default `['es2015']`) - Array of Babel presets.

```yml
custom:
  optimize:
    debug: true
    exclude: ['ajv']
  	extensions: ['.extension']
    global: true
    ignore: ['ajv']
    includePaths: ['bin/some-binary-file']
  	minify: false
  	prefix: 'dist'
  	plugins: ['transform-decorators-legacy']
  	presets: ['es2016']
```

#### Function

* **optimize** (default `true`) - When optimize is set to `false` the function won't be optimized.

```yml
functions:
  hello:
    optimize: false
```

* **exclude** - Array of modules or paths that will be excluded.
* **extensions** - Array of optional extra extensions modules that will be included.
* **global** - When global is set to `true` transforms will run inside `node_modules`.
* **ignore** - Array of modules or paths that won't be transformed with Babelify and Uglify.
* **includePaths** - Array of file paths that will be included in the bundle package. Read [here](#includepaths-files) how to call these files.
* **minify** - When minify is set to `false` Uglify transform won't run.
* **plugins** - Array of Babel plugins.
* **presets** - Array of Babel presets.

```yml
functions:
  hello:
    optimize:
      exclude: ['ajv']
      extensions: ['.extension']
      global: false
      ignore: ['ajv']
      includePaths: ['bin/some-binary-file']
      minify: false
      plugins: ['transform-decorators-legacy']
      presets: ['es2016']
```

#### includePaths Files

There is a difference you must know between calling files locally and after optimization with `includePaths`.

When Optimize packages your functions, it bundles them inside `/${prefix}/${functionName}/...` and when your lambda function runs in AWS it will run from root `/var/task/${prefix}/${functionName}/...` and your `CWD` will be `/var/task/`.

Solution in [#32](https://github.com/FidelLimited/serverless-plugin-optimize/issues/32#issuecomment-278432399) by @hlegendre. `path.resolve(process.env.LAMBDA_TASK_ROOT, ${prefix}, process.env.AWS_LAMBDA_FUNCTION_NAME, ${includePathFile})`.

## Contribute

Help us making this plugin better and future proof.

   * Clone the code
   * Install the dependencies with `npm install`
   * Create a feature branch `git checkout -b new_feature`
   * Lint with standard `npm run lint`

## License

This software is released under the MIT license. See [the license file](LICENSE) for more details.
