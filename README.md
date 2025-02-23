Usage Documentation for Router Class

The `Router` class is used to define and manage routes in a PHP application.
It allows you to define routes for different HTTP methods and generate URLs based on these routes.

Example usage:

```php
use DvTeam\Routing\Router;
use Psr\EventDispatcher\EventDispatcherInterface;

$router = new Router(
    webRoot: '/',
    httpHost: 'example.org', // From $_SERVER['HTTP_HOST']
    isHttps: true, // From $_SERVER['HTTPS']
    $dispatcher // A EventDispatcherInterface instance
);

// Define routes
$router->get(
    alias: 'home.index',
    urlPart: 'home',
    callable: [HomeController::class, 'index']
);

$router->get(
    alias: 'product.details',
    urlPart: 'details',
    callable: [ProductController::class, 'showDetails'],
    params: ['id']
);

$router->post(alias: 'submit', urlPart: 'submit', callable: [FormController::class, 'submit']);

// Generate a URL for a route
$url = $router->linkTo(['alias' => 'home']);
echo $url; // Outputs: https://example.org/home

// Generate a URL for a controller/method pair
$url = $router->linkTo(['@target' => [ProductController::class, 'showDetails'], 'id' => 123]);
echo $url; // Outputs: https://example.org/details/123

// Enter a context
$result = $router->enterContext(['alias' => 'product.details'], function(Router $router) {
    // Your code here
    return $router->linkToSelf(['id' => 123]);
});
echo $result; // Outputs: https://example.org/details/123
```