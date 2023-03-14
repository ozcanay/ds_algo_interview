# Can ```O(n)``` be faster than ```O(1)```?

```O(1)``` just means that the process takes about the same amount of time regardless of the size of the input. ```O(N)``` means the time is proportional to the size of the input, so if you double the size of the input, you roughly double the time. ```O``` describes the way the time changes in relation to the size of the input to the process, **not the running time.**

So, ```O(n)``` linear time can absolutely be faster than ```O(1)``` constant time. The reasons are:

- Constant-time operations are totally ignored in Big ```O```, which is a measure of how fast an algorithm's complexity increases as input size ```n``` increases, and nothing else. 

Here's an example:

```
int constant(int[] arr) {
    int total = 0;

    for (int i = 0; i < 10000; i++) {
         total += arr[0];
    }

    return total;
}
   
int linear(int[] arr) {
    int total = 0;        

    for (int i = 0; i < arr.length; i++) {
        total += arr[i];
    }

    return total;
}
```

In this case, constant does a lot of work, but it's fixed work that will always be the same regardless of how large ```arr``` is. linear, on the other hand, appears to have few operations, but those operations are dependent on the size of ```arr```.

In other words, as ```arr``` increases in length, constant's performance stays the same, but linear's running time increases linearly in proportion to its argument array's size.

Call the two functions with a single-item array like

```
constant(new int[] {1}); 
linear(new int[] {1});
```

and it's clear that constant runs slower than linear.

But call them like:

```
int[] arr = new int[10000000];
constant(arr);
linear(arr);
```

Which runs slower?: It depends on ```n```.

Big O is a measure of scaling. If you have a fixed number of items, every algorithm is ```O(1)```.

#

Another example:

```
void exponential(int n) {
  int iterations = pow(2,n);
  for (int i=0; i < iterations; i++) {
    sleep(1);
  }
}

void linear(int n) {
  int iterations = n;
  for (int i=0; i < iterations; i++) {
    sleep(20);
  }
}
```

Each approach requires an constant amount of processing per iteration (I used sleep as a lazy placeholder, but assume there are gears grinding there). Considering Big-O analysis, the exponential function complexity is ```O(2^n)``` and the linear function complexity is ```O(n)```.

However, the constant work performed in the linear solution is 20 times more expensive compared to the exponential case.

If our solution will operate on inputs where ```n``` < 20, the exponential solution will perform fewer operations and will run faster than the linear one. In this case, the constant factor is the primary contributor to our runtime.

- Cache matters! The way your data is stored in memory and the implementation of the data structures you use in your preferred language will have a big impact on runtime. When considering CPU cycles, RAM access time is about 50 times slower than accessing the L1 cache, making cache misses very expensive. This behavior can result (although language specific) in a linear search in an array being much faster than a hash lookup, providing the array fits entirely in a cache line. How?

Hash tables implemented with separate chaining and linked lists could potentially suffer from poor memory locality (keys and values are not “clustered” together). Moreover, traversing the linked list of values in a specific bucket makes it hard to for the CPU to know the next node in advance, leaving little room for optimizations such as cache prefetching.

### Summary

```O``` is a measure of growth rate, not running time.

To sum it up, when it comes to making programs run faster, don’t speculate!

Big-O will help you explain how your solution scales with size, but profiling your code on your intended platform is the way to go when trying to reason about runtime performance.

# Is total number of distinct unsigned 32 bit integers different from signed 32 bit integers?

While the total number of distinct unsigned 32-bit integers is the same as the total number of positive signed 32-bit integers, the distribution of values across the range is different.

A signed integer is a 32-bit datum that encodes an integer in the range [-2147483648 to 2147483647]. An unsigned integer is a 32-bit datum that encodes a nonnegative integer in the range [0 to 4294967295].

The signed integer is represented in twos complement notation.
