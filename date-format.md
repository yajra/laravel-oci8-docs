# Date Formatting
> (Note: Oracle's `DATE` & `TIMESTAMP` format is set to `YYYY-MM-DD HH24:MI:SS` by default to match PHP's common date format)

To set oracle session date and timestamp format.

```php
DB::setDateFormat('MM/DD/YYYY');
```