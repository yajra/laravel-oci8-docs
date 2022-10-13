# Installation

- [Installation](#installation)
    - [Server Requirements](#server-requirements)
    - [Installing Laravel-OCI8](#installing-laravel-oci8)
    - [Configuration](#configuration)

<a name="installation"></a>
## Installation

<a name="server-requirements"></a>
### Server Requirements

<div class="content-list" markdown="1">
- PHP >= 5.6.4
- OCI8 PHP Extension
</div>


<a name="installing-laravel-oci8"></a>
### Installing Laravel OCI8

Laravel OCI8 can be installed with [Composer](http://getcomposer.org/doc/00-intro.md). More details about this package in Composer can be found [here](https://packagist.org/packages/yajra/laravel-oci8).

Run the following command in your project to get the latest version of the package:

```
composer require yajra/laravel-oci8:^8.0
```

<a name="configuration"></a>
### Configuration

You can use Laravel OCI8 with **Laravel** as well as **Lumen**.

### With Laravel
Open the file ```config/app.php``` and then add following service provider.

```php
'providers' => [
    // ...
    Yajra\Oci8\Oci8ServiceProvider::class,
],
```
> Note: This step is (optional) for the publication of configuration files.

After completing the step above, use the following command to publish configuration settings:

```
php artisan vendor:publish --tag=oracle
```

### With Lumen
Open the file ```bootstrap/app.php``` and then add following service provider. This step is required.

```php
$app->register(Yajra\Oci8\Oci8ServiceProvider::class);
```

You should also uncomment the $app->withFacades() and $app->withEloquent() call in your `bootstrap/app.php` file.

```php
// ...

$app->withFacades();

$app->withEloquent();

// ...
```
