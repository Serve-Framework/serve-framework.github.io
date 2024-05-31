# Application access

--------------------------------------------------------

* [Instantiated reference](#instantiated-reference)
* [Static instance](#static-instance)
* [Currying](#currying)
* [Controllers and Models](#controllers-and-models)
* [Views](#views)
* [Container Aware](#container-aware)

--------------------------------------------------------

When you are working with Serve you will enter various scopes in your code (e.g. global, class function scope etc..). You will likely need a reference to the Serve instance in each scope. There are several ways to do this:

- Use Serve's static `::instance` method.
- Curry an application instance into a function scope with the use keyword.
- Use Serve's `ContainerAware` trait inside a class.
- Use the `$serve` variable inside any view template file.

--------------------------------------------------------

### Instantiated reference

Within your front-controller file where Serve is first instantiated (`app/bootstrap.php`) will always be a reference to the Serve application.

This is always the `$serve` variable. If you're working within this scope, you can simply use the instantiated instance:

```php
$SQL = $serve->Database->Builder();
```
--------------------------------------------------------

### Static instance

If you need to get a reference to the Serve application instance from a different scope (e.g in a class or function), you can use Serve's static `::instance` method to get an instance:

```php
use serve\application\Application;

...

function fooBar()
{
    $serve = Application::instance();
}
```

--------------------------------------------------------

### Currying

When declaring an anonymous function (e.g a callback), you can inject a `$serve` reference into the callback function with the use keyword:

```php
use serve\application\Application;

...

$serve = Application::instance();

$callback = function($args) use ($serve)
{ 
    $SQL = $serve->Database->Builder();
};
```
> Keep in mind that isn't the most performance efficient way to get a reference to Serve.

--------------------------------------------------------

### Controllers and Models

Serve comes with a base [model](/8.1.0/03_mvc/02_models) and [controller](/8.1.0/03_mvc/01_controllers) you can use by extending them.

The base model and controller use Serve's `ContainerAwareTrait` for application access. This means that any service that is loaded into the Serve IoC container will be available to any model or controller you create.

The following example controller retrieves the `Config` service

```php
namespace app\controllers;

use serve\mvc\controller\Controller;

/**
 * Greeting controller.
 */
class Greeting extends Controller
{
    public function welcome(): void
    {       
        $config = $this->Config;
    }
}

```

The following example model retrieves the a `SQL Builder` from `Database` service:

```php
namespace app\models;

use serve\mvc\model\Model;

/**
 * Greeting model.
 */
class Greeting extends Model
{
    public function welcome()
    {
        $sql = $this->Database->builder();
    }
}

```

--------------------------------------------------------

### Views

When working within any file loaded from the [view](/8.1.0/03_mvc/03_view), a Serve instance is always available via the `$serve` variable.

```php
$sql = $serve->Database->Builder();
```

--------------------------------------------------------

### Container Aware

Serve comes with a handy `Trait` that you can apply to any class called `ContainerAwareTrait`. 

The trait can be applied inside any class for application access. This means that any service that is loaded into the Serve application via the IoC container will be available to a class with this trait using PHP's Magic methods.

To use the trait, load the container into your class:

```php
use serve\ioc\ContainerAwareTrait;

...

class Example
{
    use ContainerAwareTrait;
}

```

You will now have access to all Serve services inside the container via via PHP's magic methods:

```php

$myClass = new Example;

$sql = $myClass->Database->builder();
```
