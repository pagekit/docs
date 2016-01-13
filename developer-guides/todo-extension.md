
# How to develop a complete extension

<p class="uk-article-lead">This guide follows all steps needed to setup and develop a full extension to manage Todo items in the admin area of Pagekit. You will learn about basic extension concepts, controllers, routing, view rendering and the Vue.js framework.</p>

## Content

<ol>
<li><a href="#step-1">Extending Pagekit using Modules</a></li>
<li><a href="#step-2">Routing and Controller</a></li>
<li><a href="#step-3">View rendering and module config</a></li>
<li><a href="#step-4">Using Vue.js in a Pagekit extension</a></li>
<li><a href="#complete">The completed example</a></li>
</ol>

If you prefer video instead of text, check out the [Youtube playlist with all screencasts](https://www.youtube.com/playlist?list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto) included in this guide.

**Note** The [completed example](https://github.com/pagekit/example-todo) is available on Github.

---

## <span id="step-1">Step 1: Extending Pagekit using Modules</span>

<p class="uk-article-lead">As a developer, you can easily extend what Pagekit already offers. Whether you want a custom theme or an extension for additional functionality, both are built following the same approach. In this first step we will introduce what a *package* and what a *module* is - both central concepts of Pagekit.</p>

<a href="https://www.youtube.com/watch?v=m6ntYDOCG4s&index=1&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto" class="uk-button">Watch this step as a video</a>

### Terminology: Packages and modules

Themes and extension might seem different from the outside, but they are both something we call a *package*. A package is just a folder that bundles a number of files and contains meta data in a file called `composer.json`.

To register anything with the Pagekit system, you also include a file called `index.php`. It contains the definition for something we call a *module*. A module is the most important building unit in the Pagekit code. Modules are self contained units that bundle functionality and are combined to form the whole system. Modules are used both in the Pagekit core and in any third party extensions.

### Where to put your package files

Every package belongs to a specific vendor name, which becomes clear when you look at the directory structure of a fresh Pagekit installation.

Core packages are part of the `pagekit` namespace and located in the appropriate subdirectory inside `/packages`. Any third party packages (yours!) will have their own vendor name and therefore sit in a separate subfolder.

```
/packages                 
    /pagekit
        /blog
        /theme-one
    /your-vendor
        /your-theme
        /your-extension
```

In every package you will find at least 2 files: `composer.json` and `index.php`.

### 1. composer.json: The package meta data

The `composer.json` contains meta data for the package. This displays in the Pagekit backend and is used if you upload your extension to the Pagekit marketplace.

`packages/pagekit/todo/composer.json`

```
{
    "name": "pagekit/todo",
    "version": "0.1",
    "type": "pagekit-extension",
    "title": "ToDo management"
}
```

### 2. index.php: The module definition

Modules bundle functionality inside Pagekit and are the way for you to hook into the Pagekit system. From a code perspective, the module definition is a PHP array that has certain properties set. The `index.php` always has to `return` a valid array.

```
<?php

use Pagekit\Application;

// packages/pagekit/todo/index.php

return [

    'name' => 'todo',

    'type' => 'extension',

	// called when Pagekit initializes the module
    'main' => function (Application $app) {
        echo "It's alive";
    }
];

?>
```

In order to test the functionality, make sure you enable the extension in the backend. When it is active you will see the printed output at the top of your screen.

This minimal example shows how small a fully functional module can be. It has access to the `Application` instance.  With this object you can access all services, trigger events and listen to events triggered by other modules.

---

## <span id="step-2">Step 2: Routing and Controller</span>

<p class="uk-article-lead">With the basic structure of an extension set up, a common task is to register your own controllers and add menu items to the admin area. For that, we will look at some additional properties that you can add to the module definition in your `index.php`.</p>

<a href="https://www.youtube.com/watch?v=Hi7WGVSI3aw&index=2&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto" class="uk-button">Watch this step as a video</a>

### Adding a controller

A controller in Pagekit is just a plain PHP class. Every public method named properly (i.e. `someAction()`) will be mounted by Pagekit's routing (i.e. `/todo/some`).

Create a file `packages/pagekit/todo/src/Controller/TodoController.php` with the following code.

```
<?php

namespace Pagekit\Todo\Controller;

/**
 * @Access(admin=true)
 */
class TodoController
{

    public function indexAction()
    {
       return "Yay.";    
    }

}
```

To limit the access to the admin area and mount this controller to an admin url, we use the annotation `@Access(admin=true)`. Annotations are keywords which are placed in the comment block above a class or method. Learn [more about annotations](http://pagekit.com/docs/developer-basics/routing#annotations).

To use this controller, we need to autoload our namespace and mount the controller to a route. Add the following properties to the configuration array in your `index.php`.

```
	// ...

	// array of namespaces to autoload from given folders
	'autoload' => [
        'Pagekit\\Example\\' => 'src'
    ],


	// array of routes
    'routes' => [

    	// identifier to reference the route from your code
        '@todo' => [

            // which path this extension should be mounted to
            'path' => '/todo',

            // which controller to mount
            'controller' => 'Pagekit\\Todo\\Controller\\TodoController'
        ]
    ],  

    // ...
```

You are now able to access the new controller at the url `<pagekit_path>/admin/todo`. Note how this URL is generated from four parts:

1. `<pagekit_path>` URLs always start from your url
1. `@Access(admin=true)` resulted in an admin url `/admin`
2. The controller is mounted as `/todo`
3. The `indexAction` is the default route at that url.

If you cannot access your route, [clear the cache](http://pagekit.com/docs/user-interface/system#cache). Pagekit caches routes for better performance.

### Debug toolbar

To see all registered routes during development, it is useful to enable the *Debug Toolbar* in Pagekit's system settings. The toolbar appears at the bottom of your screen and shows all registered routes, amongst other useful information.

Remember to turn this off for a live website.

### Adding a menu item

To add menu items use the `menu` property in your module definition. Add the following to the `index.php`.

```
// ...

'menu' => [
    'example' => [
        'label'  => 'ToDo',
        'icon'   => 'app/system/assets/images/placeholder-icon.svg',
        'url'    => '@todo',
    ]
],
// ...
```

Refresh the Pagekit backend and you will see a new menu item which links to the `@todo` route.

---

## <span id="step-3">Step 3: View rendering and module config</span>

<p class="uk-article-lead">In the past steps, we have looked at the basics of modules and routing. However, our first controller only returned simple strings. In this step, let us look at actual view rendering.</p>

<a href="https://www.youtube.com/watch?v=EwCAtxcsz18&index=3&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto" class="uk-button">Watch this step as a video</a>

### Render a view from a controller action

To pass parameters to the view renderer, a controller action returns a PHP array. The reserved key `$view` holds parameters for the view renderer, i.e. the `name` of the view file to render.

```
public function indexAction()
{       
    return [   
    	'$view' => [
            'title' => 'TODO management',
            'name' => 'todo:views/admin/index.php'
        ],
        'message' => 'Hello how's it going?'
    ];
}
```

The rendered view file can be a plain PHP template. Simple parameters (i.e. `message`) are available as PHP variables (i.e. `$message`).

`packages/pagekit/todo/views/admin/index.php`:

```
<h1><?php echo $message; ?></h1>
```


### Resource shorthands

Instead of referencing the full path to a file, we can use a shorthand syntax. `packages/pagekit/todo/views/admin/index.php` turns into `todo:views/admin/index.php`. This is faster to type and nicer to read.

You can register shorthands with a target path in your `index.php`. In this example we want to point `todo:` to the current path of the `index.php`.

```
'resources' => [
    'todo:' => ''
],
```

### Module config

A module can come with a configuration array to hold any kind of settings. We can use this as a simple storage for our TODO items.

```
'config' => [
    'entries' => [
        ['message' => 'Buy milk.', 'done' => false], // ...
    ]
],
```

From the controller, we can access this configuration as a property of the module instance.

`src/Controller/TodoController.php`:

```
use Pagekit\Application

// ...

	$module = App::module('todo');
	$config = $module->config;

// ...
```

We can store changes to the module config in the database. The changes from the default module config are merged with the stored changes so that we always have a valid configuration available in the controller.

```
// modifying the module config
App::config('todo')->set('entries', $entries);
```

---

## <span id="step-4">Step 4: Using Vue.js in a Pagekit extension</span>

<p class="uk-article-lead">When building your own screens for the admin area, you can use any library you are used to. But as Pagekit comes with Vue.js included, it makes sense to look into it and see if it's the right choice for you as well. In this step, we will be introducing the basic concepts of working with Vue.js inside Pagekit.</p>

<a href="https://www.youtube.com/watch?v=3gPGyhTroSA&list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto&index=4" class="uk-button">Watch this step as a video</a>

### 1. Passing data to JavaScript

In the past posts we have seen how data can be accessed in PHP controllers. To use this data in JavaScript, use the `$data` keyword to pass PHP arrays to the view renderer. Pagekit will automatically convert this to a JSON representation and render it in the head section of the generated HTML.


```
// packages/pagekit/todo/src/Controller/TodoController.php

// ...

public function indexAction()
{
    $module = App::module('todo');

    return [
        '$view' => [
            'name' => 'todo:views/admin/index.php'
        ],
        '$data' => $module->config
    ];
}
```

This will render the following in your `<head>` section.

```
<script>var $data = {"entries": [{"message":"Buy milk","done":false},{"message":"Drink coffee","done":true}] };</script>
```


### 2. Combine view and model

The ViewModel is a `Vue` instance that syncs the data from your model with the interface of your view. This is called *reactivity* and is one of the key features of Vue. Used correctly, this helps to keep your JavaScript components small and readable.

You can attach the `Vue` instance to a DOM element (`el: '#todo'`). The model will be whatever you pass to the `data` parameter. To use data from Pagekit, take the global `window.$data` object that your view has rendered.

Any `methods` you create can be called from your template. We will create the template markup in the next step.

```

// packages/pagekit/todo/js/todo.js

$(function(){

    var vm = new Vue({

        el: '#todo',

        data: {
            entries: window.$data.config.entries,
        },

        methods: {

            add: function(e) {
                // ...
            },

            toggle: function(entry) {
                entry.done = !entry.done;
            },

            remove: function(entry) {
                this.entries.$remove(entry);
            },

            save: function() {
                // ...
            }

        }

    });

});
```

### 2. Markup for Vue

From PHP, we still render the view file `views/admin/index.php`. In here, we include the required JavaScript files. To make Vue available in your script, add a dependency to `vue` as a third parameter.

```
<?php $view->script('todo', 'todo:js/todo.js', 'vue') ?>
```

In your HTML, include the DOM element that the Vue instance is looking for (`<div id="todo"></div>`). Inside the element, you can use Vue directives. Directives are certain keywords that tell Vue what to do with the element.

```
<p v-if="entry.done">This will be displayed if the item has been done.</p>
```

With `@click` you can bind to the click event and call methods of your view model.

To output values from your model, you can use curly braces (`{{ entry.message }}`). Pagekit provides a `trans` filter which will replace the string with a translated alternative if there is one present for the selected locale.

```
{{ 'Save' | trans }}
```

This is how a simple view might look like.

```
<!-- packages/pagekit/todo/views/admin/index.php -->

<?php $view->script('todo', 'todo:js/todo.js', 'vue') ?>

<div id="todo">

    <button @click="save()">{{ 'Save' | trans }}</button>

    <ul>
        <li v-for="entry in entries">
            {{ entry.message }}

            <button @click="click: toggle(entry)">{{ entry.done ? 'Undo' : 'Do' }}</button>
        </li>
    </ul>

</div>
```

### Learn more about Vue

Vue.js is a powerful library that makes it easy to build reactive web interfaces. To learn more, checkout the [official guide](http://vuejs.org/guide/). We can also highly recommend the Laracasts [videos on Vue.js](https://laracasts.com/series/learning-vue-step-by-step). Both are great places for an overview of what Vue can do, either in written form or as videos.

<a id="complete"></a>
## The completed example

In the completed example, we have implemented the actual saving to the backend and cleaned up the source code. It combines all previous steps where we described how to create a simple Todo extension. The result is [available on Github](https://github.com/pagekit/example-todo) for you to fiddle around with.
