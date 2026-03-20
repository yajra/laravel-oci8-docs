# Oracle Stored Procedures

Stored procedures allow you to execute business logic directly in the Oracle database. This guide covers creating and calling stored procedures in Laravel-OCI8.

<a name="prerequisites"></a>
## Prerequisites

Before calling a procedure from PHP, create it in your Oracle database using SQL*Plus, SQL Developer, or a Laravel migration.


<a name="creating-the-procedure"></a>
## Creating the Procedure

<a name="using-plain-sql"></a>
### Using Plain SQL

```sql
CREATE OR REPLACE PROCEDURE myproc(p1 IN NUMBER, p2 OUT NUMBER) AS
BEGIN
    p2 := p1 * 2;
END;
```


<a name="using-laravel-migration"></a>
### Using Laravel Migration

```php
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Support\Facades\DB;

class CreateMyprocProcedure extends Migration
{
    /**
     * Run the migrations.
     */
    public function up(): void
    {
        $command = "
            CREATE OR REPLACE PROCEDURE myproc(p1 IN NUMBER, p2 OUT NUMBER) AS
            BEGIN
                p2 := p1 * 2;
            END;
        ";

        DB::connection()->getPdo()->exec($command);
    }

    /**
     * Reverse the migrations.
     */
    public function down(): void
    {
        DB::connection()->getPdo()->exec("DROP PROCEDURE myproc");
    }
}
```


<a name="calling-procedures-from-php"></a>
## Calling Procedures from PHP

### Manual Method (PDO)

For maximum control over parameter binding:

```php
$pdo = DB::getPdo();
$p1 = 8;

$stmt = $pdo->prepare("BEGIN myproc(:p1, :p2); END;");
$stmt->bindParam(':p1', $p1, PDO::PARAM_INT);
$stmt->bindParam(':p2', $p2, PDO::PARAM_INT);
$stmt->execute();

return $p2; // Returns: 16
```

### Using the Shortcut Method

Laravel-OCI8 provides a convenient `executeProcedure` method:

```php
$procedureName = 'youpackagename.yourprocedurename';

$bindings = [
    'user_id'  => $id,
];

$result = DB::executeProcedure($procedureName, $bindings);

dd($result);
```


<a name="parameter-types"></a>
## Parameter Types


<a name="input-parameters-in"></a>
### Input Parameters (IN)

```sql
CREATE OR REPLACE PROCEDURE get_user(
    p_user_id IN NUMBER,
    p_result OUT SYS_REFCURSOR
) AS
BEGIN
    OPEN p_result FOR
        SELECT * FROM users WHERE id = p_user_id;
END;
```

```php
$result = DB::executeProcedure('get_user', [
    'p_user_id' => 123,
]);
```


<a name="output-parameters-out"></a>
### Output Parameters (OUT)

```php
$procedureName = 'calculate_total';

$bindings = [
    'p_price'    => 19.99,
    'p_quantity'  => 5,
    'p_total'     => null, // OUT parameter - pass null
];

$result = DB::executeProcedure($procedureName, $bindings);

// The total is now in $bindings['p_total']
```


<a name="in-out-parameters"></a>
### IN OUT Parameters

```sql
CREATE OR REPLACE PROCEDURE double_value(p_value IN OUT NUMBER) AS
BEGIN
    p_value := p_value * 2;
END;
```

```php
$pdo = DB::getPdo();
$value = 10;

$stmt = $pdo->prepare("BEGIN double_value(:p_value); END;");
$stmt->bindParam(':p_value', $value, PDO::PARAM_INT | PDO::INPUT_OUTPUT);
$stmt->execute();

echo $value; // Returns: 20
```


<a name="error-handling"></a>
## Error Handling

Always wrap procedure calls in try-catch blocks:

```php
use Illuminate\Support\Facades\DB;
use Exception;

try {
    $result = DB::executeProcedure('your_procedure', $bindings);
} catch (Exception $e) {
    Log::error('Procedure execution failed: ' . $e->getMessage());
    throw $e;
}
```


<a name="working-with-transactions"></a>
## Working with Transactions

Procedures that modify data should be wrapped in transactions:

```php
DB::transaction(function () {
    DB::executeProcedure('process_order', [
        'order_id' => $orderId,
    ]);
});
```


<a name="see-also"></a>
## See Also

- [Oracle Functions](function) - Working with functions
- [Oracle Cursors](cursor) - Returning result sets from procedures
- [Oracle Sequence](sequence) - Sequence management
