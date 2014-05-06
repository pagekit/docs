# Quickstart

Download the latest version of Pagekit from [pagekit.com](http://pagekit.com). This document will guide you through the installation process.

## System requirements

- Apache 2.2+
- MySQL Server 5.1+
- PHP Version 5.3.7+
- PHP extensions:
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

## Installation

First of all, extract the downloaded archive and copy the contained folders and files to your webserver directory.

### Set the permissions

Before you start with the installation process, ensure that the folders you've just uploaded have the right permission settings. Pagekit needs to be able to write to the following files and directories and each of their subdirectories:
  - `/app`
  - `/extensions`
  - `/storage`
  - `/themes`
  - `config.php`

The actual permission settings depend on the user that the webserver is running with and the owner of the folders.

| User / Group     | Permissions |
|------------------|-------------|
| same user        | 744         |
| different user, same group       | 774         |
| different user, different group  | 777         |

**Important** You should always avoid '777' permissions on your production webserver, since this will allow anyone who has access to the machine to edit the files.

### Create the database

Next thing we need to do is create an empty database for Pagekit to work with using a tool like [phpMyAdmin](http://http://www.phpmyadmin.net/).

If your hoster limits you to only one database which is already used by another application, you can still use it. Pagekit prefixes its tables to avoid conflicts.

### Run the installation

Now we are ready to run the Pagekit installation process. Open your browser and go to the Pagekit URL on your webserver followed by `/installer`, e.g. `http://example.com/pagekit/installer`. This will take you to the start screen of the installation.

#### Database

In the first step of the installation process we give Pagekit some information to connect to the database we created earlier.

| Field | Description |
|-------|-------------|
| `Host`     | Enter the host name of your database server. If the webserver and database server are on the same machine, this will be 'localhost' or '127.0.0.1'                                                                     |
| `User`     | Enter a MySQL username that has access to the database. |
| `Password` | Enter the MySQL user's password.                        |
| `Database` | Enter the database name.                                |
| `Table Prefix` | You can change the prefix that is used for the database tables. The default prefix is `pk_`.                                     |

#### Create a User

Now you need to create your user account for Pagekit. This user will have admin access and will be able to log in to Pagekit's control panel, once the installation is finished.

#### Site Settings

Finally we enter the name of your site and a description.

Once the installation was successfull you can log in to Pagekit's control panel with the account we created. You can access the login screen by appending `/admin` to your URL.

## Updating

**Important** Before you perform an update, make sure you have a backup of the current Pagekit installation and your database.

#### 1-Click Update

You can update Pagekit using the update function in Pagekit's Administration. Open *Settings > Update* and click the 'Update' button.

#### Manual Update

To update Pagekit manually, download the latest release from http://pagekit.com and extract the archive.
Now upload the folder to your webserver and overwrite the existing files in the Pagekit folder.

To make sure that the update was successful, just open *Settings > Info*. If everything went smoothly, you should see the new version number under 'Pagekit Version'.
