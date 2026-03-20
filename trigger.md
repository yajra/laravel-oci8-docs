# Oracle Triggers

Oracle triggers are database objects that automatically execute in response to certain events on a table. Laravel-OCI8 provides a simple API for managing triggers.

<a name="accessing-trigger-manager"></a>
## Accessing the Trigger Manager

```php
$trigger = DB::getTrigger();
```


<a name="available-methods"></a>
## Available Methods

<a name="create-auto-increment-trigger"></a>
### Create an Auto-Increment Trigger

Creates a trigger that automatically assigns the next sequence value to a column on insert:

```php
$trigger->autoIncrement($table, $column, $triggerName, $sequenceName);
```

**Example:**

```php
$trigger->autoIncrement(
    'users',           // Table name
    'id',              // Column to auto-increment
    'users_id_trg',     // Trigger name
    'users_id_seq'     // Sequence name
);
```

This creates a trigger similar to:

```sql
CREATE OR REPLACE TRIGGER users_id_trg
BEFORE INSERT ON users
FOR EACH ROW
BEGIN
    SELECT users_id_seq.NEXTVAL INTO :NEW.id FROM DUAL;
END;
```


<a name="drop-trigger"></a>
### Drop a Trigger

```php
$trigger->drop($triggerName);
```

**Example:**

```php
$trigger->drop('users_id_trg');
```


<a name="complete-example"></a>
## Complete Example: Setting Up Auto-Increment

Here's a complete example of manually creating the sequence and trigger:


<a name="step-1-create-sequence"></a>
### Step 1: Create the Sequence

```php
$sequence = DB::getSequence();
$sequence->create('posts_id_seq', $start = 1);
```


<a name="step-2-create-trigger"></a>
### Step 2: Create the Trigger

```php
$trigger = DB::getTrigger();
$trigger->autoIncrement('posts', 'id', 'posts_id_trg', 'posts_id_seq');
```


<a name="step-3-use-table"></a>
### Step 3: Use the Table

Now when you insert records, the ID is automatically generated:

```php
DB::connection('oracle')->table('posts')->insert([
    'title' => 'My First Post',
    'content' => 'Post content here...',
]);

// Or with Eloquent
$post = new Post();
$post->title = 'My First Post';
$post->content = 'Post content here...';
$post->save();

// $post->id is now automatically set
```


<a name="manual-trigger-creation"></a>
## Manual Trigger Creation

For more complex trigger requirements, create them manually in a migration:

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Support\Facades\DB;

class CreatePostsTrigger extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        $sql = "
            CREATE OR REPLACE TRIGGER posts_id_trg
            BEFORE INSERT ON posts
            FOR EACH ROW
            WHEN (NEW.id IS NULL)
            BEGIN
                SELECT posts_id_seq.NEXTVAL INTO :NEW.id FROM DUAL;
            END;
        ";

        DB::connection()->getPdo()->exec($sql);
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        DB::connection()->getPdo()->exec("DROP TRIGGER posts_id_trg");
    }
}
```


<a name="trigger-events"></a>
## Trigger Events

Oracle triggers can fire at different times:

| Timing | Description |
|--------|-------------|
| `BEFORE` | Executes before the triggering event |
| `AFTER` | Executes after the triggering event |
| `INSTEAD OF` | Replaces the triggering event (for views) |


<a name="trigger-levels"></a>
## Trigger Levels

| Level | Description |
|-------|-------------|
| `FOR EACH ROW` | Row-level trigger - executes once per affected row |
| Statement-level | Executes once per statement |


<a name="see-also"></a>
## See Also

- [Oracle Sequence](sequence) - Sequence management
- [Auto-Increment Support](autoincrement) - Automatic setup with migrations
- [Oracle Eloquent Model](oracle-eloquent) - Using sequences with Eloquent
