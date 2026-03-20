---
title: "General Settings"
description: "Configuration options for Laravel-OCI8 Oracle database connections."
---

# General Settings

Configure your Oracle database connection settings for Laravel-OCI8.

<a name="configuration-file"></a>
## Configuration File

The configuration file is located at `config/oracle.php`. When you publish the vendor assets, this file is automatically created.


<a name="basic-configuration"></a>
## Basic Configuration

```php
// config/oracle.php
return [
    'oracle' => [
        'driver'         => 'oracle',
        'tns'            => env('DB_TNS', ''),
        'host'           => env('DB_HOST', ''),
        'port'           => env('DB_PORT', '1521'),
        'database'       => env('DB_DATABASE', ''),
        'username'       => env('DB_USERNAME', ''),
        'password'       => env('DB_PASSWORD', ''),
        'charset'        => env('DB_CHARSET', 'AL32UTF8'),
        'prefix'         => env('DB_PREFIX', ''),
        'prefix_schema'  => env('DB_SCHEMA_PREFIX', ''),
    ],
];
```


<a name="configuration-options"></a>
### Configuration Options

| Option | Type | Default | Description |
|--------|------|---------|-------------|
| `driver` | string | `oracle` | Database driver |
| `tns` | string | `''` | TNS connection string (if using TNS naming) |
| `host` | string | `''` | Oracle server hostname |
| `port` | string | `1521` | Oracle listener port |
| `database` | string | `''` | Database name or SID |
| `username` | string | `''` | Database username |
| `password` | string | `''` | Database password |
| `charset` | string | `AL32UTF8` | Character set |
| `prefix` | string | `''` | Table prefix |
| `prefix_schema` | string | `''` | Schema prefix |


<a name="using-service-name"></a>
## Using Service Name (Recommended)

If your Oracle database uses a SERVICE NAME alias instead of SID, use this configuration:

```php
'oracle' => [
    'driver'      => 'oracle',
    'host'        => 'oracle.host',
    'port'        => '1521',
    'database'    => 'xe',
    'service_name' => 'your_service_name_alias',
    'username'    => 'hr',
    'password'    => 'hr',
    'charset'     => 'AL32UTF8',
    'prefix'      => '',
],
```

> **Tip**: Service Name is the modern and recommended way to connect to Oracle databases. It's more flexible than SID and supports features like Transparent Application Failover (TAF).


<a name="using-tns-connection-string"></a>
## Using TNS Connection String

For environments where you need complete control over the connection string:

```php
'oracle' => [
    'driver' => 'oracle',
    'tns'    => '(DESCRIPTION=
        (ADDRESS=(PROTOCOL=TCP)(HOST=your-host)(PORT=1521))
        (CONNECT_DATA=(SERVICE_NAME=your-service-name))
    )',
],
```


<a name="environment-variables"></a>
## Environment Variables

For security, sensitive values should be stored in environment variables:

```bash
# .env
DB_CONNECTION=oracle
DB_TNS=
DB_HOST=oracle.host
DB_PORT=1521
DB_DATABASE=xe
DB_SERVICE_NAME=your_service_name
DB_USERNAME=your_username
DB_PASSWORD=your_password
DB_CHARSET=AL32UTF8
```


<a name="connection-in-database"></a>
## Connection in database.php

Register your Oracle connection in `config/database.php`:

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
        'prefix_schema'  => env('DB_SCHEMA_PREFIX', ''),
    ],
],
```


<a name="see-also"></a>
## See Also

- [Installation Guide](installation) - Getting started
- [Date Formatting](date-format) - Session date format configuration
- [Stand-Alone Usage](stand-alone) - Using outside Laravel
