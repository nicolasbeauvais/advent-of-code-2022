# Part 1

> 99 bytes

```php
<?php eval('echo max(['.str_replace(["\n\n","\n"],[',','+'],implode('',file('input.txt'))).'0]);');
```

# Part 2

> 124 bytes

```php
<?php eval('$t=['.str_replace(["\n\n","\n"],[',','+'],implode('',file('input.txt'))).'0];');rsort($t);echo$t[0]+$t[1]+$t[2];
```
