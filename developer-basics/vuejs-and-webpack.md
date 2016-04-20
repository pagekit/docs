# Vue.js and Webpack

One of the central aspects that make Pagekit appealing to developers who want to work with modern technologies is that the interface is built using the Vue.js framework.

When looking through examples, you might be confused by the concepts of Vue.js, Webpack, JavaScript modules and the general file structure needed to get started. This article aims to clear things up for you.

## Terminology

**Vue.js** is a Javascript framework to create interactive web interfaces. It takes care of keeping JavaScript objects (your model) and rendered templates (your view) in sync. A great introductory article can be found in the Vue.js documentation. Check out the hands-on [Getting started guide](http://vuejs.org/guide/) and read about the more [high level concepts](http://blog.evanyou.me/2015/10/25/vuejs-re-introduction/) of the framework.

**Vue components** are logical entities of code that contain well defined functionality and typically include a view template plus a script to define the interaction that happens in the component. In Pagekit, many entities that you can add to the system are created as Vue components and registered at the Pagekit system. This includes Widgets, Dashboard Widgets and Link types. Code-wise, they are typically located in `*.vue` files, which combine JavaScript code with an HTML template. These `*.vue` files can then be compiled to pure `*.js` files using Webpack (see below). However, you can also create Vue components in `*.js` files, the template that the component uses is then defined in a different file or given as a simple string. In that case, you do not need Webpack.

**Webpack** is a module bundler that takes your development assets (like Vue components and plain Javascript files) and combines them into so called bundles. This is different from simply concatenating and minifying JavaScript files. Simply put, with webpack bundles only the modules required on the current page are loaded. To learn more about the [motivation behind Webpack](http://webpack.github.io/docs/motivation.html), head to their documentation.

**Note** A common misconception is that you have to use Webpack when developing Pagekit extensions. This is not the case. If it is easier for you to get started, simply create and include single JavaScript files. You can still move to a Webpack based setup later on.

## File structure without Webpack

If you want to use Vue.js without setting up Webpack, there is an example available in the [todo extension](https://github.com/pagekit/example-todo).

1. Create JavaScript file, i.e. `js/todo.js`.
2. Load JavaScript file in template. Make sure to require `vue` so that it is loaded before your script. `<?php $view->script('todo', 'todo:js/todo.js', 'vue') ?>`

## Why use Webpack?

Without Webpack, an example for a simple Vue component could look like this, located in a file called `example.js`:

```
new Vue({
  el: '<div>{{ message }}</div>',
  data: {
    message: 'Hello Pagekit!'
  }
})
```

This solution is perfectly fine if you prefer not to use Webpack. However, using Webpack to compile `*.vue` templates allows for a nice organizational structure of your files. You can define both markup in a readable way and your JavaScript code in a filed called `example.vue`:

```
<template>

	<div>
  		{{ message }}
	</div>

</template>

<script>
	module.exports = {
		data: {
    		message: 'Hello Pagekit!'
  		}
  	}
</script>
```

As you can see, you do not need to define a template as a clunky string. Instead, Webpack converts the readable `*.vue` file and compiles it into a `*.js` file with the template markup converted to an inline string. 

Webpack is a tool that runs on your terminal and then compiles the `*.vue` files to `*.js` files. Pagekit has a default `webpack.config.js` on the root level. When you run `webpack` or `webpack --watch` in the Pagekit folder, it will traverse all themes and extensions in the `packages` subfolders. 

Your package also needs to have its own `webpack.config.js` in your package, for example `packages/pagekit/example/webpack.config.js`. This file defines which `*.vue` files are compiled and what ouput file they should be compiled to.

The following example assumes that your `*.vue` files are located in a subfolder of your package called `packages/pagekit/example/app/components` and that they should be compiled in the output folder `packages/pagekit/example/app/bundle`.

```
module.exports = [

    {
        entry: {
            "example"  : "./app/components/example.vue",
            "example-2": "./app/components/example-2.vue",
            "example-3": "./app/components/example-3.vue"
        },
        output: {
            filename: "./app/bundle/[name].js"
        },
        module: {
            loaders: [
                { test: /\.vue$/, loader: "vue" }
            ]
        }
    }

];
```

Now open the terminal, navigate to the Pagekit root folder and run `webpack` or `webpack --watch` to run Webpack using the configuration you have just added. You will see `*.js` files appearing in the `app/bundle` folder of your package. For example `example.vue` is compiled into a bundle file called `example.js`.

**Note** When you manage your code via Git, we recommend you add the `app/bundle` folder to your `.gitignore` file because it only contains the compiled versions of your `*.vue` components.

To load and use the compiled Vue components, load the compiled bundle file from within your view template, for example from a file called `views/example.php`. You also need to require `vue` via the third parameter, so that your component is loaded only after the script file for Vue itself has been loaded.

```
<?php $view->script('example', 'hello:app/bundle/example', 'vue') ?>
```


## Summary: File structure with Webpack

For a setup including a webpack configuration, you can look at the [hello extension](https://github.com/pagekit/extension-hello).

1. Create js file or Vue component in your extension. Example Javascript file: [settings.js](https://github.com/pagekit/extension-hello/blob/master/app/views/settings.js) and Example Vue component: [link.vue](https://github.com/pagekit/extension-hello/blob/master/app/components/link.vue).
2. Set up the webpack config for your extension. The webpack config defines which JavaScript modules your extension provides, in which output file the resulting bundle should be stored and which dependencies you are using in your module. Example for a [webpack.config.js](https://github.com/pagekit/extension-hello/blob/master/)
3. Run `webpack` in the root directory of your Pagekit installation. The webpack config of Pagekit is setup so that it traverses all packages and reads their webpack configurations as well.
4. Require the generated bundle from your view files. `<?php $view->script('settings', 'hello:app/bundle/settings.js', ['vue', 'jquery']) ?>`

## Create and register Vue components

With the file structure setup, you can now create your own scripts and Vue components. The default case described above talks about assets that you include in your own view files. In some cases however, you create a Vue component that you want to register with the Pagekit system so that it is displayed in certain parts of the Pagekit admin interface. You can do so for the following elements:

* Dashboard widgets
* Site Widgets
* Link types
* Site Tree settings, displayed as tabs when editing a single page
* Modal popup for extension settings

This basically happens in four steps:

1. Write a Vue component, i.e. `app/components/link.vue`. [Source example](https://github.com/pagekit/extension-hello/blob/master/app/components/link.vue)
2. Register the Vue component via JavaScript, i.e. `window.Links.components['hello'] = module.exports;`. [Source example](https://github.com/pagekit/extension-hello/blob/master/app/components/link.vue#L39)
3. Add the Vue component to the webpack bundle, i.e. `"link": "./app/components/link.vue"`. [Source example](https://github.com/pagekit/extension-hello/blob/master/webpack.config.js#L6)
4. Make sure the component's bundle file is loaded in the backend via PHP, i.e. `$scripts->register('hello-link', 'hello:app/bundle/link.js', '~panel-link');` [Source example](https://github.com/pagekit/extension-hello/blob/master/index.php)

**Note** These are the steps needed when making use of Webpack. You can achieve the same without webpack. Simply skip step 3. You can create the Vue component in a plain `*.js` file with the template included as a literal string for the `template` property. Make sure to use the path to that JavaScript file in step 4.
