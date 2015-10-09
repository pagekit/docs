# View and Templating
<p class="uk-article-lead">Render a view to the browser or send a custom response.</p>

In most cases, the return value of a controller action will be a rendered view. Alternatively, you might want to return JSON data, redirect to a different page or return an error page. These tasks can be achieved with the View and Response services.

## Render a view
Use the `@Response` annotation to determine which view file should be rendered. The action method returns an array with data the template engine will use to render the view file.  Note how `head.title` is a special value used to render as the page's `<title>`.

Render `<extensions>/hello/views/index.razr`:

```php
/**
 * @Response("hello/view.razr")
 */
public function viewAction($id=1)
{
    return ['head.title' => __('View article'), 'id' => $id];
}
```

You can also manually access the View service to render a template file. This may come in handy when you dynamically determine which view to load.

```php
public function anotherViewAction()
{
    $view = 'extension://hello/views/view.razr';
    $data = ['head.title' => __('View article'), 'id' => 1];
    return $this['view']->render($view, $data);
}
```

The according view file utilizes the [Razr templating language](https://github.com/pagekit/razr).

```HTML
<!-- extensions/hello/views/index.razr -->

<h1>You are viewing article number @id</h1>
<p>
   ...
</p>
```

### View layout
By default, views are rendered in a surrounding layout, usually defined by the theme. That is why the above examples don't just generate two lines of output but a complete HTML document. To disable this, you can set the `layout` parameter to `false`.

Method                   | Example
------------------------ | ---------------------------------------------
Using the annotation     | `@Response("hello/index.razr", layout=false)`
Using the `view` service | `$this['view']->setLayout(false);`

Or provide a different layout `$this['view']->setLayout('hello/theme.razr');`

View file `<extensions>/hello/views/theme.razr`:

```HTML
<h1>This is the main theme</h1>

<section>
    @section('content')
</section>
```

## Templating
The views are rendered using the [Razr templating engine](https://github.com/pagekit/razr) which offers output functionality and control structures. Pagekit extends the Razr language with a few features you can use in your templates.

### Link to routes
As seen earlier, each route has a name that you can use to dynamically generate links to the specific route. This is also possible inside the Razr template.

```HTML
<a href="@url.route('@hello/default/view')">View all articles</a>
<a href="@url.route('@hello/default/view', ['id' => 23])">View article 23</a>
```

You can link to assets like images or other files using `@url.to`.

```HTML
<img src="@url.to('extensions/hello/extension.svg')" alt="Extension icon" />
```

### Localized strings
To make a string translateable, wrap it in `@trans`.

```
@trans('Save')
```

Translateable pluralisation.

```
@transchoice('{0} No posts found|{1} %posts% post found|]1,Inf[ %posts% posts found', users|length, ['%posts%' => posts|length])
```

More about i18n (internationalization) in the [translation chapter](translation.md).
