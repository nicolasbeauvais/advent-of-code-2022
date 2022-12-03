# Part 1

> 109 bytes

```php
<?php eval('echo max(['.preg_replace(['/(\n\n)/','/(\n)/'],[',','+'],file_get_contents('input.txt')).'0]);');
```

# Part 2

> 134 bytes

```php
<?php eval('$t=['.preg_replace(['/(\n\n)/','/(\n)/'],[',','+'],file_get_contents('input.txt')).'0];');rsort($t);echo$t[0]+$t[1]+$t[2];
```
