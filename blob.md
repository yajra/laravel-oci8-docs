# Oracle Blob
Querying a blob field will now load the value instead of the OCI-Lob object.

```php
$data = DB::table('mylobs')->get();
foreach ($data as $row) {
    echo $row->blobdata . '<br>';
}
```
## Inserting a blob via transaction

```php
DB::transaction(function($conn){
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

## Inserting Records Into A Table With Blob And Auto-Incrementing ID

```php
$id = DB::table('mylobs')->insertLob(
    array('name' => 'Insert Binary Test'),
    array('blobfield'=>'Lorem ipsum Minim occaecat in velit.')
    );
```
> **Note:** When using the insertLob method, you can specify the auto-incrementing column name as the third parameter in insertLob function. It will default to "id" if not specified.

## Updating Records With A Blob
```php
$id = DB::table('mylobs')->whereId(1)->updateLob(
    array('name'=>'demo update blob'),
    array('blobfield'=>'blob content here')
    );
```
> **Note:** When using the insertLob method, you can specify the auto-incrementing column name as the third parameter in insertLob function. It will default to "id" if not specified.

## Updating Blob directly using OracleEloquent
On your model, just add `use yajra\Oci8\Eloquent\OracleEloquent as Eloquent;` and define the fields that are blob via `protected $binaries = [];`

*Example Model:*

```php
use Yajra\Oci8\Eloquent\OracleEloquent as Eloquent;

class Post extends Eloquent {

    // define binary/blob fields
    protected $binaries = ['content'];

    // define the sequence name used for incrementing
    // default value would be {table}_{primaryKey}_seq if not set
    protected $sequence = null;

}
```

*Usage:*
```php
Route::post('save-post', function()
{
    $post = new Post;
    $post->title            = Input::get('title');
    $post->company_id       = Auth::user()->company->id;
    $post->slug             = Str::slug(Input::get('title'));
    // set binary field (content) value directly using model attribute
    $post->content          = Input::get('content');
    $post->save();
});
```

> Limitation: Saving multiple records with a blob field like `Post::insert($posts)` is not yet supported!
