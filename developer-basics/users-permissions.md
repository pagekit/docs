# Users &amp; Permissions

<p class="uk-article-lead">Pagekit comes with a prepared signup procedure and a powerful user manager. With all of these users in your system, your extension can easily make use of roles and permissions.</p>

<ul class="uk-list">
    <li><a href="#concepts">Concepts</a></li>
    <li><a href="#show-content-to-specific-role-only">Show content to specific role only</a></li>
    <li><a href="#register-permissions-from-a-module-definition">Register permissions from a module definition</a></li>
</ul>

## Concepts

A **user** is a representation of a person registered with your site and identified by its username. The status of a user account can be *active*, *blocked* or *new*. Users can login to your site or in the admin area. Not all users accounts are allowed to access the admin area.

**Permissions** define what actions a user can perform . A permissions is identified by a name, for example `user: access admin area`. Permission names should be descriptive and start with the name of the according module (i.e. `user:` for the user module).

**Roles** group together several user accounts. All users with the same role share the same permissions. Roles are also used to manage access to elements of your site's content. A user can belong to zero, one or multiple roles. A role can have any number of users assigned to it. Pagekit comes with default roles (Anonymous, Authenticated and Administrator) and allows you to create as many more as you need.

## Show content to specific role only

Roles are very flexible in how they can be used. You can create special content that is only accessible by selected users.

1. In *Users > Roles*, create a new role called "Premium". Don't assign any permissions to this role.
2. In *Users > List*, click a user account to edit its profile and enable the new "Premium" role for this user.
3. On every page in the *Site* area, you see a *Restrict access* section in the sidebar. Make sure to select the "Premium" role and nothing else.

This item will now be visible only to logged in users of the "Premium" role.

Note that your administrator account can also not see this content, unless you either add the user to the "Premium" role as well, or if you also enable "Administrator" in the *Restrict access* settings.

## Register permissions from a module definition

To add a permission to the system area, which can then be assigned to a role, use the `permissions` keyword in the `index.php` of an extension.

Use speaking permission names. The convention is to start with the name of the extension and then have a short phrase describing the permission, all lowercase. The `title` is the string displayed in the browser. The `_()` call makes sure this string is translatable.

```php
'permissions' => [
    'hello: manage settings' => [
        'title' => _('Manage settings')
    ],
],
```

Check if user has role of specific id.

```php
$role_id = 4;
App::user()->hasRole($role_id);
```

Check if user has role of specified name.

```php
$role_name = "Editor";
$role = Role::where('name = ?', [$role_name])->first();
App::user()->hasRole($role->id);
```
