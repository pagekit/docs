# Links

<p class="uk-article-lead">Add your own link types with custom parameters to Pagekit.</p>

Pagekit's routes can be descriped by an internal route syntax. They consist of the route name (e.g. '@hello/name'), optionally followed by GET parameters (e.g. '@hello/name?name=World'). This is called a link. It separates the actual route URI from the route itself.

Pagekit's concept of links allows for a reusable link picker (for example when linking from a menu item or when linking from the markdown editor). The user can choose a type of link and get further options depending on that choice. Linking to a page for example will present the user with a list of pages to choose from.

In this section we will explain how custom link types can be registered and how to ask for advanced options from the user.


## Register JS component

In the `events` property of your module's `index.php`, register a JavaScript file that will take care of rendering the Link interface. The second parameter is the parameter of dependencies, the tilde `~` makes sure your script is only loaded then the `panel-link` script is included.

```php
'view.scripts' => function ($event, $scripts) {
    $scripts->register('link-blog', 'hello:/link-hello.js', '~panel-link');
}
```


## JS component for link picker

In the JavaScript file, you can now render the interface.

**Note** This is most comfortable when makeing use of Vue components, storing them in a single `*.vue` file and bundling them using Webpack. A good example for this can be found in `blog/app/components/link-blog.vue`, the link picker from the Blog extension.

```js
window.Links.components['link-hello'] = {

    link: {
        label: 'Hello'
    },

    props: ['link'],

    template: '<div>Your form markup here.</div>',

    ready: function() {
        this.link = '@hello';
    },

};
```

The Vue component needs to set the 'link' property. This is probably dynamic, depending on the parameters the internal route is made up of.