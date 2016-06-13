# Events

A popular concept for communication in modern web application are events. An event is triggered during a certain phase of the code execution. Any other code, including your extensions, can register to one or multiple events and perform actions at that specific time.

An event is always defined by its identifier, a unique string (i.e. `boot`for the event that is triggered during the boot phase of the Pagekit Application).

In this document you will learn about the available events you can listen to, and how you can register an event listener in Pagekit.

## System Events

Pagekit provides a set of events during the lifecycle of page request:


- `boot`: The boot phase of the Pagekit application has started.
- `request`: The request handling of the Kernel has started.
- `controller`: A controller action is about to be called.
- `response`: The response is about to be sent to the browser.
- `terminate`: The response of Pagekit application has been sent successfully.
- `exception`: An exception has occured.


## Auth-Events

All events for authorization are defined in `Pagekit\Auth\AuthEvents`.


## Database / EntityManager

For each entity which is loaded, updated or created, a specific event is fired. For example if a widget is loaded a `model.widget.init` event is fired.

The schema for entity event names is `model.entity_short_name.event_name`. This generic event-system allows you to listen on single entities.

You can find the list of all EntityManager events in `Pagekit\Database\Events`.

## Router

The router triggers an event before and after a route is executed. Each event-name includes the executed url: `before@site/api/node/save` or `after@system/settings/save`

## Register an EventListener

Let's register a listener that is called when a page is saved.

You have to register the listener in your package `index.php`:

```
return [
    // your packge definition

    'events' => [

        'boot' => function ($event, $app) {
            $app->subscribe(new \Acme\Listener\PostSaveListener());
        }
    ]
];
```

And the listener-code:

```
<?php

namespace Acme\Listener;

use Pagekit\Event\Event;
use Pagekit\Event\EventSubscriberInterface;

class PostSaveListener implements EventSubscriberInterface
{
    /**
     * {@inheritdoc}
     */
    public function subscribe()
    {
        return [
            'model.page.saved' => 'onSaved',
        ];
    }

    /**
     * @param Event $event
     * @param object $model The saved entity
     */
    public function onSaved(Event $event, $model)
    {
        // your code
    }
}
```