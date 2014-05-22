# View and Response

In most cases, the return value of a controller action will be a rendered view.
Alternatively, you might want to return JSON data, redirect to a different page
or return an error page. These tasks can be achieved with the View and Response
services.

## Render a view

Use the `@View` annotation to determine which view file should be rendered.
The action method returns an array with data the template engine will use to
render the view file.  Note how `head.title` is a special value used to render
as the page's `<title>`.

Render `<extensions>/hello/views/index.razr.php`:

```PHP
/**
 * @View("hello/view.razr.php")
 */
public function viewAction($id=1)
{
    return array('head.title' => __('View article'), 'id' => $id);
}
```

You can also manually access the View service to render a template file. This
may come in handy when you dynamically determine which view to load.


```PHP
public function anotherViewAction()
{
    $view = 'hello/view.razr.php';
    $data = array('head.title' => __('View article'), 'id' => 1);
    return $this('view')->render($view, $data);
}
```

The according view file utilizes the [Razr templating language](https://github.com/pagekit/razr).


```HTML
<!-- extensions/hello/views/index.razr.php -->

<h1>You are viewing article number @id</h1>
<p>
   ...
</p>

```

### View layout

By default, views are rendered in a surrounding layout, usually defined
by the theme. That is why the above examples don't just generate two lines of
output but a complete HTML document. To disable this, you can set the
`layout` parameter to `false`.

| Method                       | Example                                      |
|------------------------------|----------------------------------------------|
| Using the annotation         | `@View("hello/index.razr.php", layout=false)`|
| Using the `view` service     | `$this('view')->setLayout(false);`           |

Or provide a different layout `$this('view')->setLayout('hello/theme.razr.php');`

View file `<extensions>/hello/views/theme.razr.php`:

```HTML
<h1>This is the main theme</h1>

<section>
    @action('content')
</section>
```

## Templating

The views are rendered using the [Razr templating engine](https://github.com/pagekit/razr)
which offers output functionality and control structures. Pagekit exends the Razr language with a few features you can use in your
templates.

### Link to routes

As seen earlier, each route has a name that you can use to dynamically generate
links to the specific route. This is also possible inside the Razr template.

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

## Response

### Redirect

Use `redirect($url, $parameters = array(), $status = 302, $headers = array())`
to redirect from a controller action.

```PHP
function redirectAction()
{
    return $this('response')->redirect('@hello/greet/name', ['name' => 'Someone']);
}
```

In case your controller extends `Pagekit\Framework\Controller\Controller`, you
can directly access the `redirect` method.

```PHP
public function redirectAction()
{
    return $this->redirect('@hello/greet/name', ['name' => 'Someone']);
}
```

### Return JSON

Returns a JSON representation of any object.

```PHP
public function jsonAction()
{
    $data = array('error' => true, 'message' => 'There is nothing here. Move along.');
    return $this('response')->json($data);
}

```

### Custom response and error pages

Using `create($content = '', $status = 200, $headers = array())` you
can return any custom HTTP response.

```PHP
function forbiddenAction()
{
    return $this('response')->create('Permission denied.', 401);
}
```

### Download

Using `download($file, $name = null, $headers = array())` you can link to a file
to be downloaded. Sets `Content-Disposition: attachment` to force
a *Save as* dialog in most browsers.

```PHP
public function downloadAction()
{
    return $this('response')->download('extensions/hello/extension.svg');
}
```
