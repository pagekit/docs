# Response
## Redirect
Use `redirect($url, $parameters = [], $status = 302, $headers = [])` to redirect from a controller action.

```php
function redirectAction()
{
    return $this['response']->redirect('@hello/greet/name', ['name' => 'Someone']);
}
```

In case your controller extends `Pagekit\Framework\Controller\Controller`, you can directly access the `redirect` method.

```php
public function redirectAction()
{
    return $this->redirect('@hello/greet/name', ['name' => 'Someone']);
}
```

## Return JSON
Return a JSON representation of any object using the `@Response("json")` annotation.

```php
@Response("json")
public function jsonAction()
{
    return ['error' => true, 'message' => 'There is nothing here. Move along.'];
}
```

Of course, you can manually use the `response` service to achieve the same thing.

```php
public function jsonAction()
{
    $data = ['error' => true, 'message' => 'There is nothing here. Move along.'];
    return $this['response']->json($data);
}
```

## Custom response and error pages
Using `create($content = '', $status = 200, $headers = [])` you can return any custom HTTP response.

```php
function forbiddenAction()
{
    return $this['response']->create('Permission denied.', 401);
}
```

## Download
Using `download($file, $name = null, $headers = [])` you can link to a file to be downloaded. Sets `Content-Disposition: attachment` to force a _Save as_ dialog in most browsers.

```php
public function downloadAction()
{
    return $this['response']->download('extensions/hello/extension.svg');
}
```
