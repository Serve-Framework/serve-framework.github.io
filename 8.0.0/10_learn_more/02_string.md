# String Helper

--------------------------------------------------------

- [Usage](#usage)

--------------------------------------------------------

The string helper contains methods that can be useful when working with strings.

--------------------------------------------------------

### Usage

The `reduce` method reduces a string to a given length by either characters or words. The parameters are `$string`, `$length`, `$suffix` and `$toChar` with the latter being an optional boolean. The reduce method defaults to characters:
```php
// $reduced = 'str...';
$reduced = Str::reduce('string', 3, '...', true);

// $reduced = 'one two';
$reduced = Str::reduce('one two tree', 2, '', false);
```

The `contains` method return `TRUE` if a sring contains another string or `FALSE` if not:
```php
$string = 'foobar';
$query  = 'foo';

// $contains = true
$contains = Str::contains($string, $query);
```

The `getAfterLastChar` method returns the part of the string after the last occurrence of a character. It will return the whole string if it doesn't contain the query character:
```php
// $ending = "bar"
$ending = Str::getAfterLastChar('example/foo/bar', '/');
```

The `getBeforeLastChar` method returns the part of the string before the last occurrence of a character. It will return the whole string if it doesn't contain the query character:
```php
// $start = "example/foo"
$start = Str::getBeforeLastChar('example/foo/bar', '/');
```

The `getAfterFirstChar` method returns the part of the string after the first occurrence of a character. It will return the whole string if it doesn't contain the query character:
```php
// $end = "foo/bar"
$end = Str::getAfterFirstChar('example/foo/bar', '/');
```

The `getBeforeFirstChar` method returns the part of the string before the first occurrence of a character. It will return the whole string if it doesn't contain the query character:
```php
// $start = "example"
$start = Str::getBeforeFirstChar('example/foo/bar', '/');
```

The `getAfterLastWord` method returns the part of the string after the last occurrence of a word. It will return the whole string if it doesn't contain the query character:
```php
// $after = "/foo/bar"
$after = Str::getAfterLastWord('example/foo/bar', 'example');
```

The `getBeforeLastWord` method returns the part of the string before the last occurrence of a word. It will return the whole string if it doesn't contain the query character:
```php
// $before = "/example/foo/"
$before = Str::getBeforeLastWord('example/foo/bar', 'bar');
```

The `random` method returns a random string of the selected type and length. The available constants are `ALNUM`, `ALPHA`, `HEXDEC`, `NUMERIC` and `SYMBOLS`. You can also combine pools or pass your own pool of characters:
```php
$str = Str::random(Str::ALNUM);

$str = Str::random(Str::ALPHA);

$str = Str::random(Str::NUMERIC);

$str = Str::random(Str::ALNUM . Str::SYMBOLS);

$str = Str::random(Str::ALNUM . 'æøåÆØÅ', 64);
```

The `nl2br` method converts newlines to `<br>`:
```php
$str = Str::nl2br($_POST['input']);
```

> The nl2br method will return HTML5 tags by default but you can make it return XHTML compatible tags by setting the optional second parameter to TRUE.

The `br2nl` method converts `<br>` to newlines:
```php
$str = Str::nl2br($_POST['input']);
```

The `camel2underscored` method converts camelCase to underscore:
```php
// camel_case
$underscore = Str::camel2underscored('camelCase');
```

The `camel2case` method converts camelCase to spaces:
```php
// camel Case
$spaced = Str::camel2case('camelCase');
```

The `underscored2camel` method converts under_scored to camelCase:
```php
// underScored
$camelCase = Str::camel2case('under_scored');
```

The `compareNumeric` method returns a numeric comparison between two values:
```php
// $result = true
$result = Str::compareNumeric('20', 20);
```

The `compareAlphaNumeric` method returns a string comparison between two values:
```php
// $result = true
$result = Str::compareAlphaNumeric('20', 20);
```

The `bool` method returns a boolean from a string or any other value. It processes extra string values `on`, `off`, `true`, `false`, `yes` and `no`:
```php
// $bool = false
$bool = Str::bool('false');
```

The `slug` method filters a string for a URL slug:
```php
$slug = Str::slug('this is some text');
```

The `alpha` method filters out all non alphabetic characters:
```php
$alpha = Str::alpha('text 43423');
```

The `alphaDash` method filters out all non alphabetic characters, replacing single spaces with a hyphen:
```php
$alphaDash = Str::alphaDash('text 43423 text');
```

The `alphaNum` method filters out all non alphanumeric characters:
```php
$alphaNum = Str::alphaNum('text 43423 text');
```

The `alphaNumDash` method filters out all non alphanumeric characters, replacing single spaces with a hyphen:
```php
$alphaNumDash = Str::alphaNumDash('text 43423 text');
```

The `mysqlEncode` method prepares a string for entry into a database:
```php
$data = Str::mysqlEncode('text 43423 text');
```

The `mysqlDecode` method prepares a string after retrieval from a database:
```php
$data = Str::mysqlDecode('text 43423 text');
```

The `strip_tags` method filters out specific HTML tags from a string:
```php
$plainText = Str::strip_tags('<h1>Hello World!</h1>', 'h1');
```

The `queryFilterUri` method removes the query string from a URL:
```php
$uri = Str::queryFilterUri('https://example.com?for=bar');
```

