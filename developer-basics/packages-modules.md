# Packages

<p class="uk-article-lead">A Package is Pagekit's concept for everything that can be installed in your system, most prominently extensions and themes.</p>

## Package location

All of your packages are located in the `/packages` directory, sorted in subdirectories according to their vendor.

Every package belongs to a specific vendor (for example `pagekit` for all official packages, including the blog package and the *Hello* theme and *Hello* extension).

The vendor name is a unique representation of a developer or organization. In the simplest case, it just matches a github username.

The directory name is up to you. The actual name of the package is defined inside the package and not by the directory name. However, it makes sense to give a clear name to the folder.

Although not mandatory, it is a good convention to prefix a package by its type. For example `theme-hello` for the *Hello* theme and `extension-hello` for the *Hello* extension.

## Package content

A package contains at least two files.

1. The `composer.json` contains the metadata for your package and therefore acts as the package definition.
2. The `index.php` is a so called Module definition and is used to add any actual functionality to Pagekit.

The rest of the package content depends on the package's `type`. A `pagekit-theme` will contain other files than a `pagekit-extension`.

To learn more about the actual content of a package, check out the [Theme Guide](../guides/guide-theme.md) or the [Extension Guide](../guidesguide-extension.md).

## Package definition: composer.json

A package is defined by its `composer.json`. This file includes the package name, potential dependencies to be installed by Composer and other information that displays in the Pagekit marketplace.

For a theme, this file can look as follows.

```json
{
    "name": "pagekit/theme-hello",
    "type": "pagekit-theme",
    "version": "0.9.0",
    "title": "Hello",
    "description": "A blueprint to develop your own themes.",
    "license": "MIT",
    "authors": [
        {
            "name": "Pagekit",
            "email": "info@pagekit.com",
            "homepage": "http://pagekit.com"
        }
    ],
    "extra": {
        "image": "image.jpg"
    }
}
```

<a name="module"></a>
## Module definition: index.php

Internally, Pagekit is made up of so called *Modules*. A module has a name, a type - and other optional properties. You define your Module in the `index.php` which is then loaded by Pagekit.

Think of the `index.php` as the starting point for any custom code. It contains your package's main module definition. Here you can add more modules, custom classes and your own controllers. All of this happens in this central file.

Make sure this file returns a PHP array. By setting the right properties in this array, you tell Pagekit everything it needs to know about your module.

```php
<?php

/*
 * Return a php array which is the module definition
 */
return [

    // Required: Unique module name
    'name' => 'hello',

    // Required: type (i.e. theme, extension)
    'type' => 'extension'

];
```

This minimal example is a valid module defintion, although it doesn't do anything except being loaded by Pagekit. A module is only loaded if the package it comes in has been enabled in the administration panel.

**Note:** If you start exploring Pagekit's internal structure, you will see the same module structure in many places, it's a central concept of the Pagekit architecture.

## Module properties

### `main`: Execute custom code

Use the `main` function be called when the module is loaded. The function receives the Pagekit Application Container instance as a parameter.

```php
use Pagekit\Application;

// ...

'main' => function (Application $app) {
    // bootstrap code
},
```

You can also load an extension class. The namespace has to be loaded in order for this to work (see `autoload` property).

```php
'main' => 'MyNamespace\\MyExtension',
```

### `autoload`: Register custom namespaces

Pass a list of namespaces and paths to be loaded by Pagekit. The contained classes will be available via autoloading (`use Pagekit\Hello\HelloExtension`).

```php
'autoload' => [

    'Pagekit\\Hello\\' => 'src'

],
```

<a name="nodes"></a>
### `nodes`: Register Nodes for the Site Tree

Nodes are similar to routes with the main difference that they can be dragged around in the Site Tree View and therefore dynamically result in a calculated route.

When you have added a Node, it will be available in the Site Tree. Click the *Add Page* button to see the Dropdown of all available Node types.

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

],
```

### `routes`: Mount controllers

Use the `routes` property to mount controllers to a route. Learn more about [Controllers and Routing](controller.md).

```php
'routes' => [

    '/hello' => [
        'name' => '@hello/admin',
        'controller' => [
            'Pagekit\\Hello\\Controller\\HelloController'
        ]
    ]

],
```

<a name="menu"></a>
### `menu`: Add menu items to the admin panel

You can add menu items to the admin panel's main navigation. These can link to any registered route and be limited to certain access permissions. The `access` property determines if the menu item is visible or not. The actual checking for user permissions has to be done on the [Controller](controller.md) level.

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

### `permissions`: Define permissions

Your module can define permissions. These will show up in the Pagekit User &amp; Permissions area. You can check for these permission strings in [Controllers](controller.md).

```php
'permissions' => [

    'hello: manage settings' => [
        'title' => 'Manage settings'
    ],

],
```

### `settings`: Link to a settings screen

Link to a route that renders your settings screen. Setting this property makes Pagekit render a *Settings* button next to your theme or extension in the administration panel listing.

```php
'settings' => '@hello/admin/settings',
```

### `config`: Default module configuration

If your theme or extension comes with settings that you want the user to configure, you should define the default configuration values in your module configuration.

```php
'config' => [
    'default' => 'World'
],
```

On runtime, Pagekit will merge the default config and the config options that are currently stored in the database. The merged result will be available as a property of your module.

```php
$module = App::module('hello');
$config = $module->config;
```

You can make use of this to [store any data](basics/module-config.md) that doesn't require its own database structure.

### `events`: Listen to events from Pagekit or other modules

Events are triggered at several points in the Pagekit core and potentially by other extensions. An event always has a unique name that identifies it. You can register callback functions to any event.

For more information on the Event system, check out the [Events section](../developer-basics/architecture-events.md)

```php
'events' => [

    'view.scripts' => function ($event, $scripts) {
        $scripts->register('hello-settings', 'hello:app/bundle/settings.js', '~extensions');
    }

]
```

### `resources`: Register resource shorthands

You can register strings to be used as shorter versions when working with paths. For example use `views:admin/settings.php` to reference `packages/VENDOR/PACKAGE/views/admin/settings.php`.

This works whenever the Pagekit filesystem is used (i.e. when generating the url for a file path).

```php
'resources' => [

    'views:' => 'views'

],
```

### `widgets`: Register Widgets

A Widget is also a module. With the `widgets` property you can register all widget module definition files. Each of those files is expected to return a PHP array in the form of a valid module definition. Learn more about [Widgets](widgets.md).

```php
'widgets' => [

    'widgets/form.php'

],
```
