
# C Misc Stuff

## My C Header

```c
//
// Prime_Generator.h - generate a list of primes efficiently.
//
// This is a single header file.
//
// Author   - Fletcher M
//
// Created  - 12/01/26
// Modified - 15/01/26
//
// Make sure to...
//      #define PRIME_GENERATOR_IMPLEMENTATION
//      #include "Prime_Generator.h"
// ...somewhere in your project
//
```


## Some Cool Functions

### I All-ways Mess Binary Search Up

```c
u64 binary_search(Int_Array array, s64 x) {
    s64 low = 0;
    s64 high = array.count-1;
    while (low <= high) {
        s64 mid = low + (high - low) / 2;

        // Check if x is present at mid
        // if (all_invalid_ids.items[mid] == range.start) {
        //     break;
        // }

        if (array.items[mid] < x) {
            // If x greater, ignore left half
            low = mid + 1;

        } else {
            // If x is smaller, ignore right half
            high = mid - 1;
        }
    }
    return low;
}
```

### Read An Entire File, Maybe

```c
#include <stdlib.h>

#define MAX_TEMP_FILE_SIZE      (1 * MEGABYTE)
// overwrites temporary buffer every call.
internal String temp_read_entire_file(const char *filename) {
    local_persist u8 temp_file_storeage[MAX_TEMP_FILE_SIZE];

    FILE *file = fopen(filename, "rb");
    String result = ZEROED;
    result.data = (void*) temp_file_storeage;

    if (file) {
        fseek(file, 0, SEEK_END);
        s64 size = ftell(file);
        fseek(file, 0, SEEK_SET);

        if (size >= (s64)MAX_TEMP_FILE_SIZE-1) {
            PANIC("FILE IS TO BIG, WAS %.02fMB", (f64)size / (f64)MEGABYTE);
        }

        if (size >= 0) {
            result.length = size;
            fread(result.data, 1, result.length, file);
            result.data[result.length] = 0; // zero terminate for fun.
        }
        fclose(file);
    }

    return result;
}
```


### Sorting An Array Example

```c
int compare_ints(const void *_a, const void *_b) {
    s64 a = *(s64*)_a;
    s64 b = *(s64*)_b;
    if      (a < b)  return -1;
    else if (a > b)  return  1;
    else             return  0;
}

// smallest to largest
void sort_int_array(Int_Array *array) {
    qsort(array->items, array->count, sizeof(array->items[0]), compare_ints);
}
```

