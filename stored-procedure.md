# Oracle Stored Procedure
Before running the PHP program, create a stored procedure in SQL*Plus, SQL Developer or by migration:

##Creating the Procedure with Plain SQL
```sql
CREATE OR REPLACE PROCEDURE myproc(p1 IN NUMBER, p2 OUT NUMBER) AS
BEGIN
    p2 := p1 * 2;
END;
```

##Creating the Procedure with migrations
```
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
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
     *
     * @return void
     */
    public function down()
    {
        $command = "DROP PROCEDURE myproc";
        DB::connection()->getPdo()->exec($command);
    }
```

##Running procedure the manual way
```php
$pdo = DB::getPdo();
$p1 = 8;

$stmt = $pdo->prepare("begin myproc(:p1, :p2); end;");
$stmt->bindParam(':p1', $p1, PDO::PARAM_INT);
$stmt->bindParam(':p2', $p2, PDO::PARAM_INT);
$stmt->execute();

return $p2; // prints 16
```

##Running procedure with shortcut method
```php
$procedureName = 'youpackagename.yourprocedurename';

$bindings = [
    'user_id'  => $id,
];

$result = DB::executeProcedure($procedureName, $bindings);

dd($result);
```