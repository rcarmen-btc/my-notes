# Chapter 3. Effective Functions
## 3.1 Python’s Functions Are First-Class
Python’s functions are first-class objects. You can assign them to variables, store them in data structures, pass them as arguments to other functions, and even return them as values from other functions.

Функции - это объекты превого класса, умение из использовать поможет более эффективно программировать.  Например использование lambdas or decorators

```python
def yell(s):
	print(s)

bark = yell

del yell

yell('hello?')

# NameError: "name 'yell' is not defined"

bark('Hello!')
# Hello!

bark.__name__ 
# yell
```

### Functions Can Be Stored in Data Structures
```python
>>> funcs = [bark, str.lower, str.capitalize]
>>> funcs
[<function yell at 0x10ff96510>,
<method 'lower' of 'str' objects>,
<method 'capitalize' of 'str' objects>]
```

### Functions Can Be Passed to Other Functions
```python
def greet(func):
	greeting = func('Hi, I am a Python program')
	print(greeting)
```

### Functions Can Be Nested
```python
def speak(text):
	def whisper(t):
		return t.lower() + '...'
	return whisper(text)
	
>>> speak('Hello, World')
'hello, world...'
```

### Functions Can Capture Local State

Замыкание:
```python
def pre_greet(greet):
	def inner(name):
		print(greet, name)
	return inner

a = pre_greet('hello')
a('Fucker')

pre_greet('Fuck')('me')
```

### Objects Can Behave Like Functions
While all functions are objects in Python, the reverse isn’t true. Objects aren’t functions. But they can be made callable, which allows you to treat them like functions in many cases. If an object is callable it means you can use the round parentheses function call syntax on it and even pass in function call arguments. This is all powered by the __call__ dunder method. Here’s an example of class defining a callable object:
```python
class Adder:
	def __init__(self, n):
		self.n = n
	def __call__(self, x):
		return self.n + x

>>> plus_3 = Adder(3)
>>> plus_3(4)
7
```
Behind the scenes, “calling” an object instance as a function attempts
to execute the object’s __call__ method

### Key Takeaways
• Everything in Python is an object, including functions. You can
assign them to variables, store them in data structures, and pass
or return them to and from other functions (first-class func-
tions.)
• First-class functions allow you to abstract away and pass
around behavior in your programs.
• Functions can be nested and they can capture and carry some
of the parent function’s state with them. Functions that do this
are called closures.
• Objects can be made callable. In many cases this allows you to
treat them like functions.

## 3.2 Lambdas Are Single-Expression Functions
```python
>>> (lambda x, y: x + y)(5, 3)
8
```

### Lambdas You Can Use
My most frequent use case for lambdas is writing short and concise key funcs for sorting iterables by an alternate key:
```python
>>> tuples = [(1, 'd'), (2, 'b'), (4, 'a'), (3, 'c')]
>>> sorted(tuples, key=lambda x: x[1])
[(4, 'a'), (2, 'b'), (3, 'c'), (1, 'd')]
```
or
```python 
>>> sorted(range(-5, 6), key=lambda x: x * x)
[0, -1, 1, -2, 2, -3, 3, -4, 4, -5, 5]
```

Here’s another interesting thing about lambdas: Just like regular nested functions, lambdas also work as lexical closures.
```python
>>> def make_adder(n):
...     return lambda x: x + n
>>> plus_3 = make_adder(3)
>>> plus_5 = make_adder(5)
>>> plus_3(4)
7
>>> plus_5(4)
9
```

### But Maybe You Shouldn’t…
```python
# Harmful:
>>> class Car:
...rev = lambda self: print('Wroom!')
...crash = lambda self: print('Boom!')
>>> my_car = Car()
>>> my_car.crash()
'Boom!'
```

```python
# Harmful:
>>> list(filter(lambda x: x % 2 == 0, range(16)))
[0, 2, 4, 6, 8, 10, 12, 14]
# Better:
>>> [x for x in range(16) if x % 2 == 0]
[0, 2, 4, 6, 8, 10, 12, 14]
```

### Key Takeaways
• Lambda functions are single-expression functions that are not necessarily bound to a name (anonymous).
• Lambda functions can’t use regular Python statements and always include an implicit return statement.
• Always ask yourself: Would using a regular (named) function or a list comprehension offer more clarity?


## 3.3 The Power of Decorators
I believe that the payoff for understanding how decorators work in Python can be enormous.

### Python Decorator Basics
Now, what are decorators really? They “decorate” or “wrap” another function and let you execute code before and after the wrapped function runs. Decorators allow you to define reusable building blocks that can change or extend the behavior of other functions. And, they let you do that without permanently modifying the wrapped function itself. The function’s behavior changes only when it’s decorated.

```python
def wrapper(func):
	def inner(*args, **kwargs):
		print('Start func')
		func(*args, **kwargs)
		print('Start func')
	return inner


@wrapper
def hello(n):
	for i in range(n):
		print(f'#{i}')

# wrapped_hello = wrapper(hello)
# wrapped_hello()

hello(4)
```

### Applying Multiple Decorators to a Function
```python 
def strong(func):
	def wrapper():
		return '<strong>' + func() + '</strong>'
	return wrapper

def emphasis(func):
	def wrapper():
		return '<em>' + func() + '</em>'
	return wrapper

@strong
@emphasis
def greet():
	return 'Hello!'

>>> greet()
'<strong><em>Hello!</em></strong>'
```

### Decorating Functions That Accept Arguments
```python 
def proxy(func):
	def wrapper(*args, **kwargs):
		return func(*args, **kwargs)
	return wrapper
```

Let’s expand the technique laid out by the proxy decorator into a more
useful practical example. Here’s a trace decorator that logs function
arguments and results during execution time:
```python
def trace(func):
	def wrapper(*args, **kwargs):
		print(f'TRACE: calling {func.__name__}() with {args}, {kwargs}')
		original_result = func(*args, **kwargs)
		print(f'TRACE: {func.__name__}() 'f'returned {original_result!r}')
		return original_result
	return wrapper

@trace
def say(name, line):
	return f'{name}: {line}'

>>> say('Jane', 'Hello, World')
'TRACE: calling say() with ("Jane", "Hello, World"), {}'
'TRACE: say() returned "Jane: Hello, World"'
'Jane: Hello, World'
```

### How to Write “Debuggable” Decorators
When you use a decorator, really what you’re doing is replacing one
function with another. One downside of this process is that it “hides”
some of the metadata attached to the original (undecorated) function.
For example, the original function name, its docstring, and parameter
list are hidden by the wrapper closure:
```python
def greet():
	"""Return a friendly greeting."""
	return 'Hello!'
decorated_greet = uppercase(greet)
```
If you try to access any of that function metadata, you’ll see the wrap-
per closure’s metadata instead:
```python
>>> greet.__name__
'greet'
>>> greet.__doc__
'Return a friendly greeting.'
>>> decorated_greet.__name__
'wrapper'
>>> decorated_greet.__doc__
None
```
This makes debugging and working with the Python interpreter
awkward and challenging. Thankfully there’s a quick fix for this: the
functools.wraps decorator included in Python’s standard library.
```python
import functools
def uppercase(func):
	@functools.wraps(func)
	def wrapper():
		return func().upper()
	return wrapper
```
Applying functools.wraps to the wrapper closure returned by the
decorator carries over the docstring and other metadata of the input  function:
```python
@uppercase
def greet():
	"""Return a friendly greeting."""
	return 'Hello!'

>>> greet.__name__
'greet'
>>> greet.__doc__
'Return a friendly greeting.'
```
As a best practice, I’d recommend that you use functools.wraps in
all of the decorators you write yourself. It doesn’t take much time and
it will save you (and others) debugging headaches down the road.
Oh, and congratulations—you’ve made it all the way to the end of
this complicated chapter and learned a whole lot about decorators in
Python. Great job!

## 3.4 Fun With *args and **kwargs
```python
def foo(required, *args, **kwargs):
	print(required)
	if args:
		print(args)
	if kwargs:
		print(kwargs)

>>> foo()
TypeError:
"foo() missing 1 required positional arg: 'required'"
>>> foo('hello')
hello
>>> foo('hello', 1, 2, 3)
hello
(1, 2, 3)
>>> foo('hello', 1, 2, 3, key1='value', key2=999)
hello
(1, 2, 3)
{'key1': 'value', 'key2': 999}
```
### Forwarding Optional or Keyword Arguments
It’s possible to pass optional or keyword parameters from one func-
tion to another. You can do so by using the argument-unpacking op-
erators * and ** when calling the function you want to forward argu-
ments to.

This also gives you an opportunity to modify the arguments before you
pass them along. Here’s an example:
```python
def foo(x, *args, **kwargs):
	kwargs['name'] = 'Alice'
	new_args = args + ('extra', )
	bar(x, *new_args, **kwargs)
```

This technique can be useful for subclassing and writing wrapper func-
tions. For example, you can use it to extend the behavior of a parent
class without having to replicate the full signature of its constructor
in the child class. This can be quite convenient if you’re working with
an API that might change outside of your control:
```python
class Car:
	def __init__(self, color, mileage):
		self.color = color
		self.mileage = mileage

class AlwaysBlueCar(Car):
	def __init__(self, *args, **kwargs):
		super().__init__(*args, **kwargs)
		self.color = 'blue'

>>> AlwaysBlueCar('green', 48392).color
'blue'
```

One more scenario where this technique is potentially helpful is
writing wrapper functions such as decorators. There you typically
also want to accept arbitrary arguments to be passed through to the
wrapped function.
And, if we can do it without having to copy and paste the original func-
tion’s signature, that might be more maintainable:
```python
def trace(f):
	@functools.wraps(f)
	def decorated_function(*args, **kwargs):
		print(f, args, kwargs)
		result = f(*args, **kwargs)
		print(result)
	return decorated_function

@trace
def greet(greeting, name):
	return '{}, {}!'.format(greeting, name)

>>> greet('Hello', 'Bob')
<function greet at 0x1031c9158> ('Hello', 'Bob') {}
'Hello, Bob!'
```
With techniques like this one, it’s sometimes difficult to balance the
idea of making your code explicit enough and yet adhere to the Don’t
Repeat Yourself (DRY) principle. This will always be a tough choice to
make. If you can get a second opinion from a colleague, I’d encourage
you to ask for one.

### Key Takeaways
• \*args and \*\*kwargs let you write functions with a variable
number of arguments in Python.
• \*args collects extra positional arguments as a tuple. \*\*kwargs
collects the extra keyword arguments as a dictionary.
• The actual syntax is \* and \*\*. Calling them args and kwargs is
just a convention (and one you should stick to).


## 3.5 Function Argument Unpacking
## Key Takeaways
• The * and ** operators can be used to “unpack” function argu-
ments from sequences and dictionaries.
• Using argument unpacking effectively can help you write more
flexible interfaces for your modules and functions.

## 3.6 Nothing to Return Here
### Key Takeaways
• If a function doesn’t specify a return value, it returns None.
Whether to explicitly return None is a stylistic decision.
• This is a core Python feature but your code might communicate
its intent more clearly with an explicit return None statement.