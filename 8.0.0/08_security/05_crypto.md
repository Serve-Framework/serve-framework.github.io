# Crypto

- [Access](#access)
- [Usage](#usage)
- [Passwords](#passwords)

--------------------------------------------------------

Serve's `Crypto` class is used throughout the application to encrypt and verify sensitive data.

--------------------------------------------------------

### Access

The `Crypto` component can be accessed via the IoC container.

```php
$crypto = $serve->Crypto;
```

--------------------------------------------------------

### Usage

To encrypt a string, use the `encrypt` method.

```php
$encrypted = $crypto->encrypt('foo');
```

The `decrypt` method decrypts an encrypted string. It will return `FALSE` if there was an error, or the data was tampered with.

```php
$data = $crypto->decrypt($encrypted);
```

--------------------------------------------------------

### Passwords

The `password` component can be accessed via the `password` method:
```php
$password = $crypto->password();
```

To hash a password, use `hash` method:
```php
$hashed = $password->hash('foo');
```

To verify a hashed password with a raw password, use the `verify` method. It will return `TRUE` if the password was verified and `FALSE` if not:
```php
if ($password->verify('foo', $hashed))
{
    # Valid password
}
```