# Debug

<p class="uk-article-lead">There are situations when the issue source is not clear or requires extra steps to be shown.</p>

By default Pagekit will hide any PHP error, in order to show it the **Debug Mode** must be enabled in *System > Settings* or set to true in the [configuration file](getting-started/configuration-file.md) manually.

**Note:** if the error is happening during installation the configuration file will not exist. In such case you should edit the default config file located in `app/installer/config.php`.

The same way you may enable the **Debug Toolbar** which will provide some useful information about the environment. Notice that some server errors may prevent the toolbar to be displayed.

> Remember to disable the *Debug* and *Debug Toolbar* when site deployed in a production server.
