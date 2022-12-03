# Part 1

> 163 bytes

```php
<?php $r=0;$s='str_split';foreach(file('input.txt')as$l){$b=$s($l,strlen($l)/2);$p=ord([...array_intersect($s($b[0]),$s($b[1]))][0]);$r+=$p>96?$p-96:$p-38;}echo$r;
```

# Part 2

> 178 bytes

```php
<?php $r=0;$i=-1;$l=file('input.txt');$s='str_split';while($i<count($l)-1){$p=ord([...array_intersect($s($l[++$i]),$s($l[++$i]),$s($l[++$i]))][0]);$r+=$p>96?$p-96:$p-38;}echo$r;
```
