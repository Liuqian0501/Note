# Part 14: Redirect & Errors

Flask class has a `redirect()` function. When called, it returns a response object and redirects the user to another target location with specified status code.

Prototype of redirect() function is as below −
```
Flask.redirect(location, statuscode, response)
```
In the above function −

- `location` parameter is the URL where response should be redirected.

- `statuscode` sent to browser’s header, defaults to 302.

- `response` parameter is used to instantiate response.

The following status codes are standardized −
```
HTTP_300_MULTIPLE_CHOICES
HTTP_301_MOVED_PERMANENTLY
HTTP_302_FOUND
HTTP_303_SEE_OTHER
HTTP_304_NOT_MODIFIED
HTTP_305_USE_PROXY
HTTP_306_RESERVED
HTTP_307_TEMPORARY_REDIRECT
```
The `default status` code is `302`, which is for `‘found’`.

In the following example, the `redirect()` function is used to display the `login` page again when a `login` attempt fails.

```python
from flask import Flask, redirect, url_for, render_template, request
# Initialize the Flask application
app = Flask(__name__)

@app.route('/')
def index():
   return render_template('log_in.html')

@app.route('/login',methods = ['POST', 'GET'])
def login():
   if request.method == 'POST' and
   request.form['username'] == 'admin' :
   return redirect(url_for('success'))
   return redirect(url_for('index'))

@app.route('/success')
def success():
   return 'logged in successfully'
	
if __name__ == '__main__':
   app.run(debug = True)
```
Flask class has abort() function with an error code.

```python
Flask.abort(code)
```
The Code parameter takes one of following values −

- 400 − for Bad Request

- 401 − for Unauthenticated

- 403 − for Forbidden

- 404 − for Not Found

- 406 − for Not Acceptable

- 415 − for Unsupported Media Type

- 429 − Too Many Requests

Let us make a slight change in the `login()` function in the above code. Instead of re-displaying the login page, if `‘Unauthourized’` page is to be displayed, replace it with call to `abort(401)`.

```python
from flask import Flask, redirect, url_for, render_template, request, abort
app = Flask(__name__)

@app.route('/')
def index():
   return render_template('log_in.html')

@app.route('/login',methods = ['POST', 'GET'])
def login():
   if request.method == 'POST':
      if request.form['username'] == 'admin' :
         return redirect(url_for('success'))
      else:
         abort(401)
   else:
      return redirect(url_for('index'))

@app.route('/success')
def success():
   return 'logged in successfully'

if __name__ == '__main__':
   app.run(debug = True)
```
