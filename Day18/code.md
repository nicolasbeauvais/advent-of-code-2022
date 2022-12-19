# Part 1

```php
<?php


$cubes = [];

foreach (file('input.txt') as $line) {
    [$x, $y, $z] = array_map('intval', explode(',', $line));

    $cubes["$x,$y,$z"] = [$x, $y, $z];
}

$exposed = 0;

foreach ($cubes as $cube) {
    [$x, $y, $z] = $cube;

    $neighboors = [
        [$x + 1, $y, $z],
        [$x - 1, $y, $z],
        [$x, $y + 1, $z],
        [$x, $y - 1, $z],
        [$x, $y, $z + 1],
        [$x, $y, $z - 1],
    ];

    foreach ($neighboors as $neighboor) {
        if (!array_key_exists(implode(',', $neighboor), $cubes)) {
            $exposed++;
        }
    }
}

echo $exposed;

```

# Part 2

```php
<?php

$cubes = [];
$boundaries = [
    'x_max' => -INF,
    'x_min' => INF,
    'y_max' => -INF,
    'y_min' => INF,
    'z_max' => -INF,
    'z_min' => INF,
];

foreach (file('input.txt') as $line) {
    [$x, $y, $z] = array_map('intval', explode(',', $line));

    $cubes["$x,$y,$z"] = [$x, $y, $z];

    $boundaries['x_max'] = max($boundaries['x_max'], $x);
    $boundaries['x_min'] = min($boundaries['x_min'], $x);
    $boundaries['y_max'] = max($boundaries['y_max'], $y);
    $boundaries['y_min'] = min($boundaries['y_min'], $y);
    $boundaries['z_max'] = max($boundaries['z_max'], $z);
    $boundaries['z_min'] = min($boundaries['z_min'], $z);
}

// Expand boundaries by one cube
$boundaries['x_max']++;
$boundaries['x_min']--;
$boundaries['y_max']++;
$boundaries['y_min']--;
$boundaries['z_max']++;
$boundaries['z_min']--;

$exposed = 0;

$visited = [
    "$boundaries[x_min],$boundaries[y_min],$boundaries[z_min]" => '',
];
$queue = [
    [$boundaries['x_min'], $boundaries['y_min'], $boundaries['z_min']]
];

while (count($queue)) {
    foreach ($queue as $index => $cube) {
        [$x, $y, $z] = $cube;
        unset($queue[$index]);

        $sides = [
            [$x + 1, $y, $z],
            [$x - 1, $y, $z],
            [$x, $y + 1, $z],
            [$x, $y - 1, $z],
            [$x, $y, $z + 1],
            [$x, $y, $z - 1],
        ];

        foreach ($sides as $side) {
            [$sx, $sy, $sz] = $side;

            if (array_key_exists("$sx,$sy,$sz", $visited)) {
                continue;
            }

            if (array_key_exists("$sx,$sy,$sz", $cubes)) {
                $exposed++;
                continue;
            }

            if (
                $sx <= $boundaries['x_max']
                && $sx >= $boundaries['x_min']
                && $sy <= $boundaries['y_max']
                && $sy >= $boundaries['y_min']
                && $sz <= $boundaries['z_max']
                && $sz >= $boundaries['z_min']
            ) {
                $queue[] = [$sx, $sy, $sz];
                $visited["$sx,$sy,$sz"] = '';
            }
        }
    }
}

echo $exposed;
```
