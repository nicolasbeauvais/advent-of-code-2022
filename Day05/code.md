# Part 1

> 345 bytes

```php
$p='preg_match_all';$c=file_get_contents('input.txt');$p('/^(\D){2}'.str_repeat('(.){4}',8).'/m',$c,$m);$s=array_map(fn($a)=>array_filter($a,fn($b)=>$b!=' '),$m);$p('/(\d+).*(\d+).*(\d+)/',$c,$m,PREG_SET_ORDER);for($i=0;$m[$i]??0;$i++)for([$z,$a,$b,$c]=$m[$i];$a;$a--)array_unshift($s[$c],array_shift($s[$b]));for($i=1;$i<10;print($s[$i++][0]));
```

# Part 2

> 329

```php
$p='preg_match_all';$c=file_get_contents('input.txt');$p('/^(\D){2}'.str_repeat('(.){4}',8).'/m',$c,$m);$s=array_map(fn($a)=>array_filter($a,fn($b)=>$b!=' '),$m);$p('/(\d+).*(\d+).*(\d+)$/m',$c,$m,PREG_SET_ORDER);foreach($m as$e)$s[$e[3]]=array_merge(array_splice($s[$e[2]],0,$e[1]),$s[$e[3]]);for($i=1;$i<10;print($s[$i++][0]));
```
