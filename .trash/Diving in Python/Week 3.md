# Week 3

Date: March 4, 2022 6:40 AM
Tags: OOP

### Key words | Questions

`**isinstance()**`

`@classmenthond. @staticmethod, property()`

`issubclass()`

### Notes

# ООП

---

```python
class Planet:

	conut = 0
	
	def __new__(cls, *args, **kwargs):
		obj = super().__new__(cls)
		reutrn obj

	def __init__(self, name):
		self.name = name

	def __str__(self):
		return self.name
	
	def __repr__(self):
		return self.name

	def __del__(self): # no
		print('del')

planet = Planet('Earth')

print(planet.name)
```

`@classmenthond. @staticmethod, property()`

# Наследование

---

```python

```

# **Документация**

[Наследование классов в python](https://docs.python.org/3/tutorial/classes.html#inheritance)

<aside>
📌 **SUMMARY:**

</aside>