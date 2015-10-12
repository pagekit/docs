# Response
<p class="uk-article-lead">The response represents the server's HTTP response to the client. There are a couple of different `Response` types, listed below, each with an example on how to build the response from the controller action:</p>

## String
To return a simple `String Response`, use the `response` service:

```php
public function indexAction()
{
    return $this['response']->create('My content');
}
```

## Rendered View
Pagekit can render the view and return the response for you. Simply return an array, with a key `$view` set to an array containing a _title_ and a view _name_.

All other parameters in the array, will be accessible in the view. Learn more about [Views and Templating](views-templating.md).

```php
public function indexAction($name = '')
{
    return [
        '$view' => [
            'title' => 'Hello World',
            'name' => 'hello:views/index.php',
        ],
        'name' => $name
    ];
}
```

If you don't want this to render a `Themed Response` as explained below, set a key `'layout' => false` in the `$view` array.

## Themed
A `Themed Response` embeds the controller's result within a surrounding layout, usually defined by the theme. Simply return a `String` or a  from the controller.

```php
public function indexAction()
{
    return 'My content';
}
```

## JSON
There are two ways to return a `JSON Response` from the controller:

If the action either returns an array or an object that implements \JsonSerializable. A `JsonResponse` will automatically be generated.

```php
public function jsonAction()
{
    return ['error' => true, 'message' => 'There is nothing here. Move along.'];
}
```

Of course, the `response` service can be used to achieve the same thing.

```php
public function jsonAction()
{    
    return $this['response']->json(['error' => true, 'message' => 'There is nothing here. Move along.']);
}
```

## Redirect
Use a Redirect Response to redirect the user.

```php
public function redirectAction()
{
    return $this['response']->redirect('@hello/greet/name', ['name' => 'Someone']);
}
```

## Custom response and error pages
Using `create` you can return any custom HTTP response.

```php
public function forbiddenAction()
{
    return $this['response']->create('Permission denied.', 401);
}
```

## Stream
The `Streamed Response` allows to stream the content back to the client. It takes an callback function as its first argument. Within that callback, a call to `flush` will be directly emitted to the client.

```php
public function streamAction()
{
    return $this['response']->stream(function() {
        echo 'Hello World';
        flush();
        echo 'Hello Pagekit';
        flush();
    });
}
```

## Download
The `Download Response` lets you send a file to the client. It sets `Content-Disposition: attachment` to force a _Save as_ dialog in most browsers.

```php
public function downloadAction()
{
    return $this['response']->download('extensions/hello/extension.svg');
}
```
