# Part 1

```php
collect(file('input.txt'))->map(fn ($cal) => (int)$cal)->chunkWhile(fn ($cal) => $cal > 0)->map(fn ($col) => $col->sum())->max()
```

# Part 2

```php
collect(file('input.txt'))->map(fn ($cal) => (int)$cal)->chunkWhile(fn ($cal) => $cal > 0)->map(fn ($col) => $col->sum())->sortDesc()->take(3)->sum()
```
