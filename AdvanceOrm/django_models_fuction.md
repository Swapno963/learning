### The django.db.models.functions module provides a wide range of built-in functions that you can use in your Django queries
1. *Abs*:Returns the absolute value of a field.
```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```
Ceil: Rounds a number up to the nearest integer.
Floor: Rounds a number down to the nearest integer.
Round: Rounds a number to the specified number of decimal places.


2. *Length*:Returns the length of a text field.
```ch
results = MyModel.objects.annotate(text_length=Length('text_field'))
```
Lower:Converts a text field to lowercase.
Upper: Converts a text field to uppercase.
Concat: Concatenates multiple fields or values.




3. *Substr*:Extracts a substring from a text field.
```ch
results = MyModel.objects.annotate(sub_string=Substr('text_field', 1, 5))  # Gets the first 5 characters
```
4. *Now*:Returns the current date and time.
```ch
results = MyModel.objects.filter(some_date__lte=Now())
```
5. *TruncDate*:Truncates a date or datetime field to just the date.
```ch 
results = MyModel.objects.annotate(truncated_date=TruncDate('datetime_field'))
```
ExtractMonth:Extracts the month from a date or datetime field.
ExtractDay: Extracts the day from a date or datetime field.




6. *Coalesce*:Returns the first non-null value among the given arguments.
```ch
results = MyModel.objects.annotate(coalesced_value=Coalesce('nullable_field', Value('default_value')))
```

1. *Abs*:Returns the absolute value of a field.

```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```
1. *Abs*:Returns the absolute value of a field.

```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```
1. *Abs*:Returns the absolute value of a field.

```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```
1. *Abs*:Returns the absolute value of a field.

```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```
1. *Abs*:Returns the absolute value of a field.

```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```
1. *Abs*:Returns the absolute value of a field.

```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```
1. *Abs*:Returns the absolute value of a field.

```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```
1. *Abs*:Returns the absolute value of a field.

```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```
1. *Abs*:Returns the absolute value of a field.

```ch
results = MyModel.objects.annotate(abs_value=Abs('some_field'))
```