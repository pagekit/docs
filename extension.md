# Extensions

You have started using Pagekit and want to add to its functionality - great!

Some of Pagekit's default functionality has been implemented using extensions (pages, the installer and even the admin interface). Also, search for the *Hello Extension* in the marketplace as it is a collection of examples and best-practices for extension development. Just browse the `extensions` directory for a bit.

There are several kinds of extensions, yet they all follow the same basic pattern. This chapter will give you a brief overview on the general approach.

Just like with themes, the command line tool supports you in generating the skeleton file structure for your new extension:

```
pagekit extension:generate <extension_name>`
```

To get started, let's build a simple extension called `hello` (You can download the complete Hello extension from the marketplace).

```
cd path/to/pagekit
pagekit extension:generate hello
```

You will be asked for the following information:

- *Title*: Extension title that is shown in the backend, e.g. 'Hello Extension'
- *Author*: Your name
- *Email*: Your email address
- *PHP Namespace*: PHP namespace of the extension, e.g. `Pagekit\Hello`

This produces the following file structure inside the directory `extensions/hello`. You can also just create the files manually, in case you do not want to use the command line tool.

```
.
+-- src
|   +-- Controller
|   |   +-- DefaultController.php
|   +-- HelloExtension.php
+-- extension.json
+-- extension.php
```

- `src/Controller/DefaultController.php`: Default controller class
- `src/HelloExtension.php`: Main class, boots the extension
- `extension.json`: Metadata for the system and the marketplace
- `extension.php`: Extension bootstrap and configuration

Can't see your changes in the frontend? Don't forget to enable your extension in the admin area.

## Metadata

*extension.json* is a JSON representation of your extension's metadata (license, author etc). This is mainly needed when distributing the extension via the marketplace, but is also important for how the extension is listed in the backend.

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

*extension.php* contains PHP code for extension configuration. The default file that is created takes care of autoloading your controllers and other classes in your namespace, it also determines your main Extension instance (with `HelloExtension` being a subclass of `Pagekit\Framework\Extension`). It also registers the resource locator `view://` to be expanded into the absolute path of the `views` folder inside your extension.

Learn more about configuration options in the [configuration chapter](configuration.md).

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
