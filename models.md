# Models

Models are PHP classes that are designed to work with information in your database. For example, let’s say you use X Framework to manage a blog. You might have a model class that contains functions to insert, update, and retrieve your blog data. Here is an example of what such a model class might look like:

```php
namespace App\Models;

use XFrames\Database\Model;

class Article extends Model{

}
```

## Using the Model

### Creating a model

A model can be created using the create function.

```php 
$article = new Article;

$article->create([
    "title" => "My first app",
    "body" => "This is my very first model"
]);
```

### Updating a model

A model can be updated using update function.

```php 
$article = new Article;

$article->update([
    "title" => "My second app"
]);
```

### Deleting a model

A model can be deleted using delete function

```php 
$article = new Article;

$article->delete([
    "id" => "1"
]);
```

### Finding a model

Finding a model means searching for the model with an index
column. That means that the find function will always return
one and only one model based on the index that was passed.

```php 
$articleModel = new Article;

$article = $articleModel->find(5);

dd($article);
```

### Searching a model

Searching refers to finding a model with using some data. We
can use where and fetch methods to search.

```php 
$articleModel = new Article;

$articles = $articleModel
                ->where("body", "...")
                ->limit(10)
                ->offset(20)
                ->order("title", "ASC")
                ->fetch();

dd($articles);
```

Where function is very powerful and can be used in multiple ways
to achieve flexible searching.

```php 
$articleModel = new Article;

$articles = $articleModel
                ->where("body", "...")
                ->where("view >", 10)
                ->where("OR title", "My First App")
                ->where("AND title LIKE", "App")
                ->fetch();

dd($articles);
```

## Model Parameters


### Table Name

By default X Framework tries to guess the model parameters.
The "Article" model above will always use the database
table "article". Similarly "ArticleComments" will use
the database table "article_comments". By that is not
necessary and we can specify our own table.

```php
namespace App\Models;

use XFrames\Database\Model;

class Article extends Model{

    protected $table = "blog_posts"; 

}

```

Now the "Article" model will work only in "blog_posts" database
table.

## Index

Index is the unique column that can identify one and only one
record in database. By default the index is "id" column but we
can change it.

```php
namespace App\Models;

use XFrames\Database\Model;

class Article extends Model{

    protected $index = "slug"; 

}
```

Now we can use find method with the slug.

```php 
$articleModel = new Article;

$article = $articleModel->find("my-first-app");

dd($article);
```

## Route Model Injection

The great thing about models is that they can be injected into
controllers. Since the route paramter is always a string, we
use that string to **find** a model. Lets see it in action.

**routes.php**
```php
Route::get("/articles/:article", "ArticleController@view");
```

**App/Models/Article.php**
```php
namespace App\Models;

use XFrames\Database\Model;

class Article extends Model{

}
```

**App/Controllers/ArticleController.php**
```php
namespace App\Controllers;

use App\Article;

class ArticleController{

    public function view(Article $article){

        echo $article->title;
        
        echo $article->body;

    }

}
```