# Part 1

```php
<?php

$bestScore = 0;
$signatures = [];
$iter = 0;
$qualityLevel = 0;

function simulate($factory, $robots, $resources, $boundaries, $minutes) {
    global $bestScore;
    global $iter;
    global $signatures;

    $iter++;

    $signature = implode(',', $robots) . implode(',', $resources) . ",$minutes";

    if (array_key_exists($signature, $signatures)) {
        return;
    }

    $signatures[$signature] = '';

    $robotTypes = [];

    foreach ($factory as $robotType => $factoryCosts) {
        // If we already produce >= what we are able to consume in a single turn, no need for a new robot
        if ($robots[$robotType] >= $boundaries[$robotType]) {
            continue;
        }

        // Verify if we have enough resources to construct this type of robot
        foreach ($factoryCosts as $costType => $cost) {
            if ($resources[$costType] < $cost) continue 2;
        }

        $robotTypes[] = $robotType;
    }

    $resources['ore'] += $robots['ore'];
    $resources['clay'] += $robots['clay'];
    $resources['obsidian'] += $robots['obsidian'];
    $resources['geode'] += $robots['geode'];

    if ($minutes === 24) {
        $bestScore = max($bestScore, $resources['geode']);
        return;
    }

    foreach ($robotTypes as $robotType) {
        $possibilityResources = $resources;

        foreach ($factory[$robotType] as $type => $cost) {
            $possibilityResources[$type] -= $cost;
        }

        $possibilityRobots = $robots;
        $possibilityRobots[$robotType]++;

        simulate($factory, $possibilityRobots, $possibilityResources, $boundaries, $minutes + 1);
    }

    if (
        $resources['ore'] < $boundaries['ore']
        || $resources['clay'] < $boundaries['clay']
        || $resources['obsidian'] < $boundaries['obsidian']
    ) {
        simulate($factory, $robots, $resources, $boundaries, $minutes + 1);
    }
}

foreach (file('input.txt') as $index => $line) {
    preg_match('/(\d+) ore.* (\d+) ore.* (\d+) ore.* (\d+) clay.* (\d+) ore.* (\d+) obsidian/', $line, $matches);

    $factory = [
        'ore' => [
            'ore' => (int)$matches[1],
        ],
        'clay' => [
            'ore' => (int)$matches[2],
        ],
        'obsidian' => [
            'ore' => (int)$matches[3],
            'clay' => (int)$matches[4],
        ],
        'geode' => [
            'ore' => (int)$matches[5],
            'obsidian' => (int)$matches[6],
        ]
    ];

    $robots = [
        'geode' => 0,
        'obsidian' => 0,
        'clay' => 0,
        'ore' => 1,
    ];

    $resources = [
        'ore' => 0,
        'clay' => 0,
        'obsidian' => 0,
        'geode' => 0,
    ];

    $boundaries = [
        'ore' => max([
            $factory['ore']['ore'],
            $factory['clay']['ore'],
            $factory['obsidian']['ore'],
            $factory['geode']['ore'],
        ]),
        'clay' => $factory['obsidian']['clay'],
        'obsidian' => $factory['geode']['obsidian'],
        'geode' => INF,
    ];

    simulate($factory, $robots, $resources, $boundaries, 1);

    echo "Computed blueprint " . ($index + 1) . " in $iter loops with $bestScore geodes\n";

    $qualityLevel += ($index + 1) * $bestScore;
    $bestScore = 0;
    $iter = 0;
    $signatures = [];
}

echo "Total quality level: $qualityLevel\n";
```

# Part 2

```php
<?php

$bestScore = 0;
$signatures = [];
$iter = 0;
$results = [];
function simulate($factory, $robots, $resources, $boundaries, $minutes) {
    global $bestScore;
    global $iter;
    global $signatures;

    $iter++;

    $signature = implode(',', $robots) . implode(',', $resources) . ",$minutes";

    if (array_key_exists($signature, $signatures)) {
        return;
    }

    $signatures[$signature] = '';

    $robotTypes = [];

    foreach ($factory as $robotType => $factoryCosts) {
        // If we already produce >= what we are able to consume in a single turn, no need for a new robot
        if ($robots[$robotType] >= $boundaries[$robotType]) {
            continue;
        }

        // Verify if we have enough resources to construct this type of robot
        foreach ($factoryCosts as $costType => $cost) {
            if ($resources[$costType] < $cost) continue 2;
        }

        $robotTypes[] = $robotType;
    }

    $resources['ore'] += $robots['ore'];
    $resources['clay'] += $robots['clay'];
    $resources['obsidian'] += $robots['obsidian'];
    $resources['geode'] += $robots['geode'];

    if ($minutes === 32) {
        $bestScore = max($bestScore, $resources['geode']);
        return;
    }

    foreach ($robotTypes as $robotType) {
        $possibilityResources = $resources;

        foreach ($factory[$robotType] as $type => $cost) {
            $possibilityResources[$type] -= $cost;
        }

        $possibilityRobots = $robots;
        $possibilityRobots[$robotType]++;

        simulate($factory, $possibilityRobots, $possibilityResources, $boundaries, $minutes + 1);
    }

    if (
        $resources['ore'] < $boundaries['ore']
        || $resources['clay'] < $boundaries['clay']
        || $resources['obsidian'] < $boundaries['obsidian']
    ) {
        simulate($factory, $robots, $resources, $boundaries, $minutes + 1);
    }
}

foreach (file('input.txt') as $index => $line) {
    if ($index > 2) {
        break;
    }

    preg_match('/(\d+) ore.* (\d+) ore.* (\d+) ore.* (\d+) clay.* (\d+) ore.* (\d+) obsidian/', $line, $matches);

    $factory = [
        'ore' => [
            'ore' => (int)$matches[1],
        ],
        'clay' => [
            'ore' => (int)$matches[2],
        ],
        'obsidian' => [
            'ore' => (int)$matches[3],
            'clay' => (int)$matches[4],
        ],
        'geode' => [
            'ore' => (int)$matches[5],
            'obsidian' => (int)$matches[6],
        ]
    ];

    $robots = [
        'geode' => 0,
        'obsidian' => 0,
        'clay' => 0,
        'ore' => 1,
    ];

    $resources = [
        'ore' => 0,
        'clay' => 0,
        'obsidian' => 0,
        'geode' => 0,
    ];

    $boundaries = [
        'ore' => max([
            $factory['ore']['ore'],
            $factory['clay']['ore'],
            $factory['obsidian']['ore'],
            $factory['geode']['ore'],
        ]),
        'clay' => $factory['obsidian']['clay'],
        'obsidian' => $factory['geode']['obsidian'],
        'geode' => INF,
    ];

    simulate($factory, $robots, $resources, $boundaries, 1);

    echo "Computed blueprint " . ($index + 1) . " in $iter loops with $bestScore geodes\n";

    $results[] = $bestScore;
    $bestScore = 0;
    $iter = 0;
    $signatures = [];
}

echo array_product($results) . "\n";
```
