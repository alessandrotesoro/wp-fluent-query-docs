# Eloquent & Built-in models

The Eloquent ORM provides a beautiful, simple ActiveRecord implementation for working with your database. Each database table has a corresponding "Model" which is used to interact with that table.

For more information please refer to the [official Laravel documentation](https://laravel.com/docs/8.x/eloquent).

## Built-in models

WP Fluent Query provides a few default models tailored to work with the WordPress database structure. The currently included models are:

- Post
- User
- Comment
- TermTaxonomy
- Term

## Basic usage

Once a model is defined, you are ready to start retrieving and creating records in your table.

### Retrieving all records
This will return a `Collection` of `User` models with all the users in your WordPress website.

```php
use Sematico\FluentQuery\Model\User;

$users = User::all(); // [...]
```

### Retrieving a record by primary key
This will return a `User` model for the user with the `ID` of `1`.
```php
$user = User::find(1);

var_dump($user->user_login);
```

### Querying using Eloquent models

```php
$users = User::where('ID', '>', 3)->get();

foreach ($users as $user)
{
    var_dump($user->user_login);
}
```

## User model
The `User` model provided by Fluent Query, comes with pre-configured relationships with the `Post` and `Comment` model.

### Get posts that belong to a user

Using the `posts` method on the `User` model, allows you to query posts that belong to the user.

```php
$query = User::find( 1 ); // get the user with the ID of 1

$posts = $query->posts()->get(); [...] // Collection of posts.
```

### Get comments submitted by a user

Using the `comments` method on the `User` model, allows you to query the comments submitted by a user.

```php
$query = User::find( 1 ); // get the user with the ID of 1

$comments = $query->comments()->get(); [...] // Collection of comments.
```

## Post model

The `Post` model provided by Fluent Query, comes with pre-configured relationships with the `Post`, `User` and `Comment` model.

### Get the first post ever submitted

```php
use Sematico\FluentQuery\Model\Post;

$post = Post::first(); // get the oldest post.
```

### Get the comments of a post

```php
$query = Post::first();

$comments = $query->comments()->get(); [...] // Collection of Comment models.
```

### Get the author of a post

```php
$query = Post::find( 8 );

$author = $query->author()->get();
```

### Get posts by post type

```php
$q = Post::ofType('page'); // Returns a Collection of pages
```
::: tip NOTE
You may use the `Page` model to query pages so that you do not have to use the `ofType` scope. For more information about scoping - please refer to the [Laravel documentation](https://laravel.com/docs/8.x/eloquent#query-scopes).
:::

### Get posts with the "published" status

```php
$q = Post::published()->get();
```

### Get published pages

```php
$q = Post::published()->ofType('page')->get();

// You can also do:
$q = Page::published()->get();
```

### Use a custom post status

```php
$q = Post::status('draft')->get();
```

## Comment model

The `Comment` model provided by Fluent Query, comes with pre-configured relationships with the `Post`, `User` and `Comment` model.

### Get a comment

```php
use Sematico\FluentQuery\Model\Comment;

$comment = Comment::first(); // get the oldest comment.
```

### Get the author of a comment

```php
$comment = Comment::first(); // get the oldest comment.

$author = $comment->author(); // returns a User model.
```

### Get replies to a comment


```php
$comment = Comment::first(); // get the oldest comment.

$replies = $comment->replies()->get(); // returns a collection of Comment models
```

## Ordering results

Fluent Query comes with 2 order by clauses to sort the models results by newest and oldest. Currently the `Post`, `User` and `Comment` models are the only ones supporting the clauses. 

You may use the `newest` and `oldest` methods to sort results accordingly.

```php
$q = Post::newest()->get();
```

## API documentation

For more information about all the methods, scopes and relationships provided by Fluent Query, please refer to the [source code on github](https://github.com/alessandrotesoro/wp-fluent-query) for the time being. A full documentation for the API will be provided at a later point.