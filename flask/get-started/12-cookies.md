# Part 12: Cookies

A cookie is stored on a client’s computer in the form of a text file. Its purpose is to remember and track data pertaining to a client’s usage for better visitor experience and site statistics.

A `Request object` contains a cookie’s attribute. It is a dictionary object of all the cookie variables and their corresponding values, a client has transmitted. In addition to it, a cookie also stores its expiry time, path and domain name of the site.

In Flask, cookies are set on response object. Use `make_response()` function to get response object from return value of a view function. After that, use the `set_cookie()` function of response object to store a cookie.

Reading back a cookie is easy. The `get()` method of `request.cookies` attribute is used to read a cookie.

In the following Flask application, a simple form opens up as you visit `‘/’` URL.

```python
@app.route('/')
def index():
   return render_template('index.html')
```

This HTML page contains one text input.
```html
<html>
   <body>
   
      <form action = "/setcookie" method = "POST">
         <p><h3>Enter userID</h3></p>
         <p><input type = 'text' name = 'nm'/></p>
         <p><input type = 'submit' value = 'Login'/></p>
      </form>
      
   </body>
</html>
```
The Form is posted to ‘/setcookie’ URL. The associated view function sets a Cookie name userID and renders another page.

@app.route('/setcookie', methods = ['POST', 'GET'])
def setcookie():
   if request.method == 'POST':
   user = request.form['nm']
   
   resp = make_response(render_template('readcookie.html'))
   resp.set_cookie('userID', user)
   
   return resp
   
   
The Form is posted to `‘/setcookie’` URL. The associated view function sets a Cookie name `userID` and renders another page.

```python
@app.route('/setcookie', methods = ['POST', 'GET'])
def setcookie():
   if request.method == 'POST':
   user = request.form['nm']
   
   resp = make_response(render_template('readcookie.html'))
   resp.set_cookie('userID', user)
   
   return resp
```

`‘readcookie.html’` contains a hyperlink to another view function `getcookie()`, which reads back and displays the cookie value in browser.
```python
@app.route('/getcookie')
def getcookie():
   name = request.cookies.get('userID')
   return '<h1>welcome '+name+'</h1>'

```
Run the application and visit `http://localhost:5000/`
