---
title: "Oracle Eloquent Model"
description: "Extend OracleEloquent class for Oracle-specific Eloquent features."
---

# Oracle Eloquent Model

Extend Laravel-OCI8's `OracleEloquent` class to unlock Oracle-specific features in your Eloquent models.

<a name="benefits-of-oracleeloquent"></a>
## Benefits of OracleEloquent

- **Automatic BLOB handling**: Set binary fields directly as model attributes
- **Sequence management**: Built-in support for Oracle sequences
- **Oracle-specific optimizations**: Tailored for Oracle database features


<a name="basic-usage"></a>
## Basic Usage

<a name="defining-a-model"></a>
### Defining a Model

```php
<?php

namespace App\Models;

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


<a name="working-with-blob-fields"></a>
## Working with BLOB Fields

OracleEloquent simplifies BLOB handling by allowing you to set binary fields directly:

```php
use App\Models\Post;
use Illuminate\Support\Str;
use Illuminate\Http\Request;

Route::post('save-post', function (Request $request) {
    $post = new Post();
    $post->title = $request->input('title');
    $post->company_id = auth()->user()->company->id;
    $post->slug = Str::slug($request->input('title'));
    $post->content = $request->input('content'); // Set BLOB field directly
    $post->save();
    
    return redirect()->route('posts.show', $post->id);
});
```


<a name="custom-sequence-configuration"></a>
## Custom Sequence Configuration

By default, OracleEloquent looks for a sequence named `{table}_{primaryKey}_seq`. If your sequence has a different name, configure it explicitly:

```php
<?php

namespace App\Models;

use Yajra\Oci8\Eloquent\OracleEloquent as Eloquent;

class Article extends Eloquent
{
    // Use a custom sequence name
    protected $sequence = 'article_id_seq';
}
```

Now when you save a new Article:

```php
$article = new Article();
$article->title = 'My Article';
$article->save();

echo $article->id; // Auto-populated from sequence
```


<a name="retrieving-blob-data"></a>
## Retrieving BLOB Data

When fetching records, BLOB fields are automatically loaded as values:

```php
$post = Post::find(1);

echo $post->content; // Returns the actual content, not a LOB object
```


<a name="limitations"></a>
## Limitations

> **Warning**: Bulk insert operations with BLOB fields are **not yet supported**.

```php
// This will NOT work with BLOB fields:
Post::insert($postsArray);
```

For bulk operations, use individual `save()` calls instead.


<a name="complete-example"></a>
## Complete Example: Blog Post Model

```php
<?php

namespace App\Models;

use Yajra\Oci8\Eloquent\OracleEloquent as Eloquent;

class Post extends Eloquent
{
    protected $table = 'blog_posts';

    protected $fillable = [
        'title',
        'slug',
        'content',
        'company_id',
    ];

    protected $binaries = [
        'content',
    ];

    protected $sequence = 'blog_posts_id_seq';

    public function company()
    {
        return $this->belongsTo(Company::class);
    }
}
```


<a name="see-also"></a>
## See Also

- [Oracle BLOB Support](blob) - Detailed BLOB handling guide
- [Oracle Sequence](sequence) - Working with sequences
- [Auto-Increment Support](autoincrement) - Auto-incrementing primary keys
