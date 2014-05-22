# Translation

Pagekit includes capabilities for extensions and themes to display their messages in a language according to the set locale. Notice that there is a difference between languages and locales, as there might be different versions of a certain language spoken in a particular region (for example `en_GB` vs. `en_US`).

## Language files

Pagekit's core extensions come with language files provided.

```
/languages

  /en_US
    messages.po
    messages.mo

  /de_DE
    messages.po
    messages.mo

  messages.pot
```

| Folder / File  | Description |
|----------------|-------------|
| `/en_US` <br> `/de_DE`           | Each folder corresponds to a locale, its name is used to match the user's locale. |
| `messages.po`                    | The localized version based on `messages.pot`. |
| `messages.mo`                    | A binary version compiled from `messages.po`. Translation will work without this file, but it will probably be a tiny bit slower. |
| `messages.pot`                   | This is the main file to include all translatable strings with their default translation (usually just the English version). This file is used as a base to create localized versions. |

The translation is a simple mapping from the English version of the string to a localized version.

```
msgid "No database connection."
msgstr "Keine Datenbankverbindung."
```


## Usage

To get the localized version of a string, you can use the global function `__(...)` or `@trans(...)` when inside a view. Pagekit will automatically check the user's locale, check if there is a localized version of the string available or return the string ID instead. That is why instead of using keys like `"hello_save_label"`, we simply use `"Save"` to identify the message.

```PHP
$message = __("Save");
```

```HTML
<a href="http://example.com">@trans("Visit website")</a>
```

### Variables

Suppose you have a name stored in `$name` and want to include it in a localized string. You can pass parameters to the translation functions to do simple string replacement.

```PHP
$messages = __("Hello %name%!", array("%name%" => $name));
```


```HTML
@trans("Hello %name%!", ["%name%" => name])
```

### Pluralisation

To choose from several messages depending on a number, you can use a syntax of specifying alternatives and determine certain numbers or even intervals. Use the `_c(...)` function (`@†ranschoice` in templates) that also supports replacement parameters.

```HTML
@transchoice("{0}No posts|{1}One name|]1,Inf] %names% names", names|length, ["%names%" => names|length])
```

To specify the number that matches, you can use the number in curly brackets `{0}`, labels to make it more readable `one:` `more:` or intervals `]1,Inf]`. These variants can also be mixed.

```HTML
@transchoice("{0}: No names|one: One name|more: %names% names", names|length, ["%names%" => names|length])
```

An interval can represent a finite set of numbers: `{1,2,3,4}` and it can represent numbers between two numbers: `[1, +Inf]`, `]-1,2[`. The left delimiter can be `[` (inclusive) or `]` (exclusive). The right delimiter can be `[` (exclusive) or `]` (inclusive). Beside numbers, you can use `-Inf` and `+Inf` for the infinite.

## Create Language files

To translate your extension, use the command line to which will extract all translateable string from your extension.

```bash
./pagekit extension:translate hello
```

This will create `/extension/hello/languages/messages.pot` which includes all translateable strings. These have been collected by finding all calls to one of the translation functions `__()`, `_c()` or `@trans`, `@transchoice` in your views.

Create a folder for the locale you want to translate for (for example `/de_DE`). Copy the `messages.pot` to `/de_DE/messages.pot` and start filling out the `msgstr` property for each string. If you do not want to to this manually, you can use any of the available tools, a popular one is [poEdit](http://www.poedit.net/). The advantage of tools like this is the automatic creation of the binary `*.mo` file.

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

Indeed, when we called `./pagekit extension:translate hello` earlier, the resulting `messages.pot` did not include any of the system's messages, even if they occured in the hello extension.

There might be the case when you do not want to share messages from the default domain. Just set your own domain and regenerate the `*.pot` files. You can do this for individual strings or set the parameter on all strings to keep your localization completely separate from the system.

```PHP
$msg = __("Hello Universe", array(), "hello");
```


```
@trans("Hello Universe", [], "hello")
```
