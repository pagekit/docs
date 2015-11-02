# Users
Create and manage users, user roles and access levels to control how much access users have to your site.

## List
In the _List_ tab, you have an overview of all your registered user.

![Users overview](assets/users.png)

In the user listing, you have a few quick actions available. You can quickly toggle user status from enabled to disabled by clicking the colored circle icon in the *Status* column. The colors represent the user status as follows.

Color | User Status
------|------
Green | User is enabled and can log in. If an enabled user cannot log in, make sure they are assigned to a *Role* that is allowed to access the admin area
Red   | User is disabled and cannot log in, no matter what role they belong to
Blue  | This is a new user account

## Edit user

To edit a user, click on the user name in the user listing.

Field                | Description
:------------------- | :-------------------------------------------------------------------------------------------------------
**Username**         | The username that identifies the user (limited to alphanumeric characters, numbers, dot and underscore).
**Name**             | The name that will be displayed when referencing the user.
**Email**            | The email that will be used as main contact method.
**Password**         | The user password. Click on show to reveal it.
**Status**           | The user account status.
**Roles**            | The roles the user is part of.
**Last Login**       | Displays the last time the user logged in the site.
**Registered Since** | Displays the date the user has been registered in the site or his entry created.

## User avatar

The user's profile picture is displayed as a [Gravatar](https://gravatar.com/) depending on the given email address.

## Permissions
In the _Permissions_ tab you can see an overview of user roles and the actions they are allowed to perform.

The permissions are grouped by their according extensions and can be assigned or revoked by toggling the checkboxes. Changes are stored immediately.

**Note** You will have to give the _Access admin area_ permission to all roles who should perform any action in the admin panel, even if it is limited to editing pages. If you don't,  they won't be able to reach the pages' admin panel.

## Roles
In the _Roles_ tab, you can create and manage user roles, a way of organizing users into groups with the same permissions and access levels.

Pagekit comes with a few pre-defined roles. If you need more for your use-case, just add new roles however you like. The default roles are:

User / Group  | Description
------------- | -------------------------------------------------
Anonymous     | Any random visitor to your site.
Authenticated | A user with an account who is logged in.
Administrator | A user with the privilege to perform all actions.
