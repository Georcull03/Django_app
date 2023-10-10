# Django_app

This is a Django portfolio app. Django documentation can be found [here](https://www.djangoproject.com/).

# Django

Django is a high level Python web framework that encourages rapid development and clean, pragmatic design. Built by
experienced developers, it takes care of much of the hassle of web development, so you can focus on writing your app
without needing to reinvent the wheel. It’s free and open source.

Django apps are structured so that there’s a separation of logic. It supports the model-view-controller pattern,
which is the architecture for most web frameworks. The basic principle is that each application includes three separate
files that handle the three main pieces of logic separately:

- **Model** defines the data structure. This is usually the database description and often the base layer to an
  application.
- **View** displays some or all of the data to the user with HTML and CSS.
- **Controller** handles how the database and the view interact.

# Set up dev environment

Create a directory for your project to live.

```
mkdir <project_name>
cd <project_name>
```

Once inside your project directory it is a good idea to set up a virtual environment.

```
python -m venv venv
```

This command will create a venv folder in your working directory. Inside this directory, you’ll find several files,
including a copy of the Python standard library. Later, when you install new dependencies, they’ll also live in this
directory. Next, you need to activate the virtual environment by running the following command:

```
source venv/bin/activate
```

Now that you’ve created and activated a virtual environment, it’s time to install Django. You can do this using pip:

```
python -m pip install Django 
```

# Start Django project

Once inside your project directory and the virtual environment is activated, run the following command:

```
django-admin startproject <name_of_project> .
```

If you run the above command without the . then it will create the directory with a subdirectory following the same
name. Once the ```startproject``` command has been run your project structure should look like this:

```
<project_name>/
│
├── name_of_project/
│   ├── __init__.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
│
├── venv/
│
└── manage.py
```

Now that your file structure is set up you can run a Django development server and check that your setup has been
successful. The following command runs the development server:

```
python manage.py runserver
```

Then in your browser go to [localhost](http://localhost:8000).

# Add an app

This next section will create an app containing basic welcome page.

To create the app run the following command in the root project directory:

```
python manage.py startapp <app_name>
```

This command will create an <app_name> directory with several files:

- ```__init__.py``` tells Python to treat the directory as a Python package.
- ```admin.py``` contains settings for the Django admin pages.
- ```apps.py``` contains settings for the application configuration.
- ```models.py``` contains a series of classes that Django’s ORM converts to database tables.
- ```tests.py``` contains test classes.
- ```views.py``` contains functions and classes that handle what data is displayed in the HTML templates.

Once you’ve created the app, you need to install it in your project.
In ```<project_name>/<name_of_project>/settings.py``` underneath the section called ```INSTALLED_APPS``` in a similar
format to this: "app_name.apps.App_nameConfig"

# Creating a view

For information on creating a view refer [here](https://www.tutorialspoint.com/django/django_creating_views.htm).

Once you have created a "view function"  you then need to create the html templates. You create them within a directory
like so,:

```
mkdir -p app_name/templates/app_name
touch app_name/templates/app_name/home.html
```

Then add some html code.

# Add a route

You need to hook up an URL so that you can visit the page that you’ve just created. Your name_of_project/ directory
contains a file named urls.py. In this file, you’ll include a URL configuration for the app. Info on adding routes
can be found [here](https://www.educative.io/answers/how-to-perform-url-routing-in-django).

# Add a bootstrap to your app

Create a directory named templates/ in the project_name/ folder. Inside this new directory,
create a file named base.html:

```
mkdir templates/
touch templates/base.html
```

Instead of having to import Bootstrap styles into every app, you can create a template or set of templates that all the
apps share. As long as Django knows to look for templates in this new shared directory, it can save a lot of repeated
styles.

Before you can see the inheritance and the new styled application in action, you need to tell your Django example
project that base.html exists. The default settings register templates/ directories in each app, but not in the root
directory itself. In name_of_project/settings.py, you need to update TEMPLATES like so:

```
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [
            BASE_DIR / "templates/",
        ],
        ...
```

Whenever you want create templates or import scripts that you intend to use in all of your Django apps inside a project,
you can add them to this project-level directory and extend them inside your app templates.

# Add a projects app

First you will run the command to create a directory called projects within the project_name directory.

```
python manage.py startapp projects
```

Then you need to install it to your project in the INSTALLED_APPS section in project_name/name_of_project/settings.py.

## Define a model

If you want to store data to display on a website, then you’ll need a database. Typically, if you want to create a
database with tables that contain columns, you’ll need to use SQL to manage the database. But when you use Django,
you don’t need to learn a new language because it has a built-in object relational mapper (ORM).

An ORM is a program that allows you to create classes that correspond to database tables. Class attributes correspond
to columns, and instances of the classes correspond to rows in the database. So, instead of learning a whole new
language to create your database and its tables, you can just write some Python classes. When using an ORM the classes
that represent database tables are referred to as models.

For example, say you create a model called Project to store projects that you have worked on it would look like this:

- title - String field that holds the name of a project that you worked on.
- description - String field that holds a descriptive piece of text regarding projects that you have worked on
- technology - String field thats contents will be limited to a select number of choices.

```
# projects/models.py

from django.db import models

class Project(models.Model):
    title = models.CharField(max_length=100)
    description = models.TextField()
    technology = models.CharField(max_length=20)
```

A list of built-in model types can be found [here](https://docs.djangoproject.com/en/4.2/ref/models/fields/).

To start the process of creating your database, you need to create a migration. A migration is a file containing a
Migration class with rules that tell Django what changes need you’re making to the database. To create the migration,
type the following command in the console, making sure that you’re in the project_name/ directory:

```
python manage.py makemigrations projects
Migrations for 'projects':
  projects/migrations/0001_initial.py
    - Create model Project
```

You should see that you’ve created migrations/ inside projects/, and it includes a file named 0001_initial.py. This 
file contains the instructions that Django should perform on the database.

Now to apply the migrations from in the migration file and create the database, you use the following command:

```
python manage.py migrate projects
Operations to perform:
  Apply all migrations: projects
Running migrations:
  Applying projects.0001_initial... OK
```

If you run ```makemigrations``` and ```migrate``` without the projects flag, then all the migrations for all the 
default models in your Django projects will be created and applied because Django comes with several models already.

## Using the Django shell

To add new entries to your Project database table, you need to create instances of your Project class. 
For this, you’re going to use the Django shell. To enter the Django shell enter the following command:

```
python manage.py shell
```

You can tell when you have entered the Django shell because the command prompt will become three carets (>>>) as 
opposed to a dollar sign ($). First you will need to import your model like so:

```
from projects.models import Project
```

Then create an instance like so:

```
first_project = Project(
     title="My First Project",
     description="A web development project.",
     technology="Django",
)
first_project.save()
```

After creating first_project as a new Project class instance, you need to run the .save() method. This creates a new 
entry in your projects table and saves it to the database.

Once your database is created you can run queries against it through the shell like so:

```
>>> from projects.models import Project

>>> Project.objects.all()
<QuerySet [<Project: Project object (1)>

project = Project.objects.get(pk=1)
>>> project.title
'My First Project'

>>> project.description
'A web development project.'
```

You can also run queries when creating your views for your model.

To exit the Django shell run the ```exit()``` method.

# Accessing Django admin site

In order to access the Django admin site you need to create an admin user. The admin site will allow you to manage and 
edit your existing projects as well as create new projects to be displayed on the projects page.

```
python manage.py createsuperuser
Username (leave blank to use 'root'): admin
Email address: admin@example.com
Password: 
Password (again): 
Superuser created successfully.
```


