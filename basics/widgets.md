# Widgets

<p class="uk-article-lead">Use Widgets to render small chunks of content at different positions of your site.</p>

To determine where a Widget's content will be rendered, the admin area has a *Widgets* section to publish widgets in specific positions that are defined by the theme. Extensions and themes can both come with widgets, with no difference in development.

## Define widget positions in your theme

You can define any number of widget positions in your theme`s `index.php`.

```php
'positions' => [

    'sidebar' => 'Sidebar',
    'footer' => 'Footer',

],
```

## Render Widgets in your theme

To render everything published inside a Widget position, you can use the View renderer instance available from your theme's `views/template.php`:

```php
<?php if ($view->position()->exists('sidebar')) : ?>
    <?= $view->position('sidebar') ?>
<?php endif; ?>
```

## Register new Widget type

To register a new Widget type you can make use of the `widgets` property in your `index.php`.

```php
'widgets' => [

    'widgets/hellowidget.php'

],
```

## Define new Widget type

Internally, a Widget in Pagekit is a module. It is therefore defined defined by a module definition: A PHP array with a certain set of properties.

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

        return $app->view('hello:views/widget.php');
    }

];
```

This example requires an additional JS component located at `hello:js/widget.js` and a php view file `hello:views/widgets/helloview.php` which is used to render the widget in the front-end.

`js/widget.js`:

```
window.Widgets.components['system-login:settings'] = {

    section: {
        label: 'Settings'
    },

    template: '<div>Your form markup here</div>',

    props: ['widget', 'config', 'form']

};

`views/widget.php`:

```php
<p>Hello Widget output.</p>
```

**Note** A good example of a full Widget is located at `app/system/modules/user/widgets/login.php` in the Pagekit core.

## Create a Dashboard Widget

A Dashboard Widget is created as a Vue component. You should therefore make sure to read the section on [Data Reactive Interface Design](developer/data-reactive-ui.md) and setup [Webpack](../tools/webpack-gulp.md) before continuing with the following steps.

1. Create a Vue component for your Widget, save it under your extension: `app/components/widget-hello.js`

```vue
<template>

    <form class="pk-panel-teaser uk-form uk-form-stacked" v-if="editing">

        <div class="uk-form-row">
            <label for="form-hello-title" class="uk-form-label">{{ 'Title' | trans }}</label>

            <div class="uk-form-controls">
                <input id="form-hello-title" class="uk-width-1-1" type="text" name="widget[title]" v-model="widget.title">
            </div>
        </div>        

    </form>

    <div v-if="!editing">

        <h3 v-if="widget.title">{{ widget.title }}</h3>

    </div>

</template>

<script>

    module.exports = {

        type: {

            id: 'hello',
            label: 'Hello Dashboard',
            defaults: {
                title: ''
            }

        },

        replace: false,

        props: ['widget', 'editing']

    }

    window.Dashboard.components['hello'] = module.exports;

</script>

```

2. Add the Vue component to you extension's `webpack.config.js`:

```
entry: {
    "dashboard": "./app/components/dashboard.vue",
    // ...
},
```

Run `webpack` once to have the new component be available in your bundle. When developing it is a good idea to have `webpack --watch` running in the background.

3. Register the bundled JavaScript file inside your extension's `index.php`.

Make sure the file is loaded *before* the dashboard initialization code by using
the tilde: `~dashboard`.


```php
'events' => [

    'view.scripts' => function ($event, $scripts) use($app) {
        $scripts->register('widget-hello', 'hello:app/bundle/dashboard.js', '~dashboard');
    }

]
```
