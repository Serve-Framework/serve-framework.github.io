# Configuration

--------------------------------------------------------

* [Config Files](#config-files)
* [Environment Aware](#environment-aware)
* [Access](#access)
* [Usage](#usage)

--------------------------------------------------------

The configuration and instantiation of the Serve core is done in the `app/init.php` file. This is where you set the error reporting level and define any decencies for the framework to function properly.

All of the remaining framework configuration can be done by editing the files that are located in the `app/configurations` directory.

--------------------------------------------------------

### Config Files

Serve configurations are simple `.php` files that return an array:

```php
return
[
    'key_1' => 'value',
    'key_2' => 'value',
];
```

There are separate configuration files for different application components. All default configuration files are extensively commented, please refer to these files for more details on individual configuration options.

--------------------------------------------------------

### Environment Aware

The configuration files found in `app/configurations`, use an environment aware file system to load. This gives you the ability to run your application with separate configurations for different environments - for example `sandbox` or `production`.

To create an additional configuration environment, all you have to do is create a subdirectory with the name of your environment in the `app/configurations` directory and copy the environment specific files into it.

You can then set the configuration environment in Serve's `app/init.php` file:

```php
putenv('SERVE_ENV=sandbox');
```
> When the `SERVE_ENV` is set, any configuration is loaded from the root configuration path and then merged/overwritten from the nested environment directory if it exists.

--------------------------------------------------------

### Access

Serve's configuration can be accessed via the IoC container using the `Config` key.

```php
$config = $serve->Config;
```

--------------------------------------------------------

### Usage

To retrieve a configuration value (or values), use the `get` method:
```php
$cmsConfig = $serve->Config->get('application');
```

You can also fetch config items using `dot.notation`:
```php
$postPerPage = $serve->Config->get('application.secret');
```

The above method returns the `secret` key value from the `application.php` configuration file. The method is recursive and will work on nested arrays as well:
```php
$thumbnails = $serve->Config->get('application.error_handler.log_path');
```

The `set` method allows you to set a configuration value at runtime:
```php
$serve->Config->set('application.secret', 'my_secret');
```

Removing the custom configuration is done using the `remove` method:
```php
$serve->Config->remove('application.foobar');
```

The `save` method saves the current configuration under the current environment. 
```php
$serve->Config->save();
```
> Note that if your current `SERVE_ENV` is set to `sandbox`, only the configuration files inside the `app/configurations/sandbox` directory will be overwritten when saved.