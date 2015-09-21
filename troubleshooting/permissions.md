# Permissions

The installer will check for correct permissions automatically. If you want to make sure yourself, refer to the following list. We recommend CHMOD *755*. Pagekit needs to be able to write to the following files and directories and each of their subdirectories:

| File / Folder    | Description |
|------------------|-------------|
| `/app`           | Stores temporary, cache and log files.        |
| `/extensions`    | To install and update extensions.             |
| `/storage`       | Stores binary files you upload to your site.  |
| `/themes`        | To install and update themes.                 |
| `/vendor`        | Third party libraries, managed by `composer`. |
| `config.php`     | Installer will write to this file.            |

The actual permission settings depend on the user that the webserver is running with and the owner of the folders. If your webserver has problems with the CHMOD *755*, you can also try *775* and lastly *777* in this order.

**Important** You should always avoid *777* permissions on your production webserver, since this will allow anyone who has access to the machine to edit the files.

