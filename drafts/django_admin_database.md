---
title: "Django Part 3: The Admin Page and Database"
layout: inner
post_link: 'nicholas-ko-zy.github.io/blog/django_admin_database.html'
publish_date: 2025-04-12
---

# Part 4 Admin Page

* Create new super user to login in as admin to your site.
* Create database (database migration, apply changes to database, for us, will create an auth_user table)
* Run `python manage.py makemigrations`
* Run `python manage.py migrate`
* Open terminal and enter the following command:
* `python manage.py createsuperuser`

# Part 5 Database and Migrations

* Sqlite for development
* Postgres for production

Add the stuff you need in `models.py`
```python
#models.py
from django.db import models
from django.utils import timezone
from django.contrib.auth.models import User

class Post(models.Model):
    #  Create title attribute, a character field with a max length
    title = models.CharField(max_length=100)
    # Create content attribute as a textfield
    content = models.TextField()
    date_posted = models.DateTimeField(default=timezone.now)
    # on_delete removes all the posts if the user is deleted
    author = models.ForeignKey(User, on_delete=models.CASCADE)
```
In your terminal

* Run `python manage.py makemigrations`
* Run `python manage.py sqlmigrate blog <migration_number>`
* Replace the migration_number with 0001.

This is the terminal output you'll see. 

![](img/corey_django/sql_migrate_initial.png)

Turns class into SQL commands. Saves time for us to write the SQL ourselves. Object relational mappers makes it possible to write SQL code using on Python objects.

* Run migrate command: `python manage.py migrate`. You should get an OK status.
  
Creating a new post

* Run `python manage.py shell` to go into the Python terminal that can interact with Django objects
* Run `from blog.models import Post`
* Run `from django.contrib.auth.models import User`
* Assign user var to some user, i.e. `user = User.objects.filter(username='nkzy1517').first()`
* `user = User.objects.get(id=1)`
* Create a post instance: `post_1 = Post(title='Blog 1', content='First Post Content!', author=user)`
* Save post: `post_1.save()`
* Check that your post is saved: `Post.objects.all()`
* Creating a post through 
  * Either create another post instance, specifying the author OR
  * Run `user.post_set.create(title='Blog 3', content='Third Post Content!')` and you can leave out the author and the `.save()` method.

Instead of building out posts based on hardcoded dummy data, now we try to get the information from a SQL query to a database. 

![](img/corey_django/dummy_data.png)

*Hardcoded dummy data in `views.py`*

```python
# views.py
# Old code
def home(request):
    context = {
        'posts': posts
    }
    return render(request, template_name='blog/home.html', context=context)
```

```python
# views.py
# New code
def home(request):
    context = {
        'posts': Post.objects.all()
    }
    return render(request, template_name='blog/home.html', context=context)
```
The new code chunk uses the object relational mappers, which turn OOP Python code into SQL queries to the database. Using the `objects.all()` method gives all the posts.

Next we want to have an option to add posts via the admin page, i.e. http://127.0.0.1:8000/admin/. To do that you have to go to your `blog` folder's `admin.py` file

```python
# blog/admin.py
from django.contrib import admin
from .models import Post

admin.site.register(Post)
```

This is reflected in your admin GUI.

![](img/corey_django/admin_gui_post1.png)

![](img/corey_django/admin_gui_post2.png)

# Part 6 - User Registration
Goal: Create a front-end form, the kinds you typically see when you sign up for an account with a new platform.

First, we create an app called `users`. This will handle all our user features, like creating users. To create an app, recall that we run the terminal command:

```
python manage.py startapp users
```

Then we will get a new folder. Now we need to add our newly created `users` app to out settings.py file's `INSTALLED_APPS` list. Question: How do we know what name to add to the list? Well, we can think of the app you want to add as something like an import statement. You begin with the folder, followed by the subfolders, adding a period `.` as you go down the folder levels. Finally you end with the class name of your app. The class name is found in the `apps.py` file.

```python
# users/apps.py
class UsersConfig(AppConfig):
    default_auto_field = 'django.db.models.BigAutoField'
    name = 'users'
```

So now we have 

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig',
    'users.apps.UsersConfig', # <- Newly added app file
]
```

Now we write the functions to create forms. Django comes with built-in classes for creating forms.

```python
from django.contrib.auth.forms import UserCreationForm
```

We create a template for this in HTML. Recall that to create a template for an app, we need to create this 'redundant' folder structure:

app_name > templates > app_name > template_name.html

Now instead of creating a `urls` module for our `users` app just as we did for blog, we are going to directly import the views function from the `users` app into the overall project's `urls.py` file.

See the difference between blog and users?

```python
from django.contrib import admin
from django.urls import path, include
from users import views as user_views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include('blog.urls')),
    path('register/', user_views.register, name='register'),
]
```

Now we have a basic registration page when we navigate to that URL. 

![](/img/corey_django/user_register_page.png)

Now we can make it look a little nicer using the form template. Simple change is to do `{{ form.as_p }}` instead of `{{ form }}`.

Key takeaway: User form gives us this user form all out of the box.

Now we want to handle the POST request that we get after the completed form. To do that, we would like to 

1. Handle the underlying POST
2. Redirect the user to home page and print a message communicating the status of the sign-up, i.e. If it was successful or not.


For (1), we can add a conditional to handle the POST request in the  `users/views.py` file. For (2), we will report a success message and add a highlighted bar at the top of the home-page, right above the posts.

(1)'s code change:

```python
# Create your views here.
def register(request):
    # Add in conditional to handle a POST request, to validate the form data
    if request.method == 'POST':
        form = UserCreationForm(request.POST)
        if form.is_valid():
            # Form passes validation check, get the username and return a message.
            username = form.cleaned_data.get('username')
            messages.success(request, f'Account created for {username}!')
            # After a successful sign-up, use redirect function to
            # send the user back to the homepage.
            return redirect('blog-home')
    else:
        form = UserCreationForm()
    return render(request, 'users/register.html', {'form': form})
```

(2)'s code change, focus being on the if messages block.

```html
{% raw %}
-snip-

<div class="row">
    <div class="col-md-8">
    {% if messages %}
        {% for message in messages %}
        <div class="alert alert-{{ message.tags}}"> 
            {{ message }}
        </div>
        {% endfor %}  
    {% endif %}
    {% block content %}{% endblock %}
    </div>
    <div class="col-md-4">
    <div class="content-section">
        <h3>Our Sidebar</h3>

-snip-
{% endraw %}
```

![](/img/corey_django/account_creation_message.png)

Finally, to update your database after a successful signup, just add `form.save()` to your views function.

![](/img/corey_django/new_user_added.png)

## Adding a new field to the user creation form
Notice that we are missing the email address field for our alicebob. Now we need to create a new field to add to our register template.

To resolve this, we create a custom registration form using Django's default form template. Essentially, this we make a child class from Django's `UserCreationForm` class. After that we need to replace the default form we used in the views function to our custom form class, which we call `UserRegisterForm`, which now includes an email field.

```python
# django_project/users/forms.py
from django import forms
from django.contrib.auth.models import User
from django.contrib.auth.forms import UserCreationForm

# Create a form that inherits from UserCreationForm

class UserRegisterForm(UserCreationForm):
    email = forms.EmailField(required=True)

    class Meta:
        # The model that is affected is the User model
        model = User
        # These are fields we want in the form, in this order
        fields = ['username', 'email', 'password1', 'password2']
```
Voila

![](img/corey_django/registration_form_w_email_field.png)

So let's try again, to add a new user, this time with an email address...

![](img/corey_django/new_user_w_email.png)

## Add bootstrap style to registration page to match our homepage style

Plus we want to add a highlight message to tell the user that they entered something wrong in one of the fields.

Let's try to do all our styling in our templates. C.S introduces a third-party Django framework called CrispyForms.

So we install it with `pip install django-crispy-forms`.

Add it to your settings file's `INSTALLED_APPS`:

```python
-snip-
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig',
    'users.apps.UsersConfig',
    'crispy_forms', # <- added crispy forms
]
-snip-

.
.
.
# Specify which CSS framework you wanna use, in our case, bootstrap 4
CRISPY_TEMPLATE_PACK = 'bootstrap4'
```

Then go back to your `register.html` template and load the Crispy Form in a code block and add a filter (denoted by `|`) on the `forms` variable.

```html
{% raw %}

{% extends "blog/base.html" %}
{% load crispy_forms_tags %}
{% block content %}
    <div class="content-section">
        <form method="POST">

-snip-
.
.
.
            <fieldset class="form-group">
                <legend class="border-bottom mb-4"> Join Today </legend>
                {{ form|crispy }}
            </fieldset>
-snip-

{% endraw %}
```

![](img/corey_django/registration_form_bootstrap5.png)

We can see that the both the spacing of the fields and the error messages look so much better.