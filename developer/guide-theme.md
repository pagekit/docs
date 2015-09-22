# Create a theme

<p class="uk-article-lead">Get started and create your own theme for Pagekit.</p>

## Location of theme files

The example theme we will be building in this guide is the *Hello* theme. This
tiny theme can be used as a starter theme. It's available via the Pagekit
marketplace and contains all examples we describe here.

The *Hello* theme is located in `/packages/pagekit/theme-hello`. Every theme in
Pagekit is a *Package* which is why they simply sit alongside other packages
(i.e. extensions).

Every package belongs to a specific vendor (for example `pagekit` for the
*Hello* theme) and is therefore located in the according subdirectory. The
vendor name is a unique representation of you as a developer or organization.
In the simplest case, it just matches a github username.

The directory name is up to you. The actual name of the theme is defined inside
the theme files and not by the directory name. However, it makes sense to give a
clear name to the folder and start with the prefix `theme-`. Remember that
extensions will end up in the same directory, so a clear name will make it
easy to tell that this is in fact a theme directory.

## File structure

In order for Pagekit to recognize a directory as a valid theme, you have to
follow a certain file structure, at least for a few required files.

The required files are `composer.json`, `index.php` and `views/template.php`.
Other files are optional and can be added as needed and named as you prefer it.

| File                         | Needed?  | Description             |
|------------------------------|----------|-------------------------|
| `composer.json`              | required | Package metadata        |
| `index.php`                  | required | Theme module definition |
| `views/template.php`         | required | Main template file      |
| `css/theme.css`              | optional | Theme Stylesheet        |
| `js/theme.js`                | optional | Theme JavaScript        |


## composer.json

A theme is a regular Pagekit package. Each package needs a description in
order to be recognized by Pagekit (and also if you decide to upload it to the
marketplace later). This description is located in the `composer.json` and
looks as follows.

```json
{
    "name": "pagekit/hello-theme",
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

## index.php

Internally, themes in Pagekit are handled as *Modules*. This opens up a lot of
possibilities of what you can do with a theme. The main thing to understand in
the beginning is that the definition of your theme happens in the `index.php`.

Make sure this file returns a PHP array. By setting the right properties in
this array, you tell Pagekit everything it needs to know about your theme.

Define the positions and menus of your theme, load additional scripts and much
more. To get started, here is a simple example for your `index.php`.
Explanations of the single properties follow below.

```php
<?php

return [


    'name' => 'hello-theme',

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

`resources` allow you to register a shorthand to a path reference. This makes
it easier when referencing files because you do not have to specify the full
path. For example `views:template.php` will expand to
`packages/vendor/theme/views/template.php`

Your theme defines locations to render menus and widgets.
The actual rendering happens in the `template.php`, as we will show below.
However, your theme needs to register these positions before. This happens
with the `menus` and `positions` property. These contain arrays of the position
name and a label which displays in the backend.

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

## template.php

`views/template.php` is the main file for the theme markup. It is a PHP file
that has the following objects available for rendering:

| Object         | Description                                                 |
|----------------|-------------------------------------------------------------|
| `$view`        | View renderer instance                                      |
| `$params`      | Theme parameters                                            |
| `$app`         | Application container instance                              |

**Note** With PHP templating, the short notation `<?= $var ?>` prints the value
of the the variable `$var`.

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

Because our template already includes these files, make sure to also create
the two files `js/theme.js` and `css/theme.css`. You can leave them empty for
now.

To include a JavaScript file in your template, call the script helper from the
view renderer. As an optional second parameter, you can require one or multiple
scripts that need to be loaded before.

```
<?php $view->script('theme:js/theme.js') ?>
<?php $view->script('theme:js/theme.js', 'jquery') ?>
<?php $view->script('theme:js/theme.js', ['jquery', 'uikit']) ?>
```

To include a CSS file in your template, call the style helper from the view
renderer. You can require other stylesheets the same way as with scripts.

```
<?php $view->style('theme', 'theme:css/theme.css') ?>
<?php $view->style('theme', 'theme:css/theme.css', 'uikit') ?>
<?php $view->style('theme', 'theme:css/theme.css', ['uikit', 'somethingelse']) ?>
```

## Activate theme

With this basic file structure completed, you can now go to Pagekit backend.
Navigate to *Sytem &raquo Themes* and Enable your new theme by clicking
the star icon next to it.

You should now be able to refresh the front-end and see the content without any
styling. And this is where you come in. You have the basic setup up and running
and can now start to change the markup and add your CSS and JS.

## Render sub-view

In some cases you want to split your template in several files. This can
be useful if you either have a lot of markup or if you have conditional
rendering.

A classic example would be when you want to allow different versions of the menu
to be rendered. In this case you should try to avoid nesting several `if`
clauses and instead create additional files in your `views` folder.

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

## Add a settings screen

TODO

## Default Pagekit markup

The Pagekit backend is built using the UIkit front-end framework. That is why
the Pagekit core extensions such as static pages and the blog output markup
with CSS classes from UIkit. You are, however, in no way forced to use UIkit
to create your own themes.

To style the Pagekit system output, you can just add the CSS for a few classes
instead of including the UIkit CSS. The `theme.css` that comes with the Hello
extension already comes with the classes you need to style.

If you want to completely change the markup that Pagekit itself generate, you
also have the possibility to overwrite system view files, to provide custom
widget renderer and custom menu renderer.

## Overwrite system views

To overwrite system view files, you just need to put template files in the
correct locations inside your theme folder.

| File                         | Original view file                       | Description               |
|------------------------------|------------------------------------------|---------------------------|
| `views/system/site/page.php` | `/app/system/site/views/page.php`        | Default static page view  |
| `views/blog/post.php`        | `/packages/pagekit/blog/views/post.php`  | Blog post single view     |
| `views/blog/posts.php`       | `/packages/pagekit/blog/views/posts.php` | Blog posts list view      |

To understand which variables you have available in these views, check out the
markup in the original view file.

## Add theme options to Site interface

TODO

## Add theme options to Widget interface

TODO

## Custom Error Pages

TODO

## Where to go from here

Congratulations, you've created your first theme! We've introduced the basic
file structure and talked about the most important configuration options. You
know how to change the markup, include CSS and JS and how to add widget
functionality. Now it's time to get creative and play around.

If you're looking for advanced functionality, here are some pointers to other
sections of the documentation:

- TODO
- TODO
- TODO
