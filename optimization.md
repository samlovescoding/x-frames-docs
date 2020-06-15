# Optimizing Application

Although

## Router

Using action method is very expensive. But that can be fixed by defining most used routes
through actions at the top of the file. Since traversing the route list is linear, if 
the a route is defined at the top, the action loop will stop earlier. Although this is
not a problem with small application, it can turn into serious problem with 1000+ routes
where action method is trying to traverse 1000 route list everytime it is used.
Another completely inexpensive way is to not use it at all. Think about it, you really
dont need it and we though about it too. It is just a quality of life function which
makes it easier for the developers to understand code.

```php
$url = action("ArticleController@index", ["page" => 5]);

// It can be written using a string

$url = "/articles?page=5";
```

## Caching expensive processing

Caching can be used for anything. See its documentation for more.