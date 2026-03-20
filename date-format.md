---
title: "Date Formatting"
description: "How Laravel-OCI8 handles Oracle date and timestamp format conversion."
---

# Date Formatting

Oracle stores dates and timestamps differently than PHP. Laravel-OCI8 handles the conversion automatically by setting the default session format to `YYYY-MM-DD HH24:MI:SS` to match PHP's common date format.

<a name="setting-custom-date-format"></a>
## Setting a Custom Date Format

If you need a different date format for your Oracle session, use the `setDateFormat` method:

```php
DB::setDateFormat('MM/DD/YYYY');
```

This changes how Oracle returns date values to PHP. The format you choose should match how your application processes dates.


<a name="common-date-format-models"></a>
## Common Oracle Date Format Models

| Format String | Example Output | Use Case |
|--------------|----------------|----------|
| `YYYY-MM-DD HH24:MI:SS` | `2024-01-15 14:30:00` | ISO format, recommended default |
| `MM/DD/YYYY` | `01/15/2024` | US date format |
| `DD/MM/YYYY` | `15/01/2024` | European date format |
| `DD-MON-YYYY` | `15-JAN-2024` | Oracle classic format |


<a name="persisting-format-setting"></a>
## Persisting the Format Setting

The date format is set per connection. To ensure consistent behavior, consider setting it in a service provider or middleware:

```php
// In AppServiceProvider::boot()
use Illuminate\Support\Facades\DB;

DB::connection('oracle')->setDateFormat('YYYY-MM-DD HH24:MI:SS');
```


<a name="timestamp-formats"></a>
## Timestamp Formats

For timestamp fields, you can also set the timestamp format separately:

```php
DB::setDateFormat('YYYY-MM-DD HH24:MI:SS.FF3'); // With milliseconds
```


<a name="important-notes"></a>
## Important Notes

- The date format is connection-specific and resets when the connection is re-established
- If you're using connection pooling, the format may need to be set on each connection
- Some Oracle features (like `TO_CHAR` or `TO_DATE` functions) may be affected by the session format


<a name="see-also"></a>
## See Also

- [Oracle Eloquent Model](oracle-eloquent) - Date handling in models
- [General Settings](general-settings) - Connection configuration
