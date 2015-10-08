# Module config

<p class="uk-article-lead">Pagekit offers a simple way to store your modules configuration.</p>

## Default module config

A default config for your module is defined in your module's `index.php`. This can be any PHP array structure and also be nested.

```php
'config' => [

    'message' => 'Default message',

    'title'   => [
        'enabled' => true,
        'size'    => 10
    ]

],
```

## Read module config

To read the config of a module, you can access the `config` property of the module instance. This config is the result of both the default config stored inside the `index.php` and changes that are stored in the database.

```php
use Pagekit\Application;

// ...

$module = App::module('hello');
$config = $module->config;
```

## Write module config

To store changes for a module config, use the `config()` service.

```php
use Pagekit\Application;

// ...

App::config('hello')->set('message', 'Custom message');
```

**Note**. If you directly read the config from the module, it will still have the old value. After the next request, Pagekit will have merged the changes and made them available as the `config` property of the `$module` instance.
