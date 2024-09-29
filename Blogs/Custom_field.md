# Custom fields in Django allow you to extend and create your own fields with custom behavior. Here are a few real-world examples of Custom Model Fields in Django.

## Example 1: Color Field
Imagine you're building a web app for managing digital products, and each product has a color that users can choose. Django doesn't have a built-in color field, so you can create a custom field to handle color values in the HEX format.

```python
from django.db import models
from django.core.exceptions import ValidationError
import re

class ColorField(models.CharField):
    description = "A custom field to store HEX color codes"

    def __init__(self, *args, **kwargs):
        kwargs['max_length'] = 7  # Length for #RRGGBB (7 characters)
        super().__init__(*args, **kwargs)

    def to_python(self, value):
        """Convert the value to the right format."""
        if not value:
            return None
        if not isinstance(value, str):
            raise ValidationError("Color must be a string")
        return value.upper()

    def validate(self, value, model_instance):
        """Check if the value is a valid HEX color code."""
        super().validate(value, model_instance)
        if not re.match(r'^#[0-9A-Fa-f]{6}$', value):
            raise ValidationError(f"{value} is not a valid HEX color code")

# Usage in models
class Product(models.Model):
    name = models.CharField(max_length=255)
    color = ColorField()

# Saving a product
product = Product.objects.create(name="T-Shirt", color="#FF5733")

```
## Example 2: Phone Number Field
For applications that deal with user contact information, like a CRM system, you may want to ensure that phone numbers are stored in a consistent format.
```python

from django.db import models
from django.core.exceptions import ValidationError
import re

class PhoneNumberField(models.CharField):
    description = "A custom field to store phone numbers in E.164 format"

    def __init__(self, *args, **kwargs):
        kwargs['max_length'] = 15  # +15555555555 (E.164 format)
        super().__init__(*args, **kwargs)

    def to_python(self, value):
        """Convert the value to the correct format."""
        if not value:
            return None
        if not isinstance(value, str):
            raise ValidationError("Phone number must be a string")
        return value

    def validate(self, value, model_instance):
        """Check if the value is a valid phone number."""
        super().validate(value, model_instance)
        if not re.match(r'^\+?[1-9]\d{1,14}$', value):
            raise ValidationError(f"{value} is not a valid phone number")

# Usage in models
class Contact(models.Model):
    name = models.CharField(max_length=255)
    phone_number = PhoneNumberField()

# Saving a contact
contact = Contact.objects.create(name="Alice", phone_number="+15555555555")


```
## Example 3: Percentage Field
If you're working on a financial or e-commerce platform, you might want to store percentages in a normalized way (e.g., between 0.0 and 1.0 instead of 0% to 100%).
```python
from django.db import models
from django.core.exceptions import ValidationError

class PercentageField(models.DecimalField):
    description = "A custom field to store percentage values as decimals"

    def __init__(self, *args, **kwargs):
        kwargs['max_digits'] = 5
        kwargs['decimal_places'] = 2
        super().__init__(*args, **kwargs)

    def validate(self, value, model_instance):
        """Ensure the value is between 0 and 1."""
        super().validate(value, model_instance)
        if value < 0 or value > 1:
            raise ValidationError(f"{value} is not a valid percentage. Must be between 0.0 and 1.0.")

# Usage in models
class ProductDiscount(models.Model):
    name = models.CharField(max_length=255)
    discount = PercentageField()

# Saving a product discount
discount = ProductDiscount.objects.create(name="Holiday Sale", discount=0.25)  # Represents 25% discount



```
## Example 4: MultiSelectField
Let's say you're developing a job listing site where recruiters need to select multiple skills for a candidate. Django's built-in CharField doesn't handle multiple values out of the box, so you can create a custom MultiSelectField for storing comma-separated values in a single database column.
```python
from django.db import models
from django.core.exceptions import ValidationError

class MultiSelectField(models.CharField):
    description = "A custom field to store comma-separated multiple values"

    def __init__(self, *args, **kwargs):
        kwargs['max_length'] = 255
        super().__init__(*args, **kwargs)

    def to_python(self, value):
        """Convert a string of comma-separated values into a list."""
        if not value:
            return []
        return value.split(',')

    def from_db_value(self, value, expression, connection):
        """Convert the comma-separated string from the database into a list."""
        if not value:
            return []
        return value.split(',')

    def get_prep_value(self, value):
        """Convert the list back into a comma-separated string for the database."""
        if not value:
            return ''
        if isinstance(value, list):
            return ','.join(value)
        return value

# Usage in models
class Candidate(models.Model):
    name = models.CharField(max_length=255)
    skills = MultiSelectField()

# Saving a candidate's skills
candidate = Candidate.objects.create(name="Bob", skills=['Python', 'Django', 'React'])

# Retrieving and using the skills
candidate = Candidate.objects.get(name="Bob")
print(candidate.skills)  # Outputs: ['Python', 'Django', 'React']



```
## Example 5: JSON Field with Validation
In some applications, you might need to store structured data in JSON format. Django's JSONField allows you to store JSON objects, but you can create a custom field to enforce specific validation rules.
```python
from django.db import models
from django.core.exceptions import ValidationError
import json

class ValidatedJSONField(models.JSONField):
    description = "A JSON field with custom validation"

    def validate(self, value, model_instance):
        """Ensure the value is a dictionary with a 'name' key."""
        super().validate(value, model_instance)
        if not isinstance(value, dict):
            raise ValidationError("Value must be a dictionary")
        if 'name' not in value:
            raise ValidationError("The JSON must contain a 'name' key")

# Usage in models
class Config(models.Model):
    name = models.CharField(max_length=255)
    settings = ValidatedJSONField()

# Saving a configuration
config = Config.objects.create(name="Site Settings", settings={'name': 'My Site', 'theme': 'dark'})



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