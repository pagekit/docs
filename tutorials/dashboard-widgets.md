# Dashboard Widgets

<p class="uk-article-lead">The Dashboard is the first screen that is shown after logging in to the admin panel. It gives an overview of the website and contains multiple Dashboard Widgets. You can create your own Dashboard Widget that can display useful information or offer shortcuts to other areas of your extension.</p>


A Dashboard Widget is created as a [Vue.js](http://vuejs.org) component. The following example explains how to create the Widget using a webpack configuration in your extension. If you are not sure what Vue.js and Webpack are or how they work together, you might want to read the [Vue.js and Webpack](../developer/vuejs-and-webpack.md) article first.

## Create a Vue component

To get started, create a Vue component for your Widget and save it in a subfolder of your extension extension: `<extension>/app/components/widget-hello.vue`

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

    <div v-else>

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

## Add to webpack config

Now, add the Vue component to your extension's `webpack.config.js`:

```
entry: {
    "dashboard": "./app/components/dashboard.vue",
    // ...
},
```

Run `webpack` once to have the new component be available in your bundle. When developing it is a good idea to have `webpack --watch` running in the background.

## Make sure the Dashboard widget is loaded

Register the bundled JavaScript file inside your extension's `index.php`.

Make sure the file is loaded _before_ the dashboard initialization code by using the tilde: `~dashboard`.

```php
'events' => [

    'view.scripts' => function ($event, $scripts) use($app) {
        $scripts->register('widget-hello', 'hello:app/bundle/dashboard.js', '~dashboard');
    }

]
```
