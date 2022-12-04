# Part 1

> 101 bytes

```php
<?php eval('echo max(['.str_replace(["\n\n","\n"],[',','+'],file_get_contents('input.txt')).'0]);');
```

# Part 2

> 125 bytes

```php
<?php eval('$t=['.str_replace(["\n\n","\n"],[',','+'],file_get_contents('input.txt')).'0];');rsort($t);echo$t[0]+$t[1]+$t[2];
```
