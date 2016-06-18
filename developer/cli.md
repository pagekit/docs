# Command Line Interface

<p class="uk-article-lead">Pagekit exposes some of its core functionality to developers via its command line interface (CLI). A number of commands are available that offer useful tools and helpers.</p>

## Basic usage

Open a terminal and navigate to the directory of an existing Pagekit installation. The script `pagekit` (no file extension) in that directory is a PHP script that can be run from the command line.

```sh
cd /var/www/pagekit   # navigate to pagekit directory
./pagekit             # run pagekit CLI script
```

**Note** You might need to make this script executable using `chmod +x pagekit`. Alternatively you can explicetly call your PHP interpreter `php pagekit`.

When simple invoking the CLI tool without any arguments, it will output the Pagekit version number, some basic usage information and list the available commands that you can use.

```txt
$ ./pagekit
Pagekit version 1.0.3

Usage:
  command [options] [arguments]

Options:
  -h, --help            Display this help message
  -q, --quiet           Do not output any message
  -V, --version         Display this application version
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  archive              Archives an extension or theme
  build                Builds a .zip release file
  clearcache           Clears the system cache
  help                 Displays help for a command
  install              Installs a Pagekit package
  list                 Lists commands
  migrate              Migrates Pagekit
  self-update          Checks for newer Pagekit versions and installs the latest
  setup                Setup a Pagekit installation
  start                Starts the built-in web server
  uninstall            Uninstalls a Pagekit package
  update               Updates dependencies of Pagekit packages
 extension
  extension:translate  Generates extension's translation .pot/.po/.php files
 translation
  translation:fetch    Fetches current translation files from languages repository
```

To run a command, you can add arguments when invoking the CLI tool. For example, installing the Hello extension from the marketplace will look as follows.

```sh
./pagekit install pagekit/hello
```

## Available Commands

The available commands include helpers for extension and theme developers, but also tools that make maintaining the Pagekit project itself easier for the Pagekit developer team (such as the `build` command).

### Build package archive

To create an installable `*.zip` archive from any theme or extension, run this command and provide the path to the package you want to build. The following command will create a file called `pagekit-hello.zip` in the top folder of the Pagekit installation.

Example:

```sh
./pagekit archive pagekit/hello
```

Usage, arguments and options:

```sh
Usage:
  archive [options] [--] <name>

Arguments:
  name                  Package name

Options:
      --dir[=DIR]       Write the archive to this directory
```

### Build Pagekit release archive

This command is used by the Pagekit maintainers to build a Pagekit release package. It will create a `*.zip` archive in the root folder of the Pagekit installation. This release package can then be used both as a package as you can download it from the official Pagekit website, or as an custom installation that you could deliver, e.g. to your clients.

Example:

```sh
./pagekit build
```

Usage (no arguments, no options):

```sh
Usage:
  build
```

### Clear the cache

To empty the cache you can use the `clearcache` command. This command removes all `*.cache` files from the cache directory, which is located at `tmp/cache` in a usual Pagekit installation.

Example:

```sh
./pagekit clearcache
```

Usage (no arguments, no options):

```sh
Usage:
  clearcache
```

### Display usage information for any CLI command

To learn what a CLI command actually does and how it is used, you can use the `help` command.

Example:

```sh
./pagekit help install
```

Usage and arguments:

```
Usage:
  help [options] [--] [<command_name>]

Arguments:
  command               The command to execute
  command_name          The command name [default: "help"]

Options:
      --format=FORMAT   The output format (txt, xml, json, or md) [default: "txt"]
```

### Install a package from the Pagekit marketplace

Example:

```sh
./pagekit install pagekit/hello
```

Usage, arguments and options:

```sh
Usage:
  install [options] [--] <packages> (<packages>)...

Arguments:
  packages              [Package name]:[Version constraint]

Options:
      --prefer-source   Forces installation from package sources when possible, including VCS information.
```

### List available commands

You can kist all available CLI commands with the `list` command. This creates the same output as running the CLI script without any parameters.

Example:

```sh
./pagekit list
```

Usage, arguments and options:

```sh
Usage:
  list [options] [--] [<namespace>]

Arguments:
  namespace            The namespace name

Options:
      --raw            To output raw command list
      --format=FORMAT  The output format (txt, xml, json, or md) [default: "txt"]
```

### Run Pagekit migrations

After a new Pagekit version has been installed, the system sometimes needs to apply changes to its database structure. These changes are grouped in so called _migrations_. To run any migrations that might need to be run, you can use the `migrate` command. In general though, you do not need to do this explicitely. When you login to the admin area, Pagekit will also check if any migrations are available and will run them if needed.

Example:

```sh
./pagekit migrate
```

Usage (no arguments or options):

```sh
Usage:
  migrate
```

### Upgrade Pagekit installation

Upgrade your Pagekit installation from the terminal. Optionally, you can provide a link to the new Pagekit package that should be used for running the upgrade. In that case, you also need to provide a valid SHA hash that is used to verify the downloaded file. If you do not provide URL and hash, the command will use the most recent Pagekit package from pagekit.com.

Example:

```sh
./pagekit self-update
```

Usage and options:

```sh
Usage:
  self-update [options]

Options:
  -u, --url=URL
  -s, --shasum=SHASUM
```

### Setup a full Pagekit installation

You can run a terminal command from a freshly downloaded Pagekit installation package to run the installation without opening the browser. This could be used for automated installs, either for yourself or client projects.

Example to install Pagekit using SQLite and default admin user:

```sh
./pagekit setup --password=<SOMETHING-SECURE>
```

Usage and options:

```sh
Usage:
  setup [options]

Options:
  -u, --username=USERNAME      Admin username [default: "admin"]
  -p, --password=PASSWORD      Admin account password
  -t, --title[=TITLE]          Site title [default: "Pagekit"]
  -m, --mail[=MAIL]            Admin account email [default: "admin@example.com"]
  -d, --db-driver=DB-DRIVER    DB driver ('sqlite' or 'mysql') [default: "sqlite"]
      --db-prefix[=DB-PREFIX]  DB prefix [default: "pk_"]
  -H, --db-host[=DB-HOST]      MySQL host
  -N, --db-name[=DB-NAME]      MySQL database name
  -U, --db-user[=DB-USER]      MySQL user
  -P, --db-pass[=DB-PASS]      MySQL password
  -l, --locale[=LOCALE]        Locale
```

### Start the built-in webserver

You actually do not need a full Apache server setup. You can instead run the `start` command for a self-contained server instance running on your local machine. The command will keep running until you hit CTRL + C to quit the server instance.

```sh
./pagekit start --server=127.0.0.1:8080
```

Usage and options:

```
Usage:
  start [options]

Options:
  -s, --server[=SERVER]  Server name and port [default: "127.0.0.1:8080"]
```

### Uninstall a Pagekit package

Removes a theme or extension from your Pagekit installation.

Example:

```sh
./pagekit uninstall pagekit/hello
```

Usage and arguments:

```
Usage:
  uninstall <packages> (<packages>)...

Arguments:
  packages              [Package name]
```

### Parse translation strings from package

When creating a theme or extension, you can and should use Pagekit's internationalization system to make sure the user interface can be displayed in multiple languages. Basically, this works by wrapping strings in special function calls (like `__('Translate me!')` in PHP templates or `{{ 'Translate me!' |trans }}` in Vue templates. The `extension:translate` commands will find all occurences of such function calls in a given theme or extension, and collect all translatable strings for you and write them to the `languages/` subdirectory of the specified package. These files can then be used to create translations for your package, for example using a service like [Transifex](https://www.transifex.com/pagekit/pagekit-cms/).

Example:

```sh
./pagekit extension:translate pagekit/blog
```

Usage and arguments:

```sh
Usage:
  extension:translate [<extension>]

Arguments:
  extension             Extension name
```

### Fetch new translation files

**Deprecated Warning** This command will be replaced.

This command is used by the Pagekit core maintainers to fetch translations from the Pagekit translation repository. The translation workflow and file structure is currently under discussion and this command will probably replaced in a future version of Pagekit.

Example:

```sh
./pagekit translation:fetch
```

Usage:

```sh
Usage:
  translation:fetch
```
