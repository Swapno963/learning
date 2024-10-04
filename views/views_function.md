## In order to show customized HTML when Django returns a 404, you can create an HTML template named 404.html and place it in the top level of your template tree. This template will then be served when DEBUG is set to False.

When DEBUG is True, you can provide a message to Http404 and it will appear in the standard 404 debug template. Use these messages for debugging purposes; they generally aren’t suitable for use in a production 404 template.

```python
from django.http import Http404
from django.shortcuts import render
from polls.models import Poll


def detail(request, poll_id):
    try:
        p = Poll.objects.get(pk=poll_id)
    except Poll.DoesNotExist:
        raise Http404("Poll does not exist")
    return render(request, "polls/detail.html", {"poll": p})
```

## Async views¶
As well as being synchronous functions, views can also be asynchronous (“async”) functions, normally defined using Python’s async def syntax. Django will automatically detect these and run them in an async context. However, you will need to use an async server based on ASGI to get their performance benefits.

Here’s an example of an async vie

```python
import datetime
from django.http import HttpResponse


async def current_datetime(request):
    now = datetime.datetime.now()
    html = "<html><body>It is now %s.</body></html>" % now
    return HttpResponse(html)
```
## Use Case and Benefits of Async Views vs Normal Views in Django
1. Normal Views (Synchronous)
In Django, views are typically synchronous by default. They process HTTP requests sequentially, one after the other. For most web applications, synchronous views are efficient enough and don't require any special setup.

Use Cases for Normal Views:
Simple CRUD operations: Normal views work well for common database operations where requests like creating, reading, updating, or deleting records are performed.
Low I/O Operations: If the application doesn't rely on many external services (e.g., external APIs, database queries), synchronous views are sufficient.
Small-scale applications: Smaller applications with low concurrency and minimal external integrations don’t need the added complexity of async.
Benefits of Normal Views:
Simplicity: Easier to implement and understand, especially for developers who are not familiar with asynchronous programming.
Compatible with most middleware: Most of Django’s middleware and third-party libraries are designed for synchronous views.
No risk of race conditions: Since everything runs in a sequential manner, there’s no risk of shared state being updated unexpectedly, which can happen in async views.
Challenges with Normal Views:
Blocking I/O: Normal views can block the request/response cycle while waiting for long-running tasks like external API calls or database queries, which can degrade performance under heavy load.
Scaling: Scaling an app with many concurrent users can be challenging if views are synchronous and need to handle long-running tasks.
2. Async Views (Asynchronous)
Django 3.1 introduced support for asynchronous views using Python's asyncio to handle requests. These views enable Django to handle I/O-bound tasks more efficiently.

Use Cases for Async Views:
I/O Bound Operations: Async views are ideal for tasks that involve waiting for external resources (e.g., APIs, databases, file I/O, etc.). For instance:
Making API calls to external services.
Sending emails.
Querying multiple slow databases.
Real-time applications: Applications like chat systems, video conferencing, or other real-time data streaming features that need to handle many concurrent users.
Websockets: If your application involves websockets or background tasks (like live updates), async views can handle this more effectively.
Multiple slow resources: When a view interacts with multiple slow resources, such as several database queries or remote API requests, an async view allows them to run concurrently rather than sequentially.
Benefits of Async Views:
Concurrency: Async views allow Django to handle multiple requests concurrently without blocking. This means that while one request is waiting for I/O to complete (e.g., fetching data from an API), Django can process other requests.
Non-blocking I/O: They prevent blocking the whole request cycle when waiting for I/O operations, allowing the app to be more responsive under heavy load.
Efficiency with Slow I/O: Async views are great for situations where you’re waiting for external services, as they allow Django to handle multiple requests while the system waits for a response.
Challenges with Async Views:
Complexity: Writing and debugging asynchronous code can be more challenging compared to synchronous code, especially for developers unfamiliar with asyncio.
Limited Middleware and Libraries: Not all Django middleware or third-party libraries are compatible with async views. If you use middleware or libraries that aren’t async-safe, you may run into issues.
Database Limitations: Django’s ORM is not fully async yet. You’ll need to use third-party packages for async database operations, which adds complexity to your project.
Real-World Scenario:
Normal View Use Case:
You have a simple blog application where users can read articles, post comments, and browse content. Most of the views consist of straightforward database queries, so synchronous views will work perfectly.

Async View Use Case:
You are building a dashboard where data is pulled from multiple external APIs (e.g., financial data, weather updates) and displayed to the user in real-time. Using async views will let you fetch this data concurrently from multiple APIs, improving the performance and responsiveness of your dashboard.



## get_object_or_404()
Calls get() on a given model manager, but it raises Http404 instead of the model’s DoesNotExist exception.

 
```python
from django.shortcuts import get_object_or_404


def my_view(request):
    obj = get_object_or_404(MyModel, pk=1)
```

This example is equivalent to:
```python
from django.http import Http404


def my_view(request):
    try:
        obj = MyModel.objects.get(pk=1)
    except MyModel.DoesNotExist:
        raise Http404("No MyModel matches the given query.")
```
We can also do this 
```python
get_object_or_404(Book, title__startswith="M", pk=1)
```



## get_list_or_404
Returns the result of filter() on a given model manager cast to a list, raising Http404 if the resulting list is empty.


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