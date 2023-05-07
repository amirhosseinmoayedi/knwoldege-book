---
title: Forms
updated: 2021-09-13 11:22:15Z
created: 2021-09-13 11:17:54Z
latitude: 35.69800000
longitude: 51.41150000
altitude: 0.0000
---

we can use form with form view which auto generate forms 

if we use model form instead of forms after the creation or update it uses the get\_absolute\_url on the model .

We have to use [`reverse_lazy()`](https://docs.djangoproject.com/en/3.2/ref/urlresolvers/#django.urls.reverse_lazy "django.urls.reverse_lazy") instead of `reverse()`, as the urls are not loaded when the file is imported.

The `fields` attribute works the same way as the `fields` attribute on the inner `Meta` class on [`ModelForm`](https://docs.djangoproject.com/en/3.2/topics/forms/modelforms/#django.forms.ModelForm "django.forms.ModelForm"). Unless you define the form class in another way, the attribute is required and the view will raise an [`ImproperlyConfigured`](https://docs.djangoproject.com/en/3.2/ref/exceptions/#django.core.exceptions.ImproperlyConfigured "django.core.exceptions.ImproperlyConfigured") exception if it’s not.