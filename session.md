# Session

Session data can be accessed using the class or the helper function.
Here are the two ways.

**1. Session using class (with Dependency Injection)**
```php
namespace App\Controllers;

use XFrames\Utility\Session;

class ProductController{

    public function index(Session $session){

        dd($session);

    }
}
```

**2. Session using helper function**
```php
namespace App\Controllers;

class ProductController{

    public function index(){

        dd(session());

    }
}
```

## Session Methods

#### get($key)

It returns the data stored in session.

```php
$lastPageRoute = session()->get("lastPageRoute");
```

#### set($key, $value)

It saves the data in session. This data is persisted
through subsequent requests from the user. You can
persist basic as well as complex data types in PHP.

```php
session()->set("lastPageRoute", "/articles");
```

#### flash($key)

It return the data stored in a session and then deletes
it from the session. It is useful for validation messages.

```php
echo session()->flash("lastPageRoute");
```

#### delete($key)

It deletes data from the session. The deleted data will not
be available after the function call.

```php
session()->delete("lastPageRoute");
```

#### flush()

It deletes all the data from the session, but new data can
still be added to the session.

```php
session()->flush();
```

#### destroy()

It deletes all the data from the session and destroys the
session. The session cannot be used after the destroy keyword.
To use session after reset, you must start it again using
start method.

```php
session()->destroy();
```

#### start()

It starts the session.

```php
session()->start();
```

