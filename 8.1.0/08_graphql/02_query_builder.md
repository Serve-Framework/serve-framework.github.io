# Query builder

--------------------------------------------------------

- [Access](#access)
- [Query Building](#query-building)

--------------------------------------------------------

Serve's Graphql Query Builder allows you to programmatically build GraphQL queries without having to write giant GraphQL statements or arrays.

Essentially, the Builder class is a chainable wrapper around the GraphQL syntax. All queries executed by the builder use prepared statements, thus mitigating the risk of code injection.

When chaining methods, the chaining order follows the same syntax as if you were to write an GraphQL query statement.

--------------------------------------------------------

### Access

You can access the Builder class directly through the IoC container via the `Graphql` object, note the default connection will be used:
```php
$builder = $serve->Graphql->builder();
```

Alternatively if you have a reference to an existing `Connection`, you can access the builder directly through the `connection`.
```php
$builder = $serve->Graphql->connection()->builder();
```

--------------------------------------------------------

### Query Building

You can use the query builder the same way you would write GraphQL syntax:
```php
$product = $builder->query()->glasses(':product(id: "gid://shopify/Product/1234")')->exec();
```

The `fields` method adds an array fields to current function:
```php
$product = $builder->query()->alias('glasses')->product(['id' => '1234']))->fields(['id', 'title'])->exec();
```

The `alias` method adds the next given function as an alias:
```php
$product = $builder->query()->alias('glasses')->product(['id' => '1234']))->exec();
```

The `root` method resets the callchain back to the query root allowing you to write multiple query statements:
```php
$product = $builder->query()
->alias('glasses')
->product(['id' => '1'])
->fields(['id', 'title'])
->root()
->alias('snowboards')
->product(['id' => '2'])
->fields(['id', 'title'])
->exec();
```
