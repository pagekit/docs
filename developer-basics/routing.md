# Routing
<p class="uk-article-lead">A central task solved by Pagekit is _Routing_. When the browser hits a URL, the framework determines, what action will be called.</p>

<ul class="uk-list">
    <li><a href="#controller">Controller</a></li>
    <li><a href="#register-a-controller">Register a controller</a></li>
    <li><a href="#annotations">Annotations</a></li>
    <li><a href="#generating-urls">Generating URLs</a></li>
    <li><a href="#links">Links</a></li>
</ul>

## Controller
The most common way of creating routes in Pagekit is to define a _controller_. A controller is responsible for handling requests, setting routes and rendering views.

### Register a controller
You can register a controller inside your [module configuration](modules.md). Use the `routes` property to mount controllers to a route.

```php
'routes' => [

    '/hello' => [
        'name' => '@hello/admin',
        'controller' => [
            'Pagekit\\Hello\\Controller\\HelloController'
        ]
    ]

],
```

### Basic structure
The class is annotated with `@Route("/hello")`, causing the controller to be _mounted_ at `http://example.com/hello/`. That means it will respond to all requests to that URL and sub-URLs like `http://example.com/hello/settings`.

```php
namespace Pagekit\Hello\Controller;

/**
 * @Route("/hello")
 */
class HelloController
{

    public function indexAction()
    {
        // ...
    }

    public function settingsAction()
    {
        // ...
    }
}
```

By default, your extension (or theme) will be booted and a set of default routes will be generated automatically. You can use the [developer toolbar](../troubleshooting/debug-mode.md) to view the newly registered routes (along with all core routes).

Here is how to understand a route:

Route | Example | Description
------|------------------|---------------------------------------------------
Name  |`@hello/hello/settings`| The name of the route can be used to generate URLs (has to be unique).
URI   |`/hello/settings`| The path to access this route in the browser.
Action|`Pagekit\Hello\Controller\DefaultController::settingsAction`| The controller action that will be called.

By default, routes will be of the form `http://example.com/<extension>/<controller>/<action>`. A special action is `indexAction`, which will not be mounted at `.../index`, but at `.../`. Advanced options for custom routes are available of course, as you will see in the next sections.

**Note** If a route is not unique across your application, the one that has been added first is the one being used. As this is framework internal behavior that might change, you should not rely on this but rather make sure your routes are unique.

## Annotations
A lot of the controller's behavior is determined by information annotated to the class and methods. Here is a quick overview, a detailed description for each annotation follows below.

Annotation | Description
---------- | -------------------------------------------------------------
`@Route`   | Route to mount an action or the whole controller.
`@Request` | Handle parameter passing from the http request to the method.
`@Access`  | Check for user permissions.

**Note** Annotations will only work if you start the multiline comment with two asterisks, not just with one.

```
// will NOT work:
/* @Route("/hello") */

// will work:
/** @Route("/hello") */

// will work:
/**
 * @Route("/hello")
 */
```

### @Route
Define the route the controller (or controller action) will be mounted at. It can be annotated to class and method definitions.

By default, a method called `greetAction` will be mounted as `/greet` under the route of the class. To add custom routes, you can add any number of additional routes to a method. Routes can also include dynamic parameters, which will be passed to the method.

```php
/**
 * @Route("/greet", name="@hello/greet/world")
 * @Route("/greet/{name}", name="@hello/greet/name")
 */
public function greetAction($name = 'World')
{
    // ...
}
```

Parameters can be specified to fulfill certain requirements (for example limit the value to numbers). You can name a route so that you can reference from your code. Use PHP argument defaults at the method definition to make a parameter optional.

Routes can be bound to certain HTTP-methods (e.g. `GET` or `POST`). This is especially useful for RESTful APIs.

```php
/**
 * @Route("/view/{id}", name="@hello/view/id", requirements={"id"="\d+"}, methods="GET")
 */
public function viewAction($id = 0)
{
    // ...
}
```

**Note** For detailed information and more examples, have a look at [Symfony's own documentation](http://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/annotations/routing.html) on the `@Route` annotation.

### @Request
You can specify the types of data passed via a request and match them to parameters passed to the annotated method.

The array maps _name_ to _type_. _name_ is the key inside the request data. _type_ can be `int`, `string`, `array` and advanced types like `int[]` for an array of integers. If no type is specified, `string` is assumed by default.

The order of the keys will define the order, in which parameters are being passed to the method. The parameter name in the method head can be anything.

```php
/**
 * @Request({"id": "int", "title", "config": "array"}, csrf=true)
 */
public function saveAction($id, $title, $config)
{
  // ...
}
```

You can also check for a token to protect against [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery). Add `csrf=true` to your request annotation and include the `@token` call in the view that submits a form to this method.

Check out `Pagekit\Filter\FilterManager` for a complete list over available filters. Some filters have additional options, like `pregreplace`.

```php
/**
 * @Request({"folders":"pregreplace[]"}, options={"folders" = {"pattern":"/[^a-z0-9_-]/i"}})
 */
public function deleteFolders($folders)
{
  // ...
}
```

### @Access
You can specify certain user permissions required to access a specific method or the whole controller.

Controllers should always be specific to the frontend or the admin panel. So far, we have seen controllers for the frontend. An administration controller will only be accessible for users with the `Access admin area` permission. Also, all routes for that controller will have a leading `admin/` in the URL. As a result, views will also render in the admin layout and not in the default theme layout.

```php
/**
 * @Access(admin=true)
 */
class SettingsController
{
  // ...
}
```

Now, only users with the admin area access permission can access the controller actions. If you want to use further restrictions and only allow certain users to do specific actions (like manage users etc.), you can add restrictions to single controller actions.

Define permissions in the `extension.php` (or `theme.php`) file and combine them however you want. Access restrictions from the controller level will be combined with access restrictions on the single actions. Therefore you can set a basic _minimum_ access level for your controller and limit certain actions, like administrative actions, to users with more specific permissions.

```php
/**
  * @Access("hello: manage users")
  */
  public function saveAction()
  {
    // ...
  }
```

Of course, you can also use these restrictions even if the controller is no admin area controller. You can also check for admin permissions on single controller actions.

```php
/**
  * @Access("hello: edit article", admin=true)
  */
  public function editAction()
  {
    // ...
  }
```

## Generating URLs
Using the URL service, you can generate URLs to your routes.

```php
$this['url']->route('@hello/default/index')          // '/hello/default/index'
$this['url']->route('@hello/default/index', true)    // 'http://example.com/hello/default/index'
$this['url']->route('@hello/view/id', ['id' => 23])  // '/hello/view/23'
```

## Links
Pagekit's routes can be described by an internal route syntax. They consist of the route name (e.g. '@hello/name'), optionally followed by GET parameters (e.g. '@hello/name?name=World'). This is called a link. It separates the actual route URI from the route itself.
