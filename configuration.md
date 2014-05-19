# Configuration

Both `theme.php` for a theme and `extension.php` for an extension are the bootstrap files for your custom code. Alongside the possibility to hook into system events and insert your own custom code, the file's main content is a configuration array with the following properties.

## General

Usable in `theme.php` and `extension.php`:

### Main

If you have code besides custom views and configuration files, you will write your own subclass of `Pagekit\Framework\Theme` (or `Pagekit\Framework\Extension`). With the `main` property you point to that class inside your namespace. If you don't set the property, Pagekit will internally create an instance of the base class `Theme` (or
    `Extension`).

```php
'main' => Pagekit\\Hello\\HelloExtension
```

### Autoload

Autoload all classes from the given namespace using [PSR-4](http://www.php-fig.org/psr/psr-4/).

```php
'autoload' => array(
    'Pagekit\\Hello\\' => 'src'
),
```

### Resources

The `resources` property is an array with two keys.

| Property  | Description |
|-----------|-------------|
| `export`    | Register the `view://` and `asset://` stream wrapper to point to your theme's (or extension's) `views` and `assets` directory. |
| `overrides` | Possibility to overwrite `view://` and `asset://` properties, for example to allow for custom views of a system extension. |

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

**Note** You can now use `@url('asset://mytheme/images/foo.png')` to generate image paths in your views or `$app['url']->to('asset://mytheme/images/foo.png')` from your controllers and classes.

### Settings

Set views for a settings screen and for custom widget options. Provide an array with the keys `settings` and/or `widgets` to link to the according view files. Settings screens will be explained further down, widgets are explained in a [separate chapter](widgets.md).



```php
'settings' => array(
    'system'  => 'hello/admin/settings.razr.php'
),
```

### Custom properties

Feel free to add your own configuration properties. From your `Theme` instance (or `Extension`), you will have have access to the configuration array via `$this->config`. Also, you can use `$this->getConfig('colours.background')` to access nested config arrays via dot notation.


```php
'colours' => array('background' => '#ccc', 'text' => '#333');
```

## Theme

Usable in `theme.php` only:

### Positions

The `positions` property allows you to define positions that you can publish widgets in. Note that this only defines the position. Your theme views have to take care of the rendering. See the chapter about [theming](themes.md)) for more information.

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

### Controllers

The `controllers` property defines the paths to your controllers. You can use [glob](http://php.net/glob) syntax. Used for automatic route registration. Set a string with glob syntax or an array with multiple paths.

    `'controllers' => 'src/Controller/*Controller.php'`

### Menu

The `menu` property is an array of menu items. Each element has a unique string identifier that should consist of the extension name or/and a telling title in case you have several menu items, i.e. `hello` or `hello: settings`. Each menu item is identified by an array with the following properties.

| Property  | Description |
|-----------|-------------|
| `label`     | The menu item label. |
| `parent`    | Identifier of parent menu item. |
| `access`    | List permissions needed for an item to be visible to a user. Attention: Implement access control inside your controller, visibility of a menu item does not take care of that. |
| `active`    | Use [glob](http://php.net/glob) syntax to match to current URL and determine, if a menu item is considered *active*. |
| `priority`  | Optional sort order, defaults to 0. |

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

### Permissions

The `permissions` property defines a list of permissions that can be assigned to user groups. The unique identifier should consist of the extension name followed by a brief identifying title of the permission (colons and spaces allowed). A description is optional and can be used to explain what this does. Check out *Users > Permissions* to see how this is displayed to the user.

```php
'system: manage url aliases' => array(
    'title' => 'Manage url aliases'
),
'system: manage users' => array(
    'title' => 'Manage users',
    'description' => 'Warning: Give to trusted roles only; security implications.'
),
```

## Settings screen

It is easy to add a settings screen to your theme (or extension).

Add to `theme.php`:

```php
'settings' => array(
    'system'  => 'theme://alpha/views/admin/settings.razr.php'
)
```

Create the according file holding a form with all your configuration options, i.e. `hello/views/admin/settings.razr.php`. This form has to be submitted to `@system/extensions/savesettings` (with a parameter specifying your theme name / extension name).

You can create any form element as long as you keep to a certain naming convention. The form is supposed to send an array of options,
therefore all input fields are called `option[message]` with `message` being a name you want to give that option. The option will be stored in the database by the `savesettings` action and can be accessed from your extension (or theme) using `@config['message']`.

Remember to include `@token()` inside your form for security purposes.

Your settings screen will be accessible from the extensions (or theme) overview.

```html
<form class="uk-form uk-form-horizontal" action="@url.route('@system/extensions/savesettings', ['name' => 'hello'])" method="post">

    <div class="uk-form-row">
        <label for="form-hello-message" class="uk-form-label">@trans('Message')</label>
        <div class="uk-form-controls">
            <input id="form-hello-message" type="text" name="config[message]" value="@config['message']">
        </div>
    </div>

  <div class="pk-options">
        <button class="uk-button uk-button-primary" type="submit">@trans('Save')</button>
        <a class="uk-button" href="@url.route('@system/system/index')">@trans('Close')</a>
    </div>

    <p>
        @trans('This settings page is just for demonstration purposes.')
    </p>

    @token()

</form>
```
