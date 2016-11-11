# Oracle Cursor
Example code on how to return a cursor using a procedure.
> Note: Example code below will only work on v2.3++

**SQL**
```sql
-- Create the PL/SQL stored function as:
CREATE OR REPLACE FUNCTION mycursor(p1 IN NUMBER) RETURN SYS_REFCURSOR AS
  rc SYS_REFCURSOR;
BEGIN
  OPEN rc FOR SELECT city FROM locations WHERE ROWNUM < p1;
  RETURN rc;
END;
```

**PHP**
```php
// via Query Builder
return DB::select("SELECT mycursor(5) AS mfrc FROM dual");

// via PDO
$pdo = DB::getPdo();
$stmt = $pdo->prepare("SELECT mycursor(5) AS mfrc FROM dual");
$result = $stmt->execute();
return $stmt->fetchAll(PDO::FETCH_OBJ);
```

-----

Oracle stored procedure implementation returning a cursor  as submitted by @TiagoGoddard on issue [#31](https://github.com/yajra/laravel-oci8/issues/31)

```php
$sql = "begin
    sgc.pintegracaomodoffline.ListaCidade(:pTabResultado, :pCodRetorno, :pMsgRetorno);
  end;"

return DB::transaction(function($conn) use ($sql){
    $pdo = $conn->getPdo();
    $stmt = $pdo->prepare($sql);

    $stmt->bindParam(':pTabResultado', $lista, PDO::PARAM_STMT);
    $stmt->bindParam(':pCodRetorno', $cod, PDO::PARAM_INT);

    $stmt->bindParam(':pMsgRetorno', $text, PDO::PARAM_STR, 100);
    $stmt->execute();

    oci_execute($lista, OCI_DEFAULT);
    oci_fetch_all($lista, $array, 0, -1, OCI_FETCHSTATEMENT_BY_ROW + OCI_ASSOC );
    oci_free_cursor($lista);

    return $array;
});
```