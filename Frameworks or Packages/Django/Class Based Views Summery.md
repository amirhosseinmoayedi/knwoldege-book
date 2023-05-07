---
title: Class Based Views Summery
updated: 2021-09-13 11:15:06Z
created: 2021-09-03 08:14:04Z
latitude: 55.66890000
longitude: 12.58720000
altitude: 0.0000
---

All of the generic views are created base on **View** Class.

```python
from django.views.generic import View
```

as_view is a method of View class that would create an instance of that class and it would call the **dispatch method** of class ( a method that would run base on different method of http request like GET).\

we use View for customizing everything and have full control

**Template** View is a class that only render the template view.

==get context data== for customizing the returned context.(always call the super meth)

note: mode in blow text is a variable

**List View** : for showing the list of objects , it does not need to specify the template name it will look for **model_list** template . the default context name is **model_list**

**Detail View**:like list view but for a single object

**Create View**: will create form automatically with specifying fields of model that is specified , the form is a model form so it has an attribute named instance , after registration it will redirect to created object for that we should either provide get absolute URL or redirect template .

**Update View**: a view for editing the object and it will redirect the to object detail after editing using the redirect or get absolute URL , and like create view it will need field for creating model form.

**Delete View**: a view for deleting an object it's need a model and success URL

reverse lazy is like reverse but it's will be tun only when it's called .

for customizing the query set we overwrite the  **get_queryset** method .

in create and update view we can overwrite the **get initials** for accessing the initials data stored in form or instance.

**==Franken==** Class is class that is combination of two or more class

in franken views the order of using this mixins are important

```python
class TeamCreateDetailView(CreateView,DetailView):
```

if some action is repeated multiple time we should better to transform them to mixins.

```python
class SampleMixin:
    def get_context(...):
        do something
```

## Decorating class-based views

```python
urlpatterns = [
    path('about/', login_required(TemplateView.as_view(template_name="secret.html"))),
    path('vote/', permission_required('polls.can_vote')(VoteView.as_view())),
]
```

To decorate every instance of a class-based view, you need to decorate the class definition itself. To do this you apply the decorator to the [`dispatch()`](https://docs.djangoproject.com/en/3.2/ref/class-based-views/base/#django.views.generic.base.View.dispatch "django.views.generic.base.View.dispatch") method of the class.

on views that we specify for list which query set to be run if query set is empty it would return 404 not found, we should use `allow_empty` parameter.

The URLconf here uses the named group `pk` \- this name is the default name that `DetailView` uses to find the value of the primary key used to filter the queryset.

If you want to call the group something else, you can set [`pk_url_kwarg`](https://docs.djangoproject.com/en/3.2/ref/class-based-views/mixins-single-object/#django.views.generic.detail.SingleObjectMixin.pk_url_kwarg "django.views.generic.detail.SingleObjectMixin.pk_url_kwarg") on the view.

```python
class AuthorDetailView(DetailView):

    queryset = Author.objects.all()

    def get_object(self):
        obj = super().get_object()
        # Record the last accessed date
        obj.last_accessed = timezone.now()
        obj.save()
        return obj
```