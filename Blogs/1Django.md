Firt part of "Interestiong django facks"

1.The development server automatically reloads Python code for each request as needed But actions like adding files don’t trigger a restart, so you’ll have to restart the server in these cases.

2.Power of name in url

```ch
    path('posts/<int:pk>/', PostDetail.as_view(), name='post-detail'),


class PostSerializer(serializers.ModelSerializer):
    url = serializers.SerializerMethodField()

    class Meta:
        model = Post
        fields = ['title', 'content', 'url']

    def get_url(self, obj):
        return reverse('post-detail', kwargs={'pk': obj.id})
```

and the responce is
{
"title": "My First Post",
"content": "This is the content of the first post.",
"url": "http://example.com/posts/1/"
}
the get_url function in the PostSerializer will be executed automatically by Django REST Framework (DRF) whenever an instance of PostSerializer is used to serialize a Post object

3.The path() function is passed four arguments, two required: route and view, and two optional: kwargs, and name.kwargs allows you to pass additional keyword arguments to the view function or class-based view that is associated with the URL pattern.

    path('profile/<int:user_id>/', views.profile_view, name='profile', kwargs={'editable': False}),


    def profile_view(request, user_id, editable):
        if editable:
        return HttpResponse(f'User {user_id} profile is editable.')
        else:
        return HttpResponse(f'User {user_id} profile is not editable.')

4.While you’re editing mysite/settings.py, set TIME_ZONE to your time zone.

5.verbose name
pub_date = models.DateTimeField("date published")
The verbose_name "date published" will be displayed as the label in the Django admin interface or any forms generated using this model.

6.If you’re interested, you can also run python manage.py check; this checks for any problems in your project without making migrations or touching the database.

7. interactive Python shell and play around with the free API Django gives you.
   python manage.py shell

> > > from polls.models import Choice, Question # Import the model classes we just wrote.

# No questions are in the system yet.

> > > Question.objects.all()
> > > <QuerySet []>

# Create a new Question.

# Support for time zones is enabled in the default settings file, so

# Django expects a datetime with tzinfo for pub_date. Use timezone.now()

# instead of datetime.datetime.now() and it will do the right thing.

> > > from django.utils import timezone
> > > q = Question(question_text="What's new?", pub_date=timezone.now())

# Save the object into the database. You have to call save() explicitly.

> > > q.save()

# Now it has an ID.

> > > q.id
> > > 1

# Access model field values via Python attributes.

> > > q.question_text
> > > "What's new?"
> > > q.pub_date
> > > datetime.datetime(2012, 2, 26, 13, 0, 0, 775217, tzinfo=datetime.timezone.utc)

# Change values by changing the attributes, then calling save().

> > > q.question_text = "What's up?"
> > > q.save()

# objects.all() displays all the questions in the database.

> > > Question.objects.all()
> > > <QuerySet [<Question: Question object (1)>]>

8. custom method to model

```ch
class Article(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()
    pub_date = models.DateTimeField("date published")

    def __str__(self):
        return self.title

    def was_published_recently(self):
        """
        Returns True if the article was published within the last 24 hours.
        """
        now = timezone.now()
        return now - timezone.timedelta(days=1) <= self.pub_date <= now



def article_list(request):
    articles = Article.objects.all()
    for article in articles:
        if article.was_published_recently():
            print(f"{article.title} was published recently.")
    return render(request, 'articles/article_list.html', {'articles': articles})


```

9.how to use double underscores to perform field lookups via the API
In Django, double underscores (\_\_) are used to perform field lookups in queries. These lookups allow you to filter, sort, and retrieve specific data based on related fields or specific conditions.

To filter a queryset based on an exact match of a field's value.

```ch
queryset = MyModel.objects.filter(field_name__exact='value')
```

To filter a queryset based on a case-insensitive exact match.

```ch
queryset = MyModel.objects.filter(field_name__iexact='value')

```

To filter a queryset based on whether a field contains a certain substring.

```ch
queryset = MyModel.objects.filter(field_name__contains='substring')

```

To filter a queryset based on whether a field contains a certain substring, case-insensitively.

```ch
queryset = MyModel.objects.filter(field_name__icontains='substring')

```

To filter based on whether a field's value is greater than or less than a certain value.

```ch
queryset = MyModel.objects.filter(field_name__gt=value)   # Greater than
queryset = MyModel.objects.filter(field_name__lt=value)   # Less than
queryset = MyModel.objects.filter(field_name__gte=value)  # Greater than or equal to
queryset = MyModel.objects.filter(field_name__lte=value)  # Less than or equal to

```

To filter based on specific date components (year, month, day).

```ch
queryset = MyModel.objects.filter(date_field__year=2024)
queryset = MyModel.objects.filter(date_field__month=9)
queryset = MyModel.objects.filter(date_field__day=2)

```

To filter based on whether a field's value is NULL or not.

```ch
queryset = MyModel.objects.filter(field_name__isnull=True)   # Is null
queryset = MyModel.objects.filter(field_name__isnull=False)  # Is not null
```

To filter based on a related model's field.

```ch
queryset = MyModel.objects.filter(related_model__related_field='value')

```

related_model\_\_related_field='value':

1. related_model: This is the name of the related field on MyModel. It refers to another model that is linked to MyModel via a foreign key, one-to-one, or many-to-many relationship.
2. \_\_ (double underscores): In Django, double underscores are used to perform lookups across relationships. It allows you to traverse the relationships between models.
3. related_field: This is the field in the related model that you want to filter by.
4. 'value': This is the value you are filtering for in the related_field of the related_model.

   10.The AUTH_PASSWORD_VALIDATORS setting in Django is a list of validators that are used to enforce specific rules on user passwords to enhance security. Each validator in the list serves a unique purpose, from checking the similarity of the password to user attributes to ensuring the password's length and complexity.

**UserAttributeSimilarityValidator**:his validator checks if the password is too similar to the user's personal information, such as their username, first name, last name, or email address. The idea is to prevent users from creating passwords that are easily guessable based on their known personal data.

```ch
{
    'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    'OPTIONS': {
        'user_attributes': ('username', 'first_name', 'last_name', 'email'),
        'max_similarity': 0.7,  # Default is 0.7, but you can adjust the similarity threshold
    },
}
```

_MinimumLengthValidator_:This validator enforces a minimum length requirement on passwords. It helps ensure that passwords are of a sufficient length to be secure, as shorter passwords are generally easier to crack.

```ch
{
    'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    'OPTIONS': {
        'min_length': 8,  # Default is 8, but you can set it to any number
    },
}
```

_CommonPasswordValidator_:This validator checks if the password is too common. Django maintains a list of common passwords (e.g., password, 12345678, qwerty) that are easily guessable by attackers. The validator ensures that users do not choose such passwords.

```ch
{
    'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    'OPTIONS': {
        'password_list_path': '/path/to/common_passwords.txt',  # Optional: Custom path to a list of common passwords
    },
}
```

_NumericPasswordValidator_:This validator checks if the password consists entirely of numeric digits. Numeric passwords (e.g., 12345678) are generally weaker and easier to guess. This validator ensures that the password is not purely numeric.

```ch
{
    'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
}

```

\*\*

### choice_set vs. related_name in Django

```ch
choice_set (Default Reverse Relation):

Django automatically adds a reverse relationship for ForeignKey fields.
By default, if a model Choice has a ForeignKey to Question, Django will give the reverse relation a name like question.choice_set.
Example: To get all choices related to a question, you would use question.choice_set.all().
related_name (Custom Reverse Relation):

Allows you to customize the reverse relationship name for a ForeignKey.
By specifying related_name='choices' in the Choice model, you can access related choices for a question using question.choices.all() instead of the default choice_set.
This improves code readability and avoids conflicts if there are multiple ForeignKey relationships in the same model.
Example: Instead of using choice_set, you can now use a more meaningful question.choices.all().
```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

\*\*

```ch

```

```ch

```
