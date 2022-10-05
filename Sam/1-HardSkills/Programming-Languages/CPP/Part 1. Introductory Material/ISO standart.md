Many important things are deemed implementation-defined by the standart. That is means that each implementation must provide specific, well-defined behavior for a construct and that behavior must be documented
For exampile:
```cpp
unsigned char c1 = 24;   // well defined: a char has at least 8 bits and can always hold 64
unsigned char c2 = 256;  // implementation-defined: truncation if a char has only 8 bits  
```

To maximaze portability, it is wise to be explicit about what implementation-defined features we rely on and to isolate the more subtle examples in clearly marked sections of a program.  A typical example of this practice is to present all dependencies on hardware sizes in the form of constants and type definitions in some header file. To support such techniques, the standard library provides [numeric_limits](numeric_limits)(§40.2). Many assumptions about implementation-defined features can be checked by stating them as static assertions (§2.4.3.3). 

For example:
```cpp
static_assert(4<=sizeof(int),"sizeof(int) too small");
```

Undefined behavior is nastier. A construct is deemed undefined by the standard if no reasonable behavior is required by an implementation. Typically, some obvious implementation technique will cause a program using an undefined feature to behave very badly. 

For example:
```cpp
const int size = 4∗1024;
char page[size];

void f()
{
	page[size+size] = 7; // undefined
}
```