# Metadata relationships

WP Fluent Query comes with a helpful trait that adds the ability to access meta data as if it is a property on your model. The metadata is fluent, just like using an eloquent model attribute you can set or unset metas.

::: tip NOTE
For the full list of all available methods, please refer to the [source code on github](https://github.com/alessandrotesoro/wp-fluent-query). An API documentation will be published at a later point.
:::

## Setting and getting metadata

The default models that come with WP Fluent Query are already configured to support WordPress metadata. To save and get metadata from a model, you can use the `saveMeta` and `getMeta` methods.

### Save metadata

```php
$post = Post::first();
$post->saveMeta('color', 'blue'); // creates a "color" post meta.
```

::: warning NOTE
Metadata created using the `saveMeta` method - does not use the `update_post_meta` function provided by WordPress. Therefore any hooks within the function, are not triggered when using the methods provided the models.
:::

You can create multiple meta like this:

```php
$post->saveMeta([
    'key1' => 'value1',
    'key2' => 'value2'
]);
```

### Get metadata

```php
$post = Post::first();
$post->getMeta('color'); // blue
```

When passing a 2nd argument to the `getMeta` method, the argument will be returned if no metadata is found.

```php
$post = Post::first();
$post->getMeta('not_existing', 'red'); // red
```

### Get all metadata

The `getAllMeta` method returns the list of all metadata and their value associated with the model.

```php
$post->getAllMeta(); // [....]
```

## Checking for metadata

You can use the `hasMeta` method if you want to check if there is a particular meta or not ( if meta exists but value equals to null false will be returned ).

```php
$post->hasMeta('key'); // return true or false
```

## Deleting metadata

You may use the `deleteMeta` method to delete a meta (or metas) for a given key (or keys).

```php
$post->deleteMeta( 'color' );

// More than one
$post->deleteMeta( [ 'color', 'another_key' ] );
```

### Delete all metadata

To completely delete all metadata associated with a model, you may use the `deleteAllMeta` method.

## Querying and scoping

The trait provides a number of query scopes to facilitate modifying queries based on the meta attached to your models.

### Checking for presence of a key

To only return records that have a value assigned to a particular key, you can use whereHasMeta(). You can also pass an array to this method, which will cause the query to return any models attached to one or more of the provided keys.

```php
$models = Post::whereHasMeta('notes')->get();
$models = Post::whereHasMeta(['queued_at', 'sent_at'])->get();
```

f you would like to restrict your query to only return models with meta for all of the provided keys, you can use `whereHasMetaKeys()`.

```php
$post = Post::whereHasMetaKeys(['step1', 'step2', 'step3'])->get();
```

You can also query for records that does not contain a meta key using the `whereDoesntHaveMeta()`. Its signature is identical to that of `whereHasMeta()`.

```php
$models = Post::whereDoesntHaveMeta('notes')->get();
$models = Post::whereDoesntHaveMeta(['queued_at', 'sent_at'])->get();
```

## Comparing value

You can restrict your query based on the value stored at a meta key. The `whereMeta()` method can be used to compare the value using any of the operators accepted by the Laravel query builder’s `where()` method.

```php
// omit the operator (defaults to '=')
$models = Post::whereMeta('letters', ['a', 'b', 'c'])->get();

// greater than
$models = Post::whereMeta('name', '>', 'M')->get();
```

The `whereMetaIn()` method is also available to find records where the value is matches one of a predefined set of options.

```php
$models = Post::whereMetaIn('country', ['CAN', 'USA', 'MEX']);
```

When comparing integer or float values with the <, <=, >= or > operators, use the `whereMetaNumeric()` method. This will cast the values to a number before performing the comparison.

```php
$posts = Post::whereMetaNumeric('counter', '>', 42)->get();
```

## Ordering results

You can apply an order by clause to the query to sort the results by the value of a meta key.

```php
// order by string value
$models = Post::orderByMeta('nickname', 'asc')->get();

//order by numeric value
$models = Post::orderByMetaNumeric('score', 'desc')->get();
```

By default, all records matching the rest of the query will be ordered. Any records which have no meta assigned to the key being sorted on will be considered to have a value of `null`.

To automatically exclude all records that do not have meta assigned to the sorted key, pass `true` as the third argument. This will perform an inner join instead of a left join when sorting.

```php
// sort by score, excluding models which have no score
$model = Post::orderByMetaNumeric('score', 'desc', true)->get();

//equivalent to, but more efficient than
$models = Post::whereHasMeta('score')
    ->orderByMetaNumeric('score', 'desc')->get();
```

## About hooks

Metadata created, retrieved and deleted using the `saveMeta`, `getMeta` and `deleteMeta` methods - do not use the internal functions provided by WordPress. Hooks are not triggered when using the methods provided the models.