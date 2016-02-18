## System Events

Pagekit provides a set of events during the lifecycle of page request:


- `boot`
- `request`
- `terminate`
- `controller`
- `response`
- `exception`


## Auth-Events

All events for authorization are defined in `Pagekit\Auth\AuthEvents`.


## Database / EntityManager

For each entity which is loaded, updated or created a specific event is fired. For example if a widget is loaded a `model.widget.init` event is fired.

The schema for a entity-event names is in general `model.entity_short_name.event_name`. This generic event-system allows you to listen on single entities.

You can find the list of all EntityManager events in `Pagekit\Database\Events`. 

## Router

The router fires before and after a route is executed a event. Each event-name includes the executed url: `before@site/api/node/save` or `after@system/settings/save`

## Register a EventListener

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