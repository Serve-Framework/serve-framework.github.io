# Query builder

--------------------------------------------------------

- [Access](#access)
- [Query Building](#query-building)
    - [Types](#types)
    - [Filters](#filters)
    - [Organizers](#organisers)
    - [Executions](#executions)
    - [Query Chains](#query-chains)
 - [Table Management](#table-management)
    - [Alter](#alter)
    - [Foreign Keys](#foreign-keys)
    - [Alter Chains](#alter-chains)

--------------------------------------------------------

Serve's Query Builder allows you to programmatically build SQL queries without having to write giant SQL statements.

Essentially, the Builder class is a chainable wrapper around the SQL syntax. All queries executed by the builder use prepared statements, thus mitigating the risk of SQL injections.

When chaining methods, the chaining order follows the same syntax as if you were to write an SQL query statement.

--------------------------------------------------------

### Access

You can access the Builder class directly through the IoC container via the Database object:
```php
$builder = $serve->Database->builder();
```

Alternatively if you have a reference to an existing database `connection`, you can access the builder directly through the `connection`.
```php
$builder = $serve->Database->connection()->builder();
```

--------------------------------------------------------

### Query Building

The Builder class provides a convenient syntax for most SQL query statements by providing a wrapper around the Query class. The methods can be placed into three logical sections:

- Query types
- Query filters
- Query organizers
- Query executions

#### Select

When running `Select` statement to query the database. Use the `SELECT` and `FROM` methods:
```php
$builder->SELECT('*')-FROM('table');
```

You can select multiple columns using comma notation:
```php
$builder->SELECT('id, name, email')-FROM('users')->EXEC();
```

Or optionally use an array if you prefer:
```php
$builder->SELECT(['id', 'name', 'email'])-FROM('users')->EXEC();
```

#### Joins

To execute a join statement, use one of the join methods. 
```php
$builder->SELECT('id')->FROM('users')->LEFT_JOIN_ON('groups', 'users.group_id = groups.id')->EXEC();
```

> The available joins are `INNER_JOIN_ON`, `LEFT_JOIN_ON`, `RIGHT_JOIN_ON`, `LEFT_OUTER_JOIN_ON`, `RIGHT_OUTER_JOIN_ON` and `RIGHT_OUTER_JOIN_ON`.

You can select from multiple tables using `dot.notation`:

```php
$builder->SELECT('users.id, groups.name')
->FROM('users')
->LEFT_JOIN_ON('groups', 'users.group_id = groups.id')
->EXEC();
```

Or use an array:
```php
$builder->SELECT(['users.id, 'groups.name'])
->FROM('users')
->LEFT_JOIN_ON('groups', 'users.group_id = groups.id')
->EXEC();
```

For more complex joins you can also use a multidimensional array:
```php
$builder->SELECT(['users' => ['id, 'email'], 'groups' => ['group_id', 'group_name'] ])
->FROM('users')
->LEFT_JOIN_ON('groups', 'users.group_id = groups.id')
->EXEC();
```

When using a `WHERE` clause, you should specify the table using `dot.notation` as you would in SQL:
```php
$builder->SELECT(['users.id, 'groups.name'])
->FROM('users')
->LEFT_JOIN_ON('groups', 'users.group_id = groups.id')
->WHERE('users.group_id', '=', 1)
->EXEC();
```


```php
# Set the query to query a given table
$builder->FROM($tablename);

# Set the query type to UPDATE on a given table
$builder->UPDATE($tablename);

# Set the query type to INSERT INTO on a given table
$builder->INSERT_INTO($tablename);

# Set the query type to DELETE on a given table
$builder->DELETE_FROM($tablename);

# Set the query type to SELECT on the current 
# table and set the columns to select
$builder->SELECT($tablename);

# Set the query type to INSERT INTO on the current 
# table and set the values to insert
$builder->VALUES($rows);

# Set the query type to SET on the current 
# table and set the values to set
$builder->SET($rows);
```


#### Types

Query types are where you define the type of query you are going to execute `INSERT`, `DELETE`, `UPDATE`, `SET`; as well the table upon which the query will run.

The following methods can be used to set the query type:
```php
# Set the query to query a given table
$builder->FROM($tablename);

# Set the query type to UPDATE on a given table
$builder->UPDATE($tablename);

# Set the query type to INSERT INTO on a given table
$builder->INSERT_INTO($tablename);

# Set the query type to DELETE on a given table
$builder->DELETE_FROM($tablename);

# Set the query type to SELECT on the current 
# table and set the columns to select
$builder->SELECT($tablename);

# Set the query type to INSERT INTO on the current 
# table and set the values to insert
$builder->VALUES($rows);

# Set the query type to SET on the current 
# table and set the values to set
$builder->SET($rows);
```

#### Filters

Query filters are where you build your query to filter the table results. The following methods can be used to filter the query:

```php
# Add a WHERE clause
$builder->WHERE($column, $operator, $value);

# Add an AND WHERE clause
$builder->AND_WHERE($column, $operator, $value);

# Add an OR WHERE clause
$builder->OR_WHERE($column, $operator, $value);

# Nested OR WHERE clause
$builder->OR_WHERE($column, $operator, ['foo', 'bar', 'baz']);

# Add a JOIN and ON clause
$builder->JOIN_ON($tablename, $operation);

# Add an INNER JOIN and ON clause
$builder->INNER_JOIN_ON($tablename, $operation);

# Add an LEFT JOIN and ON clause
$builder->LEFT_JOIN_ON($tablename, $operation);

# Add an RIGHT JOIN and ON clause
$builder->RIGHT_JOIN_ON($tablename, $operation);

# Add an OUTER JOIN and ON clause
$builder->OUTER_JOIN_ON($tablename, $operation);
```

#### Organisers

Query organizers are where you define how your results should be formatted. The following methods can be used to organize the query results:
```php
# Set the ORDER BY keyword
$builder->ORDER_BY($columnName, $direction = 'DESC');

# Add an GROUP BY keyword
$builder->GROUP_BY($columnName);

# Add a GROUP_CONCAT function
$builder->GROUP_CONCAT($column, $operator, $value);

# Set the limit
$builder->LIMIT($offset, $number);
```

#### Executions

Query executions are the final method to call and will immediately execute the query and return the results.
```php
# Execute the query and limit the results to single row
$builder->ROW();

# Execute the query, limit the results to single row
# if an id is provided add (id = $id) AND WHERE clause
$builder->FIND($id = null);

# Execute the query
$builder->FIND_ALL();

# Execute an INSERT, DELETE, UPDATE or SET query
$builder->QUERY();
```

#### Query Chains

Query chaining is where it all comes together and allows you to execute a query in the same syntax as if you were to write an SQL query statement. Here are some examples:
```php
# Check if an author is registered under a given email address
$email = $builder->SELECT('*')
         ->export();>FROM('users')
         ->WHERE('email', '=', 'example@email.com')
         ->FIND();

# Check if an author is registered under a given email address and username
$author = $builder->SELECT('*')
          ->export();>FROM('users')
          ->WHERE('username', '=', 'johndoe')
          ->AND_WHERE('email', '=', 'example@email.com')
          ->ROW();

# Check if an author is registered under a multiple email addresses using nested
# Or statements
$authors = $builder->SELECT('*')
           ->FROM('users')
           ->WHERE('email', '=', ['foo@bar.com', 'bar@foo.com'])
           ->FIND_ALL();

# Get all autors that are confirmed
$users = $builder->SELECT('*')
         ->FROM('users')
         ->WHERE('status', '=', 'confirmed')
         ->FIND_ALL();

# Get all of a post's tags
$tags = $builder->SELECT('tags.*')
        ->FROM('tags_to_posts')
        ->LEFT_JOIN_ON('tags', 'tags.id = tags_to_posts.tag_id')
        ->WHERE('tags_to_posts.post_id', '=', 2)
        ->FIND_ALL();

# Insert a row into the categories table
$insert = $builder->INSERT_INTO('categories')
        ->VALUES([
            'name' => 'JavaScript',
            'slug' => 'javascript'
        ])
        ->QUERY();

# Update a post status
$update = $builder->UPDATE('posts')
        ->SET(['status' => 'published'])
        ->WHERE('id', '=', 5)
        ->QUERY();

# Delete some posts
$delete = $builder->DELETE_FROM('posts')
        ->WHERE('created', '<', strtotime('-5 months'))
        ->OR_WHERE('modified', '<', strtotime('-1 year'))
        ->QUERY();
```

--------------------------------------------------------

### Table Management

The Builder class provides various methods to manipulate and interact with database tables. All the table management will return the Builder instance at hand, making them chainable.

The `CREATE_TABLE` method is used to create a table:

```php
$customPosts =
[
    'id'          => 'INTEGER | UNSIGNED | PRIMARY KEY | UNIQUE | AUTO INCREMENT',
    'created'     => 'INTEGER | UNSIGNED',
    'modified'    => 'INTEGER | UNSIGNED',
    'type'        => 'VARCHAR(255)',
];
$builder->CREATE_TABLE('custom_posts' $customPosts);
```

The `DROP_TABLE` method drops a table:
```php
$builder->DROP_TABLE('custom_posts');
```

The `TRUNCATE_TABLE` method truncates a table's columns:
```php
$builder->TRUNCATE_TABLE('custom_posts');
```

#### Alter

To start altering a table, use the `ALTER_TABLE` method:
```php
$table = $builder->ALTER_TABLE('custom_posts');
```

> The `Alter` class provides a number of helper methods to interact with the table at hand. The alter methods all return the working instance of the Alter class, making them chainable.

The `ADD_COLUMN` method adds a column to an existing table:
```php
$table->ADD_COLUMN('author_id', 'INTEGER | UNSIGNED');
```

The `DROP_COLUMN` method drops an existing column on an existing table:
```php
$table->DROP_COLUMN('author_id');
```

The `MODIFY_COLUMN` method can be used to set a column's data-type by providing a second parameter:
```php
$table->MODIFY_COLUMN('author_id', 'INTEGER | UNSIGNED | UNIQUE');
```

Or to set the working column for other methods by omitting it.
```php
$column = $table->MODIFY_COLUMN('author_id');
```

Once you have called the `MODIFY_COLUMN` method, the following column modification methods are made available:
```php
$column->ADD_PRIMARY_KEY();
$column->DROP_PRIMARY_KEY();
$column->ADD_NOT_NULL();
$column->DROP_NOT_NULL();
$column->ADD_UNSIGNED();
$column->DROP_UNSIGNED();
$column->SET_AUTO_INCREMENT();
$column->DROP_AUTO_INCREMENT();
$column->SET_DEFAULT($value = null);
$column->DROP_DEFAULT();
$column->ADD_UNIQUE();
$column->DROP_UNIQUE();
$column->ADD_FOREIGN_KEY($referenceTable, $referenceKey, $constraint = null);
$column->DROP_FOREIGN_KEY($referenceTable, $referenceKey, $constraint = null);
```

#### Foreign Keys

To set a foreign key constraint, use the `ADD_FOREIGN_KEY` method. The first parameter is the reference table, the second is the reference table's column name. The third parameter is optional and is used to set the name of the constraint. If omitted, a constraint name will be generated for you.
```php
$column->ADD_FOREIGN_KEY('users', 'id');
```

Dropping a foreign key constraint follows the same rules as the `ADD_FOREIGN_KEY` method.
```php
$column->DROP_FOREIGN_KEY('users', 'id');
```

#### Alter Chains
A simple chain starting from a Builder instance might look like this:
```php
$builder->ALTER_TABLE('custom_posts')->ADD_COLUMN('author_id', 'INTEGER | UNSIGNED');
```

A more complicated chain starting from a Builder instance might look like this:
```php
$builder->ALTER_TABLE('custom_posts')->ADD_COLUMN('author_id', 'INTEGER | UNSIGNED')->MODIFY_COLUMN('author_id')->ADD_FOREIGN_KEY('users', 'id');
```
