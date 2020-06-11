# Quick Guide

In this tutorial we will build a simple blog application
using XFrames. We will start with defining routes and then
writing an Article model. Then we will build the necessary
view files and finally, we will build the controller. We will
also see how to work with stylesheets and javascript.

## Installation

To begin with, lets create an empty project.

> composer create-project samlovescoding/x myblog

Lets install the front-end dependencies.

> cd myblog && npm install

Lets start the server.

> npm run serve

## Defining Routes

Routes are an integral part of your application and to serve
any request, you need to define a route. This ensures safety
and security across all requests. Routes are inspired by
laravel. All the routes definitions must be loaded in 
*routes.php*. We can define all types of RESTful routes here. 
Lets define a get route.

```php
Route::get("/", "ArticleController@index");
```

The first parameter defines the request endpoint "/". The
second parameter defines the name of the controller and 
method to call. Notice that the controller and method are
seperated by @. ArticleController is the name of Controller
and index is name of the method. 

Lets define a route with parameters.

```php
Route::get("/articles/:article", "ArticleController@view");
```

In this route we have a route parameter which will be
available as a parameter on called method that is
view method of ArticleController. All the route parameters
begin with : and are available as parameters on your
controller methods. We will create the ArticleController 
later in this tutorial.

Here are all the routes in the *routes.php* we need for
our blog application.

```php
use XFrames\Utility\Route;

// List all blog articles
Route::get("/", "ArticleController@index");

// Create a blog article
Route::get("/articles/create", "ArticleController@create");
Route::post("/articles/create", "ArticleController@store");

// View a blog article
Route::get("/articles/:article", "ArticleController@view");

// Edit a blog article
Route::get("/articles/:article/edit", "ArticleController@edit");
Route::post("/articles/:article/edit", "ArticleController@update");

// Delete a blog article
Route::get("/articles/:article/delete", "ArticleController@destroy");
```

## Configuration

Lets configure our application to support the database. Open
*App\Configuration\Database.php* and edit the host, username,
password and database name as per requirements. Now, run
the following command in your MySQL terminal.

```sql
CREATE TABLE article(
    id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100),
    slug VARCHAR(100),
    body TEXT,
    created_at DATETIME,
    updated_at DATETIME
);
```

This is create a table called article with the columns.

## Defining Model

Since our database is set-up, we can move forward and define
a model. The model will act as an intermediary between the
controller and the database. That being said, the model can
be accessed anywhere throughout the application.

Lets create an Article model by creating a class in
*App/Models/Article.php*. The class can be defined as.

```php
namespace App\Models;

use XFrames\Database\Model;

class Article extends Model{

}

```

As for now, our model is created. I know that the class is
empty but it has alot of power in it. Now its time to build
our views.

## Defining Controller

Here we bootstrap our controller and at this point the router
should be able to call the controller. We have defined the
basic behavior of the controller and we will come back to it
later on. Lets define it in **App/Controller/ArticleController.php**.

```php

namespace App\Controllers;

class ArticleController{
    
    public function index(){

        return view("articles/index");

    }

    public function create(){

        return view("articles/create");

    }

    public function store(){

        redirect("/");

    }

    public function edit(){

        return view("articles/edit");

    }

    public function update(){

        redirect("/");

    }

    public function destroy(){

        redirect("/");
        
    }
}
```

## Defining Views

X Framework supports views and layouts. Lets start by creating a simple layout.
I have edited the default bootstrap layout below. Notice the content function
below. That function loads the view content that has requested the layout.

**App/Views/layout.php**

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css">

    <title>My Blog</title>
  </head>
  <body>
    <div class="container pt-5">
        <?php content(); ?>
    </div>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js"></script>
    <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js"></script>
  </body>
</html>
```

Now its time to write our create form.

**App/Views/articles/create.php**

```html
<?php layout("layout"); ?>

<h1>
    
    Create an article
    
    <a href="/" class="btn btn-warning float-right">
        Go back
    </a>
    
</h1>

<form action="/articles/create" method="post" class="mt-5">
    <div class="form-group">
        <label for="title">Title</label>
        <input type="text" id="title" name="title" class="form-control" />
    </div>
    <div class="form-group">
        <label for="slug">Slug</label>
        <input type="text" id="slug" name="slug" class="form-control" />
    </div>
    <div class="form-group">
        <label for="body">Body</label>
        <textarea name="body" id="body" rows="10" class="form-control"></textarea>
    </div>
    <div class="form-group">
        <button class="btn btn-success">Create</button>
    </div>
</form>
```

Now its time to update our controller to store the article. Lets go back to
our **App\Controllers\ArticleController.php**. Lets dump the request.

```php
    public function store(){
        dd(request());
    }
```

Now try to submit the form by going to http://localhost:8008/articles/create
If you did everything correct, you should see the data that you
entered in the form. Lets save the request to the database.

```php
    public function store(){

        $request = request()->validate([
            "title" => "required",
            "slug" => "required",
            "body" => "required"
        ]);

        $request = array_merge($request, [
            "created_at" => date("Y-m-d H:i:s"),
            "updated_at" => date("Y-m-d H:i:s")
        ]);

        $article = (new Article)->create($request);

        redirect("/");

    }
```

Now our article is being saved to the database but there is no way to
view all the created articles. Lets fix that by create the index. Lets
start with **App\Controllers\ArticleController.php**. Update the index
function.

```php

    public function index(){

        $articles = (new Article)->all();

        return view("articles/index", compact("articles"));

    }

```

**App\Views\articles\index.php**

```html
<?php layout("layout"); ?>

<h1>
    
    My Blog 
    
    <a href="/articles/create" class="btn btn-primary float-right">
        Create
    </a>

</h1>

<div class="row mt-5">
    <?php foreach ($articles as $article): ?>
        <div class="card col-6">
            <div class="card-body">
                <div class="float-right">
                    <a href="/articles/<?=$article->id?>" class="btn btn-primary">View</a>
                    <a href="/articles/<?=$article->id?>/edit" class="btn btn-warning">Edit</a>
                    <a href="/articles/<?=$article->id?>/delete" class="btn btn-danger">Delete</a>
                </div>
                <h3 class="card-title">
                    <?= $article->title ?>
                </h3>
                <p class="text-muted">Last Modified on <?=$article->created_at?></p>
            </div>
        </div>
    <?php endforeach; ?>
</div>
```

## Route Model Injection

Now lets see some magic. We are going to add delete functionality for our articles.
Lets start with the controller's destroy method.

```php
    public function destroy(Article $article){

        $article->delete();
        
        redirect("/");
        
    }
```

Yup, that does it! Not only X Framework create $article for you, but it also find the
correct one.

Now lets build on top of that an edit form and update method.

**App\Views\articles\edit.php**

```html
<?php layout("layout"); ?>

<h1>
    
    Edit "<?=$article->title?>"
    
    <a href="/" class="btn btn-warning float-right">
        Go back
    </a>
    
</h1>

<form action="/articles/<?=$article->id?>/edit" method="post" class="mt-5">
    <div class="form-group">
        <label for="title">Title</label>
        <input type="text" id="title" name="title" class="form-control" value="<?=$article->title?>" />
    </div>
    <div class="form-group">
        <label for="slug">Slug</label>
        <input type="text" id="slug" name="slug" class="form-control" value="<?=$article->slug?>" />
    </div>
    <div class="form-group">
        <label for="body">Body</label>
        <textarea name="body" id="body" rows="10" class="form-control"><?=$article->body?></textarea>
    </div>
    <div class="form-group">
        <button class="btn btn-success">Update</button>
    </div>
</form>
```

And the edit and update function in **App\Controllers\ArticleController.php**

```php
    public function edit(Article $article){

        return view("articles/edit", compact("article"));

    }

    public function update(Article $article){

        $request = request()->validate([
            "title" => "required",
            "slug" => "required",
            "body" => "required"
        ]);

        $request = array_merge($request, [
            "updated_at" => date("Y-m-d H:i:s")
        ]);

        $article->update($request);

        redirect("/");

    }
```

We are almost done, but we still need to view our article. Lets do that
with Route Model Injection. Add the view function to our controller.

```php
    public function view(Article $article){

        return view("articles/view", compact("article"));

    }
```

And here is the view file.

**App\Views\articles\view.php**

```html
<?php layout("layout"); ?>

<h1>
    
    <?=$article->title?>
    
    <a href="/" class="btn btn-warning float-right">
        Go back
    </a>
    
</h1>

<p class="mt-5">
    <?=$article->body?>
</p>
```

## Slug

We can use slug rather than id in the URL. To use slug, lets edit our model
**App\Models\Article.php**

```php
namespace App\Models;

use XFrames\Database\Model;

class Article extends Model{
    
    protected $index = "slug";
    
}
```

Lets also change the id field in **App\Views\articles\index.php**

Rather than hard-coding slug here, lets use $article->getIndex() to
get the index value of the mode, which we set to be the slug.

```html
<?php layout("layout"); ?>

<h1>
    
    My Blog 
    
    <a href="/articles/create" class="btn btn-primary float-right">
        Create
    </a>

</h1>

<div class="row mt-5">
    <?php foreach ($articles as $article): ?>
        <div class="card col-6">
            <div class="card-body">
                <div class="float-right">
                    <a href="/articles/<?=$article->getIndex()?>" class="btn btn-primary">View</a>
                    <a href="/articles/<?=$article->getIndex()?>/edit" class="btn btn-warning">Edit</a>
                    <a href="/articles/<?=$article->getIndex()?>/delete" class="btn btn-danger">Delete</a>
                </div>
                <h3 class="card-title">
                    <?= $article->title ?>
                </h3>
                <p class="text-muted">Last Modified on <?=$article->created_at?></p>
            </div>
        </div>
    <?php endforeach; ?>
</div>
```

Also we need to update our edit form as well. Change the action to use getIndex method.

```html
<form action="/articles/<?=$article->getIndex()?>/edit" method="post" class="mt-5">
```

## Post Script

We have built a simple blog application. But we are far from done. There is no authentication,
no file uploads and much more is missing. We didnt event touch the advance features like
service containers and events. This tutorial serves as a quick walkthough of the structure
and development process of X Framework applications. We would love to hear your feed back
and feature requests on GitHub. Please star the project if you like it. It helps motivate
the developers and to support the developers you can fund the project as well.