# Troubleshooting

<p class="uk-article-lead">If you run into problems with Pagekit, there are a few steps you should try.</p>

## System requirements

First, please check if your webserver meets the system requirements and if all needed PHP extensions are enabled.

- Apache 2.2+ or nginx
- MySQL Server 5.1+ or SQLite 3
- PHP Version 5.4+

### PHP extensions

- JSON extension
- Session extension
- ctype extension
- Tokenizer extension
- SimpleXML extension
- cURL extension
- mbstring extension
- PCRE extension (version 8.0+)
- ZIP extension
- PDO extension with MySQL- or SQLite drivers
- APCu extension (version 4.0.2+)

### Permissions

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

## Clear Cache

If outdated data stored in the cache causes unexpected behavior, clearing the cache will fix these issues. There are several ways to clear the cache.

| Method | Description |
|------|-------------|
| From the browser       | Go to the *Settings* screen and click on the *Clear Cache* button. |
| From the command line  | Open terminal and change to the Pagekit folder, then run `./pagekit clearcache`. |
| From your file browser | To empty the cache folder manually, simply delete all files and subfolders located in `/app/cache`. |

## System Information

A lot of times when an error occurs, you will have to retrieve more information about the system environment Pagekit runs on.
Pagekit offers this information in the Control Panel, just go to *Settings > Info*.

| Menu | Description |
|------|-------------|
| *System*      | Shows the current Pagekit Version, the webserver it runs on and the browser you're using. |
| *PHP*         | Shows information on the server's PHP version. |
| *Database*    | Shows information on the database server version. |
| *Permissions* | Shows information on folder permissions. If it shows a directory that is not writable, you should correct this. |
