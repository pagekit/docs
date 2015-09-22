# Translation

<p class="uk-article-lead">Pagekit includes capabilities to display messages in
different languages. This allows the interface to be localized for any number.
of languages.</p>

**Note** There is a difference between languages and locales, as there might be
different versions of a certain language spoken in a particular region (for
example `en_GB` vs. `en_US`).

## Language files

Pagekit's core extensions come with language files provided.

```
/app/system/languages

  /en_US
    messages.php
    formats.json
    languages.json
    territories.json

  /de_DE
    messages.php
    formats.json
    languages.json
    territories.json

  messages.pot
```

| Path                     | Description                                       |
|--------------------------|---------------------------------------------------|
| `messages.pot`           | This is the main file with all translatable strings. Used as a base to create localized versions. Regularly uploaded to Transifex by the Pagekit maintainers. |
| `/en_US` <br> `/de_DE`   | Each folder corresponds to a locale               |
| `xx_XX/formats.json`     | Localized Format strings                          |
| `xx_XX/languages.json`   | Localized language names                          |
| `xx_XX/territories.json` | Localized territory names                         |

Formats, languages and territories are defined by the
[Unicode Common Locale Data Repository](http://cldr.unicode.org/).

The translation is a simple mapping from the English version of the string to a
localized version (`de_DE/messages.php`).

```php
"No database connection." => "Keine Datenbankverbindung."
```

## Usage

To get the localized version of a string, you can use the global function
`__(...)` inside a PHP file. In a Vue template, use the trans filter on a
string.

Pagekit will automatically check the active locale and return a localized version
of the string available.

In PHP files:

```php
echo __('Save');
```

In Vue templates:

```vue
{{ 'Save' | trans }}
```

### Variables

Suppose you have a name stored in `$name` and want to include it in a localized
string. You can pass parameters to the translation functions to do simple string
replacement.

```php
$message = __("Hello %name%!", ["%name%" => $name]);
```

In Vue templates you can pass parameters to the `trans` filter.

```vue
{{ 'Installing %title%' | trans {title:pkg.title} }}
```

### Pluralisation

To choose from several messages depending on a number, you can use a syntax of
specifying alternatives and determine certain numbers or even intervals. Use
the `_c(...)` function that also supports replacement parameters.

```php
$message = _c('{0} No Item enabled.|{1} Item enabled.|]1,Inf[ Items enabled.', count($ids))
```

To specify the number that matches, you can use the number in curly brackets
`{0}`, labels to make it more readable `one:` `more:` or intervals `]1,Inf]`.
These variants can also be mixed.

In Vue temmplates you can use the `transChoice` filter.

```vue
{{ '{0} %count% Files|{1} %count% File|]1,Inf[ %count% Files' | transChoice count {count:count} }}
```

An interval can represent a finite set of numbers: `{1,2,3,4}` and it can
represent numbers between two numbers: `[1, +Inf]`, `]-1,2[`. The left delimiter
can be `[` (inclusive) or `]` (exclusive). The right delimiter can be
`[` (exclusive) or `]` (inclusive). Beside numbers, you can use `-Inf` and
`+Inf` for the infinite.

## Create Language files for your extension

To translate your own extension, use the command line tool which will extract all translatable strings.

```bash
./pagekit extension:translate pagekit/extension-hello
```

This will create `/packages/pagekit/extensions-hello/languages/messages.pot` including all strings that have been found. These have been collected by finding all calls to one of the translation functions `__()`, `_c()` or `@trans`, `@transchoice` in your views.

Create a folder for the locale you want to provide (for example `/de_DE`). Copy the `messages.pot` to `/de_DE/messages.pot` and start filling out the `msgstr` property for each string. If you do not want to to this manually, you can use any of the available tools, a popular one is [poEdit](http://www.poedit.net/). An advantage of tools like this is the automatic creation of the binary `*.mo` file.

## Update language files

When you add, remove and change some strings in your extension and re-run `./pagekit extension:translate hello`, the `messages.pot` file will be regenerated. You now need to update your localized `*.po` and `*.mo` files. Of course this can be done manually. If you're using [poEdit](http://www.poedit.net/) though, there's a better method.

1. Open the already localized file in poEdit, i.e. `/de_DE/messages.po`
2. From the menu, choose *Catalog > Update from POT file*
3. Select the newly generated `messages.pot`.
4. In the next dialog, include translations for any new strings and save the file. `messages.mo` will automatically be saved as well.

## How a locale is determined

When the Installer is run, the locale is determined automatically by checking what locales the user's browser accepts. When Pagekit has been set up, you can set the language in the admin area.

**Note** You can only select languages that are available for the system extension.

## Working with message domains

The `__(...)`/`@trans` function and the `_c(...)`/`@transchoice` function have a third parameter to set a *domain*. The default domain is called `messages`, which is why we have been dealing with `messages.*` files so far. All extensions share their strings in this domain. That is why strings translated by the system extension can be used right away without the need to translate them again. This includes common terms like *Save*, *Error* or the name of a *month*.

Indeed, when we called `./pagekit extension:translate hello` earlier, the resulting `messages.pot` did not include any of the system's messages, even if they occurred in the hello extension.

There might be the case when you do not want to share messages from the default domain. Just set your own domain and regenerate the `*.pot` files. You can do this for individual strings or set the parameter on all strings to keep your localization completely separate from the system.

```php
$msg = __("Hello Universe", [], "hello");
```


```
@trans("Hello Universe", [], "hello")
```
