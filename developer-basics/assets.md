# Working with Assets

<p class="uk-article-lead">Assets are the static files needed in your project, including CSS, JS and image files.

## Register resource shorthands

In your module's `index.php`, you can register a string to reference a path.

The path is interpreted relative to the location of the `index.php`. This means that if you want to have `theme:` reference the theme folder, the path is just the empty string. To reference the `views` folder inside the theme, the path is `'views'`.

```
 theme/
     index.php
     views/
       template.php
```

```php
'resources' => [

    'theme:' => '',
    'views:' => 'views'

],
```

## Generate URL to static assets

When you have registered resource paths, you can now use the `theme:` shorthand when referencing paths.

```php
<img src="<?= $view->url()->getStatic('theme:image.jpg') ?>">
```

## Include CSS in view file

To include a CSS file in your view, call the style helper from the view renderer. You can require other stylesheets with the second parameter.

```
<?php $view->style('theme', 'theme:css/theme.css') ?>
<?php $view->style('theme', 'theme:css/theme.css', 'uikit') ?>
<?php $view->style('theme', 'theme:css/theme.css', ['uikit', 'somethingelse']) ?>
```

This will not directly output the required line in HTML. Instead, it will add the CSS file to the Pagekit Asset Manager. The stylesheet will be included in the `<head>` section of your theme as long as you have the following line added:

```php
<?= $view->render('head') ?>
```

## Include JS in view file

To include a JavaScript file in your template, call the script helper from the view renderer. As an optional second parameter, you can require one or multiple scripts that need to be loaded before.

```
<?php $view->script('theme', 'theme:js/theme.js') ?>
<?php $view->script('theme', 'theme:js/theme.js', 'jquery') ?>
<?php $view->script('theme', 'theme:js/theme.js', ['jquery', 'uikit']) ?>
```

**Note** Internally, `style()` and `script()` each work with their own Asset Manager. As these are separate from each other, you can assign the same name to a CSS and JS file without conflict (both called `theme` in above example).
