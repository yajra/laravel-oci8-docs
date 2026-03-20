---
title: "Stand-Alone Usage"
description: "Using Laravel-OCI8 outside Laravel/Lumen with Capsule Manager."
---

# Stand-Alone Usage

Laravel-OCI8 can be used outside of Laravel or Lumen by leveraging Laravel's Capsule Manager. This enables you to use Oracle database functionality in any PHP project.

<a name="installation"></a>
## Installation

1. Add the package to your `composer.json`:

```json
{
    "require": {
        "yajra/laravel-oci8": "^13.0"
    }
}
```

2. Install the dependencies:

```bash
composer install
```


<a name="basic-setup"></a>
## Basic Setup


<a name="create-database-config"></a>
### Create a Database Configuration File

```php
// database.php
require 'vendor/autoload.php';

use Illuminate\Database\Capsule\Manager as Capsule;
use Yajra\Oci8\Connectors\OracleConnector;
use Yajra\Oci8\Oci8Connection;

$capsule = new Capsule;

// Extend the Oracle driver
$capsule->getDatabaseManager()->extend('oracle', function ($config) {
    $connector = new OracleConnector();
    $connection = $connector->connect($config);
    $db = new Oci8Connection($connection, $config['database'], $config['prefix']);

    // Configure Oracle session variables
    $sessionVars = [
        'NLS_TIME_FORMAT'          => 'HH24:MI:SS',
        'NLS_DATE_FORMAT'          => 'YYYY-MM-DD HH24:MI:SS',
        'NLS_TIMESTAMP_FORMAT'     => 'YYYY-MM-DD HH24:MI:SS',
        'NLS_TIMESTAMP_TZ_FORMAT'  => 'YYYY-MM-DD HH24:MI:SS TZH:TZM',
        'NLS_NUMERIC_CHARACTERS'   => '.,',
    ];

    // Set the current schema (optional)
    if (isset($config['schema'])) {
        $sessionVars['CURRENT_SCHEMA'] = $config['schema'];
    }

    $db->setSessionVars($sessionVars);

    return $db;
});

// Add the Oracle connection
$capsule->addConnection([
    'driver'   => 'oracle',
    'host'     => 'oracle.host',
    'database' => 'xe',
    'username' => 'user',
    'password' => 'password',
    'prefix'   => '',
    'port'     => 1521,
]);
```


<a name="enable-eloquent"></a>
### Enable Eloquent (Optional)

```php
// Enable Eloquent ORM
$capsule->bootEloquent();

// Set the event dispatcher (optional)
use Illuminate\Events\Dispatcher;
use Illuminate\Container\Container;

$dispatcher = new Dispatcher(new Container);
$capsule->setEventDispatcher($dispatcher);

// Make the Capsule available globally (optional)
$capsule->setAsGlobal();
```


<a name="using-the-database"></a>
## Using the Database

Now you can use the database just like in Laravel:

```php
// database.php
require 'database.php';

// Define a model
class User extends Illuminate\Database\Eloquent\Model
{
    public $timestamps = false;
    protected $fillable = ['name', 'email'];
}

// Query the database
$user = User::find(1);

if ($user) {
    echo $user->toJson();
}

// Create a new user
$user = new User();
$user->name = 'John Doe';
$user->email = 'john@example.com';
$user->save();

// Use the query builder
$users = Capsule::table('users')
    ->where('active', true)
    ->orderBy('name')
    ->get();
```


<a name="full-example"></a>
## Full Example: Complete Setup

```php
<?php
// bootstrap.php

require __DIR__ . '/vendor/autoload.php';

use Illuminate\Database\Capsule\Manager as Capsule;
use Illuminate\Events\Dispatcher;
use Illuminate\Container\Container;
use Yajra\Oci8\Connectors\OracleConnector;
use Yajra\Oci8\Oci8Connection;

$capsule = new Capsule;

// Configure Oracle connection
$capsule->getDatabaseManager()->extend('oracle', function ($config) {
    $connector = new OracleConnector();
    $connection = $connector->connect($config);
    $db = new Oci8Connection($connection, $config['database'], $config['prefix']);

    $db->setSessionVars([
        'NLS_DATE_FORMAT' => 'YYYY-MM-DD HH24:MI:SS',
    ]);

    return $db;
});

$capsule->addConnection([
    'driver'   => 'oracle',
    'host'     => getenv('ORACLE_HOST') ?: 'localhost',
    'database' => getenv('ORACLE_DATABASE') ?: 'xe',
    'username' => getenv('ORACLE_USER') ?: 'system',
    'password' => getenv('ORACLE_PASSWORD') ?: 'oracle',
    'port'     => getenv('ORACLE_PORT') ?: 1521,
]);

// Bootstrap Eloquent ORM
$capsule->bootEloquent();
$capsule->setEventDispatcher(new Dispatcher(new Container));
$capsule->setAsGlobal();
```


<a name="benefits-of-stand-alone"></a>
## Benefits of Stand-Alone Usage

- **Framework agnostic**: Use Oracle with any PHP project
- **Familiar API**: Use Laravel's query builder and Eloquent
- **Oracle features**: Access Oracle-specific functionality


<a name="see-also"></a>
## See Also

- [Installation Guide](installation) - Laravel-specific installation
- [Oracle Eloquent Model](oracle-eloquent) - Eloquent features for Oracle
- [General Settings](general-settings) - Configuration options
