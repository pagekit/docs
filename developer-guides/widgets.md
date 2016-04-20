# Widgets
<p class="uk-article-lead">Create widgets to render small chunks of content in different positions of your site.</p>

To determine where a widget's content will be rendered, the admin panel has a _Widgets_ section to publish widgets in specific positions that are defined by the theme. Extensions and themes can both come with widgets, with no difference in development.

<ul class="uk-list">
    <li><a href="#define-widget-positions">Define widget positions</a></li>
    <li><a href="#render-widgets">Render Widgets</a></li>
    <li><a href="#register-a-new-widget-type">Register a new widget type</a></li>
    <li><a href="#define-a-new-widget-type">Define a new widget type</a></li>
</ul>

## Define widget positions
You can define any number of widget positions in your theme's `index.php`.

```php
'positions' => [

    'sidebar' => 'Sidebar',
    'footer' => 'Footer',

],
```

## Render Widgets
To render anything published inside a widget position, you can use the View renderer instance available from your theme's `views/template.php` file.

```php
<?php if ($view->position()->exists('sidebar')) : ?>
    <?= $view->position('sidebar') ?>
<?php endif; ?>
```

## Register a new widget type
To register a new widget type, you can make use of the `widgets` property in your `index.php` file.

```php
'widgets' => [

    'widgets/hellowidget.php'

],
```

## Define a new widget type
Internally, a widget in Pagekit is a module. It is therefore defined by a module definition: A PHP array with a certain set of properties.

`widgets/hellowidget.php`:

```php
<?php

return [

    'name' => 'hello/hellowidget',

    'label' => 'Hello Widget',

    'events' => [

        'view.scripts' => function ($event, $scripts) use ($app) {
            $scripts->register('widget-hellowidget', 'hello:js/widget.js', ['~widgets']);
        }

    ],

    'render' => function ($widget) use ($app) {

        // ...

        return $app->view('hello/widget.php');
    }

];
```

This example requires the following two files to render the widget in the frontend, a JavaScript file and a PHP file. 

`js/widget.js`:

```javascript
window.Widgets.components['system-login:settings'] = {

    section: {
        label: 'Settings'
    },

    template: '<div>Your form markup here</div>',

    props: ['widget', 'config', 'form']

};
```

`views/widget.php`:

```php
<p>Hello Widget output.</p>
```

**Note** A good example of a full Widget is located at `app/system/modules/user/widgets/login.php` in the Pagekit core.
