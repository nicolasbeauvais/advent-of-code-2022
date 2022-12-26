# Part 1

```php
<?php

$content = file('input.txt');

$me = null;
$map = [];
$boundaries = ['x' => [], 'y' => []];
$instructions = [];

/**
 * Parse instructions
 */

preg_match_all('/([0-9]+)|([LR])/m', trim(array_pop($content)), $matches, PREG_SET_ORDER);

foreach ($matches as $match) {
    $instructions[] = $match[0] === 'L' || $match[0] === 'R' ? $match[0] : (int)$match[0];
}

/**
 * Create base map and calculate boundaries
 */

foreach ($content as $y => $row) {
    ++$y;

    foreach (str_split(str_replace("\n", '', $row)) as $x => $col) {
        if ($col === ' ') continue;

        $x++;

        if ($me === null) {
            $me = [$x, $y, 0];
        }

        $map["$x,$y"] = [$x, $y, $col === '#', [null, null, null, null]];

        if (!array_key_exists($x, $boundaries['y'])) {
            $boundaries['y'][$x] = [INF, -INF];
        }

        if (!array_key_exists($y, $boundaries['x'])) {
            $boundaries['x'][$y] = [INF, -INF];
        }

        $boundaries['y'][$x][0] = min($boundaries['y'][$x][0], $y);
        $boundaries['y'][$x][1] = max($boundaries['y'][$x][1], $y);
        $boundaries['x'][$y][0] = min($boundaries['x'][$y][0], $x);
        $boundaries['x'][$y][1] = max($boundaries['x'][$y][1], $x);
    }
}

/**
 * Apply boundaries portal to map coordinates
 */

foreach ($boundaries['x'] as $y => $xBoundary) {
    [$xMin, $xMax] = $xBoundary;

    if (!$map["$xMin,$y"][2]) {
        $map["$xMin,$y"][3][2] = "$xMax,$y";
    }

    if (!$map["$xMax,$y"][2]) {
        $map["$xMax,$y"][3][0] = "$xMin,$y";
    }
}

foreach ($boundaries['y'] as $x => $yBoundary) {
    [$yMin, $yMax] = $yBoundary;

   if (!$map["$x,$yMin"][2]) {
        $map["$x,$yMin"][3][3] = "$x,$yMax";
    }

    if (!$map["$x,$yMax"][2]) {
        $map["$x,$yMax"][3][1] = "$x,$yMin";
    }
}

/**
 * Walk the map
 */

foreach ($instructions as $instruction) {
    if ($instruction === 'R') {
        $me[2] = ($me[2] + 1) % 4;
        continue;
    }

    if ($instruction === 'L') {
        $me[2] = ($me[2] - 1 + 4) % 4;
        continue;
    }

    foreach (range(1, $instruction) as $moves) {
        [$x, $y, $direction] = $me;

        $move = match ($direction) {
            0 => $map[($x + 1) . ",$y"] ?? $map[$map["$x,$y"][3][0]],
            1 => $map["$x," . ($y + 1)] ?? $map[$map["$x,$y"][3][1]],
            2 => $map[($x - 1) . ",$y"] ?? $map[$map["$x,$y"][3][2]],
            3 => $map["$x," . ($y - 1)] ?? $map[$map["$x,$y"][3][3]],
        };

        // Found a wall
        if ($move[2]) {
            break;
        }

        // Move to tile
        $me[0] = $move[0];
        $me[1] = $move[1];
    }
}

echo 4 * $me[0] + 1000 * $me[1] + $me[2] . "\n";
```

# Part 2

```php
<?php

$content = file('input.txt');

const RIGHT = 0;
const BOTTOM = 1;
const LEFT = 2;
const TOP = 3;

$me = null;
$map = [
    'top' => [
        'moves' => [
            RIGHT => fn ($x, $y) => ['right', 1, $y, RIGHT],
            BOTTOM => fn ($x, $y) => ['front', $x, 1, BOTTOM],
            LEFT => fn ($x, $y) => ['left', 1, 51 - $y, RIGHT],
            TOP => fn ($x, $y) => ['back', 1, $x, RIGHT],
        ]
    ],
    'bottom' => [
        'moves' => [
            RIGHT => fn ($x, $y) => ['right', 50, 51 - $y, LEFT],
            BOTTOM => fn ($x, $y) => ['back', 50, $x, LEFT],
            LEFT => fn ($x, $y) => ['left', 50, $y, LEFT],
            TOP => fn ($x, $y) => ['front', $x, 50, TOP],
        ]
    ],
    'right' => [
        'moves' => [
            RIGHT => fn ($x, $y) => ['bottom', 50, 51 - $y, LEFT],
            BOTTOM => fn ($x, $y) => ['front', 50, $x, LEFT],
            LEFT => fn ($x, $y) => ['top', 50, $y, LEFT],
            TOP => fn ($x, $y) => ['back', $x, 50, TOP],
        ]
    ],
    'left' => [
        'moves' => [
            RIGHT => fn ($x, $y) => ['bottom', 1, $y, RIGHT],
            BOTTOM => fn ($x, $y) => ['back', $x, 1, BOTTOM],
            LEFT => fn ($x, $y) => ['top', 1, 51 - $y, RIGHT],
            TOP => fn ($x, $y) => ['front', 1, $x, RIGHT],
        ]
    ],
    'front' => [
        'moves' => [
            RIGHT => fn ($x, $y) => ['right', $y, 50, TOP],
            BOTTOM => fn ($x, $y) => ['bottom', $x, 1, BOTTOM],
            LEFT => fn ($x, $y) => ['left', $y, 1, BOTTOM],
            TOP => fn ($x, $y) => ['top', $x, 50, TOP],
        ]
    ],
    'back' => [
         'moves' => [
            RIGHT => fn ($x, $y) => ['bottom', $y, 50, TOP],
            BOTTOM => fn ($x, $y) => ['right', $x, 1, BOTTOM],
            LEFT => fn ($x, $y) => ['top', $y, 1, BOTTOM],
            TOP => fn ($x, $y) => ['left', $x, 50, TOP],
        ]
    ],
];

$instructions = [];

/**
 * Parse instructions
 */

preg_match_all('/([0-9]+)|([LR])/m', trim(array_pop($content)), $matches, PREG_SET_ORDER);

foreach ($matches as $match) {
    $instructions[] = $match[0] === 'L' || $match[0] === 'R' ? $match[0] : (int)$match[0];
}

/**
 * Create the map for each face
 */

foreach ($content as $y => $row) {
    foreach (str_split(str_replace("\n", '', $row)) as $x => $col) {
        if ($col === ' ') continue;

        $face = match (true) {
            $y < 50 => match (true) {
                $x < 100 => 'top',
                default => 'right',
            },
            $y >= 50 && $y < 100 => 'front',
            $y >= 100 && $y < 150 => match (true) {
                $x < 50 => 'left',
                default => 'bottom',
            },
            default => 'back'
        };

        // Re-index coordinates for the face
        $realX = ($x % 50) + 1;
        $realY = ($y % 50) + 1;

        if ($me === null) {
            $me = [$face, $realX, $realY, 0];
        }

        $map[$face]["$realX,$realY"] = [$realX, $realY, $col === '#'];
    }
}

/**
 * Walk the map
 */

foreach ($instructions as $instruction) {
    if ($instruction === 'R') {
        $me[3] = ($me[3] + 1) % 4;

        continue;
    }

    if ($instruction === 'L') {
        $me[3] = ($me[3] - 1 + 4) % 4;

        continue;
    }

    foreach (range(1, $instruction) as $moves) {
        [$face, $x, $y, $direction] = $me;

        $move = match ($direction) {
            RIGHT => $map[$face][($x + 1) . ",$y"] ?? null,
            BOTTOM => $map[$face]["$x," . ($y + 1)] ?? null,
            LEFT => $map[$face][($x - 1) . ",$y"] ?? null,
            TOP => $map[$face]["$x," . ($y - 1)] ?? null,
        };

        if (!$move) {
            $swap = $map[$face]['moves'][$direction]($x, $y);

            // Swap tile is a wall
            if ($map[$swap[0]]["$swap[1],$swap[2]"][2]) {
                break;
            }

            $me = $swap;
            continue;
        }

        // Found a wall
        if ($move[2]) {
            break;
        }

        // Move to tile
        $me[1] = $move[0];
        $me[2] = $move[1];
    }
}

[$face, $x, $y, $direction] = $me;

$x = match ($face) {
    'front', 'top', 'bottom' => $x + 50,
    'right' => $x + 100,
    default => $x,
};

$y = match ($face) {
    'front' => $y + 50,
    'left', 'bottom' => $y + 100,
    'back' => $y + 150,
    default => $y,
};

echo 4 * $x + 1000 * $y + $direction . "\n";
```
