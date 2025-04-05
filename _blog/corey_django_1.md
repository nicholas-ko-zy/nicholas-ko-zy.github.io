---
title: "Learning Django (Part 2 - )"
layout: inner
post_link: 'nicholas-ko-zy.github.io/blog/mosh_django_tutorial.html'
publish_date: 2025-03-08
---

# Part 1
Goal is to create a blog style website, with an authentication system.

## Creating a new project
See Mosh tutorial notes

# Part 2 Application and Routes

## Create new app
See Mosh tutorial ntoes

Inside your `view.py` import http response. 

```python
from django.http import HttpResponse
```

Create a view function that returns home page. 

```python
# blog/views.py
def home(request):
    return HttpResponse('<h1> Blog Home </h1>')
```

Map a url to your view function
* Create `url.py` file inside `blog` app. Add the following lines of code inside. 

```python
from django.urls import path
from .views import home

urlpatterns = [
    # Empty path - Home page
    path('',views.home, name='blog-home'),
]
```

Inside the project `url.py` file, import include method as well, which is from django.url

```python
# django_project/urls.py
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('blog/', include('blog.urls'))
]

```
When the user navigates to some URL, the code will look through the `urlpatterns` list inside `django_project/urls.py`. Checks if there is a pattern that matches that. Then it asks, where next should I send people if they are at this URL. For example if you land on `blog/`, it will look within the `url.py` file of that particular app. For example

```python
"""
Whenever Django sees this include function, it will chop off
whatever URL it has seen up to that point, and send the 
remaining string into the urls module for futher processing. 

In this case, the user goes to blog url, but when you chop that off
there is nothing reminaing so it's just an empty string. 
"""
include('blog.urls'),
```

Which in our case an empty string for the blog's urlpatterns returns the `home` function in our `blog/views.py` file.

```python
urlpatterns = [
    # Empty path - Home page
    path('', views.home, name='blog-home'),
]
```

Summarised: User nagivates to URL, sent to project URLpatterns, sent to app url patterns, sent to the correct view function to handle the request.

# Part 3 Templates

## Creating templates
For each app, Django will look for a templates subdirectory by default. 

Create another subfolder within `blog/templates` named after the app name `blog`. Corey (paraphrase mine): "This sounds redundant, but it's just part of the Django convention."

blog -> templates -> blog -> templates.html

Corey says to add the `BlogConfig` within `blog/apps.py` inside you `INSTALLED_APPS` list inside the `settings.py` file of your project folder. As such

```python
# settings.py

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'blog.apps.BlogConfig', # <- added new app
]
```

Now, in your view function file `views.py`, change the return of your view function to render the template html file instead of the HttpResponse.

Remarks: The first argument of render is always request. Second is the filepath to the template html file.

![](img/render_function.png)

Adding dummy data into our views function, i.e. fake posts.

1. To do so create some dummy blog posts called posts.
2. Use the context parameter, enter in a dictionary defined within your view function.
3. Change your template file, i.e. `home.html`. Write a for loop to show posts.
Remark: Use `{% %}` for for-loops, `{{}}` for variables in a HTML file.

![](img/templates_for_loops_vars.png)

4. We can also use 'if/else' statements.

```html
{% if title%}
    <title></title>
{% else %}
    <title></title>
{% endif%}
```

## Templates inheritance
Reduce repeated code for html.

We can create a base template, and replace the custom html sections with blocks. Then the child template can override the the parent base template. 

```html
{% block content %} {% end block%}
```
Then in your child templates, remove everything that isn't unique to the template. 

![](img/home_about_template_compare.png)

*New `home.html`*

```html
{% raw %}
{% extends "blog/base.html" %}
{% block content %}
    {% for post in posts %}
    {% comment %} print post info, one at a time {% endcomment %}
        <h1>{{  post.title  }}</h1>
        <p> By {{ post.author }} on {{post.date_posted}}</p>
        <p> {{post.content}} </p>
    {% endfor %}
{% endblock content %}
{% endraw %}
```

*New `about.html`* 

```html
{% raw %}
    {% extends "blog/base.html" %}
    {% block content %}
        <h1>About Page</h1>
    {% endblock content %}
{% endraw %}
```

Summary: Template inheritance makes it easier to apply changes across multiple pages in the site. For example, if you wanna use "bootstrap"...Look into it.

## Bootstrap

[Bootstrap starter template](https://getbootstrap.com/docs/4.0/getting-started/introduction/#starter-template)

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/popper.js@1.12.9/dist/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
  </body>
</html>
```

^From the bootstrap boilerplate above, we extract the code with the comments "Bootstrap CSS"  and "Optional Javascript".

```html
<!DOCTYPE html>
<html>
    <head>
        <!-- Required meta tags -->
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

        <!-- Bootstrap CSS -->
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">

        {% if title%}
        <title>Django Blog - {{ title }} </title>
        {% else %}
            <title>Django Blog</title>
        {% endif%}
    </head>
    <body>
        <div class="container">
            {% block content %} {% endblock %}
        </div>
        <!-- Optional JavaScript -->
        <!-- jQuery first, then Popper.js, then Bootstrap JS -->
        <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
        <script src="https://cdn.jsdelivr.net/npm/popper.js@1.12.9/dist/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
        <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.0.0/dist/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
    </body>
</html>
```
Adding the container class gives some padding to the blog post.

Key point here is about template inheritance, to make scalable code and to add some CSS to make template look nicer, not just base html.

Part 3: Templates; Timestamp 33:07

Copy code from Corey's repo, in a file called `navigation.html`. We are trying to add in some navigation features to the website. 

Right as he was explaining how to add CSS to our website, he begins to talk about creating a new directory to store CSS. Usually the folder is called `static`. 

* Create a new folder in the `blog` directory and create a new folder `static`.
* Create another folder inside your `static` folder, name it after the app.
  * ie. Your folder structure: blog -> static -> blog 
* Create a new file called `main.css` inside the lowest subfolder. Copy and paste the code from the snippets folder, there's a file with the same name `main.css`.
* Insert a code block at the top of your `base.html` file to load your custom css file.

![](img/load_static.png)

This is what you see when you refresh the server.

![](img/django_blog_with_some_css.png)

Part 3 Timestamp 39:51, replace the article html with code snippet file of the same name. The new article html has more CSS classes to make the article looks nicer.

![](img/article_html.png)

![](img/article_with_css.png)

We want to abstract the href arguments in our `base.html` file so that we don't hard code
the links in the navigation bar.

Previously:
```html
<a class="nav-item nav-link" href="/">Home</a>
```

Now (making use of a code block): 
```html
<a class="nav-item nav-link" href="{% url 'blog-home'%}">Home</a>
```

We use the url tag and the name of the route, which we chose to be 'blog-home'.

Recap on purpose of templates: Update your site in a single location (template inheritance), use URLS for link references, avoid hardcoding.

## Part 4 Admin Page

* Create new super user to login in as admin to your site.
* Create database (database migration, apply changes to database, for us, will create an auth_user table)
* Run `python manage.py makemigrations`
* Run `python manage.py migrate`
* Open terminal and enter the following command:
* `python manage.py createsuperuser`

## Part 5 Database and Migrations

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

![](img/sql_migrate_initial.png)

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

(Part 5 Stopped at 27:58)