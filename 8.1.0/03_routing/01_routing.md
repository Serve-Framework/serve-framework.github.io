# Routing

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)
- [Regex Routes](#regex-routes)
- [Route Callbacks](#route-callbacks)
- [Route Parameters](#route-parameters)

--------------------------------------------------------

Serve's Router service lets you map URL patterns to class methods and closures. It also supports a set of regex patterns and wildcard matching making it super easy to match any kind of request.

--------------------------------------------------------

### Access

You can access the router directly through the IoC container:

```php
$router = $serve->Router;
```

--------------------------------------------------------

### Usage

To define a route on `GET` requests, use the `get` method:
```php
$router->get('/welcome', function()
{
    echo 'Hello, world!';
});
```

If you want the route to respond to `POST` requests instead then use the `post` method.
```php
$router->post('/welcome', function()
{
    echo 'Hello, world!';
});
```

The available methods are `get`, `post`, `put`, `delete`, `options`, and `head`.

> Note that using the `get` method adds a `head` route which will automatically exclude the response body. 

You can define multiple callbacks for the same route. They will be executed in the order they were defined.

The router will NOT halt when a match is found, subsequent routes will be called in the order they were defined to the router.

> If an incoming route is matched but the request method is not valid. The router will throw a `MethodNotAllowedException` exception. See [Throwing Exceptions](/8.1.0/01_getting_started/error_handling#throwing-exceptions) for more details.

--------------------------------------------------------

### Regex Routes

The Serve router comes with a handy set of predefined regex patterns to help match wildcards and generic route patterns.

The following route will forward all `GET` requests with a pattern matching regex `/([^/]+)/(\d{4})/` to the callback:

```php
$router->get('/(:any)/(:year)/', function()
{
    echo date('Y');
});
```
The full list of available regex patterns is as follows:
```php
':any'      => '[^/]+',
':num'      => '[0-9]+',
':all'      => '.*',
':year'     => '\d{4}',
':month'    => '0?[1-9]|1[012]',
':day'      => '0[1-9]|[12]\d|3[01]',
':hour'     => '0?[1-9]|1[012]',
':minute'   => '[0-5]?\d',
':second'   => '[0-5]?\d',
':postname' => '[a-z0-9 -]+',
':category' => '[a-z0-9 -]+',
':author'   => '[a-z0-9 -]+',
```

--------------------------------------------------------

### Route Callbacks

You can register a closure (callback) with the router a number of different ways. The most common of which is an anonymous function:
```php
$serve->get('/', function()
{
    # Handle route
});
```

While anonymous functions are handy, they clog the router with unnecessary parameters, resulting in poorer performance.

A more efficient way of providing a callback is via a static class method as a string:

This will call the static `myMethod` on the `app\controllers\Welcome` class.
```php
use app\controllers\Welcome;

$serve->get('/', Welcome::class . '::myMethod');
```

Or if you don't want to use a static method, you can tell the router to call a non-static method. The router will instantiate the class and call the method:
```php
$serve->get('/', Welcome::class . '@myMethod');
```

--------------------------------------------------------

### Route parameters

The router applies the `Request` and `Response` objects and a `closure` to invoke the next route callback.
```php
$router->get('/welcome', function(Request $request, Response $response, Closure $next)
{
    # Alter response output or call next
});
```

When using a static class method, the parameters are passed directly to the method:
```php
$router->get('/', Welcome::class . '::myMethod');
```

```php
namespace app\controllers;

use Closure;
use serve\http\request\Request;
use serve\http\response\Response;

class Welcome
{
    public static function myMethod(Request $request, Response $response, Closure $next)
    {
        # Handle route
    }
}
```

For non-static methods, the class constructor will receive the parameters:
```php
$router->get('/', Welcome::class . '@myMethod');
```
```php
namespace app\controllers;

use Closure;
use serve\http\request\Request;
use serve\http\response\Response;

class Welcome
{
    public function __construct(Request $request, Response $response, Closure $next)
    {
        # Save parameters to the object locally here
    }

    public function myMethod()
    {
        # Handle route
    }
}
```

You can also pass your own parameter as a fourth argument to the router. The parameter will be applied to the callback.

```php
$router->get('/welcome', function(Request $request, Response $response, Closure $next, $foo)
{
    echo $foo;

}, 'foo');
```

To supply multiple parameters, use an array:

```php
$router->get('/welcome', function(Request $request, Response $response, Closure $next, $foo, $bar)
{
    echo $foo . $bar;

}, ['foo', 'bar']);
```

It works the same way for class method callbacks (both static and non-static):

```php
$router->get('/welcome', Welcome::class . '::myMethod', ['foo', 'bar']);

$router->get('/welcome', Welcome::class . '@myMethod', ['foo', 'bar']);
```

You can also supply a namespace class as your third parameter. When using the internal `Model` and `Controller` base classes the instantiated model is made available to the controller via the the `model` property:

> See the [Controllers](/8.1.0/04_mvc/01_controllers) and [Models](/8.1.0/04_mvc/02_models) documentation for further details.

```php
use app\controllers\Welcome as WelcomeController;
use app\models\Welcome as WelcomeModel;

$router->get('/welcome', WelcomeController::class . '@myMethod', WelcomeModel::class);

```

```php
namespace app\controllers;

use serve\mvc\controller\Controller;

class Welcome
{
    public function myMethod()
    {
        $this->model->doSomething();
    }
}
```

```php
namespace app\models;

use serve\mvc\model\Model;

class Welcome extends Model
{
    public function doSomething()
    {
        // Do validation here
    }
}
```