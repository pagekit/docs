# Installation
<p class="uk-article-lead">Pagekit's installation process is quick and easy and shouldn't take up more than a few minutes.</p>

<ul class="uk-list">
    <li><a href="#step-1-download-and-open-the-installer">Step 1: Download and open the Installer</a></li>
    <li><a href="#step-2-language">Step 2: Language</a></li>
    <li><a href="#step-3-database">Step 3: Database</a></li>
    <li><a href="#step-4-site-setup">Step 4: Site setup</a></li>
</ul>

## Step 1: Download and open the Installer
First of all, you need to [download](http://pagekit.com/api/download/latest) the latest Pagekit package and extract the contents to your webserver – either to the web root directory or to a subfolder, for example `/pagekit`.

**Note** If you unzip the package locally before uploading, make sure to also include the hidden `.htaccess` file.

Open your browser and navigate to the URL of the extracted Pagekit content. You should now see the first screen of the web installer.

## Step 2: Language
In the first step of the actual setup process you choose the main language for the site. It will be the default language used on the admin panel and frontend. Both can be changed at any time later.

## Step 3: Database
In this step you enter the details to connect to the database. By default, Pagekit uses SQLite to store your site's data. Here you need to fill in the fields _Database Name_ and _Table Prefix_. The default prefix is `pk_`.

Alternatively, you can also choose to use MySQL. In that case the following details need to be entered.

Field           | Description
--------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------
_Driver_        | Enter the Database Driver. It depends on the type of database you want to use.
_Hostname_      | Enter the host name of your database server. If the webserver and database server are on the same machine, this will be `localhost` or `127.0.0.1`.
_User_          | Enter a MySQL username that has access to the database.
_Password_      | Enter the MySQL user's password.
_Database Name_ | Enter the database name.
_Table Prefix_  | You can change the prefix that is used for the database tables. The default prefix is `pk_`.

**Note** Pagekit will try to create the database during installation. You can also do this yourself using a tool like [phpMyAdmin](http://http://www.phpmyadmin.net/). Feel free to use an existing database. Pagekit prefixes its tables to avoid conflicts.

## Step 4: Site setup
After entering your site's title, you need to create a user account for Pagekit. This user will have admin access and will be able to log in to Pagekit's control panel, once the installation is finished.

Field        | Description
------------ | ------------------------
_Site Title_ | The site title.
_Username_   | Enter the admin username.
_Password_   | Enter the admin password.
_Email_      | Enter the admin email.

Once the installation was successful, you are redirected to the login screen. You can log in to Pagekit's admin panel with the account you've created. If you want to sign in to the admin area in the future, you can always reach the login screen by appending `/admin` to your site's URL.
