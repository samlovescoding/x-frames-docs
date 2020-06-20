# File System

File System API is a very elegant implementation for working with the
hard disk. We have built a wrapper around the PHP's default file
functions to provide an object oriented solution. The file system
is built up of Files and Folders. We also must understand the working
of the system to prevent un-identified memory leaks. You CAN load
a file into the memory and loading a large file into the memory can
be very expensive for the server, life of the hard drive, your data
and your customers.

It is very important that you understand each and every functionality.

Storages are a simple implementation to provide very easy implementation
of file systems without deep diving into Files or Folders. We will
talk about them in the last.

# Files

By default commit mode is on. That means any function that has to
work with the file system can directly work on it. We recommend
to have it switched on. It affects the writing functions like
write and append. Benefits of commit on is that, the data wont
be saved in the memory but directly written to the file. If you
are writing 10MB of data to the file, you dont want to store it
in the main memory.

You must save a file to implements its changes.

#### Creating a File

By default files are saved in the root folder.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";
$data = "Hello World";

$file = File::create($name, $data);
$file->save();
```

#### Loading an existing file

The file must exist, if you want to load a file.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

$file = File::load($name);

echo $file->read();
```

#### Checking file existance

This function returns true or false depending if
a file exist. By default, it only checks the app root.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

if(File::exists($name)){
    $file = File::load($name);
}else{
    $file = File::create($name);
}
```

#### Writing to a file

Lets write some content to a file. 

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

if(File::exists($name)){
    $file = File::load($name);
}else{
    $file = File::create($name);
}

$file->write("Oh hi Mark.");
//No need to save. Write function auto saves if commit is on.
```

#### Changing directory of the file

To change the directory we recommend specifying a path.
The path is relative to the root of the project. Lets
say you want to create a file in the 'Files/temp' folder.
We can easily do that using the path function.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

$file = File::create($name);

$file->path("Files/temp");

$file->write("Oh hi Mark.");
```

#### Append or Prepend data to a file

Prepend writes to the start of a file,
while append writes to the end of a file.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

if(File::exists($name)){
    $file = File::load($name);
    
    $file->prepend("Hello World. ");
    //No need to save. Prepend function auto saves if commit is on.
    
    $file->append(" Good Bye.");
    //No need to save. Append function auto saves if commit is on.
}
```

#### Empty a file

Emptying a file is deleting all the data of a file.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

if(File::exists($name)){
    $file = File::load($name);   
    $file->empty();
}
```

#### Deleting a file

It will delete a file from the disk.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

if(File::exists($name)){
    $file = File::load($name);   
    $file->delete();
}
```

#### File Size

This function will return the size of a file in bytes.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

if(File::exists($name)){
    $file = File::load($name);   
    echo $file->size();
}
```

#### Copying a file

This function will duplicate the file into a new location.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

if(File::exists($name)){
    $file = File::load($name);   
    $file->copy("world.txt");
}
```


#### Rename / Move a file

This will rename a file into a new location.

```php
use XFrames\FileSystem\File;

$name = "hello.txt";

if(File::exists($name)){
    $file = File::load($name);   
    $file->move("world.txt");
}
```

#### Save to a folder

All files can be saved to a folder if you are using folders.

```php
use XFrames\FileSystem\File;
use XFrames\FileSystem\Folder;

$folder = Folder::create("something");
$file = File::create("hello.txt");
$file->saveTo($folder);
$file->write("Hello");
```

# Folders

Folders provide an implementation of actual folders. Lets see them
in action.

#### Creating a folder

This function creates a folder if it does not exist, otherwise, it
loads it up.

```php
use XFrames\FileSystem\Folder;
$folder = Folder::create("something");
```

#### Deleting a folder

This functions deletes a folder. It will delete all the files in it.

```php
use XFrames\FileSystem\Folder;
$folder = Folder::create("something");
$folder->delete();
```

#### Load a file

This function will load a file as a `File` object.

```php
use XFrames\FileSystem\Folder;
$folder = Folder::create("something");
$file = $folder->file("hello.txt");
$file->delete();
```

#### List all files

This function will load all files.

```php
use XFrames\FileSystem\Folder;
$folder = Folder::create("something");
$size = 0;
foreach($folder->files() as $file){
    $size += $file->size();
}
echo $size;
```

You can pass true as a parameter to include Folders as well.
```php
$folder->files(true);
```

#### Add an existing file to a folder

This is same as saveTo function on a file, just the inverse.

```php
$file = $folder->add($file);
```

# Storage

Storages provide an easy way to work with files in pre configured folders.
Storages are general Folders but there path is defined in the file system
configuration. You can even add custom storages. A storage is a normal
Folder object.

```php
$logName = time() . ".log";
$log = File::create($logName, "App is working fine.");
$logs = storage("log");
$logs->add($log);
```

You can also use store function on file to easily store it to a storage.

```php
$log = File::create("1.log", "App is working fine.");
$log->store("logs");
```