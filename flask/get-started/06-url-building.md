# Part 6: URL Building
The `url_for()` function is very useful for dynamically building a URL for a specific function. The function accepts the name of a function as first argument, and one or more keyword arguments, each corresponding to the variable part of URL.

The following script demonstrates use of url_for() function.
```python
from flask import Flask, redirect, url_for
app = Flask(__name__)

@app.route('/admin')
def hello_admin():
   return 'Hello Admin'

@app.route('/guest/<guest>')
def hello_guest(guest):
   return 'Hello %s as Guest' % guest

@app.route('/user/<name>')
def hello_user(name):
   if name =='admin':
      return redirect(url_for('hello_admin'))
   else:
      return redirect(url_for('hello_guest',guest = name))

if __name__ == '__main__':
   app.run(debug = True)

```
The above script has a function `user(name)` which accepts a value to its argument from the URL.

The `User()` function checks if an argument received matches `‘admin’` or not. If it matches, the application is redirected to the `hello_admin()` function using `url_for()`, otherwise to the `hello_guest()` function passing the received argument as guest parameter to it.

Save the above code and run from Python shell.

Open the browser and enter URL as − `http://localhost:5000/hello/admin`

The application response in browser is −
```
Hello Admin
```
Enter the following URL in the browser − `http://localhost:5000/hello/mvl`

The application response now changes to −
```
Hello mvl as Guest
```
