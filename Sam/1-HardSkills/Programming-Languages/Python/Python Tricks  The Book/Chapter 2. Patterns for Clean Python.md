# Chapter 2
# Patterns for Clean Python
---
## 2.1 Covering Your A** With Assertions
You’ll learn how to use them to help automatically detect errors 
in your Python programs. This will make your programs more 
reliable and easier to debug.

At its core, Python’s assert statement is a debugging aid that 
tests a condition. 

### Assert in Python — An Example
```python
def apply_discount(product, discont):
	price = int(product['price'] * (1.0 - discont))
	assert 0 < price < product['price']
	return price

  
price = apply_discount({'name': 'Pretty Pantsu', 'price': 14900}, 0.1)
print(price)

#> 13410
```
This speeds up debugging efforts considerably, and it will make 
your programs more maintainable in the long-run. And that, my 
friend, is the power of assertions.

> **Гейзенбаг** ([англ.](https://ru.wikipedia.org/wiki/%D0%90%D0%BD%D0%B3%D0%BB%D0%B8%D0%B9%D1%81%D0%BA%D0%B8%D0%B9_%D1%8F%D0%B7%D1%8B%D0%BA "Английский язык") heisenbug) — [жаргонный термин](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%BE%D1%84%D0%B5%D1%81%D1%81%D0%B8%D0%BE%D0%BD%D0%B0%D0%BB%D0%B8%D0%B7%D0%BC%D1%8B "Профессионализмы"), используемый в [программировании](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D0%B5 "Программирование") для описания программной ошибки, которая исчезает или меняет свои свойства при попытке её обнаружения. Это слово, в отличие от слова «[баг](https://ru.wikipedia.org/wiki/%D0%9F%D1%80%D0%BE%D0%B3%D1%80%D0%B0%D0%BC%D0%BC%D0%BD%D0%B0%D1%8F_%D0%BE%D1%88%D0%B8%D0%B1%D0%BA%D0%B0 "Программная ошибка")» ([англ.](https://ru.wikipedia.org/wiki/%D0%90%D0%BD%D0%B3%D0%BB%D0%B8%D0%B9%D1%81%D0%BA%D0%B8%D0%B9_%D1%8F%D0%B7%D1%8B%D0%BA "Английский язык") bug), в русском языке используется редко. Не полностью идентичный, но достаточно близкий по значению русскоязычный термин — «плавающая ошибка».
> 

Before you move on, there are two important caveats regarding 
the use of assertions in Python that I’d like to call out.

Asserts мы используем когда мы хотим отловить баги в программе, когда багов нет, то и asserts не должны срабатывать. Asserts проверяют невозможные условия в вашей программе, если эти условия нарушены - значит есть баг. Например, процент скидки не может быть отрицательным или больше 100%. Но если это уже валидация данных, то нельзя пользоваться asserts. 

##### Caveat #1 - Don't Use Asserts for Data Validation
The biggest caveat with using asserts in Python is that assertions 
can be globally disabled 3 with the -O and -OO command line 
switches, as well as the PYTHONOPTIMIZE environment variable 
in CPython.

Главная причина описана выше, при отключении asserts вся наша валидация отключится тоже, так что лучше в для валидации использовать if-statements

##### Caveat #1 - Asserts Thats Never Fail
Запомни, что assert - это ключевое слово, а не функция. Поэтому если написать:
```python 
assert(1 == 2, 'Never run')
```
то мы передаём в assert кортеж из двух элементов и без второго аргумента, а непустой кортеж всегда true, так что это пример не вызовет assert.

Если написать такой же assert у себя в программе в тестах, то он никогда не поймает ошибку:
```python
assert (
	count > 0,
	'Wow, count under zero'
)
```

By the way, that’s also why you should always do a quick smoke test
with your unit test cases. Make sure they can actually fail before you
move on to writing the next one.

### Python Assertions — Summary
Despite these caveats I believe that Python’s assertions are a powerful
debugging tool that’s frequently underused by Python developers.
Understanding how assertions work and when to apply them can help
you write Python programs that are more maintainable and easier to
debug.

It’s a great skill to learn that will help bring your Python knowledge to
the next level and make you a more well-rounded Pythonista. I know
it has saved me hours upon hours of debugging.

### Key Takeaways
• Python’s assert statement is a debugging aid that tests a condi-
tion as an internal self-check in your program.
• Asserts should only be used to help developers identify bugs.
They’re not a mechanism for handling run-time errors.
• Asserts can be globally disabled with an interpreter setting.


## 2.2 Complacent Comma Placement Here’s 
Here’s a handy tip for when you’re adding and removing items from
a list, dict, or set constant in Python: Just end all of your lines with a
comma.

Старайся не писать так:
```python
>>> names = ['Alice', 'Bob', 'Dilbert']
```
Пиши так:
```python
>>> names = [
...     'Alice',
...     'Bob',
...     'Dilbert'
... ]
```
Так легче увидеть в git diff изменения. Ещё легко добавлять и удалять элементы. 

Удобно, чтобы не ставить косые чёрочки:
```python
my_str = ('This is a super long string constant '
		  'spread out across multiple lines. '
		  'And look, no backslash characters needed!')
```

Чтобы было ещё легче ставь запятую в конце каждого элемента:
```python
>>> names = [
...     'Alice',
...     'Bob',
...     'Dilbert', # Here comma
... ]
```

### Key Takeaways
• Smart formatting and comma placement can make your list,
dict, or set constants easier to maintain.
• Python’s string literal concatenation feature can work to your
benefit, or introduce hard-to-catch bugs.


## 2.3 Context Managers and the *with* Statement
A good way to see this feature used effectively is by looking at examples in the Python standard library. The built-in open() function provides us with an excellent use case:
```python
with open('hello.txt', 'w') as f:
	f.write('hello, world!')
```
Нужно так работать с файлами, так как это автоматичеки закроет fd'шку всегда, даже при ошибке.

Можно работать с тредами, для блокировки:
```python
some_lock = threading.Lock()
# Harmful:
some_lock.acquire()
try:
# Do something...
finally:
some_lock.release()
# Better:
with some_lock:
# Do something...
```

Контекстный менеджер помогает освобождать выделенные ресурсы, когда уже больше не нужен.

### Supporting with in Your Own Objects
Можно создавать самому эти менеджеры, для удобства:
```python
class ManagedFile:

	def __init__(self, name):
		self.name = name

	def __enter__(self):
		self.file = open(self.name, 'w')
		return self.file

	def __exit__(self, exc_type, exc_val, exc_tb):


if self.file:
	self.file.close()
```
так:
```python 
>>> with ManagedFile('hello.txt') as f:
...    f.write('hello, world!')
...    f.write('bye now')
```

### Contextlib 
The contextlib utility module in the standard library provides a few more abstractions built on top of the basic context manager protocol. This can make your life a little easier if your use cases match what’s offered by contextlib.

```python
from contextlib import contextmanager


@contextmanager
def managed_file(name, mode):

	try:
		f = open(name, mode)
		yield f
	finally:
		f.close()
  

with managed_file('text.txt', 'w+') as f:
	f.write('Hell')
```

### Writing Pretty APIs With Context Managers
Context managers are quite flexible, and if you use the with state-
ment creatively, you can define convenient APIs for your modules and
classes.

```python
class Indent:

	def __init__(self):
		self.ind_lvl = -1
	
	def __enter__(self):
		self.ind_lvl += 1
		return self
	
	def __exit__(self, exc_type, exc_val, exc_tb):
		self.ind_lvl -= 1
	
	def print(self, txt):
		print('\t' * self.ind_lvl, txt, sep='')
	
	  
  

with Indent() as ind:
	ind.print('Hello')
	with ind:
		ind.print('Bye')
		with ind:
			ind.print('!!!')
		ind.print('Bye')
	ind.print('Hello')
```
That wasn’t so bad, was it? I hope that by now you’re already feeling
more comfortable using context managers and the with statement in
your own Python programs. They’re an excellent feature that will al-
low you to deal with resource management in a much more Pythonic
and maintainable way.

### with-class based timer
```python
import time


class Timer:

	def __init__(self):
		self.start = None		

	def __enter__(self):
		self.start = time.time()
		return self

	def __exit__(self, exc_type, exc_val, exc_tb):
		print(time.time() - self.start)
```

### Key Takeaways
• The `with` statement simplifies exception handling by encapsulating standard uses of `try/finally` statements in so-called context managers.
• Most commonly it is used to manage the safe acquisition and release of system resources. Resources are acquired by the with statement and released automatically when execution leaves the with context.
• Using with effectively can help you avoid resource leaks and make your code easier to read.

## 2.4 Underscores, Dunders, and More
If you’re wondering, “What’s the meaning of single and double under-
scores in Python variable and method names?” I’ll do my best to get
you the answer here.

In this chapter we’ll discuss the following five underscore patterns and naming conventions, and how they affect the behavior of your Python programs:
- Single Leading Underscore: \_var
- Single Trailing Underscore: var_
- Double Leading Underscore: \_\_var
- Double Leading and Trailing Underscore: \_\_var\_\_
- Single Underscore: _

### 2.4.1 Single Leading Underscore: "\_var"
Этот вид нейминга просто общепринятое соглашение, означает что эта переменная приватна, например в классе.
```python
class Task:
	def __init__(self):
		self.foo = 11
		slef._bar = 23
```
Single underscores are a Python naming convention that indicates a
name is meant for internal use. It is generally not enforced by the
Python interpreter and is only meant as a hint to the programmer.

### 2.4.2 Single Trailing Underscore: “var_”
Sometimes the most fitting name for a variable is already taken by a
keyword in the Python language. Therefore, names like class or def
cannot be used as variable names in Python. In this case, you can
append a single underscore to break the naming conflict:
```python
>>> def make_object(name, class):
SyntaxError: "invalid syntax"

>>> def make_object(name, class_):
...     pass
```
In summary, a single trailing underscore (postfix) is used by conven-
tion to avoid naming conflicts with Python keywords. This convention
is defined and explained in PEP 8.

### 2.4.3 Double Leading Underscore: “\_\_var”
Если это в классе, это добавит в начало переменной, атрибута подчёркивание и имя класса. Например: "\_ClassName__var"

 И всё, чтобы не было перезаписи в дочерних классах. Если ещё что-то нужно, см. книгу.
```python
class Test:
	def __init__(self):
		self.foo = 11
		self._bar = 23
		self.__baz = 23
```

```python
>>> t = Test()
>>> dir(t)
['_Test__baz', '__class__', '__delattr__', '__dict__',
'__dir__', '__doc__', '__eq__', '__format__', '__ge__',
'__getattribute__', '__gt__', '__hash__', '__init__',
'__le__', '__lt__', '__module__', '__ne__', '__new__',
'__reduce__', '__reduce_ex__', '__repr__',
'__setattr__', '__sizeof__', '__str__',
'__subclasshook__', '__weakref__', '_bar', 'foo']
```

### Sidebar: What are dunders?
If you’ve heard some experienced Pythonistas talk about Python or
watched a few conference talks you may have heard the term dunder.
If you’re wondering what that is, well, here’s your answer:

Double underscores are often referred to as “dunders” in the Python
community. The reason is that double underscores appear quite often
in Python code, and to avoid fatiguing their jaw muscles, Pythonistas
often shorten “double underscore” to “dunder.”

For example, you’d pronounce \_\_baz as “dunder baz.” Likewise,
\_\_init\_\_ would be pronounced as “dunder init,” even though one
might think it should be “dunder init dunder.”
But that’s just yet another quirk in the naming convention. It’s like a
secret handshake for Python developers.

### 2.4.4 Double Leading and Trailing Underscore:“\_\_var\_\_”
However, names that have both leading and trailing double under-
scores are reserved for special use in the language. This rule covers
things like \_\_init\_\_ for object constructors, or \_\_call\_\_ to make ob-
jects callable.

However, as far as naming conventions go, it’s best to stay away from
using names that start and end with double underscores in your own
programs to avoid collisions with future changes to the Python lan-
guage.

### 2.4.5. Single Underscore: “\_”
Per convention, a single stand-alone underscore is sometimes used as
a name to indicate that a variable is temporary or insignificant.

For example, in the following loop we don’t need access to the running
index and we can use “_” to indicate that it is just a temporary value:
```python
>>> for _ in range(32):
...     print('Hello, World.')
```
In the following code example, I’m unpacking a tuple into separate
variables but I’m only interested in the values for the color and
mileage fields. However, in order for the unpacking expression to
succeed, I need to assign all values contained in the tuple to variables.
That’s where “\_” is useful as a placeholder variable:
```python
>>> car = ('red', 'auto', 12, 3812.4)
>>> color, _, _, mileage = car
>>> color
'red'
>>> mileage
3812.4
>>> _
12
```

Besides its use as a temporary variable, “_ ” is a special variable in most
Python REPLs that represents the result of the last expression evalu-
ated by the interpreter.

This is handy if you’re working in an interpreter session and you’d like
to access the result of a previous calculation:
```python
>>> 20 + 3
23
>>> _
23
>>> print(_)
23
```

It’s also handy if you’re constructing objects on the fly and want to
interact with them without assigning them a name first:
```python
>>> list()
[]
>>> _.append(1)
>>> _.append(2)
>>> _.append(3)
>>> _
[1, 2, 3]
```

### Key Takeaways
• **Single Leading Underscore** “\_var”: Naming convention in-
dicating a name is meant for internal use. Generally not en-
forced by the Python interpreter (except in wildcard imports)
and meant as a hint to the programmer only.
•**Single Trailing Underscore** “var_”: Used by convention to
avoid naming conflicts with Python keywords.
•Double Leading Underscore “\_\_var”: Triggers name man-
gling when used in a class context. Enforced by the Python in-
terpreter.
•**Double Leading and Trailing Underscore** “\_\_var\_\_”: In-
dicates special methods defined by the Python language. Avoid
this naming scheme for your own attributes.
•**Single Underscore** “\_”: Sometimes used as a name for tem-
porary or insignificant variables (“don’t care”). Also, it repre-
sents the result of the last expression in a Python REPL session

## 2.5 A Shocking Truth About String Formatting
```python
name = 'Mark'
myName = 'Dan'
myAge = 21

#1 Old
print('Hello, %s. My name is %s. I\'m %d ' % (name, myName, myAge))

#2 New
print('Hello, {}. My name is {}. I\'m {} '.format(name, myName, myAge))
# or
print('Hello, {name}. My name is {myName}. I\'m {myAge} '.format(name=name, myName=myName, myAge=myAge))

#3 Literal String Interpolation
print(f'Hello, {name}. My name is {myName}. I\'m {myAge} ')

#4 Template lib
from string import Template
t = Template('Hey, $name!')
t.substitute(name=name)
	```

## 2.6 “The Zen of Python” Easter Egg