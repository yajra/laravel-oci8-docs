# General Settings

You can change the database connection by updating the configuration file.

The configuration file can be found at `config/oracle.php`.

```php
// config/oracle.php
return [
    'oracle' => [
        'driver'        => 'oracle',
        'tns'           => env('DB_TNS', ''),
        'host'          => env('DB_HOST', ''),
        'port'          => env('DB_PORT', '1521'),
        'database'      => env('DB_DATABASE', ''),
        'username'      => env('DB_USERNAME', ''),
        'password'      => env('DB_PASSWORD', ''),
        'charset'       => env('DB_CHARSET', 'AL32UTF8'),
        'prefix'        => env('DB_PREFIX', ''),
        'prefix_schema' => env('DB_SCHEMA_PREFIX', ''),
    ],
];
```

> {tip} If your database uses SERVICE NAME alias, use the config below:

```php
'oracle' => [
    'driver' => 'oracle',
    'host' => 'oracle.host',
    'port' => '1521',
    'database' => 'xe',
    'service_name' => 'sid_alias',
    'username' => 'hr',
    'password' => 'hr',
    'charset' => '',
    'prefix' => '',
]
```

> {tip} If you want to use your own TNS string:

```php
'oracle' => [
    'driver' => 'oracle',
    'tns'    => 'your-tns-connection-string-here',
]
```