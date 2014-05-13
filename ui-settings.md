# Settings

Click on *Settings* in the main navigation to see all important settings in one place - split into *System settings* and *Extension settings*.

![System settings overview](/images/settings-overview.png)

## System

### Site

Fill in main information about your site, set your frontpage and enable Maintenance mode.

### Email

Configure settings for your site's email communication. Pagekit uses the *PHP mailer* by default, but you can also enable the *SMTP mailer* and enter your credentials.

### Localization

Set the language of the website and admin area and set the locale the site should use to display dates.

### System

To enable Pagekit's automatic communication with the marketplace (automatic downloads and updates of extensions), you need an *API key*. Log in to [www.pagekit.com](http//www.pagekit.com) to generate your API key.

To determine the kind of updates you want to receive, choose a *Release Channel*. For production environments, you typically select *Stable* to receive update notifications only for stable releases. Developers will want to select *Nightly* for all releases, including developer releases.

*Storage* defines the upload folder for media files.

Set a *Cache mode* (*Auto* will choose the most sensible mode).

Developers can enable debug mode (display exceptions with details - disable this in production environments), a debug toolbar (displayed at the bottom of the screen) or choose to disable cache.

## Extension & Theme settings

Using these panels, you can manage your extensions and themes.
It shows you pending updates and lets you upload new extensions. Don’t forget to enable new extensions after you've installed them.

When you have entered an API key in the *System settings*, you can also access the *Marketplace* in this area to search and install extensions and themes.

## Dashboard

Use this section to add, delete and arrange the widgets on your dashboard. The dashboard is the main screen you see after logging in to the admin area and can be accessed by clicking on the Pagekit logo in the main navigation).

## Storage

The *Storage* area gives you an overview over your uploaded files. Using the simple file manager, you can manage your media and upload new files. Simply drag and drop from your local file explorer or click the *Upload* button.

## Update

In this section you can update Pagekit with one click in case a new version is available.

## URL aliases

Pagekit allows you to change URL aliases to anything you want.


## System information

Here you can find an overview of your server's details and file permissions needed by Pagekit. Consult this section, if you run into trouble and need to provide details on your server environment to someone giving you support.

## Clear Cache

Clear Pagekit system cache as well as template cache and temporary files.
