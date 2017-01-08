# Oracle Sequence

Oracle Sequence can be loaded via `DB::getSequence()`.

```php
$sequence = DB::getSequence();
// create a sequence
$sequence->create('seq_name');
// drop a sequence
$sequence->drop('seq_name');
// next value
$sequence->nextValue('seq_name');
// current value
$sequence->currentValue('seq_name');
$sequence->lastInsertId('seq_name');
// check if exists
$sequence->exists('seq_name');
```

## Setting sequence start value

```php
$sequence = DB::getSequence();
// create a sequence
$sequence->create('seq_name', $start = 100);
```

## Setting sequence no cache option


```php
$sequence = DB::getSequence();
// create a sequence
$sequence->create('seq_name', $start = 1, $nocache = true);
```

## Incrementing Model using custom Sequence
Since v5.2.2, we can now use a custom sequence to automatically increment our primary key without the need of a trigger.
To use it, just simply add a `public $sequence = 'sequence_name` on your model and the package will automatically get the sequence value and assign to your primary key.

This feature will address issues where tables and sequence already exists on your project.

## Usage
```php
class User extends Model {
    public $sequence = 'user_id_seq';
}

$producer = new User();
$producer->name = 'Doe, John M.';
$producer->save();

dump($producer->id); // should be 1 if this is the first entry.
```
>NOTE: This feature is only applicable on Eloquent Models. When using Query Builder, you have to manually set the sequence next value on your incrementing column.