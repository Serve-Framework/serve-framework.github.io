# Validation

- [Access](#access)
- [Validation](#validation)
- [Filters](#filters)

--------------------------------------------------------

The Serve validator provides a simple and consistent way of validating user input.

--------------------------------------------------------

### Access

You can access a new Validator directly via the IoC container:
```php
$validator = $serve->Validator->create($fields, $rules);
```

--------------------------------------------------------

### Validation

First you'll need to define a set of rules that you want to validate your input against:
```php
$rules =
[
	'username' => ['required', 'min_length(4)', 'max_length(20)'],
	'password' => ['required'],
	'email'    => ['required', 'email'],
];
```

The rules defined above will make sure that the username, password and email fields are non-empty. That the username is between 4 and 20 characters long, and that the email field contains a valid email address.

> Note that most of the included validation rules will skip validation if the field is empty. 

Next you'll need to create a validator object. The first parameter is the input data you want to validate and the second is the set of validation rules you just defined:
```php
$fields = $serve->Request->fetch(); 

$validator = $serve->Validator->create($fields, $rules);
```

Now all that is left is to check if the input data is valid using either the `isValid` method or the `isInvalid` method:
```php
if($validator->isValid())
{
	// Do something
}
else
{
	// Display errors
}
```

Retrieving error messages is done using the `getErrors` method:
```php
$errors = $validator->getErrors();
```

The following validation rules are included with Serve:

| Name                     | Description                                                                                             |
|--------------------------|---------------------------------------------------------------------------------------------------------|
| alpha                    | Checks that the field value only contains valid alpha characters.                                       |
| alpha_dash               | Checks that the field value only contains valid alphanumeric, dash and underscore characters.           |
| alpha_space              | Checks that the field value only contains valid alphanumeric, dash and underscore characters.           |
| email                    | Checks that the field value is a valid email address.                                                   |
| email_domain             | Checks that the field value contains a valid MX record.                                                 |
| exact_length             | Checks that the field value is of the right length (`exact_length(20)`).                                |
| float                    | Checks that the field value is a float.                                                                 |
| greater_than             | Checks that the field value is greater than x (`greater_than(5)`).                                      |
| greater_than_or_equal_to | Checks that the field value is greater than or equal to x (`greater_than_or_equal_to(5)`).              |
| in                       | Checks that the field value contains one of the given values (`in(["foo","bar","baz"])`).               |
| integer                  | Checks that the field value is a integer.                                                               |
| ip                       | Checks that the field value is an IP address (`ip`, `ip("v4")` or `ip("v6")`).                          |
| json                     | Checks that the field value contains valid JSON.                                                        |
| less_than                | Checks that the field value is less than x (`less_than(5)`).                                            |
| less_than_or_equal_to    | Checks that the field value is less than or equal to x (`less_than_or_equal_to(5)`).                    |
| match                    | Checks that the field value matches an exact value (`match("foobar")`).                                 |
| max_length               | Checks that the field value is short enough (`max_length(20)`).                                         |
| min_length               | Checks that the field value is long enough (`min_length(10)`).                                          |
| not_in                   | Checks that the field value does not contain one of the given values (`not_in(["foo","bar","baz"])`).   |
| regex                    | Checks that the field value matches a regex pattern (`regex("/[a-z]+/i")`).                             |
| required                 | Checks that the field isn't empty.                                                                      |
| url                      | Checks that the field value is a valid URL.                                                             |

--------------------------------------------------------

### Filters

Filters allow you to filter user input after successful validation is run. Filters are defined as a third optional parameter to the validator constructor:


```php

$filters =
[
	'username' => ['string', 'lowercase'],
	'email'    => ['email'],
];

$validator = $serve->Validator->create($fields, $rules, $filters);
```

Once you have run validation you can filter the user input using the `filter` method:
```php
if($validator->isValid())
{
	$data = $validator->filter();
}
else
{
	// Display errors
}
```

> Note that all field keys do NOT need to be set. If a filter is set to a field that does not exist it will simply be skipped over. Similarly, if a filter is NOT defined for a field, the field value will be left untouched. 

The following filter rules are included with Serve:

| Name        | Description                                      |
|-------------|--------------------------------------------------|
| boolean     | Filters the field value to `boolean`             |
| email       | Filters the field value to a valid email address |
| html_decode | HTML decodes the field value                     |
| html_encode | HTML encodes the field value                     |
| integer     | Filters the field value to an `integer`.         |
| json        | Filters the field value as `JSON` to an `array`  |
| lowercase   | Filters the field value to lowercase             |
| numeric     | Filters out all non numeric field values         |
| string      | Filters the field value to a valid string        |
| strip_tags  | Strips HTML tags from the field value            |
| trim        | Trims whitespace from the field value            |
| uppercase   | Filters the field value to uppercase             |
| url_decode  | URL decodes the field value                      |
| url_encode  | URL encodes the field value                      |
