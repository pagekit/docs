# Create an Extension

<p class="uk-article-lead">Create an extension to add features of your own to Pagekit.</p>

The *Hello Extension* in the marketplace is a collection of examples and best-practices for extension development. The examples in this guide can all be found in the Hello Extension.

## Basic structure

No matter what you want to do with an extension, it always starts with the same simple structure. To get started, let's build a simple extension called *Hello*. It will be located inside `/packages/pagekit/hello`. *pagekit* is the vendor prefix and *hello* the name of your extension.

**Note** You can install the complete *Hello extension* from the marketplace.

Your extension is contained in what Pagekit calls a *Package*. A Package is defined by a `composer.json` which contains meta data.

To add any functionality to Pagekit, you create a custom module which in the simplest case is just a PHP file called `index.php`.


| File                         | Needed?  | Description             |
|------------------------------|----------|-------------------------|
| `composer.json`              | required | Package metadata        |
| `index.php`                  | required | Theme module definition |


## Package definition: composer.json

The `composer.json` contains all information on your extension package. This is needed when distributing the extension via the [marketplace](../user-interface/marketplace.md), but it's also important for how the extension is listed in the administration panel.

```json
{
    "name": "hello",
    "version": "0.8.0",
    "type": "pagekit-extension",
    "title": "Hello"
}
```

The `composer.json` acts both as a description of your Extension package and for Composer. Composer is used in the background when Pagekit installs Packages. Find more details in the [Packages section](../developer-basics/packages.md).

## Module definition: index.php

Internally, most things in Pagekit are so called *Modules*. A module has a name and other properties. When you create an extension, you define your own Module that is loaded by Pagekit.

The `index.php` is the starting point for any custom code as it contains your extension's main module definition. You can later add more modules, custom classes and your own controllers, but all of this happens from this central file.

Make sure this file returns a PHP array. By setting the right properties in this array, you tell Pagekit everything it needs to know about your module.

```php
<?php

use Pagekit\Application;

/*
 * This array is the module definition.
 */
return [

    // unique module name
    'name' => 'hello',

    // extensions are special modules (i.e. can be installed)
    'type' => 'extension',

    // main point to register custom services and access existing ones
    'main' => function (Application $app) {

        // bootstrap code

    },

    // Autoload namespaces from given paths
    'autoload' => [

        'Pagekit\\Hello\\' => 'src'

    ],

    // Default module configuration
    'config' => []

];
```

## Enable extension in administration panel

When you have created your files, you need to enable the extension in the administration panel. To do so, navigate to *System / Settings / Extensions* and click the status icon next to your extension. When your extension is disabled, the status icon is red. When your extension is enabled, it is green.

Internally, Pagekit changes a setting in the database to enable your extension. In the database table `pk_system_config` you can find a config setting with the `name`: `system`. In here, the system settings are stored as a JSON representation. The list of active extensions is stored as a property.

```
{
    // ...
    extensions":["blog", "hello"],
    // ...
}
```

## Add a controller

Create a controller class `src/Controller/HelloController.php`.

```
<?php

namespace Pagekit\Hello;

class HelloController
{
    public function indexAction()
    {
        return "Hello";
    }

}
```


The controller class has to be part of the namespaces you set via the `autoload` property in the `index.php`.

```
// Autoload namespaces from given paths
'autoload' => [

    'Pagekit\\Hello\\' => 'src'

],
```

To mount the controller, you can define your own routes in the `index.php`:

```
'routes' => [

    '/hello' => [
        'name' => '@hello',
        'controller' => [
            'Pagekit\\Hello\\Controller\\HelloController'
        ]
    ]

],
```

You can read more about [Controllers and Routing](../developer-basics/controller.md).

## What you can do with your extension

With your basic extension up and running, it's time to explore what you can do with it. There are plentiful ways to extend the Pagekit system.

The important thing to understand is the central module definition in your extension's `index.php`. To hook into Pagekit's workflow you probably just have to set the right property in the array configuration.

Here are a few ideas to get you started:

- [Add a menu item](../developer-basics/packages.md#menu) to the back-end's main navigation.
- [Add a node](../developer-basics/packages.md#nodes) to the Site Tree.
- [Create a Widget](../developer-basics/widgets.md) for the front-end or back-end
- [Define a link type](../developer-basics/links.md) for Pagekit's Link picker
- [Store simple data](../developer-basics/module-config.md) using the module config
