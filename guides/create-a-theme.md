# Create a theme

<p class="uk-article-lead">Get started and create your own theme for Pagekit.</p>

## Location of theme files

The example theme we will be building in this guide is the *Hello* theme. This tiny theme can be used as a starter theme. It's available via the Pagekit marketplace and contains all examples we describe here. When installed, the *Hello* theme is located in `/packages/pagekit/theme-hello`.

## File structure

In order for Pagekit to recognize a directory as a valid theme, you have to follow a certain file structure, at least for a few required files.

The required files are `composer.json`, `index.php` and `views/template.php`. Other files are optional and can be added as needed and named as you prefer it.

| File                         | Needed?  | Description             |
|------------------------------|----------|-------------------------|
| `composer.json`              | required | Package metadata        |
| `index.php`                  | required | Theme module definition |
| `views/template.php`         | required | Main template file      |
| `css/theme.css`              | optional | Theme Stylesheet        |
| `js/theme.js`                | optional | Theme JavaScript        |

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

Internally, Themes in Pagekit are handled as *Modules*. This opens up a lot of possibilities of what you can do with a theme, because modules allow you to access all of Pagekit's functionality. The main thing to understand in the beginning is that the definition of your theme happens in the `index.php`.

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

| Property       | Required?  | Description                                    |
|----------------|------------|------------------------------------------------|
| `name`         | required   | Theme identifier                               |
| `type`         | required   | Module type (needs to be `theme` for a theme)  |
| `resources`    | optional   | Resource paths shorthands                      |
| `menus`        | optional   | Define menu positions                          |
| `positions`    | optional   | Define widget positions                        |
| `settings`     | optional   | Route to theme settings screen                 |
| `config`       | optional   | Default module config                          |
| `events`       | optional   | Listen to events                               |
| ...            | optional   | All properties from extensions can be used     |            

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

| Object         | Description                                                 |
|----------------|-------------------------------------------------------------|
| `$view`        | View renderer instance                                      |
| `$params`      | Theme parameters                                            |
| `$app`         | Application container instance                              |

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

With this basic file structure completed, you can now go to Pagekit's admin panel. Navigate to *Sytem &raquo Themes* and Enable your new theme by clicking the star icon next to it.

You should now be able to refresh the frontend and see the content without any styling. And this is where you come in. You have the basic setup up and running and can now start to change the markup and add your CSS and JS.

## Where to go from here

Congratulations, you've created your first theme! We've introduced the basic file structure and talked about the most important configuration options. You know how to change the markup, include CSS and JS and how to add widget functionality. Now it's time to get creative and play around.

If you're looking for advanced functionality, here are some pointers to other sections of the documentation:

- TODO
- TODO
- TODO

You can find a more in-depth read in the [Developer](../developer-basics/themes.md) section.