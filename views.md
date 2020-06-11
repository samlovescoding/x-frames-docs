# Views

## Introduction

A view is simply a web page, or a page fragment, like a header,
footer, sidebar, etc. In fact, views can flexibly be embedded 
within other views (within other views, etc., etc.) if you need
this type of hierarchy.Views are never called directly, they
must be rendered. Remember that in an MVC framework, the 
Controller acts as the traffic cop, so it is responsible 
for fetching a particular view. If you have not read the 
Controllers page you should do so before continuing.

## Problem

Views are generally your template code containing PHP files 
and it is very easy to fall back to spaghetti PHP code which
can ruin your appetite for software development when using 
views. Hence we dont recommend any sort of processing in your
view files. This problem can be fixed by using a template engine in which
writing PHP code will be impossible, but the overhead that a
template engine generates on a request is very large compared
to the PHP. Hence we have decided not to use any template
engine, rather provide Views API via helper functions.

## Using Views

All views must be located in App/Views directory and whenever
using them in a controller, they must be returned so that
kernel can render them. Lets use a welcome view in HomeController.
There is no need to write .php at the end of the file name.
By convention, the names of the view files should be camel cased
or kebab cased.

**App/Controller/HomeController.php**
```php
namespace App\Controllers;

class HomeController{

    public function index(){

        return view("welcome");

    }

}
```


**App/Views/welcome.php**
```html
<h2>Welcome!</h2>
```

## View Imports

Views can be imported into each other. It means that you can
import any other view into welcome view. Lets see it in action.

```html
<h2>Welcome!</h2>
<?php import("some-other-view"); ?>
```

## View Parameters

You can pass data from a controller to view or from one view to
another using View Parameters. Simply pass a dictionary as second
parameter to be used as view paramters.


**App/Controller/HomeController.php**
```php
namespace App\Controllers;

class HomeController{

    public function index(){

        return view("welcome", [
            "name" => "Sam"
        ]);

    }

}
```


**App/Views/welcome.php**
```html
<h2>Welcome back, <?=$name?></h2>
```
