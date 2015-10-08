# Introduction

<p class="uk-article-lead">Get started with development of extensions and themes for Pagekit.</p>

Before you dive into the basics of developing a [Theme](../guides/theme.de) or an [Extension](../guides/extension.md), here are a few useful things to know when working with Pagekit as a developer.

## Use the most recent codebase

To always have our latest code revision (not necessarily stable), you can install Pagekit from source instead of using the provided packages (grab the `develop` branch from [GitHub](https://github.com/pagekit/pagekit)).

Make sure you have the following tools installed: [Composer](https://getcomposer.org/doc/00-intro.md#installation-nix), [npm](https://www.npmjs.com/), [Bower](http://bower.io/), [Webpack](http://webpack.github.io/), [Gulp](http://gulpjs.com/).

Clone the repository.

```
git clone --branch develop git://github.com/pagekit/pagekit.git
```

Navigate to the cloned directory and install PHP dependencies.

```
composer install
```

Install Node dependencies and build the frontend components:

```
npm install
```

To watch for local LESS asset changes, run `gulp watch`.

To watch for JS module changes, run `webpack --watch`.

When the installer has finished, point your browser to the Pagekit URL on your web server and follow the installer.

## Stay up to date

If you've set up Pagekit from source, run these commands to get new commits and to rebuild everything you need.

```
git pull
composer update
npm install
bower update
gulp
webpack
```


## Command line

A great tool you can - and should - consult for a lot of tasks is the Pagekit command line tool. To see a complete overview of all available commands, navigate to the Pagekit folder and run `./pagekit`. This works if you are inside an installed version of Pagekit and if the `php` command is available on your system.

```sh
./pagekit
```

## System settings

When navigating to *Settings > System* in the admin interface, you have a few settings to support you during development.

| Setting               | Description |
|-----------------------|-------------|
| *Debug mode*        | Turn on debug mode and exceptions are displayed in all their glory. Very handy when working on that new extension of yours. Remember to turn this off for production environments.  |
| *Debug toolbar*  | Display a toolbar at the bottom of the screen that helps you monitor memory usage, query count, system events, requests, headers, routes and session attributes.  |
| *Disable cache*     | You might want to turn off the cache during development to avoid the frequent "clearcache". Remember to enable the cache for production.  |
