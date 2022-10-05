# Django main

#CSS #Django #HTML #Python #SQL

1. [models.md](models.md)
2. [views.md](views.md)
3. [urls.md](urls.md)
4. [admin.md](admin.md)

## migratinos
```sql
$ ./manage.py makemigrations # read APPS moderls.py and create/evolve migratinons 

$ ./manage.py migrate # create or alter database 

$ ./manage.py check
```

## Dig a little bit into object relational mapping
```python
from django.db import connection
print(connection.queries)
```

