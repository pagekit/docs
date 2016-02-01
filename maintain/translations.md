# Pagekit translation workflow

## Overview

Pagekit supports i18n for its interface. Translations are managed on [Transifex](https://www.transifex.com/pagekit/pagekit-cms).

Several tools are available to extract translatable strings from the Pagekit source files (step B) and to pull available translations from Transifex into the Pagekit source (step D and E).

Translatable strings are located in `*.pot` files, i.e. `app/system/languages/messages.pot`. Translations are located in `*.php` files, i.e. `app/system/languages/de_DE/messages.php`. While `*.pot` files are located in the repository, pull requests are to be rejected. All contributions should happen via Transifex (and the pot files are generated automatically - see step B).

## A. Configuration

You need:

1. Running Pagekit installation and the `php` command available on your terminal
2. Transifex account with access to the Pagekit project

## B. Generate translation strings

*When?* Every now and then. Basically whenever strings have been added or removed in the source code

*What happens?* The command goes through the source files and looks for strings that are printed using any of the available translate commands (in PHP or Vue templates)

- From the system: `php pagekit extension:translate`
- From any extension: `php pagekit extension:translate pagekit/blog`
- Afterwards: Commit the files to the repository and upload to Transifex (Step C.)

## C. Upload strings to be translated to Transifex

*When?* After you have generated new string files (previous step)

*What happens?* The `*.pot` files are uploaded to Transifex which updates the existing list of strings to be translated. The "translation coverage" will drop. It usually takes a few days until the most popular languages have gone up to 100% again.

*How?* Login to Transifex. For each available resource (i.e. blog, system, theme-one), choose to *Update content*. Select the corresponding `*.pot` file for this resource and hit *Update*.

## D. Fetch new translations from Transifex

*When?* Before every release.

*What happens here?* For any languages with existing translations on the Transifex project, the translations are downloaded, converted to a PHP array and saved to a separate `languages` repository.

1. Clone `https://github.com/pagekit/languages` and follow the setup instructions in the README
2. Run `php update.php` in the cloned repository. All current translations are fetched
3. Commit & push the new files

## E. Fetch translation strings from translation repository and merge into Pagekit installation

*When?* When you need translation files during development. Not needed before build, because build task does this automatically.

*What happens here?* Language files are fetched from the languages repository and sorted into the right languages folder (of system, extension-blog and theme-one).

`php pagekit translation:fetch`
