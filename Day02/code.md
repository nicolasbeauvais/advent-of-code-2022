# Part 1

```php
$p=fn($c,$o)=>ord($c)-$o;$s=0;foreach(file('input.txt')as$f){$e=$p($f[0],64);$m=$p($f[2],87);match($e-$m){0=>$s+=3+$m,-2,1=>$s+=$m,2,-1=>$s+=6+$m};}echo$s;
```

# Part 2

```php
$t=[[3,1,2],[1,2,3],[2,3,1]];$p=fn($c,$o)=>ord($c)-$o;$s=0;foreach(file('input.txt')as$f){$m=$p($f[2],88);$e=$p($f[0],65);$s+=$m*3+$t[$e][$m];}echo$s;
```
