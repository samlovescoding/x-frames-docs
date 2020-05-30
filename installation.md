# Installation

### Requirements

1. PHP 7.0+
1. NodeJS
2. MySQL
3. Composer

### Installation

To create a project using x framework, use the following commands.

> composer create-project samlovescoding/x blog

> cd blog

> npm install

### Running

To serve the website, use the following command. To serve
using this command, make sure to have your PHP executable
in your PATH.

> npm run serve

### Stylesheets and JavaScript

You can build stylesheets and javascript using the default
webpack configuration provided by X Framework. It will build
your SASS/SCSS files and JavaScript files into main.js in the
public_html folder. You can directly include it in your views
using

```html
<script src="main.js"></script>
```

To build your static files, use the following command.

> npm run build

Otherwise you can run the watch command in the background.

> npm run watch