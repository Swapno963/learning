

1. Composite Key (Unique Together) Constraint
Definition: Ensures that a combination of two or more fields is unique across rows.
Example: A StudentCourse model where the combination of student and course must be unique.

Use Case: Ensures that a student cannot enroll in the same course multiple times.

```ch
from django.db import models

class Student(models.Model):
    name = models.CharField(max_length=100)

class Course(models.Model):
    title = models.CharField(max_length=100)

class StudentCourse(models.Model):
    student = models.ForeignKey(Student, on_delete=models.CASCADE)
    course = models.ForeignKey(Course, on_delete=models.CASCADE)

    class Meta:
        unique_together = ('student', 'course')

```


2. Unique Together (Composite Unique) Constraint using UniqueConstraint
Definition: Ensures that a specific set of fields must be unique when combined.
Example: A Flight model where the combination of flight_number and departure_date must be unique.

```ch
from django.db import models

class Flight(models.Model):
    flight_number = models.CharField(max_length=10)
    departure_date = models.DateField()

    class Meta:
        constraints = [
            models.UniqueConstraint(fields=['flight_number', 'departure_date'], name='unique_flight_schedule')
        ]
```

3. Check Constraint
Definition: Ensures that all values in a field meet a specific condition.
Example: A Person model where the age must be at least 18.


Use Case: Ensures that no person under 18 can be added to the database.

```ch
from django.db import models
from django.db.models import Q

class Person(models.Model):
    name = models.CharField(max_length=100)
    age = models.IntegerField()

    class Meta:
        constraints = [
            models.CheckConstraint(check=Q(age__gte=18), name='age_gte_18')
        ]

```


4.Ensure Start Date is Before End Date
Use Case: Ensures that the start_date is always before the end_date for an event

```ch
from django.db import models
from django.db.models import F

class Event(models.Model):
    name = models.CharField(max_length=100)
    start_date = models.DateField()
    end_date = models.DateField()

    class Meta:
        constraints = [
            models.CheckConstraint(check=Q(start_date__lt=F('end_date')), name='start_before_end')
        ]
```


5. Ensure Phone Number Length is Exactly 10 Digits
Use Case: Ensures that the phone_number is exactly 10 digits long.

```ch
from django.db import models

class Contact(models.Model):
    phone_number = models.CharField(max_length=10)

    class Meta:
        constraints = [
            models.CheckConstraint(check=Q(phone_number__length=10), name='phone_number_length_10')
        ]

```