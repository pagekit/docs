# Controller

<p class="uk-article-lead">The Controller is the central location for handling requests, setting routes
and rendering views from your extension.</p>

## Routing basics

A central task solved by Pagekit is *Routing*. When the browser hits a URL,
the framework will determine which controller action will be called.

If a route is not unique across your application, the one that has been added
first is the one being used. As this is framework internal behavior that might
change, you should not rely on this but rather make sure your routes are unique. If no route exists for a requested URL, a 404 error will be shown.


## Basic structure

Your Controller is a class that needs to be linked from your `extension.php`
(see [Configuration](configuration.md) for details). While a controller class does not necessarily have to extend
`Pagekit\Framework\Controller\Controller`, it will usually still do so. By
subclassing the base Controller class, you gain access to the Application
instance and therefore to all services (e.g. `$this['db']` for the database
service, `$this['view']` for the view service).


```php
<?php
namespace Pagekit\Hello\Controller;

use Pagekit\Framework\Controller\Controller;

/**
 * @Route("/hello")
 */
class HelloController extends Controller
{

    public function indexAction()
    {
        // ...
    }

    public function settingsAction()
    {
        // ...
    }
?>
```

The class is annotated with `@Route("/hello")`, causing the Controller to
be *mounted* at `http://example.com/hello/`, meaning it will respond to all
requests to that URL and sub-URLs like `http://example.com/hello/settings`.

By default, your extension (or theme) will be booted
and a set of default routes will be generated automatically. You can
use the command line tool to view the newly registered routes (along with all
core routes). Just execute `./pagekit routes`.

Here is how to understand a route:

| Route  | Description |
|--------|-------------|
| Name <br> `@hello/hello/settings`                                          | The name of the route, can be used to generate URLs (has to be unique). |
| URI <br> `/hello/settings`                                                 | The path to access this route in the browser. |
| Action <br> `Pagekit\Hello\Controller\DefaultController::settingsAction`   | The controller action that will be called. |

By default, routes will be of the form `http://example.com/<extension>/<controller>/<action>`. A special action is
`indexAction` which will not be mounted at `.../index`, but at `.../`.
Advanced options for custom routes are available of course, as you will see in
the next sections.

## Annotations

A lot of the controller's behavior is determined by information annotated to
the class and methods. Here is a quick overview, a detailed description for each annotation follows below.

| Annotation       | Description |
|------------------|-------------|
| `@Route`         | Route to mount an action or the whole controller.  |
| `@Request`       | Handle parameter passing from the http request to the method.  |
| `@Response`      | Render a view file or return a JSON response. |
| `@Access`        | Check for user permissions.                        |

### @Route

Define the route the controller (or controller action) will be mounted at. Can be
annotated to class and method definitions.

By default, a method called `greetAction` will be mounted as `/greet` under the
class' route. To add custom routes, you can add any number of additional routes
to a method. Routes can also include parameters which will be passed to the
method.

  ```php
/**
 * @Route("/greet", name="@hello/greet/world")
 * @Route("/greet/{name}", name="@hello/greet/name")
 * @Response("hello/greet.razr")
 */
public function greetAction($name = 'World')
{
    // ...
}
  ```

Parameters can be specified to fulfil certain requirements (for example limit
to numbers). You can name a route so that you can reference from your
code. use `defaults` in case a parameter was not specified by the user

```php
/**
 * @Route("/view/{id}", name="@hello/view/id", requirements={"id"="\d+"})
 */
public function viewAction($id = 1)
{
    // ...
}
```

**Note** For detailed information and more example, have a look at [Symfony's own documentation](http://symfony.com/doc/current/bundles/SensioFrameworkExtraBundle/annotations/routing.html) on the `@Route` annotation.

### @Request

You can specify the types of data passed via GET and POST request and match
them to parameters passed to the annotated method.

The array maps *name* to *type*. *name* is the key inside the request data.
*type* can be `int`, `string`, `array` and advanced types like `int[]` for an
array of integers. If not type is specified, `string` is assumed by default.

The order of the keys will define the order in which parameters are being
passed to the method. The parameter name in the method head can be anything.

```php
/**
 * @Request({"id": "int", "title", "config": "array"}, csrf=true)
 */
public function saveAction($id, $title, $config) { ... }
```

You can also check for a token to protect against [CSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery). Add `csrf=true` to your request annotation and include the `@token` call in the view that submits a form to this method.

Checkout `Pagekit\Component\Filter\FilterManager` for a complete list over available filters.

Some filters have additional options, like `pregreplace`:
```php
/**
 * @Request({"folders":"pregreplace[]"}, options={"folders" = {"pattern":"/[^a-z0-9_-]/i"}})
 * @Response("json")
 * @Method("DELETE")
 */
public function deleteFolders($folders)
```



### @Response

Set the view file used for rendering. This is how you render `<extensions>/hello/views/index.razr`:

```php
/**
 * @Response("extension://hello/views/index.razr")
 */
public function viewAction($id = 1)
{
    // ...
    return ['id' => $id];
}
```

More about view rendering in the [View and Response](view-response.md) chapter.


### @Access

You can specify certain user permissions required to access a specific method or the whole controller.

Controllers should always be specific for the frontend or the backend. So
far we have seen controllers for the frontend. A backend controller
will only be accessible for users with the admin area access permission. Also, all
routes for that controller will have a leading `admin/` in the URL. As a
result, views will also render in the admin layout and not in the default
theme layout.

  ```php
/**
* @Access(admin=true)
*/
class SettingsController { ...
  ```

Now, only users with the admin area access permission can access the controller
actions. If you want to use further restrictions and only allow certain users
to do specific actions (like manage users etc.) you can add restrictions to
single controller actions.

Define permissions in the `extension.php` (or `theme.php`) and
combine them however you want. Access restrictions from the controller level will
be combined with access restrictions on the single actions. Therefore you can
set a basic *minimum* access level for your controller and limit certain
actions (like administrative actions) to users with more specific permissions.

  ```php
  /**
  * @Access("hello: manage users")
  */
  public function saveAction() { ... }
  ```

Of course, you can also use these restrictions even if the controller is no
admin area controller. You can also check for admin permissions on single controller
actions.

  ```php
  /**
  * @Access("hello: edit article", admin=true)
  */
  public function editAction() { ... }
  ```

## Generating URLs

Using the URL service, you can generate URLs to your routes.

```php
$this['url']->route('@hello/default/index')
$this['url']->route('@hello/view/id', ['id' => 23])
```
