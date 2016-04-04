# Users

<p class="uk-article-lead">Create and manage users, user roles and access levels to control how much access users have to your site.</p>

<ul class="uk-list">
    <li><a href="#list">List</a></li>
    <li><a href="#permissions">Permissions</a></li>
    <li><a href="#roles">Roles</a></li>
    <li><a href="#settings">Settings</a></li>
    <li><a href="#registration-and-login">Registration and Login</a></li>
</ul>

## List
The _List_ section provides an overview of all your registered users, their status and their roles. You can create new users or edit existing ones. Users can be sorted by name or their e-mail address by clicking on the corresponding table heading in the overview. The active sorting parameter is highlighted with an arrow icon.

![Users overview](assets/users.png)

If you click on the **Roles** parameter in the table heading, you can sort users by their roles or respectively by their status, if you click on the **Status** parameter. By clicking the colored circle icon in the *Status* column, you toggle the status from enabled to disabled. The colors represent the user status as follows.

Color | User Status
------|------
Green | User is enabled and can log in. If an enabled user can't log in, make sure they are assigned to a *Role* that is allowed to access the admin area.
Red   | The user is disabled and can't log in, no matter what role they belong to.
Blue  | This is a new user account and the user is enabled.

### Add and edit users

To create a new user, hit the _Add User_ button in the top right hand corner; to edit an existing one, click on the user name in the user listing. The following fields will be available.

Field                | Description
:------------------- | :-------------------------------------------------------------------------------------------------------
**Username**         | The username that identifies the user. Note that you can only use alphanumeric characters, numbers, hyphen, dot and underscore.
**Name**             | The name that will be displayed when referencing the user.
**Email**            | The email that will be used as the main contact method.
**Password**         | The user password. Existing users can change their password. Click on **Show** to reveal it.
**Status**           | The user account status can be **Blocked** or **Active**.
**Roles**            | The roles the user is part of. All logged in users are automatically **Authenticated**.
**Last Login**       | Displays the last time the user logged into the site.
**Registered Since** | Displays the date the user has been registered in the site or his entry was created.

### User avatar

Pagekit implements [Gravatar](https://gravatar.com/) to create users' profile pictures. Just enter the e-mail address that is associated with your Gravatar account and the image will be fetched automatically.

## Permissions
In the _Permissions_ tab you can see an overview of user roles and the actions they are allowed to perform.

The permissions are grouped by their according extensions and can be assigned or revoked by toggling the checkboxes. Changes are stored immediately.

**Note** You will have to give permission to _Access admin area_ to all roles that are supposed to perform any action in the admin panel, even if it is limited to editing pages. If you don't,  they won't be able to reach the pages' admin panel.

## Roles
In the _Roles_ tab, you can create and manage user roles, a way of organizing users into groups with the same permissions and access levels.

Pagekit comes with a few pre-defined roles. If you need more for your use-case, just hit the _Add Role_ button. The default roles are 
described in the table below.

User / Group  | Description
------------- | -------------------------------------------------
Anonymous     | Any random visitor to your site.
Authenticated | A user with an account who is logged in.
Administrator | A user with the privilege to perform all actions.

## Settings

This section holds a number of global user settings.

User / Group  | Description
------------- | -------------------------------------------------
Registration     | Allow users to create their own account. Available settings are **Disabled**, **Enabled** and **Enabled, but approbal is required**.
Verification | Checking this option will require users to provide their e-mail address in order in register. An e-mail with a verification link will be sent to anyone registering with your site, if this option is enabled.
Login Redirect | Enter a URL or select a pick a link that users will be redirected to after logging in successfully.

## Registration and Login

Pagekit provides a link type for the user extension to help you create registration, login, password reset and similar pages.

Just go to the _Site_ section of the Pagekit administration and [add a new page](site.md#pages). Make sure you select the type _Link_. Once in the Edit view, hit the _Select_ button to use the link picker and choose _User_ from the **Extension** select field. Now you can define the page by choosing one of the options from the _View_ select field. Available views are **User Login**, **User Logout**, **User Registration**, **User Profile** and **User Password Reset**.

**Note** When creating a **User Registration**, **User Login** or **User Password Reset** page, you might want to restrict access to **Anonymous** users and correspondingly restrict **User Logout** and **User Profile** to **Authenticated** users or **Administrator**.