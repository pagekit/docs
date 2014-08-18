# Server configuration

<p class="uk-article-lead">Pagekit will run on all common web servers. Here's how it works with the most popular ones.</p>

## Apache 2.2+

With Apache, all necessary configuration comes with the included `.htaccess` file. 

Make sure `mod_rewrite` is loaded for pretty URLs. If the module is not available, Pagekit will still work but fall back to a URL format of the form `http://example.com/index.php/page/welcome`.

## nginx

With Nginx, connect [PHP to Nginx](http://wiki.nginx.org/PHPFcgiExample) and add the following to your config file to support nice URLs.

```nginx
location / {
    try_files $uri $uri/ /index.php?$args;
}
```
