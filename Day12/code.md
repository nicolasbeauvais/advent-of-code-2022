# Part 1

```php
<?php

$end = ord('z') + 1;
$start = ord('a') - 1;

$input = str_replace(['S', 'E'], [chr($start), chr($end)], file_get_contents('input.txt'));

$map = array_map(fn($r) => array_map('ord', str_split(trim($r))), explode("\n", trim($input)));

$visited = [[43,20]];
$nodes = [[43,20]];
$queue = [];
$moves = 1;

while (count($nodes)) {
    foreach ($nodes as $node) {
        [$x, $y] = $node;

        $possibleMoves = [
            'top' => $y > 0 ? [$x, $y - 1] : null,
            'bottom' => $y < 40 ? [$x, $y + 1] : null,
            'left' => $x > 0 ? [$x - 1, $y] : null,
            'right' => $x < 66 ? [$x + 1, $y] : null,
        ];

        foreach ($possibleMoves as $move) {
            if ($move === null) {
                continue;
            }

            if ($map[$y][$x] - $map[$move[1]][$move[0]] > 1) {
                continue;
            }

            if (in_array($move, $visited)) {
                continue;
            }

            if ($map[$move[1]][$move[0]] === $start) {
                echo "Found path with $moves moves";die;
            }

            $visited[] = $move;
            $queue[] = $move;
        }
    }

    $moves++;
    $nodes = $queue;
    $queue = [];
}
```

# Part 2

```php
<?php

// 377

$end = ord('z') + 1;
$start = ord('a') - 1;

$input = str_replace(['S', 'E'], [chr($start), chr($end)], file_get_contents('input.txt'));

$map = array_map(fn($r) => array_map('ord', str_split(trim($r))), explode("\n", trim($input)));

$visited = [[43,20]];
$nodes = [[43,20]];
$queue = [];
$moves = 1;

while (count($nodes)) {
    foreach ($nodes as $node) {
        [$x, $y] = $node;

        $possibleMoves = [
            'top' => $y > 0 ? [$x, $y - 1] : null,
            'bottom' => $y < 40 ? [$x, $y + 1] : null,
            'left' => $x > 0 ? [$x - 1, $y] : null,
            'right' => $x < 66 ? [$x + 1, $y] : null,
        ];

        foreach ($possibleMoves as $move) {
            if ($move === null) {
                continue;
            }

            if ($map[$y][$x] - $map[$move[1]][$move[0]] > 1) {
                continue;
            }

            if (in_array($move, $visited)) {
                continue;
            }

            if ($map[$move[1]][$move[0]] === $start + 1) {
                echo "Found path with $moves moves";die;
            }

            $visited[] = $move;
            $queue[] = $move;
        }
    }
    
    $moves++;
    $nodes = $queue;
    $queue = [];
}
```
