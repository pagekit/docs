# Dashboard Widgets
<p class="uk-article-lead">Use Dashboard Widgets to render small chunks of content at the admin panel's dashboard.</p>

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

1. Add the Vue component to you extension's `webpack.config.js`:

```
entry: {
    "dashboard": "./app/components/dashboard.vue",
    // ...
},
```

Run `webpack` once to have the new component be available in your bundle. When developing it is a good idea to have `webpack --watch` running in the background.
1. Register the bundled JavaScript file inside your extension's `index.php`.

Make sure the file is loaded _before_ the dashboard initialization code by using the tilde: `~dashboard`.

```php
'events' => [

    'view.scripts' => function ($event, $scripts) use($app) {
        $scripts->register('widget-hello', 'hello:app/bundle/dashboard.js', '~dashboard');
    }

]
```
