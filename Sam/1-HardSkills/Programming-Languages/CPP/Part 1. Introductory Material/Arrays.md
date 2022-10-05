An array is C++’s fundamental way of representing a sequence of objects in memory. 
If what you want is a simple fixed-length sequence of objects of a given type in 
memory, an array is the ideal solution. For every other need, an array has serious 
problems.

An array can be allocated statically, on the [../../../Устройство компьютера/Аппаратный стек.md](../../../Устройство%20компьютера/Аппаратный%20стек.md), and on the free store. 
For example:
```cpp
int a1[10];  // 10 ints in static storage

void f()
{
	int a2 [20];
	int∗p = new int[40];
	// ...
}
```
That’s most easily and most reliably done by having the lifetime of the free-store
array controlled by a resource handle (e.g., [string](string), [vector](vector), or [unique_ptr](unique_ptr)). 

We can initialize like this:
```cpp
int ar[] = {1, 2, 3}
char v2[] = { 'a', 'b', 'c', 0 };
```

When you need assignment to a collection of objects, use a [vector](vector), an [array](array), or a [valarray](valarray) instead. An array of characters can be conveniently initialized by a [string literal](string%20literal).