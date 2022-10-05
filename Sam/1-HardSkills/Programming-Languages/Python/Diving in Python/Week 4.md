# Week 4

Date: March 4, 2022 11:05 AM

### Key words | Questions

### Notes

# Магические методы

`__init__(), __new__(), __del__(), __str__(), __hash__(), __eq__(), __getattr__(), __setattr__(), __delatr__(), __call__(), __add__()`

# Итераторы

# Контекстные менеждеры

```python
class open_file:
    def __init__(self, filename, mode) -> None:
        self.f = open(filename, mode)

    def __enter__(self):
        return self.f

    def __exit__(self, *args):
        self.f.close()

with open_file('fuck', 'a') as f:  
    f.write('hi!\n')
```

- Есть возможность ловить исключения, которые выбрасываются внутри контекстных менеждеров:
    
    ```python
    with suppress_exaption(ZeroDivisionError): # Подавление исключения в блоке with, такой менеджер уже есть в либе
    																					 # **import contextlib**
    																					 #      **with contextlib.suppress(ValueError):**
    ```
    
- Измерить время выполнения блока контекстного менеджера:
    
    ```python
    import time
    
    class timer:
        def __init__(self):
            self.start = time.time()
    
        def __enter__(self):
            return
    
        def __exit__(self, *args):
            print(time.time() - self.start)
    
    with timer():
    	time.sleep(1)
    
    ### Улучшенная версия с current time
    
    import time
    
    class timer:
        def __init__(self):
            self.start = time.time()
    
        def current(self):
            print(time.time() - self.start)
    
        def __enter__(self):
            return self
    
        def __exit__(self, *args):
            print(time.time() - self.start)
    
    with timer() as t:
        time.sleep(1)
        t.current()
        time.sleep(1)
    ```
    

# Дескрипторы

---

```python
class Descriptor:
    def __get__(self, obj, obj_type):
        print('get')
    
    def __set__(self, obj, obj_type):
        print('set')
    
    def __delete__(self, obj, obj_type):
        print('delete')
    
    
class Class:
    attr = Descriptor()
    

instance = Class()

instance.attr
# get

instance.attr = 10
# set

del instance.attr
# delete
```

# Метаклассы

---

```python

```

# **Документация**

[Data model](https://docs.python.org/3/reference/datamodel.html)

[Дескрипторы](https://docs.python.org/3/howto/descriptor.html)

# Отладка и тестирование

---

- `import pdb` or `python -m pdb script.py`

# Тестирование

---

- `import unittest`

<aside>
📌 **SUMMARY:**

</aside>