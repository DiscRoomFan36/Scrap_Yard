
# A Generic Print

This whole thing just makes me annoyed at `_Generic`

```c
void print_s64   (void *_x) { s64 x    = *(s64*)   _x; printf("%ld", x); }
void print_u64   (void *_x) { u64 x    = *(u64*)   _x; printf("%ld", x); }

void print_s32   (void *_x) { s32 x    = *(s32*)   _x; printf("%d",  x); }
void print_u32   (void *_x) { u32 x    = *(u32*)   _x; printf("%d",  x); }

void print_s16   (void *_x) { s16 x    = *(s16*)   _x; printf("%d",  x); }
void print_u16   (void *_x) { u16 x    = *(u16*)   _x; printf("%d",  x); }

void print_s8    (void *_x) { s8  x    = *(s8 *)   _x; printf("%d",  x); }
void print_u8    (void *_x) { u8  x    = *(u8 *)   _x; printf("%d",  x); }

void print_f32   (void *_x) { f32 x    = *(f32*)   _x; printf("%f",  x); }
void print_f64   (void *_x) { f64 x    = *(f64*)   _x; printf("%f",  x); }


void print_bool  (void *_x) { bool x   = *(bool*)  _x; printf("%s", x ? "true" : "false"); }

void print_string(void *_x) { String x = *(String*)_x; printf("\""S_Fmt"\"", S_Arg(x)); }

void print_range(void *_range) { Range range = *(Range*)_range; printf("%ld-%ld", range.start, range.end); }


void print_int_array(void *_array) {
    Int_Array array = *(Int_Array*)_array;
    printf("{\n    ");
    for (u64 i = 0; i < array.count; i++) {
        if (i != 0 && i % 10 == 0) printf("\n    ");
        printf("%6ld, ", array.items[i]);
    }
    printf("\n}");
}

void print_string_array(void *_array) {
    String_Array array = *(String_Array*)_array;
    printf("{\n");
    for (u64 i = 0; i < array.count; i++) {
        printf("    ");
        print_string(&array.items[i]);
        printf(",\n");
    }
    printf("}");
}

void print_range_array(void *_ranges) {
    Range_Array ranges = *(Range_Array*)_ranges;
    printf("{\n");
    for (u64 i = 0; i < ranges.count; i++) {
        printf("    ");
        print_range(&ranges.items[i]);
        printf(",\n");
    }
    printf("}\n");
}



// Cannot accept pointers to things, just use debug().
#define generic_print(x)                                    \
    _Generic(x,                                             \
        s64: print_s64(&x),  u64: print_u64(&x),            \
        s32: print_s32(&x),  u32: print_u32(&x),            \
        s16: print_s16(&x),  u16: print_u16(&x),            \
        s8 : print_s8 (&x),  u8 : print_u8 (&x),            \
        f32: print_f32(&x),  f64: print_f64(&x),            \
        bool: print_bool(&x),                               \
        String: print_string(&x),                           \
        Range: print_range(&x),                             \
        Int_Array: print_int_array(&x),                     \
        String_Array: print_string_array(&x),               \
        Range_Array: print_range_array(&x),                 \
        default: printf("?UNKNOWN_TYPE?")                   \
    )


#define debug(x)  do { Typeof(x) _x = (x); printf("DEBUG: %s = ", #x); generic_print(_x); printf("\n"); } while (0)
```

