# Vue.js and Webpack

One of the central aspects that make Pagekit appealing to developers who want to work with modern technologies is that the interface is built using the Vue.js framework.

When looking through examples, you might be confused by the concepts of Vue.js, Webpack, JavaScript modules and the general file structure needed to get started. This article aims to clear things up for you.

## Terminology

**Vue.js** is a Javascript framework to create interactive web interfaces. It takes care of keeping JavaScript objects (your model) and rendered templates (your view) in sync. A great introductory article can be found in the Vue.js documentation. Check out the hands-on [Getting started guide](http://vuejs.org/guide/) and read about the more [high level concepts](http://blog.evanyou.me/2015/10/25/vuejs-re-introduction/) of the framework.

**Vue components** are logical entities of code that contain well defined functionality and typically include a view template plus a script to define the interaction that happens in the component. Code-wise, they are located in `*.vue` files. In Pagekit, many entities that you can add to the system are created as Vue components and registered at the Pagekit system. This includes Widgets, Dashboard Widgets and Link types.

**Webpack** is a module bundler that takes your development assets (like Vue components and plain Javascript files) and combines them into so called bundles. This is different from simply concatenating and minifying JavaScript files. Simply put, with webpack bundles only the modules required on the current page are loaded. To learn more about the [motivation behind Webpack](http://webpack.github.io/docs/motivation.html), head to their documentation.

**Note** A common misconception is that you have to use Webpack when developing Pagekit extensions. This is not the case. If it is easier for you to get started, simply create and include single JavaScript files. You can still move to a Webpack based setup later on.

## File structure without Webpack

If you want to use Vue.js without setting up Webpack, there is an example available in the [todo extension](https://github.com/pagekit/example-todo).

1. Create JavaScript file, i.e. `js/todo.js`.
2. Load JavaScript file in template. Make sure to require `vue` so that it is loaded before your script. `<?php $view->script('todo', 'todo:js/todo.js', 'vue') ?>`

Limitations of this approach: One request per JavaScript file that is required by the browser.

## File structure with Webpack

For a setup including a webpack configuration, you can look at the [hello extension](https://github.com/pagekit/extension-hello).

1. Create js file or Vue component in your extension. Example Javascript file: [settings.js](https://github.com/pagekit/extension-hello/blob/master/app/views/settings.js) and Example Vue component: [link.vue](https://github.com/pagekit/extension-hello/blob/master/app/components/link.vue).
2. Set up the webpack config for your extension. The webpack config defines which JavaScript modules your extension provides, in which output file the resulting bundle should be stored and which dependencies you are using in your module. Example for a [webpack.config.js](https://github.com/pagekit/extension-hello/blob/master/).webpack.config.js
3. Run `webpack` in the root directory of your Pagekti installation. The webpack config of Pagekit is setup to traverse all packages and use their webpack
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

**Note** These are the steps needed when making use of Webpack. You can achieve the same without webpack. Simply skip setp 3. You can create the Vue component in a plain `*.js` file with the template included as a literal string for the `template` property. Make sure to use the path to that JavaScript file in step 4.
