# Database

--------------------------------------------------------

- [Access](#access)
- [Connections](#connections)
- [Connection Handler](#connection-handler)
- [Queries](#queries)
- [Cache](#cache)
- [Schema](#schema)

--------------------------------------------------------

Serve's Database service provides simple yet surprisingly robust handler for managing database connections and queries.

When you instantiate an installed Serve installation, it will automatically connect to the database for you.

All interactions with the database are done through Serve's single `Database` class. The `Database` class automatically prepares all your statements and queries for you, so you don't need to worry about SQL injection.

--------------------------------------------------------

### Access

You can access the Database connection manager directly through the IoC container:

```php
$database = $serve->Database;
```

--------------------------------------------------------

### Connections

Creating a database connection is done using the `connection` method:
```php
# Returns connection object using the "default" database configuration defined in the config file
$connection = $database->connection();

# Returns connection object using the "mydb" database configuration defined in the config file
$connection = $database->connection('mydb');
```

The `connections` method returns an array of connection objects for all active connections:
```php
$connections = $database->connections();
```

The `connect` method attempts to connect to the database and throws a `PDOException` if it fails. If successful, a `PDO` extension instance is returned:
```php
# You may need to try catch this if you don't want the exception to be thrown
try
{
    $pdo = $connection->connect();
}
catch(PDOException $e)
{
    # Do something else here
}
```

The `isConnected` method check if the connection is connected to the database and returns a boolean:
```php
if ($connection->isConnected())
{
}
```

The `reconnect` method attempts to reconnect to the database and throws a `PDOException` if it fails. If successful, a `PDO` extension instance is returned:

```php
if ($connection->isConnected())
{
    try
	{
	    $pdo = $connection->reconnect();
	}
	catch(PDOException $e)
	{
	    # Do something else here
	}
}
```

The `pdo` method is similar to the reconnect method and attempts to connect to the database and throws a `PDOException` if it fails. If successful, a `PDO` extension instance is returned:
```php
try
{
    $pdo = $connection->pdo();
}
catch(PDOException $e)
{
    # Do something else here
}
```

The `isAlive` method is checks if the current connection is alive and returns a boolean:
```php
if ($connection->isAlive())
{
}
```

The `close` method closes the connection and destructs the `PDO` extension instance.
```php
$connection->close();
```

The `tablePrefix` method returns the string prefix of the database table or an empty string if one is not set.
```php
$prefix = $connection->tablePrefix();
```

--------------------------------------------------------

### Connection Handler

Connections come with a handy `connectionHandler` for interacting with the database and executing queries. When running queries through any database the `connectionHandler` should always be used.

You can call the `handler` method to get a connection's handler instance.
```php
$handler = $connection->handler();
```

The `getLog` method returns an array of all queries executed by the handler.
```php
$log = $handler->getLog();

$lastQuery = array_pop($handler->getLog());
```

--------------------------------------------------------

### Queries

Database queries should be run through a `connectionHandler` instance.

The `query` method executes a given query:
```php
$users = $handler->query('SELECT * FROM serve_users');
```

The `row` method returns a single row or an empty array if the results are empty:
```php
$users = $handler->row('SELECT * FROM serve_users WHERE email = :email', ['email' => 'email@example.com']);
```

The `single` method always returns a single value of a record:
```php
$name = $handler->single('SELECT name FROM serve_users WHERE id = :id', ['id' => 1]);
```

The `column` method always returns a single column:
```php
$names = $handler->column('SELECT name FROM serve_users');
```

#### Bindings

Binding parameters is the best way to prevent SQL injection. The class prepares your SQL query and binds the parameters afterwards. There are three different ways to bind parameters:
```php
# 1. Read friendly method  
$handler->bind('id', 1);
$handler->bind('name','John');
$user = $handler->query('SELECT * FROM serve_users WHERE name = :name AND id = :id');

# 2. Bind more parameters
$handler->bindMore(['name'=>'John','id'=>'1']);
$user = $handler->query('SELECT * FROM serve_users WHERE name = :name AND id = :id');

# 3. Or just give the parameters to the method
$user = $handler->query('SELECT * FROM serve_users WHERE name = :name', ['name'=>'John','id'=>'1']);
```

#### Delete / Update / Insert

When executing the `delete`, `update`, or `insert` statements via the query method the affected rows will be returned:

```php
# Delete
$delete = $handler->query('DELETE FROM serve_users WHERE Id = :id', ['id'=>'1']);
```

```php
# Update
$update = $handler->query('UPDATE serve_users SET name = :f WHERE Id = :id', ['f'=>'Jan','id'=>'32']);
```

```php
# Insert
$insert = $handler->query('INSERT INTO serve_users(name,Age) VALUES(:f,:age)', ['f'=>'Vivek','age'=>'20']);

# Do something with the data 
if($insert > 0 )
{
    return 'Succesfully created a new person !';
}
```

The `lastInsertId` method returns the last inserted id:
```php
if ($handler->query('INSERT INTO serve_users(name,Age) VALUES(:f,:age)', ['f'=>'Vivek','age'=>'20']))
{
    $id = $handler()->lastInsertId();
}
```

#### Method Params

The `row` and the `query` method have a third optional parameter which is the fetch style.

The default fetch style is `PDO::FETCH_ASSOC` which returns an associative array. You can change this behavior by providing a valid [PHP PDO fetch_style](http://php.net/manual/en/pdostatement.fetch.php) as the third parameter.
```php
# Fetch style as third parameter
$authorNum = $connection->row('SELECT * FROM serve_users WHERE id = :id', ['id' => 1 ], PDO::FETCH_NUM);

print_r($person_num);
# [ [0] => 1 [1] => Johny [2] => Doe [3] => M [4] => 19 ]
```

--------------------------------------------------------

### Cache
The `ConnectionHandler` comes with a very basic cache implementation for caching `SELECT` query results across a single request.

When enabled, the cache will check to see if the same query has already been run during the request and attempt to load it from the cache.

If you execute an `UPDATE` or `DELETE` query, the results will be cleared from the cache.

To access the cache, use the cache method on a `ConnectionHandler` instance:
```php
$cache = $handler->cache();
```

The `disable` method disables the cache:
```php
$cache->disable();
```

The `enable` method enables the cache:
```php
$cache->enable();
```

The `enabled` method returns a boolean on the cache status:
```php
if ($cache->enabled())
{   
}
```

--------------------------------------------------------

### Schema
Serve doesn't ship with a default database schema. The `Gatekeeper` service however requires a user table to function correctly.

> For more details see the [Gatekeeper](/8.0.0/08_security/02_gatekeeper) documentation.