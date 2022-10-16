# Week 1

Date: February 24, 2022 7:52 PM
Tags: base, contitions, cycles, useful links

### Key words | Questions

1. **Именование, форматирование, документирование.** 
2. **Базовые типы.**
    1. **Численные типы**
    2. **Логический тип**
    3. **Строки (str)**
        1. **Форматирование строк**
    4. **Байтовые строки (bytes)**
    5. **None**
3. **Конструкции управления потоком**
4. **Модули и пакеты**
5. **Виртуальное окружение (venv)**
6. **Сайты**
7. **Библиеотеки**
8. **Книги**

`**help()` - man по языку**

`**type()` - тип объекта**

**snake_case**

**doc strings**

`**int**`

`**float**`

`**complex**`

`**decimal**`

`**fractions**`

**OR, XOR, AND**

`id()` **- адрес в памяти**

`**.format()` - форматирование**

`**input()` - чтение с 0 фд**

`**str.encode()` - кодирование**

`**str.decode()` - дикодирование**

`**import this**`

`**dir()` - методы объекта**

### Notes

# Именование, форматирование, документирование.

---

- Переменные. Имена могут содержать **буквы, цифры и символ подчёркивания.** Но они могут начинаться либо с **буквы**, либо с **символа подчёркивания.**
    
    ```python
    a = 1; # ok
    _a = 2; # ok
    49a = 3; # error
    ```
    
- Используется **snake_case.**
- Строка документирования.
    
    ```python
    def a():
    	"""
    	This is a doc string
    	"""
    
    print(a.__doc__)
    ```
    
     Терминал:
    
    ```bash
    $ This is a doc string
    ```
    

# **Базовые типы.**

---

## **Численные типы**

- Python поддерживает длинную арифметику
- `int` - целые
    
    ```python
    a = 1
    b = -3
    c = 100_000_000
    ```
    
- `float` - с плавающей точкой
    
    ```python
    a = 1.1
    b = -3.3
    c = 100_000.000_001
    d = 1.5e2 # 1.5 * 10 ^ 2
    ```
    
- `complex` - комплесные числа
    
    ```python
    a = 14 + 1j
    
    print(num.real)
    print(num.imag)
    ```
    
- `decimal` - для работы с вещественными числами с фиксированной точностью.
- `fractions`- для работы с рациональными числами.
- **Операции** с числами
    - `+, -, *, /`
    - `x // y` - Целочисленное деление. **Div**
    - `x % y` - Деление по модулю. **Mod**
    - `x ** y` - В степень
- **Побитовые** операции
    - `x | y` - Побитовое **ИЛИ (OR)**
    - `x ^ y` - Исключающее **ИЛИ (XOR)**
    - `x & y` -  Побитовое **И (AND)**
    - `x << y` -  Битовый сдвиг влево
    - `x >> y` -  Битовый сдвиг вправо
    - `~x` -  Инверсия битов
- Меняем значения двух переменных
    
    ```python
    a = 10
    b = 20
    
    a, b = b, a
    ```
    
- Вместо `x, y = 0, 0`
    
    ```python
    x = y = 0
    ```
    

## **Логический тип**

---

- `bool` - это подтип целого числа
    - `True - 1`
    - `False - 0`
- **Операторы** сравнения
    - `>, <, >=, <=, ==, !=`
    - Множественное сравнение
        
        ```python
        1 < x <= 10
        ```
        
- **Логические** выражения
    - `and`
    - or
    - `not`
    - Ленивые
        
        ```python
        x = 12
        y = False
        print(x or y)
        ***# 2***
        
        z = 12
        k = 'Boom'
        print(z and k)
        ***# Boom***
        ```
        

## С**троки (str)**

---

```python
a = 'Hello, World'
b = "It's Sam"

c = "I love my book \"Anya\"" # Экранирование
d = r"I hate the war!" # Сырые строки

e = "Hi" \
		"Hello" 
***# HiHello***

f = """
He is
not my friend
"""
***#
# He is
# not my firend
#***

g = "Hello, " + "World" # Конкатинация
h = "Hello, " * 3 # Умножение
```

- `id()` - узнать адрес в памяти
- Срезы строк
    
    ```python
    a = "Course about python"
    
    ***# a[start:stop:step]***
    
    a[4:]
    ***# se about python***
    
    a[4:7]
    ***# se ab***
    
    a[-4:]
    ***# thon***
    
    a[::4]
    ***# Csb h***
    
    a[::-1]
    ***# nohtyp tuoba esruoC*** 
    ```
    
- `in`
    
    ```python
    "ANYA" in "UltrANYAsha"
    ***# True***
    ```
    
- Оператор `for ... in` позволяет итерироваться по строке
- Конвертация типов
    
    ```python
    str(999.32)
    ***# '999.32'***
    ```
    
- Больше информации про методы строк в документации.
    
    ## Форматирование строк
    
    - `%s`
        
        ```python
        a = '%s is my gf, she likes %s'
        a % ("Madina", "cats")
        ```
        
    - `.formam(), .format(var=30, var1=20)`
        
        ```python
        # Без имен
        '{} is my gf, she likes {}'.format("Madina", "cats")
        
        # С именами
        '{name} is my gf, she likes {animal}'.format(name="Madina", animal="cats")
        ```
        
    - `f`-строки
        
        ```python
        name = "Madina"
        animal = "cats"
        
        f'{name} is my gf, she likes {animal}'
        ```
        
        - Модификаторы форматирования (см. документацию строк)

## Б**айтовые строки (bytes)**

---

```python
name_in_b = b"Madina"

for byte in name_in_b:
  print(byte)

print(name_in_b)
# 77
# 97
# 100
# 105
# 110
# 97
# b'Madina'
```

- `str.encode()` - кодирование
- `str.decode()` - дикодирование

## None

---

```python
answer = None

bool(None)
***# False***
```

# **Конструкции управления потоком**

---

- `if`
    
    ```python
    if [condition]:
    	# code
    [elif]:
    	# code
    [else]:
    	# code
    ```
    
- `if...else...`
    
    ```python
    a = 'Madina' if [cond] else 'Beka'
    ```
    
- `while`
    
    ```python
    while [cond]:
    	# code
    ```
    
- `for ... in ...` и `range()`
    
    ```python
    for [elem] in [seq]:
    	# code
    
    for num in range(start, [stop], [step]): # Итерация по целым числам
    	# code 
    ```
    
- `pass`
    
    ```python
    while [cond]:
    	pass
    ```
    
- `break, continue`
    
    ```python
    while [cond]:
    	# code
    	if [condition]:
    		pass
    	else:
    		continue
    ```
    

# Модули и пакеты

---

- `import`
    
    ```python
    # mypackage/utils.py
    
    def multiply(a, b):
    	return a * b;
    ```
    
    ```python
    import mypackage.utils 
    # or 
    from mypackage.utils import multiply
    # or 
    from mypackage.utils import * 
    ```
    
- **Модуль** - файл
- **Пакет** - каталог с файлами, но они тоже модули.
- ***__init__.py*** - обязательный файл в каталоге пакета. Выполняется, когда мы импортируем пакет.
- ***__name_*_** - переменная окружения содержит название модуля, а запускаемы модуль это ***__main__.***
    
    ```python
    if __name__ == "__main__": # Если этот модуль главный, то ...
    	pass
    ```
    

- Где находится модуль?
    
    ```python
    import inspect
    
    inspect.getfile(this)
    ```
    
- `import this` - дзен

# Виртуальное окружение (venv)

---

- Создание
    
    ```bash
    python3 -m venv env
    ```
    
- Активация
    
    ```bash
    source env/bin/activate
    ```
    
- Диактивация
    
    ```bash
    deactivate
    ```
    

- Считать переданный параметр можно с помощью модуля стандартной библиотеки **sys**
    
    ```python
    import sys
    
    digit_string = sys.argv[1]
    ```
    

---

# **Сайты**

- [Документация Python](https://docs.python.org/3/).
- Исходный код [CPython](https://github.com/python/cpython).
- [PEP 8](https://www.python.org/dev/peps/pep-0008/). Есть [утилита autopep8](https://pypi.python.org/pypi/autopep8), которая позволяет автоматически приводить код к виду, соответствующему PEP 8.

# **Библиеотеки**

- [PyPI (Python Package Index)](https://pypi.python.org/pypi).
- На GitHub есть [коллекция хороших библиотек](https://github.com/vinta/awesome-python) для решения всевозможных задач.

# **Книги**

- Марк Саммерфилд - Программирование на Python 3. Подробное руководство
- Марк Лутц - Изучаем Python, 4-е издание

<aside>
📌 **SUMMARY:** В питоне всё объект!

</aside>