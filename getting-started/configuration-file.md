# Configuration File
<p class="uk-article-lead">The Pagekit configuration file is automatically created when you install Pagekit. If you want to change configuration settings manually, this article explains syntax and content of the file.</p>

Usually, you do not need to fiddle with the configuration file `config.php` after it has been created by the installer. The normal way of changing configuration is through _System > Settings_ in the Pagekit admin panel.

Sometimes manually editing this file is still necessary and useful, for example when troubleshooting a broken installation or when moving an existing Pagekit installation to a new server.

In the following code listing you see an example configuration with the most common settings.

Usually you only have one database connection present. The example includes both examples to show how configuration works for different database drivers. Only the `default` connection will be used by Pagekit (in this example `sqlite` is used).

```php
'database' => [
  'default' => 'sqlite',     // default database connection
  'connections' => [         // array of database connections
    'sqlite' => [            // database driver name, here: sqlite
      'prefix' => 'pk_',     // prefix in front of every table
    ],
    'mysql' => [             // database driver name, here: mysql
      'host' => 'localhost', // server host name
      'user' => 'user',      // server user name
      'password' => 'pass',  // server user password
      'dbname' => 'pagekit', // database name
      'prefix' => 'pk_'      // prefix in front of every table
    ],
  ]
],
'system' => [
  'secret' => 'secret'       // a secret string generated during installation
],
'system/cache' => [
  'caches' => [
    'cache' => [
      'storage' => 'auto'    // the cache method to be used, if enabled
    ]
  ],
  'nocache' => false         // the cache state - disable entirely by setting to true
],
'system/finder' => [
  'storage' => '/storage'    // relative path to a folder used for uploads, cache etc.
],
'application' => [
  'debug' => false           // debug mode state, enable while developing to get debug output
],
'debug' => [
  'enabled' => false         // debug toolbar state, enable to get information, about requests, routes etc.
]
```
