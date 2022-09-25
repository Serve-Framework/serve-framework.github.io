# Access

--------------------------------------------------------

-   [Access](#access)
-   [Usage](#usage)

--------------------------------------------------------

Serve provides an Access service to manage your `robots.txt` as well as whitelist and blacklist ip addresses.

--------------------------------------------------------

### Access

The `Access` service can be accessed via the IoC container:

```php
$access = $serve->Access;
```

--------------------------------------------------------

### Usage


The `ipBlockEnabled` method returns the IP blocking status:
```php
$enabled = $access->ipBlockEnabled();
```

The `isIpAllowed` method checks if the current request IP is blacklisted:
```php
if ($access->isIpAllowed())
{
    // Do something here
}
```

The `block` method blocks the current request and throws a `ForbiddenException`. No further code is executed:
```php
$access->block();
```

> Configuration for IP blocking and whitelisting can be found in the `app/configurations/application.php` file under `security`.

The `defaultRobotsText` method returns a string for the default `robots.txt` file, which allows all agents from crawling:
```php
$robots = $access->defaultRobotsText();
```

The `blockAllRobotsText` method returns a string for blocking all bot user agents from crawling:
```php
$robots = $access->blockAllRobotsText();
```

The `saveRobots` method saves the `robots.txt` file with the provided content:
```php
// Blocks all crawlers
$access->saveRobots($access->blockAllRobotsText());
```

The `deleteRobots` method deletes the `robots.txt` file:
```php
// Blocks all crawlers
$access->saveRobots($access->blockAllRobotsText());
```