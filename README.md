### Task 1 - Configuration init

- Create `.env` config file under the root directory in the project folder
- Add configuration environment variables
  - `FLASK_APP = 'wsgi.py'`
  - `FLASK_ENV = 'Development'`
  - `SECRET_KEY = '...'`
  - `TESTING = False`

### Task2 - Configure Flask app object

- Add `Config` in `people_web_app/__init__.py `
  - Add `test_config` argument to `create_app()` method, default to `None`
  - Add `app.config.from_object('config.Config')`

### Task3 - Construct a simple home page

- Add `jinja2` to PyCharm `template language` setting tab

- Add `home.html` to `templates` folder

- Add below information into `home.html` as a simple home page

  ```jinja2
  {% extends 'layout.html' %}
  
  {% block content %}
  
      <p>Welcome to my page!</p>
  
  {% endblock %}
  ```

### Task4 - Construct a template for displaying a single person

- Add `list_person.html` under `templates` folder

- Add below code chunk to the `list_person.html` template

  ```jinja2
  {% extends 'layout.html' %}
  
  {% block content %}
  
      {% if person %}
          <p>First name: {{ person.firstname }}, Last name: {{ person.lastname }}, ID: {{ person.id_number }}</p>
  
      {% else %}
          <p>There is no matching person</p>
      
      {% endif %} 
  
  {% endblock %}
  ```

### Task5 - Register the blueprint

- To register the blueprint, add below code to `__init__.py` file in the repo root directory

  ```python
  with app.app_context():
  	from .people_blueprint import people
  	app.register_blueprint(people.people_blueprint)
  ```

- The above code register the `people_bluprint` in the `app` object
