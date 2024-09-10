## Faker is a Python package used for generating fake data, which is helpful for testing, seeding databases, or creating dummy datasets for development. It can generate a wide variety of fake data types such as names, addresses, phone numbers, emails, job titles, and more. This is particularly useful when you need realistic yet randomly generated data without having to manually create it.

Key Features:
1. Generates random data for names, addresses, emails, phone numbers, dates, and more.
2. Supports multiple locales (e.g., English, French, etc.) for locale-specific data.
3. Easily extensible and customizable.



## Installtion :
```ch
pip install faker
```
## Example of using Faker in Django to populate a model:
```ch
from faker import Faker
from myapp.models import User

fake = Faker()

for _ in range(10):
    User.objects.create(
        name=fake.name(),
        email=fake.email(),
        address=fake.address(),
    )
```