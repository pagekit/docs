# Introduction

<p class="uk-article-lead">Get started with development of extensions and themes for Pagekit.</p>

To always have our latest code revision (not necessarily stable), checkout our master branch from [github](https://github.com/pagekit/pagekit). To do so, change to your web server's directory in the terminal and run the following commands:

```shell
git clone git@github.com:pagekit/pagekit.git
composer install
```

Now open `/install` in the browser to run the web-based installer.

## Command line

A great tool you can - and should - consult for a lot of tasks is the Pagekit command line tool. To see a complete overview of all available commands, navigate to the Pagekit folder and run `./pagekit list`.

```shell
cd path/to/pagekit/
./pagekit list
```

The most notable commands in day-to-day use are probably:

| Command    | Description |
|------------------|-------------|
| `clearcache`     | Use this to empty the cache.  |
| `routes`         | Use this to list all registered routes (very useful when adding a new controller).  |
| `extension:generate <name>` <br> `theme:generate <name>`| Use this to generate the basic file structure when creating a new extension or theme.  |

## System settings

When navigating to *Settings > System* in the admin interface, you have a few settings to support you during development.

| Setting               | Description |
|-----------------------|-------------|
| *Debug mode*        | Turn on debug mode and exceptions are displayed in all their glory. Very handy when working on that new extension of yours. Remember to turn this off for production environments.  |
| *Profiler toolbar*  | Display a toolbar at the bottom of the screen that helps you monitor memory usage, query count, system events, requests, headers and session attributes.  |
| *Disable cache*     | You might want to turn off the cache during development to avoid the frequent "clearcache". Remember to enable the cache for production.  |
