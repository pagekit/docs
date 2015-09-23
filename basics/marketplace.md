# Marketplace

<p class="uk-article-lead">An extension or theme in the marketplace will be installable from any Pagekit admin interface.</p>

## Get an API key

Log in on the [Pagekit website](http://pagekit.com) and grab your *API key* from the account settings.
Navigate to your local Pagekit installation and paste the key in the
according textbox in *Settings > System*.

## Create a package

Before we can upload our extension for the first time, we have to create
a package in the marketplace.

Log in on pagekit.com and navigate to the *Developer* section. Hit the
*New Package* button and enter your details.

| Field  | Description |
|--------|-------------|
| *Name*           | The extension name. Must match your local extension name. Unique across the marketplace. |
| *Title*          | The extension title. |
| *Package type*   | It can be an extension or theme. |

Hit *Create* and you will be presented with a settings screen for your new
package. Leave this as it is, but notice how the *Status* of the package is set
to *Unpublished* by default. We'll get back to this after uploading our
extension.

## Upload

From the command line, use the `pagekit` command line tool to upload your
extension. In this example `foobar` is the extension name.

```bash
./pagekit extension:upload foobar
```

**Note** If the command fails with `Error: package not found`, please make sure the
extension name matches the package name in the marketplace.

## Alternative upload

If, for any reason, you prefer not to use the command line tool you can also upload your extension manually.

1. Simply *zip* the contents of your extension folder.
2. Login to [pagekit.com](http://pagekit.com), navigate to your extension and select the *Releases* tab.
3. Upload the ZIP file and a new, but still unpublished release will be created automatically.

**Note** Be careful to have your files on the root level of the zip file or the file will not be considered a valid package.

## Releases

When an extension has been uploaded you still have to release it before it can be accessed from the marketplace.

1. Login to [pagekit.com](http://pagekit.com), navigate to your extension and select the *Releases* tab (refresh the page if the needed).
2. A newly generated release *0.0.1* is available but still marked
as *Draft*. This version number is taken from the extension's `extension.json` or the theme's `theme.json`.
3. Hit the *Publish* button next to the release to make the release
available to the public.
4. Switch to the *Settings* tab and set the *Status* of your package to *Published* as well.

Congratulations, your extension is now part of the Pagekit marketplace.

## Versions

When you have finished a new version of your extension and try to upload it
to the marketplace, make sure to increase the *version* number in the
`extension.json` (or `theme.json`) file. Otherwise you will be presented with a
failure telling you that your uploaded version does already exist.

Version numbers must comply to a certain naming scheme like explained in the
[PHP docs](http://php.net/version_compare). A simplified (and
incomplete) regex for a valid version number is (with x being a number):

```
x.x.x(-(pre|beta|RC|alpha))?x?
```

This is what correctly applied version numbers could look like: `0.0.1`, `1.0.0`, `0.1.2-alpha`, `0.1.2-alpha-2`, `0.1.2-RC2`.
