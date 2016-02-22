# ORM
<p class="uk-article-lead">The Pagekit Object-relational mapper (ORM) helps you create model classes of your application data where each property is mapped to the according table column automatically. You can also define relations between your entities and the existing entities from Pagekit (i.e. users)</p>

## Setup

### Create tables

Run the following, i.e. in the `install` hook of your extension's `scripts.php`. For more general information on creating tables, have a look at the [database chapter](database.md).

Example:

```php
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
class Topic
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

A few things to note:

A model is a plain PHP class uses the trait `Pagekit\Database\ORM\ModelTrait`. If you are unfamiliar with traits, have a quick looks at the [official PHP documentation on traits](http://php.net/manual/en/language.oop5.traits.php). Basically it is a concept to pull certain behaviour into a class - similar to simple class inheritance. The main difference is that a class can use multiple traits while it could only inherit from one single class.

The annotation `@Entity(tableClass="@my_table")` binds the Model to the database table `pk_my_table` (`@` is automatically replaced by the database prefix of your installation )

Annotations will only work if you start the multiline comment with two asterisks, not just with one.

```
// will NOT work:
/* @Column */

// will work:
/** @Column */

// will work:
/**
 * @Column
 */
```

When defining a property in a class, you can bind that variable to a table column, by putting the `/** @Column(type="string") */` annotation right above the property definition. You can use any types supported by [Doctrine DBAL](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/types.html).

The class you reference in your model class also has to exist in the database.


## Relations

The application data you represent in your database model has certain relations amongst its instances. A blog post has a number of comments related to it and it belongs to exactly one User instance. The Pagekit ORM offers mechanisms to define these relations and also to query them in a programmatic manner.

### Belongs-to relation

The basic annotation that is used across the different relation types it the `@BelongsTo` annotation above a model property. In the following example (taken from the `Post` model of the Blog) we specify a `$user` property, which is defined to point to the instance of the Pagekit `User` model.

The `keyFrom` parameter specify which source property is used to point to the user id. Note how we also need to define the according `user_id` property in order for the relationship to be resolved by a query.

Example:

```
/** @Column(type="integer") */
public $user_id;

/**
 * @BelongsTo(targetEntity="Pagekit\User\Model\User", keyFrom="user_id")
 */
public $user;
```

### One-to-many relation

In this relationship, a single model instance has references to an arbitrary
amount of instances of another model. A classic example for this is a `Post`
which has any number of `Comment` instances that belong to it. On the inverse
side, a comment belongs exactly one `Post`.

Example from the blog package, in `Pagekit\Blog\Model\Post`.

```
/**
 * @HasMany(targetEntity="Comment", keyFrom="id", keyTo="post_id")
 */
public $comments;
```

Define the inverse of the relation in `Pagekit\Blog\Model\Comment`:

```
/** @Column(type="integer") */
public $post_id;

/** @BelongsTo(targetEntity="Post", keyFrom="post_id") */
public $post;
```

To query the Model, you can use the ORM class.

```
use Pagekit\Blog\Post;

// ...

// fetch posts without related comments
$posts = Post::findAll();
var_dump($posts);
```

Output:

```
array (size=6)
  1 =>
    object(Pagekit\Blog\Model\Post)[4513]
      public 'id' => int 1
      public 'title' => string 'Hello Pagekit' (length=13)
      public 'comments' => null
      // ...

  2 =>
    object(Pagekit\Blog\Model\Post)[3893]
      public 'id' => int 2
      public 'title' => string 'Hello World' (length=11)
      public 'comments' => null
      // ...

  // ...
```

```
use Pagekit\Blog\Post;

// ...

// fetch posts including related comments
$posts = Post::query()->related('comments')->get();
var_dump($posts);
```

Output:

```
array (size=6)

  1 =>
    object(Pagekit\Blog\Model\Post)[4512]
      public 'id' => int 1
      public 'title' => string 'Hello Pagekit' (length=13)
      public 'comments' =>
        array (size=0)
          empty
      // ...

  2 =>
    object(Pagekit\Blog\Model\Post)[3433]
      public 'id' => int 2
      public 'title' => string 'Hello World' (length=11)
      public 'comments' =>
        array (size=1)
          6 =>
            object(Pagekit\Blog\Model\Comment)[4509]
              ...
      // ...

  // ...
```

### One-to-one relation

A very simple relationship is the one-to-one relation. A `ForumUser` might have exactly one `Avatar` assigned to it. While you simply include all information about the avatar inside the `ForumUser` model, it sometimes makes sense to split these in separate models.

To implement the one-to-one relation, you can use the `@BelongsTo` annotation in each model class.

`/** @BelongsTo(targetEntity="Avatar", keyFrom="avatar_id", keyTo="id") */`

- `targetEntity`: The target model class
- `keyFrom`: foreign key in this table pointing to the related model
- `keyTo`: primary key in the related model

Example model `ForumUser`:

```php
<?php

namespace Pagekit\Forum\Model;

use Pagekit\Database\ORM\ModelTrait;

/**
 * @Entity(tableClass="@forum_user")
 */
class ForumUser
{

    use ModelTrait;

    /** @Column(type="integer") @Id */
    public $id;

    /** @Column */
    public $name = '';

    /** @Column(type="integer") */
    public $avatar_id;

    /** @BelongsTo(targetEntity="Avatar", keyFrom="avatar_id", keyTo="id") */
    public $avatar;

}
```

Example model `Avatar`:

```php
<?php

namespace Pagekit\Forum\Model;

use Pagekit\Database\ORM\ModelTrait;

/**
 * @Entity(tableClass="@forum_avatars")
 */
class Avatar
{

    use ModelTrait;

    /** @Column(type="integer") @Id */
    public $id;

    /** @Column(type="string") */
    public $path;

    /** @Column(type="integer") */
    public $user_id;

    /** @BelongsTo(targetEntity="ForumUser", keyFrom="user_id", keyTo="id") */
    public $user;

}
```

To make sure the related model is included in a query result, fetch the `QueryBuilder` instance from the model class and explicitly list the relation property in the `related()` method.

```php
<?php

use Pagekit\Forum\Model\ForumUser;
use Pagekit\Forum\Model\Avatar;

// ...

// get all users including their related $avatar object
$users = ForumUser::query()->related('avatar')->get();
foreach ($users as $user) {
    var_dump($user->avatar->path);
}

// get all avatars including their related $user object
$avatars = Avatar::query()->related('user')->get();
foreach ($avatars as $avatar) {
    var_dump($avatar->user);
}
```


### Many-to-many relation

Sometimes, two models are in a relation where there are potentially *many instances* on both sides of the relation. An example would be a relation between tags and posts: One post can have several tags assigned to it. At the same time, one tag can be assigned to multiple posts.

A different example that is listed below, is the scenario of favorite topics in a discussion forum. A user can have multiple favorite topics. One topic can be favorited by multiple users.

To implement the many-to-many relation, you need an additional database table. Each entry in that table represents a connection from a `Topic` instance to a `ForumUser` instance and vice versa. In database modelling, this is called a [junction table](https://en.wikipedia.org/wiki/Associative_entity).

Example tables (i.e. in `scripts.php`):

```
$util = $app['db']->getUtility();

// forum user table
if ($util->tableExists('@forum_users') === false) {
    $util->createTable('@forum_users', function ($table) {
        $table->addColumn('id', 'integer', ['unsigned' => true, 'length' => 10, 'autoincrement' => true]);
        $table->addColumn('name', 'string', ['length' => 255, 'default' => '']);
        $table->setPrimaryKey(['id']);
    });
}

// topics table
if ($util->tableExists('@forum_topics') === false) {
    $util->createTable('@forum_topics', function ($table) {
        $table->addColumn('id', 'integer', ['unsigned' => true, 'length' => 10, 'autoincrement' => true]);
        $table->addColumn('title', 'string', ['length' => 255, 'default' => '']);
        $table->addColumn('content', 'text');
        $table->setPrimaryKey(['id']);
    });
}

// junction table
if ($util->tableExists('@forum_favorites') === false) {
    $util->createTable('@forum_favorites', function ($table) {
        $table->addColumn('id', 'integer', ['unsigned' => true, 'length' => 10, 'autoincrement' => true]);
        $table->addColumn('user_id', 'integer', ['unsigned' => true, 'length' => 10, 'default' => 0]);
        $table->addColumn('topic_id', 'integer', ['unsigned' => true, 'length' => 10, 'default' => 0]);
        $table->setPrimaryKey(['id']);
    });
}
```

The relation itself is then defined in each Model class where you want to be able to query it. If you only want to list the favorite posts for a specific user, but you do not lists all user who have favorited a given post, you would only define the relation in one model. In the following example however, the `@ManyToMany` annotation is located in both model classes.

The `@ManyToMany` annotation takes the following parameters.

- `targetEntity`: The target model class
- `tableThrough`  Name of the junction table
- `keyThroughFrom` Name of the foreign key in "from" direction
- `keyThroughTo` Name of the foreign key in "to" direction
- `orderBy` (optional) Order by statement

Example annotation:

```php
/**
 * @ManyToMany(targetEntity="ForumUser", tableThrough="@forum_favorites", keyThroughFrom="topic_id", keyThroughTo="forum_user_id")
 */
public $users;
```

Example model `Topic`:

```php
<?php

namespace Pagekit\Forum\Model;

use Pagekit\Database\ORM\ModelTrait;

/**
 * @Entity(tableClass="@forum_topics")
 */
class Topic
{

    use ModelTrait;

    /** @Column(type="integer") @Id */
    public $id;

    /** @Column */
    public $title = '';

    /** @Column(type="text") */
    public $content = '';

    /**
     * @ManyToMany(targetEntity="ForumUser", tableThrough="@forum_favorites", keyThroughFrom="topic_id", keyThroughTo="forum_user_id")
     */
    public $users;

}
```

Example model `ForumUser`:

```php
<?php

namespace Pagekit\Forum\Model;

use Pagekit\Database\ORM\ModelTrait;

/**
 * @Entity(tableClass="@forum_user")
 */
class ForumUser
{

    use ModelTrait;

    /** @Column(type="integer") @Id */
    public $id;

    /** @Column */
    public $name = '';

    /**
     * @ManyToMany(targetEntity="Topic", tableThrough="@forum_favorites", keyThroughFrom="forum_user_id", keyThroughTo="topic_id")
     */
    public $topics;

}
```

Example queries:

```php
// resolve many-to-many relation in query

// fetch favorite ropics for given user
$user_id = 1;
$user = ForumUser::query()->where('id = ?', [$user_id])->related('topics')->first();

foreach ($user->topics as $topic) {
    //
}

// fetch users that have favorited a given topic
$topic_id = 1;
$topic = Topic::query()->where('id = ?', [$topic_id])->related('users')->first();

foreach ($topic->users as $user) {
    // ...
}
```

## ORM Queries

Fetch a model instance with a given id.

```
$post = Post::find(23)
```

Fetch all instances of a model.

```
$posts = Post::findAll();
```

With the above queries, relations will not be expanded to include related instances. In above example, the `Post` instance will not have its `$comments` property initialized.

```
// related objects are not fetched by default
$post->comments == null;
```

The reason for this is performance. By default, the required subqueries are not performed, which saves execution time. So if you need the related objects, you can use the `related()` method on the `QueryBuilder` to explicitly state which relations to resolve in this query.

So, to fetch a `Post` instance and include the associated `Comment` instances, you need to build a query which fetches the related objects.

```
// fetch all, including related objects
$posts = Post::query()->related('comments')->get();

// fetch single instance, include related objects
$id = 23;
$post = Post::query()->related('comments')->where('id = ?', [$id])->first();
```

Note how the `find(23)` has been replaced with `->where('id = ?', [$id])->first()`. This is because `find()` is a method defined on the Model. In the second example however, we have an instance of `Pagekit\Database\ORM\QueryBuilder`.

For more details on ORM queries and the regular queries, check out the documentation on [database queries](database.md#queries)

## Create new model instance

You can create and save a new model by calling the `save()` method on a fresh model instance.

```php
$user = new ForumUser();
$user->name = "bruce";
$user->save();
```

Alternatively you can call the `create()` method on the model class directly and provide an array of existing data to initialize the instance. Call `save()` afterwards to store the instance to the database.

```php
$user = ForumUser::create(["name" => "peter"]);
$user->save();
```

## Modify existing instance

Fetch an existing instance, perform any changes on the object and then call the `save()` method to store changes to the database.

```php
$user = ForumUser::find(2);
$user->name = "david";
$user->save();
```

## Delete existing instance

Fetch an existing model instance and call the `delete()` method to remove this instance from the database.

```php
$user = ForumUser::find(2);
$user->delete();
```
