# Part 1

> 192 bytes

```php
<?php $c=$r=1;$a=0;$f=fn(&$c,&$a,$r)=>in_array($c,[20,60,100,140,180,220])?$a+=$c++*$r:$c++;foreach(file('input.txt')as$l)$f($c,$a,$r)&&$l[0]=='a'&&$f($c,$a,$r)&&$r+=explode(' ',$l)[1];echo$a;
```

# Part 2

> 247 bytes

```php
<?php $c=$r=1;$s=array_fill(0,240,'.');$f=fn(&$c,&$s,$r)=>in_array($c%40,[$r,++$r,++$r])?$s[$c++-1]='#':$c++;foreach(file('input.txt')as$l)$f($c,$s,$r)&&$l[0]=='a'&&$f($c,$s,$r)&&$r+=explode(' ',$l)[1];foreach($s as$i=>$p)echo$p.(++$i%40?'':"\n");
```
