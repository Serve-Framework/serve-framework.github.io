# CDN Support

- [Configuration](#configuration)
- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Serve's `CDN` service is used to filter the HTTP response body. The `CDN` helper is used by the `Response` to filter the HTTP response body so that asset URLS and images are loaded from a `CDN` host.

--------------------------------------------------------

### Configuration

The cdn options can be configured in the `app/configurations/cdn.php` file.

--------------------------------------------------------

### Access

An instance can be accessed via the `Response` object.

```php
$cdn = $serve->Response->cdn();
```

--------------------------------------------------------

### Usage

The `enabled` method checks if the `CDN` is enabled:
```php
if ($cdn->enabled())
{
    # The CDN is enabled
}
```

The `disable` method disables the CDN for the current request:
```php
$cdn->disable();
```

The `enable` method enables the CDN for the current request:
```php
$cdn->enable();
```

Use the `filter` method to filter your own HTML content:
```php
$html = $cdn->filter($HTML);
```