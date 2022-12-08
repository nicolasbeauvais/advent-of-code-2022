# Part 1

> 481 bytes

```php
<?php $r=array_map(fn($r)=>array_map('intval',str_split(trim($r))),file('input.txt'));$c=[];$s=[];for($i=0;$i<count($r[0]);$c[]=array_column($r,$i++));$y=count($c);foreach($r as $m=>$t){foreach($t as $n=>$q){$id=($m*$y)+$n;if($id<$y||$id>$y*(count($r)-1)||$n===0||$n===$y-1)$s[]=$id;foreach([array_slice($t,0,$n),array_slice($t,$n+1),array_slice($c[$n],0,$m),array_slice($c[$n],$m+1)] as $v){count(array_filter($v,fn($val)=>$val>=$q))===0&&$s[]=$id;}}}echo count(array_unique($s));
```

# Part 2

> 506 bytes

```php
<?php $r=array_map(fn($r)=>array_map('intval',str_split(trim($r))),file('input.txt'));$s=[];$c=[];$l='array_reverse';$q='array_slice';for($j=0;$j<count($r[0]);$c[]=array_column($r,$j++));$y=count($c);foreach($r as$m=>$o){foreach($o as$n=>$v){$i=($m*$y)+$n;if($i<$y||$i>$y*(count($r)-1)||$n===0||$n===$y-1)continue;$d=[0,0,0,0];foreach([$l($q($o,0,$n)),$q($o,$n+1),$l($q($c[$n],0,$m)),$q($c[$n],$m+1)]as$j=>$a)foreach($a as$b){$d[$j]+=1;if($b>=$v)break;};$s[]=array_product(array_values($d));}}echo max($s);
```
