# Installation
<p class="uk-article-lead">This document will guide you through the Pagekit installation process.</p>

[Download](http://pagekit.com/api/download/latest) the latest Pagekit archive and extract the contents to your webserver - either to the web root directory or to a subfolder, for example called `/pagekit`.

**Note** If you unzip the package locally before uploading, make sure to also include hidden files `.htaccess`.

## Initial Setup
Open your browser and navigate to the URL of the extracted Pagekit content. You should see the first screen of the web installer.

### Step 1 - Language
In the first step of the setup process you choose the main language for the site. It will be the default language used on the admin panel and frontend. Both can be changed at any time later.

### Step 2 - Database
In this step you enter the details to connect to the database.

Field           | Description
--------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------
`Driver`        | Enter the Database Driver. It depends on the type of database you want to use.
`Hostname`      | Enter the host name of your database server. If the webserver and database server are on the same machine, this will be `localhost` or `127.0.0.1`.
`User`          | Enter a MySQL username that has access to the database.
`Password`      | Enter the MySQL user's password.
`Database Name` | Enter the database name.
`Table Prefix`  | You can change the prefix that is used for the database tables. The default prefix is `pk_`.

**Note** Pagekit will try to create the database during installation. You can also do this yourself using a tool like [phpMyAdmin](http://http://www.phpmyadmin.net/). Feel free to use an existing database. Pagekit prefixes its tables to avoid conflicts.

### Step 3 - Site setup
After entering your site's title, you need to create a user account for Pagekit. This user will have admin access and will be able to log in to Pagekit's control panel, once the installation is finished.

Field        | Description
------------ | ------------------------
`Site Title` | The site title.
`Username`   | Enter the admin username.
`Password`   | Enter the admin password.
`Email`      | Enter the admin email.

Once the installation is successful you are redirected to the login screen. You can log in to Pagekit's admin panel with the account you created. If you want to login to the admin area in the future, you can always reachthe login screen by appending `/admin` to your site's URL.
