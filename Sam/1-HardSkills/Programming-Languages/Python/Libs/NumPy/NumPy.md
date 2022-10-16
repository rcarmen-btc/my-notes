# NumPy

From: Documentation
Tags: Math, Python
cert: no

### Key words | Questions

### Notes

# **NumPy quickstart**

---

# **NumPy: the absolute basics for beginners**

---

## **What is an array?**

---

An array is a grid of values and it contains information about the raw data, how to locate an element, and how to interpret an element. It has a grid of elements that can be indexed in [various ways](https://numpy.org/doc/stable/user/quickstart.html#quickstart-indexing-slicing-and-iterating). The elements are all of the same type, referred to as the array `dtype`.

An array can be indexed by a tuple of nonnegative integers, by booleans, by another array, or by integers. The `rank` of the array is the number of dimensions. The `shape` of the array is a tuple of integers giving the size of the array along each dimension.

One way we can initialize NumPy arrays is from Python lists, using nested lists for two- or higher-dimensional data.

For example:

```python
a = np.array([1, 2, 3, 4, 5, 6])
# or
a = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12]])
```

## How to create a basic array

*This section covers* `np.array()`, `np.zeros()`, `np.ones()`, `np.empty()`, `np.arange()`, `np.linspace()`, `dtype`

---

```python
**import** numpy **as** np
a **=** np**.**array**([1,** **2,** **3])**
```

You can visualize your array this way:

![Untitled](NumPy%2059629/Untitled.png)

.Fill with zeros, ones, empty(random, better), range:

```python
z = np.zeros(2)

o = np.ones(2)

e = np.empty(2)

r = np.arange(10)
r = np.arange(2, 10, 2)
```

You can also use `np.linspace()` to create an array with values that are spaced linearly in a specified interval:

```python
l = np.linspace(0, 10, num=5)
# array([ 0. ,  2.5,  5. ,  7.5, 10. ])

```

**Specifying your data type**

While the default data type is floating point (`np.float64`), you can explicitly specify which data type you want using the `dtype` keyword.

```python
>>> x **=** np**.**ones**(2,** dtype**=**np**.**int64**)**
>>> x
*array([1, 1])*
```

**...**

# 

<aside>
📌 **SUMMARY:**

</aside>