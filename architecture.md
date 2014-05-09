# Folder structure

To get an overview of the files and folders in Pagekit see the (Folder Structure)[folder-structure.md] document.

As a developer you will be interested in the `vendor` folder. In this folder you will find all the code, except from bootstrapping code, like:

- JavaScript and CSS libraries, i.e. jQuery, UIKit etc.
- third party PHP libraries Symfony, Doctrine etc.
- Razr Templating Engine, Pagekit's Components and the Pagekit Framework.


# Architecture

In the center of Pagekit's architecture sits the Application, an instance of
`Pagekit\Framework\Application`. The application is the first instance to
receive a request and return a response (more to that in the *request lifecycle*
section).

The application also functions as a [Pimple dependency injection container](http://pimple.sensiolabs.org/),
the main location to hold all service providers. Service providers are a
central concept to understand, as they bundle the functionality of certain
Components and other parts of the framework and make it accessible throughout
the application - also from your extensions and themes.

A service provider is a class that implements the two methods `register` and
`boot`. The `register` method will be called in the Application's bootstrap
phase, the `boot` in the Application's request handling process.
`register` is expected to assign the service to a property of the Application,
i.e. `$app['db']` to access the database service from every point in the code.

The application holds a `Router` object which sits on top of the `HttpKernel`
which takes care of the lowest level of request handling.

Communication of the different parts of the system happens through events. More
to that in the *Events* section below.

# Request lifecycle

For a deeper understanding of the control flow inside Pagekit's internals, let
us have a look at what happens when a request goes through all Pagekit layers.

The entry point of all request handling is `index.php` which the webserver calls
on every request. Here, the two basic phases get kicked off. First,
`app/app.php` is loaded which holds all bootstrap code to setup the Application.
The second phase is the actual request handling phase. Let's look at these two
separately.

## 1. The bootstrap phase

Bootstrapping happens in `app/app.php` and consists of reading the default
config `app/config/app.php`, merging it with the user config `config.php` and
passing the resulting configuration array to a fresh Application instance.

Then, all service providers defined in the configuration are being registered
at the application. A connection to the database is established and an array
of extension to load later is generated (the actual loading happens in the second
phase).

If the config file is missing or the database connection reveals that Pagekit
has not been installed yet, this is the place where the extensions to be loaded
is limited to the *installer* extension which will result in the installer to
be displayed to the user at the end of phase two.

## 2. The request handling phase

When the bootstrap process has finished, the Application's run method will start
handling the http request. From the Application's perspective, request handling
will be performed by the Router.

The `Router` sits on top of the `HttpKernel`, the lowest level of the
architecture that dispatches calls to controllers
depending on the result of the route evaluation. The kernel will then return
a response, meaning that it will render views and strings into a response or
throw an exception to be returned. This response then gets handed back to the
layers sitting above which can modify the response object.

# Events

The central component of communication between all parts of the system is
the EventDispatcher which can be accessed via `$app['events']` (or
`$this('events')` when extending ApplicationAware). Every part can
listen to certain events and trigger events itself.

To subscribe to events, a class must implement the `EventSubscriberInterface`
and implement the static method `getSubscribedEvents`. This method will return
an array with the subscribed elements as keys and the callback function as the
according value.

```PHP
<?php

namespace Pagekit\Hello\Event;

use Pagekit\Framework\Event\EventSubscriberInterface;
use Pagekit\Framework\ApplicationAware;

class HelloListener extends ApplicationAware implements EventSubscriberInterface
{
    public function onBoot($event, $eventName, $dispatcher)
    {
        // do something
    }

    public function anyName() // omit parameters if you do not need them
    {
        // do something
    }

    public static function getSubscribedEvents()
    {
        return array(
            'hello.boot' => 'onBoot',

            // use any name and add a priority int if desired
            'hello.boot' => array('anyName', 10)
        );
    }
}
```

In the `boot` method of your extension (or theme), you need to register your
listener so it can subscribe to events.

```PHP
$this('events')->addSubscriber(new HelloListener());
```

You can also use the Application's `on` shorthand to react to events and also
just pass a closure.

```PHP
$app->on('hello.boot', function($event) use ($app){
    // do something
});
```

Dispatch events with the following syntax.

```PHP
// dispatch event, optional second $event parameter
$app['events']->dispatch('hello.boot');
```

## Event types

There is a number of different event types. A lot of the events you encounter
actually come from the kernel, you can find them with a detailed description
in `Symfony\Component\HttpKernel\KernelEvents`. Since every component can
dispatch events at any given time, there are a lot of different events.

A great tool to see the events is the profiler toolbar that you can enable in
Pagekit's admin area in the *Settings > System* section. You will get a bar
at the bottom of the browser window - just navigate to the *Events* tab to see
a detailed overview of events in your system and event listeners that were not
called during this request.
