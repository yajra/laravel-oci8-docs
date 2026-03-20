# Oracle Functions

Stored functions allow you to execute business logic directly in the database and return values to your PHP application.

<a name="prerequisites"></a>
## Prerequisites

Before running any PHP code, create the stored function in your Oracle database using SQL*Plus, SQL Developer, or a Laravel migration.

### Create the Function (SQL)

```sql
CREATE OR REPLACE FUNCTION myfunc(p IN NUMBER) RETURN NUMBER AS
BEGIN
    RETURN p * 3;
END;
```


<a name="calling-functions-from-php"></a>
## Calling Functions from PHP

### Using the Query Builder

The simplest approach uses the Query Builder with a `SELECT` statement:

```php
$result = DB::selectOne("SELECT myfunc(2) AS value FROM dual");

return $result->value; // Returns: 6
```

### Using PDO with Bind Parameters

For better performance and security with complex values:

```php
$pdo = DB::getPdo();
$x = 2;

$stmt = $pdo->prepare("BEGIN :y := myfunc(:x); END;");
$stmt->bindParam(':y', $y, PDO::PARAM_INT);
$stmt->bindParam(':x', $x, PDO::PARAM_INT);
$stmt->execute();

return $y; // Returns: 6
```


<a name="using-shortcut-method"></a>
## Using the Shortcut Method

Laravel-OCI8 provides a convenient shortcut for calling functions:

```php
$result = DB::executeFunction(
    'function_name',          // Function name
    ['binding_1' => 'hi'],    // Input bindings
    PDO::PARAM_LOB            // Return type (optional)
);
```

### Example: Calling myfunc

```php
$result = DB::executeFunction('myfunc', ['p' => 3], PDO::PARAM_INT);

return $result; // Returns: 9
```


<a name="functions-with-multiple-parameters"></a>
## Functions with Multiple Parameters

```sql
CREATE OR REPLACE FUNCTION calculate_total(
    p_price IN NUMBER,
    p_quantity IN NUMBER
) RETURN NUMBER AS
BEGIN
    RETURN p_price * p_quantity;
END;
```

```php
$result = DB::executeFunction('calculate_total', [
    'p_price' => 19.99,
    'p_quantity' => 5,
], PDO::PARAM_INT);

return $result; // Returns: 99.95
```


<a name="functions-returning-cursors"></a>
## Functions Returning Cursors

For functions that return result sets, see the [Oracle Cursors documentation](cursor).


<a name="error-handling"></a>
## Error Handling

Wrap function calls in try-catch blocks to handle Oracle errors gracefully:

```php
use Illuminate\Support\Facades\DB;
use Exception;

try {
    $result = DB::executeFunction('myfunc', ['p' => 3]);
} catch (Exception $e) {
    Log::error('Function call failed: ' . $e->getMessage());
    throw $e;
}
```


<a name="see-also"></a>
## See Also

- [Oracle Stored Procedures](stored-procedure) - Working with procedures
- [Oracle Cursors](cursor) - Returning result sets
