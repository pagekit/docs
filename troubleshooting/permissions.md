# Permissions
<p class="uk-article-lead">The installer will check for correct permissions during the installation. If you need to fix the file permissions, this guide will help.</p>

Pagekit needs to be able to write to the following files and directories and each of their subdirectories.

File/Folder | Description
------------- | --------------------------------------------
`/app`        | To access the system files.
`/packages`   | To install and update extensions.
`/storage`    | Stores binary files you upload to your site.
`/tmp`        | Stores temporary, cache and log files.
`config.php`  | Installer will write to this file.

The actual permission settings depend on the user that PHP and the webserver are running on and the owner of the folders. If your webserver has problems with the CHMOD _755_, you can also try _775_.

**Important** You should always avoid _777_ permissions on your production webserver, since this will allow anyone, who has access to the machine, to edit the files.
