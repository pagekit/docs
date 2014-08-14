# Quickstart

<p class="uk-article-lead">This document will guide you through Pagekit's installation process.</p>

<a class="uk-button uk-button-large uk-button-primary" href="http://pagekit.com">Download Pagekit</a>

## Requirements

Make sure your server meets the following requirements.

- Apache 2.2+ or nginx
- MySQL Server 5.1+ or SQLite 3
- PHP Version 5.4+

In addition, Pagekit needs certain PHP extensions to be enabled. Check out the detailed list in the [server requirements section](troubleshooting.md).

## Installation

Extract the downloaded archive and copy the contents to your webserver, either to web root directory or to a subfolder, for example called `/pagekit`.

Open your browser. If you have moved your files to the web root, navigate to your domain directly: `http://example.com`. If your files are located in a subfolder, navigate to that location, e.g. `http://example.com/pagekit`. 

You should see the first screen of the web installer.

#### Step 1 - Database information

In the first step of the installation process we give Pagekit some information to connect to the database we created earlier.

| Field | Description |
|-------|-------------|
| `Host`     | Enter the host name of your database server. If the webserver and database server are on the same machine, this will be *localhost* or *127.0.0.1*  |
| `User`     | Enter a MySQL username that has access to the database. |
| `Password` | Enter the MySQL user's password.                        |
| `Database` | Enter the database name.                                |
| `Table Prefix` | You can change the prefix that is used for the database tables. The default prefix is `pk_`.  |

**Note** Pagekit will try to create the database during installation. You can also do this yourself using a tool like [phpMyAdmin](http://http://www.phpmyadmin.net/). Feel free to use an existing database. Pagekit prefixes its tables to avoid conflicts.

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
