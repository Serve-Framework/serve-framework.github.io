# Array Helper

--------------------------------------------------------

- [Usage](#usage)

--------------------------------------------------------

The array helper contains methods that can be useful when working with arrays.

--------------------------------------------------------

### Usage

The `get` method returns a value from an array using "dot notation":
```php
$array =
['foo' => ['bar' => 'baz']];

$bar = Arr::get($array, 'foo.bar');

// You can also specify a default value if the key doesn't exist

$baz = Arr::get($array, 'foo.baz', 'nope');
```

The `set` method sets an array value using "dot notation":
```php
Arr::set($array, 'foo.baz', 'hello world');
```

The `delete` method deletes an array value using "dot notation":
```php
Arr::delete($array, 'foo.bar');
```

The `random` method returns a random array value:
```php
Arr::random(['green', 'blue', 'red', 'orange']);
```

The `isAssoc` method returns `TRUE` if the array is associative and `FALSE` if not:
```php
# $assoc will be set to FALSE

$assoc = Arr::isAssoc([1, 2, 3]);

# $assoc will be set to TRUE

$assoc = Arr::isAssoc(['one' => 1, 'two' => 2, 'three' => 3]);
```

The `isMulti` method returns `TRUE` if the array is multi-dimensional and `FALSE` if not:
```php
# $multi will be set to FALSE

$multi = Arr::isMulti(['one' => 1, 'two' => 2, 'three' => 3]);

# $multi will be set to TRUE

$multi = Arr::isMulti(['one' => 1, 'two' => 2, 'three' => ['one' => 1] ]);
```

The `pluck` method returns the values from a single column of the input array, identified by the key:
```php
$fruits =
[
    ['name' => 'apple', 'color' => 'green'],
    ['name' => 'banana', 'color' => 'yellow'];
];

$colors = Arr::pluck($fruits, 'color');
```

The `insertAt` method inserts a value into an array at a specific index:
```php
$fruits =  ['apple', 'banana', 'orange'];

// ['apple', 'banana', 'cherry', 'orange']
$fruits = Arr::insertAt($fruits, 'cherry', 2);
```

The `issets` method allows you to validate an array of keys exist in an array:
```php
$needles =
[
    'one',
    'two',
    'three'
];

$haystack =
[
    'one' => 1,
    'two' => 2
];

// $issets will be set to FALSE
$issets = Arr::issets($needles, $haystack);
```

The `unsets` method allows you to unset an array of keys from an array:
```php
$needles =
[
    'one',
    'two',
];

$haystack =
[
    'one'   => 1,
    'two'   => 2,
    'three' => 3
];

// $result =
[ 'three' => 3 ];
$result = Arr::unsets($needles, $haystack);
```

The `sortMulti` method will sort a multi-dimensional array by key value. You can use the dot notation to sort by a nested value. To define the sort order, supply a third parameter as either `ASC` or `DESC`:
```php
$haystack =
[
    ['key' => 'g'],
    ['key' => 'a']
];

/*
$sort =
[
    ['key' => 'a'],
    ['key' => 'g']
];
*/
$sort = Arr::sortMulti($haystack, 'key');
$sort = Arr::sortMulti($haystack, 'key', 'DESC');
```

The `implodeByKey` method will implode a given key value of multi-dimensional array:
```php
$haystack = 
[
    [
        'key' => 'value1',
        'foo' => 'bar'
    ],
    [
        'key' => 'value2',
        'foo' => 'bar'
    ]
];

// $implode = "value1 value2"
$implode = Arr::implodeByKey($haystack, 'key', ' ');
```

The `inMulti` method will recursively check if an array key exists in a multi-dimensional array:
```php
$haystack = 
[
    [
        'key' => 'value1',
        'foo' => 'bar'
    ],
    [
        'key' => 'value2',
        'foo' => 'bar'
    ]
];

// $exists = true
$exists = Arr::inMulti('key', $haystack);
```

The `paginate` method will paginate an array of data as well as validate if the current page exists:
```php
$haystack =
[
    [
        'key' => 'value 1',
        'bar' => 'foo',
        'foo' => 'bar'
    ],
    [
        'key' => 'value 2',
        'bar' => 'foo',
        'foo' => 'bar'
    ],
    [
        'key' => 'value 3',
        'bar' => 'foo',
        'foo' => 'bar'
    ],
];
$curentPage = 0;
$perPage    = 2;
$paged      = Arr::paginate($haystack, $curentPage, $perPage);
```