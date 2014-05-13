# Links

Pagekit's concept of links allows for a reusable link picker (for
example when linking from a menu item or when linking from the
markdown editor). The user can choose a type of link and get further
options depending on that choice. Linking to a *page* for example
will present the user with a *list of pages* to choose from.

In this section we will explain how custom link types can be
registered and how to ask for advanced options from the user.

## Basic structure

Extend `Pagekit\System\Link\Link` and implement the three methods `getRoute`, `getLabel` and `renderForm`. Note: `$this(‘view')` is a shortcut to access the `view` service. A detailed explanation will follow in the
[Application](application.md) chapter.

In your extension, create the file `hello/src/HelloLink.php`:

```php
<?php

namespace Pagekit\Hello;
use Pagekit\System\Link\Link;

class HelloLink extends Link
{
    public function getRoute()
    {
        return '@hello/greet/name';
    }

    public function getLabel()
    {
        return __('Hello World');
    }

    public function renderForm()
    {
        return $this('view')->render('hello/admin/link.razr.php', array('route' => $this->getRoute()));
    }
}
```

## Render the custom form

When creating a link, the user is presented with a dropdown menu. If the *Hello
World* type is selected, we want to display custom controls. We also need to make
sure that changes done by the user are stored. That is why we'll need some
JavaScript as well.

```html
<!-- views/admin/link.razr.php -->

<div class="uk-form-controls">
    <input class="uk-width-1-1" name="name" value="" type="text" placeholder="@trans('Hello World')">
</div>

<script>

    require(['jquery', 'link'], function($, Link) {

        Link.register('@route', function(link, form) {

            var $name = $('[name="name"]', form)
                .on('change', function() {
                    link.set($name.serialize());
                });

            return {
                show: function(params, url) {
                    $name.val(params['name'] ? params['name'] : '').trigger('change');
                }
            }

        });

    });
</script>
```

The HTML with the input text field is straight-forward. The JavaScript is a bit
more complex. As you can see, we register a link defined by our route (`@route`
will be rendered to be `@hello/greet/name` by razr).

What we basically do is to solve two tasks. One task is to react to changes in the text field and store the new value on our `link` object.

```js
var $name = $('[name="name"]', form)
    .on('change', function() {
        link.set($name.serialize());
    });
```

The other task is to select the correct item in the dropdown menu when the form
is displayed.

```js
return {
    show: function(params, url) {
        $name.val(params['name'] ? params['name'] : '').trigger('change');
    }
}
```

## Register the link type

To make sure Pagekit knows about the new link type, register `HelloLink`
from the `boot` method of the Extension class (or Theme class) located in
`extension.php` (or `theme.php`).

```php
<?php

namespace Pagekit\Hello;

use Pagekit\Extension\Extension;
use Pagekit\Framework\Application;
use Pagekit\System\Event\LinkEvent;

class HelloExtension extends Extension
{

    public function boot(Application $app)
    {
        parent::boot($app);

        $app->on('system.link', function(LinkEvent $event) {
            $event->register('Pagekit\Hello\HelloLink');
        });
    }
}
```
