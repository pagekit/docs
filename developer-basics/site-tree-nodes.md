# Site Tree Nodes

<p class="uk-article-lead">Pagekit's Site Tree can be extended by definining your own Node type.</p>

When inside the Site tree, you can add new elements with the *Add Page* button. A dropdown will appear that shows all available Node types. Once you have created a node, it can be dragged around the Tree of your Site. A node can be seen as a dynamic route, as the actual URL of the node depends on the location of the node inside the Site Tree.

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
