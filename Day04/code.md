# Part 1

> 190 bytes

```php
<?php $r=0;foreach(file('input.txt')as$l){[$a,$b]=array_map(fn($p)=>'.'.implode('.',range(...explode('-',$p))).'.',explode(',',$l));(str_contains($a,$b)||str_contains($b,$a))&&$r++;}echo$r;
```

# Part 2

> 138 bytes

```php
<?php $r=0;foreach(file('input.txt')as$l){array_intersect(...array_map(fn($p)=>range(...explode('-',$p)),explode(',',$l)))&&$r++;}echo$r;
```
