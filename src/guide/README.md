# Introduction

WP Fluent Query provides a convenient wrapper around Eloquent. It acts as a layer sitting on top of the internal WordPress `$wpdb` class and all queries are run through it.

This package has been created with the purpose of being used in public WordPress plugins where developers don't always have full control of the environment.

::: warning
This package is currently in beta. Breaking changes may occur until version 1.0.0 is tagged.
:::

## Requirements

- MySQL 5.7.
- PHP 7.3 or higher.
- Composer.

## Installation

Fluent Query is available as a composer package and can be installed using the following command in the root of your plugin:

```sh
composer require sematico/wp-fluent-query
```

In order to access the classes, make sure to include `vendor/autoload.php` in your plugin.

```php
include 'vendor/autoload.php';
```

## Get started

This guide provides a very quick overview on how to access the database. Other sections of the documentation will provide deeper insight into various use-cases.

To access the database and run queries, you must create a new instance of the `DatabaseCapsule` class and `boot` it.

```php
use Sematico\FluentQuery\DatabaseCapsule;

$db = new DatabaseCapsule();
$db->boot();
```

## Security Issues
If you discover a security vulnerability within Fluent Query, please email [alessandro.tesoro@icloud.com](mailto:alessandro.tesoro@icloud.com). All security vulnerabilities will be promptly addressed.

## License

Distributed under the MIT License. See `LICENSE` file for more information.
