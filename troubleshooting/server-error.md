# Server Error

<p class="uk-article-lead">No matter the cause of the error this article will try explain how to isolate it and solve the most common ones.</p>

## Request Errors

If you are able to access your Pagekit installation but are getting an error when trying to perform a task, it's usually related to a Request error. Its response should reveal to us what is going on. In order to see the response you must check the browser console and make sure the [Debug Mode](troubleshooting/debug.md) is on.

## Pagekit Errors

If the error is braking the page and a message **Whoops! Something went wrong** is displaying instead, it means Pagekit is being executed but an PHP error ocurred. In order to see the actual error enable the [Debug](troubleshooting/debug.md) mode and refresh the view.

## Internal Server Errors

If the error is breaking the page and a message **The server encountered an internal error or misconfiguration and was unable to complete your request** is displaying instead, it means the error is happening before Pagekit execution and to get the actual error to be displayed would require modifying the `php.ini` file.

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
