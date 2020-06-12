# Authentication

X Framework provides an elegant API that makes authentication very
easy. It also can also perform authorization too. But before see the
API of these two, we must clearly define what authentication and
authorization is.

**Authentication**

In general, it is the process of verifying if something is authentic or is 
not fake. In Computer Softwares, we use authentication to see if we can
trust a guest to be an actual user. So the guest must be tested and should
pass actual tests and present proofs that they are the real 'authentic' user
and not a fake imposter. Authentication is again more briefly known as 
'Logging in a User'. Another thing that authentication must handle is 
registration of a guest. Generally any guest should be able to register,
but not using somebody else's identity. This process, known as registration
is also an important part of Authentication.

**Authorization**

Authorization is the process of verifying if a user is authorized to perform
a specific task or in simpler words if a user can perform a specific task.
Authorization should also handle if a user is trying to access restricted
content. To be more explanative, example of authorization are 
1. If a user can edit an article
2. If a user can delete another user
3. If a user can add comment
It is a completely different process from authentication.

**Our implementation**

X Framework has an authentication system that can also handle basic authorization
tasks. Dont confuse betweem the two. We handle authorization under authentication
name because the API that we provide for authorization is too small to be something
of its own. Although we will be updating in the future. Furthermore we do not 
provide an API to register a user, since it is the same as creating a model of the
same.

## Loading Authentication

Authentication can be accessed using the class or the helper function.
Here are the two ways.

**1. Authentication using class (with Dependency Injection)**
```php
namespace App\Controllers;

use XFrames\Library\Authentication;

class OrderController{

    public function index(Authentication $auth){

        dd($auth);

    }
}
```

**2. Authentication using helper function**
```php
namespace App\Controllers;

class OrderController{

    public function index(){

        dd(auth());

    }
}
```

## Login and Logout

Logging a user can be handled by the Authentication API. Use the
```remember``` function to remember a model using session.

```php
namespace App\Controllers\Authentication;

use App\User;

class LoginController{

    public function handle(User $user){

        $request = request()->validate([
            "username" => "is_required",
            "password" => "is_required"
        ]);

        if(request()->passes()){

            $user = User::where($request)->fetch();

            if($user != null){

                auth()->remember($user);

            }

        }

        redirect("/login");

    }
}
```

Logout can be performed by using ```forget``` which forgets the user.

```php
namespace App\Controllers\Authentication;

use App\User;

class LogoutController{

    public function handle(User $user){

        auth()->forget();

        redirect("/login");

    }
}
```

Checking if there is an authentication user can be performed
using the ```check``` which returns a bool depending if there
is an authentication user or not. It returns true if there
is a user authenticated.

```php
if(!auth()->check()){
    redirect("/login");
}
```

## Unauthorized Route

As you can see that there is a lot of redirecting going on. We
fix that by having a single point where of authentication
failure is redirected to and we call it the unauthorized route.
You can set it in routes.php and we have one preset for you.

```php
Authentication::setUnauthorizedRoute("/login");
```

It allows us to use the ```require``` function on authentication, which
automatically redirects if the user is not authentication.

```php
namespace App\Controllers\Authentication;

use App\User;

class ArticleController{

    public function edit(Product $product){

        auth()->require();

        // Do the controller stuff
        // Because it will automatically redirect
        // to the unauthorized route if there is 
        // no user.

    }
}
```

## Authorization

#### Concept

X Framework provides simple policy based authorization. You can
create a model policy using CLI. In your controller you can 
check if the user can perform a task on a model. This concept really
shines in a code.

#### Authorizing tasks

You can authorize tasks to a user using can method. Make sure that
there exists a policy for the model and the policy is saved in the
configuration.

```php
namespace App\Controllers\Authentication;

use App\User;

class ArticleController{

    public function edit(Product $product){

        auth()->can("edit", $product);

    }
}
```

It will check if the authenticated user can edit the model. If it is
false, then we redirect to the unauthorized route. Otherwise, we are
good to go. Lets take a look at Product Policy.

```php
namespace App\Policies;

use App\Models\Product;
use App\Models\User;

class ProductPolicy{

    public function create(Product $product, User $user){

        return true;
        // Since every user can create a product.

    }

    public function edit(Product $product, User $user){

        if($product->seller == $user->id){
            // Since only  seller can edit a product

            return true;

        }

        return false;
        

    }

    public function view(Product $product, User $user){

        return true;
        // Since every user can view a product.

    }

    public function delete(Product $product, User $user){

        if($product->seller == $user->id){
            // Since only  seller can edit a product

            return true;

        }

        return false;

    }

}
```

Whenever we call the can method on authentication, it check
for the method in the policy of a model.

The policy must be registered in configuration to work.