# Folder Structure

<p class="uk-article-lead">The following table gives you an overview on the folder structure in Pagekit.</p>

    + /app                      // main system files
      - assets                  // main assets
      - modules                 // modules files. Each module has its own subfolder
      - system                  // System is a core extension
      - vendor                  // external libraries that are used by Pagekit
    + /packages                 // core and 3rd party extensions
      - /pagekit                // core extensions
        - /blog                 // Blog extension
        - /theme-on             // the default theme distributed with Pagekit
    - /storage                  // site media files. You can change this location in System > Settings
    + /tmp                      // temporary files
      - /logs                   // log files
      - /sessions               // user sessions
