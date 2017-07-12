# Part 4: Routing
Modern web frameworks use the routing technique to help a user remember application URLs. It is useful to access the desired page directly without having to navigate from the home page.

The `route()` decorator in Flask is used to bind URL to a function. For example −
```pyrhon
@app.route(‘/hello’)
def hello_world():
   return ‘hello world’
```
Here, URL `‘/hello’` rule is bound to the `hello_world()` function. As a result, if a user visits `http://localhost:5000/hello` URL, the output of the `hello_world()` function will be rendered in the browser.

The `add_url_rule()` function of an application object is also available to bind a URL with a function as in the above example, `route()` is used.

A decorator’s purpose is also served by the following representation −
```python
def hello_world():
   return ‘hello world’
app.add_url_rule(‘/’, ‘hello’, hello_world)
```

