# Response

--------------------------------------------------------

- [Access](#access)
- [Status](#status)
- [Headers](#headers)
- [Body](#body)
- [Format](#format)
- [Protocol](#protocol)
- [Cache](#cache)
- [Helpers](#helpers)

--------------------------------------------------------

The response service is an abstraction of Serve's `HTTP` response that is returned to the `HTTP` client. An instance of the response object is always available in via the IoC container.

The response class provides helper methods, that help you interact with these HTTP response properties. The default response object will send a 200 OK HTTP response with the text/html content type.

--------------------------------------------------------

### Access

You can access the Response object directly through the IoC container:

```php
$response = $serve->Response;
```

--------------------------------------------------------

### Status

The `status` method returns the status object which is used for managing the response status.

```php
$status = $serve->Response->status();
```

The `set` method sets the status code:
```php
$status->set(200);
```

The `get` method returns the currently set status code:
```php
$code = $status->get();
```

The `message` method returns the message for the current code.
```php
$message = $status->message();
```

To validate the response status, use one of the built in helper methods:
```php
$status->isEmpty();
$status->isInformational();
$status->isOk();
$status->isSuccessful();
$status->isRedirect();
$status->isForbidden();
$status->isNotFound();
$status->isClientError();
$status->isServerError();
```

--------------------------------------------------------

### Headers

The `headers` method returns the Headers object which is used to manage response headers:
```php
$headers = $serve->Response->headers();
```

To set a header, use the `set` method:
```php
$headers->set('Keep-Alive', 'timeout=5, max=100');
```

To set multiple headers at once, use the `setMultipl`e method with an array:
```php
$headers->setMultiple([
    'Keep-Alive' => 'timeout=5, max=100',
    'Date'       => date('c'),
]);
```

The `get` method returns a header value by key or `false` if it doesn't exist:
```php
$keepAlive = $headers->get('Keep-Alive');
```

The `has` method checks if a header exists:
```php
if ($headers->has('Keep-Alive'))
{
    # Do something here
}
```

The `remove` method removes a header by key:
```php
$headers->remove('Keep-Alive');
```

The `clear` method removes all the headers:
```php
$headers->clear();
```

You can also iterate the response headers as an array:
```php
foreach ($headers as $k => $v)
{
    # Do something here
}
```

The `asArray` method returns an associative array of all the headers:
```php
$headerArray = $headers->asArray();
```

The `sent` method checks if the headers have been sent:
```php
if ($headers->sent())
{

}
```

The `send` method immediately sends the headers. Note that this is handled internally by the `Response` object.
```php
$headers->send();
```
--------------------------------------------------------

### Body

The `body` method returns the response body which is used to manage the response body:
```php
$body = $serve->Response->body();
```

To set the body, use the `set` method:
```php
$body->set('Hello World!');
```

To get the body, use the `get` method:
```php
$html = $body->get();
```

The `append` method appends a string to the body:
```php
$body->append('foo');
```

The `clear` method clears the body:
```php
$body->clear();
```

The `length` method returns the body length:
```php
$length = $body->length();
```
--------------------------------------------------------

### Format

The `format` method returns the response format which is sent as a header:
```php
$format = $serve->Response->format();
```

To set the response format, use the `set` method with a valid mime type:
```php
$format->set('text/html');
```

You can pass a file extension instead, the method will automatically convert it to the correct mime type:
```php
$format->set('json');
```

The `setEncoding` method sets the format encoding:
```php
$format->setEncoding('UTF-8');
```

The `get` method returns the current format as a mime type:
```php
$mime = $format->get();
```

The `getEncoding` method returns the current format encoding:
```php
$encoding = $format->getEncoding();
```

--------------------------------------------------------

### Protocol

The `protocol` method returns the Protocol object which is used to set the response protocol:
```php
$protocol = $serve->Response->protocol();
```

To set the response protocol, use the `set` method:
```php
$protocol->set('https');
```

To get the response protocol, use the `get` method:
```php
$proto = $protocol->get();
```

The `isSecure` method checks if the response will sent over SSL:
```php
if (!$protocol->isSecure())
{
    # No https
}
```

--------------------------------------------------------

### Cache

The response has built in support for native cleint-side HTTP caching.

The idea here that an Etag is sent on responses, the browser then stores that content. On subsequent requests, the browser will send the Etag to the server. If the page hasn't changed and the Etag matches the current one a `304 Not Modified` response is sent.


The `enableCaching` method enables the response cache:
```php
$response->enableCaching();
```

The `disableCaching` method disables the response cache:
```php
$response->disableCaching();
```

The `isCacheable` method checks if the response can be cached:
```php
if ($response->isCacheable())
{
    # Do something
}
```

If caching is disabled, the following header will be sent:
```
Cache-Control : no-store, no-cache, must-revalidate, max-age=0

```

If caching is enabled, the following header will be sent:
```
Cache-Control : no-store, no-cache, must-revalidate, max-age=3600

ETag : 'hashed response body'
```


--------------------------------------------------------

### Helpers

The Response object provides a number of helper methods to quickly send common responses.

The `send` method immediately sends the response to the HTTP cleint. It will also filter the response via the [CDN](#/learn-more/CDN) (if enabled) and load the body from the cache (if enabled).

> Note that the sending of the response is handled by the framework internally. You should not need to send the response manually under normal circumstances.

```php
$response->send();
```

> The following methods all throw request exceptions. Please see the [Throwing Exceptions](/getting-started/error-handling#throwing-exceptions) section for more details on output.

The `redirect` method immediately sends an HTTP redirect (302) response to the client and halts the application. No code will be executed after `redirect` is called.
```php
$response->redirect('http://example.com/moved');
```

The redirect method uses a temporary 302 status code by default. You can set a different redirect code via a second parameter:
```php
$response->redirect('http://example.com/moved-permanently', 304);
```

The `notFound` method immediately sends an HTTP not found (404) response to the client and halts the application. No code will be executed after `notFound` is called.
```php
$response->notFound();
```

The `forbidden` method immediately sends an HTTP forbidden (403) response to the client and halts the application. No code will be executed after `forbidden` is called.
```php
$response->forbidden();
```

The `invalidToken` method immediately sends an HTTP Invalid Token (498) response to the client and halts the application. No code will be executed after `invalidToken` is called.
```php
$response->invalidToken();
```

The `methodNotAllowed` method immediately sends a HTTP Method Not Allowed (405) response to the client and halts the application. No code will be executed after `methodNotAllowed` is called.
```php
$response->methodNotAllowed();
```
