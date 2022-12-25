# Part 1

```php
<?php

$values = [];
$monkeys = [];

foreach (file('input.txt') as $line) {
    [$monkey, $value] = explode(': ', $line);

    if (is_numeric($value)) {
        $values[$monkey] = (int)$value;
    }

    $monkeys[$monkey] = explode(' ', trim($value));
}

while (true) {
    foreach ($monkeys as $monkey => $value) {
        if (array_key_exists($value[0], $values) && array_key_exists($value[2], $values)) {
            eval(sprintf('$values[$monkey] = %d %s %d;', $values[$value[0]], $value[1], $values[$value[2]]));

            if ($monkey === 'root') {
                echo $values['root'] . "\n";

                break 2;
            }
        }
    }
}

```

# Part 2

```php
<?php

$monkeys = [];

foreach (file('input.txt') as $line) {
    [$monkey, $value] = explode(': ', $line);

    if ($monkey === 'humn') {
        continue;
    }

    if (is_numeric($value)) {
        $monkeys[$monkey] = (int)$value;
        continue;
    }

    $monkeys[$monkey] = explode(' ', trim($value));
}

function solve($solve) {
    global $monkeys;

    foreach ($monkeys as $monkey => $value) {
        if (is_int($value)) {
            continue;
        }

        [$left, $operator, $right] = $value;

        if ($left === $solve) {
            $left = $monkeys[$solve];
        }

        if ($right === $solve) {
            $right = $monkeys[$solve];
        }

        $monkeys[$monkey] = [$left, $operator, $right];

        if (is_int($left) && is_int($right)) {
            eval("\$monkeys[\$monkey] = $left $operator $right;");
            solve($monkey);
        }
    }
}

foreach ($monkeys as $monkey => $value) {
    if (!is_int($value)) {
        continue;
    }

    solve($monkey);
}

function walk($search): int|string {
    global $monkeys;

    if (is_int($search)) {
        return $search;
    }

    if (is_int($monkeys[$search])) {
        return $monkeys[$search];
    }

    [$left, $operator, $right] = $monkeys[$search];

    $left = $left === 'humn' ? 'x' : walk($left);
    $right = $right === 'humn' ? 'x' : walk($right);

    if (is_int($left) && is_int($right)) {
        eval("\$value = $left $operator $right;");
        return $value;
    }

    return implode('', [
        '(',
        $left,
        $operator,
        $right,
        ')'
    ]);
}

[$left, $operator, $right] = $monkeys['root'];

echo walk($left) . '=' . walk($right);

```
