# Part 3: Hello world application
In order to test `Flask` installation, type the following code in the editor as `Hello.py`
```python
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
   return 'Hello World’

if __name__ == '__main__':
   app.run()
```
Importing flask module in the project is mandatory. An object of Flask class is our WSGI application.

**Flask constructor** takes the name of `current module (__name__)` as argument.

The `route()` function of the Flask class is a decorator, which tells the application **which URL should call the associated function**.
```
app.route(rule, options)
```
* The `rule` parameter represents URL binding with the function.

* The `options` is a list of parameters to be forwarded to the underlying Rule object.

In the above example, `‘/’` URL is bound with hello_world() function. Hence, when the **home page** of web server is opened in browser, the output of this function will be rendered.

Finally the run() method of Flask class runs the application on the local development server.
```
app.run(host, port, debug, options)
```
All parameters are optional.

|Sr.No | Parameters & Description  |
|---|:---:|
|  1 | `host`: Hostname to listen on. Defaults to 127.0.0.1 (localhost). Set to ‘0.0.0.0’ to have server available externally |
|  2 |  `port`:Defaults to 5000 |
|  3 |  `debug`:Defaults to false. If set to true, provides a debug information|
|  4 |`options`: To be forwarded to underlying Werkzeug server.|


| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |



The above given `Python` script is executed from Python shell.
```sh
Python Hello.py
```
A message in Python shell informs you that
```
* Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
```
Open the above URL (`localhost:5000`) in the browser. `‘Hello World’` message will be displayed on it.

# Debug mode
A `Flask` application is started by calling the `run()` method. However, while the application is under development, it should be restarted manually for each change in the code. To avoid this inconvenience, enable debug support. The server will then reload itself if the code changes. It will also provide a useful debugger to track the errors if any, in the application.

The Debug mode is enabled by setting the debug property of the application object to True before running or passing the debug parameter to the run() method.
```
app.debug = True
app.run()
app.run(debug = True)
```
