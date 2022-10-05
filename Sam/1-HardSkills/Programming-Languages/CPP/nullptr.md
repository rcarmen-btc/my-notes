The literal nullptr represents the null pointer, that is, a pointer that does not point to
an object. It can be assigned to any pointer type, but not to other built-in types:
```cpp
int∗ pi = nullptr;
double∗ pd = nullptr;
int i = nullptr; // error : i is not a pointer
```
There is just one `nullptr`, which can be used for every pointer type, rather than a null
pointer for each pointer type.

Before nullptr was introduced, zero (`0`) was used as a notation for the null pointer.
For example:
```cpp
int∗ x = 0; // x gets the value nullptr
```
No object is allocated with the address `0`, and `0` (the all-zeros bit pattern) is the most 
common representation of nullptr. Zero (`0`) is an `int`. However, the standard 
conversions allow `0` to be used as a constant of pointer or pointer-to-member type.
It has been popular to define a macro `NULL` to represent the null pointer. 
For example:
```cpp
int∗ p = NULL; // using the macro NULL. However, there are differences in the efinition
```
of `NULL` in different implementations; for example, `NULL` might be `0` or `0L`. In C, `NULL`
is typically `(void∗)0`, which makes it illegal in C++:
```cpp
int∗ p = NULL; // error: can’t assign a void* to an int*
```
Using nullptr makes code more readable than alternatives and avoids potential
confusion when a function is overloaded to accept either a pointer or an integer.