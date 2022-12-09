# Part 1

> 316 bytes

```php
<?php $t=$h=[0,0];$v=[];foreach(file('input.txt')as$l){[$d,$c]=explode(' ',$l);for($e=0;$e++<$c;match($d){'R'=>$h[0]++,'L'=>$h[0]--,'U'=>$h[1]++,'D'=>$h[1]--}){if(abs($h[0]-$t[0])<=1&&abs($h[1]-$t[1])<=1)continue;foreach([0,1]as$j)$h[$j]!==$t[$j]&&$t[$j]+=$h[$j]>$t[$j]?1:-1;$v[]=$t;}}echo count(array_unique($v,0));
```

# Part 2

> 419 bytes

```php
<?php $k=array_fill(0,10,[0,0]);$v=[];foreach(file('input.txt')as$l){[$d,$c]=explode(' ',$l);for($e=0;$e++<$c;match($d){'R'=>$k[0][0]++,'L'=>$k[0][0]--,'U'=>$k[0][1]++,'D'=>$k[0][1]--}){foreach(range(1,9)as$i){$a=fn($t)=>abs($k[$i-1][$t]-$k[$i][$t])<=1;if($a(0)&&$a(1))continue;foreach([0,1]as$j)$k[$i-1][$j]!==$k[$i][$j]&&$k[$i][$j]+=$k[$i-1][$j]>$k[$i][$j]?1:-1;$i==9&&$v[]=$k[$i];}}}echo count(array_unique($v,0))+1;

```
