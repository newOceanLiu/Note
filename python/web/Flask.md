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

  ​



