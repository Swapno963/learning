# Django Proxy Models

 proxy models in Django can do more than just customize get_queryset() in their managers. They are essentially regular models that inherit from an existing model, allowing you to:

Add or customize methods
Implement custom behavior for existing methods
Override model metadata (via Meta class)
Extend or change functionality without modifying the database schema
Here are other things you can do with proxy models beyond just customizing get_queryset():

## Use Cases

### 1. Add Custom Methods
You can define custom methods in a proxy model that provide additional functionality unique to the subclass.


In this example, InProgressTask adds a custom method is_urgent() that checks if a task is urgent based on its due date. This method is available only on InProgressTask instances, not on the base Task model.

```python
class InProgressTask(Task):
    class Meta:
        proxy = True

    def is_urgent(self):
        return self.due_date <= timezone.now() + timedelta(days=1)
```



### 2. Add Custom Model Properties
You can add properties to the proxy model to derive new attributes based on the existing fields.

This days_since_published property is specific to PublishedBook and calculates the number of days since the book was published.
```python
class PublishedBook(Book):
    class Meta:
        proxy = True

    @property
    def days_since_published(self):
        return (timezone.now() - self.published_date).days

```
### 3. Custom Model Methods with Additional Logic
You can override and add methods that work with the same database but provide different behavior.

```python
class ActiveEmployee(Employee):
    class Meta:
        proxy = True

    def retire(self):
        self.is_active = False
        self.save()

```
### 4. Change Model Metadata (Meta Class)
You can use the Meta class in a proxy model to alter its behavior. This includes things like default ordering, custom permissions, or unique constraints, without affecting the base model

Here, the proxy model CompletedOrder has a different ordering and display names (verbose_name, verbose_name_plural) in the Django admin interface.
```python
class CompletedOrder(Order):
    class Meta:
        proxy = True
        ordering = ['-completed_date']
        verbose_name = "Completed Order"
        verbose_name_plural = "Completed Orders"

```
### 5. Add/Customize Permissions
You can add custom permissions for your proxy models that are separate from the original model.

This adds a custom permission can_access_vip_section for the VIPCustomer proxy model, allowing you to assign it separately from the base Customer model.
```python
class VIPCustomer(Customer):
    class Meta:
        proxy = True
        permissions = [
            ("can_access_vip_section", "Can access VIP section")
        ]

```
### 6. Override Save/Delete Methods
You can override the save() or delete() methods of proxy models to introduce custom save logic without changing the base model’s behavior.

In this case, ArchivedPost overrides the save() method to automatically set is_archived to True whenever an ArchivedPost is saved.
```python
class ArchivedPost(Post):
    class Meta:
        proxy = True

    def save(self, *args, **kwargs):
        self.is_archived = True
        super().save(*args, **kwargs)

```
### 7. Customize Validation
You can override the clean() method in a proxy model to introduce specific validation logic, ensuring that proxy objects meet additional constraints.

```python
class PublishedBook(Book):
    class Meta:
        proxy = True

    def clean(self):
        if self.publish_date > timezone.now():
            raise ValidationError("Publish date cannot be in the future")

```
### 8.  Change Default Queryset Behavior
You can also alter the default manager to change how queries are made by default.

```python
class PublishedArticle(Article):
    class Meta:
        proxy = True

    def save(self, *args, **kwargs):
        if not self.is_published:
            raise ValidationError("Article must be published")
        super().save(*args, **kwargs)

```
### 9. Use Proxy Models to Restrict Views
You can create proxy models to simplify views or other parts of your application.

```python
class OpenComplaint(Complaint):
    class Meta:
        proxy = True

    def close(self):
        self.status = 'Closed'
        self.save()

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```
### . 

```python

```