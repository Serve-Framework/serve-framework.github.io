# Gatekeeper

--------------------------------------------------------

-   [Access](#access)
-   [Usage](#usage)
-   [Schema](#schema)

--------------------------------------------------------

Serve provides a number of utilities and classes for authentication. This section provides documentation on Serve's `Gatekeeper` class which is Serve's core implementation for user authentication.

--------------------------------------------------------

### Access

Serve's Gatekeeper class acts as Serve's core implementation for all user authentication. You can access the Gatekeeper class directly through the IoC container.

```php
$gatekeeper = $serve->Gatekeeper;
```

--------------------------------------------------------

### Usage

The Gatekeeper provides the following rudimentary validation methods.

The `getUser` method returns a `User` object if they are logged in. The object is a wrapper around the user table in the database:
```php
$user = $gatekeeper->getUser();
```

To validate if a user is logged in use the `isLoggedIn` method:
```php
if ($gatekeeper->isLoggedIn())
{
    # Do something
}
```

Similarly you can validate if a user is not allowed to access the admin panel in with the `isGuest` method:
```php
if ($gatekeeper->isGuest())
{
    # Do something
}
```

Or use the `isAdmin` method to check if the user is an `administrator` or `writer`:
```php
if ($gatekeeper->isAdmin())
{
    # Do something
}
```

The `verifyToken` method validates a user's access token. The token is stored as a cookie. If the user is logged in, the token is also cross referenced with the database:
```php
if ($gatekeeper->verifyToken($token))
{
    # Do something
}
```

The `refreshUser` method refreshed the gatekeeper's current user data. This is useful if you change a user's data:
```php
$gatekeeper->refreshUser();
```

The `login` method attempts to log a user in based on their username and password. The method will return `TRUE` on success and a status code if not.

```php
$success = $gatekeeper->login($username, $password);
```

The possible status codes for failed logins:

| Constant                       | Description                              |
|--------------------------------|------------------------------------------|
| `Gatekeeper::LOGIN_INCORRECT`  | The username or password was incorrect.  |
| `Gatekeeper::LOGIN_ACTIVATING` | The account is not yet activated.        |
| `Gatekeeper::LOGIN_LOCKED`     | The account is currently locked.         |
| `Gatekeeper::LOGIN_BANNED`     | The account has been permanently banned. |


```php
$login = $gatekeeper->login($username, $password);

if ($login === Gatekeeper::LOGIN_INCORRECT)
{
    # Your password or username was incorrect.
}
else if ($login === Gatekeeper::LOGIN_ACTIVATING)
{
    # Your account is not yet activated
}
else if ($login === Gatekeeper::LOGIN_LOCKED)
{
    # Your account has been locked
}
else if ($login === Gatekeeper::LOGIN_BANNED)
{
    # Your account has been banned
}
else if ($login === true)
{
    # Success you logged in
}
```

You can pass a third parameter as `TRUE` to force the login, regardless of the password:
```php
$gatekeeper->login($username, $password, true);
```

The `logout` method will attempt to log the current user out:
```php
$gatekeeper->logout();
```

The `forgotPassword` method will generate and return `password_key` for a given user by `email` or `username`. The password key is saved to user in the database for validation.

```php
$key = $gatekeeper->forgotPassword($usernameOrEmail);

// Send email
```

The `resetPassword` method allows you to to reset a user's password based on their `password_key`. The user's reset key must be provided. The method will return `TRUE` on success and `FALSE` if the reset fails.

```php
$reset = $gatekeeper->resetPassword($password, $key);
```

The `forgotUsername` method returns a user's username by email:

```php
$username = $gatekeeper->forgotUsername($email);
```

--------------------------------------------------------

### Schema

Serve's Gatekeeper service requires the following table schema:

```php
CREATE TABLE `users` (
    `id` int(11) unsigned NOT NULL AUTO_INCREMENT,
    `username` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    `email` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    `hashed_pass` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    `name` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    `slug` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    `role` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    `status` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    `access_token` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    `register_key` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    `password_key` varchar(255) COLLATE utf8_unicode_ci NOT NULL,
    PRIMARY KEY (`id`),
    UNIQUE KEY `username` (`username`),
    UNIQUE KEY `email` (`email`),
    UNIQUE KEY `slug` (`slug`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
```
