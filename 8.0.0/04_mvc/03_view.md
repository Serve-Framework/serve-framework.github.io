# View

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Serve's `View` object serves as a helper class to output content via the response object. Your view files should be loaded via the View.

--------------------------------------------------------

### Access

You can access the View directly through the IoC container:
```php
$view = $serve->View;
```

Or via the Response's view method:
```php
$view = $serve->Response->view();
```

--------------------------------------------------------

### Usage

The `include` method allows you to provide PHP files that you want to make available within the template that gets loaded. This allows you to decouple any logic from the view template itself - and follow a pull design pattern.
```php
$view->include('path/to/functions.php');
```

To include multiple files, use the `includes` method with an array:
```php
$view->includes([
	'path/to/functions.php',
	'path/to/extras.php'
]);
```

The `display` method loads a file and returns the contents:
```php
$html = $view->display('path/to/view.php');
```

You can optionally pass an array of data to be available inside the view scope:
```php
$html = $view->display('path/to/view.php', ['foo' => 'bar']);
```
Inside the `path/to/view.php` file:

```php
# outputs 'bar'
echo $foo;
```

The most common way to use the `View` is via Serve's `Response`:
```php
$serve->Response->body()->set($serve->View->display('path/to/view.php'));
```

