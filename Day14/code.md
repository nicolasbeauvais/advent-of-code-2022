# Part 1

```php
<?php

$rocks = [];

$boundaries = [
    'x_min' => INF,
    'x_max' => -INF,
    'y_max' => -INF,
];

/**
 * Parse input to build the lines of rocks,
 * and detect the map boundaries.
 */
foreach (file('input.txt') as $line) {
    preg_match_all('/(\d+),(\d+)/', $line, $coordinates, PREG_SET_ORDER);

    // Parse coordinates to integer
    $coordinates = array_map(
        static fn ($coordinate) => [(int)$coordinate[1], (int)$coordinate[2]],
        $coordinates
    );

    [$x1, $y1] = array_shift($coordinates);

    while (count($coordinates)) {
        [$x2, $y2] = array_shift($coordinates);

        // Draw horizontal line
        if ($x1 !== $x2) {
            foreach (range($x1, $x2) as $x) {
                if (!in_array([$x, $y1], $rocks, true)) {
                    $rocks[] = [$x, $y1];
                }
            }
        }

        // Draw vertical line
        if ($y1 !== $y2) {
            foreach (range($y1, $y2) as $y) {
                if (!in_array([$x1, $y], $rocks, true)) {
                    $rocks[] = [$x1, $y];
                }
            }
        }

        // Look for map boundary
        $boundaries['x_min'] = min($boundaries['x_min'], $x1, $x2);
        $boundaries['x_max'] = max($boundaries['x_max'], $x1, $x2);
        $boundaries['y_max'] = max($boundaries['y_max'], $y1, $y2);

        // Set coordinates for next loop
        $x1 = $x2;
        $y1 = $y2;
    }
}

// De-duplicate rocks
$rocks = array_unique($rocks, SORT_REGULAR);

/**
 * Simulate units of sands
 */
$restingSandUnits = 0;
[$xs, $ys] = [500, 0];

while (true) {
    // Verify if the unit of sand can move down
    if (!in_array([$xs, $ys + 1], $rocks, true)) {
        $ys++;

        // If the coordinate is out of the map boundary, stop
        if ($ys > $boundaries['y_max']) {
            break;
        }

        continue;
    }

    // Verify if the unit of sand can diagonally move left
    if (!in_array([$xs - 1, $ys + 1], $rocks, true)) {
        $ys++;
        $xs--;

        // If the coordinate is out of the map boundary, stop
        if ($ys > $boundaries['y_max'] || $xs < $boundaries['x_min']) {
            break;
        }

        continue;
    }

    // Verify if the unit of sand can diagonally move right
    if (!in_array([$xs + 1, $ys + 1], $rocks, true)) {
        $ys++;
        $xs++;

        // If the coordinate is out of the map boundary, stop
        if ($ys > $boundaries['y_max'] || $xs > $boundaries['x_max']) {
            break;
        }

        continue;
    }

    // The sand unit has come to rest, it became a sandstone...
    $rocks[] = [$xs, $ys];
    [$xs, $ys] = [500, 0];
    $restingSandUnits++;
}

echo $restingSandUnits;
```

# Part 2

```php
<?php

$rocks = [];

$boundaries = [
    'x_min' => INF,
    'x_max' => -INF,
    'y_max' => -INF,
];

/**
 * Parse input to build the lines of rocks,
 * and detect the map boundaries.
 */
foreach (file('input.txt') as $line) {
    preg_match_all('/(\d+),(\d+)/', $line, $coordinates, PREG_SET_ORDER);

    // Parse coordinates to integer
    $coordinates = array_map(
        static fn ($coordinate) => [(int)$coordinate[1], (int)$coordinate[2]],
        $coordinates
    );

    [$x1, $y1] = array_shift($coordinates);

    while (count($coordinates)) {
        [$x2, $y2] = array_shift($coordinates);

        // Draw horizontal line
        if ($x1 !== $x2) {
            foreach (range($x1, $x2) as $x) {
                if (!array_key_exists("$x,$y1", $rocks)) {
                    $rocks["$x,$y1"] = '';
                }
            }
        }

        // Draw vertical line
        if ($y1 !== $y2) {
            foreach (range($y1, $y2) as $y) {
                if (!array_key_exists("$x1,$y", $rocks)) {
                    $rocks["$x1,$y"] = '';
                }
            }
        }

        // Look for map boundary
        $boundaries['x_min'] = min($boundaries['x_min'], $x1, $x2);
        $boundaries['x_max'] = max($boundaries['x_max'], $x1, $x2);
        $boundaries['y_max'] = max($boundaries['y_max'], $y1, $y2);

        // Set coordinates for next loop
        $x1 = $x2;
        $y1 = $y2;
    }
}

// Add cave bottom at Y max boundary + 2, extend X boundary by min & max 100
$boundaries['x_min'] -= 1000;
$boundaries['x_max'] += 1000;
foreach (range($boundaries['x_min'], $boundaries['x_max']) as $x) {
    $rocks["$x," . ($boundaries['y_max'] + 2)] = '';
}

/**
 * Simulate units of sands
 */
$restingSandUnits = 0;
[$xs, $ys] = [500, 0];

while (true) {
    // Verify if the unit of sand can move down
    if (!array_key_exists("$xs," . ($ys + 1), $rocks)) {
        $ys++;

        continue;
    }

    // Verify if the unit of sand can diagonally move left
    if (!array_key_exists(($xs - 1) . ',' . ($ys + 1), $rocks)) {
        $ys++;
        $xs--;

        continue;
    }

    // Verify if the unit of sand can diagonally move right
    if (!array_key_exists(($xs + 1) . ',' . ($ys + 1), $rocks)) {
        $ys++;
        $xs++;

        continue;
    }

    // The sand unit block the source
    if ($xs === 500 && $ys === 0) {
        break;
    }

    // The sand unit has come to rest, it became a sandstone...
    $rocks["$xs,$ys"] = '';
    [$xs, $ys] = [500, 0];
    $restingSandUnits++;
}

echo $restingSandUnits + 1;

```
