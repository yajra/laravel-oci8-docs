#Oracle Trigger
Oracle Trigger can be loaded via `DB::getTrigger()`.

```php
$trigger = DB::getTrigger();
// create an auto-increment trigger
$trigger->autoIncrement($table, $column, $triggerName, $sequenceName);
// drop a trigger
$trigger->drop($triggerName);
```
