# Widgets

<p class="uk-article-lead">Use Widgets to render small chunks of content at different positions of your site.</p>

To determine where a Widget's content will be rendered, the admin area has a *Widgets* section to publish widgets in specific positions that are defined by the theme. Extensions and themes can both come with widgets, with no difference in development.

## Basic structure

The central location of the widget's behaviour is defined in a class
that must implement the interface `Pagekit\Widget\Model\TypeInterface`.
In the following example the class extends `ApplicationAware` in order to
have `$this(‘view')` available for view rendering (and all other Application
services actually). Don't be confused by this, a detailed explanation follows
in the [Application](application.md) chapter.

**Note** Wrapping a string in a call of the double underscore function `__(...)` makes it translatable. More on that in the [translation chapter](translation.md).

`hello/src/HelloWidget.php`:

```php
<?php

namespace Pagekit\Hello;

use Pagekit\Framework\ApplicationAware;
use Pagekit\Widget\Model\TypeInterface;
use Pagekit\Widget\Model\WidgetInterface;

class HelloWidget extends ApplicationAware implements TypeInterface
{
    /* unique identifier */
    public function getId()
    {
        return 'widget.hello';
    }

    /* name displayed in admin area */
    public function getName()
    {
        return __('Hello Widget!');
    }

    /* description displayed in admin area */
    public function getDescription(WidgetInterface $widget = null))
    {
        return __('Hello Demo Widget');
    }

    /* returns information representing the current configuration
    of the widget. A weather widget would return the configured
    location for example. Displayed in the widget listing in
    Settings > Dashboard.
    */
    public function getInfo(WidgetInterface $widget)
    {
        return __('Hello Demo Widget');
    }

    /* Rendering the widget. Will usually render a view */
    public function render(WidgetInterface $widget, $options = [])
    {
        return $this('view')->render('extension://hello/views/widget.razr');
    }

    /* Define a form for the Advanced section in the widget admin settings */
    public function renderForm(WidgetInterface $widget)
    {
        return __('Hello Widget Form.');
    }
}
```

**Important** In order for this example to work make sure to create a view file
`hello/views/widget.php` that could contain the following.

```HTML
Hello Widget!
```

## Read and write widget configuration

As you can see, the methods `getInfo`, `render` and `renderForm` have a `$widget`
parameter of the type `WidgetInterface`. That object holds a representation
of the widget's configuration (actually the data stored in the `system_widget`
table in the database). Use this object to read and write widget configuration.

Read properties with:

| Configuration | Description |
|---------------|-------------|
| `getId()`       | Get the widget's unique ID. |
| `getTitle()`    | Get the widget's title. |
| `getType()`     | Get the widget's type, `widget.hello` in our case. |
| `getPosition()` | Get the position the widget is assigned to. |
| `getStatus()`   | Get the widget's status (`WidgetInterface::STATUS_ENABLED` or `WidgetInterface::STATUS_DISABLED`). |
| `getSettings()` | Get the complete settings array. |

Read and write single settings properties with:

| Configuration | Description |
|---------------|-------------|
| `get($name, $default = null)`  | Get a specific widget setting.   |
| `set($name, $value)`           | Write a specific widget setting. |
| `remove($name)`                | Remove a specific widget setting.|

## Register the widget

Now that the basic widget behaviour has been defined, we need to register a widget instance. This is done in the `extension.php` or
`theme.php`.

```php
<?php

namespace Pagekit\Hello;

use Pagekit\Extension\Extension;
use Pagekit\Framework\Application;
use Pagekit\Widget\Event\RegisterWidgetEvent;

class HelloExtension extends Extension
{

    public function boot(Application $app)
    {
        parent::boot($app);

        $app->on('system.widget', function(RegisterWidgetEvent $event) {
            $event->register(new HelloWidget);
        });

    }
}
```

When our extension boots we make sure to call the `boot` method of our parent. Then we are free to hook into the Application's `system.widget` event with a callback.
With that callback, we can now access the `widgets` service of our Application
instance to register our `HelloWidget`. Note how `register` requires the
given class to implement `TypeInterface` as we've seen in the sample code
above.

## Try out your widget

Go to the admin area and make sure to enable the extension. You can download
the `HelloExtension` from the marketplace, which will include the `HelloWidget`.

Switch to the *Widgets* tab. Click the *Add Widget* button and choose *Hello
Widget!*. Notice how the form you're presented with has an *Advanced* section
that says *Hello Widget Form.* right now. This output is generated by
`HelloWidget::renderForm` as seen above.

Choose a *Title* for your widget and make sure to set the *Status* to enabled and
to choose a position (for example *Top*). The position will determine the location
where the output will be rendered. Switch to the *Assignment* tab and select all
pages where the widget should be visible. We choose *Home* in this example. *Save* your widget and navigate to the Home page to see your rendered widget
in action.

## Create a dashboard widget

You can also create widgets for the *Dashboard* in the admin area. Those work
exactly the same, the only difference being the way the widget is registered
at the Application (and the configuration is stored in or in the `system_user`
table, because dashboard widgets are specific for every user's dashboard).

```php
<?php

namespace Pagekit\Hello;

use Pagekit\Extension\Extension;
use Pagekit\Framework\Application;
use Pagekit\Widget\Event\RegisterWidgetEvent;

class HelloExtension extends Extension
{

    public function boot(Application $app)
    {
        parent::boot($app);

        $app->on('system.dashboard', function(RegisterWidgetEvent $event) {
            $event->register(new HelloWidget);
        });
    }
}
```

To see your widget in action, add it to your admin dashboard:

1. Open *Settings > Dashboard* in the admin area.
2. Click *Add Widget* and choose the *Hello Widget*.
3. Save and go to the admin dashboard to view the widget