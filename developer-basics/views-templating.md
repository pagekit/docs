# Views and Templating
<p class="uk-article-lead">While the controller is handling the incoming request, the view is responsible for rendering the response. To achieve this, it utilizes a templating engine. Currently Pagekit supports a PHP engine only.</p>

## Render a view
There are two ways of rendering a view. The most common would be to return a [Rendered View](developer-basics/response.md#rendered-view) from your controller action.

You can also manually access the View service to render a template file. This may come in handy when you dynamically determine which view to load.

```php

use Pagekit\Application as App;

public function anotherViewAction()
{
    return App::render('extension://hello/views/view.php', ['id' => 1]);
}
```

The according view file:

```HTML
<!-- extensions/hello/views/index.php -->

<h1>You are viewing article number <?= $id ?></h1>
<p>
   ...
</p>
```

## Templating
The views are rendered using the PHP templating engine, which offers defined global template variables and a set of view helpers.

### Including other views
Rendering sub views from your view is done with the `$view` helper. The `render`method evaluates and returns the content of the given template file. This is identical to rendering the view manually from the controller.

```php
$view->render('extension://hello/views/view.php', ['id' => 1])
```

### Link to routes
As seen earlier, each route has a name that you can use to dynamically generate links to the specific route. There is a `Url` helper that exposes the functions of the `UrlProvider`.

```HTML
<a href="<?= $view->url('@hello/default/view') ?>">View all articles</a>
<a href="<?= $view->url('@hello/default/view', ['id' => 23]) ?>">View article 23</a>
```

You can link to assets like images or other files using `@url.to`.

```HTML
<img src="<?= $view->url()->getStatic('extensions/hello/extension.svg') ?>" alt="Extension icon" />
```

## Working with Assets
<p class="uk-article-lead">Assets are the static files needed in your project, including CSS, JS and image files.

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
