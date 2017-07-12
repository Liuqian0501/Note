# Part 9: static files

A web application often requires a static file such as a `javascript` file or a `CSS` file supporting the display of a web page. Usually, the web server is configured to serve them for you, but during the development, these files are served from static folder in your package or next to your module and it will be available at `/static` on the application.

A special endpoint ‘static’ is used to generate URL for static files.

In the following example, a `javascript` function defined in `hello.js` is called on OnClick event of HTML button in `index.html`, which is rendered on `‘/’` URL of the Flask application.

```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route("/")
def index():
   return render_template("index.html")

if __name__ == '__main__':
   app.run(debug = True)
```
The HTML script of `index.html` is given below.

```html

<html>

   <head>
      <script type = "text/javascript" 
         src = "{{ url_for('static', filename = 'hello.js') }}" ></script>
   </head>
   
   <body>
      <input type = "button" onclick = "sayHello()" value = "Say Hello" />
   </body>
   
</html>

```
`hello.js` contains sayHello() function.
```
function sayHello() {
   alert("Hello World")
}
```
