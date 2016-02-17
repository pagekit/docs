# Database

<p class="uk-article-lead">This chapter talks about the basics of configuring the database connection, creating tables, running database scripts from the extensions and manually building database queries.</p>

**Note** To map your application data to database tables in a comfortable way, the recommended way is the [Pagekit Object-relational mapper (ORM)](orm.md) which is described in its own chapter.


## Configuration
Database credentials are stored in `config.php`. Pagekit supports `mysql`and `sqlite`.

```
'database' => [
    'connections' => [
      'mysql' => [
        'host' => 'localhost',
        'user' => 'root',
        'password' => 'PASSWORD',
        'dbname' => 'DATABASE',
        'prefix' => 'PREFIX_',
      ],
    ],
  ],
  ...
```

## Working with database prefixes
All table names include the prefix of your Pagekit installation. To dynamically address tables in the backend, use the table name with the `@` symbol as a placeholder for the prefix. As a convention you should start the table name with your extension name, e.g. _options_ table for the `foobar` extension: `@foobar_option`

## Database utility
You can manage your database schema using the database service utility (see the following examples).

```
$util = $this['db']->getUtility();
```

## Check if tables exists

```
if ($util->tablesExist(['@table1', '@table2'])) {
  // tables exists
}
```

## Create table
Use `Utility::createTable($table, \Closure $callback)` to create a table, the first parameter passed to the callback will be a `Doctrine\DBAL\Schema\Table` instance.

```
$util->createTable('@foobar_option', function($table) {
    $table->addColumn('id', 'integer', ['unsigned' => true, 'length' => 10, 'autoincrement' => true]);
    $table->addColumn('name', 'string', ['length' => 64, 'default' => '']);
    $table->addColumn('value', 'text');
    $table->addColumn('autoload', 'boolean', ['default' => false]);
    $table->setPrimaryKey(['id']);
    $table->addUniqueIndex(['name'], 'OPTION_NAME');
});
```

The `$table` object is an instance of `\Doctrine\DBAL\Schema\Table`. You can find its [class reference](http://www.doctrine-project.org/api/dbal/2.5/class-Doctrine.DBAL.Schema.Table.html) in the official Doctrine documentation.

When creating a column using `addColumn`, you might want to look at the available [data types](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/types.html) and the availabe [column options](http://docs.doctrine-project.org/projects/doctrine-dbal/en/latest/reference/schema-representation.html#portable-options) from the Doctrine documentation as well.

Creating a table is commonly done in the `install` hook of the `scripts.php` inside your extension. Read more about [Migrations](#migrations) in the next section.

## Migrations

Database migrations are defined in the `'updates'` section of the `scripts.php` in your extension. A full example for the [scripts.php](https://github.com/pagekit/extension-hello/blob/master/scripts.php) can be found in the Hello extension. Remember to link that file from your `composer.json` so that it is actually executed:

```
    "extra": {
       "scripts": "scripts.php"
    },
```

Within the `scripts.php`, you can hook into different events of the extension lifecycle.

- `install`: Called when the extension is installed. Usually you create your tables here.
- `enable`: Called when the extension is enabled in the admin area.
- `uninstall`: Called when the extension is removed. This is the place to tidy up whatever you have created, i.e. drop all tables of your extension.
- `updates`: Run any code when your extension is updated. Expects an array where each key is the version number from which this code should be run. Example:

    ```
     /*
     * Runs all updates that are newer than the current version.
     *
     */
    'updates' => [
        '0.5.0' => function ($app) {
            // executed when upgrading from a version older than 0.5.0
        },
        '0.9.0' => function ($app) {
            // executed when upgrading from a version older than 0.9.0
        },
    ],
    ```

### Alter an existing table

To alter an existing table, use the existing tools of the underlying Doctrine DBAL. To add columns to an existing table, you can include the following snippets in one of the `updates` version hooks of your extension's `scripts.php`.

```
use Doctrine\DBAL\Schema\Comparator;

// ...

$util    = App::db()->getUtility();
$manager = $util->getSchemaManager();

if ($util->tableExists('@my_table')) {

    $tableOld = $util->getTable('@my_table');
    $table = clone $tableOld;

    $table->addColumn('title', 'string', ['length' => 255]);

    $comparator = new Comparator;
    $manager->alterTable($comparator->diffTable($tableOld, $table));
}

```

The `$table` object is an instance of `\Doctrine\DBAL\Schema\Table`. You can find its [class reference](http://www.doctrine-project.org/api/dbal/2.5/class-Doctrine.DBAL.Schema.Table.html) in the official Doctrine documentation.

## Queries

There are several ways of accessing the database. Pagekit offers an abstracting on the underlying MySQL or SQLite, so there is no need to use PDO or similar mechanisms.

### 1. Query builder

The [QueryBuilder](https://github.com/pagekit/pagekit/blob/develop/app/modules/database/src/Query/QueryBuilder.php) allows for a more comfortable way of creating queries.

Example:

```
$result = Application::db()->createQueryBuilder()->select('*')->from('@blog_post')->where('id = :id', ['id' => 1])->execute()->fetchAll();
```

#### Get a query builder object

```
use Pagekit\Application;

// ...

$query = Application::db()->createQueryBuilder();
```

#### Basic selects and conditions

- `select($columns = ['*'])`: Creates and adds a "select" to the query.
- `from($table)`: Creates and sets a "from" to the query.
- `where($condition, array $params = [])`: Creates and adds a "where" to the query.
- `orWhere($condition, array $params = [])`: Creates and adds a "or where" to the query.

Example:

```
// create query
$query = $query = Application::db()->createQueryBuilder();

// fetch title and content of all blog posts that do not have any comments
$comments = $query
    ->select(['title', 'content'])
    ->from('@blog_post')
    ->where('comment_count = ?', [0])
    ->get();
```

#### Query execution

- `get($columns = ['*'])`: Execute the query and get all results.
- `first($columns = ['*'])`: Execute the query and get the first result.
- `count($column = '*')`: Execute the query and get the "count" result.
- `execute($columns = ['*'])`: Execute the "select" query.
- `update(array $values)`: Execute the "update" query with the given values.
- `delete()`: Execute the "delete" query.


#### Aggregate functions

- `min($column)`: Execute the query and get the "min" result.
- `max($column)`: Execute the query and get the "max" result.
- `sum($column)`: Execute the query and get the "sum" result.
- `avg($column)`: Execute the query and get the "avg" result.

Example:

```
// create query
$query = $query = Application::db()->createQueryBuilder();

// determine total number of blog comments
$count = $query
    ->select(['comment_count'])
    ->from('@blog_post')
    ->sum('comment_count');
```

#### Advanced query methods

- `whereIn($column, $values, $not = false, $type = null)`: Creates and adds a "where in" to the query.
- `orWhereIn($column, $values, $not = false)`
- `whereExists(Closure $callback, $not = false, $type = null)`: Creates and adds a "where exists" to the query.
- `orWhereExists(Closure $callback, $not = false)`: Creates and adds a "or where exists" to the query.
- `whereInSet($column, $values, $not = false, $type = null)`: Creates and adds a "where FIND_IN_SET" equivalent to the query.
- `groupBy($groupBy)`: Creates and adds a "group by" to the query.
- `having($having, $type = null)`: Creates and adds a "having" to the query.
- `orHaving($having)`: Creates and adds a "or having" to the query.
- `orderBy($sort, $order = null)`: Creates and adds an "order by" to the query.
- `offset($offset)`: Sets the offset of the query which means that the results will not start with the first result but with the result defined by the integer index `$offset`. This is useful for paging.
- `limit($limit)`: Sets the limit of the query. `$limit` defines the maximum count of results to be returned.
- `getSQL()`: Gets the query SQL.

#### Joins

- `join($table, $condition = null, $type = 'inner')`: Creates and adds a "join" to the query.
- `innerJoin($table, $condition = null)`: Creates and adds an "inner join" to the query.
- `leftJoin($table, $condition = null)`: leftJoin($table, $condition = null)
- `rightJoin($table, $condition = null)`: Creates and adds a "right join" to the query.

### 2. ORM Queries

When you have set up [ORM](orm.md) in your extension, you can create very readable queries using your model class.

Example:

```
$result = Role::where(['id <> ?'], [Role::ROLE_ANONYMOUS])->orderBy('priority')->get();
```

The following methods are available (defined in the [ModelTrait](https://github.com/pagekit/pagekit/blob/develop/app/modules/database/src/ORM/ModelTrait.php)).

- `create($data = [])`: Creates a new instance of this model from the passed in data array.
- `where($condition, array $params = [])`: Specify a where condition. Question marks in the condition are replaced by the parameters that you pass in. Returns a `QueryBuilder` object so you can chain method calls for more specific queries. Example: `User::where(['name = ?'], ['peter'])`
- `find($id)`: Retrieves a model entity by its identifier.
- `findAll()`: Retrieves all entities of this model.
- `save(array $data = [])`: Saves the model entity.
- `delete()`: Deletes the model entity.
- `toArray(array $data = [], array $ignore = [])`: Returns the model data as an array. Pass in a list of property keys to be included as the `$data` parameter. Pass in a list of property keys to be excluded as the `$ignore` parameter.
- `query()`: Returns a `ORM\QueryBuilder` instance to use any methods from that class. This instance offers all methods from the regular query builder, plus some additional ones, specifically for ORM.

#### ORM Query Builder: Additional methods

- `get()`: Executes the query and gets all results.
- `first()`: Executes the query and gets the first result.
- `related($related)`: Set the relations that will be eager loaded.
- `getRelations()`: Gets all relations of the query.
- `getNestedRelations($relation)`: Gets all nested relations of the query.

Example:

```
$comments = Comment::query()->related(['post' => function ($query) {
    return $query->related('comments');
}])->get();
```

### 3. Raw queries

The plainest way to query the database is by sending raw queries to the database. This is basically just a wrapper around PDO.

```
$result = Application::db()->executeQuery('select * from @blog_post')->fetchAll();
$result = Application::db()->executeQuery('select * from @blog_post WHERE id = :id', ['id' => 1])->fetchAll();
```

## Insert

Inserting data in the database can be done using the database connection instance that you can fetch via `Application::db()` (remember to `use Pagekit\Application;` at the top of your file).

Use the method `insert($tableExpression, array $data, array $types = array())`


Example:

```
Application::db()->insert('@system_page', [
    'title' => 'Home',
    'content' => "<p>Hello World</p>",
    'data' => '{"title":true}'
]);
```

When using [ORM](#ORM), you just need to create a new model instance and call the `save()` method.



## ORM

With the Object-relational mapping (ORM) in Pagekit, you can bind a Model class to a database table. While this takes a few more lines to setup than the QueryBuilder, the ORM takes a lot of manual work out of your hands. Using ORM is the recommended way of managing how you store and retrieve your application data to and from the database. Read more about the [Pagekit ORM](orm.md).
