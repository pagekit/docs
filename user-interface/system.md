# System

The System area holds all the tools related to the Site's system administration. The main tasks you can accomplish here are running System updates or enabling installed extensions and themes.

## Settings
### System

Field         | Description
:------------ | :----------------------------------------------------------------------------
**Storage**   | Defines the upload folder for media files.
**Developer** | Allows enabling the [debug mode and debug toolbar](../troubleshooting/debug-mode.md).

### Localization

Field            | Description
:--------------- | :------------------------------------------------
**Site Locale**  | The default frontend language and date format.
**Admin Locale** | The default admin panel language and date format.

### Cache

Field     | Description
:-------- | :--------------------------------------------------------------------------
**Cache** | Auto. Pagekit determines the best possible caching system.
          | APC. Pagekit uses the APC cache if available.
          | XCache. Pagekit uses the XCache if available.
          | File. Pagekit uses a file based cache.
Developer | If enabled the cache will be disabled. Use only on development environment.

The _Clear Cache_ button allows deleting system and theme temporary files.

### Mail
Configure settings for your site's email communication. Pagekit uses the _PHP mailer_ by default, but you can also enable _SMTP mailer_ and enter your SMTP credentials.

Field          | Description
:------------- | :---------------------------------------------------------------------------------------------------------------
**From Email** | Email address that the e-mail should appear to come from.
**From Name**  | Name that will be displayed as the sender instead of the email address itself.
**Mailer**     | Mail engine that will be used to send the email, PHP or SMTP.
**SMTP**       |
**Port**       | Port required by mail server. Defaults to 25.
**Host**       | Host where mail server is located. Defaults to `localhost`.
**User**       | User to connect to the mail server.
**Password**   | Password to connect to mail server. Click on the show button to display the password.
**Encryption** | Encryption required by mail server

The _Check Connection_ button (only visible if using SMTP mailer) will allow to test the connection to the SMTP server.

The _Send Test Email_ button will send a test email using the selected mailer (PHP or SMTP).

## Extensions & Themes
Using these panels, you can manage your extensions and themes. It shows pending updates and lets you upload new ones. Don't forget to enable new extensions after you've installed them. You can do this by clicking the round status icon. A red icon means the extension is disabled, a green icon means it is enabled.

You can enable a theme by hovering over the theme thumbnail and clicking the appearing star icon.

## Update
In this section you can update Pagekit with one click in case a new version is available. Remember to always backup your installation including files located on the server and data from your database. If anything goes wrong during the update process or things do not work with a new version, you can always go back to your working installation.

# Info
Here you can find an overview of your server's details and file permissions needed by Pagekit. Consult this section, if you run into trouble and need to provide details on your server environment to someone giving you support.
