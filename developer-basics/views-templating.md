# Views &amp; Templating
<p class="uk-article-lead">While the controller handles the incoming request, the view is responsible for rendering the response. To achieve this, it utilizes a templating engine. Currently, Pagekit only supports a PHP engine.</p>

<ul class="uk-list">
    <li><a href="#rendered-view-response">Rendered view response</a></li>
    <li><a href="#render-a-view-manually">Render a view manually</a></li>
    <li><a href="#templating">Templating</a></li>
    <li><a href="#working-with-assets">Working with Assets</a></li>
</ul>

## Rendered view response

The most common way to render a view is to return an array from your controller action. Use the `'$view'` property to pass parameters to your view renderer.

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
<!-- packages/hello/views/index.php -->

<h1>Hello <?= $name ?></h1>
<p>
   ...
</p>
```


This view is wrapped in the main layout by default. To avoid this behavior, you can change `'layout' => false` in the $view array.

## Render a view manually

You can also manually access the `View` service to render a template file. This may come in handy, if you dynamically determine which view to load. Note that `hello` in the example below refers to the package name.

```php

use Pagekit\Application as App;

class MyController {

    public function anotherViewAction()
    {
        return App::render('hello/views/view.php', ['id' => 1]);
    }

}
```

The according view file:

```HTML
<!-- packages/hello/views/index.php -->

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
$view->render('hello/views/view.php', ['id' => 1])
```

### Link to routes
As seen earlier, each route has a name that you can use to dynamically generate links to the specific route. There is a `Url` helper that exposes the functions of the `UrlProvider`.

```HTML
<a href="<?= $view->url('@hello/default/view') ?>">View all articles</a>
<a href="<?= $view->url('@hello/default/view', ['id' => 23]) ?>">View article 23</a>
```

You can link to assets, like images or other files, using `@url.to`.

```HTML
<img src="<?= $view->url()->getStatic('hello/extension.svg') ?>" alt="Extension icon" />
```

## Working with Assets
Assets are the static files needed in your project, including CSS, JS and image files.

### Generate URL to static assets
To generate a route to a static asset, use the `getStatic` method of the `UrlProvider` class.

```php
<img src="<?= $view->url()->getStatic('my-assets/image.jpg') ?>">
```

### Include CSS in view file
To include a CSS file in your view, call the `style helper` from the `View`. You can define its dependencies through the second parameter.

```
<?php $view->style('theme', 'theme:css/theme.css') ?>
<?php $view->style('theme', 'theme:css/theme.css', 'uikit') ?>
<?php $view->style('theme', 'theme:css/theme.css', ['uikit', 'somethingelse']) ?>
```

**Note** This will not directly output the required line in HTML. Instead, it will add the CSS file to the Pagekit Asset Manager. The stylesheet will be included in the `<head>` section of your theme.

### Include JS in view file
To include a JavaScript file in your template, call the `script helper` from the `View`. You can define its dependencies through the second parameter.

```
<?php $view->script('theme', 'theme:js/theme.js') ?>
<?php $view->script('theme', 'theme:js/theme.js', 'jquery') ?>
<?php $view->script('theme', 'theme:js/theme.js', ['jquery', 'uikit']) ?>
```

**Note** Internally, `style()` and `script()` each work with their own Asset Manager. As these are separate from each other, you can assign the same name to a CSS and JS file without conflict (both called `theme` in the above example).

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


