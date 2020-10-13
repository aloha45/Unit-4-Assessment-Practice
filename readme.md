![Token Survivor Gif](https://i.imgur.com/b2kgM3y.gif)

# Unit 4 Assessment Practice

1. Create a postgres database:
    `createdb todo`

2. Create a new django project: 
    `django-admin startproject todos`

3. Create a main_app:
   `mkdir main_app`
   or `python3 manage.py startapp main_app`

4. In `settings.py`, insert `'main_app'` into `INSTALLED_APPS`
   
5. Also in `settings.py`, set up database:

    `DATABASES = {`
    `'default': {`
        `'ENGINE': 'django.db.backends.postgresql',`
        `'NAME': 'todo',`
    `}`
`}`

6. Make a model:
    `import django.db import models`

    `class Todo(models.Model):`
        `name = models.CharField(max_length=100)`

7. Migrate the model:
    `python3 manage.py makemigrations`
    `python3 manage.py migrate`

8. Add a path in `urls.py` in todos, be sure to import as well:
    `from django.urls import path, includes`

    `path('', include('main_app.urls')`

9.  Add `urls.py` in main_app directory

    `touch main_app/urls.py`

10. In `main_app/urls.py` add:

    `from django.urls import path`
    `from . import views`

    `urlpatterns = [`
        `path('/', views.home.as_view(), name='home'),`
    `]`

11. Off to views! `views.py`

    `from django.shortcuts import render`
    `from django.views.generic import ListView`
    `from django.views.generic.edit import CreateView, DeleteView`
    `from .models import Todo`

    `class Home(ListView):`
        `model = Todo`

12. Within `main_app`, `mkdir templates`
13. Within `main_app/templates` create `mkdir main_app`
14. Within `main_app/templates/main_app`, `touch todo_list.html`

15. In `todo_list.html`, add an `<h1>` tag to test. (it works!)

16. In `main_app/urls.py`, add a path to create a todo:

    `path('todo/create/), views.CreateTodo.as_view(), ``name='create_todo')`

17. In `main_app/views.py`:

    `class CreateTodo(CreateView):`
        `model = Todo`
        `fields = ['name']`

18. Add UI to the create path in the html:

    `<a href="{ url 'create_todo' %}">Create Todo</a>`

19. Touch main_app/template/todo_form.html, and add a form:

    `<form action="" method="POST">`
    `<!-- Django requires the following for security purposes -->`
    `{% csrf_token %}`
    `<table>`
      `<!-- Render the inputs inside of <tr>s & <td>s -->`
      `{{ form.as_table }}`
    `</table>`
    `<input type="submit" value="Submit!" class="btn">`
    `</form>`


20. In `models.py`, update Todo model to include:
    
    `from django.urls import reverse`

    `def get_absolute_url(self):`
        `return reverse('home')`

21. Enter a todo on the page.

22. Add the following code to `todo_list.html` to render:

    `<h1>Todo List</h1>`
    `{% if todo_list %}`
    `<ul>`
    `{% for todo in todo_list %}`
    `<li>`
        `{{todo.name}}`
    `</li>`
    `{% endfor %}`
    `{% else %}`
    `<h3>There are no todos!</h3>`
    `</ul>`
    `{% endif %}`

23. In `views.py`:

    `class DeleteTodo(DeleteView):`
        `model = Todo`
        `success_url = ' /'`

    or

    `def delete(request, id):`
        `Todo.objects.get(id=id).delete()`
        `return redirect('/')`

24. Add a urlpattern for deletion:

    `path('todo/<int:pk>/delete/', views.DeleteTodo.asview(),name='delete_todo')`

25. Update UI to include path for deletion:

    `<ul>`
    `{% for todo in todo_list %}`
    `<li>`
        `{{todo.name}}`
    `</li>`
    `<a href="{% url 'delete_todo' todo.id %}">`

26. Touch `main_app/templates/main_app/todo_confirm.html`, add boilerplate, and the following code:

    `<h4>Are you sure you want to delete <span class="teal-text">{{ object.name }}</span>?</h4>`
  `<form action="" method="POST">`
    `{% csrf_token %}`
    `<input type="submit" value="Yes - Delete!" class="btn">`
    `<a href="{% url 'home' %}">Cancel</a>`
    `</form>`

27. Profit.