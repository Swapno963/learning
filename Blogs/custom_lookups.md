# Custom lookups in Django allow developers to create their own query expressions that can be used in filter(), exclude(), or other querysets, enhancing the ORM’s capabilities. Custom lookups extend the functionality of Django querysets by adding new ways to perform comparisons or matches.

Here are a few real-world examples of how to create and use custom lookups in Django:

Example 1: Case-Insensitive Partial Match Lookup
Suppose you want to create a custom lookup to perform case-insensitive partial string matching. By default, iexact provides case-insensitive matches, but it’s for exact matches only. Let’s create a lookup that finds items where the field contains part of the string, ignoring case sensitivity.

Defining the Lookup
```python
from django.db.models import Lookup, CharField
from django.db.models.lookups import IContains
from django.db.models import Transform

# Create a custom transform to lower case the field
class Lowercase(Transform):
    lookup_name = 'lower'
    
    def as_sql(self, compiler, connection):
        lhs, params = compiler.compile(self.lhs)
        return 'LOWER(%s)' % lhs, params

CharField.register_lookup(Lowercase)

# Create a custom lookup for case-insensitive partial match
class CaseInsensitivePartialMatch(Lookup):
    lookup_name = 'ci_contains'  # Name of the lookup
    
    def as_sql(self, compiler, connection):
        lhs, lhs_params = self.process_lhs(compiler, connection)
        rhs, rhs_params = self.process_rhs(compiler, connection)
        params = lhs_params + rhs_params
        return f"LOWER({lhs}) LIKE LOWER(%s)", ["%" + rhs[0] + "%"] + params

CharField.register_lookup(CaseInsensitivePartialMatch)


```
# Usage Now, in your queries, you can use this custom lookup:
```python
# Suppose we have a Product model with a name field
from myapp.models import Product

# Filter products with a case-insensitive partial match in the name
products = Product.objects.filter(name__ci_contains='laptop')

```




## Example 2: Date Field Custom Lookup: Future Dates
If you need a custom lookup for filtering objects where a DateField is in the future, this can be useful for filtering events, deadlines, etc.

Defining the Lookup
```python
from django.db.models import Lookup, DateField
from django.utils import timezone

class FutureDateLookup(Lookup):
    lookup_name = 'future'

    def as_sql(self, compiler, connection):
        lhs, lhs_params = self.process_lhs(compiler, connection)
        # Use timezone.now() to get the current date and compare
        return f"{lhs} > %s", [timezone.now()]

DateField.register_lookup(FutureDateLookup)

```
## useage
```python
from myapp.models import Event

# Get events that are scheduled for a future date
future_events = Event.objects.filter(start_date__future=True)

```


## Example 3: String Field Lookup: Matching Prefix or Suffix
Let’s create a custom lookup that checks whether a string field starts with or ends with a specific character or sequence of characters.
```python
from django.db.models import Lookup, CharField

class StartsWithVowelLookup(Lookup):
    lookup_name = 'startswith_vowel'

    def as_sql(self, compiler, connection):
        lhs, lhs_params = self.process_lhs(compiler, connection)
        # Check if the field starts with any vowel
        return f"{lhs} ~* %s", ['^[aeiouAEIOU]']

CharField.register_lookup(StartsWithVowelLookup)

```

# Useage
```python
from myapp.models import Article

# Get all articles where the title starts with a vowel
vowel_titles = Article.objects.filter(title__startswith_vowel=True)

```




```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```
```python

```