# Folder Structure

The following table gives you an overview on the folder structure in Pagekit.

| Folder | Description |
|--------|-------------|
| `/app` | Contains the Pagekit configuration, cache and user sessions. |
| `/app/cache`          | Contains the cache files. |
| `/app/config`         | Contains the default configuration. |
| `/app/logs`           | Contains log files. |
| `/app/sessions`       | Contains user sessions. |
| `/app/temp`           | Contains temporary files. |
| `/extensions`         | Contains the Pagekit extensions, each extension creates its own subfolder. |
| `/extensions/system`  | The Pagekit System (this is a core extension). |
| `/extensions/install` | The Pagekit Installer (this is a core extension). |
| `/extensions/page`    | The Page extension, allows you to create static pages. |
| `/storage`            | Contains your uploads like pictures and videos. You can change the location in the *Settings > System*. |
| `/themes`             | Contains the Pagekit themes, each theme creates its own subfolder. |
| `/themes/alpha`       | This is the default theme, that comes with Pagekit. |
| `/vendor`             | Contains external libraries that are used by Pagekit. |