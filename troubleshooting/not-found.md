# Not Found

There are a couple of reasons you might receive a **Not Found** error, and they are each caused by different factors.

**Note** The examples below are for the Apache Web Server which is the most common server software used.

### Missing .htaccess File

The first thing to check is if you have the provided `.htaccess` file at the root of your Pagekit installation. Because this is a **hidden** file, you won't normally see it in your explorer or finder windows. If you have extracted Pagekit then **selected** and **moved** or **copied** the files, you may well have left this very important file behind.

It is **strongly advised** to unzip Pagekit and move the **entire folder** into place, then simply rename the folder. This will ensure all the files retain their proper positions.

### AllowOverride All

In order for the Pagekit-provided `.htaccess` to be able to set the rewrite rules required for routing to work properly, Apache needs to first read the file. when your `<Directory>` or `<VirtualHost>` directive is setup with `AllowOverride None`, the `.htaccess` file is completely ignored. The simplest solution is to change this to `AllowOverride All`.

More details on `AllowOverride` and all the possible configuration options can be found in the [Apache Documentation](http://httpd.apache.org/docs/2.4/mod/core.html#allowoverride).

### RewriteBase Issue

If the homepage of your Pagekit site loads, but **any other page** displays this very rough _Apache-style_ error, then the most likely cause is that there is a problem with your `.htaccess` file.

The default `.htaccess` that comes bundled with Pagekit works fine out-of-the-box in most cases. However, there are certain setups involving virtual hosts where the file system does not match the virtual hosting setup directly. In these cases you must configure the `RewriteBase` option in the `.htaccess` to point to the correct path.

There is a short explanation of this in the `.htaccess` file itself:

```
##
# If you are getting 404 errors on subpages, you may have to uncomment the RewriteBase entry
# You should change the '/' to your appropriate subfolder. For example if you have
# your Pagekit install at the root of your site '/' should work, else it might be something
# along the lines of: RewriteBase /<your_sub_folder>
##

# RewriteBase /
```

Simply remove the `#` before the `RewriteBase /` directive to uncomment it, and adjust the path to match your server environment.

### Missing Rewrite or Headers Modules

Some webserver packages do not come with the Apache **rewrite** or **headers** modules enabled by default. They usually can be enabled from the configuration settings for Apache, or you can do so manually via the `httpd.conf` by uncommenting these lines so they are loaded by Apache and restarting your Apache server.

```
#LoadModule rewrite_module modules/mod_rewrite.so
#LoadModule headers_module modules/mod_headers.so
```

### Error 404 Page

If you receive a _Pagekit-style_ error saying **Error 404** then your `.htaccess` is functioning correctly, but your trying to reach a page that Pagekit cannot find. The most common cause of this is simply that the page has been moved or renamed.
