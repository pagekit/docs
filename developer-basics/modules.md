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


## Available properties

The following table gives an overview of all keys that you can use in the module definition array. The sections below then explain the properties in detail and include code examples. You can directly jump to each section by clicking one of the *Details* links.

Key       | Description                       | More
--------- | --------------------------------- | -----
`main`   | Bootstrap code, executed when module is loaded | [Details](#bootstrap-code)
`autoload` | Register namespaces to be autoloaded | [Details](#register-custom-namespaces)
`routes`    | Mount controllers | [Details](#mount-controllers)
`permissions`  | Define and register permission names | [Details](#define-permissions)
`resources`  | Register resource shorthands | [Details](#register-resource-shorthands)
`events`    | Listen to events from Pagekit or other modules | [Details](#listen-to-events)
`config`    | Default module configuration | [Details](#default-module-configuration)
`nodes`    | Register Nodes for the Site Tree | [Details](#register-nodes-for-site-tree)
`settings`    | Link to a settings screen | [Details](#link-to-a-settings-screen)
`menu` | Add menu items to the admin panel | [Details](#add-menu-items-to-the-admin-panel)
`widgets`    | Register Widgets | [Details](#register-widgets)

## Bootstrap code

To execute any kind of PHP code you can assign a callback function to the `main` property. The function receives the Pagekit Application container instance as a parameter.

```php
use Pagekit\Application;

// ...

'main' => function (Application $app) {
    // bootstrap code
}
```

This function is called when this module is loaded, so on every regular page request. However, the module's `index.php` needs to be located in a valid package and the package needs to be enabled in the admin panel. This means that an extension needs to be installed and also enabled in order for the bootstrap code to be loaded. If you use this inside a theme, only the bootstrap code from the currently activated theme is executed.

Instead of assigning a callback function and having all of your code directly in the `index.php`, you can also create a dedicated module class in a separate file. You then specify the full name of your module class (including the namespace) as the value of the `main` property.

```php
'main' => 'MyNamespace\\MyModule',
```

**Note** The referenced namespace has to be [autoloaded](#register-custom-namespaces) in order for this to work. Make sure the `MyModule` class implements the interface `Pagekit\Module\ModuleInterface`.

## Register custom namespaces

Pass a list of namespaces and paths to be autoloaded by Pagekit. The path is relative to the module's path, so that `src` in the following example is located at `packages/VENDOR/PACKAGE/src`, assuming the module definition is located at `packages/VENDOR/PACKAGE/index.php`.

```php
'autoload' => [

    'Pagekit\\Hello\\' => 'src'

]
```

The classes from the linked directory are then available to referenced in `use` statements across the Pagekit codebase.

```
<?php
use Pagekit\Hello\HelloExtension;
```

## Mount controllers
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

## Define permissions
Your module can define permissions. These can then be assigned to roles in the Pagekit User &amp; Permissions area. 

The unique permission names you define (here `hello: manage settings`) are used as an identifier across the codebase. You can use this identifier to protect your routes from users who do not have this permission and prevent users from performing unauthorized actions.

```php
'permissions' => [

    'hello: manage settings' => [
        'title' => 'Manage settings'
    ]

]
```

A simple way to then protect a controller action is by using an annotation as follows. Read about [annotations](routing.md#annotations) to find out more.

```
<?php

class MyController {
	
	/**
  	* @Access("hello: manage settings")
  	*/
	public function settingsAction() {
	}
}
```

## Register resource shorthands
You can register prefixes to be used as shorter versions when working with paths. For example use `views:admin/settings.php` to reference `packages/VENDOR/PACKAGE/views/admin/settings.php`. Pagekit registers a few paths for extensions and themes by default already.

This works whenever the Pagekit filesystem is used (i.e. when generating the url for a file path or rendering a view from a controller).

```php
'resources' => [

    'views:' => 'views'

],
```

## Listen to events
Events are triggered at several points in the Pagekit core and potentially by other modules. An event always has a unique name that identifies it. You can register callback functions to any event.

For more information on the Event system, check out the [Events section](../developer-basics/architecture-events.md)

```php
'events' => [

    'view.scripts' => function ($event, $scripts) {
        $scripts->register('hello-settings', 'hello:app/bundle/settings.js', '~extensions');
    }

]
```

## Default module configuration
In many cases you want to allow the user to change settings for your module, for example by providing a settings screen. To make sure your module always has some configuration values right from the start, you can provide a default module configuration. 

```php
'config' => [
    'default' => 'World'
],
```

Any changes to this configuration array will later be stored by the database. The default values are then always merged with the values from the database and the merge result will be available as the config property of the module object as you can see in the examples in the next two sections.

### Read config
To read the config of a module, you can access the `config` property of the module instance. This object is the result of both the default config stored inside the `index.php` and changes that are stored in the database.

```php
$config = $app->module('hello')->config;
```

### Write config
To store changes for a module config, use the `config()` service. These changes will automatically propagate to the database.

```php
// Complete config
$app->config()->set('hello', $config);

// Single Value
$app->config('hello')->set('message', 'Custom message');
```

**Note**. If you directly read the config from the module, it will still have the old value. After the next request, Pagekit will have merged the changes and made them available as the `config` property of the `$module` instance.

## Register Nodes for Site Tree
Nodes are similar to routes with the main difference that they can be dragged around in the Site Tree View and therefore dynamically result in a calculated route.

When you have added a Node, it will be available in the Site Tree. Click the _Add Page_ button to see the Dropdown of all available Node types.

For more information on nodes, check out the [Routing section](../developer-basics/routing.md).

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

## Add menu items to the admin panel
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

## Link to a settings screen
Link to a route that renders your settings screen. Setting this property makes Pagekit render a _Settings_ button next to your theme or extension in the admin panel listing.

```php
'settings' => '@hello/admin/settings',
```

## Register Widgets
A Widget is also a module. With the `widgets` property you can register all widget module definition files. Each of those files is expected to return a PHP array in the form of a valid module definition. Learn more about [Widgets](../developer-guides/widgets.md).

```php
'widgets' => [

    'widgets/form.php'

],
```