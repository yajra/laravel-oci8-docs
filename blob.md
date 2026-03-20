# Oracle BLOB Support

Laravel-OCI8 provides special handling for Oracle BLOB (Binary Large Object) fields. When querying a BLOB field, the value is automatically loaded instead of the raw OCI-Lob object.

<a name="querying-blob-data"></a>
## Querying BLOB Data

```php
$data = DB::table('mylobs')->get();

foreach ($data as $row) {
    echo $row->blobdata;
}
```


<a name="inserting-blob-via-transaction"></a>
## Inserting a BLOB via Transaction

For more control over BLOB insertion, use a transaction with PDO:

```php
DB::transaction(function ($conn) {
    /** @var \Illuminate\Database\Connection $conn */
    $pdo = $conn->getPdo();
    $sql = "INSERT INTO mylobs (id, blobdata)
            VALUES (mylobs_id_seq.nextval, EMPTY_BLOB())
            RETURNING blobdata INTO :blob";
    
    $stmt = $pdo->prepare($sql);
    $stmt->bindParam(':blob', $lob, PDO::PARAM_LOB);
    $stmt->execute();
    $lob->save('blob content');
});
```


<a name="inserting-records-with-blob"></a>
## Inserting Records with BLOB and Auto-Incrementing ID

Use the `insertLob` convenience method for simpler cases:

```php
$id = DB::table('mylobs')->insertLob(
    ['name' => 'Insert Binary Test'],
    ['blobfield' => 'Lorem ipsum Minim occaecat in velit.']
);
```

> **Note**: The auto-incrementing column name defaults to `id` if not specified as the third parameter.


<a name="updating-records-with-blob"></a>
## Updating Records with BLOB

```php
$id = DB::table('mylobs')
    ->whereId(1)
    ->updateLob(
        ['name' => 'demo update blob'],
        ['blobfield' => 'blob content here']
    );
```

> **Note**: When using `updateLob`, the auto-incrementing column name defaults to `id` if not specified as the third parameter.


<a name="using-oracleeloquent"></a>
## Using OracleEloquent for Automatic BLOB Handling

For a more convenient approach, extend the `OracleEloquent` class instead of the standard Eloquent Model. This allows you to set BLOB fields directly as model attributes.

### Defining the Model

```php
<?php

use Yajra\Oci8\Eloquent\OracleEloquent as Eloquent;

class Post extends Eloquent
{
    // Define which fields are binary/BLOB types
    protected $binaries = ['content'];

    // Define the sequence name for auto-incrementing
    // Defaults to {table}_{primaryKey}_seq if not set
    protected $sequence = null;
}
```

### Saving a Record

```php
use App\Models\Post;
use Illuminate\Support\Str;

// In a controller or route
$post = new Post();
$post->title = $request->input('title');
$post->company_id = auth()->user()->company->id;
$post->slug = Str::slug($request->input('title'));
$post->content = $request->input('content'); // Set BLOB field directly
$post->save();
```


<a name="limitations"></a>
## Limitations

> **Warning**: Saving multiple records with BLOB fields using `Post::insert($posts)` is **not yet supported**. You must use `save()` on individual model instances.


<a name="see-also"></a>
## See Also

- [Oracle Eloquent Model](oracle-eloquent) - Extended Eloquent features for Oracle
- [Oracle Sequence](sequence) - Working with sequences
