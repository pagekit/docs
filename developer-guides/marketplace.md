# Publish a Package at the Marketplace
<p class="uk-article-lead">An extension or theme in the Marketplace can be installed directly from the Pagekit admin interface.</p>

## Create an account at pagekit.com
Before you can upload a package for the first time, you have to create an account at [pagekit.com](https://pagekit.com).

Log in on [pagekit.com](https://pagekit.com) and navigate to the _Developer_ section. Hit the _Submit Package_ button and upload your package.

**Note** Be careful to have your files on the root level of the zip file or the file will not be considered a valid package.

Hit _Submit_ and you will be presented with a settings screen for your new package. Notice how the _Status_ of the package is set to _Unpublished_ by default.

## Versions
When you have finished a new version of your extension and try to upload it to the marketplace, make sure to increase the _version_ number in the `composer.json` file.

Version numbers must comply to a certain naming scheme like explained in the [PHP docs](http://php.net/en/version_compare). A simplified (and incomplete) regex for a valid version number is (with x being a number):

```
x.x.x(-(pre|beta|RC|alpha))?x?
```

This is what correctly applied version numbers could look like: `0.0.1`, `1.0.0`, `0.1.2-alpha`, `0.1.2-alpha-2`, `0.1.2-RC2`.
