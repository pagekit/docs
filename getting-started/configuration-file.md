# Configuration File

<p class="uk-article-lead">Change settings in Pagekit's configuration file.</p>

Pagekit's configuration is stored in `config.php` in the Pagekit root folder. Here you will find the database credentials, debug and cache settings. You can configure the settings in Pagekit *System > Settings* or edit them in this config file.

## Database

The database connection settings are stored in `database['connections']`. Here you can find the connection data you have provided during the installation process:

```php
'database' => [
  'default' => 'mysql',                     // default database connection
  'connections' => [                        // array of database connections
    'mysql' => [                            // database connection name
      'host' => 'localhost',                // database server host name
      'user' => 'user',                     // database server user name
      'password' => 'pass',                 // database password
      'dbname' => 'pagekit',                // database name
      'prefix' => 'pk_'                     // database prefix
    ],
    'sqlite' => [                           // database connection name
      'prefix' => 'pk_'                     // database prefix
    ]
  ]
]
```

**Note** As reflected above is possible to configure more than one database connection and set the default one in `database['default']`.

## System

```php
'system' => [
  'secret' => 'secret'                      // the secret string generated during installation
]
```

## Cache

In `cache` section the caching method is stored. Initially the cache is set to `auto` and you can disable it entirely by setting the `nocache' option as `true`.

```php
'system/cache' => [
  'caches' => [
    'cache' => [
      'storage' => 'auto'                   // The cache method to be used
    ]
  ],
  'nocache' => false                        // the cache state. Remember to keep it enabled on production server
]
```

## Finder

```php
'system/finder' => [
  'storage' => '/storage'                   // the site storage folder path
]
```

## Application

```php
'application' => [
  'debug' => false                          // enable debug mode while developing to get debug output
]
```

## Debug

Enabling the debug toolbar will give you valuable information while developing, the toolbar is located at the bottom of the page. It collects data about the requests that are made, monitors the memory usage, counts database queries and so on.

```php
'debug' => [
  'enabled' => false                        // the debug toolbar state
]
```
