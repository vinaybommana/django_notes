# Basics

The import order in a Django Project is :

* Standard library imports
* Imports from core Django
* Imports from third-party apps including those unrelated to Django
* Imports from the apps that you created as part of your Django Project

* Avoid using import *

## Create an Application

```bash
python3 manage.py startapp myapp
```

  myapp/
    __init__.py
    admin.py
    models.py
    tests.py
    views.py

* `__init__.py` - Just to make sure python handles this folder as a package.

* `admin.py` - This file helps you make the app modifiable in the admin interface.

* `models.py` - This is where all the applications are stored.

* `tests.py` - This is where your units tests are.

* `views.py` - This is where your application views are.

## views.py

* To call the view, we need to map it to a URL - and for this we need
  a URLconf.

* To create a URLconf in the polls directory, create a file called urls.py

* The next step is to point the root URLconf at the polls.urls module

* In mysite/urls.py, add an import for `django.conf.urls.include` and
  insert an `include()` in the urlpatterns list.

### When to use include()

* we should always use `include()` when you include other URL patterns
  `admin.site.urls` is the only exception to this.

* The `url()` function is passed four arguments, two required: `regex` and `view`
  and two optional : `kwargs` and `name`.

####`url()` argument : regex

* Note that these regular expressions do not search GET and POST parameters, or the
  domain name.
  https://www.example.com/myapp/ the URLconf will look for myapp/
  https://www.example.com/myapp/?page=3 the URLconf will also look for myapp/

####`url()` argument : view

When Django finds a regular expression match, Django calls the specified view 
function, with an HttpRequest object as the first argument and any "captured"
values from the regular expression as other arguments. If the regex uses simple
captures, values are passed as positional arguments; if it uses named captures, 
values are passed as key word arguments.

## settings.py

In `settings.py`, find the line that contains `TIME_ZONE` and modify
it to choose your own timezone.

```python
TIME_ZONE = 'Asia/Kolkata'
```

## setting up a database

* we need to change the default sqlite3 database to mysql
* go to `settings.py` and under DATABASES list

```python
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'todolist',
        'USER': 'root',
        'PASSWORD': '******', # you need to put the PASSWORD you have
                              # given to the mysql server
        'HOST': '',
        'PORT': ''
```

* In order to login to mysql
    `sudo mysql -u root -p`


## Differences between `__str__` and `__repr__`

* `__str__` is more readable than `__repr__` which gives the python object
  usually eval will convert it back to that object.
* __repr__ is a printable representation of the object. 
  This method calls `repr` builtin function.

## Django Models

A model is a class that represents table or collection in our Database,
and where every attribute of the class is a field of the table or collection.
Models are defined in the app/models.py

* Every model inherits from  `django.db.models.Model`

object.save()

* hard coding the values to the data base
* don't do this.

```python
todo1 = Todo(
  website = "www.polo.com",
  mail = "sorez@polo.com",
  name = "sorez",
  phone = "002376970"
)
todo1.save()
# if we want to delete an entry
todo_delete = Todo.objects.get(name = "sorez")
todo_delete.delete()
#Filtering data
queries = Todo.objects.filter(name="paul")
# Ordering results
results = Todo.objects.order_by("name")
```

### models.ForeignKey

* It is a link to another model

A many-to-one relationship. Requires two positional arguments: The class
to which the model is related and the on_delete option.

To create a recursive relationship - an object that has a many-to-one
relationship with itself - use

`models.ForeignKey('self', on_delete=models.CASCADE)`

* `ForeignKey` accepts other arguments that defines the details of how the 
  relation works.


## Django - URL Mapping

After creating view, we want to access that view via a URL.
Django has his own way for URL mapping and its done by editing the project
`url.py`

* url stands for Uniform Resource Locator

### What happens when someone requests a website from your server ?

When a request comes to a webserver, it's passed to Django which tries to
figure out what is actually requested. It takes a web page address first
and tries to figure out what to do. This part is done by Django's _urlresolver_
It is not very smart -- it takes a list of patterns and tries to match
the URL. Django checks patterns from top to bottom and if something is 
matched, then Django passes the request to the associated function 
(which is called *view*)


### Django views

A _view_ is a place where we put the "logic" of our application. It will request 
information from the `model` we've created before and pass it to a `template`.
* views are just python functions.
* views connects the models and templates

### Django ORM and Query Sets

#### QuerySet

A QuerySet is, in essence, a list of objects of a given Model
QuerySet allow you to read the data from the database, filter it and order it.

`python3 manage.py shell`

The effect should be like:
```python3
(InteractiveConsole)
>>>
>>> from blog.models import Post
>>> Post.objects.all()
<QuerySet []>
```

#### Creating an Object

```python3
>>> Post.objects.create(author=me, title='Sample title', text='Test')

>>> from django.contrib.auth.models import User
>>> User.objects.all()
<QuerySet [<User: ola>]>
>>> me = User.objects.get(username='ola')
>>> # now creating a post
>>> Post.objects.create(author=me, title='Sample title', text='Test')

>>> Post.objects.filter(title__contains='title')
>>> post = Post.objects.get(title="Sample title")
>>> post.publish()
>>> exit
```
