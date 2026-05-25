
# Math is supposed to be fun. Generics mean fun.

Generic math functions.

```go
type Int    interface { ~int | ~uint | ~int8 | ~uint8 | ~int16 | ~uint16 | ~int32 | ~uint32 | ~int64 | ~uint64 }
type Float  interface { ~float32 | ~float64 }
type Number interface { Int | Float }


// this function (and Ceil) also accepts int's, witch is dumb, but makes it a better api to call.
func Floor[T Number](x T) int { return int(math.Floor(float64(x))); }
func Ceil[ T Number](x T) int { return int(math.Ceil( float64(x))); }


func Div_Ceil[T Int](x, y T) T { return (x + y - 1) / y; }

// floating point mod, always returns a number [0, 1)
// Modf but better.
func mod1[T Float](x T) T {
    _, a_f64 := math.Modf(float64(x));

    // assert(abs(a) < 1)
    a := T(a_f64);
    // if the sign was negative, make it positive,
    if a < 0 { return 1 + a;
    } else   { return a; }
}

func Clamp[T Number](x, mini, maxi T) T {
    // TODO remove this?
    if mini > maxi { panic("mini was bigger than maxi"); }

    if x < mini { return mini; }
    if x > maxi { return maxi; }
    return x;
}

func Lerp[T Float](a, b, t T) T { return (1-t)*a + t*b; }


func Abs[T Number](x T) T {
    // this would be faster if the type was known but w/e
    if x < 0 { x = -x; }
    return x;
}

// To Nearest Whole number
func Round[T Number](x T) int {
    // Adds 0.5, works for ints and floats, look out for overflow
    var rounded int;
    if x > 0 { rounded = int((x*2 + 1) / 2);
    } else {   rounded = int((x*2 - 1) / 2); }

    return rounded;
}

func Sloppy_Equal[T Float](a, b T) bool {
    // some small number
    const EPSILON = 0.000000001;
    return Abs(a - b) < EPSILON;
}

// outputs a number from [0, b). ignore the float64. go math module is dumb.
func Proper_Mod[T Float](a, b T) T {
    return T(math.Mod(math.Mod(float64(a), float64(b))+float64(b), float64(b)));
}

// i should make a int sqrt function for go...
// eh, ill just copy the c one.
func Sqrt[T Float] (x T) T { return T(math.Sqrt(float64(x))); }
```

If you need 2D Vector look into Fletcher's Website
