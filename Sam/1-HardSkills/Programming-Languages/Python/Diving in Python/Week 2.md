# Week 2

Date: February 26, 2022 8:20 AM
Tags: base, decorators, func, generators

### Key words | Questions

`**enumerate(collections)**`

`**random**`

`**.append()**`

**`.extend()`**

**`max()`**

`**min()**`

**`sum()`**

**`.join()`**

**`.sort()`**

**`sorted()`**

**`reversed()`**

### Notes

# Коллекции

---

## Списки (list)

---

- **Очень похожи на строки**

```python
empty_list = []
empty_list = list()

none_list = [None] * 10

not_empty_list = ['Anya', 'Madina', 'Masha']

matrix = [
	['Anya', 'Madina', 'Masha'],
	['20', '18', '21'],
]

nums = [1, 2, 3]

# Добавление одного элемента
empty_list.append('Bekao')

# Добавление списка в список
empty_list.extend(not_empty_list)
empty_list += not_empty_list

max(empty_list)
min(empty_list)
sum(nums)

print(empty_list)
print(', '.join(empty_list))

print(sorted(empty_list))
empty_list.sort(reverse=True)
print(empty_list)

print(list(reversed(empty_list)))
```

## Кортежи (tuple)

---

- **Очень похожи на списки**

```python
one_tuple = (1,)
empty_tuple = tuple()
```

## Словари (dict)

```python
empty_dict = {}
empty_dict = dict()

not_empty_dict = {
	'one': 1,
	'two': 2,
}

not_empty_dict['one']
not_empty_dict.get('three')

not_empty_dict.update({'three': 3})
not_empty_dict.pop('three')
```

## Множества (set)

- Есть много операций и методов

```python
empty_dict = {}
empty_dict = set()

not_empty_dict = {
	'one',
	'two',
}

not_empty_dict['one']
```

# Файлы

```python
with open('filename') as f:
	print(f.read())
```

# Функции

---

- Есть аннотация типов

```python
def get_lvl():
	""" Return level """
	return 21

get_lvl.__doc__ # 'Return level'
get_lvl.__name__# 'get_lvl'
```

- Списки передаются по ссылке
- Строки передаются по значению
- Использовать **`return`**
- Именнованные аргументы
- Из фунции не видими ничего вне функции
- Аргументы по умолчанию. Использовать `**None**` вместо изменяемых структуры данных для первой итерации
- **`*args **kwargs`**

# Замыкания

---

```python
def wrapper(cof):
    def inner(n):
        return (n * cof)
    return inner

inn = wrapper(2) 

print(inn(3))
# 6
```

- `map(), filter(), lambda(), reduce(), partial(), zip()`

# Декораторы

---

Без параметров

```python
import functools

### Логирование возвращаемых значений в файл ###
def logger(func):
    @functools.wraps(func) # for __ methods
    def wrapped(*args, **kwargs):
        result = func(*args, **kwargs)
        with open('log.txt', 'w') as f:
            f.write(str(result) + '\n')
        return result
    return wrapped

@logger
def summator(num_list):
    return sum(num_list)

s = summator(list(range(1, 6)))

print(s) 
```

С параметрами

```python
import functools

def logger(filename):
    def decorator(func):
        @functools.wraps(func) # for __ methods
        def wrapped(*args, **kwargs):
            result = func(*args, **kwargs)
            with open(filename, 'w') as f:
                f.write(str(result) + '\n')
            return result
        return wrapped
    return decorator

@logger('new_log.txt')
def summator(num_list):
    return sum(num_list)

s = summator(list(range(1, 6)))

print(s)
```

# Генараторы

---

```python
def even_range(start, end):
    current = start
    while current < end:
        yield current
        current += 2

for n in even_range(0, 10):
    print(n)

# 0
# 2
# 4
# 6
# 8

ranger = even_range(0, 4)
next(ranger)
# 0
next(ranger)
# 2
```

# **Документация**

---

[Туториал по функциям из документации](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)

[Работа с файлами](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files)

[Встроенные функции](https://docs.python.org/3/library/functions.html)

[Сортировка](https://docs.python.org/3/howto/sorting.html)

[Функциональное программирование](https://docs.python.org/3/howto/functional.html)

[Модуль functools](https://docs.python.org/3/library/functools.html)

[Декораторы](http://python-3-patterns-idioms-test.readthedocs.io/en/latest/PythonDecorators.html)

<aside>
📌 **SUMMARY:**

</aside>