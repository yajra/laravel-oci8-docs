# Security

Security is important. If you discover a security vulnerability within Laravel-OCI8, please report it responsibly.

<a name="reporting-security-issues"></a>
## Reporting Security Issues

**Please do not report security vulnerabilities through public GitHub issues.**

Instead, send an email directly to the maintainer:

- **Email**: [aqangeles@gmail.com](mailto:aqangeles@gmail.com)


<a name="what-to-include"></a>
## What to Include

When reporting a security issue, please include:

1. **Description**: A clear description of the vulnerability
2. **Steps to Reproduce**: How to reproduce the issue
3. **Impact**: Potential impact of the vulnerability
4. **Suggested Fix**: If you have one, your suggested solution


<a name="response-timeline"></a>
## Response Timeline

We aim to acknowledge security reports within 48 hours and provide a timeline for fixes based on severity.


<a name="security-best-practices"></a>
## Security Best Practices

When using Laravel-OCI8, follow these security practices:


<a name="use-environment-variables"></a>
### Use Environment Variables

Never hardcode database credentials. Always use environment variables:

```php
// config/database.php
'connections' => [
    'oracle' => [
        'username' => env('DB_USERNAME'),
        'password' => env('DB_PASSWORD'),
    ],
],
```


<a name="limit-database-privileges"></a>
### Limit Database Privileges

Grant only the minimum privileges needed by your application:

```sql
-- Create a user with limited privileges
CREATE USER app_user IDENTIFIED BY "strong_password";
GRANT CONNECT, RESOURCE TO app_user;
```


<a name="protect-sensitive-data"></a>
### Protect Sensitive Data

Use Laravel's encryption features for sensitive data:

```php
// Encrypt before storing
$user->setAttribute('ssn', encrypt($request->input('ssn')));

// Decrypt when retrieving
$ssn = decrypt($user->getAttribute('ssn'));
```


<a name="sanitize-input"></a>
### Sanitize Input

Always use Laravel's query builder or Eloquent for database operations to benefit from built-in SQL injection protection:

```php
// Safe - uses parameterized queries
DB::table('users')->where('email', $email)->first();

// Avoid raw queries when possible
// DB::select("SELECT * FROM users WHERE email = '$email'");
```


<a name="dependency-security"></a>
## Dependency Security

Keep your dependencies up to date to receive security patches:

```bash
composer update --prefer-stable
```


<a name="license"></a>
## License

See the [License](license) file for terms and conditions.
