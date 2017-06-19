# Debug Mode
<p class="uk-article-lead">There are situations where the root cause for an error is not clear and requires extra steps to detect.</p>

By default, Pagekit will hide any PHP errors. In order to show them, the **Debug Mode** must be enabled in _System > Settings_ or it must be set to `true` in the [configuration file](../getting-started/configuration-file.md) manually.

**Note** If an error happens during the installation, the configuration file will not be generated. In that case, you need to edit the default config file located in `app/installer/config.php`.

You can enable the **Debug Toolbar**, which provides some useful information about the environment, the same way. Notice that some server errors may prevent the toolbar from being displayed.

Remember to disable the _Debug_ and _Debug Toolbar_ when the site is deployed on a production server.
