# Themes

<p class="uk-article-lead">Create a theme to change the look of your site.</p>

To get started with creating your own theme, read the [Theme Guide](guide-theme.md). Afterwards, this documents offers more information and advanced configuration possibilities.

## Doing more with themes

Themes and extensions in Pagekit are very much the same. Try not to think in terms of developing a theme vs. developing an extension but rather understand that you have access to Pagekit's framework all the time.

Most importantly, the module definition in your theme's `index.php` can contain all properties, no matter if you have a theme or an extension. If you want to add a Settings screen to the backend, register additional tabs to the Site Tree or the Widget management interface, all of that works exactly the same.

## Render sub-view

In some cases you want to split your template in several files. This can be useful if you either have a lot of markup or if you have conditional rendering.

A classic example would be when you want to allow different versions of the menu to be rendered. In this case you should try to avoid nesting several `if` clauses and instead create additional files in your `views` folder.

```php
<?= $view->menu('main', 'menu-navbar.php') ?>
```

`menu-navbar.php` could look as follows:

```php
<ul class="uk-navbar-nav">

    <?php foreach ($root->getChildren() as $node) : ?>
    <li>

    <!-- ... more markup ... -->

    </li>
    <?php endforeach ?>

</ul>
```

The same works for widget positions.

```php
<?= $view->position('hero', 'position-grid.php') ?>
```

`position-grid.php` can look as follows:

```php
<?php foreach ($widgets as $widget) : ?>
<div class="uk-width-1-<?= count($widgets) ?>">

    <div>

        <h3><?= $widget->title ?></h3>

        <?= $widget->get('result') ?>

    </div>

</div>
<?php endforeach ?>
```

## Default Pagekit markup

The Pagekit backend is built using the UIkit front-end framework. That is why the Pagekit core extensions such as static pages and the blog output markup with CSS classes from UIkit. You are, however, in no way forced to use UIkit to create your own themes.

To style the Pagekit system output, you can just add the CSS for a few classes instead of including the UIkit CSS. The `theme.css` that comes with the Hello extension already comes with the classes you need to style.

If you want to completely change the markup that Pagekit itself generate, you also have the possibility to overwrite system view files, to provide custom widget renderer and custom menu renderer.

## Overwrite system views

To overwrite system view files, you just need to put template files in the correct locations inside your theme folder.

| File                         | Original view file                       | Description               |
|------------------------------|------------------------------------------|---------------------------|
| `views/system/site/page.php` | `/app/system/site/views/page.php`        | Default static page view  |
| `views/blog/post.php`        | `/packages/pagekit/blog/views/post.php`  | Blog post single view     |
| `views/blog/posts.php`       | `/packages/pagekit/blog/views/posts.php` | Blog posts list view      |

To understand which variables you have available in these views, check out the markup in the original view file.


## Add theme options to Site interface

This is done via JavaScript, most comfortably when you make use of Vue components.

Load your own JS when the Site Tree interface is currently active. In your `index.php`, you can do that when you listen to the right event:

```
'events' => [

    'view.system/site/admin/settings' => function ($event, $view) use ($app) {
        $view->script('site-theme', 'theme:js/site-theme.js', 'site-settings');
        $view->data('$theme', $this);
    },

    // ...

],
```

The `js/site-theme.js` contains a Vue component which renders the interface and takes care of the storing of theme settings.

**Note**: Although it's possible to do all of this in a single JS file and have the markup be represented in a string, best practice is to actually create `*.vue` files with your Vue component. Examples can be found in the `app/components` folder of the default *One* theme.


```js
window.Site.components['site-theme'] = {

    section: {
            label: 'Theme',
            icon: 'pk-icon-large-brush',
            priority: 15
    },

    template: '<div>Your form markup here</div>',

    data: function () {
        return window.$theme;
    },

    events: {

        save: function() {

            var config = _.omit(this.config, ['positions', 'menus', 'widget']);

            this.$http.post('admin/system/settings/config', {name: this.name, config: config}).error(function (data) {
                this.$notify(data, 'danger');
            });

        }

    }

};
```

## Add Theme tab to Node configuration in Site Tree

Often you want to attach theme options to a specific Node in the Site tree. For example you want to allow the user to pick a Hero image which can be different per page. To do so, we can add a *Theme* tab to the Site interface.

```php
'events' => [

    // ...

    'view.system/site/admin/edit' => function ($event, $view) {
        $view->script('node-theme', 'theme:js/node-theme.js', 'site-edit');
    },

    // ...
];    
```

Example for `js/node-theme.js`:

```js
window.Site.components['node-theme'] = {

    section: {
        label: 'Theme',
        priority: 90
    },

    props: ['node'],

    template: '<div>Your form markup here</div>'

};
```

**Note:** Compare with full Vue components in `app/components` folder of the default *One* theme.

## Add theme options to Widget interface

Register a script to be loaded in Widget edit view.

```
'view.system/widget/edit' => function ($event, $view) {
    $view->script('widget-theme', 'theme:app/bundle/widget-theme.js', 'widget-edit');
},
```

Example for `widget-theme.js`:

```js
window.Widgets.components['widget-theme'] = {

    section: {
        label: 'Theme',
        priority: 90
    },

    props: ['widget', 'config'],

    template: '<div>Your form markup here</div>'

};

```

**Note:** Compare with full Vue components in `app/components` folder of the default *One* theme.


## Add a settings screen manually

If the prepared ways of adding a settings screen do not satisfy your needs, you can also manually create a completely new interface. With the module definition in `index.php` you have full control and can something like the following:

1. Create a View file for your settings screen.
2. Create a new Controller with an action that renders the view file.
3. Register controller and routes inside your `index.php`
4. Optional: Set the `settings` property to the new settings screen route
