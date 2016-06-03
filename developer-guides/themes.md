# Themes
<p class="uk-article-lead">A theme changes the look of your site. In its simplest form a theme generates the surrounding HTML markup for the content output of your extensions.</p>

**Note** The examples in this guide are taken from the _Hello_ theme, which is available in the Marketplace. When installed, the _Hello_ theme is located in `/packages/pagekit/theme-hello`.

<ul class="uk-list">
    <li><a href="#package-definition">Package definition</a></li>
    <li><a href="#module-definition">Module definition</a></li>
    <li><a href="#layout-file">Layout file</a></li>
    <li><a href="#menu-and-position-renderer">Menu and Position renderer</a></li>
    <li><a href="#default-pagekit-markup">Default Pagekit markup</a></li>
    <li><a href="#overwrite-system-views">Overwrite system views</a></li>
    <li><a href="#add-theme-options-to-the-site-interface">Add theme options to the Site interface</a></li>
    <li><a href="#add-a-theme-tab-to-the-node-configuration-in-the-site-tree">Add a Theme tab to the Node configuration in the Site Tree</a></li>
    <li><a href="#add-theme-options-to-the-widget-interface">Add theme options to the Widget interface</a></li>
    <li><a href="#manually-add-a-settings-screen">Manually add a settings screen</a></li>
</ul>

## Package definition
A theme is a regular Pagekit [package](../developer-basics/packages.md) of the type `pagekit-theme`. Each package needs a description in order to be recognized by Pagekit. This description is located in the `composer.json` file and looks as follows. For detailed information, take a look the [Packages](../developer-basics/packages.md) chapter.

```json
{
    "name": "pagekit/theme-hello",
    "type": "pagekit-theme",
    "version": "0.9.0",
    "title": "Hello"
}
```

## Module definition
A theme in itself is simply a [module](../developer-basics/modules.md). So you may want to read up on modules first. This opens up a lot of possibilities with regard to what a theme can do.

Define the positions and menus of your theme, load additional scripts and much more. Here is a shortened example of the `index.php` to get you started. Explanations of the theme specific properties follow below.

```php

return [

    'name' => 'theme-hello',

    /**
     * Define menu positions.
     */
    'menus' => [

        'main' => 'Main',

    ],

    /**
     * Define widget positions.
     */
    'positions' => [

        'sidebar' => 'Sidebar',

    ]

];
```

Your theme defines locations to render menus and widgets. The actual rendering happens in the `template.php`, as is demonstrated below. However, your theme needs to register these positions before. This happens with the `menus` and `positions` property. These contain arrays of the position name and a label, which is displayed in the admin panel.

### Menus
In your theme, you can render menus from the Pagekit system in as many positions as you want. To make these positions known to Pagekit, you need to register them using the `menus` property.

Each menu position is defined by an identifier (e.g. `main`) and a label to be displayed to the user (e.g. _Main_).

```php
'menus' => [

    'main' => 'Main',
    'offcanvas' => 'Offcanvas'

],
```

### Positions
Widget positions allow users to publish widgets in several locations of your theme markup. They appear in the _Widgets_ area of the Pagekit admin panel and can be selected by the user when setting up a widget.

Each widget position is defined by an identifier (i.e. `sidebar`) and a label to be displayed to the user (i.e. _Sidebar_).

```php
'positions' => [

    'sidebar' => 'Sidebar',

],
```

## Layout file
Apart from the mandatory module files, a theme brings its own `views/template.php` file. It is the main file for the theme's markup with the following objects available for rendering.

Object    | Description
--------- | ------------------------------
`$view`   | View renderer instance
`$params` | Theme parameters
`$app`    | Application container instance

**Note** With PHP templating, the short notation `<?= $var ?>` prints the value of the the variable `$var`.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <!-- Always render head section from the system -->
        <?= $view->render('head') ?>

        <!-- Include theme css -->
        <?php $view->style('theme', 'theme:css/theme.css') ?>

        <!-- Include theme JS, require jQuery which will also be included -->
        <?php $view->script('theme', 'theme:js/theme.js', 'jquery') ?>
    </head>
    <body>

        <!-- Render logo with site URL -->
        <?php if ($logo = $params['logo']) : ?>
        <a href="<?= $view->url()->get() ?>">
            <img src="<?= $this->escape($logo) ?>" alt="">
        </a>
        <?php endif; ?>

        <!-- Render menu position -->
        <?php if ($view->menu()->exists('main')) : ?>
            <?= $view->menu('main') ?>
        <?php endif; ?>

        <!-- Render widget position -->
        <?php if ($view->position()->exists('sidebar')) : ?>
            <?= $view->position('sidebar') ?>
        <?php endif; ?>

        <!-- Render system messages -->
        <?= $view->render('messages') ?>

        <!-- Render content -->
        <?= $view->render('content') ?>

        <!-- Insert code before the closing body tag  -->
        <?= $view->render('footer') ?>

    </body>
</html>
```

## Menu and Position renderer
You might want to use a custom menu or position renderer. Below you'll find two examples of how to use them.

```php
<?= $view->menu('main', 'menu-navbar.php') ?>
```

In this case the `main` menu will be rendererd with the `menu-navbar.php` layout file.

```php
<ul class="uk-navbar-nav">

    <?php foreach ($root->getChildren() as $node) : ?>
    <li>

    <!-- ... more markup ... -->

    </li>
    <?php endforeach ?>

</ul>
```

The same works for widget positions.

```php
<?= $view->position('hero', 'position-grid.php') ?>
```

Here, the widget position `hero` will be rendered with the `position-grid.php` layout file.

```php
<?php foreach ($widgets as $widget) : ?>
<div class="uk-width-1-<?= count($widgets) ?>">

    <div>

        <h3><?= $widget->title ?></h3>

        <?= $widget->get('result') ?>

    </div>

</div>
<?php endforeach ?>
```

## Default Pagekit markup
The Pagekit admin panel is built using the UIkit frontend framework. That is why Pagekit core extensions, like static pages and the blog, output markup with CSS classes from UIkit. You are, however, in no way obliged to use UIkit to create your own themes.

To style the Pagekit system output, you can just add the CSS for a few classes instead of including the entire UIkit CSS. The `theme.css` file that comes with the Hello extension already includes the necessary classes.

If you want to completely change the markup that Pagekit itself generates, you also have the possibility to overwrite system view files.

## Overwrite system views
To overwrite system view files, you just need to create corresponding folders inside your theme to mimic the original structure and put the template files there, as shown in the table below.

File                         | Original view file                       | Description
---------------------------- | ---------------------------------------- | ------------------------
`views/system/site/page.php` | `/app/system/site/views/page.php`        | Default static page view
`views/blog/post.php`        | `/packages/pagekit/blog/views/post.php`  | Blog post single view
`views/blog/posts.php`       | `/packages/pagekit/blog/views/posts.php` | Blog posts list view

To understand which variables are available in these views, check out the markup in the original view file.

## Add theme options to the Site interface
This is done via JavaScript, most comfortably when you make use of Vue components.

Load your own JS when the Site Tree interface is currently active. In your `index.php`, you can do this when you listen to the right event.

```
'events' => [

    'view.system/site/admin/settings' => function ($event, $view) use ($app) {
        $view->script('site-theme', 'theme:js/site-theme.js', 'site-settings');
        $view->data('$theme', $this);
    },

    // ...

],
```

The `js/site-theme.js` contains a Vue component which renders the interface and takes care of storing of the theme settings.

**Note** Although it's possible to do all of this in a single JS file and have the markup represented in a string, the best practice is to actually create `*.vue` files with your Vue component. Examples can be found in the `app/components` folder of the default _One_ theme.

```js
window.Site.components['site-theme'] = {

    section: {
        label: 'Theme',
        icon: 'pk-icon-large-brush',
        priority: 15
    },

    template: '<div>Your form markup here</div>',

    data: function () {
        return window.$theme;
    },

    events: {

        save: function() {

            var config = _.omit(this.config, ['positions', 'menus', 'widget']);

            this.$http.post('admin/system/settings/config', {name: this.name, config: config}).error(function (data) {
                this.$notify(data, 'danger');
            });

        }

    }

};
```

## Add a Theme tab to the Node configuration in the Site Tree
Often you want to attach theme options to a specific Node in the Site tree. For example, you want to allow the user to pick a Hero image, which can be different per page. To do so, we can add a _Theme_ tab to the Site interface.

```php
'events' => [

    // ...

    'view.system/site/admin/edit' => function ($event, $view) {
        $view->script('node-theme', 'theme:js/node-theme.js', 'site-edit');
    },

    // ...
];
```

Example for `js/node-theme.js`:

```js
window.Site.components['node-theme'] = {

    section: {
        label: 'Theme',
        priority: 90
    },

    props: ['node'],

    template: '<div>Your form markup here</div>'

};
```

**Note** Compare with full Vue components in the `app/components` folder of the default _One_ theme.

## Add theme options to the Widget interface
Register a script to be loaded in the _Widget_ edit view.

```
'view.system/widget/edit' => function ($event, $view) {
    $view->script('widget-theme', 'theme:app/bundle/widget-theme.js', 'widget-edit');
},
```

Example for `widget-theme.js`:

```js
window.Widgets.components['widget-theme'] = {

    section: {
        label: 'Theme',
        priority: 90
    },

    props: ['widget', 'config'],

    template: '<div>Your form markup here</div>'

};
```

**Note** Compare with full Vue components in the `app/components` folder of the default _One_ theme.

## Manually add a settings screen
If the prepared ways of adding a settings screen do not satisfy your needs, you can also manually create a completely new interface. With the module definition in the `index.php` file you have full control and can do something like the following:
1. Create a View file for your settings screen.
2. Create a new Controller with an action that renders the view file.
3. Register controller and routes inside your `index.php`
4. Optional: Set the `settings` property to the new settings screen route
