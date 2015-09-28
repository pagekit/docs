# Installation Hooks

<p class="uk-article-lead">A package (can be a extension or theme) can be either enabled, disabled or not installed. When changing the state, you might need to modify your database schema or run other custom code.</p>

## Custom Scripts File

All hooks are loaded from a custom scripts file which can be defined inside the `composer.json` file of the package:

```json
    "extra": {
        "scripts": "scripts.php"
    }
```

A custom scripts file has to return a Php array, containing callbacks:

```php
<?php

return [

    /*
     * Installation hook.
     */
    'install' => function ($app) {

    },

    /*
     * Enable hook.
     */
    'enable' => function ($app) {

    },

    /*
     * Uninstall hook.
     */
    'uninstall' => function ($app) {

    },

    /*
     * Runs all updates that are newer than the current version.
     */
    'updates' => [

        '0.5.0' => function ($app) {

        },

        '0.9.0' => function ($app) {

        },

    ],

];

```

## Install Hook

A install hook is executed after a package was *installed*.

## Enable Hook

A enable hook is executed after a package was *enabled*.

## Disable Hook

A disable hook is executed after a package was *disabled*.

Pagekit will not modify the tables you have created, even when your extension
gets *disabled* or *uninstalled* in the admin interface. You will have to take
care of needed database changes yourself.

## Uninstall Hook

A uninstall hook is executed after a package was *uninstalled*.

When your extension gets uninstalled, you should probably drop all tables and configuration values the
package has created.

## Updates

On enabling a package Pagekit checks if there are update hooks newer than the current version available.
If so they are executed sequentially.

