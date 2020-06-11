# Controllers

## What are controllers

Controllers are the main entry point of a route. All the routes, whenever
called, will load a controller method. Each controller has its own 
responsibility split into various methods. The idea is that a controller
will process your request, then load data from database and then finally
send the data to the view. How to use and structure your application
with Controllers is entirely up to you. We will also layout common
Controller tricks that are used in the industry.

## Making a controller via CLI

Use the X Framework CLI to make a new controller. We recommend following
a convention to make controller names. Although there is no need for a 
convention we recommend suffixing your controller with the keyword
Controller and the name should be in camel casing.

> php x

## Making a controller manually

Create a file **App/Controllers/ArticleController.php** and define a
PSR-4 Class.

**App/Controllers/ArticleController.php**

```php
namespace App\Controllers;

class ArticleController{

    public function index(){

        //

    }

}
```

By default all controllers are placed in **App/Controllers/** directory
and the namespace App\Controllers is prefixed in all routes. You can
edit the namespace in system configuration.

## Controller Techniques

### Resource Controllers

A resource controller defines basic bootstrapping for a CRUD Controller
around model. A resource controller can be created using CLI. It defines
CRUD functions that are frequently used with a Model. Here are the resource
controller functions.

1. **GET Index**

    This method handles the listing of a model. This will render the layout
    for rendering a list of all the models.

2. **GET View**

    This method handle the information display of a model. The method will
    render a view which displays the information about a single model.

3. **GET Create**

    This method displays a form to create a new model. The form will send
    data to the POST Store method. 

4. **POST Store**

    This method will create a new model using the request.

5. **GET Edit**

    This method displays a form to edit a model. The form will send data
    to PATCH Store method. But since PATCH is not supported by browsers,
    you can use POST too.

6. **PATCH Update**

    This method will update an existing model using the request.

7. **DELETE Destroy**

    This method will delete an existing model.

### Controller Parameters

Parameters of controller methods can be used get data from route (see route >
route parameters) or inject classes. X-Framework comes with an elegant 
dependency injection container. The example shows class injection.

```php
namespace App\Controllers;

use XFrames\Utility\{ Request, Session };

class ArticleController{

    public function index(Request $request, Session $session){

        dd($request);

    }

}
```

### Controller Rendering

You can return any object of interface type XFrames\Blueprints\Renderable 
from a controller method. The object will be automatically rendered on
to webpage. This is especially useful for building custom classes to 
handle responses like in APIs, you can have custom Transformer Classes
that can render data into JSON or XML as required. The rendering is
handled by Kernel.

Views implement XFrames\Blueprints\Renderable and hence can be returned 
to render themselves.