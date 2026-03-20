---
title: "Support"
description: "Support resources and troubleshooting for Laravel-OCI8."
---

# Support

Laravel-OCI8 integrates seamlessly with Laravel's database system. This guide shows how to establish Oracle database connections in your application.

<a name="setting-up-connection"></a>
## Setting Up the Connection

Configure your Oracle database connection in `config/database.php`:

```php
'connections' => [
    'oracle' => [
        'driver'          => 'oracle',
        'host'            => env('DB_HOST', ''),
        'port'           => env('DB_PORT', '1521'),
        'database'       => env('DB_DATABASE', ''),
        'service_name'   => env('DB_SERVICE_NAME', ''),
        'username'       => env('DB_USERNAME', ''),
        'password'       => env('DB_PASSWORD', ''),
        'charset'        => env('DB_CHARSET', 'AL32UTF8'),
        'prefix'         => env('DB_PREFIX', ''),
    ],
],
```


<a name="accessing-oracle-connection"></a>
## Accessing the Oracle Connection


<a name="using-query-builder"></a>
### Using the Query Builder

```php
use Illuminate\Support\Facades\DB;

// Query the Oracle connection
$users = DB::connection('oracle')
    ->table('users')
    ->where('active', true)
    ->get();
```


<a name="setting-default-connection"></a>
### Setting a Default Connection

In `config/database.php`:

```php
'default' => env('DB_CONNECTION', 'oracle'),
```


<a name="working-with-models"></a>
## Working with Models


<a name="using-the-config"></a>
### Using the Config

In your Eloquent models, specify the connection:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    protected $connection = 'oracle';
    protected $table = 'users';
}
```


<a name="using-a-base-model"></a>
### Using a Base Model

For projects that use Oracle exclusively, set the default connection in a base model:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

abstract class BaseModel extends Model
{
    protected $connection = 'oracle';
}
```


<a name="learning-more"></a>
## Learning More

For comprehensive documentation on Laravel's database features, see the official Laravel documentation:

- [Laravel Database Documentation](https://laravel.com/docs/database)
- [Query Builder](https://laravel.com/docs/queries)
- [Eloquent ORM](https://laravel.com/docs/eloquent)


<a name="getting-help"></a>
## Getting Help


<a name="community-support"></a>
### Community Support

- **GitHub Issues**: [Report bugs and request features](https://github.com/yajra/laravel-oci8/issues)
- **Stack Overflow**: Ask questions using the `laravel-oci8` tag


<a name="professional-support"></a>
### Professional Support

For commercial support and consulting, contact the maintainer directly.


<a name="troubleshooting"></a>
## Troubleshooting


<a name="common-issues"></a>
### Common Issues

**Connection Refused**

```php
// Verify your Oracle credentials
// Check that the Oracle listener is running
// Ensure the host and port are correct
```

**Invalid Identifier**

```php
// This usually means the table or column doesn't exist
// Check your table names match exactly (Oracle is case-sensitive)
// Remember: Oracle auto-uppercases unquoted identifiers
```

**Character Set Mismatch**

```php
// Ensure your database charset matches your connection charset
// AL32UTF8 is recommended for Unicode support
```

For more troubleshooting guidance, see the [GitHub Issues](https://github.com/yajra/laravel-oci8/issues).


<a name="see-also"></a>
## See Also

- [Installation Guide](installation) - Getting started
- [General Settings](general-settings) - Configuration options
- [Oracle Eloquent Model](oracle-eloquent) - Eloquent features for Oracle
