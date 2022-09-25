# Session

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)
- [Token](#token)
- [Flash](#flash)

--------------------------------------------------------

Serve's session manager is used throughout the application to manage session data.

--------------------------------------------------------

### Access

The session manager can be accessed via the `Response` object
```php
$session = $serve->Response->session();
```

--------------------------------------------------------

### Usage

To set a session value, use the `set` method:
```php
$session->set('foo', 'bar');
```

To set multiple session values at once, use the `setMultiple` method with an array:
```php
$session->setMultiple([
    'foo' => 'bar',
    'bar' => 'foo'
]);
```

The `get` method returns a session value by key or `FALSE` if it doesn't exist:
```php
$foo = $session->get('foo');
```

The `has` method checks if a session value exists by key:
```php
if ($session->has('foo'))
{
    # Do something here
}
```

The remove method removes a session value by key:
```php
$session->remove('foo');
```

The `clear` method removes all the session key/pairs:
```php
$session->clear();
```

The `destroy` method destroys the session completely:
```php
$session->destroy();
```

You can also iterate the session values as an array.
```php
foreach ($session as $k => $v)
{
    # Do something here
}
```

The `asArray` method returns an associative array of all the session data:
```php
$data = $session->asArray();
```
--------------------------------------------------------

### Token

The token method returns the token object used to prevent CSRF:
```php
$token = $session->token();
```

The `get` method returns the token string:
```php
$_token = $token->get();
```

The `set` method sets the token string:
```php
$token->set('foo');
```

The `regenerate` method resets the token to a secure random value:
```php
$token->regenerate();
```

The `verify` method verifies a provided string with the client's token:
```php
if (!$token->verify('my-token'))
{
    $response->invalidToken();
}
```

The `save` method immediately saves and sends the session cookie data. Note that this is handled internally by the framework however.
```php
$session->save();
```

> Serve will automatically generate a new token upon a successful login and logout when using the Gatekeeper authentication library.

--------------------------------------------------------

### Flash

Sometimes you'll want to store temporary data that should expire after the next request (e.g., error and status messages). You can handle flash data via the `Flash` object:

```php
$flash = $session->flash();
```

The `get` method returns flash value by key if it exists or `FALSE` if not:
```php
$foo = $flash->get('foo');
```

Or an associative array of key/values if no key is provided:
```php
$data = $flash->get();
```

The `set` method sets a value by key:
```php
$flash->set('foo', 'bar');
```

To set multiple session values at once, use the `setMultiple` method with an array:
```php
$flash->setMultiple([
    'foo' => 'bar',
    'bar' => 'foo'
]);
```

The `has` method checks if a flash value exists by key:
```php
if ($flash->has('foo'))
{
    # Do something here
}
```

The `remove` method removes a flash value by key:
```php
$flash->remove('foo');
```

The `clear` method removes all the flash key/pairs:
```php
$flash->clear();
```

You can also iterate the session values as an array:
```php
foreach ($flash as $k => $v)
{
    # Do something here
}
```