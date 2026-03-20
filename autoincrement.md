---
title: "Oracle Auto-Increment Support"
description: "Configure auto-incrementing primary keys in Oracle using sequences and triggers."
---

# Oracle Auto-Increment Support

Oracle databases don't support auto-increment columns natively like MySQL or PostgreSQL. Instead, Laravel-OCI8 uses **sequences** and **triggers** to achieve the same functionality.

<a name="requirements"></a>
## Requirements

To use auto-increment in Laravel-OCI8, you must meet these requirements:

1. **Sequence**: Your table must have a corresponding sequence named using the format `{table}_{column}_seq`
2. **Trigger**: The sequence's next value must be executed before the insert query

<a name="automatic-setup"></a>
## Automatic Setup with Laravel Migrations

When using Laravel's Schema Builder, the required sequence and trigger are automatically created for you:

```php
Schema::create('posts', function ($table) {
    $table->increments('id');
    $table->string('title');
    $table->string('slug');
    $table->text('content');
    $table->timestamps();
});
```

This migration creates the following Oracle objects:

| Object Type | Name | Description |
|-------------|------|-------------|
| Table | `posts` | The main data table |
| Sequence | `posts_id_seq` | Generates sequential IDs |
| Trigger | `posts_id_trg` | Automatically sets the ID on insert |

> **Important**: Oracle object names are truncated to 30 characters. If your table/column names are long, the naming convention may not be followed exactly. We recommend limiting table and column names to 20 characters or fewer to avoid conflicts with the suffixes added by the Schema Builder (`_seq`, `_trg`, `_unique`, etc.).

<a name="custom-start-value"></a>
## Custom Start Value and No Cache

You can customize the auto-increment behavior:

```php
Schema::create('posts', function ($table) {
    // Start auto-increment at 10,000 and disable cache
    $table->increments('id')->start(10000)->nocache();
    $table->string('title');
});
```

### Available Options

| Method | Description |
|--------|-------------|
| `->start($value)` | Sets the starting value for the sequence |
| `->nocache()` | Disables sequence caching (default: caches 20 values) |

<a name="inserting-records"></a>
## Inserting Records

When inserting records with an auto-incrementing ID, use the `insertGetId` method:

```php
$id = DB::connection('oracle')->table('users')->insertGetId(
    ['email' => 'john@example.com', 'votes' => 0],
    'userid'
);
```

> **Note**: The second parameter specifies the auto-incrementing column name. If omitted, it defaults to `id`.

<a name="working-with-custom-sequences"></a>
## Working with Custom Sequences

If your table already has an existing sequence, see the [Oracle Sequence documentation](sequence) for information on using custom sequences with your models.
