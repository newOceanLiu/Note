# Flask

## Requst-Response Cycle

### Context

- application context
  - current_app: application instance
  - g: temporary storage usring the handling of the request, reset for each request
- request context
  - request: content of a http request from the client
  - session: the user session, a dictionary that application can store values  between requests

### Request Dispatching

* use route to find the right view function to call, to see the full url map:

  ```python
  from hello import app
  app.url_map
  ```

### Request Hooks

* sometimes it is useful to execute code before and after each request is processed, Flask provid 4 hooks
  * `before_first_request`: before first request is handled
  * `before_request`: before each request
  * `after_request`: run after each request, if no unhandled exceptions
  * `teardown_request`: run after each request, even if unhandled exception occurred
* a common pattern to share data between hook functions and view functions is to use `g` context global
  * eg: a `before_request` handler can load logged-in user from the database and store in `g.user` and when view function is invoked, it can access the user from there

### Responses

* view function can return a couple for response
  * first one is the message
  * second one is the status code
  * third is a list of headers
* Reponse can also be a `Response` object
  * `make_response()` function take 1/2/3 arguments
* view function can also returns a `redirect` function with link
* it can also issue `abort` function, which can be used for error handling
  * *Note*: `abort`  function does not return control back to the fucntions that calls it, but give it back to the web service that raise the exception

## Flask Extensions

* flask extensions are under flask.ext namespace

### Flask-script

* pass startup configuration options from command line

* `(venv) $ pip install flask-script`

* ```python
  from flask.ext.script import Manager
  manager = Manager(app)
  # ...
  if __name__ == '__main__':
  manager.run()
  ```


### Flask-Bootstrap

* integration for twitter bootstrap, using Jinja2's template inheritance

  ```python
  from flask.ext.bootstrap import Bootstrap
  # ...
  bootstrap = Bootstrap(app)
  ```

* base template blocks, *don't overwrite since Flask-Bootstrap is using them it self*, use super() for adding new content on top

  * doc
  * html_attribs
  * html
  * head
  * title
  * metas
  * styles
  * body_attribs
  * navbar
  * content
  * scripts

### Flask-Moment

* provide localization for date and time

### Flask-WTF

* handling web form

### Flask-SQLAlchemy

* powerful relational database framework that support several database backedn
* the url of the application is configured as `SQLALCHEMY_DATABASE_URL` 
* set `SQLALCHEMY_COMMIT_ON_TEARDOWN` to true to enable automatic commits of database changes at the end of each request

## Template

### Jinja2

* Filters: use pipe to modify/filter the variable `{{ name|capitalize }}`

  * safe, capitalize, lower, upper, title, trim, striptags
  * safe should not be used for user input data

* Macro: similar to python function

  ```html
  {% macro render_comment(comment) %}
  <li>{{ comment }}</li>
  {% endmacro %}
  <ul>
  {% for comment in comments %}
  {{ render_comment(comment) }}
  {% endfor %}
  </ul>
  ```

  * can store macro files in stand alone files, and import them from template

    ```html
    {% import 'macros.html' as macros %}
    <ul>
    {% for comment in comments %}
    {{ macros.render_comment(comment) }}
    {% endfor %}
    </ul>
    ```

  * macro files also support inheritance like python

    * use `block` to define elements that a derived template can change

      * base

        ```html
        <html>
        <head>
        {% block head %}
        <title>{% block title %}{% endblock %} - My Application</title>
        {% endblock %}
        </head>
        <body>
        {% block body %}
        {% endblock %}
        </body>
        </html>
        ```

      * child

        ```html
        {% extends "base.html" %}
        {% block title %}Index{% endblock %}
        {% block head %}
        {{ super() }}
        <style>
        </style>
        {% endblock %}
        {% block body %}
        <h1>Hello, World!</h1>
        {% endblock %}
        ```

### Static file

* use `url_for()` for dynamic url, as well as static file 

  ```html
  {% block head %}
  {{ super() }}
  <link rel="shortcut icon" href="{{ url_for('static', filename = 'favicon.ico') }}"
  type="image/x-icon">
  <link rel="icon" href="{{ url_for('static', filename = 'favicon.ico') }}"
  type="image/x-icon">
  {% endblock %
  ```

  ​

## Web Forms

### CSRF Protection

* `app.config` is the general-purpose place to store config, can be used to put CSRF token `SECRET_KEY`


### Redirects and user session

* if the last action was a post, when user refresh the page, the broswer would try to resend that post action, which is undesirable. 
  * so it is a good practice to never leave a post request as the last request sent by the browswer
  * this practice can be achieved by responding to a post request with a redirect instead of normal response
  * *Post/Redirect/Get pattern*
  * with this approach, we need the application to remember the form data from one request to the next, we can store them in user session
  * by defuat, user session would store in client-side cookies that are signed using `SECRET_KEY`

## Large Application Structure

* Flask does not enforce any project structure, the choice is up to the developer, here is one of the example structure

|-[your app name]

​	|-app

​		|- template

​		|- static

​		|-main

​	|-tests

​	|-venv

​	|-requirements.txt

​	|-config.py

​	|-manage.py

### Configuration options

* since each application can have several config set, it is easier to have a hierarchy of configuration classes where dev/test/prod config can based of a base config

### Applicatoin package

* we want to delay the creation of application since we want to apply configuration changes dynamically, thus we move it into a `factory function` that can be explicitily invoke from the script. 
* `factory function` can give script time to set configuration and also create multiple application instances, which is very useful for testing

### Blueprint

* `app.route` works when application instance exists in the global scope, but when using the `create_app()` factory function, it doesn't work since it would not exist after the `create_app()` is invoked, which is too late, that's why we need *blueprint*
* routes associated with blueprint are in a dormant state untile the blueprint is registered with an application
* for application that apply blueprint, there are two difference in code
  * the route decorator comes from blueprint
  * the parameter for `url_for()` will be within a namespace

### 

