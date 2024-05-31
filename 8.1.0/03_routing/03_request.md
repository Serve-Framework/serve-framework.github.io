# Request

--------------------------------------------------------

- [Access](#access)
- [Usage](#usage)
- [Environment](#environment)
- [Headers](#headers)
- [Files](#files)

--------------------------------------------------------

Serve's `Request` service serves an abstraction of the current `HTTP` request and allows you to easily interact Serve's `HTTP` request methods.

In other words, the `Request` service is a wrapper class for validating and/or interacting with the incoming `HTTP` request.

--------------------------------------------------------

### Access

You can access the `Request` object directly through the IoC container:

```php
$request = $serve->Request;
```

--------------------------------------------------------

### Usage

The `getMethod` method returns the current HTTP request method:
```php
$method = $request->getMethod();
```

To validate the request method, use one of the built in helper methods:

```php
$request->isGet();
$request->isPost();
$request->isPut();
$request->isPatch();
$request->isDelete();
$request->isHead();
$request->isOptions();
$request->isAjax();
$request->isFileGet();
```

To check if the request was sent over SSL, use the `isSecure` method:
```php
if (!$request->isSecure())
{
    // Not secure
}
```

Use the `fetch` method to get data from the incoming request:
```php
// Returns $_POST data on POST requests
$postForm = $request->fetch();

// Returns $_GET data on GET requests
$data = $request->fetch();
```

If you want to fetch a specific part of the requested URL, you can do it easily by providing a key:
```php
# Request url = http://www.example.com/foo?bar

# Returns 'bar'
$request->fetch('query');
```

If you need more data, simply omit the key:

```php
# Request url = http://example.com/foo?bar=foo

$data = $request->fetch();

# returns
[
  'scheme' => 'http',
  'host'   => 'example.com',
  'port'   => 8888,
  'path'   => '/foo',
  'query'  => 'bar=foo',
  'page'   => 0
];
```

The `fetch` method is useful when you need to validate a `POST` request:
```php
$serve->Router->post('/my-page/', function()
{
    $input = $request->fetch('form_input');

    if ($input)
    {
        # Do something
    }
});
```

Or when you need to validate the requested URL:
```php
$serve->Router->get('/my-page/', function()
{
    $path = $request->fetch('path');

    if ($path)
    {
        # Do something
    }
});
```

Use the `queries` method to get an associative array of the requested URL queries:
```php
# Request url = http://example.com/foobar?token=foo&key=bar

$queries = $request->queries();

# returns
[
'token'   => 'foo',
'key'     => 'bar',
];
```

If you want a specific value by key, provide a key. If the key is not found in the URL query, `FALSE` is returned.
```php
# Request url = http://example.com/foobar?token=foo&key=bar

$token = $request->queries('token');

# $token = 'foo';
```

> The `queries` method will automatically parse the keys and values through PHP's `urldecode` method.

--------------------------------------------------------

### Environment

The request environment provides a consistent interface to the incoming `HTTP` request environment.

The class provides an application-wide abstraction of PHP's `$_SERVER` superglobal.

To access the environment, use the environment method.

```php
$environment = $request->environment();
```

You can access the properties with PHP's magic methods:
```php
$root = $request->environment()->DOCUMET_ROOT;
```

To get all the properties as an associative array, use the `asArray` method:
```php
# request = http://example.com/foobar

$env = $request->environment()->asArray();

# Returns
[
  'REQUEST_METHOD'  => 'GET',
  'SCRIPT_NAME'     => 'index.php',
  'SERVER_NAME'     => 'example.com',
  'SERVER_PORT'     => '8888',
  'HTTP_PROTOCOL'   => 'http',
  'DOCUMENT_ROOT'   => '/usr/name/httpdocs',
  'HTTP_HOST'       => 'http://example.com',
  'DOMAIN_NAME'     => 'example.com',
  'REQUEST_URI'     => '/foobar',
  'REQUEST_URL'     => 'http://example.com/foobar',
  'QUERY_STRING'    => '',
  'REFERER'         => '',
  'REMOTE_ADDR'     => '192.168.1.1',
  'HTTP_USER_AGENT' => 'Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.17 (KHTML, like Gecko) Chrome/24.0.1312.57 Safari/537.17',
];
```

--------------------------------------------------------

### Headers

The request headers provides a consistent interface to the incoming HTTP request headers. The object provides an application-wide abstraction of PHP's `$_SERVER` superglobal with the headers.

To access the headers, use the headers method.
```php
$headers = $request->headers();
```

You can access the properties with PHP's magic method:
```php
$mime = $headers->HTTP_ACCEPT;
```

To get all the properties as an associative array, use the `asArray` method:
```php
$headers = $headers->asArray();
```

> All request headers are normalised with `HTTP_` prefix.

The `acceptableContentTypes` method returns an array of acceptable content types in descending order of preference.
```php
$contentTypes = $headers->acceptableContentTypes();
```

The `acceptableLanguages` method returns an array of acceptable languages in descending order of preference.
```php
$languages = $headers->acceptableLanguages();
```

The `acceptableCharsets` method returns an array of acceptable character sets in descending order of preference.
```php
$charsets = $headers->acceptableCharsets();
```

The `acceptableEncodings` method returns an array of acceptable encodings in descending order of preference.
```php
$encodings = $headers->acceptableEncodings();
```

--------------------------------------------------------

### Files
The files object is a handy way of validating and retrieving incoming file uploads.

To access the files object, use the `files` method.
```php
$files = $request->files();
```

The `get` method returns an array of file arrays or a single file array if specified:

```php
# Get all files
$uploads = $files->get();

# Get files under the 'foo' key
$upload = $files->get('foo');

# If you're fetching a file from a multi upload then you'll have to include the key as well
$upload = $files->get('foo.0');
```