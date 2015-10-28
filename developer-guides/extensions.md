# Create an Extension
<p class="uk-article-lead">An extension adds features and functionality to Pagekit. It can be a tiny plugin or a full blown application.</p>

**Note** The examples in this guide are taken from the _Hello_ extension. It's available via the Pagekit marketplace. When installed, the _Hello_ extension is located in `/packages/pagekit/extension-hello`.

## Package definition: composer.json
An extension is a regular Pagekit [package](../developer-basics/packages.md) of the type `pagekit-extension`. Each package needs a description in order to be recognized by Pagekit. This description is located in the `composer.json` and looks as follows. Detailed information is available in the [Packages](../developer-basics/packages.md) chapter.

```json
{
    "name": "hello",
    "version": "0.8.0",
    "type": "pagekit-extension",
    "title": "Hello"
}
```

## Module definition: index.php
An extension in itself is simply a [Module](../developer-basics/modules.md). So you may want to read up on modules first.

```php

/*
 * This array is the module definition.
 */
return [

    // unique module name
    'name' => 'hello',

    // main point to register custom services and access existing ones
    'main' => function (Pagekit\Application $app) {

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

You can read more about [Controllers and Routing](../developer-basics/routing.md).

## What you can do with your extension
With your basic extension up and running, it's time to explore what you can do with it. There are plentiful ways to extend the Pagekit system.

The important thing to understand is the central module definition in your extension's `index.php`. To hook into Pagekit's workflow you probably just have to set the right property in the array configuration.

## Links
Pagekit's concept of links allows for a reusable link picker (for example when linking from a menu item or when linking from the markdown editor). The user can choose a type of link and get further options depending on that choice. Linking to a page for example will present the user with a list of pages to choose from.

In this section we will explain how custom link types can be registered and how to ask for advanced options from the user.

### Register JS component
In the `events` property of your module's `index.php`, register a JavaScript file that will take care of rendering the Link interface. The second parameter is the parameter of dependencies, the tilde `~` makes sure your script is only loaded then the `panel-link` script is included.

```php
'view.scripts' => function ($event, $scripts) {
    $scripts->register('link-blog', 'hello:/link-hello.js', '~panel-link');
}
```

### JS component for link picker
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

```
TODO
```

Here are a few ideas to get you started:

- [Add a menu item](../developer-basics/modules.md#menu) to the admin panel's main navigation.
- [Add a node](../developer-basics/modules.md#nodes) to the Site Tree.
- [Create a Widget](../developer-guides/widgets.md) for the frontend or admin panel dashboard
- [Define a link type](../developer-basics/routing.md#links) for Pagekit's Link picker
