# Installation

<p class="uk-article-lead">This document will guide you through Pagekit's installation process.</p>

## Installation

### Option 1: install with a package

[Download](http://pagekit.com/api/download/latest) and extract the latest Pagekit archive into the the contents of your webserver, either to web root directory or to a subfolder, for example called `/pagekit`.

**Note** If you downloaded the package file and then plan to move it to your webroot, please move the ENTIRE FOLDER because it contains several hidden files (such as .htaccess) that will not be selected by default.

## Option 2: install from Github

Clone the repository.

```
git clone --branch develop git://github.com/pagekit/pagekit.git
```

Navigate to the cloned directory and install PHP dependencies.

```
composer install
```

Install Node dependencies and build the front-end components:

```
npm install
```

**Note** For more detailed and up to date instructions review the [main Pagekit repository](https://github.com/pagekit/pagekit).

## Initial Setup

Through the browser navigate to your domain and folder where the Pagekit content has been extracted. You should see the first screen of the web installer.

### Step 1 - Language

In the first step of the setup process we have to choose the main language for the site. It will be the default language used on the backend and frontend. Both can be changed any time later.

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

### Step 3 - Create a User

Now you need to create your user account for Pagekit. This user will have admin access and will be able to log in to Pagekit's control panel, once the installation is finished.

| Field | Description |
|-------|-------------|
| `User`     | Enter the User username. |
| `Password` | Enter the User password. |
| `Email`    | Enter the User email.    |

### Step 4 - Site Settings

Finally we enter the the site details.

| Field | Description |
|-------|-------------|
| `Name`        | The site Name.        |
| `Description` | The site Description. |

Once the installation was successful you can log in to Pagekit's control panel with the account we created. You can access the login screen by appending `/admin` to the site URL.
