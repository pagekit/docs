# Configuration File

Pagekit's configuration is stored in `config.php` in the Pagekit root folder. Here you will find the database credentials, debug, profiler and cache settings. The configuration is stored in a PHP Array.


## Database

The database connection settings are stored in `database['connections']`. Here you can find the connection data  you have provided during the installation process:

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

**Note** It is also possible to configure more than one database connection.
The default connection is defined in `database['default']`. In the example above Pagekit uses `mysql`.

## App

In the `app` section you can enable the debug output or disable the cache. This is useful when you are developing a theme or extension for Pagekit.

| Field | Description |
|-------|-------------|
| `key` | A unique key, used for encryption. |
| `debug` | Enable debug mode if you are a developer to get debug output. |
| `nocache` | Disabling the cache can be useful in a development environment. Remember to enable it on the production server. |

This is what a production configuration might look like:
```php
'app' =>
  array (
    'key' => '66235f24939aa374932d09bd2805fdb93d064d6d',
    'debug' => '0',
    'nocache' => '0',
  ),
```


## Cache

Pagekit offers two different caching methods `apc` and `file`. Initially the cache is set to `auto` so Pagekit will select the caching method automatically.

| Field | Description |
|-------|-------------|
| `storage` | Configure which cache system should be used by Pagekit. The possible values are `auto`, `file` and `apc`. |


In this example the caching method is selected automatically:
```php
'cache' =>
  array (
    'storage' => 'auto',
  ),
```


## Profiler

If you are a developer, you'll want to enable the profiler. It collects data about the requests that are made, monitors the memory usage, counts database queries and so on. This is useful when debugging your code.
When enabled, you will see the profiler toolbar at the bottom of the page.

| Field | Description |
|-------|-------------|
| `enabled` | Enable or disable the profiler. |

In this example the profiler is disabled:
```php
'profiler' =>
  array (
    'enabled' => '0',
  ),
```
