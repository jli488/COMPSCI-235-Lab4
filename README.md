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

### Task6 - Implement the view method `list_people()`

- In the view method of `list_people()` under `people` blueprint, added `render_template` for `list_people.html` page

  ```python
  @people_blueprint.route('/list')
  def list_people():
      return render_template(
          'list_people.html',
          people=repo.repo_instance,
          find_person_url=url_for('people_bp.find_person'),
          list_people_url=url_for('people_bp.list_people')
      )
  ```

### Task7 - Prepare to use WTForms

- Add `WTF_CSRF_SECRET_KEY` into the `.env` file under the root directory

### Task8 - Make a WTForm class

- `SearchForm` inherits from `FlaskForm`
- Two arguments for `IntegerField` are `label` and `validators`
- Place below code in `person.py`
  ```python
  class SearchForm(FlaskForm):
      person_id = IntegerField(label='Person ID', validators=[DataRequired(message='Person ID is required')])
      submit = SubmitField(label='Submit')
  ```

### Task9 - Construct a template for search form

- Generate the template for `find_person` as below:

  ```jinja2
  {% extends 'layout.html' %}
  
  {% block content %}
  
  <main>
      <div>
          <form method="POST" action="{{ handler_url }}">
              {{ form.csrf_token }}
              <div>
                  {{ form.person_id.label }} {{ form.person_id }}
              </div>
              {{ form.submit }}
          </form>
      </div>
  </main>
  
  {% endblock %}
  ```



### Task10 - Implement `find_person()` method

- Firstly need to instantiate a form object: `form = SearchForm()`

- In case of a `GET` request or validation failed, we return the form as below:

  ```python
  return render_template(
      'find_person.html',
      form=form,
      list_people_url=url_for('people_bp.list_people'),
      find_person_url=url_for('people_bp.find_person')
  )
  ```

- If the validation succeeded, it means it was a `POST` request and the validation is successful, then we search the people and render the template as below:

  ```python
  person_id = form.person_id.data
  person = repo.repo_instance.get_person(person_id)
  return render_template(
      'list_person.html',
      person=person,
      list_people_url=url_for('people_bp.list_people'),
      find_person_url=url_for('people_bp.find_person')
  )
  ```