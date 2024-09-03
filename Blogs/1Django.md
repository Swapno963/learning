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
