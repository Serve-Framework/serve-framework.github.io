# Commands

--------------------------------------------------------

* [Basics](#basics)
	- [Getting started](#getting-started)
	- [Registering commands](#registering-commands)
* [Command Info](#command-info)
* [Input](#input)
	- [Arguments and options](#Parameters)
	- [Flags](#flags)
* [Output](#output)
	- [Basics](#basics)
	- [Helpers](#helpers)
	- [Formatting](#formatting)
* [Dependency injection](#dependency_injection)

--------------------------------------------------------

The Serve command line tool comes with a set of useful commands but you can also create your own.

--------------------------------------------------------

### Basics

#### Getting started

All commands must extend the `serve\console\Command` base command and implement the `execute` method.

```php
namespace app\console\commands;

use serve\console\Command;

class Hello extends Command
{
	protected $description = 'Prints hello world';

	protected $commandInformation = 
	[
		['--foo', 'Provides an input string', 'Yes']
	];

	public function execute()
	{
		$this->write('Hello, World!');
	}
}
```

The command `description` and `commandInformation` properties are printed out when the `--help` parameter or `-h` flag is used:
```bash
php console hello --help
```

The above command will print out:
```bash
Command: php console hello
Description: Prints hello world

Arguments and options:

--------------------------------------------------
| Name     | Description              | Optional |
--------------------------------------------------
| --foo    | Provides an input string | Yes      |
--------------------------------------------------
```

#### Registering commands

You'll have to register your command with the console command line tool before you can use it.

Commands are registered in the `app/configurations/application.php` configuration file. The array key is the name of your command and the value is the command class name.

```php
'commands' =>
[
	'hello' => '\app\console\commands\Hello',
],
```

You can now call your custom command like this.

```bash
php console hello
```

--------------------------------------------------------

### Input

You can access the input wrapper via the `input` property of a command:

```php
$input = $this->input;
```

#### Parameters

Arguments are automatically parsed and processed for you. You can access any parameters via the `parameter` method. An associative array will be returned:

```php
$params = $this->input->parameters();
```

For example the following command, will produce the following parameters:
```php
# php console hello --foo=bar

//$this->input->parameters();
[
	'foo' => 'bar'
];
```

You can access a single parameter by key with the `parameters` method. `Null` will be returned if the parameter was not provided via the CLI:
```php
$foo = $this->input->parameter('foo');
```

> You can use any combination of both short-hand and long-hand syntax for parameters e.g. `-f=bar` or even `--foo bar`.

##### Flags

Flags are automatically parsed and processed for you. You can access any flag via the `options` method. An array will be returned:
```php
$flags = $this->input->options();
```

For example the following command, will produce the following options:
```php
# php console hello -a -b -c

//$this->input->options();
['a', 'b', 'c'];
```

You can access a single flag by key with the `option` method. `Null` will be returned if the parameter was not provided via the CLI:
```php
$foo = $this->input->option('f');
```

--------------------------------------------------------


### Output


#### Basics

The `write` method lets you write output:

```php
$this->write('Hello, World!');
```

To make the text coloured, provide a supported color as a second parameter:
```php
$this->write('Hello, World!', 'green');
```

The `dump` method dumps to the output via php's `var_export`:
```php
$this->dump(['foo' => 'bar']);
```

The `clear` method lets you clear all output from the terminal window:
```php
$this->clear();
```

The `error` method writes to output as white text highlighted in red:
```php
$this->error('Something went wrong!');
```


#### Helpers

The `table` method lets you output a nice ASCII table:
```php
$this->table(['Col1', 'Col2'], [['R1 C1', 'R1 C2'], ['R2 C1', 'R2 C2']]);
```

This code above will result in a table looking like this:
```bash
-----------------
| Col1  | Col2  |
-----------------
| R1 C1 | R1 C2 |
| R2 C1 | R2 C2 |
-----------------
```

The `ol` method lets you output an ordered list:
```php
$this->ol(['one', 'two', 'three', ['one', 'two'], 'four']);
```

The example above will output the following list:
```bash
1. one
2. two
3. three
   1. one
   2. two
4. four
```

The `ul` method lets you output an unordered list:
```php
$this->ul(['one', 'two', 'three', ['one', 'two'], 'four']);
```

The example above will output the following list:
```bash
* one
* two
* three
  * one
  * two
* four
```

#### Formatting

You can format your output using formatting tags:
```php
$this->write('<blue>Hello, World!</blue>');
```

You can also nest formatting tags. Just make sure to close them in the right order:
```php
$this->write('<bg_green><black>Hello, World</black><yellow>!<yellow></bg_green>');
```

If you find yourself using the same nested set of formatting tags over and over again, then you'll probably want to define your own custom tags. This can be done using the `Formatter::addStyle()` method.

```php
$this->output->formatter()->addStyle('awesome', ['bg_green', 'black', 'blinking']);
```

Tags can also be escaped by perpending them with a backslash:
```php
$this->write('\<blue>Hello, World\</blue>');
```

If you want to escape all tags in a string then you can use the `Formatter::escape()` method.

```php
$escaped = $this->output->formatter()->escape($string);
```

| Tag        | Description                  |
|------------|------------------------------|
| clear      | Clears all formatting styles |
| bold       | Bold text                    |
| faded      | Faded colors                 |
| underlined | Underlined text              |
| blinking   | Blinking text                |
| reversed   | Reversed text                |
| hidden     | Hidden text                  |
| black      | Black text                   |
| red        | Red text                     |
| green      | Green text                   |
| yellow     | Yellow text                  |
| blue       | Blue text                    |
| purple     | Purple text                  |
| cyan       | Cyan text                    |
| white      | white text                   |
| bg_black   | Black background             |
| bg_red     | Red background               |
| bg_green   | Green background             |
| bg_yellow  | Yellow background            |
| bg_blue    | Blue background              |
| bg_purple  | Purple background            |
| bg_cyan    | Cyan background              |
| bg_white   | white background             |

> Note that formatting will only work on linux/unix and windows consoles with ANSI support.
>
> Formatting is stripped when the output is redirected (e.g. to a log file `php console command > log.txt`).
