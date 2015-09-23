# Themes

<p class="uk-article-lead">Create your own theme to control the look of your site.</p>

To get started with creating your own theme, ckeck out the
[Theme Guide](guide-theme.md). Afterwards, this documents offers more
information and advanced configuration possibilities.

## Doing more with themes

Themes and extensions in Pagekit are very much the same. Try
not to think in terms of developing a theme vs. developing an extension but
rather understand that you have access to Pagekit's framework all the time.

Most importantly, the module definition in your theme's `index.php` can contain
all properties, no matter if you have a theme or an extension. If you want to
add a Settings screen to the backend, register additional tabs to the Site
Tree or the Widget management interface, all of that works exactly the same.

## Render sub-view

In some cases you want to split your template in several files. This can
be useful if you either have a lot of markup or if you have conditional
rendering.

A classic example would be when you want to allow different versions of the menu
to be rendered. In this case you should try to avoid nesting several `if`
clauses and instead create additional files in your `views` folder.

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

## Add a settings screen

TODO

## Default Pagekit markup

The Pagekit backend is built using the UIkit front-end framework. That is why
the Pagekit core extensions such as static pages and the blog output markup
with CSS classes from UIkit. You are, however, in no way forced to use UIkit
to create your own themes.

To style the Pagekit system output, you can just add the CSS for a few classes
instead of including the UIkit CSS. The `theme.css` that comes with the Hello
extension already comes with the classes you need to style.

If you want to completely change the markup that Pagekit itself generate, you
also have the possibility to overwrite system view files, to provide custom
widget renderer and custom menu renderer.

## Overwrite system views

To overwrite system view files, you just need to put template files in the
correct locations inside your theme folder.

| File                         | Original view file                       | Description               |
|------------------------------|------------------------------------------|---------------------------|
| `views/system/site/page.php` | `/app/system/site/views/page.php`        | Default static page view  |
| `views/blog/post.php`        | `/packages/pagekit/blog/views/post.php`  | Blog post single view     |
| `views/blog/posts.php`       | `/packages/pagekit/blog/views/posts.php` | Blog posts list view      |

To understand which variables you have available in these views, check out the
markup in the original view file.

## Add theme options to Site interface

TODO

## Add theme options to Widget interface

TODO

## Custom Error Pages

TODO
