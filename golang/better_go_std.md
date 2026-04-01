
# The Go Standard Library is Garbage

## Its annoying to work with Slices

```go
func Append[T any](slice *[]T, items ...T) {
	*slice = append(*slice, items...);
}
```

```go
func Pop[T any](slice *[]T) T {
	item  := (*slice)[ len(*slice)-1];
	*slice = (*slice)[:len(*slice)-1];
	return item;
}

func Pop_Safe[T any](slice *[]T) (bool, T) {
    var item T;
    if len(*slice) == 0 { return false, item; }

	item   = (*slice)[ len(*slice)-1];
	*slice = (*slice)[:len(*slice)-1];
	return true, item;
}
```

```go
// good old swap and remove
func Remove_Unordered[T any](slice *[]T, index int) {
	if index != len(*slice)-1 {
		(*slice)[index] = (*slice)[len(*slice)-1];
	}
	*slice = (*slice)[:len(*slice)-1];
}

func Remove_Ordered[T any](slice *[]T, index int) {
	*slice = append((*slice)[:index], (*slice)[index+1:]...);
}
```

```go
// keeps the array the same size, inserts an element.
func Fixed_Size_Array_Insert[T any](arr []T, item T, index int) {
    // copy() has memmove() properties, so lets do 1 big copy.
    //
    // if index == len(arr)-1, nothing happens, so its fine. :)
    copy(
        arr[index+1:len(arr)  ],
        arr[index  :len(arr)-1],
    )

    arr[index] = item;
}
```

## Maps

```go
func Contains[T comparable, U any](the_map map[T]U, item T) bool {
    _, found := the_map[item];
    return found;
}
```

## Grids

```go
// makes a grid, with default values. i hope go can turn this into something cool.
//
// might be better to make([]T, m*n) then grab some slices.
func make_grid[T any](n, m int) [][]T {
    grid := make([][]T, m);
    for j := range m { grid[j] = make([]T, n); }
    return grid;
}
```
