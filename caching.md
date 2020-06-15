# Caching

Caching is a very important thing to have in an application. Caching is currently
supported by FileSystem, but better caching drivers will be added later. Lets,
understand the base concept of caching and how it is being used in the web
today. Then we will look at the X Framework implementation of caching with
helper functions and classes. Unlike other parts of the framework, the helper
function is not the same as the class, rather it provides an easy to use 
implementation of the Caching Class. If you looking to build complex caching
systems, then using the class will be better, but the helper function will
provide you with the necessary caching.

## Why do we need to cache

Talking to database is very expensive. Just starting a connection with the
database takes time and running a long query which maybe returns 100s-1000s
of records each second can slow your database server and website server.
Lets say there is a database request for data that takes 1s to complete.
If we implemented caching, the database request will be called for only the
first time. The subsequent requests will be served from file system. So yes,
the request will be called the first time but all the subsequent request will
be way faster since they load the data directly rather than using the database.

## Caching in the Modern Web

There are alot of services that provide caching tools like redis, memcache etc.
They are even faster than the file system. MemCache uses ram to store cache.
It was estimated that facebook was using 28 Terabytes of RAM with MemCache in
the year 2012.

## Cache Helper

Cache helper can be used to cache a single object.

Syntax
```php
cache($name, $timeToLive, $callback);
```

Example
```php
$takesVeryLongToProcess = cache("sum", 60, function(){

    sleep(5);

    return 100 + 1200;

});
```


## Cache Class

Cache class should be used to define custom caching functionality. To cache
a simple variable, please use the cache helper.

Example usage
```php
use XFrames\Cache\Cache;

if(Cache::valid("x")){
    $x = Cache::get("x");
}else{
    $x = 100 * 100;
    Cache::set("x", $x);
}
```

#### set($name, $value, $timeToLive)

This function saves the value into the name entry. The cache will remain
alive for timeToLive which is defined in seconds.

#### get($name)

This function will get any value, if it is stored in that name. If name entry
is not available, then it will throw a CacheNotFound exception.

#### has($name)

This function returns true if the name entry exists on the server.

#### valid($name)

This function returns true only if the name entry exists on the server
and is still alive or has time to live.

#### expired($name)

This function returns true only if the name entry exists on the server
and is not alive or has no time to live. In other words, this name can
be safely deleted.
