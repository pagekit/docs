# Server Configuration
<p class="uk-article-lead">Pagekit is written in PHP and can run on several web server configurations. Official support exists for Apache 2.2+ and nginx.</p>

## Apache 2.2+
Although Pagekit should run fine on Apache 2.2+ without additional configuration, during installation you may receive a warning message. If you do, you should verify that the `.htaccess` file is present in the root of your Pagekit folder.

**Note** The `.htaccess` file is a Apache configuration file and it is hidden on Unix-based systems; as such it is easy to miss when uploading the package initially. If it is not present, copy it from the Pagekit package.

It is possible as well that your webserver does not allow the server's configuration to be overridden through an `.htaccess` file. In that case, contact your hosting provider and ask them to change the AllowOverride directive.

Another common problem is that the `mod_rewrite` module is not enabled on your webserver, in which case you'll also have to turn to your hosting provider to have them enable this Apache module. If the module is not available, Pagekit will still work but fall back to a URL format of the form `http://example.com/index.php/page/welcome`.

## nginx

With Nginx, connect [PHP to Nginx](http://wiki.nginx.org/PHPFcgiExample). Update your nginx config according to the [basic example configuration](https://gist.github.com/xorinzor/a5b68445ac34bc9078a434d2a534731f). Please note that out of the box, the Apache solution provides more features from its configuration, such as compression and cache headers for assets. These are currently not included in the nginx configuration.

If you have trouble with pretty URLs, there is an extension to [enforce pretty URLs](https://pagekit.com/marketplace/package/tobbe/enforce-modrewrite) on your site. Before you install that extension, make sure that your installation successfully resolves pretty URLs. Otherwise you will lock yourself out of the admin area and have to disable the extension in the database directly.
