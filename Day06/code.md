# Part 1

> 122 bytes

```php
<?php preg_match(str_replace('w','(\w)(?!\1','/w)w|\2)w|\2|\3)w|\2|\3|\4)/m'),file('input.txt')[0],$m,256);echo$m[0][1]+4;
```

# Part 2

> 146 bytes

```php
<?php $c=file('input.txt')[0];$i=0;$s=$c[0];while(strlen($s)<14){$s=str_contains($s,$c[++$i])?substr($s,strpos($s,$c[$i])+1):$s.=$c[$i];}echo$i+2;
```
