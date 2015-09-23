# Server Error

<p class="uk-article-lead">Server errors are almost always caused by misconfiguration of Pagekit or something unexpected happened.</p>

> The server encountered an internal error or misconfiguration and was unable to complete your request.

When you see this message it means your server is running in `Production` mode to hide potentially sensitive information from displaying to the users. The error itself will be stored in the `tmpl/temp/debug.db` file. You may examine this file with a [SQLiteBrowser](http://sqlitebrowser.org) to determine the exact nature of the error.

Possible reasons include:

* Not fulfilling the system requirements
* Out-of-date configuration
* Incorrect file permissions
* Configuration issues
* Installation issues

The first thing you should do is flush the cache to ensure that the configuration is up to date by manually removing the `tmp/cache` folder or through the console:

```
./pagekit clearcache
```
