# Controllers

--------------------------------------------------------

- [Usage](#usage)
- [Helpers](#helpers)

--------------------------------------------------------

Controllers serve as callbacks from the router. They communicate with a model before altering the response object and displaying the view.

Serve comes with a super handy base controller that you can extend for your own purposes. You should store your controllers in the `app/controllers` directory.

--------------------------------------------------------

### Usage

To create a controller extend Serve's base controller:
```php
use serve\mvc\controller\Controller;

class Example extends Controller
{

}
```

Controllers should be instantiated from the router directly. An additional string paramater defines the model the controller will load:

```php
use app\controllers\Welcome as WelcomeController;
use app\models\Welcome as WelcomeModel;

$router->get('/welcome', WelcomeController::class, WelcomeModel::class);
```

Or via a middleware layer using the framework's [Onion](/routing/middleware):
```php
$onion->addLayer(WelcomeController::class, WelcomeModel::class);
```

Once inside the controller, the model will always be available via the `model` property.

```php
if ($this->model->someValidation())
{
    # Do something here
}
```

--------------------------------------------------------

### Helpers

Controllers are `ContainerAware` so you can access all properties of the dependency container via class properties:
```php
$this->Response->body()->set('Hello World!');
```

The `nextMiddleware` method invokes the next midlleware layer:
```php
public function welcome()
{       
    $this->Response->body()->set('Hello World!');

    $this->nextMiddleware();
}
```

The `fileResponse` method allows you to quickly load a template file into the view:
```php
public function welcome()
{       
   $this->fileResponse('path/to/view.php');
}
```

You can optionally pass an array of data to be available inside the view scope, as well as set the format and encoding:

> The format and encoding default to `html` and `utf-8`

```php
$this->fileResponse('path/to/view.php', ['foo' => 'bar'], 'html', 'utf-8');
```
Inside the `path/to/view.php` file:

```php
# outputs 'bar'
echo $foo;
```

The `jsonResponse` method allows you to return an array as JSON as the response, this is great for `AJAX` requests:
```php
$this->jsonResponse(['response' => 'Hello World!']);
```

The `redirectResponse` method lets you quickly send a temporary redirect response:
```php
$this->redirectResponse('/moved/to/somewhere');
```

The `notFoundResponse` method lets you quickly send a 404 response:
```php
$this->notFoundResponse();
```

An example controller can be found in the `app/controllers/Greeting.php` file.