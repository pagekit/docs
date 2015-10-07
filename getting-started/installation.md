# Installation

<p class="uk-article-lead">This document will guide you through Pagekit's installation process.</p>

[Download](http://pagekit.com/api/download/latest) the latest Pagekit archive and extract the contents to your webserver - either to the web root directory or to a subfolder, for example called `/pagekit`.

**Note** If you downloaded the package file and then plan to move it to your webroot, make sure to copy the hidden files (such as .htaccess) too.

## Initial Setup

Through the browser navigate to your domain and folder where the Pagekit content has been extracted. You should see the first screen of the web installer.

### Step 1 - Language

In the first step of the setup process we have to choose the main language for the site. It will be the default language used on the backend and frontend. Both can be changed at any time later.

### Step 2 - Database

In this step we give Pagekit the information details to connect to the database.

| Field | Description |
|-------|-------------|
| `Driver`   | Enter the Database Driver. It depends on the type of database you are willing to connect with. |
| `Hostname` | Enter the host name of your database server. If the webserver and database server are on the same machine, this will be `localhost` or `127.0.0.1`. |
| `User`     | Enter a MySQL username that has access to the database. |
| `Password` | Enter the MySQL user's password.                        |
| `Database Name` | Enter the database name.                           |
| `Table Prefix` | You can change the prefix that is used for the database tables. The default prefix is `pk_`. |

**Note** Pagekit will try to create the database during installation. You can also do this yourself using a tool like [phpMyAdmin](http://http://www.phpmyadmin.net/). Feel free to use an existing database. Pagekit prefixes its tables to avoid conflicts.

### Step 3 - Site setup

After entering your site's title, you need to create your user account for Pagekit now. This user will have admin access and will be able to log in to Pagekit's control panel, once the installation is finished.

| Field | Description |
|-------|-------------|
| `Name`     | The site Name. |
| `User`     | Enter the User username. |
| `Password` | Enter the User password. |
| `Email`    | Enter the User email.    |

Once the installation was successful you'll be redirected to the login screen. You can log in to Pagekit's administration panel with the account we created. Optionally you can access the login screen by appending `/admin` to the site URL.
