# Part 1

```php
<?php

$y = 2000000;
$row = [];
$boundaries = [
    'min' => INF,
    'max' => -INF,
];

foreach (file('input.txt') as $line) {
    preg_match('/x=(-?\d+).*y=(-?\d+):.*x=(-?\d+).*y=(-?\d+)/', $line, $coordinates);

    [$m, $sx, $sy, $bx, $by] = array_map('intval', $coordinates);

    // If the sensor is on the watched row, add it
    if ($sy === $y) {
        $row[$sx] = 'S';
    }

    // If the beacon is on the watched row, add it
    if ($by === $y) {
        $row[$bx] = 'B';
    }

    $boundaries['min'] = min($boundaries['min'], $sx, $bx);
    $boundaries['max'] = max($boundaries['max'], $sx, $bx);

    $manhattanDistance = abs($sx - $bx) + abs($sy - $by);

    if (($y >= $sy && $y <= $sy + $manhattanDistance) || ($y <= $sy && $y >= $sy - $manhattanDistance)) {
        $distanceToSensor = abs(abs($sy) - abs($y));

        $distanceToCoverageBoundary = $manhattanDistance - $distanceToSensor;

        $width = $distanceToCoverageBoundary * 2 + 1;

        foreach (range($sx - $distanceToCoverageBoundary, $sx + $distanceToCoverageBoundary) as $cx) {
            if (!array_key_exists($cx,$row)) {
                $row[$cx] = '#';
            }
        }
    }
}

echo count(array_filter($row, fn ($row) => $row === '#'));
```

# Part 2

```php
<?php

$free = [];
$rows = [];
$boundaries = [
    'min' => 0,
    'max' => 4000000,
];

foreach (file('input.txt') as $index => $line) {
    preg_match('/x=(-?\d+).*y=(-?\d+):.*x=(-?\d+).*y=(-?\d+)/', $line, $coordinates);

    [$m, $sx, $sy, $bx, $by] = array_map('intval', $coordinates);

    $manhattanDistance = abs($sx - $bx) + abs($sy - $by);

    $from = max($boundaries['min'], $sy - $manhattanDistance);
    $to = min($boundaries['max'], $sy + $manhattanDistance);

    if (!array_key_exists($sy, $rows)) {
        $rows[$sy] = [];
    }

    // Add sensor to row
    if (
        $sx >= $boundaries['min']
        && $sx <= $boundaries['max']
        && $sy >= $boundaries['min']
        && $sy <= $boundaries['max']
        && !array_key_exists($sx, $rows[$sy])
    ) {
        $rows[$sy][$sx] = $sx;
    }

    // Add beacon to row
    if (
        $bx >= $boundaries['min']
        && $bx <= $boundaries['max']
        && $by >= $boundaries['min']
        && $by <= $boundaries['max']
        && !array_key_exists($sx, $rows[$sy])
    ) {
        $rows[$by][$bx] = $bx;
    }

    // Top part of the diamond shape
    foreach (range($sy, $from) as $distance => $y) {
        $width = $manhattanDistance - $distance;

        $start = max($sx - $width, $boundaries['min']);
        $end = min($sx + $width, $boundaries['max']);

        $rows[$y][$start] = max($rows[$y][$start] ?? -INF, $end);
    }

    // Bottom part of the diamond shape
    foreach (range($sy, $to) as $distance => $y) {
        $width = $manhattanDistance - $distance;

        $start = max($sx - $width, $boundaries['min']);
        $end = min($sx + $width, $boundaries['max']);

        $rows[$y][$start] = max($rows[$y][$start] ?? -INF, $end);
    }

    echo "Computed sensor $index\n";
}

ksort($rows);

foreach ($rows as $y => $ranges) {
    $check = [];

    ksort($ranges);

    $firstKey = array_key_first($ranges);

    $range = [
        $firstKey,
        $ranges[$firstKey]
    ];

    unset($ranges[$firstKey]);

    foreach ($ranges as $start => $end) {
        if ($start <= $range[1]) {
            $range[1] = max($range[1], $end);
        } else {
            echo "Found beacon at x=" . $start - 1 . ", y=$y\n";
            die;
        }
    }
}
```
