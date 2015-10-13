# Modules
<p class="uk-article-lead">Pagekit uses *Modules* to set up the application code. The module definition is used to provide bootstrapping, routing and other configuration options. Here you can listen for events, add custom classes and your own controllers.</p>

## Definition: index.php
In order to load and configure a module, Pagekit has a ModuleManager. It will look for an `index.php` file in the root of the module's directory and expect it to return a PHP array. Think of this array as the bootstrap for the modules code.

By setting the right properties in this array, you tell Pagekit everything it needs to know about your module.

```php
<?php

/*
 * Return a php array which is the module definition.
 */
return [

    // Required: Unique module name
    'name' => 'hello',

];
```

This minimal example is a valid module definition, although it doesn't do anything except being loaded by Pagekit. A module is only loaded if the package it comes in has been enabled in the admin panel.

**Note:** If you start exploring Pagekit's internal structure, you will see the same module structure in many places, it's a central concept of the Pagekit architecture.

### `main`: Bootstrap code
The `main` function gets called when the module is loaded. The function receives the Pagekit Application Container instance as a parameter.

```php
use Pagekit\Application;

// ...

'main' => function (Application $app) {
    // bootstrap code
}
```

You can also load an module class. Its namespace has to be loaded in order for this to work (see `autoload` property). Also the class needs to implement the `Pagekit\Module\ModuleInterface`.

```php
'main' => 'MyNamespace\\MyExtension',
```

### `autoload`: Register custom namespaces
Pass a list of namespaces and paths to be auto loaded by Pagekit. The contained classes will be available via autoloading (`use Pagekit\Hello\HelloExtension`). The path is relative to the module's path.

```php
'autoload' => [

    'Pagekit\\Hello\\' => 'src'

]
```

### `routes`: Mount controllers
Use the `routes` property to mount controllers to a route. Learn more about [Routing and Controllers](routing.md).

```php
'routes' => [

    '/hello' => [
        'name' => '@hello/admin',
        'controller' => [
            'Pagekit\\Hello\\Controller\\HelloController'
        ]
    ]

]
```

### `permissions`: Define permissions
Your module can define permissions. These will be managed in the Pagekit User & Permissions area. You can protect your routes with these permissions or prevent users from performing unauthorized actions.

```php
'permissions' => [

    'hello: manage settings' => [
        'title' => 'Manage settings'
    ]

]
```

### `resources`: Register resource shorthands
You can register prefixes to be used as shorter versions when working with paths. For example use `views:admin/settings.php` to reference `packages/VENDOR/PACKAGE/views/admin/settings.php`. Pagekit registers a few paths for extensions and themes by default already.

This works whenever the Pagekit filesystem is used (i.e. when generating the url for a file path or rendering a view from a controller).

```php
'resources' => [

    'views:' => 'views'

],
```

### `events`: Listen to events from Pagekit or other modules
Events are triggered at several points in the Pagekit core and potentially by other modules. An event always has a unique name that identifies it. You can register callback functions to any event.

For more information on the Event system, check out the [Events section](../developer-basics/architecture-events.md)

```php
'events' => [

    'view.scripts' => function ($event, $scripts) {
        $scripts->register('hello-settings', 'hello:app/bundle/settings.js', '~extensions');
    }

]
```

### `config`: Default module configuration
These are the module's default configuration values.

```php
'config' => [
    'default' => 'World'
],
```

#### Read config
To read the config of a module, you can access the `config` property of the module instance. This config is the result of both the default config stored inside the `index.php` and changes that are stored in the database.

```php
$config = $app->module('hello')->config;
```

#### Write config
To store changes for a module config, use the `config()` service. These changes will automatically propagate to the database.

```php
// Complete config
$app->config()->set('hello', $config);

// Single Value
$app->config('hello')->set('message', 'Custom message');
```

**Note**. If you directly read the config from the module, it will still have the old value. After the next request, Pagekit will have merged the changes and made them available as the `config` property of the `$module` instance.

### `nodes`: Register Nodes for the Site Tree
Nodes are similar to routes with the main difference that they can be dragged around in the Site Tree View and therefore dynamically result in a calculated route.

When you have added a Node, it will be available in the Site Tree. Click the _Add Page_ button to see the Dropdown of all available Node types.

For more information on nodes, check out the [Routing section](../developer-basics/routing.md

```php
'nodes' => [

    'hello' => [

        // The name of the node route
        'name' => '@hello',

        // Label to display in the admin panel
        'label' => 'Hello',

        // The controller for this node. Each controller action will be mounted
        'controller' => 'Pagekit\\Hello\\Controller\\SiteController'
    ]

]
```

### `menu`: Add menu items to the admin panel
You can add menu items to the admin panel's main navigation. These can link to any registered route and be limited to certain access permissions. The `access` property determines if the menu item is visible or not.

```php
'menu' => [

        // name, can be used for menu hierarchy
        'hello' => [

            // Label to display
            'label' => 'Hello',

            // Icon to display
            'icon' => 'hello:icon.svg',

            // URL this menu item links to
            'url' => '@hello/admin',

            // Optional: Expression to check if menu item is active on current url
            // 'active' => '@hello*'

            // Optional: Limit access to roles which have specific permission assigned
            // 'access' => 'hello: manage hellos'
        ],

        'hello: panel' => [

            // Parent menu item, makes this appear on 2nd level
            'parent' => 'hello',

            // See above
            'label' => 'Hello',
            'icon' => 'hello:icon.svg',
            'url' => '@hello/admin'
            // 'access' => 'hello: manage hellos'
        ]

    ],
```

### `settings`: Link to a settings screen
Link to a route that renders your settings screen. Setting this property makes Pagekit render a _Settings_ button next to your theme or extension in the admin panel listing.

```php
'settings' => '@hello/admin/settings',
```

### `widgets`: Register Widgets
A Widget is also a module. With the `widgets` property you can register all widget module definition files. Each of those files is expected to return a PHP array in the form of a valid module definition. Learn more about [Widgets](widgets.md).

```php
'widgets' => [

    'widgets/form.php'

],
```

## Module config

```
TODO
```
