# Create a theme
<p class="uk-article-lead">Create a theme to change the look of your site.</p>

## Location of theme files
The example theme we will be building in this guide is the _Hello_ theme. This tiny theme can be used as a starter theme. It's available via the Pagekit marketplace and contains all examples we describe here. When installed, the _Hello_ theme is located in `/packages/pagekit/theme-hello`.

## File structure
In order for Pagekit to recognize a directory as a valid theme, you have to follow a certain file structure, at least for a few required files.

The required files are `composer.json`, `index.php` and `views/template.php`. Other files are optional and can be added as needed and named as you prefer it.

File                 | Needed?  | Description
-------------------- | -------- | -----------------------
`composer.json`      | required | Package metadata
`index.php`          | required | Theme module definition
`views/template.php` | required | Main template file
`css/theme.css`      | optional | Theme Stylesheet
`js/theme.js`        | optional | Theme JavaScript

## Package definition: composer.json
A theme is a regular Pagekit package. Each package needs a description in order to be recognized by Pagekit. This description is located in the `composer.json` and looks as follows. Detailed information is availabe in the [Packages](../developer-basics/packages.md) chapter.

```json
{
    "name": "pagekit/theme-hello",
    "type": "pagekit-theme",
    "version": "0.9.0",
    "title": "Hello"
}
```

## Module definition: index.php
Internally, Themes in Pagekit are handled as _Modules_. This opens up a lot of possibilities of what you can do with a theme, because modules allow you to access all of Pagekit's functionality. The main thing to understand in the beginning is that the definition of your theme happens in the `index.php`.

Make sure this file returns a PHP array. By setting the right properties in this array, you tell Pagekit everything it needs to know about your theme.

Define the positions and menus of your theme, load additional scripts and much more. To get started, here is a simple example for your `index.php`. Explanations of the single properties follow below.

```php
<?php

return [


    'name' => 'theme-hello',

    'type' => 'theme',

    /**
     * Resource shorthands.
     */
    'resources' => [

        'theme:' => '',
        'views:' => 'views'

    ],

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

    ],

    /**
     * Default theme configuration.
     */
    'config' => []

];
```

`resources` allow you to register a shorthand to a path reference. This makes it easier when referencing files because you do not have to specify the full path. For example `views:template.php` will expand to `packages/vendor/theme/views/template.php`

Your theme defines locations to render menus and widgets. The actual rendering happens in the `template.php`, as we will show below. However, your theme needs to register these positions before. This happens with the `menus` and `positions` property. These contain arrays of the position name and a label which displays in the admin panel.

Other properties can be added for more advanced settings.

Property    | Required? | Description
----------- | --------- | ---------------------------------------------
`name`      | required  | Theme identifier
`type`      | required  | Module type (needs to be `theme` for a theme)
`resources` | optional  | Resource paths shorthands
`menus`     | optional  | Define menu positions
`positions` | optional  | Define widget positions
`settings`  | optional  | Route to theme settings screen
`config`    | optional  | Default module config
`events`    | optional  | Listen to events
...         | optional  | All properties from extensions can be used

### `menus`: Register menu positions for a theme
In your theme you can render menus from the Pagekit system in as many positions as you want. To make these positions known to Pagekit, you need to register them using the `menus` property.

Each menu position is defined by an identifier (i.e. `main`) and a label to be displayed to the user (i.e. `Main`).

```php
'menus' => [

    'main' => 'Main',
    'offcanvas' => 'Offcanvas'

],
```

### `positions`: Register widget positions for a theme
Widget positions allow users to publish Widgets in several locations of your theme markup. They appear in the Widget area of the Pagekit admin panel are selectable by the user when setting up a Widget.

Each widget position is defined by an identifier (i.e. `sidebar`) and a label to be displayed to the user (i.e. `Sidebar`).

```php
'positions' => [

    'sidebar' => 'Sidebar',

],
```

## template.php
`views/template.php` is the main file for the theme markup. It is a PHP file that has the following objects available for rendering:

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
        <?php $view->script('theme:js/theme.js', 'jquery') ?>
    </head>
    <body>

        <!-- Render logo with site URL -->
        <?php if ($logo = $params['logo']) : ?>
        <a href="<?= $view->url()->get() ?>">
            <img src="<?= $this->escape($logo) ?>" alt="">
        </a>
        <?php endif ?>

        <!-- Render menu position -->
        <?php if ($view->menu()->exists('main')) : ?>
            <?= $view->menu('main') ?>
        <?php endif ?>

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

## theme.css &amp; theme.js
Because our template already includes these files, make sure to also create the two files `js/theme.js` and `css/theme.css`. You can leave them empty for now.

```
<?php $view->script('theme:js/theme.js') ?>
<?php $view->style('theme', 'theme:css/theme.css') ?>
```

To learn more about working with static files, read the [Assets](../developer-basics/assets.md) section.

## Activate theme
With this basic file structure completed, you can now go to Pagekit's admin panel. Navigate to _Sytem &raquo Themes_ and Enable your new theme by clicking the star icon next to it.

You should now be able to refresh the frontend and see the content without any styling. And this is where you come in. You have the basic setup up and running and can now start to change the markup and add your CSS and JS.

## Where to go from here
Congratulations, you've created your first theme! We've introduced the basic file structure and talked about the most important configuration options. You know how to change the markup, include CSS and JS and how to add widget functionality. Now it's time to get creative and play around.

If you're looking for advanced functionality, here are some pointers to other sections of the documentation:
- TODO
- TODO
- TODO

## Doing more with themes
Themes and extensions in Pagekit are very much the same. Try not to think in terms of developing a theme vs. developing an extension but rather understand that you have access to Pagekit's framework all the time.

Most importantly, the module definition in your theme's `index.php` can contain all properties, no matter if you have a theme or an extension. If you want to add a Settings screen to the admin panel, register additional tabs to the Site Tree or the Widget management interface, all of that works exactly the same.

## Render sub-view
In some cases you want to split your template in several files. This can be useful if you either have a lot of markup or if you have conditional rendering.

A classic example would be when you want to allow different versions of the menu to be rendered. In this case you should try to avoid nesting several `if` clauses and instead create additional files in your `views` folder.

```php
<?= $view->menu('main', 'menu-navbar.php') ?>
```

`menu-navbar.php` could look as follows:

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

`position-grid.php` can look as follows:

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
The Pagekit admin panel is built using the UIkit frontend framework. That is why the Pagekit core extensions such as static pages and the blog output markup with CSS classes from UIkit. You are, however, in no way forced to use UIkit to create your own themes.

To style the Pagekit system output, you can just add the CSS for a few classes instead of including the UIkit CSS. The `theme.css` that comes with the Hello extension already comes with the classes you need to style.

If you want to completely change the markup that Pagekit itself generates, you also have the possibility to overwrite system view files, to provide custom widget renderer and custom menu renderer.

## Overwrite system views
To overwrite system view files, you just need to put template files in the correct locations inside your theme folder.

File                         | Original view file                       | Description
---------------------------- | ---------------------------------------- | ------------------------
`views/system/site/page.php` | `/app/system/site/views/page.php`        | Default static page view
`views/blog/post.php`        | `/packages/pagekit/blog/views/post.php`  | Blog post single view
`views/blog/posts.php`       | `/packages/pagekit/blog/views/posts.php` | Blog posts list view

To understand which variables you have available in these views, check out the markup in the original view file.

## Add theme options to Site interface
This is done via JavaScript, most comfortably when you make use of Vue components.

Load your own JS when the Site Tree interface is currently active. In your `index.php`, you can do that when you listen to the right event:

```
'events' => [

    'view.system/site/admin/settings' => function ($event, $view) use ($app) {
        $view->script('site-theme', 'theme:js/site-theme.js', 'site-settings');
        $view->data('$theme', $this);
    },

    // ...

],
```

The `js/site-theme.js` contains a Vue component which renders the interface and takes care of the storing of theme settings.

**Note**: Although it's possible to do all of this in a single JS file and have the markup be represented in a string, best practice is to actually create `*.vue` files with your Vue component. Examples can be found in the `app/components` folder of the default _One_ theme.

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

## Add Theme tab to Node configuration in Site Tree
Often you want to attach theme options to a specific Node in the Site tree. For example you want to allow the user to pick a Hero image which can be different per page. To do so, we can add a _Theme_ tab to the Site interface.

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

**Note:** Compare with full Vue components in `app/components` folder of the default _One_ theme.

## Add theme options to Widget interface
Register a script to be loaded in Widget edit view.

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

**Note:** Compare with full Vue components in `app/components` folder of the default _One_ theme.

## Add a settings screen manually
If the prepared ways of adding a settings screen do not satisfy your needs, you can also manually create a completely new interface. With the module definition in `index.php` you have full control and can do something like the following:
1. Create a View file for your settings screen.
2. Create a new Controller with an action that renders the view file.
3. Register controller and routes inside your `index.php`
4. Optional: Set the `settings` property to the new settings screen route
