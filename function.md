# Oracle Function

Before running the PHP program, create a stored function in SQL\*Plus or SQL Developer:

**SQL**

```sql
CREATE OR REPLACE FUNCTION myfunc(p IN NUMBER) RETURN NUMBER AS
BEGIN
    RETURN p * 3;
END;
```

**PHP**

```php
// via Query Builder
$result = DB::selectOne("select myfunc(2) as value from dual");
return $result->value; // prints 6

// via PDO
$pdo = DB::getPdo();
$x = 2;

$stmt = $pdo->prepare("begin :y := myfunc(:x); end;");
$stmt->bindParam(':y', $y);
$stmt->bindParam(':x', $x);
$stmt->execute();

return $y; // prints 6
```

**With shortcut Method** (https://github.com/yajra/laravel-oci8/pull/183/files)

```php
$result = DB::executeFunction('function_name', ['binding_1' => 'hi', 'binding_n' => 'bye'], PDO::PARAM_LOB)
```

**MyFunc shortcut method:**

```php
DB::executeFunction('myfunc', ['p' => 3], PDO::PARAM_INT)
```
