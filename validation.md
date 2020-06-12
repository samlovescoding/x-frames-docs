# Requests & Validation

Request data can be accessed using the class or the helper function.
Here are the two ways.

**1. Request using class (with Dependency Injection)**
```php
namespace App\Controllers;

use XFrames\Utility\Request;

class ProductController{

    public function index(Request $request){

        dd($request);

    }
}
```

**2. Request using helper function**
```php
namespace App\Controllers;

class ProductController{

    public function index(){

        dd(request());

    }
}
```

## Request Methods

#### Getting Data

All the request data can be accessed using class member data.
For an example, lets say you have title in request, you can
access it using the following.
```php
class ProductController{

    public function index(){

        echo request()->title;

    }
}
```

#### has($key)

You can verify if some data exists in request using has method.
To be on the good side.

```php
class ProductController{

    public function index(){

        if(request()->has("title")){

            echo request()->title;

        }

    }
}
```

#### only(...$keys)

Furthermore you can get a dictionary of the keys in a request
using only method.

```php
class ProductController{

    public function index(){

        $request = request()->only("id", "title", "body");

        dd($request);

    }
}
```

#### validate($rules)

X Framework allows you to validate a request using the validate
function. We will discuss defining the rules later in this post.
The function itself returns only the validated data as a
dictionary.

```php
class ProductController{

    public function index(){

        $validatedRequest = request()->validate([
            "title" => "is_required|is_min:6",
            "slug" => "is_required|is_max:32",
            "body" => "is_required"
        ]);

        dd($validatedRequest);

    }
}
```

#### passed()

This method returns true when the requests passes validation.

```php
use App\Product;

class ProductController{

    public function index(){

        $validatedRequest = request()->validate([
            "title" => "is_required|is_min:6",
            "slug" => "is_required|is_max:32",
            "body" => "is_required"
        ]);

        if(request()->passed()){

            $product = new Product;

            $product->create($validatedRequest);

        }

        redirect("/products");

    }
}
```

#### failed()

This method returns true only when the request fails validation.
It is opposite of passed() method.

#### file($key)

This method returns an instance of UploadedFile if there is a file
that has been uploaded. We will dicuss this later in File Systems.

# Validation Rules

Validation is done by defining rules. Usually any dictionary can be
validated. Validation rules are generally passed as values. 
They can also have parameters. Like rule for minimum length can have
a parameter which defines the threshold for minimum length. Multiple
rules can be seperated using pipe "|" symbol.

```php
request()->validate([
    "title" => "is_required|is_alpha"
]);
```

The rules parameters can be defined using colon ":" symbol.

```php
request()->validate([
    "title" => "is_required|is_min:6|is_max:32"
]);
```

If a rule requires multiple parameters, you can seperate them
using a comma.

```php
request()->validate([
    "title" => "is_required|is_min_max:6,32"
]);
```

## List of validation rules

```php 
is_required()
```

```php
is_min($minimum)
```

```php
is_max($maximum)
```

```php
is_min_max($minimum, $maximum)
```

```php
is_alpha()
```

```php
is_num()
```

```php
is_alpha_num()
```

```php
is_file()
```

```php
has_filesize($bytes)
```

```php
is_image()

//Currently supported types for images are png, jpg, jpeg, gif, svg.
```

```php
has_extension($extension)
```
