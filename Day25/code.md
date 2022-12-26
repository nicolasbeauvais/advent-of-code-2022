# Part 1

```php
<?php

$result = 0;
$snafuResult = '';

$chars = [
    '=' => -2,
    '-' => -1,
    0 => 0,
    1 => 1,
    2 => 2,
];

foreach (file('input.txt') as $line) {
    $snafu = array_reverse(str_split(trim($line)));

    foreach ($snafu as $index => $char) {
        $char = $chars[$char];

        if ($index === 0) {
            $result += $char;
            continue;
        }

        $result += $char * pow(5, $index);
    }
}

$snafuResult = '';

while ($result > 0) {
    $rest = $result % 5;

    if (!in_array($rest, $chars)) {
        $rest -= 5;
        $result += 5;
    }

    $snafuResult .= array_search($rest, $chars);
    $result = floor($result / 5);
}

echo strrev($snafuResult);
```
