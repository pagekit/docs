# System

<p class="uk-article-lead">The System area holds all the tools related to the site's system administration. The main tasks you can accomplish here are running system updates or enabling installed extensions and themes.</p>

<ul class="uk-list">
    <li><a href="#settings">Settings</a></li>
    <li><a href="#extensions-themes">Extensions &amp; Themes</a></li>
    <li><a href="#update">Update</a></li>
    <li><a href="#info">Info</a></li>
</ul>

## Settings

The _Settings_ tab provides general configurations for your Pagekit website. You can define **System** settings, **Localisation**, **Cache** and your site's **e-mail**.

### System

In this area you can set a different upload folder for media files. By default, the folder is _/storage_. Additionally, you can enable [debug mode and the debug toolbar](../troubleshooting/debug-mode.md).

### Localization

Select the default language and date format for the frontend as well as for the admin panel.

### Cache

Pagekit offers a number of caching systems. If you select **Auto (File)**, Pagekit automatically determines the best possible caching system. **APC**, **XCache** and **File** are also available. Alternatively, you can disable cache altogether. However, this is only advisable in a development environment.
The _Clear Cache_ button deletes temporary system and theme files.

### Mail
Configure settings for your site's e-mail communication. The address you provide here will be displayed in system e-mails to your users. Pagekit uses _PHP mailer_ by default, but you can also enable _SMTP mailer_ and enter your SMTP credentials.

Field          | Description
:------------- | :---------------------------------------------------------------------------------------------------------------
**From Email** | The e-mail address that the e-mail should appear to come from.
**From Name**  | The name that will be displayed as the sender instead of the e-mail address itself.
**Mailer**     | The mail engine that will be used to send the e-mail, PHP or SMTP.
**SMTP**       |
**Port**       | The mail server requires a port. By default, it is set to **25**.
**Host**       | This is the host where the mail server is located; default is `localhost`.
**User**       | The user to connect to the mail server usually depends on your e-mail provider.
**Password**   | The password to connect to your mail server. Hit the _Show_ button to display the password.
**Encryption** | Select the encryption for your mail server. Available options are **None**, **SSL** and **TSL**.

The _Check Connection_ button (only visible, if using SMTP mailer) allows you to test the connection to the SMTP server. The _Send Test Email_ button will send a test e-mail using the selected mailer (PHP or SMTP).

## Extensions

This panel displays all your installed extensions inside a list. Whenever there's an update available for a theme, a button will appear that allows you to perform a one-click update. To install a new extension, hit the _Upload_ button in the upper right corner.

Don't forget to enable new extensions after you've installed them. You can do this by clicking the round status icon. A red icon means the extension is disabled, a green icon means it is enabled.

When hovering a item, an info button and additional options, like a permissions view, appear on the right hand side.

## Themes

Here you can find all themes with a preview image. When hovering a theme, a star and a trash icon will appear. Click on the star to activate a theme.

The _Customize_ button on the active theme will take you directly to the _Settings_ tab in the _Site_ section of your Pagekit administration.

Whenever there's an update available for a theme, a button will appear that allows you to perform a one-click update.

## Update
In this section you can update Pagekit with one click, if a new version is available. Remember to always backup your installation, including files located on the server and data from your database. If anything goes wrong during the update process or things do not work with a new version, you can always go back to your working installation.

## Info
Here you can find an overview of your server's details and file permissions needed by Pagekit. The section covers information on **System**, **PHP**, **Database** and **Permissions**. Consult it, if you run into trouble and need to provide details on your server environment to someone giving you support.
