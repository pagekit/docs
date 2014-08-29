# Themes

<p class="uk-article-lead">Create your own theme to control the look of your site.</p>

Themes and extensions in Pagekit are very much the same. The biggest difference
you will encounter will be in naming things `theme.php` instead of
`extension.php` and a few basic configuration differences. Except for that, try
not to think in terms of developing a theme vs. developing an extension but
rather understand that you have access to Pagekit's framework all the time.

## Basic structure

The easiest way to get started with a new theme is by using
our command line tool to create the skeleton file structure.

```bash
cd path/to/pagekit
./pagekit theme:generate MY-THEME
```

You will be prompted for some information needed to initialize the theme.

| Input             | Example               | Description |
|-------------------|-----------------------|--------------|
| `Title`           | `My Theme`            | The human readable name of your theme
| `Author`          | `YOOtheme`            | Your name or your company's name
| `Email`           | `demo@yootheme.com`   | Your email address
| `PHP Namespace`   | `MyTheme`             | Identifier used to organize your code files. PHP namespaces usually follow a CamelCase syntax.

This produces the following file structure inside the directory `/themes/MY-THEME`. Note that this is the minimal setup for a theme, these 3 files are required for a valid theme.

| Folder / File | Description |
|---------------|-------------|
| `/templates` | The markup files are located in this folder |
| `/templates/template.razr` | The main markup file |
| `theme.json` | Holds the metadata for the system and the marketplace |
| `theme.php` | Holds the themes configuration, setup and custom code |

**Note** Can't see your changes in the frontend? Don't forget to enable your theme in the admin area.

## Metadata

`theme.json` is a JSON representation of your theme's metadata (license, author etc). This is mainly needed when distributing the theme via the marketplace, but is also important for how the theme is listed in the backend.

```json
{
    "name": "MY-THEME",
    "version": "0.0.1",
    "type": "theme",
    "title": "MyTheme",
    "description": "",
    "license": "",
    "authors": [
        {
            "name": "Joe",
            "email": "joe@example.com",
            "homepage": "http://example.com"
        }
    ],
    "extra": {
      "image": "theme.jpg"
    }
}
```

## Configuration

`theme.php` contains PHP code for the theme configuration. In the beginning it's fine to start with a `theme.php` that just returns an empty configuration array. You can set options later when you need them. We will talk about detailed configuration options in the [Configuration](configuration.md) chapter.


```php
<?php

// config array
return [];
```

## Templating

Templating in Pagekit is powered by the [Razr Templating engine](https://github.com/pagekit/razr). `/templates/template.razr` is the main layout file.
Here's a minimal example:

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        @section('head')
    </head>
    <body>
        @section('messages')
        @section('content')
    </body>
</html>
```

Note how `@section(...)` calls an action that views and controllers can hook into to add their own output. `head` is for scripts, resources and everything that ends up in the `<head>` section, `messages` is for system messages and `content` is where the main output ends up. You will see later how you can register your own actions.

Now head to the browser, enable your theme from the backend and give it a go. You should get a very plain output of your content. Now that you've seen the basics, let's go for something more fancy.

## Adding CSS and JavaScript

So far, our site looks pretty dull. To add some of your own CSS and JavaScript, add the following line in the `<head>` section of `/templates/template.razr`.

```html
@style('MY-THEME', 'theme://MY-THEME/assets/css/MY-THEME.css')
@script('MY-THEME', 'theme://MY-THEME/assets/js/theme.js', ['jquery', 'uikit'])
```

**Note** Make sure to create `/themes/MY-THEME/assets/css/MY-THEME.css` and `/themes/MY-THEME/assets/js/theme.js`.

The `@style` and `@script` calls link stylesheet and script files to the generated page according to an optional list of dependencies. All calls are resolved to include scripts and styles in the correct order and only once.

The mandatory first parameter is an identifier used to reference this script or stylesheet. Styles and scripts have separate namespaces, which is why `MY-THEME` can be used in both cases. The second parameter is mandatory and contains the file path. The third parameter is optional and a list of dependencies to be included beforehand.

Note how the JavaScript in our example has two dependencies added as a list. These get resolved to be included before `/js/theme.js` is included. `jquery` and `uikit` are aliases that come pre-defined with Pagekit.

## Widget positions

Positions are a concept of defining areas in your markup that are known to Pagekit, so that you can publish multiple widgets inside those positions. Usually, these positions are used for the logo, menus, sidebars and so on. In this example we want to add a footer position.

Define the position in your `theme.php`.

```php
return [
    'positions' => [
        'footer' => 'Footer',
        ...
    ],
    ...
```

Inside `/templates/template.razr` you will determine the actual rendering location.

```html
@if (hasSection('footer'))
    <footer>
         @section('footer')
    </footer>
@endif
```

Whenever a widget is published in the `footer` position, it will now be rendered at your specified location.

## Renderer

By default, a widget will be be rendered to the widget position without any additional markup. To change this, you can provide a custom renderer to the `render` function. A renderer can be implemented in PHP or using the Razr template engine.

To use a custom renderer `footer`, you can pass it as a value for the `renderer` parameter.

```html
@section('footer', ['renderer' => 'footer'])
```
We will need to register the custom renderer to make it available in the theme. To do so, open the main class and register the renderer in the `boot` method.
```php
$app['sections']->addRenderer('footer', 'theme://MY-THEME/views/renderer/position.footer.razr');
```

Now we create a renderer `position.footer.razr` in `/views/renderer`.
In this file you can use PHP code to process the data of the widget. You can access the widgets data with `@($value)` (or `$value` if you're writing the renderer in plain PHP).

The renderer could look like this:

```html
@foreach ($value as $widget)
<div class="footerclass">
    @($widget.showTitle ? "<h3>" . $widget.title . "</h3>")
    @($widget.render($options)
</div>
@endforeach
```

All widgets published in the footer position will now be rendered inside a `<div>` with the CSS class `.footerclass`. If the title should be displayed, it will be rendered as a `<h3>`.

## Where to go from here?

Now that you have a basic theme, you may want to:

- Add additional configuration to your theme. See [Configuration](configuration.md) for more information.
- Add a settings page to your theme. See [Settings](settings.md) for more information.
- Upload your theme to the Pagekit marketplace so others can enjoy it. See [Marketplace](marketplace.md) for more information.
