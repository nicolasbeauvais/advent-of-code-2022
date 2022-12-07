# Part 1

> 404 bytes

```php
<?php $n=fn($p)=>(object)['p'=>$p,'c'=>[],'s'=>0];$c=$t=$n(null);function w($n,$s){$n->s+=$s;$n->p&&w($n->p,$s);}preg_match_all('/\$ cd ([.\w]+)|(\d+)/m',file_get_contents('input.txt'),$m,2);foreach($m as$i){$i[1]?($c=$i[1][0]==='.'?$c->p:($c->c[]=$n($c))):([$s,$f]=explode(' ',$i[2])&&w($c,(int)$i[2]));}function r($n,$r=0){if($n->s<100001)$r+=$n->s;foreach($n->c as$c){$r+=r($c);}return$r;}echo r($t);
```

# Part 2

> 421 bytes

```php
<?php $n=fn($p)=>(object)['p'=>$p,'c'=>[],'s'=>0];$c=$t=$n(null);function w($n,$s){$n->s+=$s;$n->p&&w($n->p,$s);}preg_match_all('/\$ cd ([.\w]+)|(\d+)/m',file_get_contents('input.txt'),$m,2);foreach($m as$i){$i[1]?($c=$i[1][0]==='.'?$c->p:($c->c[]=$n($c))):([$s,$f]=explode(' ',$i[2])&&w($c,(int)$i[2]));}function r($n,$f){if($n->s>=4965705&&$n->s<$f)$f=$n->s;foreach($n->c as$c){$f=r($c,$f);}return$f;}echo r($t,$t->s);
```
