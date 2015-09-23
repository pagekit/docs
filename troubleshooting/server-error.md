# Server Error

<p class="uk-article-lead">No matter the cause of the error this article will try explain how to isolate it and solve the most common ones.</p>

## Request Errors

If you are able to access your Pagekit installation but getting an error when trying to perform some task, is usually related to a Request error which Response should reveal us what is going on. In order to see the response you must check the browser console and make sure the [Debug](troubleshooting/debug.md) mode is on.

## Pagekit Errors

If the error is braking the page and a message **Whoops! Something went wrong** is displaying instead, it means Pagekit is being executed but an PHP error ocurred. In order to see the actual error enable the [Debug](troubleshooting/debug.md) mode and refresh the view.

## Internal Server Errors

If the error is braking the page and a message **The server encountered an internal error or misconfiguration and was unable to complete your request** is displaying instead, it means the error is happening before Pagekit execution and to get the actual error to be displayed would require modifying the `php.ini` file.

## Common Errors

There are errors that are common to many situations, review them and make sure your one is not related to any of them.

## Corrupted or Outdated Cache

To ensure that the configuration is up to date and the Cache is not causing the issue, you can flush it manually removing the `tmp/cache` folder or through the console:

```
./pagekit clearcache
```

## PHP not executing

To ensure PHP is working properly on your server create a temporary file (remove it afterwards for security!) in the root of your Pagekit folder called `info.php`. This file should have the following PHP code:

```
<?php phpinfo();
```

Then point your browser at the file, e.g. `http://yourpagekitsite.com/info.php`. If PHP is running as expected you should get a report page listing all the information related to the PHP configuration including version and extensions loaded.

### Register Globals Issue

If you have recently upgraded to PHP 5.4 from version 5.3 you may still have some out of date settings in the `php.ini` file. One item that can cause a *500 Internal Server Error* is the `register_globals` setting. Simply remove or comment out the setting line and restart your Apache server.

```
register_global = On
```

### ThreadStackSize on Windows

If your server is running on Windows, you could be getting a *500 Internal Server Error* due to the fact that the `ThreadStackSize` is too small. Simply append this code to the bottom of your `httpd.conf` file and restart your Apache server.

```
<IfModule mpm_winnt_module>
  ThreadStackSize 8388608
</IfModule>
```
