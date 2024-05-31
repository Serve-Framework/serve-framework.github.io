# Graphql

--------------------------------------------------------

- [Access](#access)
- [Configuration](#Configuration)
- [Connections](#connections)
- [Connection Handler](#connection-handler)
- [Queries](#queries)
- [Response](#response)
- [Cache](#cache)

--------------------------------------------------------

Serve's Graphql service provides simple yet robust handler for managing graphql connections and queries.

--------------------------------------------------------

### Access

You can access the Graphql connection manager directly through the IoC container:

```php
$graphql = $serve->Graphql;
```

--------------------------------------------------------

### Configuration

Configurations can be found the `graphql.php` configuration file.

| Key                  | Var Type               | Description                                 |
|:---------------------|:-----------------------||:-------------------------------------------|
| domain               | `string`               | Graphql endpoint domain                     |
| path                 | `string`               | Endpoint path                               |
| auth                 | `array`                | Array of authentification headers to send   |
| cache                | `boolean`              | Enable or disabled caching requests         |
| pre_connect          | `Colsure|null`         | Optional pre connection callback            |
| throttle             | `array`                | Array of throttle config                    |
| throttle.enabled     | `boolean`              | Enable or disable request throttling        |
| throttle.limit       | `int`                  | Total request limits per miliseconds        |
| throttle.miliseconds | `int`                  | miliseconds per max requests                |

--------------------------------------------------------

### Connections

Creating a graphql connection is done using the `connection` method:
```php
# Returns connection object using the "default" graphql configuration defined in the config file
$connection = $graphql->connection();

# Returns connection object using the "shopify" graphql configuration defined in the config file
$connection = $graphql->connection('shopify');
```

The `connections` method returns an array of connection objects for all active connections:
```php
$connections = $graphql->connections();
```

The `connect` method attempts to connect to a graphql service and returns a `Client` instance:
```php
# You may need to try catch this if you don't want the exception to be thrown
$client = $connection->connect();
```

The `handler` method returns the `connectionHandler` instance:

```php
$handler = $connection->handler();
```

The `builder` method returns a query `Builder` instance:

```php
$builder = $connection->builder();
```

The `isConnected` method check if the connection `Client` has been instantiated:
```php
if ($connection->isConnected())
{
}
```

--------------------------------------------------------

### Connection Handler

Connections come with a handy `connectionHandler` for interacting with graphql and executing queries. When running queries through any graphql the `connectionHandler` should always be used.

You can call the `handler` method to get a connection's handler instance.
```php
$handler = $connection->handler();
```

--------------------------------------------------------

### Queries

The `query` method allows you to send query in string format. A [Response](#Response) object will be returned:
```php
$queryStr = '
query {
  products(first: 10, query: "product_type:snowboards") {
    edges {
      node {
        title
      }
    }
  }
}
';

$response = $handler->query($queryStr);
```

The `getLog` method returns an array of all queries executed by the handler.
```php
$log = $handler->getLog();

$lastQuery = array_pop($handler->getLog());
```

Binding parameters is the protects from malicious code injection. The class prepares your query and binds the parameters afterwards. There are three different ways to bind parameters:

The `bind` method binds a key/value pair to a name in the query:
```php
$handler->bind('id', 1);
```

The `bindMultiple` method binds an array of key/value pairs to value in a query:
```php
$handler->bindMultiple(['id', 1, 'type' => 'snowboards']);
```

You can optionally bind parameters as a second argument to the `query` method:
```php
$response = $handler->query($queryStr, ['productType'=> 'snowboards']);
```

Below is an example of a full query:
```php
$queryStr = '
query {
  products(first: 10, query: "product_type:snowboards") {
    edges {
      node {
        title
      }
    }
  }
}
';

$response = $handler->query($queryStr);
```

--------------------------------------------------------

### Response

When a query is executed, a `Response` object is always returned. The `Response` object provides a consistent way to handle GraphQL responses.

```php
$response = $handler->query($queryStr);
```

The `message` method returns the HTTP response message:
```php
$message = $response->message();
```

The `code` method returns the HHTP response status code:
```php
$code = $response->code();
```

The `body` method returns the HHTP body as an array:
```php
$body = $response->body();
```

The `body` method returns the HHTP body as an array:
```php
$body = $response->body();
```

If `true` is provided as an arugment, the returned response body will attempt to be normalized which removes unnessary `edges` and `node` keys from the response array making it easier to handle: 
```php
$body = $response->body(true);
```

The `errors` method returns an array of any response errors:
```php
$errors = $response->errors();
```

The `headers` method returns an array of response headers:
```php
$response->headers();
```

The following helper methods are provided for quick and easy validation:

```php
$response->isNotModified()
$response->isEmpty()
$response->isInformational()
$response->isOk()
$response->isSuccessful()
$response->isRedirect()
$response->isForbidden()
$response->isNotFound()
$response->isClientError()
$response->isServerError()
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
