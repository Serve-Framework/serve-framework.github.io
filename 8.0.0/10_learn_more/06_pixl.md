# Pixl

--------------------------------------------------------

- [Configuration](#configuration)
- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

The `Pixl` service allows you to manipulate images through a consistent API using PHP's GD library.

--------------------------------------------------------

### Configuration

Configuration for the `Pixl` services are in the `app/configurations/pixl.php` file.

--------------------------------------------------------

### Access

You can access the `Pixl` service via the IoC container.
```php
$pixl = $serve->Pixl;
```

--------------------------------------------------------

### Usage

Set the image you want to work on by calling `loadImage`:
```php
$pixl->loadImage('path/to/image.png');
```
> The `Image` class supports `.png`, `.jpg` and `.gif` image files.

The `resizeToHeight` method resizes an image to a fixed height (in pixels), while automatically setting the width based on the image's aspect ratio.

The second (optional) parameter defines whether to allow enlarging the image. This defaults to `FALSE`.
```php
// Don't allow enlarging
$pixl->resizeToHeight(300);

// Allow enlarging
$pixl->resizeToHeight(300, true);
```

The `resizeToWidth` method resizes an image to a fixed width (in pixels), while automatically setting the height based on the image's aspect ratio.

The second (optional) parameter defines whether to allow enlarging the image. This defaults to `FALSE`.
```php
// Don't allow enlarging
$pixl->resizeToWidth(300);

// Allow enlarging
$pixl->resizeToWidth(300, true);
```

The `scale` method scales an image by a percentage while preserving the image's aspect ratio:
```php
// Origional image is 50x50px
// New image will be 150x150px
$pixl->scale(150);
```

The `resize` method resizes an image by scaling and cropping while preserving image's aspect ratio.

The third (optional) parameter defines whether to allow enlarging the image. This defaults to `FALSE`.

> When specifying both the width and height, the parameter order is always `width` - `height`

```php
// Don't allow enlarging
$pixl->resize(300, 150);

// Allow enlarging
$pixl->resize(300, 150, true);
```

The `crop` method crops an image to a fixed width and height. The third (optional) parameter defines whether to allow enlarging the image. This defaults to `FALSE`.

```php
// Don't allow enlarging
$pixl->crop(300, 150);

// Allow enlarging
$pixl->crop(300, 150, true);
```

The `addBackground` method allows you to add a background color to an image using `RGB` values:
```php
// Add a white background to an image
$pixl->addBackground(255, 255, 255);
```

The `save` method saves any changes made. The parameters are `$filename`, `$image_type`, `$quality` and `$permissions` with only the first being mandatory.

```php
$pixl->save('path/to/destination.png', IMAGETYPE_PNG, 80);
```