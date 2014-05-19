# Configuration File

Pagekit's configuration is stored in `config.php` in the Pagekit root folder. Here you will find the database credentials, debug, profiler and cache settings.

You can configure the settings in Pagekit *Settings > System* or edit them in this config file.


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
| `key` | A unique key that is used for encryption. |
| `debug` | Enable debug mode if you are a developer to get debug output. |
| `nocache` | Disabling the cache can be useful in a development environment. Remember to enable it on the production server. |

```php
'app' =>
  array (
    'key' => '66235f24939aa374932d09bd2805fdb93d064d6d',
    'debug' => '0',
    'nocache' => '0',
  ),
```

## Cache

In `cache` section the configured caching method is stored. Initially the cache is set to `auto`.

| Setting | Description |
|---------|-------------|
| `auto` | Pagekit will automatically select the best caching method that is available. |
| `apc` | Set the caching method to use [APC](http://www.php.net/manual/de/book.apc.php) |
| `file` | Set the caching method to store caching data in a file in '/app/cache' |


```php
'cache' => array (
    'caches' => array (
        'main' => array (
            'storage' => 'auto'
        )
    )
),
```

## Profiler

Enabling the profiler will give you valuable information as a developer. It collects data about the requests that are made, monitors the memory usage, counts database queries and so on. This is useful when debugging and profiling your code.
When enabled, the profiler toolbar is located at the bottom of the page.

| Field | Description |
|-------|-------------|
| `enabled` | Enable or disable the profiler. |

```php
'profiler' =>
  array (
    'enabled' => '0',
  ),
```
