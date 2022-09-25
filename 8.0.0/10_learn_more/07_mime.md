# Mime

--------------------------------------------------------

- [Usage](#usage)

--------------------------------------------------------

The `Mime` utility can be used to output HTTP mime types based on file extensions

--------------------------------------------------------

### Usage

The `fromExt` method converts a file extension to a valid mime type:
```php
use serve/utility/Mime;

// outputs image/svg+xml
$mime = Mime::fromExt('svg');
```

The `toExt` method a mime type back to a file extension
```php
use serve/utility/Mime;

// outputs svg
$mime = Mime::fromExt('image/svg+xml');
```