# Server configuration

<p class="uk-article-lead">Pagekit will run on all common web servers. Here's how it works with the most popular ones.</p>

## Apache 2.2+

With Apache, all necessary configuration comes with the included `.htaccess` file. 

Make sure `mod_rewrite` is loaded for pretty URLs. If the module is not availabe, Pagekit will still work but fall back to a URL format of the form `http://example.com/index.php/page/welcome`.

## nginx

Add the following rules to your server config when running Pagekit on nginx.

```
location / {
    root /var/www/;
    try_files $uri $uri/ /index.php?$args;
}
location ~ \.php$ {
    root /var/www/;
    try_files $uri $uri/ /index.php?$args;
    index index.html index.htm index.php;
    fastcgi_index index.php;
    fastcgi_param PATH_INFO $fastcgi_path_info;
    fastcgi_param PATH_TRANSLATED $document_root$fastcgi_path_info;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param HTTP_MOD_REWRITE On;
    fastcgi_pass unix:/var/run/php5-fpm.sock;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    include fastcgi_params;
}
```

