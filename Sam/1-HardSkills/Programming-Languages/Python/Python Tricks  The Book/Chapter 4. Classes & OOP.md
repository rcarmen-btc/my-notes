# Chapter 4. Classes & OOP
## 4.1 Object Comparisons: “is” vs “\=\=”
The **\=\=** operator compares by checking for equality: if these cats were
Python objects and we compared them with the == operator, we’d get
“both cats are equal” as an answer.

The **is** operator, however, compares identities: if we compared our
cats with the is operator, we’d get “these are two different cats” as an
answer.

```python
a = [i for i in range(10)]
b = a

c = [i for i in range(10)]
d = list(a)

print(a == b)
# True

print(a == c)
# True

print(a is b)
# True

print(a is c)
# False

print(a is d)
# False
```

• An is expression evaluates to True if two variables point to the same (identical) object.
• An == expression evaluates to True if the objects referred to by the variables are equal (have the same contents).

## 4.2 String Conversion (Every Class Needs a \_\_repr\_\_)
```python
class Car:
	def __init__(self, color, mileage):
		self.color = color
		self.mileage = mileage

>>> my_car = Car('red', 37281)
>>> print(my_car)
<__console__.Car object at 0x109b73da0>
>>> my_car
<__console__.Car object at 0x109b73da0>
```
Let’s take a look at how these methods work in practice. To get started, we’re going to add a __str__ method to the Car class we defined earlier:
``` python
class Car:
	def __init__(self, color, mileage):
		self.color = color
		self.mileage = mileage

	def __str__(self):
		return f'a {self.color} car'
```
When you try printing or inspecting a Car instance now, you’ll get a different, slightly improved result:
```python
>>> my_car = Car('red', 37281)
>>> print(my_car)
'a red car'
>>> my_car
<__console__.Car object at 0x109ca24e0>
>>> str(my_car)
'a red car'
>>> '{}'.format(my_car)
'a red car'
```

### \_\_str__ vs \_\_repr\_\_
```python
class Car:
	def __init__(self, color, mileage):
		self.color = color
		self.mileage = mileage
	def __repr__(self):
		return '__repr__ for Car'
	def __str__(self):
		return '__str__ for Car'

>>> my_car = Car('red', 37281)
>>> print(my_car)
__str__ for Car
>>> '{}'.format(my_car)
'__str__ for Car'
>>> my_car
__repr__ for Car
```

Interestingly, containers like lists and dicts always use the result of \_\_repr\_\_to represent the objects they contain.


### Why Every Class Needs a \_\_repr\_\_
Here’s how to add basic string conversion support to your classes
quickly and efficiently. For our Car class we might start with the
following __repr__:
```python
def __repr__(self):
	return f'Car({self.color!r}, {self.mileage!r})'
```
The benefit is you won’t have to modify the __repr__ implementation
when the class name changes. This makes it easy to adhere to the
Don’t Repeat Yourself (DRY) principle:
```python
def __repr__(self):
	return (f'{self.__class__.__name__}('
			f'{self.color!r}, {self.mileage!r})')

>>> repr(my_car)
'Car(red, 37281)'
```

Printing the object or calling str() on it returns the same string be-
cause the default __str__ implementation simply calls __repr__:
```python
>>> print(my_car)
'Car(red, 37281)'
>>> str(my_car)
'Car(red, 37281)'
```

### Key Takeaways
• You can control to-string conversion in your own classes using
the __str__ and __repr__ “dunder” methods.
• The result of __str__ should be readable. The result of
__repr__ should be unambiguous.
• Always add a __repr__ to your classes. The default implemen-
tation for __str__ just calls __repr__.
• Use __unicode__ instead of __str__ in Python 2.

## 4.3 Defining Your Own Exception Classes
Let’s say you wanted to validate an input string representing a per-
son’s name in your application. A toy example for a name validator
```python
def validate(name):
	if len(name) < 10:
		raise ValueError
```

If the validation fails, it throws a ValueError exception. That seems
fitting and kind of Pythonic already. So far, so good.
However, there’s a downside to using a “high-level” generic exception
class like ValueError. Imagine one of your teammates calls this func-
tion as part of a library and doesn’t know much about its internals.
When a name fails to validate, it’ll look like this in the debug stack
trace:
```python
>>> validate('joe')
Traceback (most recent call last):
	File "<input>", line 1, in <module>
	validate('joe')
	File "<input>", line 3, in validate
	raise ValueError
ValueError
```
This stack trace isn’t really all that helpful. Sure, we know that some-
thing went wrong and that the problem had to do with an “incorrect
value” of sorts, but to be able to fix the problem your teammate almost
certainly has to look up the implementation of validate(). However,
reading code costs time. And it can add up quickly.

Luckily we can do better. Let’s introduce a custom exception type to
represent a failed name validation. We’ll base our new exception class
on Python’s built-in ValueError, but make it speak for itself by giving
it a more explicit name:
```python
class NameTooShortError(ValueError):
	pass
def validate(name):
	if len(name) < 10:
		raise NameTooShortError(name)
```

Now we have a “self-documenting” NameTooShortError exception
type that extends the built-in ValueError class. Generally, you’ll want
to either derive your custom exceptions from the root Exception
class or the other built-in Python exceptions like ValueError or
TypeError—whicever feels appropriate.

Here’s how to create a custom exception hierarchy for all exceptions
in a module or package. The first step is to declare a base class that
all of our concrete errors will inherit from:
```python
class BaseValidationError(ValueError):
	pass
```

Now, all of our “real” error classes can be derived from the base error
class. This gives a nice and clean exception hierarchy with little extra
effort:
```python
class NameTooShortError(BaseValidationError):
	pass
class NameTooLongError(BaseValidationError):
	pass
class NameTooCuteError(BaseValidationError):
	pass
```

For example, this allows users of your package to write try…except
statements that can handle all of the errors from this package without
having to catch them manually:
```python
try:
	validate(name)
except BaseValidationError as err:
	handle_validation_error(err)
```

### Key Takeaways
• Defining your own exception types will state your code’s intent
more clearly and make it easier to debug.
• Derive your custom exceptions from Python’s built-in
Exception class or from more specific exception classes
like ValueError or KeyError.
• You can use inheritance to define logically grouped exception
hierarchies.

## 4.4 Python and some of the caveats involved.
Let’s start by looking at how to copy Python’s built-in collections.
Python’s built-in mutable collections like lists, dicts, and sets can be
copied by calling their factory functions on an existing collection:
```python
new_list = list(original_list)
new_dict = dict(original_dict)
new_set = set(original_set)
```

### Making Shallow Copies
In the example below, we’ll create a new nested list and then shallowly
``` python
copy it with the list() factory function:
>>> xs = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
>>> ys = list(xs) # Make a shallow copy
```
This means ys will now be a new and independent object with the
same contents as xs. You can verify this by inspecting both objects:
```python
>>> xs
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
>>> ys
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```
To confirm ys really is independent from the original, let’s devise a
little experiment. You could try and add a new sublist to the original
(xs) and then check to make sure this modification didn’t affect the
copy (ys):
```python
>>> xs.append(['new sublist'])
>>> xs
[[1, 2, 3], [4, 5, 6], [7, 8, 9], ['new sublist']]
>>> ys
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```
As you can see, this had the expected effect. Modifying the copied list
at a “superficial” level was no problem at all.

However, because we only created a shallow copy of the original list,
ys still contains references to the original child objects stored in xs.
These children were not copied. They were merely referenced again
in the copied list.
Therefore, when you modify one of the child objects in xs, this modi-
fication will be reflected in ys as well—that’s because both lists share
the same child objects. The copy is only a shallow, one level deep copy:
```python
>>> xs[1][0] = 'X'
>>> xs
[[1, 2, 3], ['X', 5, 6], [7, 8, 9], ['new sublist']]
>>> ys
[[1, 2, 3], ['X', 5, 6], [7, 8, 9]]
```
In the above example we (seemingly) only made a change to xs. But
it turns out that both sublists at index 1 in xs and ys were modified.
Again, this happened because we had only created a shallow copy of
the original list.

Had we created a deep copy of xs in the first step, both objects
would’ve been fully independent. This is the practical difference
between shallow and deep copies of objects.

### Making Deep Copies
Let’s repeat the previous list-copying example, but with one impor-
tant difference. This time we’re going to create a deep copy using the
deepcopy() function defined in the copy module instead:
```python
>>> import copy
>>> xs = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
>>> zs = copy.deepcopy(xs)
```
When you inspect xs and its clone zs that we created with
copy.deepcopy(), you’ll see that they both look identical again—just
like in the previous example:
```python
>>> xs
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
>>> zs
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```
However, if you make a modification to one of the child objects in the
original object (xs), you’ll see that this modification won’t affect the
deep copy (zs).

Both objects, the original and the copy, are fully independent this time.
xs was cloned recursively, including all of its child objects:
```python
>>> xs[1][0] = 'X'
>>> xs
[[1, 2, 3], ['X', 5, 6], [7, 8, 9]]
>>> zs
[[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```
By the way, you can also create shallow copies using a function in the
copy module. The copy.copy() function creates shallow copies of
objects.

### Copying Arbitrary Objects
same shit...

### Key Takeaways
• Making a shallow copy of an object won’t clone child objects.
Therefore, the copy is not fully independent of the original.
• A deep copy of an object will recursively clone child objects. The
clone is fully independent of the original, but creating a deep
copy is slower.
• You can copy arbitrary objects (including custom classes) with
the copy module.