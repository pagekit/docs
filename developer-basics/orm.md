# ORM
<p class="uk-article-lead">The Pagekit Object-relational mapper (ORM) helps you create model classes of your application data where each property is mapped to the according table column automatically. You can also define relations between your entities and the existing entities from Pagekit (i.e. users)</p>

## Setup

### Create tables

Run the following, i.e. in the `install` hook of your extension's script.php.

Example:

```
$util = $app['db']->getUtility();

if ($util->tableExists('@forum_topics') === false) {
    $util->createTable('@forum_topics', function ($table) {
        $table->addColumn('id', 'integer', ['unsigned' => true, 'length' => 10, 'autoincrement' => true]);
        $table->addColumn('user_id', 'integer', ['unsigned' => true, 'length' => 10, 'default' => 0]);
        $table->addColumn('title', 'string', ['length' => 255, 'default' => '']);
        $table->addColumn('date', 'datetime');
        $table->addColumn('modified', 'datetime', ['notnull' => false]);
        $table->addColumn('content', 'text');
        $table->addIndex(['user_id'], 'FORUM_TOPIC_USER_ID');
        $table->setPrimaryKey(['id']);
    });
}
```


### Define a Model class

Example:

```
<?php

namespace Pagekit\Forum\Model;

use Pagekit\Database\ORM\ModelTrait;

/**
 * @Entity(tableClass="@forum_topics")
 */
class Topic implements \JsonSerializable
{

    use ModelTrait;

    /** @Column(type="integer") @Id */
    public $id;

    /** @Column */
    public $title = '';

    /** @Column(type="datetime") */
    public $date;


    /** @Column(type="text") */
    public $content = '';

    /** @Column(type="integer") */
    public $user_id;

    /**
     * @BelongsTo(targetEntity="Pagekit\User\Model\User", keyFrom="user_id")
     */
    public $user;

}

```

## Relations

### Belongs-to relation

Example:

```
/** @Column(type="integer") */
public $user_id;

/**
 * @BelongsTo(targetEntity="Pagekit\User\Model\User", keyFrom="user_id")
 */
public $user;
```

### Has-many relation

TODO

### Has-one relation

TODO

### Many-to-many relation

TODO

### Many-to-many relation

TODO

## ORM Queries


Fetch a `Topic` instance with a given id.

```
Topic::find(23)
```

With the above query, relations will not be expanded to have more optimal queries.

```
$topic->user == null;
```

If you want to fetch the associated `User` object as well, you need to build a query which fetches the related objects.

```
$id = 23;
Topic::query()->related('user')->where('id = ?', [$id])->first();
```

Note how the `find(23)` has been replaced with `->where('id = ?', [$id])->first()`. This is because `find()` is a method defined on the Model. In the second example however, we have an instance of `Pagekit\Database\ORM\QueryBuilder`.
