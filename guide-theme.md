# How to create a theme

<p class="uk-article-lead">Get started and create your own theme for Pagekit.</p>

## Create file structure

Each theme in Pagekit is located in its own folder in the `/themes` directory. In order for Pagekit to recognize directory as a valid theme, you have to follow a certain file structure, at least for a few required files.

You can create the basic structure manually or use Pagekit's provided command line tool that creates the initial structure for you. In this tutorial we will be using the command line tool. 

Open the terminal and navigate to your Pagekit folder. To create a theme called `mytheme` , just run the following command.

```bash
php pagekit theme:generate mytheme
```

You will be prompted for some information needed to initialize the theme.

| Input             | Example               | Description |
|-------------------|-----------------------|--------------|
| `Title`           | `My Theme`            | The human readable name of your theme 
| `Author`          | `YOOtheme`            | Your name or your company's name
| `Email`           | `demo@yootheme.com`   | Your email address
| `PHP Namespace`   | `MyTheme`             | Identifier used to organize your code files. PHP namespaces usually follow a CamelCase syntax.

Have a look at the generated `/themes/mytheme` folder. In this tutorial, we will look at theses files one by one, whenever we add a a feature, we will talk about the fiels needed.

![Generated file structure](images/guide-theme-files.png)

To begin with our theme, we just want to display a simple message. To do so, please open `themes/mytheme/templates/template.razr.php`, the content will look as follows.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        @action('head')
    </head>
    <body>

    </body>
</html>
```

In the `<body>` section, insert your message so that the file looks something like this.

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        @action('head')
    </head>
    <body>
        <h1>Hello Pagekit.</h1>
    </body>
</html>
```

Save the file and navigate to the Pagekit admin area in your browser to activate the theme. To do so, go to the *Settings* screen and click on *Themes*. Amongst the installed themes you should see your theme. Click the *Enable button*. Now have a look at the Pagekit installation, it should display the message you've inserted in the template file.

## Set up the main layout

## Add CSS and JS

## Prepare for the market place

- details and screenshot in theme.json