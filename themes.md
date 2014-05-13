# Themes

Themes and extensions in Pagekit are very much the same. The biggest difference
you will encounter will be in naming things *theme.php* instead of
*extension.php* and a few basic configuration differences. Except for that, try
not to think in terms of *developing a theme* vs. *developing an extension* but
rather understand that you have access to Pagekit's framework all the time.

To give you full control over Pagekit's appearance, a powerful theming system is
right by your side. The easiest way to get started with a new theme is by using
our command line tool to create the skeleton file structure.

```
cd path/to/pagekit
./pagekit theme:generate mytheme
```

This produces the following file structure inside the directory `themes/mytheme`. Note that this is the minimal setup for a theme, these 3 files are required for a valid theme.

| Folder / File | Description |
|---------------|-------------|
| `/templates` | The markup files are located in this folder |
| `/templates/template.razr.php` | The main markup file |
| `theme.json` | Holds the metadata for the system and the marketplace |
| `theme.php` | Holds the themes configuration, setup and custom code |

Can't see your changes in the frontend? Don't forget to enable your theme in the admin area.

## Metadata

*theme.json* is a JSON representation of your theme's metadata (license, author etc). This is mainly needed when distributing the theme via the marketplace, but is also important for how the theme is listed in the backend.

```json
{
    "name": "mytheme",
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

Note how the `extra` property can be used to set a screenshot that is displayed in the admin area.

## Configuration

*theme.php* contains PHP code for theme configuration. In the beginning it's fine to start with a *theme.php* that just returns an empty configuration array. You can set options later when you need them. We will talk about detailed configuration options in the [configuration](configuration.md) chapter.


```php
<?php

// config array
return array();
```

## Templating

Templating in Pagekit is powered using the [Razr Templating engine](https://github.com/pagekit/razr). *templates/template.razr.php* is the main layout file. 
Here's a minimal example:

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        @action('head')
    </head>
    <body>
        @action('messages')
        @action('content')
    </body>
</html>
```

Note how `@action(...)` calls an action that views and controllers can hook into to add their own output. `head` is for scripts, resources and everything that ends up in the `<head>` section, `messages` is for system messages and `content` is where the main output ends up. You will see later how you can register your own actions.

Now head to the browser, enable your theme from the backend and give it a go. You should get a very plain output of your content. Now that you've seen the basics, let's go for something more fancy.

## Adding CSS and JavaScript

So far, our site looks pretty dull. To add some of your own css and JavaScript, add the follwing line in the `<head>` section of *templates/template.razr.php*.

```html
@style('mytheme', 'theme://mytheme/assets/css/mytheme.css')
@script('mytheme', 'theme://mytheme/assets/js/theme.js', ['jquery', 'uikit'])
```

Make sure to create `<themes>/mytheme/assets/css/mytheme.css` and `<themes>mytheme/assets/js/theme.js`.
Note how the JavaScript we include has two requirements, that we add as a list. These get automatically
resolved to be included *before* `js/theme.js` is included. `jquery` and `uikit` are aliases that
come pre-defined with Pagekit.

## Widget positions

Positions are a concept of defining areas in your markup that are known to Pagekit, so that you can publish multiple widgets inside those positions. Usually, these positions are used for the logo, menus, sidebars and so on. In this example we want to add a footer position.

Define the position in your `theme.php`.

```php
return array(
    'positions' => array(
        'footer'    => 'Footer',
        ...
    ),
    ...
```

Inside `templates/template.razr.php` you will determine the actual rendering location.

```html
@if (app.positions.exists('footer'))
    <footer>
         @app.positions.render('footer')
    </footer>
@endif
```

Whenever a widget is published in the *footer* position, it will now be rendered at your specified location.

## Renderer

By default, a widget will be be rendered to the widget position without any additional markup. To change this, you can provide a custom renderer to the `render` function. A renderer can be implemented in PHP or using the Razr template engine.

To use a custom renderer `footer`, you can pass it as a value for the `renderer` parameter. 

```html
@app.position.render('footer', ['renderer' => 'footer'])
```

Now we create a renderer `position.footer.razr.php` in `views/renderer/`.
In this file you can use PHP code to process the data of the widget. You can access the widgets data with `@widgets` (or `$widgets` if you're writing the renderer in plain PHP).

The renderer could look like this:

```html
@foreach (widgets as widget)
<div class="footerclass">
    @(widget.showTitle ? "<h3>" ~ widget.title ~ "</h3>")
    @provider.render(widget, options)
</div>
@endforeach
```

All widgets published in the footer position will now rendered in a `<div>`-box with the CSS class `footerclass`. If the title should be displayed it will be rendered in `<h3>`.

## Where to go from here?

Now that you have a basic theme, you may want to:

- add additional configuration to your theme. See (Configuration)[configuration.md]
- add a settings page to your theme. See (Settings)[settings.md]
- upload your theme to the Pagekit marketplace so others can enjoy it. See (Marketplace)[marketplace.md]
