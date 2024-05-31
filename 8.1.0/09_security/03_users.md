# Users

--------------------------------------------------------

- [Roles](#roles)
- [Creating Users](#creating-users)
	- [New Users](#new-users)
	- [New Admins](#new-admins)
	- [Status Codes](#status-codes)
- [User Provider](#user-provider)

--------------------------------------------------------

Users are defined by their `role` value in the database. Users can used throughout your application for both front-end and back-end management.

--------------------------------------------------------

### Roles

In Serve, users are given privileges to your application based on their role. There are two types of users - `administrator` and `guest`.

The two roles are arbitrary for the most part however they are used by the `Gatekeeper` to validate access. It's still up to you how you want to handle this in your application.

> You can define as many roles as you like.

--------------------------------------------------------

### Creating Users

Users are managed via the `UserManager`. You can access the `UserManager` class directly through the IoC container:
```php
$userManager = $serve->userManager;
```

#### New Users

The `create` method attempts to create a new user. The method will return a `User` instance on success and a status code if not:
```php
$user = $userManager->create($email, $password, $name, $username, $role, $activate);
```

> Only `$email` is required to create a user. The other parameters are optional and will be set to defaults if not provided

```php
$user = $userManager->create(string $email, string $password = '', string $name = '', string $username = '', string $role = 'guest', bool $activate = false)
```

New users are set to `pending` by default. You can override this behavior by setting $activate to true:
```php
$user = $userManager->create($email, $password, $name, $username, $role, true);
```

#### New Admins

The `createAdmin` method attempts to register a new administrator. The method will return a `User` instance on success and a status code if not:
```php
$admin = $userManager->createAdmin(string $email, string $password = '', string $name = '', string $username = '', bool $activate = false);
```

#### Status Codes

The possible status codes for failed new users are:

| Constant                       | Description                                 |
|--------------------------------|---------------------------------------------|
| `UserManager::USERNAME_EXISTS` | Another user with the same username exists. |
| `UserManager::SLUG_EXISTS`     | Another user with the same slug exists.     |
| `UserManager::EMAIL_EXISTS`    | Another user with the same email exists.    |

```php
$user = $userManager->create($email, $username, $password, 'guest');

if ($user === $userManager::USERNAME_EXISTS)
{
	// Username taken
}
else if ($user === $userManager::SLUG_EXISTS)
{
	// Slug taken
}
else if ($user === $userManager::EMAIL_EXISTS)
{
	// Email taken
}
else if ($user instanceof User)
{
	// Success
}
```

--------------------------------------------------------

### User Provider

The `provider` method returns the CMS user provider:
```php
$userProvider = $userManager->provider();
```

The `byId` method returns a single user class by id:
```php
$user = $userProvider->byId(1);
```

The `byEmail` method returns a single user by `email` if it exists or `FALSE` if not:
```php
$user = $userManager->byEmail('foo@bar.com');
```

The `byUsername` method returns a single user by `username` if it exists or `FALSE` if not:
```php
$user = $userManager->byUsername('johndoe');
```

The `byToken` method returns a single user by their CSRF `token` if it exists or `FALSE` if not:
```php
$user = $userManager->byToken('csrf-token');
```

The `byKey` method returns an array of users by key/value from the database `users` table. The third argument defines to limit the results to single row:
```php
# Get the first user with name is 'John Doe'
$users = $userProvider->byKey('name', 'John Doe' true);

# Get all the users that under name 'John Doe'
$users = $userProvider->byKey('name', 'John Doe');
```

Once you have reference to a user, you can set values user PHP's magic methods. The `save` method will save the user:
```php
$user->name  = 'John Doe';
$user->email = 'John@doe.com';
$user->save();
```

The `delete` method will delete the user:
```php
$user->delete();
```

The `generateAccessToken` method generates a new access token for the user and saves it to the database:
```php
$user->generateAccessToken();

$token = $user->access_token;
```

The `activate` method allows you to activate an existing unconfirmed user by their activation token. The method will return `TRUE` on success and `FALSE` if the activation fails:

```php
$activated = $userManager->activate($token);
```