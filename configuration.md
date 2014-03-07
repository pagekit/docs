# Configuration

Both `theme.php` for a theme and `extension.php` for an extension are the bootstrap files for your custom code. Alongside the possibility to hook into system events and insert your own custom code, the file's main content is a configuration array with the following properties.

## General

Usable in `theme.php` and `extension.php`:

  - **main**: If you have code besides custom views and configuration files, you will write your own subclass of `Pagekit\Framework\Theme` (or `Pagekit\Framework\Extension`). With the `main` property you point to that class inside your namespace. If you don't set the property, Pagekit will internally create an instance of the base class `Theme` (or
    `Extension`).

    Example: `'main' => Pagekit\\Hello\\HelloExtension`

  - **autoload**: Autoload all classes from the given namespace using [PSR-4](http://www.php-fig.org/psr/psr-4/).

    ```php
    'autoload' => array(
        'Pagekit\\Hello\\' => 'src'
    ),
    ```

  - **resources**: Array with two keys.

    - **export**: Register the `view://` and `asset://` stream wrapper to point to your theme's (or extension's) `views` and `assets` directory.

    - **overrides**: Possibility to overwrite `view://` and `asset://` properties, for example to allow for custom views of a system extension.

    ```php
    'resources' => array(
      'export' => array(
        'view'  => 'views',
        'asset' => 'assets'
      ),
      'overrides' => array(
        ...
      )
    ),
    ```

    You can now use `@url('asset://mytheme/images/foo.png')` to generate image paths in your views or `$app['url']->to('asset://mytheme/images/foo.png')` from your controllers and classes.

  - **settings**: The route to your settings screen. If set, a *Settings* button will appear next to your theme (or extension) in the admin interface.

    Example: `'settings' => '@hello/hello/settings'`

  - Feel free to add your own configuration properties. From your `Theme` instance (or `Extension`), you will have have access to the configuration array via `$this->config`. Also, you can use `$this->getConfig('colours.background')` to access nested config arrays via dot notation.

    Example:

    ```php
    'colours' => array('background' => '#ccc', 'text' => '#333');
    ```

## Theme

Usable in `theme.php` only:

- **positions**: As we've seen before, you can define positions that you can publish widgets (menus, ...) in. Note that this only defines the position. Your theme views have to take care of the rendering. (See chapter about [theming](theme.md))

  ```php
  return array(
      'positions' => array(
          'logo'      => 'Logo',
          'sidebar'   => 'Sidebar',
          'footer'    => 'Footer',
          ...
      ),
      ...
  ```

## Extension

Usable in `extension.php` only:

  - **controllers**: The paths to your controllers. You can use [glob](http://php.net/glob) syntax. Used for automatic route registration. Set a string with glob syntax or an array with multiple paths.

    Example: `'controllers' => 'src/Controller/*Controller.php'`

  - **menu**: Array of menu items. Each element has a unique string identifier that should consist of the extension name or and a telling title in case you have several menu items, i.e. `hello` or `hello: settings`. Each menu item is identified by an array with the following properties.

      - **label**: Label of the menu item. String is localized automatically if a translation is available.
      - **parent**: Identifier of parent menu item
      - **url**: Named route
      - **access**: List permissions needed for an item to be visible to a user. Attention: Implement access control inside your controller, visibility of a menu item does not take care of that.
      - **active**: Use [glob](http://php.net/glob) syntax to match to current url and determine, if a menu item is considered *active*
      - **priority**: Optional sort order, defaults to 0.

    ```php
    'menu' => array(
        'hello' => array(
            'label'  => 'Hello',
            'url'    => '@hello/hello/index',
            'active' => '/admin/hello*',
            'access' => 'hello: manage hellos'
        )
    ),
    ```

  - **permissions**: List of permissions that can be assigned to user groups. the unique identifier should consist of the extension name followed by a brief identifying title of the permission (colons and spaces allowed). A description is optional and can be used to explain what this does. Check out *Users > Permissions* to see how this is displayed to the user.

  ```php
  'system: manage url aliases' => array(
      'title' => 'Manage url aliases'
  ),
  'system: manage users' => array(
      'title' => 'Manage users',
      'description' => 'Warning: Give to trusted roles only; security implications.'
  ),
  ```
