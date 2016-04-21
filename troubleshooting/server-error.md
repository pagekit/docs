# Server Error
<p class="uk-article-lead">This guide will help you to isolate and solve the most common server errors.</p>

<ul class="uk-list">
    <li><a href="#request-errors">Request Errors</a></li>
    <li><a href="#pagekit-errors">Pagekit Errors</a></li>
    <li><a href="#internal-server-errors">Internal Server Errors</a></li>
    <li><a href="#corrupted-or-outdated-cache">Corrupted or Outdated Cache</a></li>
    <li><a href="#php-not-executing">PHP not executing</a></li>
</ul>

## Request Errors
If you are able to access your Pagekit installation but are getting an error when trying to perform a task, it's usually related to a request error. Its response should reveal to us what is going on. In order to see the response, you must check the browser console and make sure [Debug Mode](debug-mode.md) is on.

## Pagekit Errors
If the error is breaking the page and a message **Whoops! Something went wrong** is being displayed, it means that Pagekit is being executed but a PHP error has occurred. In order to see the actual error, enable [Debug](debug-mode.md) mode and refresh the view.

## Internal Server Errors
If the error is breaking the page and a message **The server encountered an internal error or misconfiguration and was unable to complete your request** is being displayed, it means the error is happening before Pagekit executes. Getting the actual error to be displayed, requires modifying the `php.ini` file.

## Corrupted or Outdated Cache
To ensure that the configuration is up to date and the cache is not causing the issue, you can flush it manually by removing the `tmp/cache` folder or through the console.

```
./pagekit clearcache
```

## PHP not executing
To ensure PHP is working properly on your server, create a temporary file in the root of your Pagekit folder called `info.php`. This file should contain the following PHP code.

```
<?php phpinfo();
```

Then navigate your browser to the file, e.g. `http://example.com/info.php`. If PHP is running as expected, you should get a report page listing all information related to the PHP configuration, including version and extensions loaded.

**Important** Remove the file afterwards for security.