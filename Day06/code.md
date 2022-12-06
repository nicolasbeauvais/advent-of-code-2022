# Part 1

> 96 bytes

```php
<?php for($i=0;count(array_unique(str_split(substr(file('input.txt')[0],++$i,4))))<4;);echo$i+4;
```

# Part 2

> 100 bytes

```php
<?php for($i=0;count(array_unique(str_split(substr(file('input.txt')[0],++$i,14))))<14;);echo$i+14;
```
