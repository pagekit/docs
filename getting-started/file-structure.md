# File Structure
<p class="uk-article-lead">When you get started using Pagekit, it is important that you know your way around the file structure. As Pagekit has a very clear separation of core code and third party files, this shouldn't be a big deal.</p>

## Explanation video

The following video goes through the structure and explains everything you need to know.

<iframe class="uk-responsive-width" width="1280" height="720" src="https://www.youtube.com/embed/xfCBW1kynmo" frameborder="0" allowfullscreen></iframe>

## Compact overview

For a brief overview, have a look at the following listing.

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

## Places to explore

While it always takes some getting used to a new project's structure, you will quickly find your way around the important parts. The essential thing to know is that themes and extensions that you develop always sit in the `/packages` directory, inside a subfolder with your vendor name.

Additionally, it is a good idea to have a look at the official packages located in `/packages/pagekit` - for inspiration and a deeper understanding of the Pagekit concepts. Also, check out the modules in `/app/modules` and `/app/system/modules` to see examples of what can be done with the module pattern.
