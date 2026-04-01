
# Other Random Stuff


```go
// great function to have.
func Unsafe_Slice_Transmute[T any, U any](slice []T) []U {
	var x T; var y U;
	T_size := unsafe.Sizeof(x);
	U_size := unsafe.Sizeof(y);

	flag := false;
	if U_size >= T_size { flag = (U_size % T_size != 0);
	} else {              flag = (T_size % U_size != 0); }
	if flag { panic("T and U must have sizes that are multiples of each other"); }

	data := unsafe.Pointer(unsafe.SliceData(slice));
	size := uintptr(len(slice)) * T_size / U_size;
	return unsafe.Slice((*U)(data), size);
}
```


```go
// returns the string split into two pieces,
//
// if the separator was not in the string,
// before contains the whole string, and after is empty.
// (and found == false)
func split_once(str, sep string) (found bool, before, after string) {
    index := strings.Index(str, sep);
    if index == -1 { return false, str, ""; }

    _before := str[:index];
    _after  := str[index+len(sep):];

    return true, _before, _after;
}
```


```go
// returns an array 'is_not_prime' of bools with  is_not_prime[m] == true  if m is not a prime.
//
// up to but not including n.
func sieve_of_Eratosthenes(n int) []bool {
	is_not_prime := make([]bool, n);

	if n <= 0 { return is_not_prime; }
	is_not_prime[0] = true;
	if n <= 1 { return is_not_prime; }
	is_not_prime[1] = true;

	// do all evens for speed
	for i := 2 * 2; i < n; i += 2 { is_not_prime[i] = true; }

	curr_prime := 3;
	for curr_prime < n {
		// find the next prime.
		for (curr_prime < n) && is_not_prime[curr_prime] { curr_prime += 1; }

		// we know all evens are already done. so skip faster.
		for i := curr_prime * 3; i < n; i += curr_prime * 2 {
            is_not_prime[i] = true;
		}
		curr_prime += 1;
	}

	return is_not_prime;
}
```


```go
// this is O(n*len(nums))
//
// could be made O(log(n)*len(nums)) with a heap.
func Smallest_N_Numbers_In_Order(nums []int, n int) []int {
    if len(nums) < n { panic("array less than n"); }

    // can use math.MaxInt
    const INF = math.MaxInt;

    // this is a sorted array of the smallest N elements
    smallest_n := make([]int, n);

    // funny
    for i := range smallest_n { smallest_n[i] = nums[i]; }
    slices.Sort(smallest_n);

    for j := n; j < len(nums); j++ {
        num := nums[j];

        // check if this number needs to be placed in this array at all.
        if num >= smallest_n[len(smallest_n)-1] { continue; }

        // this could be technically faster, if you use a heap, and pop the top when its num is smaller.
        // but i dont like the look of golangs, heap container.
        index_to_insert, _ := slices.BinarySearch(smallest_n, num);
        Fixed_Size_Array_Insert(smallest_n, num, index_to_insert);
    }

    return smallest_n;
}
```
