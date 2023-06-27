## Como utilizar o RestApiClient

```php
require_once 'RestApiClient.php';
$rest = new RestApiClient();
```

### GET

Requisição GET sem o token de acesso ao backend

```php
$rest->get('user');
```

Requisição GET com query string

```php
$rest->get('user', ['id' => 1]);
```

Requisição GET com o token de acesso ao backend e sem query string

```php
$response = $rest->get("user", [], $token);
```

Requisição GET com o token de acesso ao backend e com query string

```php
$response = $rest->get("user", ['id' => 1], $token);
```

### POST

Requisição POST sem o token de acesso ao backend

```php
$rest->post('user', ['name' => 'Victor']);
```

Requisição POST com o token de acesso ao backend

```php
$response = $rest->post("user", ['name' => 'Victor'], $token);
```

### PUT

Requisição PUT sem o token de acesso ao backend

```php
$rest->put('user/1', ['id' => 1, 'name' => 'Victor']);
```

Requisição PUT com o token de acesso ao backend

```php
$response = $rest->put("user/1", ['id' => 1, 'name' => 'Victor'], $token);
```

### DELETE

Requisição DELETE sem o token de acesso ao backend

```php
$rest->delete('user/1');
```

Requisição DELETE com o token de acesso ao backend

```php
$response = $rest->delete("user/1", $token);
```

### Response

O response passa por dois tratamentos (handleResponse e handleException) antes de ser retornado, o handleResponse trata o response de sucesso e o handleException trata o response de erro.

handleResponse

```php
private function handleResponse(ResponseInterface $response)
  {
    $statusCode = $response->getStatusCode();
    $body = $response->getBody()->getContents();
    $data = json_decode($body, true);

    if ($statusCode >= 200 && $statusCode < 300) {
      return $data;
    } else {
      return $data['mensagem'] ?? 'An error occurred';
    }
  }

```

handleException

```php
  private function handleException(RequestException $e)
  {
    $response = $e->getResponse();
    $statusCode = $response ? $response->getStatusCode() : null;

    if ($statusCode === 401) {
      return 'Unauthorized';
    } elseif ($response) {
      $body = $response->getBody()->getContents();
      $data = json_decode($body, true);

      if (isset($data['mensagem'])) {
        return $data['mensagem'];
      }
    }

    return 'An error occurred';
  }
```
