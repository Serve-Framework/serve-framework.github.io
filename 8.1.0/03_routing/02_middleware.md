# Middleware

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Route middleware allows you to alter the `Request` and `Response` both before and after a route callback gets executed.

--------------------------------------------------------

### Access

You can access the `Onion` service via the IoC container to add middleware.
```php
$onion = $serve->Onion;
```
--------------------------------------------------------

### Usage

Middleware follow the same design patterns as routes. To add a layer, use the `addLayer` method:
```php
$onion->addLayer(function()
{
    // Do something here
});
```

> Middleware get executed in the order that they are assigned. So from first-assigned to last-assigned.

You can also define a static class method as a callback
```php
use app\controller\Welcome;

$onion->addLayer(Welcome::class . '::method');
```

Or a standard class method:
```php
$onion->addLayer(Welcome::class . '@method');
```

Middleware layers receive the `Request`, `Response` objects and a `closure` to invoke the next layer.
```php
use Closure;
use serve\http\request\Request;
use serve\http\response\Response;

$onion->addLayer(function(Request $request, Response $response, Closure $next)
{
    if (!$someCondition)
    {
        $next();
    }
});
```

If the next `closure` is not called, the next middleware layer will not be invoked:
```php
$onion->addLayer(function(Request $request, Response $response, Closure $next)
{
	echo 'foo';

	// Don't call next();
});

$onion->addLayer(function(Request $request, Response $response, Closure $next)
{
	// This will not output
	echo 'bar';
});

```

You can also pass your own parameter as a second argument:
```php
$onion->addLayer(function(Request $request, Response $response, Closure $next, $foo)
{
    
}, 'foo');
```

Or pass multiple parameters with an array:
```php
$onion->addLayer(function(Request $request, Response $response, Closure $next, $foo, $bar)
{
    
}, ['foo', 'bar']);
```

This works the same way for class method callbacks:
```php
$onion->addLayer('/welcome', Welcome::class . '::myMethod', ['foo', 'bar']);

$onion->addLayer('/welcome', Welcome::class . '@myMethod', ['foo', 'bar']);
```

The `layers` method returns an array of all middleware layers in onion:
```php
$layers = $onion->layers();
```
