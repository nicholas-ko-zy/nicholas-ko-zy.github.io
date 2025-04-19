---
title: "Django Part 2: Bootstrap Templates"
layout: inner
post_link: 'nicholas-ko-zy.github.io/blog/django_templates.html'
publish_date: 2025-04-11
---

# Django Part 2: Bootstrap Templates

*This post uses material from Part 1 to 3 of Corey Schafer's Django tutorial. I'll only write about Part 3, since Part 1 and 2 are on setting up a Django project which I've covered that in my previous post.*

In this post, I document how to use Bootstrap templates to make Django look nicer than just plain HTML.

**Go from this to that**

![](/img/corey_django/before_after_templates.png)

*(I realise the text on right image is a bit small, but I already edited the web-app since I took the screenshot and I don't really want to rollback my repo.)*

## Creating a template for your app: `template.html`
These are the steps to create a folder to store your app's templates. In this post, when I refer to 'template' file, I mean the HTML file that the view function renders. And `blog` is just an app that Corey Schafer used in his tutorial, it can be any app in your Django project.

**Step 1**: Let's say your template file is called `template.html`, and your app is `blog`. Then you'll need a folder structure like this. 

You'll notice is a second `blog` subfolder. It sounds redundant, but apparently it's just part of the Django convention.

`blog` example:
```
django_app 
    └── blog 
        └── templates 
            └── blog 
                └── templates.html
```

Generally:
```
Your project home directory
    └── Your app folder
        └── App's template subfolder
            └── Subfolder named after app
                └── Template HTML file
```

**Step 2:** Make sure that your view function within `blog/views.py` renders the template file instead of giving a HttpResponse.

Remarks: The first argument of render is always request. Second argument is the relative filepath to the template html file.

![](/img/corey_django/render_function.png)


```python
# django_project/blog/views.py
def home(request):
    context = {'posts': Post.objects.all()}
    return render(request, template_name='blog/home.html', context=context)
```

## Add mock posts to our blog app

1. To do so create some dummy blog posts called posts.
   
2. Inside your view function, define `context` as a dictionary. 
   
3. Change your template file, i.e. `home.html`. Write a for loop to show posts.
Remark: Use {% raw %}{% `{% %}` %}{% endraw %} for for-loops and {% raw %} `{{}}` {% endraw%} for variables in a HTML file.

![](/img/corey_django/templates_for_loops_vars.png)

We can also use 'if/else' statements.

```html
{% raw %}
{% if title%}
    <title></title>
{% else %}
    <title></title>
{% endif%}
{% endraw %}
```

## Templates inheritance
Reduce repeated code for html.

We can create a base template, and replace the custom html sections with blocks. Then the child template can override the the parent base template. 

```html
{% raw %}
{% block content %} {% end block%}
{% endraw %}
```
Then in your child templates, remove everything that isn't unique to the template. 

![](/img/corey_django/home_about_template_compare.png)

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
{% raw %}
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
{% endraw %}
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

![](/img/corey_django/load_static.png)

This is what you see when you refresh the server.

![](/img/corey_django/django_blog_with_some_css.png)

Part 3 Timestamp 39:51, replace the article html with code snippet file of the same name. The new article html has more CSS classes to make the article looks nicer.

![](/img/corey_django/article_html.png)

![](/img/corey_django/article_with_css.png)

We want to abstract the href arguments in our `base.html` file so that we don't hard code
the links in the navigation bar.

Previously:
```html
<a class="nav-item nav-link" href="/">Home</a>
```

Now (making use of a code block): 
```html
{% raw %}
<a class="nav-item nav-link" href="{% url 'blog-home'%}">Home</a>
{% endraw %}
```

We use the url tag and the name of the route, which we chose to be 'blog-home'.

Recap on purpose of templates: Update your site in a single location (template inheritance), use URLS for link references, avoid hardcoding.

