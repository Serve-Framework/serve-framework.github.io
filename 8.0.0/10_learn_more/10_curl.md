# Curl

--------------------------------------------------------

- [Usage](#usage)

--------------------------------------------------------

The `Curl` utility can be used to send requests to an external endpoint for processing.

--------------------------------------------------------

### Usage

The `post` method sends a `POST` request:
```php
use serve/utility/Curl;

$mime = Curl::post(string $url, array $headers = [], array $postData = [], $auth = '', string $contentType = 'application/json', $accept = 'application/json')
```

The `get` method sends a `GET` request:
```php
use serve/utility/Curl;

$mime = Curl::get(string $url, array $_options = [], array $headers = [], array $queries = [], $auth = '', string $contentType = 'text/html', $accept = 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3')
```