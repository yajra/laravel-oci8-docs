### Updating Blob directly using OracleEloquent

On your model, just add `use Yajra\Oci8\Eloquent\OracleEloquent as Eloquent`; and define the fields that are blob via `protected $binaries = [];`

Example Model:
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
Usage:

```php
Route::post('save-post', function()
{
    $post = new Post;`
    $post->title            = Input::get('title');
    $post->company_id       = Auth::user()->company->id;
    $post->slug             = Str::slug(Input::get('title'));
    // set binary field (content) value directly using model attribute
    $post->content          = Input::get('content');
    $post->save();
});
```

> Limitation: Saving multiple records with a blob field like Post::insert($posts) is not yet supported!