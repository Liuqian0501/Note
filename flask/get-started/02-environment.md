# part 1: Developing environment of Flask python
## I. Prerequisite
`Python 2.6` or higher is usually required for installation of `Flask`. Although `Flask` and its dependencies work well with `Python 3` (Python 3.3 onwards), many Flask extensions do not support it properly. Hence, it is recommended that Flask should be installed on` Python 2.7`.

## II. Install `virtualenv` for development environment
virtualenv is a virtual Python environment builder. It helps a user to create multiple Python environments side-by-side. Thereby, it can avoid compatibility issues between the different versions of the libraries.

The following command installs `virtualenv` under C:/pythonX/scripts path.Here X is the version name of Python.
```sh
pip install virtualenv
```
The output should be like this −
```
Collecting virtualenv
  Downloading virtualenv-15.0.1-py2.py3-none-any.whl (1.8MB)
    100% |################################| 1.8MB 204kB/s
Installing collected packages: virtualenv
Successfully installed virtualenv-15.0.1
```
This command needs administrator privileges. Add `sudo` before `pip` on Linux/Mac OS. If you are on Windows, log in as Administrator. On Ubuntu `virtualenv` may be installed using its package manager.
```sh
Sudo apt-get install virtualenv
```
Once installed, new virtual environment is created in a folder.
```
mkdir newproj
cd newproj
virtualenv venv
```
To activate corresponding environment, on Linux/OS X, use the following −
```
venv/bin/activate
```
On `Windows`, following can be used −
```
venv\scripts\activate
```

## III. Install Flask 
We are now ready to install Flask in this environment.
```
pip install Flask
```
The output should be like this −
```
Collecting Flask
  Downloading Flask-0.10.1.tar.gz (544kB)
    100% |################################| 544kB 410kB/s
Collecting Werkzeug>=0.7 (from Flask)
  Downloading Werkzeug-0.11.4-py2.py3-none-any.whl (305kB)
    100% |################################| 307kB 531kB/s
Collecting Jinja2>=2.4 (from Flask)
  Downloading Jinja2-2.8-py2.py3-none-any.whl (263kB)
    100% |################################| 266kB 935kB/s
Collecting itsdangerous>=0.21 (from Flask)
  Downloading itsdangerous-0.24.tar.gz (46kB)
    100% |################################| 49kB 1.6MB/s
Collecting MarkupSafe (from Jinja2>=2.4->Flask)
  Downloading MarkupSafe-0.23.tar.gz
Installing collected packages: Werkzeug, MarkupSafe, Jinja2, itsdangerous, Flask
  Running setup.py install for MarkupSafe
  Running setup.py install for itsdangerous
  Running setup.py install for Flask
Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.4 itsdangerous-0.24
```
The above command can be run directly, without virtual environment for system-wide installation.
