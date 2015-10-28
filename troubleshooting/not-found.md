# Not Found
A **Not Found** error can be caused by different reasons.

## Missing .htaccess File
Verify that the `.htaccess` file is present in the root of your Pagekit folder. The `.htaccess` file is a Apache configuration file and it's hidden on Unix-based systems; as such it may not have been unpacked. If it is not present, copy it from the Pagekit package.

It is possible as well that your web server does not allow the server's configuration to be overridden through an `.htaccess` file. In that case, contact your hosting provider and ask them to change the AllowOverride directive.

## Missing Rewrite Module
Another common problem is that the `mod_rewrite` module is not enabled on your web server, in which case you'll also have to turn to your hosting provider to have them enable this Apache module. If the module is not available, Pagekit will still work but fall back to a URL format of the form `http://example.com/index.php/page/welcome`.

## RewriteBase Issue
If you are having trouble displaying pages other than the homepage of your Pagekit site. Your site might be running in a VirutalDocumentRoot.

In most cases the provided `.htaccess` works fine. However, if your filesystem does not match with the virtual hosting setup. You'll have to edit the `RewriteBase` option in the `.htaccess` file to match your server environment.

```
# Set base if your site is running in a VirtualDocumentRoot
# RewriteBase /
```

Uncomment the `RewriteBase /` option by removing the `#` in front of it.

## Error 404 Page
If you receive a Pagekit styled error saying **Error 404** then your `.htaccess` is functioning correctly, but your trying to reach a page that Pagekit cannot find. The most common cause of this is simply that the page has been moved or renamed.
