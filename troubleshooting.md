# Troubleshooting

If you run into problems with Pagekit, there are a few steps you should try.

## System requirements

First, please check if your webserver meets the system requirements and if all needed PHP extensions are enabled.

- Apache 2.2+
- MySQL Server 5.1+
- PHP Version 5.3.7+

**PHP extensions**

- JSON extension
- Session extension
- ctype extension
- Tokenizer extension
- SimpleXML extension
- cURL extension
- mbstring extension
- PCRE extension (version 8.0+)
- ZIP extension
- PDO extension with MySQL drivers

## Clear Cache

If outdated data stored in the cache causes unexpected behavior, clearing the cache will fix these issues. There are several ways to clear the cache.

| Method | Description |
|------|-------------|
| From the browser       | Go to the *Settings* screen and click on the *Clear Cache* button. |
| From the command line  | Open terminal and change to the Pagekit folder, then run `./pagekit clear-cache`. |
| From your file browser | To empty the cache folder manually, simply delete all files and subfolders located in `/app/cache`. |

## System Information

A lot of times when an error occurs, you will have to retrieve more information about the system environment Pagekit runs on.
Pagekit offers this information in the Control Panel, just go to *Settings > Info*.

| Menu | Description |
|------|-------------|
| *System*      | Shows the current Pagekit Version, the webserver it runs on and the browser you're using. |
| *PHP*         | Shows information on the server's PHP version (make sure your version is 5.3.7+). |
| *Database*    | Shows information on the database server version. |
| *Permissions* | Shows information on folder permissions. If it shows a directory that is not writable, you should correct this. |
