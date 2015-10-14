# File Structure
<p class="uk-article-lead">The following table gives you an overview on the file structure in Pagekit.</p>

```
/app                      // main system files
  assets                  // system assets
  console                 // console extension files
  installer               // core Install/Update extension files
  modules                 // core modules files. Each module has its own subfolder
  system                  // core System extension files
  vendor                  // external libraries that are used by Pagekit
/packages                 // Pagekit packages and 3rd party packages
  composer                // packager related files
  pagekit                 // Pagekit default packages
    blog                  // default Blog extension
    theme-one             // the default theme distributed with Pagekit
/storage                  // site media files. You can change this location in System > Settings
/tmp                      // temporary files
  cache                   // cache files
  logs                    // log files
  packages                // temporary package files
  sessions                // file based user sessions
  temp                    // general temporary files
.htaccess                 // the Apache configuration file. Make sure it exists if using Apache
CHANGELOG.md              // changelog file
config.php                // configuration file generated during the installation
pagekit                   // the CLI entry point
pagekit.db                // the database file (only present if using SQLite)
```
