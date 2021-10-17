# Database access

Once [properly instantiated](/guide), you can access the database in different ways. Direct SQL queries are possible from this moment. To fully understand how the query builder works, please refer to the [official Laravel documentation](https://laravel.com/docs/8.x/database).

## Quick example

```php
use Sematico\FluentQuery\DatabaseCapsule;

$db = new DatabaseCapsule();
$db->boot();

$exists = $db::schema()->hasTable('users'); // true
```

:::tip NOTE
WP Fluent Query automatically detects the site's database prefix and uses it while executing queries through some of the internal facades.
:::

## Direct SQL queries

You may run queries using the DB facade. The DB facade provides methods for each type of query: `select`, `update`, `insert`, `delete`, and `statement`.

```php
$results = $db::select( 'select * from wp_users' );
```

For more information please refer to the [official Laravel documentation](https://laravel.com/docs/8.x/database).

## Schema builder

The Laravel Schema class provides a mechanism to declare table structures, FKS and indexes using a declarative approach and schema comparison.

### Creating & Dropping Tables

To create a new database table, the `schema()->create` method is used:

```php
$result = $db::schema()->create('my_table', function($table) {
    $table->increments('id');
});
```

The first argument passed to the `create` method is the name of the table, and the second is a `Closure` which will receive a `Blueprint` object which may be used to define the new table.

To drop a table, you may use the `schema()->drop` method:

```php
$result = $db::schema()->drop('my_table');
```

For more information please refer to the [official Laravel documentation](https://laravel.com/docs/8.x/migrations#migration-structure).

## Query builder

The database query builder provides a convenient, fluent interface to creating and running database queries. You may use the `table` method provided by the DB facade to begin a query. The `table` method returns a fluent query builder instance for the given table.

### Retrieving all rows from a table

```php
$result = $db::table('users')->get();
```

For more information please refer to the [official Laravel documentation](https://laravel.com/docs/8.x/queries).