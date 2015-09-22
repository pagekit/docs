# Packages

<p class="uk-article-lead">A Package is Pagekit's concept for everything that
can be installed in your system, most prominently extensions and themes.</p>

## Package location

All of your packages are located in the `packages` directory, sorted in
subdirectories according to their vendor.

Every package belongs to a specific vendor (for example `pagekit` for all
official packages, including the blog package and the *Hello* theme and *Hello*
extension).

The vendor name is a unique representation of a developer or organization. In
the simplest case, it just matches a github username.

The directory name is up to you. The actual name of the package is defined
inside the package and not by the directory name. However, it makes sense to
give a clear name to the folder.

Although not mandatory, it is a good convention to prefix a package by its type.
For example `theme-hello` for the *Hello* theme and `extension-hello` for the
*Hello* extension.

## Package definition: composer.json

A package is defined by its `composer.json`. This file includes the package
name, potential dependencies to be installed by Composer and other information
that displays in the Pagekit marketplace.

For a theme, this file can look as follows.

```json
{
    "name": "pagekit/hello-theme",
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

The following properties are available.

## Package content

The package content depends on the package `type`. A `pagekit-theme` will
contain other files than a `pagekit-extension`.

However, a package usually contains at least a file called `index.php` which
defines the module that this package contains. A module can be both a theme and
an extension. It can also be a widget or some other type that is used by
Pagekit internally. For more general information check out the
[Modules](modules.md) chapter.

To learn more about the actual content of a package, check out the
[Theme Guide](guide-theme.md) or the [Extension Guide](guide-extension.md).
