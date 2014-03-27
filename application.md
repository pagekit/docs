# The application

All services you have available in Pagekit are set as dependency-injected
properties at the `Application` instance. `$app['db']` for example will give you
access to the database service.

To access the `Application` instance, there are several ways, depending on
the context you are in at the moment.

## Configuration context

When your extension (or theme) is loaded, Pagekit loads the `extension.php`
(or `theme.php`). Here, you can simply access the `Application` instance `$app`.
Accessing the cache service would look as follows.

```
$app['cache']
```

## Controller and extension context

When inside a controller or your extension code (to be exact, any class that
extends `Pagekit\Framework\Controller` or `Pagekit\Framework\Extension`), you
can access application properties by calling `$this` with the desired service
name.

```
$this('cache')
```

## Custom classes

To get access to the Application instance in one of your custom classes, extend
the `ApplicationAware` trait. This will pull in a static `$app` reference and
make it accessible by calling the self-reference. As a matter of fact, this
is exactly what happens for `Controller` and `Extension` in the background.

```PHP
namespace Pagekit\HelloExtension;

use Pagekit\Framework\ApplicationAware;

class MyClass extends ApplicationAware
{
  ...
}
```

Now, you can access the Application instance exactly like you do in controllers
and extension classes.


```
$this('cache')
```


The functionality of `Pagekit\Framework\ApplicationAware` is also available as a
trait `Pagekit\Framework\ApplicationTrait`, so you are not limited to
`extend`, as seen in the above example, but you can also insert the
functionality provided with the `use` keyword. **Attention**: This breaks
PHP 5.3 compatibility.

```PHP
namespace Pagekit\HelloExtension;

use Pagekit\Framework\ApplicationTrait;

class MyClass
{
  use ApplicationTrait;

  ...
}
```
