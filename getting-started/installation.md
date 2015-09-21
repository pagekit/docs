# Installation

<p class="uk-article-lead">This document will guide you through Pagekit's installation process.</p>

## Download

<a class="uk-button uk-button-large uk-button-primary" href="http://pagekit.com">Download Pagekit</a>

## Installation

Extract the downloaded archive and copy the contents to your webserver, either to web root directory or to a subfolder, for example called `/pagekit`.

Open your browser. If you have moved your files to the web root, navigate to your domain directly: `http://example.com`. If your files are located in a subfolder, navigate to that location, e.g. `http://example.com/pagekit`.

You should see the first screen of the web installer.

#### Step 1 - Language

#### Step 2 - Database

In the first step of the installation process we give Pagekit some information to connect to the database we created earlier.

| Field | Description |
|-------|-------------|
| `Host`     | Enter the host name of your database server. If the webserver and database server are on the same machine, this will be *localhost* or *127.0.0.1*  |
| `User`     | Enter a MySQL username that has access to the database. |
| `Password` | Enter the MySQL user's password.                        |
| `Database` | Enter the database name.                                |
| `Table Prefix` | You can change the prefix that is used for the database tables. The default prefix is `pk_`.  |

**Note** Pagekit will try to create the database during installation. You can also do this yourself using a tool like [phpMyAdmin](http://http://www.phpmyadmin.net/). Feel free to use an existing database. Pagekit prefixes its tables to avoid conflicts.

#### Step 3 - Create a User

Now you need to create your user account for Pagekit. This user will have admin access and will be able to log in to Pagekit's control panel, once the installation is finished.

#### Step 4 - Site Settings

Finally we enter the name of your site and a description. Once the installation was successful you can log in to Pagekit's control panel with the account we created. You can access the login screen by appending `/admin` to your URL.

## Installation from Github