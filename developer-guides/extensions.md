# Create an Extension
<p class="uk-article-lead">Create an extension to add features of your own to Pagekit.</p>

The _Hello Extension_ in the marketplace is a collection of examples and best-practices for extension development. The examples in this guide can all be found in the Hello Extension.

## Basic structure
No matter what you want to do with an extension, it always starts with the same simple structure. To get started, let's build a simple extension called _Hello_. It will be located inside `/packages/pagekit/hello`. _pagekit_ is the vendor prefix and _hello_ the name of your extension.

**Note** You can install the complete _Hello extension_ from the marketplace.

Your extension is contained in what Pagekit calls a _Package_. A Package is defined by a `composer.json` which contains meta data.

To add any functionality to Pagekit, you create a custom module which in the simplest case is just a PHP file called `index.php`.

File            | Needed?  | Description
--------------- | -------- | -----------------------
`composer.json` | required | Package metadata
`index.php`     | required | Theme module definition

## Package definition: composer.json
The `composer.json` contains all information on your extension package. This is needed when distributing the extension via the [marketplace](../user-interface/marketplace.md), but it's also important for how the extension is listed in the admin panel.

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
Internally, most things in Pagekit are so called _Modules_. A module has a name and other properties. When you create an extension, you define your own Module that is loaded by Pagekit.

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

## Enable extension in admin panel
When you have created your files, you need to enable the extension in the admin panel. To do so, navigate to _System / Settings / Extensions_ and click the status icon next to your extension. When your extension is disabled, the status icon is red. When your extension is enabled, it is green.

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

## Links
Pagekit's concept of links allows for a reusable link picker (for example when linking from a menu item or when linking from the markdown editor). The user can choose a type of link and get further options depending on that choice. Linking to a page for example will present the user with a list of pages to choose from.

In this section we will explain how custom link types can be registered and how to ask for advanced options from the user.

## Register JS component
In the `events` property of your module's `index.php`, register a JavaScript file that will take care of rendering the Link interface. The second parameter is the parameter of dependencies, the tilde `~` makes sure your script is only loaded then the `panel-link` script is included.

```php
'view.scripts' => function ($event, $scripts) {
    $scripts->register('link-blog', 'hello:/link-hello.js', '~panel-link');
}
```

## JS component for link picker
In the JavaScript file, you can now render the interface.

**Note** This is most comfortable when making use of Vue components, storing them in a single `*.vue` file and bundling them using Webpack. A good example for this can be found in `blog/app/components/link-blog.vue`, the link picker from the Blog extension.

```js
window.Links.components['link-hello'] = {

    link: {
        label: 'Hello'
    },

    props: ['link'],

    template: '<div>Your form markup here.</div>',

    ready: function() {
        this.link = '@hello';
    },

};
```

The Vue component needs to set the 'link' property. This is probably dynamic, depending on the parameters the internal route is made up of.

# Site Tree
<p class="uk-article-lead">Pagekit's Site Tree can be extended by definining your own Node type.</p>

When inside the Site tree, you can add new elements with the _Add Page_ button. A dropdown will appear that shows all available Node types. Once you have created a node, it can be dragged around the Tree of your Site. A node can be seen as a dynamic route, as the actual URL of the node depends on the location of the node inside the Site Tree.

## Register node
To register a new node, use the `nodes` property inside your module's `index.php`:

```php
'nodes' => [

    'hello' => [

        // The name of the node route
        'name' => '@hello',

        // Label to display in the backend
        'label' => 'Hello',

        // The controller for this node. Each controller action will be mounted
        'controller' => 'Pagekit\\Hello\\Controller\\SiteController'
    ]

],
```

## Add configuration tab to node edit screen
When you have added a node or are editing an existing node, Pagekit displays a screen with option for that particular node.

The options include things that come from the Pagekit core (i.e. access management), each located in a single tab. You can register a tab with custom options that you want the suer to configure.

```
TODO
```

Here are a few ideas to get you started:
- [Add a menu item](../developer-basics/packages.md#menu) to the admin panel's main navigation.
- [Add a node](../developer-basics/packages.md#nodes) to the Site Tree.
- [Create a Widget](../developer-basics/widgets.md) for the frontend or admin panel dashboard
- [Define a link type](../developer-basics/links.md) for Pagekit's Link picker
- [Store simple data](../developer-basics/module-config.md) using the module config
