# Shell

- [Usage](#usage)
- [Chain](#chain)

--------------------------------------------------------

Serve's `Shell` provides a persistent and safe interface into the shell via PHP.

--------------------------------------------------------

### Usage

Create a new `Shell` instance:
```php
use serve\shell\Shell;

$shell = new Shell;
```

You can optionally pass a directory path to the constructor to set the working directory for subsequent commands:
```php
$shell = new Shell('path/to/working/dir');
```

Or use the `cd` command:
```php
$shell->cd('path/to/working/dir');
```

To set the command, use the `cmd` method:
```php
$shell->cmd('git', 'clone');
```

To set a command option use the `option` method:
```php
$shell->option('bare');
```

To set an option with a value, pass the value as a second parameter:
```php
$shell->option('user.name', 'Foo Bar');
```

Or set multiple options with the `options` method:
```php
$shell->options([
	'bare',
	'user.name' => 'Foo Bar'
]);
```

To set a parameter, use the `param` method:
```php
$shell->param('https://github.com/Serve-Framework/Framework.git');
```

Or set multiple parameters with the `params` method:
```php
$shell->params([
	'https://github.com/Serve-Framework/Framework.git',
	'.'
]);
```

To set an input file, use the `input` method:
```php
$shell->input('path/to/src.txt');
```

To set an output, use the `input` method:
```php
$shell->output('path/to/dest.txt');
```

To run a command use the `run` method:
```php
$output = $shell->run();
```

To run a command and return any output errors pass `TRUE`:
```php
$output = $shell->run(true);
```

To check if a command was successful use the `is_successful` method:
```php
if ($shell->is_successful())
{
}
```

--------------------------------------------------------

### Chain

The `Shell` methods are all chainable and return the `Shell` instance. Here is an example:
```php

$output = $shell->cd('/path/to/dir')
                ->cmd('git', 'clone')
                ->option('bare')
                ->param('https://github.com/Serve-Framework/Framework.git')
                ->param('.')
                ->run();

if ($shell->is_successful())
{
	// Command was successful
}
else
{
	// There was an error
}
```
