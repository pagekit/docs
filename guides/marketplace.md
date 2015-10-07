# Publish a Package at the Marketplace

<p class="uk-article-lead">An extension or theme in the marketplace will be installable from any Pagekit admin interface.</p>

## Create a Account at pagekit.com

Before you can upload a package for the first time, you have to create a account at [pagekit.com](https://pagekit.com).

Log in on [pagekit.com](https://pagekit.com) and navigate to the *Developer* section. Hit the *Submit Package* button and upload your package.

**Note** Be careful to have your files on the root level of the zip file or the file will not be considered a valid package.

Hit *Submit* and you will be presented with a settings screen for your new
package. Notice how the *Status* of the package is set
to *Unpublished* by default.

## Synchronize Releases with Github



## Versions

When you have finished a new version of your extension and try to upload it
to the marketplace, make sure to increase the *version* number in the
`composer.json` file.

Version numbers must comply to a certain naming scheme like explained in the
[PHP docs](http://php.net/en/version_compare). A simplified (and
incomplete) regex for a valid version number is (with x being a number):

```
x.x.x(-(pre|beta|RC|alpha))?x?
```

This is what correctly applied version numbers could look like: `0.0.1`, `1.0.0`, `0.1.2-alpha`, `0.1.2-alpha-2`, `0.1.2-RC2`.
