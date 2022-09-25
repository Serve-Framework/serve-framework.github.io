# Cache

--------------------------------------------------------

- [Configuration](#configuration)
- [Access](#access)
- [Usage](#usage)

--------------------------------------------------------

Serve implements a file cache for storing persistent data data over multiple requests. The Cache is un-opinionated and can be used for any type of data you want to store.

--------------------------------------------------------

### Configuration

The `Cache` service can be configured in the `app/configurations/cache.php` file.

--------------------------------------------------------

### Access

You can access the `Cache` directly through the IoC container:
```php
$cache = $serve->Cache;
```

--------------------------------------------------------

### Usage

To save data to the cache use the `put` method:
```php
$cache->put('key', 'value');
```

To check if a value exists, use the `has` method:
```php
if ($cache->has('key'))
{
    # Do something here
}
```

To retrieve saved content, use the `get` method. `FALSE` will be returned if nothing is cached:
```php
$value = $cache->get('key');
```

To delete a cached value, use the `delete` method:
```php
$cache->delete('key');
```

The `expired` method checks if a cached item is expired:
```php
if ($cache->expired('key'))
{
    # The item expired
}
```

The `clear` method deletes all cached items:
```php
$cache->clear();
```