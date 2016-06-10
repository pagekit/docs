# Views &amp; Templating
<p class="uk-article-lead">While the controller handles the incoming request, the view is responsible for rendering the response. To achieve this, it utilizes a templating engine. In Pagekit, you can use plain PHP templates or the Twig templating engine.</p>

<ul class="uk-list">
    <li><a href="#rendered-view-response">Rendered view response</a></li>
    <li><a href="#render-a-view-manually">Render a view manually</a></li>
    <li><a href="#templating">Templating</a></li>
    <li><a href="#working-with-assets">Working with Assets</a></li>
    <li><a href="#twig-templates">Twig templates</a></li>
</ul>

## Rendered view response

The most common way to render a view is to return an array from your controller action. Use the `$view` property to pass parameters to your view renderer.

```php
public function indexAction($name = '')
{
    return [
        '$view' => [

            // Rendered in the site's <title>
            'title' => 'Hello World',

            // view file that is rendered
            'name' => 'hello:views/index.php',
        ],

        // pass parameters to view file
        'name' => $name
    ];
}
```

The rendered view file could look like this:

```php
<!-- packages/pagekit/extension-hello/views/index.php -->

<h1>Hello <?= $name ?></h1>
<p>
   ...
</p>
```

This view is wrapped in the main layout by default. To avoid this behavior, you can change `'layout' => false` in the $view array.

## Render a view manually

You can also manually access the `View` service to render a template file. This may come in handy, if you dynamically determine which view to load.

```php

use Pagekit\Application as App;

class MyController {

    public function anotherViewAction()
    {
        return App::render('hello:views/view.php', ['id' => 1]);
    }

}
```

The according view file:

```HTML
<!-- packages/pagekit/extension-hello/views/index.php -->

<h1>You are viewing article number <?= $id ?></h1>
<p>
   ...
</p>
```

## Templating
The views are rendered using the PHP templating engine, which offers defined global template variables and a set of view helpers.

### Including other views
Rendering sub views from your view is done with the `$view` helper. The `render` method evaluates and returns the content of the given template file. This is identical to rendering the view manually from the controller.

```php
$view->render('hello:views/view.php', ['id' => 1])
```

### Link to routes
As seen earlier, each route has a name that you can use to dynamically generate links to the specific route. There is a `Url` helper that exposes the functions of the `UrlProvider`.

```HTML
<a href="<?= $view->url('@hello/default/view') ?>">View all articles</a>
<a href="<?= $view->url('@hello/default/view', ['id' => 23]) ?>">View article 23</a>
```

You can link to assets, like images or other files, using `@url.to`.

```HTML
<img src="<?= $view->url()->getStatic('extensions/hello/extension.svg') ?>" alt="Extension icon" />
```

## Working with Assets
Assets are the static files needed in your project, including CSS, JS and image files.

### Generate URL to static assets
To generate a route to a static asset, use the `getStatic` method of the `UrlProvider` class.

```php
<img src="<?= $view->url()->getStatic('my-assets/image.jpg') ?>">
```

### Include CSS in view file
To include a CSS file in your view, call the `style helper` from the `View`. The first parameter is a unique identifier for the stylesheet, while the second parameter is the path of the stylesheet where `theme:` is a reference to the root directory of a package named 'theme'. You can define its dependencies through the third parameter.

```
<?php $view->style('name', 'package:dir/style.css') ?>
<?php $view->style('theme', 'theme:css/theme.css') ?>
<?php $view->style('theme', 'theme:css/theme.css', 'uikit') ?>
<?php $view->style('theme', 'theme:css/theme.css', ['uikit', 'somethingelse']) ?>
```

**Note** This will not directly output the required line in HTML. Instead, it will add the CSS file to the Pagekit Asset Manager. The stylesheet will be included in the `<head>` section of your theme.

### Include JS in view file
To include a JavaScript file in your template, call the `script helper` from the `View`. The first parameter is a unique identifier for the script, while the second paramter is the path of the stylesheet. You can define its dependencies through the third parameter.

```
<?php $view->script('theme', 'theme:js/theme.js') ?>
<?php $view->script('theme', 'theme:js/theme.js', 'jquery') ?>
<?php $view->script('theme', 'theme:js/theme.js', ['jquery', 'uikit']) ?>
```

**Note** Internally, `style()` and `script()` each work with their own Asset Manager. As these are separate from each other, you can assign the same name to a CSS and JS file without conflict (both called `theme` in the above example). However, no two scripts or stylesheets may have the same identifier. For example, when adding two stylesheets, one could be named 'theme', and the other 'custom'. 

### Async and deferred script execution

By default, a script that is included in the head section of the rendered HML markup, is fetched and executed immediately. Only afterwards does the browser continue to parse the page.

To change this behaviour, you can use the keywords `async` and `defer` when loading a script. Set the appropriate option in your PHP code and the resulting `<script>` tag will have these attributes set accordingly.

Attribute | Description
--------- | -----------
`async` | Tell the browser to execute the script asynchronously which means that parsing the page continues even while the script is being executed
`defer` | Tell the browser that the script is meant to be executed after the document has been parsed. Browser support of this HTML feature is not perfect, but using it might only cause problems in cases where the execution order of scripts is important.

Example: Deferred execution, no dependencies.

```
<?php $view->script('theme', 'theme:js/theme.js', [], ['defer']) ?>
```

Example: Deferred and async execution, with dependencies.

```
<?php $view->script('theme', 'theme:js/theme.js', ['jquery', 'uikit'], ['defer', 'async']) ?>
```

## Twig templates

Instead of using plain PHP for templating, you can also use Twig for your theme and extension templates. You can find documentation on the basic [Twig syntax and features](http://twig.sensiolabs.org/doc/templates.html) in the official Twig docs. Similar to the official Hello theme for default PHP templating, a [boilerplate Twig theme](https://github.com/florianletsch/theme-twig) also exists and can be used as a starting point.

### Building themes with Twig

By default, the main template file that Pagekit uses from a theme is located at `views/template.php` and it is rendered using the default PHP renderer. In your theme's `index.php` you can define a different layout file to be used as the main template.

```
/**
 * Change default layout to use a Twig template.
 */
'layout' => 'views:template.twig',
```

The file extension `*.twig` will cause Pagekit to render this template with the Twig engine. A basic example for your theme's `views/template.twig` looks as follows.

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        {{ view.render('head') | raw }}
        {{ view.style('theme', 'theme:css/theme.css') }}
        {{ view.script('theme', 'theme:js/theme.js') }}
    </head>
    <body>

        <!-- Render logo or title with site URL -->
        <a href="{{ view.url().get() }}">
            {% set logo = params.logo %}
            {% if logo %}
                <img src="{{ logo }}" alt="Logo">
            {% else %}
                {{ params.title }}
            {% endif %}
        </a>

        <!-- Render menu position -->
        {% if view.menu().exists('main') %}
            {{ view.menu('main') | raw }}
        {% endif %}

        <!-- Render widget position -->
        {% if view.position().exists('sidebar') %}
            {{ view.position('sidebar') | raw }}
        {% endif %}

        <!-- Render system messages -->
        {{ view.render('messages') | raw }}

        <!-- Render content -->
        {{ view.render('content') | raw }}

        <!-- Insert code before the closing body tag  -->
        {{ view.render('footer') | raw }}

    </body>
</html>
```

As you can see, you can use basic Twig syntax for outputting escaped content (`{{ ... }}`) and non-escaped output (`{{ ... | raw }}`). Default control structures like `if` and `for` are available, for details please look at the [Twig syntax](http://twig.sensiolabs.org/doc/templates.html) documentation.

Pagekit's view helpers and functions are exposed via the view renderer, which is available as `view` inside the template. PHP expressions can simply be translated to Twig syntax, for example for adding a CSS file to the current page:

```html
<!-- PHP version -->
<?php $view->style('theme', 'theme:css/theme.css'); ?>

<!-- Twig version -->
{{ view.style('theme', 'theme:css/theme.css') }}
```

