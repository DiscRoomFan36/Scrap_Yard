
# C Typedefs & Macros


## Never leave home without them

```c
typedef uint64_t        u64;
typedef uint32_t        u32;
typedef uint16_t        u16;
typedef uint8_t         u8;

typedef int64_t         s64;
typedef int32_t         s32;
typedef int16_t         s16;
typedef int8_t          s8;

typedef u64             b64;
typedef u32             b32;
typedef u16             b16;
typedef u8              b8;

typedef float           f32;
typedef double          f64;
```


## Common C Macros

```c
#define ARRAY_LEN(xs)   (sizeof(xs) / sizeof((xs)[0]))
```

```c
// I always forget how to call typeof()
#define Typeof(x)       __typeof__(x)
```

```c
// Not wrapping stuff in () because this is purely text based, nothing fancy can be done here.
#define Member(type, member)        ( ((type*)0)->member )
#define Member_Size(type, member)   sizeof( Member(type, member) )
```

Not sure where to put this one.
```c
#define debug_break() asm("int3")
```


## Math Macros

```c
#define Min(a, b)   ({ Typeof(a) _a = (a); Typeof(b) _b = (b); _a < _b ? _a : _b; })
#define Max(a, b)   ({ Typeof(a) _a = (a); Typeof(b) _b = (b); _a > _b ? _a : _b; })
```

```c
#define Div_Ceil(x, y)      (((x) + (y) - 1) / (y))
#define Div_Floor(x, y)     ((x) / (y))

#define Proper_Mod(x, y) ((((x) % (y)) + (y)) % (y))
```

```c
// integers only
#define Is_Pow_2(n)                 (((n) != 0) && (((n) & ((n)-1)) == 0))
```

```c
#define Sign(T, x)  ((T)((x) > 0) - (T)((x) < 0))
#define Abs(x)      (Sign(Typeof(x), (x)) * (x))

// TODO this has the same problem as Min & Max
#define Clamp(x, min, max)              ((x) < (min) ? (min) : (((x) > (max)) ? (max) : (x)))

#define Is_Between(x, lower, upper)     ({ Typeof(x) _x = (x); ((lower) <= _x) && (_x <= (upper)); })
```


## Constants

```c
#define PI              3.1415926535897932384626433
#define TAU             6.283185307179586476925286766559
#define E               2.718281828459045235360287471352
#define ROOT_2          1.414213562373095048801688724209

#define KILOBYTE        (1024UL)
#define MEGABYTE        (1024UL * 1024UL)
#define GIGABYTE        (1024UL * 1024UL * 1024UL)
#define TERABYTE        (1024UL * 1024UL * 1024UL * 1024UL)

#define THOUSAND        (1000UL)
#define MILLION         (1000UL * 1000UL)
#define BILLION         (1000UL * 1000UL * 1000UL)
#define TRILLION        (1000UL * 1000UL * 1000UL * 1000UL)
```

