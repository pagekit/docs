# Users and Permissions

<p class="uk-article-lead">Pagekit comes with a prepared signup procedure and a powerful user manager. With all of these users in your system, your extension can easily make use of roles and permissions.</p>

## Concepts

1. A **user** is a representation of a person registered with your site. The user account can be active, blocked or new. Users can login to your site or in the admin area. Not all users accounts are allowed to access the admin area.
2. **Permissions** define what actions a user can perform and which content they have access to. A permissions is identified by a name, for example `user: access admin area`. This permission defines whether a user can login to the admin area. Permission names should be descriptive and start with the name of the according module (i.e. `user:` for the user module).
3. **Roles** are the mapping of users to permissions. A user can belong to zero, one or multiple roles. A role can have any number of users assigned to it. In the admin area you can select which permissions a role should have. Pagekit comes with three default roles (Anonymous, Authenticated and Administrator). You can create as many additional roles as you like. Roles are also used to manage access to elements of your site's content.

## Register permissions from a module definition

To add a "permission" to the system area, which can then be assigned to a role, we have the `permissions` keyword in the index.php of the extension.

Use speaking permission names. The convention is to start with the name of the extension and then have a short phrase describing the permission, all lowercase. The `title` is the string displayed in the browser. The `__()` call makes sure this string is translatable.

```
'permissions' => [
    'hello: manage settings' => [
        'title' => __('Manage settings')
    ],
],
```

## Code examples

The following examples assume that the Pagekit Application is available. Make sure to add this to the top of your controller class file.

```php
use Pagekit\Application as App;
```

Fetch the user that is currently logged in. If no user is logged in, this will return a user object that belongs to the Anonymous Role.

```php
$user = App::user();
```

Check if user is logged in.

```php
if($user->isAuthenticated()) {
    // ...
}
```

Check if user is administrator.

```php
if($user->isAdministrator()) {
    // ...
}
```

Check access of a user. This returns true if the user has the permission explicitely assigned to one of its role, or if the user is an administrator.

```php
if(!$user->hasAccess("hello: manage settings")) {
    return "Nope";
}
```

Alternatively, you can use an annotation above a controller class or

```php
/**
 * @Access("hello: manage settings")
 */
```

Check if user has role of specific id

```php
$role_id = 4;
App::user()->hasRole($role_id);
```

Check if user has role of specified name

```php
$role_name = "Editor";
$role = Role::where('name = ?', [$role_name])->first();
App::user()->hasRole($role->id);
```
