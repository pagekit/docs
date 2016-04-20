# Server Configuration
<p class="uk-article-lead">Pagekit is written in PHP and can run on several web server configurations. Official support exists for Apache 2.2+ and nginx.</p>

## Apache 2.2+
Although Pagekit should run fine on Apache 2.2+ without additional configuration, during installation you may receive a warning message. If you do, you should verify that the `.htaccess` file is present in the root of your Pagekit folder.

**Note** The `.htaccess` file is a Apache configuration file and it is hidden on Unix-based systems; as such it is easy to miss when uploading the package initially. If it is not present, copy it from the Pagekit package.

It is possible as well that your webserver does not allow the server's configuration to be overridden through an `.htaccess` file. In that case, contact your hosting provider and ask them to change the AllowOverride directive.

Another common problem is that the `mod_rewrite` module is not enabled on your webserver, in which case you'll also have to turn to your hosting provider to have them enable this Apache module. If the module is not available, Pagekit will still work but fall back to a URL format of the form `http://example.com/index.php/page/welcome`.

## nginx

With Nginx, connect [PHP to Nginx](http://wiki.nginx.org/PHPFcgiExample). The following two sections explain how to enable pretty URLs and pretty URL generation. For other user contributed nginx configuration, please refer to [Github issue #26](https://github.com/pagekit/pagekit/issues/62).

### Enable pretty URLs

To support pretty URLs which do not contain `index.php`, add the following to your nginx configuration.


```nginx
location / {
    try_files $uri $uri/ /index.php?$args;
}
```

An example of a nginx server configuration looks like follows. Make sure to replace the default port, host and the paths referencing log and config files. The file might look look different on your server depending on what else you have configured.

```nginx
server {
    listen       8080;
    server_name  localhost;
    root         /var/www/;

    access_log  /usr/local/etc/nginx/logs/default.access.log  main;

    location / {
        include   /usr/local/etc/nginx/conf.d/php-fpm;
        try_files $uri $uri/ /index.php?$args;
    }

}
```

### Make sure Pagekit generates pretty URLs

With the above configuration, pretty URLs will work already. However, Pagekit will still generate URLs including the filename `index.php`, for example `example.com/index.php/blog`.

To make sure Pagekit generates pretty URLs, e.g `example.com/blog`, you can tell nginx to pretend it has `mod_rewrite` enabled. Add the following line to your FastCGI configuration.

```
fastcgi_param  HTTP_MOD_REWRITE  On;
```

An example of a full `php-fpm` configuration looks as follows. It might look look different on your server depending on what else you have configured.

```nginx
location ~ \.php$ {
    try_files      $uri = 404;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    fastcgi_param  SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param  HTTP_MOD_REWRITE  On;
    include        fastcgi_params;
}
```

