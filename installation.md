# Installation

This guide walks you through installing and configuring Laravel-OCI8 in your Laravel or Lumen application.

<a name="server-requirements"></a>
## Server Requirements

Before you begin, ensure your environment meets these requirements:

- **PHP**: 8.3 or higher
- **OCI8 Extension**: The PHP OCI8 extension must be installed and enabled
- **Oracle Client Libraries**: Oracle Instant Client (or equivalent) must be available on your system


<a name="verifying-oci8-extension"></a>
### Verifying OCI8 Extension

You can verify the OCI8 extension is installed by running:

```bash
php -m | grep oci8
```

If you don't see `oci8` in the output, you'll need to [install the OCI8 extension](https://www.php.net/manual/en/oci8.installation.php) with your Oracle client libraries.


<a name="installing-laravel-oci8"></a>
## Installing Laravel-OCI8

Laravel-OCI8 is distributed as a Composer package. Run the following command in your project root to install the latest stable version:

```bash
composer require yajra/laravel-oci8:"^13.0"
```

> **Note**: The `^13.0` constraint ensures you receive version 13.x of the package while remaining compatible with future minor versions. You can view more details on [Packagist](https://packagist.org/packages/yajra/laravel-oci8).


<a name="configuration"></a>
## Configuration

Laravel-OCI8 supports both Laravel and Lumen frameworks. Choose the setup that matches your application.


<a name="laravel"></a>
### Laravel

Laravel-OCI8 supports auto-discovery in Laravel 11+ and will automatically register the service provider. For Laravel 10 and below, add the service provider manually:

1. **Register the Service Provider (Laravel 10 and below)**

   Open `config/app.php` and add the service provider to the `providers` array:

   ```php
   'providers' => [
       // ...
       Yajra\Oci8\Oci8ServiceProvider::class,
   ],
   ```

   For Laravel 11+, the service provider is automatically discovered. If you need to register it manually, add it to `bootstrap/providers.php`:

   ```php
   Application::configure(basePath: dirname(__DIR__))
       ->withProviders([
           // ...
           Yajra\Oci8\Oci8ServiceProvider::class,
       ])
       ->withExceptions();
   ```

2. **Publish Configuration (Optional)**

   After registering the provider, you can publish the configuration file to customize settings:

   ```bash
   php artisan vendor:publish --tag=oracle
   ```

   This creates a configuration file at `config/oracle.php` where you can configure your Oracle connection details.


<a name="lumen"></a>
### Lumen

1. **Register the Service Provider**

   Open `bootstrap/app.php` and register the service provider:

   ```php
   $app->register(Yajra\Oci8\Oci8ServiceProvider::class);
   ```

2. **Enable Facades and Eloquent**

   Ensure the following calls are uncommented in your `bootstrap/app.php` file:

   ```php
   $app->withFacades();
   $app->withEloquent();
   ```

3. **Register Oracle Connection**

   Add your Oracle database configuration to the `database.connections` array in `config/database.php`:

   ```php
   'connections' => [
       'oracle' => [
           'driver' => 'oracle',
           'host' => env('DB_HOST', ''),
           'port' => env('DB_PORT', '1521'),
           'database' => env('DB_DATABASE', ''),
           'username' => env('DB_USERNAME', ''),
           'password' => env('DB_PASSWORD', ''),
           'charset' => env('DB_CHARSET', 'AL32UTF8'),
           'prefix' => env('DB_PREFIX', ''),
       ],
   ],
   ```


<a name="next-steps"></a>
## Next Steps

- [Configure General Settings](general-settings) - Set up your Oracle connection
- [Auto-Increment Support](autoincrement) - Configure auto-incrementing IDs
- [Stored Procedures](stored-procedure) - Work with Oracle stored procedures
- [Stand-Alone Usage](stand-alone) - Use outside of Laravel/Lumen
