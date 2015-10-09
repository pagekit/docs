# System
The System is a core extensions holding the main Pagekit system. On its view are managed the general settings, extensions and themes installed, core updates as provided useful environment information.

## Settings
### System

Field         | Description
:------------ | :----------------------------------------------------------------------------
**Storage**   | Defines the upload folder for media files.
**Developer** | Allows enabling the [debug mode and debug toolbar](troubleshooting/debug.md).

### Localization

Field            | Description
:--------------- | :------------------------------------------------
**Site Locale**  | The default frontend language and date format.
**Admin Locale** | The default admin panel language and date format.

### Cache

Field     | Description
:-------- | :--------------------------------------------------------------------------
**Cache** | Auto. It will auto determine the best possible caching system.
          | APC. It will use the APC cache if available.
          | XCache. It will use the XCache if available.
          | File. It will use the file cache system.
Developer | If enabled the cache will be disabled. Use only on development environment.

The _Clear Cache_ button allows deleting system and theme temporary files.

### Mail
Configure settings for your site's email communication. Pagekit uses the _PHP mailer_ by default, but you can also enable the _SMTP mailer_ and enter your credentials.

Field          | Description
:------------- | :---------------------------------------------------------------------------------------------------------------
**From Email** | The email address that the e-mail should appear to come from.
**From Name**  | The name that will be displayed as the sender instead of the email address itself.
**Mailer**     | The mail engine that will be used to send the email, PHP or SMTP.
**SMTP**       |
**Port**       | The port that will be used to sending the email. Defaults to 25.
**Host**       | The host where the mail server is located. Defaults to 'localhost'.
**User**       | The user that will be used to connect to the email server.
**Password**   | The password that will be used to connect to the email server. Click on the show button to display the password.
**Encryption** | The password that will be used to connect to the email server. Click on the show button to display the password.

The _Check Connection_ button (only visible if using SMTP mailer) will allow to test the connection to the SMTP server. While the _Send Test Email_ will send an test email regardless of the used mailer.

## Extensions & Themes
Using these panels, you can manage your extensions and themes. It shows you pending updates and lets you upload new ones. Don't forget to enable new extensions after you've installed them.

## Update
In this section you can update Pagekit with one click in case a new version is available.

# Info
Here you can find an overview of your server's details and file permissions needed by Pagekit. Consult this section, if you run into trouble and need to provide details on your server environment to someone giving you support.
