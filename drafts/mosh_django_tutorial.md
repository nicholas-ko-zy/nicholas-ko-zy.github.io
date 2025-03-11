---
title: "Learning Django (Part 1 - Mosh's Tutorial)"
layout: inner
post_link: 'nicholas-ko-zy.github.io/blog/mosh_django_tutorial.html'
publish_date: 2025-03-08
---

<!-- omit in toc -->
# Learning about Django from Mosh

Django is a high-level web framework that makes it easy to create Python web applications. A web framework is a set of tools makes it easier to do web-development tasks. Tasks like structuring your project directory or creating user interfaces.

My goal is to eventually build a GIS web app using Django. Some ideas I've had for the web app are:

* A map that displays the schedule of what's on at my favourite music venues and cinemas. I don't like visiting 5 different websites to plan my weekend. Why not have one site that puts it all in one place? (Not that my weekends are *that* free now that I'm writing this blog...) 🙄

* A map to visualise solving the Capacitated Vehicle Routing Problem using real-world locations but without real-time traffic data. I'm thinking of getting the vertices and edges from OpenStreetMap.

There are many video tutorials promising to teach you Django in the least painful way. I browsed through a few before settling on [Mosh's tutorial](https://www.youtube.com/watch?v=rHux0gMZ3Eg). It's a great tutorial, but not complete, as I wrote in [the conclusion](#conclusion).

Here's a summary of what I learnt from Mosh's tutorial:

- [Starting a Django project](#starting-a-django-project)
- [Apps](#apps)
  - [Creating an app called `playground`](#creating-an-app-called-playground)
- [Views](#views)
- [Templates](#templates)
- [Debugging a Django app in VSCode](#debugging-a-django-app-in-vscode)
- [Django Debug Toolbar](#django-debug-toolbar)

## Starting a Django project

Assuming you've activated a virtual environment with Django installed, these are the steps to create a Django project from scratch. 

Let's suppose your project name is "storefront", because you're building an e-commerce site. [^1] Run this in your terminal to set up your project folder directory:

```
django-admin startproject storefront .
```
*You can replace `storefront` with any project name.*

Remember not to leave out the period, `.`, otherwise Django will create **another** parent folder like this:

```
.
└── storefront
    └── storefront
```
*Then you have a redundant parent folder named `storefront`.*


Inside the `storefront` folder, you'll get this default directory structure.

```
.
└── storefront
    ├── __init.py
    ├── asgi.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```

* `__init__.py`: Turns the directory into a package, so you can import it from elsewhere.

* `settings.py`: A config file for the entire website. Here's where you specify things like where you database is.

* `urls.py`: Defines what urls get used for what in your website.

* `asgi.py`/`wsgi.py`: For web deployment. Too complicated for me to understand now.

* Other files that are created:

    * `manage.py`: A wrapper for `django-admin`. It is used to manage your project with admin privileges. Here's an example of similar terminal commands used to deploy the website locally.
  
      ```
      django-admin runserver
      ```
        *^Doesn't look through your `settings.py`* 
      
      vs
        ```
        python manage.py runserver
        ```

        *^Looks through your `settings.py`* 


If you run `python manage.py runserver` in your terminal, you'll go to your development site. 

![Empty Django Site](/img/mosh_django/empty_django_site.png)
*Empty Django Site.*

## Apps

*Mosh (paraphrased): "Each Django app is just a collection of apps, each providing several functionalities."*

Django breaks down a large web-app into smaller apps (or Python modules) in an object oriented way. Going back to what I learnt about SOLID principles, I can apply the 'S' for 'Single-reponsibility principle' here. Say we still want to create an E-Commerce web app, then I should have one app to handles the responsibility for seperate tasks.

* **Customers:** Handles customers data.
  
* **Products:** Handles product data.

* **Cart:** Handles products that the customer selected.

Etc.

To view your apps, go into your settings module and look for `INSTALLED_APPS`. 

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

These are the default apps that appear when you create a Django project from scratch. We have apps for admin, authentication etc. 

### Creating an app called `playground`
Following the tutorial, I created a custom app (called "playground"). To do so, you need to run the following terminal command in a new terminal. There should be another terminal with the website running in the background.

```
python manage.py startapp playground
```

What it does is create a new folder with yet another standard Django structure. This folder has the same name as our app, "playground". After creating an app, you need to add your app to `INSTALLED_APPS` in your `settings.py` file.

*Before*
```python
# settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
```

*After*
```python
# settings.py
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Our newly added app
    'playground' # <-
]
```

We'll come back to the `playground` app after we define a Python function to return a response to the client.

## Views

*From django documentation: "A view function, or view for short, is a Python function that takes a web request and returns a web response."* More accurately, a View function is a [Request Handler](https://en.wikipedia.org/wiki/Request%E2%80%93response). For example, I want to...

```python
from django.shortcuts import render
from django.http import HttpResponse
# Create your views here.

def say_hello(request):
    """
    This is a 'View' function that can do anything you want
    it to do as long as it's properly defined. For example,
    you can write a function to
    * Pull data from a database
    * Show an image
    * Send email response
    For now we will get it to just say "Hello".
    """
    return HttpResponse('Hello World')
    
```


To map this url to your 'View' function, we add a file called `url.py` in the playground folder. (Remark: You can allthe file anything, the name doesn't matter.)

After writing your URL map in our custom app `playground`, we want to add that to the overall URL config in `storefront`. Inside
`storefront/urls.py`, we see this helpful comment at the top.

```python
"""
--cut--
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
--cut--
"""
```
So we follow the instructions given in the comments.

*Excerpt from `storefront/urls.py`*
```python
urlpatterns = [
    path('admin/', admin.site.urls),
    path('playground/', include('playground.urls'))

```

*Excerpt from `playground/urls.py`*
```python
urlpatterns = [
    # NOTE: Always end route with a forward slash.
    path(route='hello/', view=views.say_hello)
]
```

![Hello World URL](/img/mosh_django/url_routing_hello.png)


## Templates

Mosh (paraphrased): "Views in Django are not really views, they are more like request handlers. What you often call a 'view' in other frameworks is called a 'template' in Django."

Templates are used to return html content to the client.

Step 1: Create a new folder called `templates` in your playground folder. 

Step 2: Create a template file, name it whatever you want. In Mosh's example, he names it `hello.html`. 

Step 3: Now that we have a template, we want to display our response (the output of the view function) to the client in a custom html template. For example, we want the 'View' function to return 'Hello World' using within `<h1>` tags. And we don't want to hard code this inside our view function, but instead customise it in our template `hello.html`, which again, resides in our `templates` folder. 

We have to make some tweaks to the view function, since we no longer want to print plain text but render it through the HTML template of our choice. Before we had the 'View' function return a HTTP response `HttpResponse('Hello World')` but now we want to return a **render** of the HTML template. 

*Before*
```python
def say_hello(request):
    return HttpResponse('Hello World')
```

*After*
```python
def say_hello(request):
    return render(request=request, template_name='hello.html')
```

`hello.html`:
```html
<h1>Hello World</h1>
```

*Me to myself: "capeesh?"*

![Hello World rendered through the hello.html template](/img/mosh_django/hello_world_h1_template.png)

Now we can change the html file to also output variables, not just hard-coded plain text. To do so:

Step 1: Pass in an argument to the `context` parameter in your 'View' function `say_hello`.

```python
def say_hello(request):
    return render(request=request, template_name='hello.html',
                  context={'name':'Nicholas'})
```

Step 2: Edit your html file to include the variable name, which is the key value of the dictionary. In our case, `'name'`. The HTML will render the value, `'Nicholas'`.

```html
<h1>Hello {{ name }}</h1>
```

![Hello Nicholas](/img/mosh_django/hello_nicholas.png)

## Debugging a Django app in VSCode

Step 1: In your IDE, click the 'Run & Debug' button, create a `launch.json` file.

Add breakpoint, run debugger. Invoke breakpoint by going to playground/hello url.

You can click 'Step Over' (shortcut F10) which will run the breakpoint line and go to the next one. You'll have a local variable `x` now. 'Step Over' runs the code line-by-line.

![](/img/mosh_django/step_over_gives_local_var_x_1.png)

You also have 'Step Into', which allows you to inspect what is going on within a function, in case there are any bugs. The difference being that 'Step Over' will evaluate the functio and return the output but 'Step Into' will go inside the function.

![](/img/mosh_django/step_into.gif)

You can also 'Step Out' which returns you back to the function call line you were previously at. 

Remember to remove your breakpoints after you are done debugging.

Tip: You can use CTRL + F5 to run the project instead of running the `python manage.py runserver` command in your terminal.

## Django Debug Toolbar

Step 1: After you've pip-installed `django-debug-toolbar`, add it to your installed apps.

Step 2: Add a new urlpattern inside the main URLconf module.

Step 3: Add middleware

Step 4: Add IP address to your `settings.py` file. It is not created by default, just copy and paste this anywhere in the file.

```python
INTERNAL_IPS = [
    # ...
    "127.0.0.1",
    # ...
]
```

^ [All steps found on django-debug-tool-bar documentation site.](https://django-debug-toolbar.readthedocs.io/en/latest/installation.html)


<!-- omit in toc -->
## Conclusion

Ok...so at this point I realised that the 

<!-- omit in toc -->
# Footnotes

[^1]: [Unless you're time-travelling Paul Graham from 1995, in which case you're already set using Lisp.](https://www.paulgraham.com/first.html)