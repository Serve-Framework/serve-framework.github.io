# Callback

--------------------------------------------------------

- [Usage](#usage)

--------------------------------------------------------

The `Callback` utility can be used as a consistenat way to call a method, class or class method.

--------------------------------------------------------

### Usage

The `apply` method will call a function
```php
$func = function($foo)
{
	echo $foo;
}

$mime = Callback::apply($func, 'foo');
```

If a namespaced class is provided the class will be instantiated with the provided args to the constructor:
```php
$mime = Callback::apply(MyClass::class, 'foo');
```

You can provide multiple args via an array. The parameters will be provided to the class constructor:
```php
$mime = Callback::apply(MyClass::class, ['foo', 'bar']);
```

You can instantiate a class and call one of its methods using the `@` syntax. Arguments are applied to the class constructor:
```php
$mime = Callback::apply(MyClass::class . '@methodName', 'foo');
```

You can also call a static method on a class using the `::` syntax.  Arguments are applied to the static method:
```php
$mime = Callback::apply(MyClass::class . ':methodName', 'foo');
```