# Packages

<p class="uk-article-lead">A Package is Pagekit's concept for everything that can be installed in your system, most prominently extensions and themes.</p>

## Package location

All of your packages are located in the `/packages` directory, sorted in subdirectories according to their vendor.

Every package belongs to a specific vendor (for example `pagekit` for all official packages, including the blog package and the *Hello* theme and *Hello* extension).

The vendor name is a unique representation of a developer or organization. In the simplest case, it just matches a github username.

The directory name is up to you. The actual name of the package is defined inside the package and not by the directory name. However, it makes sense to give a clear name to the folder.

Although not mandatory, it is a good convention to prefix a package by its type. For example `theme-hello` for the *Hello* theme and `extension-hello` for the *Hello* extension.

## Package content

A package contains at least two files.

1. The `composer.json` contains the metadata for your package and therefore acts as the package definition.
2. The `index.php` is a so called Module definition and is used to add any actual functionality to Pagekit.

The rest of the package content depends on the package's `type`. A `pagekit-theme` will contain other files than a `pagekit-extension`.

To learn more about the actual content of a package, check out the [Theme Guide](../guides/guide-theme.md) or the [Extension Guide](../guidesguide-extension.md).

## Package definition: composer.json

A package is defined by its `composer.json`. This file includes the package name, potential dependencies to be installed by Composer and other information that displays in the Pagekit marketplace.

For a theme, this file can look as follows.

```json
{
    "name": "pagekit/theme-hello",
    "type": "pagekit-theme",
    "version": "0.9.0",
    "title": "Hello",
    "description": "A blueprint to develop your own themes.",
    "license": "MIT",
    "authors": [
        {
            "name": "Pagekit",
            "email": "info@pagekit.com",
            "homepage": "http://pagekit.com"
        }
    ],
    "extra": {
        "image": "image.jpg"
    }
}
```


## Installation Hooks

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

