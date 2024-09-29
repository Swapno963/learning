# Managing multiple databases in Django is useful for scaling applications, load balancing, data segmentation, or using different databases for different purposes (e.g., analytics, primary data). Here are some real-world examples and techniques of working with multiple databases in Django.

## Example 1: Read/Write Split
A common use case for multiple databases is having a primary database for writes and a replica database for reads. This is useful for load balancing, where the write operations go to the primary database, and the read operations can be handled by one or more replicas.

Settings Configuration
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'myuser',
        'PASSWORD': 'mypassword',
        'HOST': 'primary-db-server',
        'PORT': '5432',
    },
    'replica': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'mydatabase',
        'USER': 'myuser',
        'PASSWORD': 'mypassword',
        'HOST': 'replica-db-server',
        'PORT': '5432',
    }
}

```
Usage: Directing Queries to Different Databases
```python
from django.db import connections

# Writing to the primary database
with connections['default'].cursor() as cursor:
    cursor.execute("INSERT INTO myapp_table (name) VALUES (%s)", ['Sample Data'])

# Reading from the replica database
with connections['replica'].cursor() as cursor:
    cursor.execute("SELECT * FROM myapp_table WHERE name = %s", ['Sample Data'])
    rows = cursor.fetchall()

```


## Example 2: Different Databases for Different Apps
In some cases, different applications in a Django project might require separate databases. For instance, a company might want to store user data and analytics data in separate databases.

users/models.py
```python
from django.db import models

class UserProfile(models.Model):
    username = models.CharField(max_length=255)
    email = models.EmailField()

    class Meta:
        db_table = 'user_profile'

```

analytics/models.py
```python
from django.db import models

class UserActivity(models.Model):
    user_id = models.IntegerField()
    page_viewed = models.CharField(max_length=255)

    class Meta:
        db_table = 'user_activity'

```

Database Routers
Django allows you to define database routers to direct queries from certain models or apps to specific databases.

routers.py
```python
class DatabaseRouter:
    def db_for_read(self, model, **hints):
        """Direct read operations for models to appropriate database"""
        if model._meta.app_label == 'analytics':
            return 'analytics'
        return 'default'

    def db_for_write(self, model, **hints):
        """Direct write operations for models to appropriate database"""
        if model._meta.app_label == 'analytics':
            return 'analytics'
        return 'default'

    def allow_relation(self, obj1, obj2, **hints):
        """Allow relations only if both models are in the same database"""
        if obj1._meta.app_label == obj2._meta.app_label:
            return True
        return False

    def allow_migrate(self, db, app_label, model_name=None, **hints):
        """Ensure the apps are migrated to their respective databases"""
        if app_label == 'analytics':
            return db == 'analytics'
        return db == 'default'

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
```python

```