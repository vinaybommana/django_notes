# Basics

The import order in a Django Project is :

* Standard library imports
* Imports from core Django
* Imports from third-party apps including those unrelated to Django
* Imports from the apps that you created as part of your Django Project

* Avoid using import *

## Create an Application

```python
python3 manage.py startapp myapp
```

  myapp/
    __init__.py
    admin.py
    models.py
    tests.py
    views.py

* __init__.py - Just to make sure python handles this folder as a package.

* admin.py - This file helps you make the app modifiable in the admin interface.

* models.py - This is where all the applications are stored.

* tests.py - This is where your units tests are.

* views.py - This is where your application views are.

## polls/views.py

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


## setting up a database

* In order to login to mysql
    `sudo mysql -u root -p`


## Differences between __str__ and __repr__

* __str__ is more readable than __repr__ which gives the python object
  usually eval will convert it back to that object.
* __repr__ is a printable representation of the object. 
  This method calls `repr` builtin function.

## Django Models

A model is a class that represents table or collection in our Database,
and where every attribute of the class is a field of the table or collection.
Models are defined in the app/models.py

* Every model inherits from  `django.db.models.Model`

## Django - URL Mapping

After creating view, we want to access that view via a URL.
Django has his own way for URL mapping and its done by editing the project
`url.py`