# CLI

--------------------------------------------------------

- [Usage](#usage)

--------------------------------------------------------

Serve includes a command line tool called `console`. Console comes with a set of useful commands out of the box and you can also create your own custom commands. Commands are especially useful when creating long running tasks such as cron jobs or queue workers.

--------------------------------------------------------

### Usage

To list the available commands just type the following in your console:
```bash
php app/console
```

It should print something similar to the following on a default Serve installation.

Usage:
```bash
php console [command] [arguments] [options]

Available commands:

---------------------------------------------------------
| Command         | Description                         |
---------------------------------------------------------
| encrypt         | Secularly encrypts a string         |
| generate_secret | Generates a new application secret. |
| list_routes     | Lists available HTTP routes         |
| list_services   | Lists available container services. |
---------------------------------------------------------
```
> Note you will need to use the full path to the PHP binary that has access to your database or if you intend to access a Serve database connection via the CLI.

Executing a command is easy and can be done like this:
```bash
php app/console list_routes
```

You can list detailed information about a specific command using the --help flag.
```bash
php app/console list_routes --help
```
