# Part 1

> 133 bytes

```php
<?php $s=0;foreach(file('input.txt')as$f){$m=ord($f[2])-87;match(ord($f[0])-64-$m){0=>$s+=3+$m,-2,1=>$s+=$m,2,-1=>$s+=6+$m};}echo$s;
```

# Part 2

> 128 bytes

```php
<?php $t=[[3,1,2],[1,2,3],[2,3,1]];$s=0;foreach(file('input.txt')as$f){$m=ord($f[2])-88;$s+=$m*3+$t[ord($f[0])-65][$m];}echo$s;
```
