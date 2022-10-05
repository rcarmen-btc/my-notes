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