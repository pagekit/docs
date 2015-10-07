# Configuration File

<p class="uk-article-lead">Change settings in Pagekit's configuration file.</p>

Pagekit stores parts of its configuration in a file located at the root level - `/config.php`. It is auto generated during the installation. The normal way of editing it would be through Pagekit *System > Settings* admin area, although there are situations where the manual editing is necessary. The following sections describe the most common settings.

```php
'database' => [
  'default' => 'mysql',                     // default database connection
  'connections' => [                        // array of database connections
    'mysql' => [                            // database driver name, mysql or sqlite
      'host' => 'localhost',                // database server host name
      'user' => 'user',                     // database server user name
      'password' => 'pass',                 // database password
      'dbname' => 'pagekit',                // database name
      'prefix' => 'pk_'                     // database prefix
    ]
  ]
],
'system' => [
  'secret' => 'secret'                      // the secret string generated during installation
],
'system/cache' => [
  'caches' => [
    'cache' => [
      'storage' => 'auto'                   // the cache method to be used, if enabled
    ]
  ],
  'nocache' => false                        // the cache state - you can disable it entirely by setting to 'true'
],
'system/finder' => [
  'storage' => '/storage'                   // the relative path to the storage folder - e.g. media files will be stored here
],
'application' => [
  'debug' => false                          // the debug mode state - enable while developing to get debug output
],
'debug' => [
  'enabled' => false                        // the debug toolbar state - enable it to get information, among others, about requests, memory usage
]
```
