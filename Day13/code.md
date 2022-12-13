# Part 1

```php
<?php

$pairs = explode("\n\n", file_get_contents('input.txt'));

$rightOrderPairs = [];
$index = 0;

function isRightOrder($left, $right): ?bool {
    if (is_int($left) && is_int($right)) {
        return $left !== $right ? $left < $right : null;
    }

    if (!is_array($left) || !is_array($right)) {
        return isRightOrder((array)$left, (array)$right);
    }

    while (true) {
        if (empty($left) && empty($right)) {
            return null;
        }

        if (empty($left)) {
            return true;
        }

        if (empty($right)) {
            return false;
        }

        $isRightOrder = isRightOrder(array_shift($left), array_shift($right));

        if (is_bool($isRightOrder)) {
            return $isRightOrder;
        }
    }
}

foreach ($pairs as $pair) {
    $index++;

    [$left, $right] = array_map('json_decode', explode("\n", $pair));

    if (isRightOrder($left, $right) === true) {
        $rightOrderPairs[] = $index;
    }
}

echo array_sum($rightOrderPairs);
```

# Part 2

```php
<?php

$pairs = array_map('json_decode', array_filter(file('input.txt'), fn ($line) => !empty(trim($line))));

function isRightOrder($left, $right): int {
    if (is_int($left) && is_int($right)) {
        if ($left === $right) {
            return 0;
        }

        return $left < $right ? -1 : 1;
    }

    if (!is_array($left) || !is_array($right)) {
        return isRightOrder((array)$left, (array)$right);
    }

    while (true) {
        if (empty($left) && empty($right)) {
            return 0;
        }

        if (empty($left)) {
            return -1;
        }

        if (empty($right)) {
            return 1;
        }

        $isRightOrder = isRightOrder(array_shift($left), array_shift($right));

        if ($isRightOrder !== 0) {
            return $isRightOrder;
        }
    }
}

$pairs[] = [[2]];
$pairs[] = [[6]];

usort($pairs, "isRightOrder");

echo (array_search([[2]], $pairs) + 1) * (array_search([[6]], $pairs) + 1);
```
