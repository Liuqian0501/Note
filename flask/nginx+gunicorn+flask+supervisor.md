# nginx + gunicorn + flask + supervisor

## step 1:  start a project

```
mkdir pythonwebframework
```

## step 2: create virtualenv

```
cd pythonwebframework

virtualenv venv

source venv/bin/activate
```

## step 3: add and install requirements.txt


- install web framework
  ```
  pip install flask
  ```
- add to requirements.txt for others to read
  ```
  pip freeze > requirements.txt
  ```
```sh
nano requirement.txt 
pip install -r requirement.txt 

#requiremetn.txt
  Fabric==1.10.0
  Flask==0.10.1
  Jinja2==2.7.3
  MarkupSafe==0.23
  Werkzeug==0.9.6
  argparse==1.2.1
  ecdsa==0.11
  gunicorn==19.1.1
  itsdangerous==0.24
  meld3==1.0.0
  paramiko==1.15.1
  pycrypto==2.6.1
  supervisor==3.1.2
  wsgiref==0.1.2

```

## step 4: start a web app
```sh
nano myapp.py  
```

```python  
# myapp.py

from flask import Flask
app = Flask(__name__)

@app.route('/')
def index():
    return 'hello flask'


if __name__ == '__main__':
    app.debug = True
    app.run()
```
 
## step 5 : test the webapp

```
python myapp.py
```
browser the url `http://127.0.0.1:5000`. flask is not good enough for industry level.

## step 6 : deploy by gunicorn

use gunicorn as a wsgi container，and support python flask. This time we open port 8000
```
pip install gunicorn
gunicron -w4 -b0.0.0.0:8000 myapp:app browser the url http://127.0.0.1:8000
#~/virtualenvs/pythonwebframework/bin/gunicorn -w4 -b0.0.0.0:8000 myapp:app browser the url http://127.0.0.1:8000
```
To end gunicorn `process`, need pid, take a lot of work. (need help)
```
pkill gunicorn //pkill - which will kill all processes matching the search text:
```
```
ps ax|grep gunicorn // show id
kill xxxx //where xxxx is the number in the first column
```

## step 7 : `supervisor` for `process` supervising
`supervisor`, A tool dedicated to managing the process, and you can manage the system's tooling process.。

install supervisor
```
pip install supervisor
echo_supervisord_conf > supervisor.conf
vim supervisor.conf
```

my work dictionary: `/home/qian/pythonwebframework`

my venv dictionary: `/home/qian/virtualenvs/pythonwebframework/bin/gunicorn`

append the follwing code to the end of supervisor.conf
```sh
[program:myapp]
command=/home/qian/virtualenvs/pythonwebframework/bin/gunicorn -w4 -b0.0.0.0:2170 myapp:app    ; supervisor set gunicorn to run myapp on 0.0.0.0:2170
directory=/home/qian/pythonwebframework                                                 ; project dirc
startsecs=0                                                                             ; start time
stopwaitsecs=0                                                                          ; end wait time
autostart=false                                                                         ; 
autorestart=false                                                                       ; 
stdout_logfile=/home/qian/pythonwebframework/log/gunicorn.log                           ; log file
stderr_logfile=/home/qian/pythonwebframework/log/gunicorn.err                           ; err file

```

supervisor can mamage web，**edit** folowing
```
[inet_http_server]         ; inet (TCP) server disabled by default
port=127.0.0.1:9001        ; (ip_address:port specifier, *:port for all iface)
username=user              ; (default is no username (open server))
password=123               ; (default is no password (open server))

[supervisorctl]
serverurl=unix:///tmp/supervisor.sock ; use a unix:// URL  for a unix socket
serverurl=http://127.0.0.1:9001 ; use an http:// url to specify an inet socket
username=user              ; should be same as http_username if set
password=123                ; should be same as http_password if set
;prompt=mysupervisor         ; cmd line prompt (default "supervisor")
;history_file=~/.sc_history  ; use readline history if available
```

supervisor base method
```
supervisord -c supervisor.conf                             通过配置文件启动supervisor
supervisorctl -c supervisor.conf status                    察看supervisor的状态
supervisorctl -c supervisor.conf reload                    重新载入 配置文件
supervisorctl -c supervisor.conf start [all]|[appname]     启动指定/所有 supervisor管理的程序进程
supervisorctl -c supervisor.conf stop [all]|[appname]      关闭指定/所有 supervisor管理的程序进程
```

we can use supervsior to start gunicorn. run cmd:  `supervisord -c supervisor.conf`

## step 8: set up nginx

add following code to supervisor.conf

```
[program:nginx]
command=/usr/sbin/nginx
startsecs=0
stopwaitsecs=0
autostart=false
autorestart=false
stdout_logfile=/home/qian/pythonwebframework/log/nginx.log
stderr_logfile=/home/qian/pythonwebframework/log/nginx.err
```





