# Oracle Auto-Increment Support
To support auto-increment in Laravel-OCI8, you must meet the following requirements:
- Table must have a corresponding sequence with this format ```{$table}_{$column}_seq```
- Sequence next value are executed before the insert query.

***********

> **Note:** If you will use [Laravel Migration](http://laravel.com/docs/migrations) feature, the required sequence and a trigger will automatically be created. Please also note that **trigger, sequence and indexes name will be truncated to 30 chars** when created via [Schema Builder](http://laravel.com/docs/schema) hence there might be cases where the naming convention would not be followed. I suggest that you limit your object name not to exceed 20 chars as the builder added some naming convention on it like _seq, _trg, _unique, etc...

***********

```php
Schema::create('posts', function($table)
{
    $table->increments('id');
    $table->string('title');
    $table->string('slug');
    $table->text('content');
    $table->timestamps();
});
```

This script will trigger Laravel-OCI8 to create the following DB objects
- posts (table)
- posts_id_seq (sequence)
- posts_id_trg (trigger)

##Auto-Increment Start With and No Cache Option
- You can now set the auto-increment starting value by setting the `start` attribute.
- If you want to disable cache, then add `nocache` attribute.

```php
Schema::create('posts', function($table)
{
    $table->increments('id')->start(10000)->nocache();
    $table->string('title');
}
```

##Inserting Records Into A Table With An Auto-Incrementing ID

```php
  $id = DB::connection('oracle')->table('users')->insertGetId(
      array('email' => 'john@example.com', 'votes' => 0), 'userid'
  );
```

> **Note:** When using the insertGetId method, you can specify the auto-incrementing column name as the second parameter in insertGetId function. It will default to "id" if not specified.
