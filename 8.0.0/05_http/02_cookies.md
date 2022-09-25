# Cookies

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Serve's cookie manager is used throughout the application to manage cookie data.

--------------------------------------------------------

### Access

The cookie manager can be accessed via the `Response` object
```php
$session = $serve->Response->cookie();
```

--------------------------------------------------------

### Usage

To set a cookie, use the `set` method:
```php
$cookies->set('foo', 'bar');
```

To set multiple cookies at once, use the `setMultiple` method with an array:
```php
$cookies->setMultiple([
    'foo' => 'bar',
    'bar' => 'foo'
]);
```

The `get` method returns a cookie value by key or `FALSE` if it doesn't exist:
```php
$foo = $cookies->get('foo');
```

The `has` method checks if a cookie exists:
```php
if ($cookies->has('foo'))
{
    # Do something here
}
```

The `remove` method removes a cookie by key:
```php
$cookies->remove('foo');
```

The `clear` method removes all the cookies:
```php
$cookies->clear();
```

The `destroy` method destroys the cookie completely:
```php
$cookies->destroy();
```

The `login` method sets the login cookie to `TRUE`:
```php
$cookies->login();
```

The `logout` method sets the login cookie to `FALSE`:
```php
$cookies->logout();
```

You can also iterate the response cookies as an array:
```php
foreach ($cookies as $k => $v)
{
    # Do something here
}
```

The `asArray` method returns an associative array of all the cookies:
```php
$cookieArray = $cookies->asArray();
```

The `sent` method checks if the cookies have been sent.
```php
if ($cookies->sent())
{

}
```

The `send` method immediately sends the cookies. Note that this is handled internally by the framework however.
```php
$cookies->send();
```

> Cookies are automatically signed and encrypted/decrypted using the Crypto service.
