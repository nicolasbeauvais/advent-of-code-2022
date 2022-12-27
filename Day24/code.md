# Part 1

```php
<?php

const TOP = 0;
const RIGHT = 1;
const LEFT = 2;
const BOTTOM = 3;

$best = INF;
$maps = [];
$signatures = [];
$content = file('input.txt');
$boundaries = [
    TOP => 1,
    RIGHT => strlen(trim($content[0])) - 2,
    LEFT => 1,
    BOTTOM => count($content) - 2,
];

/**
 * Create the map
 */

foreach ($content as $y => $row) {
    foreach (str_split(trim($row)) as $x => $col) {
        if ($col === '#') continue;

        if ($y === 0) {
            $maps[0]["$x,$y"] = [];
            $start = [$x, $y];
            continue;
        }

        if ($y === count($content) - 1) {
            $maps[0]["$x,$y"] = [];
            $destination = [$x, $y];
            continue;
        }

        $maps[0]["$x,$y"] = $col === '.' ? [] : [match ($col) {
            '^' => TOP,
            '>' => RIGHT,
            '<' => LEFT,
            'v' => BOTTOM,
        }];
    }
}

function move($me, $minutes) {
    global $best;
    global $maps;
    global $signatures;
    global $boundaries;
    global $destination;

    if ($minutes >= $best) {
        return;
    }

    if (abs($me[0] - $destination[0]) + abs($me[1] - $destination[1]) + $minutes + 1 >= $best) {
        return;
    }

    // Verify signature
    $signature = $minutes . ',' . implode(',', $me);

    if (array_key_exists($signature, $signatures)) {
        return;
    }

    $signatures[$signature] = '';

    if (!array_key_exists($minutes, $maps)) {
        $maps[$minutes] = array_map(fn () => [], $maps[$minutes - 1]);

        foreach ($maps[$minutes - 1] as $coords => $elements) {
            [$x, $y] = array_map('intval', explode(',', $coords));

            foreach ($elements as $element) {
                $move = match ($element) {
                    RIGHT => [$x + 1, $y],
                    BOTTOM => [$x, $y + 1],
                    LEFT => [$x - 1, $y],
                    TOP => [$x, $y - 1],
                };

                if (!array_key_exists("$move[0],$move[1]", $maps[$minutes - 1])) {
                    $move = match ($element) {
                        RIGHT => [$boundaries[LEFT], $y],
                        BOTTOM => [$x, $boundaries[TOP]],
                        LEFT => [$boundaries[RIGHT], $y],
                        TOP => [$x, $boundaries[BOTTOM]],
                    };
                }

                $maps[$minutes]["$move[0],$move[1]"][] = $element;
            }
        }
    }

    $moves = [
        [$me[0], $me[1] + 1],
        [$me[0] + 1, $me[1]],
        [$me[0], $me[1] - 1],
        [$me[0] - 1, $me[1]],
        [$me[0], $me[1]]
    ];

    foreach ($moves as $move) {
        if (!array_key_exists("$move[0],$move[1]", $maps[$minutes])) {
            continue;
        }

        if (!empty($maps[$minutes]["$move[0],$move[1]"])) {
            continue;
        }

        if ($move[0] === $destination[0] && $move[1] === $destination[1]) {

            if ($best > $minutes) {
                $best = $minutes;
            }

            return;
        }

        move($move, $minutes + 1);
    }
}

move($start, 1);

echo "$best\n";
```

# Part 2

```php
<?php

const TOP = 0;
const RIGHT = 1;
const LEFT = 2;
const BOTTOM = 3;

$best1 = 2000;
$best2 = 2000;
$best3 = 2000;

$maps = [];
$signatures = [];
$content = file('input2.txt');
$boundaries = [
    TOP => 1,
    RIGHT => strlen(trim($content[0])) - 2,
    LEFT => 1,
    BOTTOM => count($content) - 2,
];

/**
 * Create the map
 */

foreach ($content as $y => $row) {
    foreach (str_split(trim($row)) as $x => $col) {
        if ($col === '#') continue;

        if ($y === 0) {
            $maps[0]["$x,$y"] = [];
            $start = [$x, $y];
            continue;
        }

        if ($y === count($content) - 1) {
            $maps[0]["$x,$y"] = [];
            $destination = [$x, $y];
            continue;
        }

        $maps[0]["$x,$y"] = $col === '.' ? [] : [match ($col) {
            '^' => TOP,
            '>' => RIGHT,
            '<' => LEFT,
            'v' => BOTTOM,
        }];
    }
}

function move($from, $to, &$best, $minutes) {
    global $maps;
    global $signatures;
    global $boundaries;

    if ($minutes >= $best) {
        return;
    }

    if (abs($from[0] - $to[0]) + abs($from[1] - $to[1]) + $minutes + 1 >= $best) {
        return;
    }

    // Verify signature
    $signature = $minutes . ',' . implode(',', $from);

    if (array_key_exists($signature, $signatures)) {
        return;
    }

    $signatures[$signature] = '';

    if (!array_key_exists($minutes, $maps)) {
        $maps[$minutes] = array_map(fn () => [], $maps[$minutes - 1]);

        foreach ($maps[$minutes - 1] as $coords => $elements) {
            [$x, $y] = array_map('intval', explode(',', $coords));

            foreach ($elements as $element) {
                $move = match ($element) {
                    RIGHT => [$x + 1, $y],
                    BOTTOM => [$x, $y + 1],
                    LEFT => [$x - 1, $y],
                    TOP => [$x, $y - 1],
                };

                if (!array_key_exists("$move[0],$move[1]", $maps[$minutes - 1])) {
                    $move = match ($element) {
                        RIGHT => [$boundaries[LEFT], $y],
                        BOTTOM => [$x, $boundaries[TOP]],
                        LEFT => [$boundaries[RIGHT], $y],
                        TOP => [$x, $boundaries[BOTTOM]],
                    };
                }

                $maps[$minutes]["$move[0],$move[1]"][] = $element;
            }
        }
    }

    $moves = $from[0] < $to[0] ? [
        [$from[0], $from[1] + 1],
        [$from[0] + 1, $from[1]],
        [$from[0], $from[1] - 1],
        [$from[0] - 1, $from[1]],
        [$from[0], $from[1]]
    ] : [
        [$from[0], $from[1] - 1],
        [$from[0] - 1, $from[1]],
        [$from[0], $from[1] + 1],
        [$from[0] + 1, $from[1]],
        [$from[0], $from[1]]
    ];

    foreach ($moves as $move) {
        if (!array_key_exists("$move[0],$move[1]", $maps[$minutes])) {
            continue;
        }

        if (!empty($maps[$minutes]["$move[0],$move[1]"])) {
            continue;
        }

        if ($move[0] === $to[0] && $move[1] === $to[1]) {
            $best = $minutes;
            return;
        }

        move($move, $to, $best, $minutes + 1);
    }
}

move($start, $destination, $best1, 1);

echo "$best1\n";

$maps = [$maps[$best1]];
$signatures = [];

move($destination, $start, $best2, 1);

echo "$best2\n";

$maps = [$maps[$best2]];
$signatures = [];

move($start, $destination, $best3, 1);

echo "$best3\n";

echo "Total: " . ($best1 + $best2 + $best3) . "\n";
```
