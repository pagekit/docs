# Packages
<p class="uk-article-lead">A package is Pagekit's concept for extending its functionality. Packages come in two different types: Extensions and Themes.</p>

<ul class="uk-list">
    <li><a href="#package-location">Package location</a></li>
    <li><a href="#package-content">Package content</a></li>
    <li><a href="#package-definition">Package definition</a></li>
    <li><a href="#installation-hooks">Installation hooks</a></li>
</ul>

## Package location
All packages reside in the `/packages` directory, sorted in subdirectories according to their vendor.

Every package belongs to a specific vendor &ndash; for example `pagekit` for all official packages, including the _Blog_ extension and the _One_ theme.

The vendor name is a unique representation of a developer or organization. In the simplest case, it just matches a GitHub username. The package name will also define the name of the directory it is stored in.

## Package content
A package contains at least two files.
1. The `composer.json` contains the metadata for your package and therefore acts as the package definition.
2. The `index.php` is a so called [Module definition](modules.md) and adds actual functionality to Pagekit.

The rest of the package content depends on the package's `type`. To learn more about the actual content of a package, check out the [Theme Guide](../developer-guides/themes.md) or the [Extension Guide](../developer-guides/extension.md).

## Package definition
A package is defined by its `composer.json`. This file includes the package name, potential dependencies to be installed by [Composer](https://getcomposer.org) and other information that displays in the Pagekit marketplace.

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

For more details on this file see the [Composer Documentation](https://getcomposer.org/doc/01-basic-usage.md).

## Installation hooks
A package can be either enabled, disabled or not installed. When changing the state, you might need to modify your database schema or run other custom code.

Pagekit offers installation hooks through a custom script file. This file needs to be defined in your Package definition, the `composer.json` file.

```json
    "extra": {
        "scripts": "scripts.php"
    }
```

A custom scripts file has to return a PHP array, containing callbacks.

```php
return [

    'install' => function ($app) {},
    'uninstall' => function ($app) {},
    'enable' => function ($app) {},
    'disable' => function ($app) {},
    'updates' => [

        '0.5.0' => function ($app) {},
        '0.9.0' => function ($app) {}

    ]
];
```

### Install
The install hook is executed after a package was _installed_.

### Uninstall
The uninstall hook is executed before a package was _uninstalled_.

Pagekit will not modify the tables you have created, even when your extension is _disabled_ or _uninstalled_ in the admin panel. You will have to take care of needed database changes yourself.

### Enable
The enable hook is executed after a package was _enabled_.

### Disable
The disable hook is executed before a package was _disabled_.

### Updates
Upon enabling a package, Pagekit checks if newer update hooks than the current version are available. If so, they are executed sequentially.
