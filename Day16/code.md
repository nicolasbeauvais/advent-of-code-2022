# Part 1

```php
<?php

$valves = [];

foreach (file('input.txt') as $line) {
    preg_match('/^Valve\s(\w+)\s.*rate=(\d+).*valves?\s(.*)$/', $line, $valve);

    [$m, $id, $rate, $connect] = $valve;

    $valves[$id] = [
        'id' => $id,
        'rate' => (int)$rate,
        'connections' => explode(', ', $connect),
    ];
}

$bests = array_fill(1, 31, 0);

function explore($valve, $open, $minutes, $score) {
    global $valves;
    global $bests;

    if ($score < $bests[$minutes]) return;
    if ($score > $bests[$minutes]) $bests[$minutes] = $score;

    if ($minutes === 31) {
        return;
    }

    foreach ($open as $openValve) {
        $score += $openValve['rate'];
    }

    foreach ($valve['connections'] as $id) {
        explore($valves[$id], $open, $minutes + 1, $score);
    }

    if ($valve['rate'] > 0 && !array_key_exists($valve['id'], $open)) {
        $open[$valve['id']] = $valve;

        explore($valve, $open, $minutes + 1, $score);
    }
}

explore($valves['AA'], [], 1, 0);

echo $bests[31] . "\n";
```

# Part 2

```php
<?php


$valves = [];
$valvesToOpen = 0;
$maxRelease = 0;

foreach (file('input.txt') as $line) {
    preg_match('/^Valve\s(\w+)\s.*rate=(\d+).*valves?\s(.*)$/', $line, $valve);

    [$m, $id, $rate, $connect] = $valve;

    $valves[$id] = [
        'id' => $id,
        'rate' => (int)$rate,
        'connections' => explode(', ', $connect),
    ];

    if ($valves[$id]['rate'] > 0) {
        $valvesToOpen++;
        $maxRelease += $valves[$id]['rate'];
    }
}

$bestScore = 0;
$bestScoreAtMinute = array_fill(5, 22, 0);
function explore($valve1, $valve2, $open, $release, $minutes, $score) {
    global $valves;
    global $valvesToOpen;
    global $maxRelease;
    global $bestScore;
    global $bestScoreAtMinute;

    if ($minutes > 26) {
        if ($score > $bestScore) $bestScore = $score;

        return;
    }

    $score += $release;

    if ($score + (26 - $minutes) * $maxRelease <= $bestScore) return;

    if ($minutes > 10 && $minutes <= 15 && $score < $bestScoreAtMinute[$minutes] * 0.6) return;
    if ($minutes > 15 && $minutes <= 20 && $score <= $bestScoreAtMinute[$minutes] * 0.8) return;
    if ($minutes > 20 && $score <= $bestScoreAtMinute[$minutes] * 0.9) return;
    if ($minutes > 10 && $score > $bestScoreAtMinute[$minutes]) $bestScoreAtMinute[$minutes] = $score;

    $valve1Open = false;
    $valve2Open = false;
    $paths = [];

    foreach ($valve1['connections'] as $id1) {
        foreach ($valve2['connections'] as $id2) {
            if ($id1 === $id2 || array_key_exists("$id1,$id2", $paths) || array_key_exists("$id2,$id1", $paths)) continue;

            $paths["$id1,$id2"] = '';
            $paths["$id2,$id1"] = '';

            explore($valves[$id1], $valves[$id2], $open, $release, $minutes + 1, $score);
        }
    }

    if ($valve1['rate'] > 0 && !array_key_exists($valve1['id'], $open)) {
        $open[$valve1['id']] = '';
        $release += $valve1['rate'];
        $valve1Open = true;

        if (count($open) === $valvesToOpen) {
            $finalScore = $score + (26 - $minutes) * $release;

            if ($finalScore > $bestScore) $bestScore = $finalScore;

            return;
        }
    }

    if ($valve2['rate'] > 0 && !array_key_exists($valve2['id'], $open)) {
        $open[$valve2['id']] = '';
        $release += $valve2['rate'];
        $valve2Open = true;

        if (count($open) === $valvesToOpen) {
            $finalScore = $score + (26 - $minutes) * $release;

            if ($finalScore > $bestScore) $bestScore = $finalScore;

            return;
        }
    }

    if ($valve1Open && $valve2Open) {
        explore($valve1, $valve2, $open, $release, $minutes + 1, $score);
    }

    if ($valve1Open && !$valve2Open) {
        foreach ($valve2['connections'] as $id2) {
            if ($id2 === $valve1['id']) continue;
            explore($valve1, $valves[$id2], $open, $release, $minutes + 1, $score);
        }
    }

    if ($valve2Open && !$valve1Open) {
        foreach ($valve1['connections'] as $id1) {
            if ($id1=== $valve2['id']) continue;
            explore($valves[$id1], $valve2, $open, $release, $minutes + 1, $score);
        }
    }
}

explore($valves['AA'], $valves['AA'], [], 0, 1, 0);

echo $bestScore;
```
