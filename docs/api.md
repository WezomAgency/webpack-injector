# API

:arrow_left: [Documentation](./index.md)

#### *Sections*

- [Methods](#helpers)
    - [app()](#app)
    - [cssLoaderOptions()](#cssloaderoptions)
    - [customRule()](#customRule)
    - [exportConfig()](#exportconfig)
    - [external()](#external)
    - [postcssLoaderPlugin()](#postcssLoaderPlugin)
    - [publicPath()](#publicpath)
    - [plugin()](#plugin)
    - [resolveAlias()](#resolveAlias)
    - [resolveModules()](#resolveModules)
    - [sass()](#sass)
    - [sassLoaderOptions()](#sassloaderoptions)
    - [silent()](#silent)
    - [sourcemaps()](#sourcemaps)
    - [styleLoaderOptions()](#styleloaderoptions)
- [Properties](#properties)
    - [isProduction](#isproduction)
    - [isWatching](#iswatching)
    - [isHot](#ishot)
- [Methods](#methods)
- [Helpers](#helpers)
    - [helpers.clear()](#helpersclear)
    - [helpers.copy()](#helperscopy)




---




## Methods




### app()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.app(sourceFile, distFile): Injector
```

Specify your main single JS file.  
_**Note!** You can have only one file!_

_Parameters:_

| Name | Type | Default | Description |
| :--- | :--- | :------ | :---------- |
| **sourceFile**  | `string` | --- | Relative FS path from yout CWD = source file |
| **distFile**  | `string` | --- | Relative FS path from yout CWD = destinaion file (css result) |

_Method returns:_ `Injector` instance

_Usage example:_

```js
injector.app('./src/app.js', './dist/bundled-app.js');
```




---




### cssLoaderOptions()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.cssLoaderOptions(options): Injector
```

Specify your _css-loader_ options.  
See list of all options [css-loader#options](https://github.com/webpack-contrib/css-loader#options)

_Parameters:_

| Name | Type | Default | Description |
| :--- | :--- | :------ | :---------- |
| **options**  | `Object` | _see description below_ | Relative FS path from yout CWD = source file |

_Default options:_

| Name | Value | Description |
| :--- | :---- | :---------- |
| **sourceMap** | _calculated_  | Use value from [sourcemaps()](#sourcemaps) |
| **importLoader**  | `1` | ---

_Method returns:_ `Injector` instance

_Usage example:_

```js
injector
    .app('./src/app.js', './dist/bundled-app.js')
    .sass('./src/sass/style.scss', './dist/bundled-style.css')
    .cssLoaderOptions({
        sourceMap: true,
        url: false,
        import: false
    });
```




---




### customRule()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.customRule(rule): Injector
```

Specify your own rules for module config.  
See https://webpack.js.org/configuration/module#rule  
Call this method for each rule you need.

> _**Note!** This method for advanced users_

_Parameters:_

| Name | Type | Default | Description |
| :--- | :--- | :------ | :---------- |
| **rule**  | `Object` | --- | https://webpack.js.org/configuration/module/#nested-rules |

_Method returns:_ `Injector` instance

_Usage example:_

```js
injector
    .customRule({
        test: /\.json$/,
        type: 'javascript/auto',
        loader: 'custom-json-loader'
    })
    .customRule({
        test: /\.modernizrrc$/,
        loader: 'modernizr-loader!json5-loader'
    });
```




---




### exportConfig()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.exportConfig(): WebpackOptions
```

_Method returns:_ `Object` (WebpackOptions)  
Use this method for export generated webpack configuration

_Usage example:_

```js
const injector = require('webpack-injector');
// setup...
module.exports = injector.exportConfig();
```




---




### external()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.external(rule): Injector
```

Setting wepback.config option `externals`   
See https://webpack.js.org/configuration/externals/  
Call this method for each module you need.

_Parameters:_

| Name | Type | Default | Description |
| :--- | :--- | :------ | :---------- |
| **module**  | `string|function|Object|RegExp` | --- | external module |

_Method returns:_ `Injector` instance

_Usage example:_

```js
injector
    .external({
        jquery: 'jQuery',
        subtract: ['./math', 'subtract']
    })
    .external(function(context, request, callback) {
        if (/^yourregex$/.test(request)){
            return callback(null, 'commonjs ' + request);
        }
        callback();
    })
    .external(/^(custom_regex|\$)$/i);
```




---




### postcssLoaderPlugin()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.postcssLoaderPlugin(plugin): Injector
```

Specify plugins for [postcss-loader](https://github.com/postcss/postcss-loader).  
Call this method for each plugin you need.

_Parameters:_

| Name | Type | Default | Description |
| :--- | :--- | :------ | :---------- |
| **plugin**  | `*` | --- | https://github.com/postcss/postcss-loader#plugins |

_Method returns:_ `Injector` instance

_Usage example:_

```js
injector
    .postcssLoaderPlugin(
    	require('autoprefixer')({
            browsers: ['> 1%', 'ie 11'],
            cascade: false
        })
    )
    .postcssLoaderPlugin(
    	require('cssnano')({
            preset: [
                'default',
                {
                    zindex: false,
                    autoprefixer: false
                }
            ]
        })
    );
```




---




### publicPath()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.publicPath(path): Injector
```

This option specifies the public URL of the output directory when referenced in a browser.  
See [webpack configuration -> output.publicPath](https://webpack.js.org/configuration/output#outputpublicpath)

_Parameters:_

| Name | Type | Default | Description |
| :--- | :--- | :------ | :---------- |
| **path**  | `string` | --- | public URL |

_Method returns:_ `Injector` instance

_Usage example:_

```js
injector
    .app('./src/app.js', './dist/bundled-app.js')
    .publicPath('./src/app.js', './dist/bundled-app.js');
```




---





### plugin()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.plugin(plugin): Injector
```

Specify webpack plugins for your project.  
Call this method for each plugin you need.

_Parameters:_

| Name | Type | Default | Description |
| :--- | :--- | :------ | :---------- |
| **plugin**  | `*` | --- | https://webpack.js.org/configuration/plugins#plugins |

_Method returns:_ `Injector` instance

Plugins that are already underhood:

- [mini-css-extract-plugin](https://github.com/webpack-contrib/mini-css-extract-plugin)
- [webpack-build-notifier](https://github.com/RoccoC/webpack-build-notifier)
- [browser-sync-webpack-plugin](https://github.com/Va1/browser-sync-webpack-plugin)
- or [browser-sync-dev-hot-webpack-plugin](https://github.com/itgalaxy/browser-sync-dev-hot-webpack-plugin) (_if [isHot](#ishot)_)

_Usage example:_

```js
injector
    .plugin(new webpack.DefinePlugin({
        // Definitions...
    }));
```




---





### silent()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.silent(value): injector
```

Disable/enable information logs in terminal.  
_**Note!** The error logs will not be suppressed_

_Parameters:_

| Name       | Type      | Attributes    | Default       | Description                                    |
| :--------- | :-------- | :------------ | :------------ | :--------------------------------------------- |
| **value**  | _boolean_ |               |               | `true` - disable / `false` - enable logs again |

_Method returns:_ `Injector` instance

_Usage example:_

```js
const injector = require('webpack-injector');
injector.helpers.clear('./public/assets/');
injector.helpers.clear('./my-logs/stats.json');
```




---




## Properties


### isProduction

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.isProduction: boolean
```

_readonly_  
_type: `boolean`_  
_default: `false`_

Determined value - is webpack will be runned in production mode or development mode

```js
if (injector.isProduction) {
    console.log('production mode');
}
```


---



### isWatching

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.isWatching: boolean
```

_readonly_  
_type: `boolean`_  
_default: `false`_

Determined value - is webpack will be watching files or not

```js
if (injector.isWatching) {
    console.log('incremental');
}
```


---



### isHot

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

```js
injector.isHot: boolean
```

_readonly_  
_type: `boolean`_  
_default: `false`_

Determined value - is [Hot Module Replacement (HMR)](https://webpack.js.org/concepts/hot-module-replacement/) usage turn on or not

```js
if (injector.isHot) {
    console.log('HMR turn on!');
}
```




---




## Helpers




### helpers.clear()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

Clear some folders or files, before webpack starts to bundle your project

```js
injector.helpers.clear(paths): void
```

_Parameters:_

| Name       | Type              | Attributes    | Default       | Description               |
| :--------- | :---------------- | :------------ | :------------ | :------------------------ |
| **paths**  | _string/string[]_ |               |               | paths that need to remove |


_Returns:_ `undefined`

_Examples:_

```js
const injector = require('webpack-injector');

injector.helpers.clear('./public/assets/');
injector.helpers.clear('./my-logs/stats.json');
```




---




### helpers.copy()

:arrow_left: [Documentation](./index.md) | :arrow_up: [Top](#readme)

Copy file from `sourceFile` to `distFile`.  
This can be useful when need to copy files from directories 
that are not included in the main distribution of the project or repository.

```js
injector.helpers.copy(sourceFile, distFile[, onlyIfNewer]): void
```

_Parameters:_

| Name            | Type       | Attributes    | Default       | Description                                    |
| :-------------- | :--------- | :------------ | :------------ | :--------------------------------------------- |
| **sourceFile**  | _string_   |               |               | relative path from your CWD to the source file |
| **distFile**    | _string_   |               |               | relative path from your CWD to the destination file. **Note**, if folders path does not exist - it will be created |
| **onlyIfNewer** | _boolean_  | `<optional>`  | `false`       | copy only if source file is newer than destination file |


_Returns:_ `undefined`

_Examples:_

```js
const injector = require('webpack-injector');

injector.helpers.copy('./node_modules/jquery/dist/jquery.min.js', './public/assets/js/vendors/jquery.js', true);
injector.helpers.copy('./node_modules/webpack/readme.md', './dist/TEST.md', true);
```




---


