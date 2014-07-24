# Server configuration

<p class="uk-article-lead">Pagekit will run on all common web servers. Here's how it works with the most popular ones.</p>

## Apache 2.2+

With Apache, all necessary configuration comes with the included `.htaccess` file. 

Make sure `mod_rewrite` is loaded for pretty URLs. If the module is not availabe, Pagekit will still work but fall back to a URL format of the form `http://example.com/index.php/page/welcome`.

## nginx

Add the following rules to your server config when running Pagekit on nginx.

```
location / {
    try_files $uri $uri/ /index.php?q=$uri&$args;
}
location ~ \.php$ {
    root /var/www/;
    include /etc/nginx/fastcgi_params;
    fastcgi_pass unix:/var/run/php-fpm.socket;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param PATH_INFO $fastcgi_path_info;
    fastcgi_param REQUEST_URI $request_uri;
    fastcgi_param REMOTE_ADDR $remote_addr;
    fastcgi_param SCRIPT_NAME $fastcgi_script_name;
    fastcgi_param HTTP_MOD_REWRITE On;
    fastcgi_read_timeout 300;
    fastcgi_buffer_size 16k;
    fastcgi_buffers 4 16k;
}
```

