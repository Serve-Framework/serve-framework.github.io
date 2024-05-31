# Humanizer

--------------------------------------------------------

- [Usage](#usage)

--------------------------------------------------------

The `Humanizer` helps convert various data into more human friendly format for output.

--------------------------------------------------------

### Usage

The `fileSize` method returns a human readable version of a file size. It assumes the input value is in bytes:
```php
// $size = "4.24 MB"
$size = Humanizer::fileSize(4242423);
```

The `timeAgo` method returns a human readable version of the time since a date. You can supply a valid UNIX timestamp or any string based time value:
```php
// "15 years ago"
echo Humanizer::timeAgo('July 1, 2000').' ago';
```

The `pluralize` method converts any word into its plural:
```php
// 'avocados'
$avocados = Humanizer::pluralize('avocado');

```

You can also provide a second argument to define an amount:
```php
$avocados = Humanizer::pluralize('avocado', 1);
# 'avocado'

$avocados = Humanizer::pluralize('avocado', 0);
# 'avocados'
```