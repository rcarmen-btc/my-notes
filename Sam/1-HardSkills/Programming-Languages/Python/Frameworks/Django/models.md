# Models (Object Relational Mapping)
*See SQL query:*
```bash
 ./manage.py sqlmigrate study 0001
```

---
## Field options
### Null [Ture | False]
Is used for no data cases. Avoid using [`null`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.null "django.db.models.Field.null") on string-based fields such as [`CharField`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.CharField "django.db.models.CharField") and [`TextField`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.TextField "django.db.models.TextField").  One exception is when a [`CharField`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.CharField "django.db.models.CharField") has both `unique=True` and `blank=True` set. In this situation, `null=True` is required to avoid unique constraint violations when saving multiple objects with blank values.

---
### Blank[Ture | False] 
Note that this is different than [`null`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.null "django.db.models.Field.null"). [`null`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.null "django.db.models.Field.null") is purely database-related, whereas [`blank`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.blank "django.db.models.Field.blank") is validation-related. If a field has `blank=True`, form validation will allow entry of an empty value. If a field has `blank=False`, the field will be required.
>**Supplying missing values**
	> `blank=True` can be used with fields having `null=False`, but this will require implementing [`clean()`](https://docs.djangoproject.com/en/4.0/ref/models/instances/#django.db.models.Model.clean "django.db.models.Model.clean") on the model in order to programmatically supply any missing values.
---
### Choices  [(A, B), (A, B) ...] : A - value, B - human readable name
``` Python
	YEAR_IN_SCHOOL_CHOICES = [
		 ('FR', 'Freshman'),
		 ('SO', 'Sophomore'),
		 ('JR', 'Junior'),
		 ('SR', 'Senior'),
		 ('GR', 'Graduate'),
	]
```
Generally, it’s best to define choices inside a model class, and to define a suitably-named constant for each value
``` Python
	class Student(models.Model):
		FRESHMAN = 'FR'
		SOPHOMORE = 'SO'
		JUNIOR = 'JR'
		SENIOR = 'SR'
		GRADUATE = 'GR'
		YEAR_IN_SCHOOL_CHOICES = [
			(FRESHMAN, 'Freshman'),
			(SOPHOMORE, 'Sophomore'),
			(JUNIOR, 'Junior'),
			(SENIOR, 'Senior'),
			(GRADUATE, 'Graduate'),
		]
		...
```
Unless [`blank=False`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.blank "django.db.models.Field.blank") is set on the field along with a [`default`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.default "django.db.models.Field.default") then a label containing `"---------"` will be rendered with the select box. To override this behavior, add a tuple to `choices` containing `None`; e.g. `(None, 'Your String For Display')`. Alternatively, you can use an empty string instead of `None` where this makes sense - such as on a [`CharField`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.CharField "django.db.models.CharField").
> **Note**
> A new migration is created each time the order of `choices` changes.
##### Enumeration types[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#enumeration-types "Permalink to this headline") 
In addition, Django provides enumeration types that you can subclass to define choices in a concise way.

``` Python
class Student(models.Model):

	class YearInSchool(models.TextChoices):
		FRESHMAN = 'FR', ('Freshman')
		SOPHOMORE = 'SO', ('Sophomore')
		JUNIOR = 'JR', ('Junior')
		SENIOR = 'SR', ('Senior')
		GRADUATE = 'GR', ('Graduate')

	year_in_school = models.CharField(
		max_length=2,
		choices=YearInSchool.choices,
		default=YearInSchool.FRESHMAN,
	)

	def is_upperclass(self):
		return self.year_in_school in {
			self.YearInSchool.JUNIOR,
			self.YearInSchool.SENIOR,
		}
```
These work similar to [`enum`](https://docs.python.org/3/library/enum.html#module-enum "(in Python v3.10)") from Python’s standard library, but with some modifications:
-   Enum member values are a tuple of arguments to use when constructing the concrete data type. Django supports adding an extra string value to the end of this tuple to be used as the human-readable name, or `label`. The `label` can be a lazy translatable string. Thus, in most cases, the member value will be a `(value, label)` two-tuple. See below for [an example of subclassing choices](https://docs.djangoproject.com/en/4.0/ref/models/fields/#field-choices-enum-subclassing) using a more complex data type. If a tuple is not provided, or the last item is not a (lazy) string, the `label` is [automatically generated](https://docs.djangoproject.com/en/4.0/ref/models/fields/#field-choices-enum-auto-label) from the member name.
-   A `.label` property is added on values, to return the human-readable name.
-   A number of custom properties are added to the enumeration classes – `.choices`, `.labels`, `.values`, and `.names` – to make it easier to access lists of those separate parts of the enumeration. Use `.choices` as a suitable value to pass to [`choices`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.choices "django.db.models.Field.choices") in a field definition.
> **Warning** 
> These property names cannot be used as member names as they would conflict.
- The use of enum.unique() is enforced to ensure that values cannot be defined multiple times. This is unlikely to be expected in choices for a field.

Since the case where the enum values need to be integers is extremely common, Django provides an `IntegerChoices` class. For example:Since the case where the enum values need to be integers is extremely common, Django provides an `IntegerChoices` class. For example:
``` python
class Card(models.Model):

    class Suit(models.IntegerChoices):
        DIAMOND = 1
        SPADE = 2
        HEART = 3
        CLUB = 4

    suit = models.IntegerField(choices=Suit.choices)
```
It is also possible to make use of the [Enum Functional API](https://docs.python.org/3/library/enum.html#functional-api) with the caveat that labels are automatically generated as highlighted above:
```python
>>> MedalType = models.TextChoices('MedalType', 'GOLD SILVER BRONZE')
>>> MedalType.choices
[('GOLD', 'Gold'), ('SILVER', 'Silver'), ('BRONZE', 'Bronze')]
>>> Place = models.IntegerChoices('Place', 'FIRST SECOND THIRD')
>>> Place.choices
[(1, 'First'), (2, 'Second'), (3, 'Third')]
```
---
### `db_column`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#db-column "Permalink to this headline")

The name of the database column to use for this field. If this isn’t given, Django will use the field’s name.

If your database column name is an SQL reserved word, or contains characters that aren’t allowed in Python variable names – notably, the hyphen – that’s OK. Django quotes column and table names behind the scenes'

---
### `db_index`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#db-index "Permalink to this headline")
If `True`, a database index will be created for this field.

---
### `db_tablespace`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#db-tablespace "Permalink to this headline")

The name of the [database tablespace](https://docs.djangoproject.com/en/4.0/topics/db/tablespaces/) to use for this field’s index, if this field is indexed. The default is the project’s [`DEFAULT_INDEX_TABLESPACE`](https://docs.djangoproject.com/en/4.0/ref/settings/#std:setting-DEFAULT_INDEX_TABLESPACE) setting, if set, or the [`db_tablespace`](https://docs.djangoproject.com/en/4.0/ref/models/options/#django.db.models.Options.db_tablespace "django.db.models.Options.db_tablespace") of the model, if any. If the backend doesn’t support tablespaces for indexes, this option is ignored.

---
### `default`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#default "Permalink to this headline")
The default value for the field. This can be a value or a callable object. If callable it will be called every time a new object is created.

---
### `editable`
If `False`, the field will not be displayed in the admin or any other [`ModelForm`](https://docs.djangoproject.com/en/4.0/topics/forms/modelforms/#django.forms.ModelForm "django.forms.ModelForm"). They are also skipped during [model validation](https://docs.djangoproject.com/en/4.0/ref/models/instances/#validating-objects). Default is `True`.

---
### `error_messages`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#error-messages "Permalink to this headline")
The `error_messages` argument lets you override the default messages that the field will raise. Pass in a dictionary with keys matching the error messages you want to override.

---
### `help_text`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#help-text "Permalink to this headline")

Extra “help” text to be displayed with the form widget. It’s useful for documentation even if your field isn’t used on a form.

Note that this value is _not_ HTML-escaped in automatically-generated forms. This lets you include HTML in [`help_text`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.help_text "django.db.models.Field.help_text") if you so desire. For example:
``` python
help_text="Please use the following format: <em>YYYY-MM-DD</em>."
```
---
### `primary_key`
If `True`, this field is the primary key for the model.

---
### `unique`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#unique "Permalink to this headline")

If `True`, this field must be unique throughout the table.

---
### `verbose_name`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#verbose-name "Permalink to this headline")

A human-readable name for the field. If the verbose name isn’t given, Django will automatically create it using the field’s attribute name, converting underscores to spaces. See [Verbose field names](https://docs.djangoproject.com/en/4.0/topics/db/models/#verbose-field-names).

---
### `validators`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#validators "Permalink to this headline")

A list of validators to run for this field. See the [validators documentation](https://docs.djangoproject.com/en/4.0/ref/validators/) for more information.

#### Registering and fetching lookups[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#registering-and-fetching-lookups "Permalink to this headline")

`Field` implements the [lookup registration API](https://docs.djangoproject.com/en/4.0/ref/models/lookups/#lookup-registration-api). The API can be used to customize which lookups are available for a field class, and how lookups are fetched from a field.

---
## Types
### `BooleanField`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#booleanfield "Permalink to this headline")

_class_ `BooleanField`(_**options_)[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.BooleanField "Permalink to this definition")

A true/false field.

The default form widget for this field is [`CheckboxInput`](https://docs.djangoproject.com/en/4.0/ref/forms/widgets/#django.forms.CheckboxInput "django.forms.CheckboxInput"), or [`NullBooleanSelect`](https://docs.djangoproject.com/en/4.0/ref/forms/widgets/#django.forms.NullBooleanSelect "django.forms.NullBooleanSelect") if [`null=True`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.null "django.db.models.Field.null").

The default value of `BooleanField` is `None` when [`Field.default`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.Field.default "django.db.models.Field.default") isn’t defined.

---
### `CharField`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#charfield "Permalink to this headline")

``` Python
class CharField(max_length=None, **options)
```

A string field, for small- to large-sized strings.

For large amounts of text, use [`TextField`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.TextField "django.db.models.TextField").

The default form widget for this field is a [`TextInput`](https://docs.djangoproject.com/en/4.0/ref/forms/widgets/#django.forms.TextInput "django.forms.TextInput").
CharField¶

For large amounts of text, use TextField.

The default form widget for this field is a TextInput.

#### CharField has two extra arguments: 
##### CharField.max_length¶

**Required**. The maximum length (in characters) of the field. The max_length is enforced at the database level and in Django’s validation using MaxLengthValidator.

>**Note**
If you are writing an application that must be portable to multiple database backends, you should be aware that there are restrictions on max_length for some backends. Refer to the database backend notes for details.

##### CharField.db_collation¶
Optional. The database collation name of the field.
[`CharField`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.CharField "django.db.models.CharField") has two extra arguments:

---

### `DateField`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#datefield "Permalink to this headline")

``` Python
class DateField(auto_now=False, auto_now_add=False,  **options)
```

A date, represented in Python by a `datetime.date` instance. Has a few extra, optional arguments:

`DateField.auto_now`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.DateField.auto_now "Permalink to this definition")

Automatically set the field to now every time the object is saved. Useful for “last-modified” timestamps. Note that the current date is _always_ used; it’s not just a default value that you can override.

The field is only automatically updated when calling [`Model.save()`](https://docs.djangoproject.com/en/4.0/ref/models/instances/#django.db.models.Model.save "django.db.models.Model.save"). The field isn’t updated when making updates to other fields in other ways such as [`QuerySet.update()`](https://docs.djangoproject.com/en/4.0/ref/models/querysets/#django.db.models.query.QuerySet.update "django.db.models.query.QuerySet.update"), though you can specify a custom value for the field in an update like that.

`DateField.auto_now_add`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.DateField.auto_now_add "Permalink to this definition")

Automatically set the field to now when the object is first created. Useful for creation of timestamps. Note that the current date is _always_ used; it’s not just a default value that you can override. So even if you set a value for this field when creating the object, it will be ignored. If you want to be able to modify this field, set the following instead of `auto_now_add=True`:

-   For [`DateField`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.DateField "django.db.models.DateField"): `default=date.today` - from [`datetime.date.today()`](https://docs.python.org/3/library/datetime.html#datetime.date.today "(in Python v3.10)")
-   For [`DateTimeField`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.DateTimeField "django.db.models.DateTimeField"): `default=timezone.now` - from [`django.utils.timezone.now()`](https://docs.djangoproject.com/en/4.0/ref/utils/#django.utils.timezone.now "django.utils.timezone.now")

The default form widget for this field is a [`DateInput`](https://docs.djangoproject.com/en/4.0/ref/forms/widgets/#django.forms.DateInput "django.forms.DateInput"). The admin adds a JavaScript calendar, and a shortcut for “Today”. Includes an additional `invalid_date` error message key.

The options `auto_now_add`, `auto_now`, and `default` are mutually exclusive. Any combination of these options will result in an error.

---

### DecimalField¶
```python
class DecimalField(max_digits=None, decimal_places=None, **options)
```
#### Has two required arguments:

##### DecimalField.max_digits¶
The maximum number of digits allowed in the number. Note that this number must be greater than or equal to decimal_places.

##### DecimalField.decimal_places¶
The number of decimal places to store with the number. For example, to store numbers up to 999.99 with a resolution of 2 decimal places, you’d use:
```python
models.DecimalField(..., max_digits=5, decimal_places=2)
```
And to store numbers up to approximately one billion with a resolution of 10 decimal places:
```python
models.DecimalField(..., max_digits=19, decimal_places=10)
```
The default form widget for this field is a NumberInput when localize is False or TextInput otherwise.

> **Note**
> For more information about the differences between the FloatField and DecimalField classes, please see FloatField vs. DecimalField. You should also be aware of SQLite limitations of decimal fields.

---
### `EmailField`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#emailfield "Permalink to this headline")
```python
class EmailField(max_length=254, **options)
```

A [`CharField`](https://docs.djangoproject.com/en/4.0/ref/models/fields/#django.db.models.CharField "django.db.models.CharField") that checks that the value is a valid email address using [`EmailValidator`](https://docs.djangoproject.com/en/4.0/ref/validators/#django.core.validators.EmailValidator "django.core.validators.EmailValidator").

---
### `FileField`[¶](https://docs.djangoproject.com/en/4.0/ref/models/fields/#filefield "Permalink to this headline")

```python
class FileField(upload_to='', storage=None, max_length=100, **options)
```

A file-upload field.

---

## CRUD
[Documetation](https://docs.djangoproject.com/en/4.0/topics/db/queries/)
```python
# ======= create =======
u = User(...)
u.save()

u = User.objects.create(...) # creates note at right time

# ======= read =======
User.objects.all()
User.objects.get(...)
User.objects.values() # SELECT * FROM tab_name;
User.objects.filter(...).values()  # SELECT * FROM tab_name WHERE conditions;

# ======= update =======
User.objects.filter(...).update(...) # ...

u.name = 'Sam'
u.save()

# ======= delete =======
User.objects.filter(...).values().delete() # DELETE ...;

# ======= order by =======
User.objects.values().order_by(...) # SELECT * FROM tab_name ORDER BY ...;

# ======= filter =======
User.objects.filter(...).values().delete() # DELETE ...;
```

## Meta
```python
def __str__(self):
	return [self.field_name]

```

