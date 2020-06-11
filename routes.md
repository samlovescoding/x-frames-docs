# Routes

Routes are integral to your application and they define 
how people are going to use your website. X Framework 
looks for all the routes in **routes.php** and that is where
all your routes should be defined. Although, there are
so many other places to define routes, we are going to
learn about a few of them later in this article.

## HTTP Verbs

X Framework allows you to define routes under several
request methods. We refer to these request methods as
HTTP Verbs. A few of them you have used before in your
PHP applications. They are GET and POST. But X Framework
allows you to have 6 different types of HTTP Verbs. We define
these verbs to be compatible with a REST approach. These
are the HTTP Verbs usable in X Framework - 

- GET
- POST
- PUT
- PATCH
- DELETE
- OPTIONS

By default browsers can only send GET and POST request
without using a JavaScript Fetch API or a better HTTP
framework.

## Defining Routes

To define a route, goto **routes.php** and add a new line

```php
Route::get("/", "HomeController@welcome");
```

The first parameter is the URL which your website users will
visit, and the second parameter is Controller and Method that
will be called when someone visits the website.

Here are all other types of routes that you can define.

```php
Route::post("/create-article", "HomeController@handle");

Route::put("/add-article-comment", "HomeController@put");

Route::patch("/update-article", "HomeController@patch");

Route::delete("/delete-article", "HomeController@delete");

Route::options("/articles", "HomeController@options");
```

## Routing Techniques

### Route Overloading

You can overload routes in X Framework. Lets assume we have
product edit form. Atleast 2 routes are required i.e. a
GET route and a POST route. We can overload a URL to handle
form display and form submit. Here is an example:

```php
Route::get("/product/edit", "EditProductController@form");

Route::post("/product/edit", "EditProductController@handle");
```

Whenever the route "/product/edit" is accessed via GET request,
the *form* method will be called and when the same route is
access via POST request, the *handle* method will be called.

### Route Parameters

Every good router supports simple route parameters. All the
route parameters are passed on as strings. Here is how you
can add route parameters to your application.

**routes.php**
```php
Route::get("/product/:product", "ProductController@view");
```
Here we pass a parameter called product. Notice the colon
before the variable name.

**App/Controllers/ProductController.php**

```php
namespace App\Controllers;

class ProductController{

    public function view($product){

        echo $product;

    }

}
```

The product parameter from the router is passed as the product
parameter of the controller.

**:product parameter will only be passed as $product parameter.**

### Route Injections

Route injection makes app development faster and seamless. Most
of the X Framework library can be injected. Here is how we can
load Str class in the previous ProductController example.

```php
namespace App\Controllers;

use XFrames\Utility\Str;

class ProductController{

    public function view($product, Str $string){

        echo $string->camelCase($product);

    }

}
```