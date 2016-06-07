# How to develop a theme
In this guide you will learn how to develop your own theme from our Hello Theme blueprint. You will find out about the theme structure and follow the essential steps to add new positions and options.

## Getting started

For this guide, we assume you have a running Pagekit installation on a local server environment. If that is not the case, [download](https://pagekit.com/download) a Pagekit installation package and quickly [install](../getting-started/installation.md) it. Then login to the admin area and have a look at the Theme section of the integrated Marketplace.

The Pagekit Marketplace features our [Hello Theme](https://pagekit.com/marketplace/package/pagekit/theme-hello), a blueprint for developing themes, including code examples and a general foundation to help you get started.

![Hello Theme unstyled](assets/howto-theme-hello-plain.png)

_The Hello theme doesn't provide any styling, but gives you a good starting point to develop your own theme._

First of all [download](https://pagekit.com/package/pagekit/theme-hello.zip) and install the theme and have a look at the overall file structure. When installed, the *Hello* theme is located in *packages/pagekit/theme-hello*. It is recommendable to install a simple theme on the side for reference, like [Theme One](https://pagekit.com/marketplace/package/pagekit/theme-one). That way you can compare structural elements and get some inspiration.

## Structure
Let's take a look at some of the central files and folders that you will deal with when developing your own theme.

```
/css
  theme.css               // skeleton file containing a number of pre-defined classes for navigations, forms and utilities that you can use
/js
  theme.js                // empty file for your scripts
/views
  template.php            // renders the theme's output; logo, menu, main content, sidebar and footer positions are available by default
composer.json             // package definition so that Pagekit recognizes the theme
image.jpg                 // preview screenshot
index.php                 // contains theme information and configures the default settings of all positions and options
CHANGELOG.md              // contains information on the current and previous package versions
README.md                 // contains basic information and points you to the documentation
```

### CSS
Hello theme doesn't contain any styles. The included `css/theme.css` file lists all CSS classes that are rendered by the Pagekit core extensions without additional styling. You can add your own CSS to these classes.

The Pagekit admin interface and the Pagekit default themes are built with the [UIkit front-end framework](http://getuikit.com/). So you might want to consider using it for your project as well. In the following example we will be building our theme using UIkit. If you use another framework or no framework at all, the basic approach of including CSS is similar.

There are many possible ways to set up your theme's file structure. We can recommend two approaches here. One is the "classic" way of including plain CSS. The second one is more complex to set up initially, but allows for far more flexibility.

#### The simple way: Include plain CSS files

Go to the [UIkit website](http://getuikit.com/), download the latest release and unpack it. UIkit comes with three themes: Default, Gradient and Almost Flat. To include the Default theme, copy the `css/uikit.min.css` file from the UIkit package and paste it into the `theme-hello/css/` folder of your theme.

To make sure the file is loaded by Pagekit, open the main layout file of the theme which is located at `theme-hello/views/template.php`. 

In the `<head>` section of this layout file, we see that one css file is included already.

```
<?php $view->style('theme', 'theme:css/theme.css') ?>
```

Now add the following line **above** to make sure that the `uikit.min.css` file is loaded.

```
<?php $view->style('my-uikit', 'theme:css/ukit.min.css') ?>
```

That's it, your theme now contains UIkit's CSS. To add your own CSS rules, simply edit `theme-hello/css/theme.css`.

#### The advanced way: Setup with Gulp and LESS

In the previous section we have seen how easy it is to add plain CSS files to your theme. If you have experience with building websites, you are probably familiar with more flexible ways of styling your content. A good example for this is using a CSS pre-processor like LESS. This allows you to use things such as variables, which make your code easier to read and manage.

When using UIkit, this has the great advantage that you can simply modify a variable to apply global changes, for example altering the primary theme color.

To comfortably work with the LESS pre-processor, you should have a few tools installed and available on your command line: *npm*, *gulp* and *bower*. If you do not have them installed, do a quick Google search, there are plenty of tutorials available online.

There are a number of possible file structure setups. In the following passage we suggest the structure that is also used for the official Pagekit themes. While it might look like many steps when you create a theme for the first time, in our experience this setup allows for well structured code, easy UIkit customizations and a comfortable way to keep UIkit up to date using Bower.

1. In your theme, create the new files `package.json`, `bower.json`, `.bowerrc`, `gulpfile.js` and `less/theme.less`. Paste the following contents into these files. If you have named your theme differently, replace any occurences of the string `theme-hello` with your theme name.

	`package.json`. This file defines all JavaScript dependencies that are installed when running `npm install` and includes several npm packages that we will we use, for example when compiling LESS to CSS:
	
	```
	{
	    "name": "theme-hello",
		"devDependencies": {
		     "bower": "*",
		     "gulp": "*",
		     "gulp-less": "*",
		     "gulp-rename": "*"
		 }
	}
	```
	
	`bower.json` tells bower to fetch the newest release of UIkit. That way you can always run `bower install` to fetch the current LESS source files from UIkit:
	
	```js
	{
		"name": "theme-hello",
		"dependencies": {
			"uikit": "*"
		},
		"private": true
	}
	```
	
    `.bowerrc` includes configuration settings for bower. By default, bower installs everything in a directory called `/bower_components` inside the theme directory. Simply out of preference, we change that default directory:
	
	```js
	{
	    "directory": "app/assets"
	}
	```
	
	`gulpfile.js` contains all tasks which we can run using Gulp. We only need a task to compile LESS to CSS. For convenience we also add a `watch` task that can be run to automatically recompile LESS when any changes to the files have been detected:
	
	```js
	var gulp       = require('gulp'),
	    less       = require('gulp-less'),
	    rename     = require('gulp-rename');
	
		gulp.task('default', function () {
		    return gulp.src('less/theme.less', {base: __dirname})
		        .pipe(less({compress: true}))
		        .pipe(rename(function (file) {
		            // the compiled file should be stored in the css/ folder instead of the less/ folder
		            file.dirname = file.dirname.replace('less', 'css');
		        }))
		        .pipe(gulp.dest(__dirname));
        });
		
		gulp.task('watch', function () {
		    gulp.watch('less/*.less', ['default']);
		});

    ```
	
	`less/theme.less` is the place where you store your theme's styles. Mind that you first need to import UIkit so that it is also compiled by the Gulp task we have defined above.
	
	```
    less
	@import "uikit/uikit.less";
	
	// your theme styles will follow here...
	```
	
	`.gitignore` is an optional file you could create, if you manage your code using Git. You probably don't want to commit the downloaded packages by bower and the generated CSS. Just make sure to include the generated CSS when you upload the theme to your server or the Pagekit Marketplace.
	
	```
	/app/assets/*
	/css
	/node_modules
	.DS_Store
	.idea
	*.zip
	```
	
2. After you have created the files above, go to the [UIkit Github repository](https://github.com/uikit/uikit), download the Zip, unpack it and find the `themes/default` folder (or one of the other themes, if you like).
3. Create a `/less` folder inside your theme, copy and paste the `default` theme folder in there and rename it to `/uikit`, so that it is located at `less/uikit` inside your theme folder.
4. The style we just copied needs to import the core UIkit LESS, so that it can be compiled successfully. To make this possible, you need to update the import path in your theme's `less/uikit/uikit.less` file. Make sure to change the import in line to the following path: `@import "../../app/assets/uikit/less/uikit.less";`
5. Open your theme in a new console tab (for example `cd pagekit/packages/theme-hello`) and run `npm install`, `bower install` and `gulp`.

Now, that were quite a few steps. Make sure your file structure looks as follows now (plus additional theme files that were there before):

```
app/
    assets/
        uikit/     result of bower install
        jquery/    result of bower install
less/
    uikit/
        ... many uikit components
    	uikit.less
    theme.less
.bowerrc
bower.json
gulpfile.js
package.json
... other theme files 
```

With this file setup in place, we have now achieved the following:

- **Separation** of theme styles and UIkit customizations. Add your own styles in `less/theme.less`, customize UIkit in `less/uikit/*`
- **Easily customize UIkit**: Every UIkit component's settings are located in its own `*.less` file. For example, to change the body font size, open `less/uikit/base.less` and change the value of `@base-body-font-size`, then re-run `gulp`. To use any of the [UIkit add-on components](http://getuikit.com/docs/components.html), open `less/uikit.less` and import the add-on's less file from the `app/assets` directory, for example for the slideshow, add the line: `@import "../../app/assets/uikit/less/components/slideshow.less";` and re-run `gulp.`
- **Easily update UIkit**: Run `bower install` to fetch the newest version of UIkit and run `gulp` to re-compile your LESS files to CSS.
 

## Adding JavaScript

Included in Hello theme, you will find an empty JavaScript file `js/script.js`. Here, you can add your own JavaScript code. In the Hello Theme, the file is loaded because it is included in the `template.php` as follows:

```
<?php $view->script('theme', 'theme:js/theme.js') ?>
```

When including a script, it needs a unique identifier (`theme`) and the path to the script file (`theme:js/theme.js`). As you can see, you can use `theme:` as a short version for the file path to your theme directory. To add more JavaScript files, simply add more lines in the same way.

### Adding multiple JavaScript files with dependencies

In the earlier examples we have now worked with CSS from UIkit. If you also want to use UIkit's JavaScript components and utilities, it makes sense to add the UIkit JavaScript files, as well. 

One way to add the UIkit JavaScript would be to move the UIkit JavaScript files to the `js/` directory, along with jQuery. Note that UIkit requires that you load jQuery before you can use the UIkit JavaScript components. 

Including these three files would look as follows. 

```
<?php $view->script('theme-jquery', 'theme:js/jquery.min.js') ?>
<?php $view->script('theme-uikit', 'theme:js/uikit.min.js', 'theme-jquery') ?>
<?php $view->script('theme', 'theme:js/theme.js', 'theme-uikit') ?>
```

Note how we now add a third parameter, which defines _dependencies_ of the script that we are loading. Dependencies are other JavaScript files that have to be loaded earlier. So in the example, Pagekit will definitely make sure to load the three files in the following order: jQuery, UIkit and then our own `theme.js`. In this specific example, this mechanism does not seem very useful, because the scripts will probably be loaded in exactly the order that we put them in the `template.php`, right? That is correct, but imagine these lines being located in different files and sub-templates of your theme. Defining dependencies will make sure that Pagekit always loads the files in a correct order.

As you can already see in the example above, dependencies are referenced using the unique string identifier (e.g. `theme-jquery`). In our example, this identifier is given to the script the first time it is included using the `script()` method. So, as you have seen now, this method takes three parameters: `$view->script($identifier, $path_to_script, $dependencies)`.

### Adding third party scripts, like jQuery

You may ask yourself why we called the included jQuery script `theme-jquery` and not simply `jquery`. In general, it is always useful to prefix your own identifiers, to avoid collisions with other extensions. However, in this specific example, the identifiers `jquery` and `uikit` are already taken, because Pagekit itself includes jQuery and UIkit. This means that you can already load these JavaScript files without including them in your theme. That way, all themes and extensions can share a single version of jQuery (and UIkit, if they use UIkit) to avoid conflicts.

```
<?php $view->script('theme', 'theme:js/theme.js', ['uikit', 'jquery']) ?>
```

As you can see in the example, the third parameter of the `script()` method can also take a list of multiple dependencies. In the earlier example we have only passed in a single string (for example `theme-jquery`). Pass in a string for a single dependency, or a list for multiple depencies — both are possible.

The currently loaded version of jQuery and UIkit depend on the current version of Pagekit. With new releases of Pagekit, the versions of these libraries will continually be updated. While this allows for always having a current version available, a potential downside would be that you need to make sure your code also works for the new versions of these libraries.

## Layout
The central files for your theme's layout are `views/template.php` and `index.php`. The actual rendering happens in the `template.php`. 

As you can see, this file contains a very basic setup for you to start creating your own content. So, you might want to add a bit of a layout to the existing content. Let's try wrapping a container around our main content and dividing the system output and sidebar into a grid.

Around **line 27** the `views/template.php` file renders the sidebar position, system messages and the actual content.

```
<!-- Render widget position -->
<?php if ($view->position()->exists('sidebar')) : ?>
    <?= $view->position('sidebar') ?>
<?php endif; ?>

<!-- Render system messages -->
<?= $view->render('messages') ?>

<!-- Render content -->
<?= $view->render('content') ?>
```

Using UIkit's [Block](http://getuikit.com/docs/block.html) and [Utility](http://getuikit.com/docs/utility.html) components we will create a position block and a container with a fluid width.

It is always a good idea to prefix your own classes, so they will not collide with other CSS you might be using. For example, all UIkit classes are prefixed `uk-`. To distinguish classes or IDs that come from this theme, we will use the prefix `tm-`. Consequently, we add the class and ID `tm-main` to identify the section.

```
<div id="tm-main" class="tm-main uk-block">
    <div class="uk-container uk-container-center">

        <!-- Render widget position -->
        <?php if ($view->position()->exists('sidebar')) : ?>
            <?= $view->position('sidebar') ?>
        <?php endif; ?>

        <!-- Render system messages -->
        <?= $view->render('messages') ?>

        <!-- Render content -->
        <?= $view->render('content') ?>

    </div>
</div>
```

Now we want the system output and sidebar to actually be side by side. The [Grid](http://getuikit.com/docs/grid.html) component can help us here. For a more semantic layout, we will use `&lt;main&gt;` and `&lt;aside&gt;` elements for the containers.

```
<div id="tm-main" class="tm-main uk-block">
    <div class="uk-container uk-container-center">

        <div class="uk-grid" data-uk-grid-match data-uk-grid-margin>

            <main class="<?= $view->position()->exists('sidebar') ? 'uk-width-medium-3-4' : 'uk-width-1-1'; ?>">

                <!-- Render system messages -->
                <?= $view->render('messages') ?>

                <!-- Render content -->
                <?= $view->render('content') ?>

            </main>

            <?php if ($view->position()->exists('sidebar')) : ?>
            <aside class="uk-width-medium-1-4">
                <?= $view->position('sidebar') ?>
            </aside>
            <?php endif; ?>

        </div>

    </div>
</div>
```

### Theme elements

To create more complex layouts, you can add your own widget positions, menus and options for both. A regular theme basically consists of widgets, menus and the actual page content.

The page <em>content</em> is nothing other than Pagekit's system output. That means that the content of any page you create will be rendered in this area.

<em>Widgets</em> are small chunks of content that you can render in different positions of your site, so that they will be displayed in specific locations of your site's markup.

To navigate through any site, you first need to set up a <em>menu</em>. For this purpose, Pagekit provides different menu positions that allow users to publish menus in several locations of the theme markup.

[SCREENSHOT]

_A typical site layout consists of the main navigation, the page content and a sidebar_

However, your theme needs to register all positions before. This happens in the `index.php` file through the `menus` and `positions` properties. These contain arrays of the position name and a label, which is displayed in the admin panel. This file is also used to load additional scripts and much more.

## Navbar

One of the first things you will want to render in your theme is the main navigation. 

![Main navigation unstyled](assets/howto-theme-menu-unstyled.png)

_By default, the Hello theme renders menu items in a very simple vertical navigation._

1. Hello theme comes with the predefined *Main* menu position. When adding a new position it needs to be defined by an identifier (i.e. `main`) and a label to be displayed to the user (i.e. *Main*).

    ```
    'menu' => [
    
        'main' => 'Main',
    
    ]
    ```
    
    ![Menu position in Site Tree](assets/howto-theme-menu-position.png)
	
    _A menu can be published to the defined positions in the Pagekit Site Tree_

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

3. To render the actual navbar in the `template.php` file, create a `<nav>` element and add the `.uk-navbar` class. Load the `menu-navbar.php` file inside the element as follows:

	```
    <nav class="uk-navbar">
	
        <?php if ($view->menu()->exists('main')) : ?>
        <div class="uk-navbar-flip">
            <?= $view->menu('main', 'menu-navbar.php') ?>
        </div>
        <?php endif ?>
	
    </nav>
	```

    The main menu should now automatically be rendered in the new *Navbar* position. 

4. You will probably also want the logo to appear inside the navbar. So wrap the `&lt;nav&gt;` element around the logo as well and add the `.uk-navbar-brand` class, to apply the appropriate spacing.

    ```
    <nav class="uk-navbar">

        <!-- Render logo or title with site URL -->
        <a class="uk-navbar-brand" href="<?= $view->url()->get() ?>">
            <?php if ($logo = $params['logo']) : ?>
                <img src="<?= $this->escape($logo) ?>" alt="">
            <?php else : ?>
                <?= $params['title'] ?>
            <?php endif ?>
        </a>

        <?php if ($view->menu()->exists('main')) : ?>
        <div class="uk-navbar-flip">
            <?= $view->menu('main', 'menu-navbar.php') ?>
        </div>
        <?php endif ?>

    </nav>
    ```

    ![Horizontal navbar](assets/howto-theme-navbar.png)

    _With our changes, menu items are now rendered in a horizontal navbar._

### Adding theme options

Pagekit uses [Vue.js](https://vuejs.org/) to build its administration interface. Here is a [Video tutorial](https://youtu.be/3gPGyhTroSA?list=PL2rU5GxE-MQ7aYIcxTmDh4-BTHRM-9lto) on Vue.js in Pagekit.

A frequently requested feature is for the navbar to remain fixed at the top of the browser window when scrolling down the site. In the following steps, we are going to add this as an option to our theme.

1. First, we need to create the folder `app/components` and in it the file `site-theme.vue`. Settings stored in this file affect the entire website and can be found under *Theme* in the *Settings* tab of the Site Tree. They cannot be applied to a specific page.

    ![Site Tree](assets/howto-theme-site-tree.png)

    _You can add any kind of Settings screen to the admin area_

    In the freshly created file `site-theme.vue` we add the option which will be displayed in the Pagekit administration.

    ```
    <template>

        <div class="uk-margin uk-flex uk-flex-space-between uk-flex-wrap" data-uk-margin>
            <div data-uk-margin>
                <h2 class="uk-margin-remove">{{ 'Theme' | trans }}</h2>
            </div>
            <div data-uk-margin>
                <button class="uk-button uk-button-primary" type="submit">{{ 'Save' | trans }}</button>
            </div>
        </div>

        <div class="uk-form uk-form-horizontal">

            <div class="uk-form-row">
                <label for="form-navbar-mode" class="uk-form-label">{{ 'Navbar Mode' | trans }}</label>
                <p class="uk-form-controls-condensed">
                    <label><input type="checkbox" v-model="config.navbar_sticky"> {{ 'Sticky Navigation' | trans }}</label>
                </p>
            </div>

        </div>

    </template>
    ```

2. Now we still have to make this option available in the Site Tree. To do so, we can create a *Theme* tab in the interface by adding the following script below the option in the `site-theme.vue` file.

    ```
    <script>

        module.exports = {

            section: {
                label: 'Theme',
                icon: 'pk-icon-large-brush',
                priority: 15
            },

            data: function () {
                return _.extend({config: {}}, window.$theme);
            },

            events: {

                save: function() {

                    this.$http.post('admin/system/settings/config', {name: this.name, config: this.config}).catch(function (res) {
                        this.$notify(res.data, 'danger');
                    });

                }

            }

        };

        window.Site.components['site-theme'] = module.exports;

    </script>
    ```

3. To actually apply the script and add the option to the Site Tree, also add the following to the `index.php` file.

    ```
    'events' => [

        'view.system/site/admin/settings' => function ($event, $view) use ($app) {
            $view->script('site-theme', 'theme:app/bundle/site-theme.js', 'site-settings');
            $view->data('$theme', $this);
        }

    ]
    ```

4. The default setting for the navbar mode needs to be added in the `index.php`.

    ```
    'config' => [
            'navbar_sticky' => false
        ],
    ```

5. Vue components need to be compiled into JavaScript using Webpack. To do so, create the file `webpack.config.js` in your theme folder:

        ```
        module.exports = [

            {
                entry: {
                    "site-theme": "./app/components/site-theme.vue"
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

6. After that, run the command *webpack* on the theme folder and `site-theme.vue` will be compiled into `/bundle/site-theme.js` with the template markup converted to an inline string.

    Whenever you apply changes to the vue component, you need to run this task again. Alternatively, you can run `webpack --watch` which will stay active and automatically recompile when you changed the vue component. You can quit this command with the shortcut *Ctrl + C* For more information on Vue and Webpack, take a closer look at [this doc](https://pagekit.com/docs/developer-basics/vuejs-and-webpack).

7. Lastly we want to load the necessary JavaScript dependencies in the head of our `views/template.php` file. In our case we are using the [Sticky component](http://getuikit.com/docs/sticky.html) from UIkit. Since iis not included in the framework core, it needs to be loaded seperately with theme's JavaScript.

    ```
    <?php $view->script('theme', 'theme:js/theme.js', ['uikit-sticky']) ?>
    ```

    Now all you need to do is render the option into the actual navbar.

    ```
    <nav class="uk-navbar<?= $params['navbar_sticky'] ? ' data-uk-sticky' : '' ?>">
    ```

## Widgets

Widget positions allow users to publish widgets in several locations of your theme markup. They appear in the *Widgets* area of the Pagekit admin panel and can be selected by the user when setting up a widget.

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
In this case we want to add the option to apply a different background color to our new Top position.

1. First, we need to create the file `node-theme.vue` inside the folder `app/components`. Here we add the option which will be displayed in the Pagekit administration. Settings that are stored in this file can be applied to each page separately by entering the appropriate item in the site tree and clicking the *Theme* tab.

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

2. Now we still have to make this option available in the Site Tree. To do so, we can create a *Theme* tab in the interface by adding the following to the `index.php` file.

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

4. In the chapter about theme options we created the file `webpack.config.js`. Our `node-theme.vue` file needs to be registered with this file, as well, to be compiled into JavaScript.

	```
    entry: {
        "node-theme": "./app/components/node-theme.vue",
        "site-theme": "./app/components/site-theme.vue"
    },
	```

5. After that you can run the command *webpack* on the theme folder and `node-theme.vue` will be compiled into `/bundle/node-theme.js` with the template markup converted to an inline string.

6. Lastly, to actually render the chosen setting into the widget position, we need to add the `.uk-block` class and the style parameter to the position itself in the `template.php` file.

	```
    <div id="top" class="tm-top uk-block <?= $params['top_style'] ?>">
	```

### Adding widget options
You can also add specific options to widgets themselves. In this case, we would like to provide a panel style option that can be selected for each widget.

[SCREENSHOT]

_A theme can add any kind of options to the Widget editor_

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
	

## Wrapping up

In this guide, you have learned the basic knowledge and tools to create themes for Pagekit. Let us summarize which topics we have covered.

- You are now familiar with the **file structure** of a Pagekit theme. The main entry point for configuration and custom code is the theme's `index.php`, the main template file is located at `views/template.php` inside the theme
- To **add JavaScript** to your theme, you can add your own code and include the script files in your template. You can also add third party libraries like UIkit and jQuery that help you to add interaction to your website.
- **Widgets and menus** are managed from the Pagekit admin area and are rendered in special positions of your theme. You have learned how to create these positions and how to change the default widget rendering.
- To make sure your theme is customizable from the admin area, you can add your own **settings screens**. These can be added to the Site Tree, to the Site settings and to the Widget editor.

With these skills you are now in a position to create Pagekit themes, both for client projects and also for the Pagekit marketplace.



