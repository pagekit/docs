# Database
Instead of using PDO, you can and should use the database service provided by Pagekit.

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

## Insert
TODO

## Select
TODO

## Joins
TODO

## Migrations
TODO

## ORM
TODO
