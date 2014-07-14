# Quickstart

<p class="uk-article-lead">This document will guide you through Pagekit's installation process.</p>

<a class="uk-button uk-button-large uk-button-primary" href="http://pagekit.com">Download Pagekit</a>

## System requirements

Make sure your server meets the following requirements.

- Apache 2.2+
- MySQL Server 5.1+
- PHP Version 5.4.0+
In addition, Pagekit needs certain PHP extensions to be enabled. Check out the detailed list in the [server requirements section](troubleshooting.md).

## Installation

First of all, extract the downloaded archive and copy the contained folders and files to your webserver directory.

### 1. Set the permissions

Before you start with the installation process, ensure that the folders you've just uploaded have the right permission settings. We recommend CHMOD *755*. Pagekit needs to be able to write to the following files and directories and each of their subdirectories:

| File / Folder    | Description |
|------------------|-------------|
| `/app`           | Stores temporary, cache and log files.        |
| `/extensions`    | To install and update extensions.             |
| `/storage`       | Stores binary files you upload to your site.  |
| `/themes`        | To install and update themes.                 |
| `config.php`     | Installer will write to this file.            |

The actual permission settings depend on the user that the webserver is running with and the owner of the folders. If your webserver has problems with the CHMOD *755*, you can also try *775* and lastly *777* in this order.

**Important** You should always avoid *777* permissions on your production webserver, since this will allow anyone who has access to the machine to edit the files.

### 2. Create the database

Next thing we need to do is create an empty database for Pagekit to work with using a tool like [phpMyAdmin](http://http://www.phpmyadmin.net/). If your hoster limits you to only one database which is already used by another application, you can still use it. Pagekit prefixes its tables to avoid conflicts.

### 3. Run the installation

Now we are ready to run the Pagekit installation process. Open your browser and go to the Pagekit URL on your webserver followed by `/installer`, e.g. `http://example.com/pagekit/installer`. This will take you to the start screen of the installation.

#### Step 1 - Database information

In the first step of the installation process we give Pagekit some information to connect to the database we created earlier.

| Field | Description |
|-------|-------------|
| `Host`     | Enter the host name of your database server. If the webserver and database server are on the same machine, this will be *localhost* or *127.0.0.1*  |
| `User`     | Enter a MySQL username that has access to the database. |
| `Password` | Enter the MySQL user's password.                        |
| `Database` | Enter the database name.                                |
| `Table Prefix` | You can change the prefix that is used for the database tables. The default prefix is `pk_`.  |

#### Step 2 - Create a User

Now you need to create your user account for Pagekit. This user will have admin access and will be able to log in to Pagekit's control panel, once the installation is finished.

#### Step 3 - Site Settings

Finally we enter the name of your site and a description. Once the installation was successful you can log in to Pagekit's control panel with the account we created. You can access the login screen by appending `/admin` to your URL.

## Updating

Before you perform an update, make sure you have a backup of the current Pagekit installation and your database. That way you can always recover previous states of your installation in case something goes wrong.

### 1-Click Update

You can update Pagekit using the update function in Pagekit's Administration. Open *Settings > Update* and click the *Update* button.

### Manual Update

To update Pagekit manually, download the latest release from http://pagekit.com and extract the archive.
Now upload the folder to your webserver and overwrite the existing files in the Pagekit folder.

To make sure that the update was successful, just open *Settings > Info*. If everything went smoothly, you should see the new version number under *Pagekit Version*.
