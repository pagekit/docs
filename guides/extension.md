# Extensions

<p class="uk-article-lead">Create an extension to add features of your own to Pagekit.</p>

The *Hello Extension* in the marketplace as it is a collection of examples and best-practices for extension development. The examples in this guide can all be found in the Hello Extension.

## Basic structure

No matter what what want to do with an extension, it always starts with the same simple structure. To get started, let's build a simple extension called *Hello*. It will be located inside `/packages/pagekit/extension-hello`.

**Note** You can install the complete *Hello extension* from the marketplace.

Your extension is contained in what Pagekit calls a *Package*. A Package is defined by a `composer.json` which contains meta data.

To add any functionality to Pagekit, you create a custom module which in the simplest case is just a PHP file called `index.php`.


| File                         | Needed?  | Description             |
|------------------------------|----------|-------------------------|
| `composer.json`              | required | Package metadata        |
| `index.php`                  | required | Theme module definition |


## Package definition: composer.json

The `composer.json` contains all information on your extension package. This is needed when distributing the extension via the [marketplace](marketplace.md), but is also important for how the extension is listed in the backend.

```json
{
    "name": "hello",
    "version": "0.8.0",
    "type": "pagekit-extension",
    "title": "Hello"
}
```

The `composer.json` acts both as a description of your Extension package but also as a description for Composer. Composer is used in the background when Pagekit installs Packages. Find more details in the [Packages section](../basics/packages.md).

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

## Enable extension in backend

When you have created your files, you need to enable the extension in the backend. To do so, navigate to *System / Settings / Extensions* an click the status icon next to your extension. When your extension is disabled, the status icon is red. When your extension is enabled, it is green.

Internally, Pagekit changes a setting in the database to enable your extension. In the database table `pk_system_config` you can find a config setting with the `name`: `system`. In here, the system settings are stored as a JSON representation. The list of active extensions is stored as a property.

```
{
    // ...
    extensions":["blog", "hello"],
    // ...
}
```

## Add controller

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

You can read more about [Controllers and Routing](../basics/controller.md).

## Add menu item

Add menu items using the `menu` property in your extension's `index.php`.

```
'menu' => [

    'hello' => [

        'label' => 'Hello',
        'icon' => 'hello:icon.svg',
        'url' => '@hello/admin'

    ],

],
```

Read more about the `menu` property in the [Modules chapter](../basics/packages.md).
