# Part 1

```php
<?php

$elves = [];
$moves = [];
$boundaries = [
    'N' => -INF,
    'S' => INF,
    'E' => -INF,
    'W' => INF,
];

$playbook = [
    ['N', 'NE', 'NW'],
    ['S', 'SE', 'SW'],
    ['W', 'NW', 'SW'],
    ['E', 'NE', 'SE'],
];

foreach (file('input.txt') as $y => $row) {
    foreach (str_split(trim($row)) as $x => $col) {
        if ($col === '.') continue;

        $elves["$x,$y"] = [$x, $y];
    }
}


foreach (range(1, 10) as $round) {
    foreach ($elves as $elfKey => $elf) {
        [$x, $y] = $elf;

        $coordinates = [
            'N' => [$x, $y - 1],
            'NE' => [$x + 1, $y - 1],
            'E' => [$x + 1, $y],
            'SE' => [$x + 1, $y + 1],
            'S' => [$x, $y + 1],
            'SW' => [$x - 1, $y + 1],
            'W' => [$x - 1, $y],
            'NW' => [$x - 1, $y - 1],
        ];

        $noNeighbourElf = true;

        foreach ($coordinates as $coordinate) {
            if (array_key_exists(implode(',', $coordinate), $elves)) {
                $noNeighbourElf = false;
            }
        }

        if ($noNeighbourElf) {
            continue;
        }

        foreach ($playbook as $directions) {
            foreach ($directions as $direction) {
                if (array_key_exists(implode(',', $coordinates[$direction]), $elves)) {
                    continue 2;
                }
            }

            $moveKey = implode(',', $coordinates[$directions[0]]);

            if (array_key_exists($moveKey, $moves)) {
                $moves[$moveKey][] = $elfKey;
            } else {
                $moves[$moveKey] = [$elfKey];
            }

            break;
        }
    }

    foreach ($moves as $coordinates => $movingElves) {
        if (count($movingElves) > 1) {
            continue;
        }

        unset($elves[$movingElves[0]]);
        $elves[$coordinates] = array_map('intval', explode(',', $coordinates));
    }

    $moves = [];
    $playbook[] = array_shift($playbook);
}

// Find boundaries
foreach ($elves as $elf) {
    [$x, $y] = $elf;

    $boundaries['N'] = max($boundaries['N'], $y);
    $boundaries['S'] = min($boundaries['S'], $y);
    $boundaries['E'] = max($boundaries['E'], $x);
    $boundaries['W'] = min($boundaries['W'], $x);
}

$groundTiles = 0;

echo ($boundaries['N'] + 1 - $boundaries['S']) * ($boundaries['E'] + 1 - $boundaries['W']) - count($elves);
```

# Part 2

```php
<?php

$round = 1;
$elves = [];
$moves = [];
$boundaries = [
    'N' => -INF,
    'S' => INF,
    'E' => -INF,
    'W' => INF,
];

$playbook = [
    ['N', 'NE', 'NW'],
    ['S', 'SE', 'SW'],
    ['W', 'NW', 'SW'],
    ['E', 'NE', 'SE'],
];

foreach (file('input.txt') as $y => $row) {
    foreach (str_split(trim($row)) as $x => $col) {
        if ($col === '.') continue;

        $elves["$x,$y"] = [$x, $y];
    }
}


while (true) {
    foreach ($elves as $elfKey => $elf) {
        [$x, $y] = $elf;

        $coordinates = [
            'N' => [$x, $y - 1],
            'NE' => [$x + 1, $y - 1],
            'E' => [$x + 1, $y],
            'SE' => [$x + 1, $y + 1],
            'S' => [$x, $y + 1],
            'SW' => [$x - 1, $y + 1],
            'W' => [$x - 1, $y],
            'NW' => [$x - 1, $y - 1],
        ];

        $noNeighbourElf = true;

        foreach ($coordinates as $coordinate) {
            if (array_key_exists(implode(',', $coordinate), $elves)) {
                $noNeighbourElf = false;
            }
        }

        if ($noNeighbourElf) {
            continue;
        }

        foreach ($playbook as $directions) {
            foreach ($directions as $direction) {
                if (array_key_exists(implode(',', $coordinates[$direction]), $elves)) {
                    continue 2;
                }
            }

            $moveKey = implode(',', $coordinates[$directions[0]]);

            if (array_key_exists($moveKey, $moves)) {
                $moves[$moveKey][] = $elfKey;
            } else {
                $moves[$moveKey] = [$elfKey];
            }

            break;
        }
    }

    foreach ($moves as $coordinates => $movingElves) {
        if (count($movingElves) > 1) {
            continue;
        }

        unset($elves[$movingElves[0]]);
        $elves[$coordinates] = array_map('intval', explode(',', $coordinates));
    }

    if (empty($moves)) {
        echo $round;
        break;
    }

    $round++;
    $moves = [];
    $playbook[] = array_shift($playbook);
}
```
