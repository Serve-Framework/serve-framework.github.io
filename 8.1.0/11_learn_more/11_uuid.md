# UUID

--------------------------------------------------------

- [Usage](#usage)

--------------------------------------------------------

The UUID class contains methods used to validate and generate UUIDs.

--------------------------------------------------------

### Usage

The validate method checks if a string is a valid `UUID`. It will return `TRUE` if is does and `FALSE` if not:
```php
$valid = UUID::validate('f47ac10b-58cc-4372-a567-0e02b2c3d479'); // "true"

$valid = UUID::validate('x47ac10b-58cc-4372-a567-0e02b2c3d479'); // "false"
```

The `v3` method will generate and return a version 3 UUID:
```php
// The namespace must be a valid UUID
$uuid = UUID::v3('f47ac10b-58cc-4372-a567-0e02b2c3d479', 'foobar');

// You can also use one of the predefined namespaces (DNS, URL, OID and X500)
$uuid = UUID::v3(UUID::OID, 'foobar');
```

The `v4` method will return a version 4 UUID:
```php
$uuid = UUID::v4();
```

The `v5` method will return a version 5 UUID:
```php
// The namespace must be a valid UUID

$uuid = UUID::v5('f47ac10b-58cc-4372-a567-0e02b2c3d479', 'foobar');

// You can also use one of the predefined namespaces (DNS, URL, OID and X500)

$uuid = UUID::v5(UUID::OID, 'foobar');

```
