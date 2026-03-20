# Oracle Sequence

Oracle sequences are database objects that generate sequential numbers. Laravel-OCI8 provides a convenient API for managing sequences.

<a name="accessing-sequence-manager"></a>
## Accessing the Sequence Manager

```php
$sequence = DB::getSequence();
```


<a name="available-methods"></a>
## Available Methods

<a name="create-sequence"></a>
### Create a Sequence

```php
$sequence->create('seq_name');
```


<a name="drop-sequence"></a>
### Drop a Sequence

```php
$sequence->drop('seq_name');
```


<a name="get-next-value"></a>
### Get Next Value

```php
$nextValue = $sequence->nextValue('seq_name');
```


<a name="get-current-value"></a>
### Get Current Value

```php
$currentValue = $sequence->currentValue('seq_name');
```


<a name="get-last-insert-id"></a>
### Get Last Insert ID

```php
$lastId = $sequence->lastInsertId('seq_name');
```


<a name="check-if-sequence-exists"></a>
### Check if Sequence Exists

```php
if ($sequence->exists('seq_name')) {
    // Sequence exists
}
```


<a name="setting-sequence-start-value"></a>
## Setting Sequence Start Value

When creating a sequence, you can specify the starting value:

```php
$sequence->create('seq_name', start: 100);
```

This creates a sequence that begins at 100.

## Creating a Sequence with No Cache

By default, Oracle caches sequence values for performance. To disable caching:

```php
$sequence->create('seq_name', nocache: true);
```

This creates a sequence that begins at 100.


<a name="creating-sequence-with-no-cache"></a>
## Creating a Sequence with No Cache

By default, Oracle caches sequence values for performance. To disable caching:

```php
$sequence->create('seq_name', $start = 1, $nocache = true);
```


<a name="when-to-disable-caching"></a>
### When to Disable Caching

Disable caching when:

- You need guaranteed sequential numbers without gaps
- You're using sequences with external systems
- Audit requirements demand no skipped values


<a name="using-custom-sequences"></a>
## Using Custom Sequences with Eloquent Models

Since version 5.2.2, you can automatically use custom sequences with Eloquent models. This is useful when tables and sequences already exist in your database.


<a name="step-1-define-sequence"></a>
### Step 1: Define the Sequence on Your Model

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class User extends Model
{
    public $sequence = 'user_id_seq';
}
```


<a name="step-2-save-model"></a>
### Step 2: Save the Model

```php
$user = new User();
$user->name = 'John Doe';
$user->save();

echo $user->id; // Returns the next sequence value (e.g., 1)
```

When you save the model, Laravel-OCI8 automatically:

1. Fetches the next value from the sequence
2. Assigns it to the primary key
3. Inserts the record

> **Note**: This feature only works with Eloquent models. When using the Query Builder, you must manually set the sequence next value.


<a name="manual-sequence-usage"></a>
## Manual Sequence Usage with Query Builder

```php
// Get the next sequence value
$nextId = DB::getSequence()->nextValue('user_id_seq');

// Use it in an insert
DB::table('users')->insert([
    'id' => $nextId,
    'name' => 'John Doe',
]);
```


<a name="complete-example-user-registration"></a>
## Complete Example: Creating a User Registration System

```php
use Illuminate\Support\Facades\DB;

function createUser(array $data): int
{
    $sequence = DB::getSequence();

    // Create sequence if it doesn't exist
    if (!$sequence->exists('users_id_seq')) {
        $sequence->create('users_id_seq', start: 1);
    }

    // Get next ID
    $id = $sequence->nextValue('users_id_seq');

    // Insert user
    DB::table('users')->insert([
        'id' => $id,
        'email' => $data['email'],
        'password' => bcrypt($data['password']),
        'created_at' => now(),
    ]);

    return $id;
}
```


<a name="see-also"></a>
## See Also

- [Auto-Increment Support](autoincrement) - Automatic sequence creation with migrations
- [Oracle Trigger](trigger) - Working with triggers
- [Oracle Eloquent Model](oracle-eloquent) - Using sequences with Eloquent
