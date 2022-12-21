# Part 1

```php
<?php

$file = file('input.txt');
$fileCount = count($file);

$value = current($file);

while (true) {
    if (!is_int($value)) {
        $value = (int)$value;
        $key = key($file);

        if ($value === 0) {
            $file[$key] = 0;
        } else {
            $position = ($key + $value) % ($fileCount - 1);

            unset($file[$key]);

            $file = array_values($file);

            $position === 0 ? $file[] = $value : array_splice($file, $position, 0, $value);
        }

        $value = reset($file);
    } else {
        $value = next($file);
    }

    if ($value === false) {
        break;
    }
}

$zero = array_search(0, $file);

echo $file[($zero + 1000) % $fileCount] + $file[($zero + 2000) % $fileCount] + $file[($zero + 3000) % $fileCount] . "\n";
```

# Part 2

```php
<?php

$file = file('input.txt');

foreach ($file as $index => &$line) {
    $line = [(int)$line * 811589153, $index];
} unset($line);

$decrypt = 10;
$fileCount = count($file);

while ($decrypt) {
    foreach (range(0, $fileCount - 1) as $order) {
        foreach ($file as $index => $line) {
            if ($line[1] !== $order) continue;

            break;
        }

        if ($line[0] === 0) {
            $file[$index] = $line;
        } else {
            $position = ($index + $line[0]) % ($fileCount - 1);

            unset($file[$index]);

            $file = array_values($file);

            $position === 0 ? $file[] = $line : array_splice($file, $position, 0, [$line]);
        }

        $value = reset($file);
    }

    $decrypt--;
}

foreach ($file as $index => $line) {
    if ($line[0] === 0) break;
}

echo $file[($index + 1000) % $fileCount][0] + $file[($index + 2000) % $fileCount][0] + $file[($index + 3000) % $fileCount][0] . "\n";
```
