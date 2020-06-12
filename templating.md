# Templating with PHP

PHP offers an alternative syntax for some of its control structures; namely, if, while, for, foreach, and switch. In each case, the basic form of the alternate syntax is to change the opening brace to a colon (:) and the closing brace to endif;, endwhile;, endfor;, endforeach;, or endswitch;, respectively.

We recommend using this way of templating in your views.

## Printing

You can use this syntax to print a string.

```php
<?= $variable ?>
```

## Conditions

```php
<?php if ($a == 5): ?>
A is equal to 5
<?php endif; ?>
```

In the above example, the HTML block "A is equal to 5" is nested within an if statement written in the alternative syntax. The HTML block would be displayed only if $a is equal to 5.

The alternative syntax applies to else and elseif as well. The following is an if structure with elseif and else in the alternative format:

```php
<?php if ($a == 5): ?>
    a equals 5
<?php elseif ($a == 6): ?>
    a equals 6
<?php else: ?>
    a is neither 5 nor 6
<?php endif; ?>
```

## Loops

For Loop

```php
<?php for($i = 1; $i <= 10; $i++): ?>
    <?= $i ?>
<?php endfor; ?>
```

Foreach Loop

```php
<?php foreach($fruits as $fruit): ?>
    <?= $fruit ?>
<?php endforeach; ?>
```

While Loop

```php
<?php $i = 1; ?>
<?php while($i <= 10): ?>
    <?= $i++ ?>
<?php endwhile; ?>
```

## Want more

Here are a few articles on templating with PHP.

- https://www.joeldare.com/wiki/php:using_php_as_a_template_engine