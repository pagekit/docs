# Internal Server Error

<p class="uk-article-lead">There are errors not related to Pagekit that looks like the following.</p>

> The server encountered an internal error or misconfiguration and was unable to complete your request.

This error is usually triggered by a server misconfiguration (`httpd.conf`), `.htaccess` issues, `mod_security` or similar. You can indeed contact your server administrator but before you may make few tests.

## PHP execution

The first thing you should do is ensure PHP is working properly on your server, and Pagekit is not the direct cause of the issue. To test this simply create a temporary file (remove it afterwards for security!) in the root of your Pagekit folder called `info.php`. This file should have the following PHP code:

```
<?php phpinfo();
```

Then point your browser at the file, e.g. `http://yourpagekitsite.com/info.php`. If PHP is running as expected you should get a report page listing all the information related to the PHP configuration including version and extensions loaded.

## Register Globals Issue

If you have recently upgraded to PHP 5.4 from version 5.3 you may still have some out of date settings in the `php.ini` file. One item that can cause a *500 Internal Server Error* is the `register_globals` setting. Simply remove or comment out the setting line and restart your Apache server.

```
register_global = On
```

## ThreadStackSize on Windows

If your server is running on Windows, you could be getting a *500 Internal Server Error* due to the fact that the `ThreadStackSize` is too small. Simply append this code to the bottom of your `httpd.conf` file and restart your Apache server.

```
<IfModule mpm_winnt_module>
  ThreadStackSize 8388608
</IfModule>
```
