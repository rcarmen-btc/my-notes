# Templates
## Syntax
### Variables[¶](https://docs.djangoproject.com/en/4.0/topics/templates/#variables "Permalink to this headline")
A variable outputs a value from the context, which is a dict-like object mapping keys to values.

Variables are surrounded by `{{` and `}}` like this:

```html
My first name is {{ first_name }}. My last name is {{ last_name }}.
```

With a context of `{'first_name': 'John', 'last_name': 'Doe'}`, this template renders to:

``` HTML
My first name is John. My last name is Doe.
```
Dictionary lookup, attribute lookup and list-index lookups are implemented with a dot notation:

```HTML
{{ my_dict.key }}
{{ my_object.attribute }}
{{ my_list.0 }}
```

If a variable resolves to a callable, the template system will call it with no arguments and use its result instead of the callable.

---
### Tags[¶](https://docs.djangoproject.com/en/4.0/topics/templates/#tags "Permalink to this headline")
Tags provide arbitrary logic in the rendering process.

This definition is deliberately vague. For example, a tag can output content, serve as a control structure e.g. an “if” statement or a “for” loop, grab content from a database, or even enable access to other template tags.

Tags are surrounded by `{%` and `%}` like this:
``` python
{% csrf_token %}
```

Most tags accept arguments:
```python
{% cycle 'odd' 'even' %}
```

Some tags require beginning and ending tags:
```python
{% if user.is_authenticated %}Hello, {{ user.username }}.{% endif %}
```

A [reference of built-in tags](https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#ref-templates-builtins-tags) is available as well as [instructions for writing custom tags](https://docs.djangoproject.com/en/4.0/howto/custom-template-tags/#howto-writing-custom-template-tags).

---
### Filters[¶](https://docs.djangoproject.com/en/4.0/topics/templates/#filters "Permalink to this headline")
Filters transform the values of variables and tag arguments.

They look like this:
```python
{{ django|title }}
```

With a context of `{'django': 'the web framework for perfectionists with deadlines'}`, this template renders to:
```python
The Web Framework For Perfectionists With Deadlines
```

Some filters take an argument:
```python
{{ my_date|date:"Y-m-d" }}
```

A [reference of built-in filters](https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#ref-templates-builtins-filters) is available as well as [instructions for writing custom filters](https://docs.djangoproject.com/en/4.0/howto/custom-template-tags/#howto-writing-custom-template-filters).

---
### Comments[¶](https://docs.djangoproject.com/en/4.0/topics/templates/#comments "Permalink to this headline")

Comments look like this:

```python
{# this won't be rendered #}
```

A [`{% comment %}`](https://docs.djangoproject.com/en/4.0/ref/templates/builtins/#std:templatetag-comment) tag provides multi-line comments.