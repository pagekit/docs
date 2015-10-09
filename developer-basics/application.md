# Application
<p class="uk-article-lead">The Application functions as Pagekit's dependency container. The application makes Pagekit's functionality and services configurable, extendable, interchangeable and accessible throughout the [modules](modules.md).</p>

All services you have available in Pagekit are set as dependency-injected properties on the `Application` instance. `$app['db']` for example will give you access to the database service.

## Accessing a Service
To access the `Application` instance, there are mainly two ways. Depending on the context you are in at the moment, you have either access to a `$app` variable or through a static call to the `Pagekit\Application` class.

```php
// Getter
$app['cache']

use Pagekit\Application as App;
App::cache();
```

As you can see, the container implements `\ArrayAccess` as well as a magic `__call` method to access the container's services.

## Defining a Service
Adding a service to the application can easily be achieved by setting an array key on the container to be a closure. This will not be evaluated until accessed for the first time.

```php
$app['cache'] = function () {
    return new Cache();
};
```
