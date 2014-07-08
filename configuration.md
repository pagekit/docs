# Configuration

<p class="uk-article-lead">Theme and extension configuration from a developer's perspective.</p>

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
    'system'  => 'hello/admin/settings.razr'
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

## Add a settings screen

If you want to keep your theme customizable and your extension configurable, you will want to offer some settings that can easily be changed without modifying any code. To do so, you can simply point to a template file that will be linked from the backend. Add the following parameter to the `theme.php` or `extension.php` (and point to your extension folder in that case).

```PHP
'settings' => array(
        'system'  => 'theme://mytheme/views/admin/settings.razr'
    ),
```

Create a file at that exact location `/themes/mytheme/views/admin/settings.razr` and add the markup for a form you want to display. When naming the form elements in a certain pattern, Pagekit will automatically handle the storing and update of settings for you. The form is supposed to send an array of config options, therefore all input fields are called `config[OPTION]`  with `OPTION` being a name you want to give that option.

Make sure the form will be submitted to `@url('@system/themes/savesettings', ['name' => 'mytheme'])` as a `POST` request just like in the following example.

**Note** As Pagekit comes pre-provided with UIkit, it is best to use UIkit's form markup.


```PHP
<form class="uk-form uk-form-horizontal" action="@url('@system/themes/savesettings', ['name' => 'mytheme'])" method="post">

    <div class="uk-form-row">
        <label for="form-sidebar-a-width" class="uk-form-label">@trans('Show Copyright')</label>
        <div class="uk-form-controls">
            <select id="form-sidebar-a-width" class="uk-form-width-large" name="config[show_copyright]">
                <option value="1"@( $config['show_copyright'] ? ' selected' : '')>Show</option>
                <option value="0"@( !$config['show_copyright'] ? ' selected' : '')>Hide</option>
            </select>
        </div>
    </div>

    <p>
        <button class="uk-button uk-button-primary" type="submit">@trans('Save')</button>
        <a class="uk-button" href="@url('@system/themes')">@trans('Close')</a>
    </p>

</form>
```


The configuration values are available in PHP.

```php
$theme = $app['theme.site']->getConfig();
$show_copyright = $theme['show_copyright'];
```
