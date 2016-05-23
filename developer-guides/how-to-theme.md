# How to develop a theme
In this guide you will learn how to develop your own theme from our Hello Theme blueprint. You will find out about the theme structure and follow the essential steps to add new positions and options.

## Getting started
The Pagekit Marketplace features our [Hello Theme](https://pagekit.com/marketplace/package/pagekit/theme-hello), a blueprint for developing themes, including code examples and a general foundation to help you get started.

First of all [download](https://pagekit.com/package/pagekit/theme-hello.zip) and install the theme and get familiar with the overall file structure. When installed, the *Hello* theme is located in *packages/pagekit/theme-hello*. It is recommendable to install a simple theme on the side for reference, like [Theme One](https://pagekit.com/marketplace/package/pagekit/theme-one). That way you can compare structural elements and get some inspiration.

## Structure
Let's take a look at some of the central files and folders that you will deal with when developing your own theme.

```
/css
  theme.css               // skeleton file containing a number of pre-defined classes for navigations, forms and utilities that you can use
/js
  theme.js                // empty file for your scripts
/views
  template.php            // renders the theme's output; logo, menu, main content, sidebar and footer positions are available by default
composer.json             // package definition needed for the theme in order to be recognized by Pagekit
image.jpg                 // preview screenshot of the theme
index.php                 // contains theme information and configures the default settings of all positions and options
CHANGELOG.md              // contains information on the current and previous package versions
README.md                 // contains basic information and points you to the documentation
```

### CSS
Hello theme doesn't contain any styles. The included `css/theme.css` file lists all CSS classes that are rendered by the Pagekit core extensions without additional styling. You can add your own CSS to these classes.

The Pagekit admin interface and the Pagekit default themes are built with the [UIkit front-end framework](http://getuikit.com/). So you might want to consider using it for your project as well.
To do so, go to the [UIkit website](http://getuikit.com/), download the latest release and unpack it. UIkit includes three themes: Default, Gradient and Almost Flat. To include the Default theme, copy the `css/uikit.min.css` file from the UIkit package, rename it to `theme.css` and replace `theme-hello/css/theme.css` in your Pagekit theme with it.

Alternatively, you can also use the source LESS files. This would give you access to all UIkit components, so you have full control over your theme's styling. Follow these steps in order to do so:

1. Go to the [UIkit Github repository](https://github.com/uikit/uikit), download the Zip, unpack it and copy the `themes/default` folder (or one of the other themes).
2. Create a `/less` folder inside your theme, paste the `/default` folder in there and rename it to `/uikit`.
3. Less files need to be compiled, so they can be interpreted as CSS by the browser. To make this possible, you need to update the import path in your theme's `less/uikit/uikit.less` file:
    @import "../../app/assets/uikit/less/uikit.less";
4. Copy the following files from an existing theme (Theme One): `gulpfile.js`, `package.json`, `bower.json` and `.bowerrc` (hidden).
5. Open your theme in a new console tab and run *bower install* and *gulp*

Now can use all UIkit components to style your theme. For more information, take a look at the [UIkit documentation](http://getuikit.com/docs/documentation_get-started.html).

## Layout
The central files for your theme's layout are `views/template.php` and `index.php`. The actual rendering happens in the `template.php`. However, your theme needs to register all positions before. This happens in the `index.php` file through the `menus` and `positions` properties. These contain arrays of the position name and a label, which is displayed in the admin panel. This file is also used to load additional scripts and much more.

### Navbar
One of the first things you will want to render in your theme is the main navigation. To do so, you need to create the appropriate widget position.

Widget positions allow users to publish widgets in several locations of your theme markup. They appear in the *Widgets* area of the Pagekit admin panel and can be selected by the user when setting up a widget. As mentioned above, new positions need to be registered with the `index.php` file.

1. Each widget position is defined by an identifier (i.e. `navbar`) and a label to be displayed to the user (i.e. *Navbar*).

	```
    'positions' => [
	
        'navbar' => 'Navbar',
        'sidebar' => 'Sidebar',
	
    ]
	```

2. With the concept of modularity in mind, Pagekit renders position layouts in separate files. For the navigation, create the `views/menu-navbar.php` file containing the following:

	```
    <?php if ($root->getDepth() === 0) : ?>
    <ul class="uk-navbar-nav">
    <?php endif ?>
	
        <?php foreach ($root->getChildren() as $node) : ?>
        <li class="<?= $node->hasChildren() ? 'uk-parent' : '' ?><?= $node->get('active') ? ' uk-active' : '' ?>" <?= ($root->getDepth() === 0 && $node->hasChildren()) ? 'data-uk-dropdown':'' ?>>
            <a href="<?= $node->getUrl() ?>"><?= $node->title ?></a>
	
            <?php if ($node->hasChildren()) : ?>
	
                <?php if ($root->getDepth() === 0) : ?>
                <div class="uk-dropdown uk-dropdown-navbar">
                <?php endif ?>
	
                    <?php if ($root->getDepth() === 0) : ?>
                    <ul class="uk-nav uk-nav-navbar">
                    <?php elseif ($root->getDepth() === 1) : ?>
                    <ul class="uk-nav-sub">
                    <?php else : ?>
                    <ul>
                    <?php endif ?>
                        <?= $view->render('menu-navbar.php', ['root' => $node]) ?>
                    </ul>
	
                <?php if ($root->getDepth() === 0) : ?>
                </div>
                <?php endif ?>
	
            <?php endif ?>
	
        </li>
        <?php endforeach ?>
	
    <?php if ($root->getDepth() === 0) : ?>
    </ul>
    <?php endif ?>
	```

3. To render the actual navbar in the `template.php` file, create a `<nav>` element and add the `.uk-navbar` class. Render the `menu-navbar.php` file inside the element as follows:

	```
    <nav class="uk-navbar">
	
        <?php if ($view->menu()->exists('main') || $view->position()->exists('navbar') ) : ?>
        <div class="uk-navbar-flip">
            <?= $view->menu('main', 'menu-navbar.php') ?>
        </div>
        <?php endif ?>
	
    </nav>
	```

The main menu should now automatically be rendered in the new *Navbar* position.

## Widgets
Widgets are small chunks of content that you can render in different positions of your site.

### Rendering a widget position
1. To render a new widget position, you first need to register it with the `index.php` file. For example, if we want a create a new *Top* position, we will define it through the `positions` property.

	```
    'positions' => [

        'navbar' => 'Navbar',
        'top' => 'Top',

    ],
	```

2. Now we have made the new position known to Pagekit, we need to create a position renderer. To lay out the widgets of the position in a grid, create the file `views/position-grid.php`:

	```
    <?php foreach ($widgets as $widget) : ?>
    <div class="uk-width-medium-1-<?= count($widgets) ?>">

        <div class="uk-panel">

            <h3 class="uk-panel-title"><?= $widget->title ?></h3>

            <?= $widget->get('result') ?>

        </div>

    </div>
    <?php endforeach ?>
	```

3. To render the new position in the theme's markup, we need to add it to the `views/template.php` file, as well:

	```
    <?php if ($view->position()->exists('top')) : ?>
    <div id="top" class="tm-top">
        <div class="uk-container uk-container-center">

            <section class="uk-grid uk-grid-match" data-uk-grid-margin>
                <?= $view->position('top', 'position-grid.php') ?>
            </section>

        </div>
    </div>
    <?php endif ?>
	```

### Adding position options
Pagekit uses [Vue.js](https://vuejs.org/) to build its administration interface. Here is a [Video tutorial](https://youtu.be/3gPGyhTroSA?list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto) on Vue.js in Pagekit. In this case we want to add the option to apply a different background color to our new Top position.

1. First, we need to create the folder `app/components` and in it the file `node-theme.vue`. Here we add the option which will be displayed in the Pagekit administration.

	```
    <template>

        <div class="uk-form-horizontal">

            <div class="uk-form-row">
                <label for="form-top-style" class="uk-form-label">Top {{ 'Position' | trans }}</label>
                <div class="uk-form-controls">
                    <select id="form-top-style" class="uk-form-width-large" v-model="node.theme.top_style">
                        <option value="uk-block-default">{{ 'Default' | trans }}</option>
                        <option value="uk-block-muted">{{ 'Muted' | trans }}</option>
                    </select>
                </div>
            </div>

        </div>

    </template>
	```

2. Now we still have to make this option available in the Site Tree. To do so, we can create a *Theme* tab to the interface by adding the following to the `index.php` file.

	```
    'events' => [

        'view.system/site/admin/edit' => function($event, $view) {
            $view->script('node-theme', 'theme:app/bundle/node-theme.js', 'site-edit');
        }

    ]
	```

3. The default setting for the widget position also needs to be added in the `index.php`.

	```
    'node' => [

        'top_style' => 'uk-block-muted',

    ],
	```

4. Vue components need to be compiled into JavaScript using Webpack. To do so, create the file `webpack.config.js` in your theme folder:

	```
    module.exports = [

        {
            entry: {
                "node-theme": "./app/components/node-theme.vue",
            },
            output: {
                filename: "./app/bundle/[name].js"
            },
            module: {
                loaders: [
                    { test: /\.vue$/, loader: "vue" }
                ]
            }
        }

    ];
	```

5. After that you can run the command *webpack* on the theme folder and `node-theme.vue` will be compiled into `/bundle/node-theme.js` with the template markup converted to an inline string.

Whenever you apply changes to the vue component, you need to run this task again. Alternatively, you can run `webpack --watch` which will stay active and automatically recompile when you changed the vue component. You can quit this command with the shortcut *Ctrl + C* For more information on Vue and Webpack, take a closer look at [this doc](https://pagekit.com/docs/developer-basics/vuejs-and-webpack).

6. Lastly, to actually render the chosen setting into the widget position, we need to add the `.uk-block` class and the style parameter to the position itself in the `template.php` file.

	```
    <div id="top" class="tm-top uk-block <?= $params['top_style'] ?>">
	```

### Adding widget options
You can also add specific options to widgets themselves. In this case, we would like to provide a panel style option that can be selected for each widget.

1. First, we need to create a `app/components/widget-theme.vue` file:

	```
    <template>

        <div class="uk-form-horizontal">

            <div class="uk-form-row">
                <label for="form-theme-panel" class="uk-form-label">{{ 'Panel Style' | trans }}</label>
                <div class="uk-form-controls">
                    <select id="form-theme-panel" class="uk-form-width-large" v-model="widget.theme.panel">
                        <option value="">{{ 'None' | trans }}</option>
                        <option value="uk-panel-box">{{ 'Box' | trans }}</option>
                        <option value="uk-panel-box uk-panel-box-primary">{{ 'Box Primary' | trans }}</option>
                        <option value="uk-panel-box uk-panel-box-secondary">{{ 'Box Secondary' | trans }}</option>
                        <option value="uk-panel-header">{{ 'Header' | trans }}</option>
                    </select>
                </div>
            </div>

        </div>

    </template>

    <script>

        module.exports = {

            section: {
                label: 'Theme',
                priority: 90
            },

            props: ['widget', 'config']

        };

        window.Widgets.components['theme'] = module.exports;

    </script>
	```

2. Add the widget component to the `webpack.config.json` file, so it will be compiled to JavaScript when running *webpack*.

3. To make the option available in the widget administration, we can create a *Theme* tab to the interface by adding the following to the `index.php` file.

	```
    'view.system/widget/edit' => function ($event, $view) {
        $view->script('widget-theme', 'theme:app/bundle/widget-theme.js', 'widget-edit');
    },
	```

4. The default setting for the widget also needs to be added in the `index.php`.

	```
    'widget' => [

        'panel' => ''

    ],
	```

5. Now all we need to do is render the chosen setting into the widget in the `position-grid.php` file.

	```
    <div class="uk-panel <?= $widget->theme['panel'] ?>">
	```




