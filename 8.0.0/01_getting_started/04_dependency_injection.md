# Dependency injection

--------------------------------------------------------

* [Auto Loading](#auto-loading)
* [Registering Services](#registering-services)
* [Access](#access)
* [Usage](#usage)

--------------------------------------------------------

Serve comes with an easy to use inversion of control container.

The IoC Container uses dependency injection to prepare, manage, and inject application dependencies making Serve more maintainable and decoupled.

All Serve services are instantiated using the IoC container making it easy to inject dependencies.

--------------------------------------------------------

### Auto Loading

Serve's uses composer's autoloader for registering external dependencies. For more details, please see the [Composer Documentation](https://getcomposer.org/doc/).

--------------------------------------------------------

### Registering Services

If you want register a service inside Serve's IoC container, you can create a custom boot service inside the `app/services` directory.

```php
namespace app\services;

use serve\application\services\Service;

class CustoService extends Service
{
    /**
     * {@inheritdoc}
     */
    public function register()
    {
        $this->container->singleton('MyClass', function ()
        {
            return new namespace\MyClass;
        });
    }
}
```

Then get Serve to boot the service at runtime by adding it into Serve's `app/configuraions/application.php` file under the `services.app` key.

```php
'app' =>
[
    '\app\services\CustoService'
],
```

The `CustoService` service will now boot at runtime.

--------------------------------------------------------

### Access

You can get direct access to Serve's container via a Serve instance using the container method:

```php
$container = $serve->container();
```

If you don't have access to the Serve application use the instance method:

```php
use serve\application\Application;

...

$container = Application::instance()->container();
```

--------------------------------------------------------

### Usage

To register a dependency in the container use the `set` method:
```php
$container->set('foo', new ClassName;
```

You can also register dependencies using a `closure`. The `closure` will not be executed until it is required.
```php
$container->set('foo', function($container)
{
    return new ClassName;
});
```

The `singleton` method does the same as the `set` method except that it makes sure that the same instance is returned every time the class is resolved through the container.
```php
$container->singleton('foo', function($container)
{
    return new lassName($container->foobar);
});
```

The `setInstance` method is similar to the `singleton` method. The only difference is that it allows you to register an existing instance in the container.
```php
$container->setInstance('foo', new Bar);
```

The `has` method allows you to check for the presence of an item in the container.
```php
if ($container->has('foo'))
{
    // do something
}
```

The `get` method lets you resolve a dependency through the IoC container.
```php
$foo = $container->get('foo');
```

You can also use PHP's magic methods to retrieve dependencies.

```php
$foo = $container->foo;
```

> All Serve services use Title casing to identify a service - i.e `Config` not `config`. This isn't a requirement for your own application but helps keep naming standards consistent.
