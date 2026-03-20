---
title: "Oracle Cursors"
description: "Working with Oracle cursors for returning result sets from stored procedures."
---

# Oracle Cursors

Cursors allow you to return result sets from stored procedures and functions. This guide shows how to work with cursors in Laravel-OCI8.

<a name="returning-cursor-from-function"></a>
## Returning a Cursor from a Function

### SQL: Create the Function

```sql
CREATE OR REPLACE FUNCTION mycursor(p1 IN NUMBER) RETURN SYS_REFCURSOR AS
    rc SYS_REFCURSOR;
BEGIN
    OPEN rc FOR SELECT city FROM locations WHERE ROWNUM < p1;
    RETURN rc;
END;
```


<a name="php-fetch-cursor-result"></a>
### PHP: Fetch the Cursor Result

**Using the Query Builder:**

```php
$result = DB::select("SELECT mycursor(5) AS mfrc FROM dual");

return $result[0]->mfrc;
```

**Using PDO directly:**

```php
$pdo = DB::getPdo();
$stmt = $pdo->prepare("SELECT mycursor(5) AS mfrc FROM dual");
$stmt->execute();
$result = $stmt->fetchAll(PDO::FETCH_OBJ);

return $result[0]->mfrc;
```


<a name="returning-cursor-from-stored-procedure"></a>
## Returning a Cursor from a Stored Procedure

The following example demonstrates calling a procedure that returns a cursor via an OUT parameter, as contributed by the community.

```php
$sql = "BEGIN
    sgc.pintegracaomodoffline.ListaCidade(:pTabResultado, :pCodRetorno, :pMsgRetorno);
END;";

return DB::transaction(function ($conn) use ($sql) {
    $pdo = $conn->getPdo();
    $stmt = $pdo->prepare($sql);

    // Bind output parameters
    $stmt->bindParam(':pTabResultado', $lista, PDO::PARAM_STMT);
    $stmt->bindParam(':pCodRetorno', $cod, PDO::PARAM_INT);
    $stmt->bindParam(':pMsgRetorno', $text, PDO::PARAM_STR, 100);

    $stmt->execute();

    // Execute the cursor
    oci_execute($lista, OCI_DEFAULT);
    oci_fetch_all($lista, $array, 0, -1, OCI_FETCHSTATEMENT_BY_ROW + OCI_ASSOC);
    oci_free_cursor($lista);

    return $array;
});
```

> **Note**: This example uses raw OCI8 functions (`oci_execute`, `oci_fetch_all`, `oci_free_cursor`) for maximum control over cursor handling. For simpler use cases, consider using the function shortcut method.


<a name="tips"></a>
## Tips

- Always execute cursors within a transaction when modifying data
- Use `OCI_DEFAULT` mode when you need to control commit behavior manually
- Remember to free cursor resources with `oci_free_cursor()` to prevent memory leaks


<a name="see-also"></a>
## See Also

- [Oracle Functions](function) - Working with stored functions
- [Oracle Stored Procedures](stored-procedure) - Working with stored procedures
