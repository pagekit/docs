# File Structure

<p class="uk-article-lead">The following table gives you an overview on the file structure in Pagekit.</p>

    /app                      // main system files
      assets                  // system assets
      console                 // console extension files
      installer               // core Install/Update extension files
      modules                 // core modules files. Each module has its own subfolder
      system                  // core System extension files
      vendor                  // external libraries that are used by Pagekit
    /packages                 // core and 3rd party extensions
      composer                // packager related files
      pagekit                 // Pagekit extensions
        blog                  // core Blog extension
        theme-one             // the default theme distributed with Pagekit
    /storage                  // site media files. You can change this location in System > Settings
    /tmp                      // temporary files
      cache                   // cache files
      logs                    // log files
      packages                // packages temporal files
      sessions                // file based user sessions
      temp                    // general temporal files
    .htaccess                // the Apache configuration file. Make sure is always there if using Apache
    CHANGELOG.md             // the Pagekit changelog file
    config.php               // the Pagekit configuration file
    pagekit.db               // the database file only present if using SQLite
