# C Time Helpers

## Getting Time

```c
#include <time.h>

#define NANOSECONDS_PER_SECOND  (1000UL * 1000UL * 1000UL)

u64 nanoseconds_since_unspecified_epoch(void) {
#ifdef _WIN32
    #error "Sorry, haven't implemented this yet."
#else
    struct timespec ts;

    #ifndef CLOCK_MONOTONIC
        #define VSCODE_IS_DUMB___THIS_NEVER_HAPPENS
        #define CLOCK_MONOTONIC 69420
    #endif
    // don't worry, this will never trigger.
    assert(CLOCK_MONOTONIC != 69420);

    clock_gettime(CLOCK_MONOTONIC, &ts);

    return NANOSECONDS_PER_SECOND * ts.tv_sec + ts.tv_nsec;
#endif
}
```


## Printing time.
```c
typedef struct {
    u64 ns;
    u64 us;
    u64 ms;
    u64  s;

    f64 in_seconds;
} Time_In_Parts;

Time_In_Parts time_in_ns_to_time_in_parts(u64 time_in_ns) {
    Time_In_Parts result = {
        .ns = (time_in_ns           ) % 1000,
        .us = (time_in_ns / THOUSAND) % 1000,
        .ms = (time_in_ns / MILLION ) % 1000,
        . s = (time_in_ns / BILLION ) % 1000,

        // this might result in the loss of some accuracy,
        // but as long as the time is small it should be ok.
        .in_seconds = (f64)time_in_ns / NANOSECONDS_PER_SECOND,
    };
    return result;
}

#define print_time_in_parts(time)                                       \
    printf("%4lds, %4ldms, %4ldus, %4ldns", (time).s, (time).ms, (time).us, (time).ns)

#define print_time_ns_in_parts(time_ns)                                 \
    do {                                                                \
        Time_In_Parts time = time_in_ns_to_time_in_parts(time_ns);      \
        print_time_in_parts(time);                                      \
    } while (0)

//
// print formatted time, Kinda line `S_Fmt` and `S_Arg()` from `Bested.h`
//
#define Time_Fmt            "%4lds, %4ldms, %4ldus, %4ldns"
// Accepts a 'Time_In_Parts'
#define Time_Arg(t)         (time).s, (time).ms, (time).us, (time).ns
```

