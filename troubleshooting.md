# Troubleshooting

If you run into problems with Pagekit, there are a few steps you should try.

## System requirements

Check, if your webserver meets the system requirements mentioned in the [Quickstart chapter](quickstart.md).

## Clear Cache


If outdated data stored in the cache causes unexpected behavior, clearing the cache will fix these issues.

###From the browser
Go to the *Settings* screen and click on the *Clear Cache* button.

###From the command line
You can also clear the cache with Pagekit's command line interface. Open terminal and change to the Pagekit folder, then run `./pagekit clear-cache`.


## System Information

A lot of times when an error occurs, you will have to retrieve more information about the system environment Pagekit runs on.
Pagekit offers this information in the Control Panel, just go to *Settings > Info*.

| Menu | Description |
|------|-------------|
| *System*      | Shows the current Pagekit Version, the webserver it runs on and the browser you're using. |
| *PHP*         | Shows information on the server's PHP version. |
| *Database*    | Shows information on the database server version. |
| *Permissions* | Shows information on folder permissions. If it shows a directory that is not writable, you should correct this. |
