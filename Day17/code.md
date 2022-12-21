# Part 1

```php
<?php

$shapeRound = 0;
$jetRound = 0;
$highestRock = 0;
$shape = null;
$rocks = [
    '0,0' => '',
    '1,0' => '',
    '2,0' => '',
    '3,0' => '',
    '4,0' => '',
    '5,0' => '',
    '6,0' => '',
];

$jet = str_split(trim(file_get_contents('input.txt')));
$jetModulo = count($jet);
abstract class Shape {

    public array $coordinates;

    protected array $left = [];
    protected array $right = [];
    protected array $bottom = [];

    public function moveLeft(array $rocks): bool
    {
        foreach ($this->left as $index) {
            if (
                $this->coordinates[$index][0] - 1 <= -1
                || array_key_exists(($this->coordinates[$index][0] - 1) . ',' . $this->coordinates[$index][1], $rocks)
            ) {
                return false;
            }
        }

        foreach ($this->coordinates as &$coordinate) {
            $coordinate[0]--;
        }

        return true;
    }

    public function moveRight(array $rocks): bool
    {
        foreach ($this->right as $index) {
            if (
                $this->coordinates[$index][0] + 1 >= 7
                || array_key_exists(($this->coordinates[$index][0] + 1) . ',' . $this->coordinates[$index][1], $rocks)
            ) {
                return false;
            }
        }

        foreach ($this->coordinates as &$coordinate) {
            $coordinate[0]++;
        }

        return true;
    }

    public function moveDown($rocks): bool
    {
        foreach ($this->bottom as $index) {
            if (array_key_exists($this->coordinates[$index][0] . ',' . ($this->coordinates[$index][1] - 1), $rocks)) {
                return false;
            }
        }

        foreach ($this->coordinates as &$coordinate) {
            $coordinate[1]--;
        }

        return true;
    }

    public function highestRock(): int
    {
        return $this->coordinates[0][1];
    }
}

class HorizontalLine extends Shape {
    protected array $left = [0];
    protected array $right = [3];
    protected array $bottom = [0, 1, 2, 3];
    public function __construct($y)
    {
        $this->coordinates =  [
            [2, $y], [3, $y], [4, $y], [5, $y]
        ];
    }
}

class Cross extends Shape {
    protected array $left = [0, 1, 4];
    protected array $right = [0, 3, 4];
    protected array $bottom = [1, 3, 4];

    public function __construct($y)
    {
        $this->coordinates =  [
            /**      */  [3, $y + 2], /**      */
            [2, $y + 1], [3, $y + 1], [4, $y + 1],
            /**      */  [3, $y]      /**      */
        ];
    }
}

class Elbow extends Shape{
    protected array $left = [0, 1, 2];
    protected array $right = [0, 1, 4];
    protected array $bottom = [2, 3, 4];

    public function __construct($y)
    {
        $this->coordinates =  [
            /**  */  /**  */  [4, $y + 2],
            /**  */  /**  */  [4, $y + 1],
            [2, $y], [3, $y], [4, $y]
        ];
    }
}

class VerticalLine extends Shape{
    protected array $left = [0, 1, 2, 3];
    protected array $right = [0, 1, 2, 3];
    protected array $bottom = [3];

    public function __construct($y)
    {
        $this->coordinates =  [
            [2, $y + 3],
            [2, $y + 2],
            [2, $y + 1],
            [2, $y]
        ];
    }
}

class Square extends Shape{
    protected array $left = [0, 2];
    protected array $right = [1, 3];
    protected array $bottom = [2, 3];

    public function __construct($y)
    {
        $this->coordinates =  [
            [2, $y + 1], [3, $y + 1],
            [2, $y], [3, $y]
        ];
    }
}

while ($shapeRound < 2022) {
    $shape = $shape ?? match ($shapeRound % 5) {
        0 => new HorizontalLine($highestRock + 4),
        1 => new Cross($highestRock + 4),
        2 => new Elbow($highestRock + 4),
        3 => new VerticalLine($highestRock + 4),
        4 => new Square($highestRock + 4),
    };

    $jet[$jetRound % $jetModulo] === '>' ? $shape->moveRight($rocks) : $shape->moveLeft($rocks);

    $jetRound++;

    if ($shape->moveDown($rocks)) {
        continue;
    }

    foreach ($shape->coordinates as $coordinate) {
        $rocks["$coordinate[0],$coordinate[1]"] = '';
    }

    $highestRock = max($highestRock, $shape->highestRock());
    $shape = null;
    $shapeRound++;
}

echo $highestRock;
```

# Part 2

```php
<?php

$maxRound = 1000000000000;
$shapeRound = 0;
$jetRound = 0;
$highestRock = 0;
$signatures = [];
$shape = null;
$rocks = [
    '0,0' => '',
    '1,0' => '',
    '2,0' => '',
    '3,0' => '',
    '4,0' => '',
    '5,0' => '',
    '6,0' => '',
];

$jet = str_split(trim(file_get_contents('input.txt')));
$jetModulo = count($jet);
abstract class Shape {

    public array $coordinates;

    protected array $left = [];
    protected array $right = [];
    protected array $bottom = [];

    public function moveLeft(array $rocks): bool
    {
        foreach ($this->left as $index) {
            if (
                $this->coordinates[$index][0] - 1 <= -1
                || array_key_exists(($this->coordinates[$index][0] - 1) . ',' . $this->coordinates[$index][1], $rocks)
            ) {
                return false;
            }
        }

        foreach ($this->coordinates as &$coordinate) {
            $coordinate[0]--;
        }

        return true;
    }

    public function moveRight(array $rocks): bool
    {
        foreach ($this->right as $index) {
            if (
                $this->coordinates[$index][0] + 1 >= 7
                || array_key_exists(($this->coordinates[$index][0] + 1) . ',' . $this->coordinates[$index][1], $rocks)
            ) {
                return false;
            }
        }

        foreach ($this->coordinates as &$coordinate) {
            $coordinate[0]++;
        }

        return true;
    }

    public function moveDown($rocks): bool
    {
        foreach ($this->bottom as $index) {
            if (array_key_exists($this->coordinates[$index][0] . ',' . ($this->coordinates[$index][1] - 1), $rocks)) {
                return false;
            }
        }

        foreach ($this->coordinates as &$coordinate) {
            $coordinate[1]--;
        }

        return true;
    }

    public function highestRock(): int
    {
        return $this->coordinates[0][1];
    }
}

class HorizontalLine extends Shape {
    protected array $left = [0];
    protected array $right = [3];
    protected array $bottom = [0, 1, 2, 3];
    public function __construct($y)
    {
        $this->coordinates =  [
            [2, $y], [3, $y], [4, $y], [5, $y]
        ];
    }
}

class Cross extends Shape {
    protected array $left = [0, 1, 4];
    protected array $right = [0, 3, 4];
    protected array $bottom = [1, 3, 4];

    public function __construct($y)
    {
        $this->coordinates =  [
            /**      */  [3, $y + 2], /**      */
            [2, $y + 1], [3, $y + 1], [4, $y + 1],
            /**      */  [3, $y]      /**      */
        ];
    }
}

class Elbow extends Shape{
    protected array $left = [0, 1, 2];
    protected array $right = [0, 1, 4];
    protected array $bottom = [2, 3, 4];

    public function __construct($y)
    {
        $this->coordinates =  [
            /**  */  /**  */  [4, $y + 2],
            /**  */  /**  */  [4, $y + 1],
            [2, $y], [3, $y], [4, $y]
        ];
    }
}

class VerticalLine extends Shape{
    protected array $left = [0, 1, 2, 3];
    protected array $right = [0, 1, 2, 3];
    protected array $bottom = [3];

    public function __construct($y)
    {
        $this->coordinates =  [
            [2, $y + 3],
            [2, $y + 2],
            [2, $y + 1],
            [2, $y]
        ];
    }
}

class Square extends Shape{
    protected array $left = [0, 2];
    protected array $right = [1, 3];
    protected array $bottom = [2, 3];

    public function __construct($y)
    {
        $this->coordinates =  [
            [2, $y + 1], [3, $y + 1],
            [2, $y], [3, $y]
        ];
    }
}

function rockSignature($rocks, $highestRock): string
{
    $rockSignature = [];

    foreach (array_slice($rocks, -100) as $rock => $value) {
        [$x, $y] = explode(',', $rock);

        $rockSignature[] = "$x," . ($highestRock - (int)$y);
    }

    return implode(',', $rockSignature);
}

while ($shapeRound < $maxRound) {
    $shape = $shape ?? match ($shapeRound % 5) {
        0 => new HorizontalLine($highestRock + 4),
        1 => new Cross($highestRock + 4),
        2 => new Elbow($highestRock + 4),
        3 => new VerticalLine($highestRock + 4),
        4 => new Square($highestRock + 4),
    };

    $jet[$jetRound % $jetModulo] === '>' ? $shape->moveRight($rocks) : $shape->moveLeft($rocks);

    $jetRound++;

    if ($shape->moveDown($rocks)) {
        continue;
    }

    foreach ($shape->coordinates as $coordinate) {
        $rocks["$coordinate[0],$coordinate[1]"] = '';
    }

    $highestRock = max($highestRock, $shape->highestRock());
    $shape = null;
    $shapeRound++;

    if ($jetRound > $jetModulo && is_array($signatures)) {
        $signature = implode(',', [$jetRound % $jetModulo, $shapeRound % 4, rockSignature($rocks, $highestRock)]);

        if (array_key_exists($signature, $signatures)) {
            [$prevShapeRound, $prevHighestRock] = $signatures[$signature];

            $shapeRoundDiff = $shapeRound - $prevShapeRound;
            $highestRockDiff = $highestRock - $prevHighestRock;

            $remaining = ($maxRound - $prevShapeRound) % $shapeRoundDiff;
            $multiplier = ($maxRound - $prevShapeRound - $remaining) / $shapeRoundDiff;

            $shapeRound = $prevShapeRound + ($shapeRoundDiff * $multiplier);
            $newHighestRock = $prevHighestRock + ($highestRockDiff * $multiplier);
            $yMove = $newHighestRock - $highestRock;
            $highestRock = $newHighestRock;

            $newRocks = [];

            foreach (array_slice($rocks, -200) as $key => $value) {
                [$x, $y] = explode(',', $key);

                $newRocks["$x," . ($yMove + (int)$y)] = '';
            }

            $rocks = $newRocks;

            $signatures = null;
            continue;
        }

        $signatures[$signature] = [$shapeRound, $highestRock];
    }
}

echo $highestRock;
```
