# Extensions

<p class="uk-article-lead">Add features of your own to Pagekit.</p>

Some of Pagekit's default functionality has been implemented using extensions (Pages, the Installer and even the Admin interface). Also, search for the *Hello Extension* in the marketplace as it is a collection of examples and best-practices for extension development. Just browse the `/extensions` directory for a bit.

## Basic structure

There are several kinds of extensions, yet they all follow the same basic pattern. This chapter will give you a brief overview on the general approach. Just like with themes, the command line tool supports you in generating the skeleton file structure for your new extension:

```bash
pagekit extension:generate <extension_name>
```

To get started, let's build a simple extension called *Hello*. 

**Note** You can download the complete *Hello extension* from the marketplace.

```bash
cd path/to/pagekit
pagekit extension:generate hello
```

You will be asked for the following information:

| Information     | Description |
|-----------------|-------------|
| *Title*         | Extension title that is shown in the backend, e.g. 'Hello Extension'. |
| *Author*        | Enter your name. |
| *Email*         | Enter your email address. |
| *PHP Namespace* | PHP namespace of the extension, e.g. `Pagekit\Hello`. |

This produces the following file structure inside the directory `/extensions/hello`. You can also just create the files manually, in case you do not want to use the command line tool.

| Folder / File | Description |
|---------------|-------------|
| `/src` | Place all your code in this folder. |
| `/src/Controller` | Place your controllers here. |
| `/src/Controller/DefaultController.php`| This is the default controller class. |
| `/src/HelloExtension.php` | The main class of the extension, that will boot the extension. |
| `extension.json` | Holds metadata for the system and the marketplace. |
| `extension.php` | Holds the extension bootstrap and configuration code. |

**Important** Can't see your changes in the frontend? Don't forget to enable your extension in the admin area.

## Metadata

`extension.json` is a JSON representation of your extension's metadata (license, author etc). This is mainly needed when distributing the extension via the [marketplace](marketplace.md), but is also important for how the extension is listed in the backend.

```json
{
    "name": "hello",
    "version": "0.8.0",
    "type": "extension",
    "title": "Hello",
    "description": "A blueprint to develop your own extensions.",
    "license": "MIT",
    "authors": [
        {
            "name": "Pagekit",
            "email": "info@pagekit.com",
            "homepage": "http://pagekit.com"
        }
    ],
    "extra": {
        "image": "extension.svg"
    }
}
```

## Configuration

`extension.php` contains PHP code for extension configuration. The default file that is created takes care of autoloading your controllers and other classes in your namespace. It also determines your main Extension instance (with `HelloExtension` being a subclass of `Pagekit\Framework\Extension`). It also registers the resource locator `view://` to be expanded into the absolute path of the `views` folder inside your extension.

Learn more about configuration options in the [Configuration chapter](configuration.md).

```php
<?php

return array(
    'main' => 'Pagekit\\Hello\\HelloExtension',
    'autoload' => array(
        'Pagekit\\Hello\\' => 'src'
    ),
    'resources' => array(
        'export' => array(
            'view' => 'views'
        )
    ),
    'controllers' => 'src/Controller/*Controller.php'
);
```
