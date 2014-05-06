# Configuration File

Pagekit's configuration is stored in `config.php` in the Pagekit root folder. Here, you will find the database credentials, debug, profiler and cache settings and so on.

If you have to change, say the database connection settings because the database server has changed, you can do it in this file. The configuration is stored in a PHP Array.


## Database

The database connection settings are stored in `database['connections']`. You will see the connection data that you have provided during the installation process here.

Example:
```php
'database' =>
  array (
    'default' => 'mysql',
    'connections' =>
    array (
      'mysql' =>
      array (
        'host' => 'localhost',
        'user' => 'user',
        'password' => 'pass',
        'dbname' => 'pagekit',
        'prefix' => 'pk_',
      ),
    ),
  ),
```

It is also possible to configure more than one database connection.
The default connection is then defined in `database['default']`, in the example above `mysql` is the connection Pagekit uses.

## App

In the `app` section you can enable the debug output or disable the cache. This is useful when you are developing a theme or extension for Pagekit.

| Field | Description |
|-------|-------------|
| `key` | A unique key, used for encryption. |
| `debug` | Enable debug mode if you are a developer to get debug output. |
| `nocache` | Disabling the cache can be useful in a development environment. Remember to enable it on the production server. |

Example:
```php
'app' =>
  array (
    'key' => '66235f24939aa374932d09bd2805fdb93d064d6d',
    'debug' => '0',
    'nocache' => '0',
  ),
```


## Cache

Pagekit offers two different caching methods *APC* and *File*. Initially the cache is set to `auto` so Pagekit will select the caching method automatically.

| Field | Description |
|-------|-------------|
| `storage` | Configure which cache system should be used by Pagekit. The possible values are `auto`, `file` and `apc`. |

Example:
```php
'cache' =>
  array (
    'storage' => 'auto',
  ),
```


## Profiler

If you are a developer, you'll want to enable the profiler. It collects data about the requests that are made, monitors the memory usage, counts database queries and so on. This is useful when debugging your code.
When enabled, you will see the profiler-toolbar at the bottom of the page.

| Field | Description |
|-------|-------------|
| `enabled` | Enable or disable the profiler. |

Example:
```php
'profiler' =>
  array (
    'enabled' => '0',
  ),
```
