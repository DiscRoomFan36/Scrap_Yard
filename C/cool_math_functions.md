# C Cool Math Functions

## Int Sqrt

```c
// https://en.wikipedia.org/wiki/Integer_square_root
//
// this is pretty fast, but there is a functions that works even faster when provided with a good guess,
// we could guess the actual result the last time we called this,
u64 int_sqrt(u64 n) {
    u64 L = 1, R = n;
    while (L < R) {
        R = L + ((R - L) / 2);
        L = n / R;
    }
    return R;
}
```

## Int Pow

```c
s64 int_pow(s64 x, u64 y) {
    if (x == 0) return 0;

    s64 result = 1;
    s64 keep_squaring = x;
    while (y != 0) {
        if (y & 1) result *= keep_squaring;
        y = y >> 1;
        keep_squaring = keep_squaring*keep_squaring;
    }

    return result;
}

// handles the float edge cases, an can accept negative y
f64 f64_int_pow(f64 x, s64 y) {
    if (x == 0) return 0;

    if (y < 0) {
        x = 1 / x;
        y = -y;
    }

    f64 result = 1;
    f64 keep_squaring = x;
    while (y != 0) {
        if (y & 1) result *= keep_squaring;
        y = y >> 1;
        keep_squaring = keep_squaring*keep_squaring;
    }

    return result;
}
```

## Int Log

```c
// static_assert(int_log_n(int_pow(n, x), n) == x);

// all int log's round down.

u32 int_log_10(u64 x) {
    if (x == 0) return 0;
    u32 result = 0;
    while (x > 0) { result += 1; x /= 10; }
    return result - 1;
}

u32 int_log_2(u64 x) {
    if (x == 0) return 0;
    u32 result = 0;
    while (x > 0) { result += 1; x /=  2; }
    return result - 1;
}

u32 int_log_n(u64 x, u64 n) {
    if (x == 0) return 0;

    // these are much faster because constant division is faster.
    if (n == 2)  return int_log_2 (x);
    if (n == 10) return int_log_10(x);

    ASSERT(n != 0);
    u32 result = 0;
    while (x > 0) { result += 1; x /=  n; }
    return result-1;
}
```

