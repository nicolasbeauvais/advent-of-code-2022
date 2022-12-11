# Part 1

```php
<?php

$monks = [];
$currentMonk = -1;
$rounds = 1;
$stopAtRound = 20;

// Parse input
foreach (file('input.txt') as $line) {
    if ($line[0]==='M') $currentMonk++;

    $line = trim($line);

    if (!isset($monks[$currentMonk])) {
        $monks[$currentMonk] = [
            'test'=> [],
            'inspected' => 0,
        ];
    }

    if (str_contains($line, 'items:')) {
        $monks[$currentMonk]['items'] = array_map('intval', explode(', ', explode(': ', $line)[1]));
    }

    if (str_contains($line, 'new =')) {
        [$left, $operator, $right] = explode(' ', explode('new = ', $line)[1]);
        $monks[$currentMonk]['operation'] = compact('left', 'operator', 'right');
    }

    if (str_contains($line, 'Test:')) {
        $monks[$currentMonk]['test']['value'] = (int)explode(' by ', $line)[1];
    }

    if (str_contains($line, 'true:')) {
        $monks[$currentMonk]['test']['true'] = (int)explode(' monkey ', $line)[1];
    }

    if (str_contains($line, 'false:')) {
        $monks[$currentMonk]['test']['false'] = (int)explode(' monkey ', $line)[1];
    }
}

// Compute rounds
while (true) {
    foreach ($monks as &$monk) {
        foreach ($monk['items'] as $key => $level) {
            $left = (int)($monk['operation']['left'] === 'old' ? $level : $monk['operation']['left']);
            $right = (int)($monk['operation']['right'] === 'old' ? $level : $monk['operation']['right']);

            $level = match ($monk['operation']['operator']) {
                '+' => $left + $right,
                '*' => $left * $right,
            };

            $level = floor($level / 3);

            unset($monk['items'][$key]);

            if ($level % $monk['test']['value'] === 0) {
                $monks[$monk['test']['true']]['items'][] = $level;
            } else {
                $monks[$monk['test']['false']]['items'][] = $level;
            }

            $monk['inspected']++;
        }
    }

    if ($rounds === $stopAtRound) {
        break;
    }

    $rounds++;
}

// Output
$values = array_column($monks, 'inspected');
rsort($values);
echo array_product(array_slice($values, 0, 2));
```

# Part 2

```php
<?php

$monks = [];
$currentMonk = -1;
$rounds = 1;
$stopAtRound = 20;

// Parse input
foreach (file('input.txt') as $line) {
    if ($line[0]==='M') $currentMonk++;

    $line = trim($line);

    if (!isset($monks[$currentMonk])) {
        $monks[$currentMonk] = [
            'test'=> [],
            'inspected' => 0,
        ];
    }

    if (str_contains($line, 'items:')) {
        $monks[$currentMonk]['items'] = array_map('intval', explode(', ', explode(': ', $line)[1]));
    }

    if (str_contains($line, 'new =')) {
        [$left, $operator, $right] = explode(' ', explode('new = ', $line)[1]);
        $monks[$currentMonk]['operation'] = compact('left', 'operator', 'right');
    }

    if (str_contains($line, 'Test:')) {
        $monks[$currentMonk]['test']['value'] = (int)explode(' by ', $line)[1];
    }

    if (str_contains($line, 'true:')) {
        $monks[$currentMonk]['test']['true'] = (int)explode(' monkey ', $line)[1];
    }

    if (str_contains($line, 'false:')) {
        $monks[$currentMonk]['test']['false'] = (int)explode(' monkey ', $line)[1];
    }
}

$commonModulator = array_product(array_column(array_column($monks, 'test'), 'value'));

while (true) {
    foreach ($monks as &$monk) {
        foreach ($monk['items'] as $key => $level) {
            $left = (int)($monk['operation']['left'] === 'old' ? $level : $monk['operation']['left']);
            $right = (int)($monk['operation']['right'] === 'old' ? $level : $monk['operation']['right']);

            $level = match ($monk['operation']['operator']) {
                '+' => $left + $right,
                '*' => $left * $right,
            };

            $level = $level % $commonModulator;

            unset($monk['items'][$key]);

            if ($level % $monk['test']['value'] === 0) {
                $monks[$monk['test']['true']]['items'][] = $level;
            } else {
                $monks[$monk['test']['false']]['items'][] = $level;
            }

            $monk['inspected']++;
        }
    }

    if ($rounds === 10000) {
        break;
    }

    $rounds++;
}

// Output
$values = array_column($monks, 'inspected');
rsort($values);
echo array_product(array_slice($values, 0, 2));
```
