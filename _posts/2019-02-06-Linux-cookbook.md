---
title: 'Linux cookbook'
date: 2019-02-16
modified: 2019-05-15
permalink: /posts/2019/02/Linux-cookbook/
excerpt_separator: <!--more-->
tags:
  - cookbook
---

This cookbook is built for quick and easy search for commands and deployments of Linux, Pip, Conda, Jupyter, Remote pycharm, Apache
<!--more-->

## Linux

### Commands

- open HTTP server with specific port

`python -m SimpleHTTPServer $PORT &`
`python -m http.server $PORT`

- Recursively count the number of file under current dirctory, including subdirctory

`ls -lR | grep "^-"| wc -l`

- get the size of the file

`du -sh`
where `s` refers to SUM, and `h` refers to be friendly to human to view

- display port usage

`netstat -tunlp | grep $PORT`

- print CUDA version

`cat /usr/local/cuda/version.txt`
`nvcc -V`

- Trash bin path

`~/.local/share/Trash/files/`

- Look up self public ip

`curl cip.cc`  

- look up public ip of the domain address

`nslookup $DOMAIN_ADDRESS`

- count the number of line of files

`wc -l $FILE_NAME`

- change access permission

`chmod [Option] [ugoa][+-=][rwx] $FILE_NAME`

Option: -R recursive

|  User                               | letter |
|-------------------------------------|--------|
| The user who owns it                | u      |
| Other users in the file's Group     | g      |
| Other users not in the file's group | o      |
| All users                           | a      |

|  Permission                         | letter |
|-------------------------------------|--------|
| Read    							  | r      |
| Write								  | w      |
| Execute (or access for directories) | x      |

### .bashrc file

- `:` is to segregate the `PATH`

```bash
echo $PATH
>> /home/jiahl/code-server/:/home/jiahl/.linuxbrew/bin
```

- append local executable file to environment variable

`~/.bashrc` is a file to store `alias`, `path` etc.

For example, after appending the following to the `.bashrc` file, everything executable can be executed in any directory.

```bash
CODE_SERVER_PATH=/home/jiahl/code-server/
PATH=$CODE_SERVER_PATH:$PATH
export CODE_SERVER_PATH PATH
```

**Note that** using `source ~/.bashrc` to make bash resource file effect.

- `-x` make `.sh` file executable

Generally, `.sh` file is not executable execept its mode has been changed:

`chmod a+x <FILE_NAME>.sh`


## Shell

- **leave no space**

```bash
STR = "foo" # wrong
STR="foo" 	# correct
```

- use ```` or `$()` to assign a command

```bash
DIRS=$(find . * -type d | grep -v "\.")
echo ${DIRS}
```

- calculate and loop

**`assign` must not leave space**
**`expr` must leave space**

```bash
#!/bin/bash
a=1 # no space
for i in $(seq 1 2 20)
do
	a=`expr ${a} + 1`
    echo "Welcome $i times"
    echo "a = $a"
done
```
output:
```
sh clear.sh
Welcome 1 times
a = 2
Welcome 3 times
a = 3
Welcome 5 times
a = 4
Welcome 7 times
a = 5
Welcome 9 times
a = 6
```


## Pip 

- pip accelerate

```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple $PACKAGE
```

- pip ungrade package

```
pip install $PACKAGE --upgrade
```

## Conda

- check all envs

```
conda info -e
```

- create the env

```
conda create -n xxxx 
```

- create and clone the env

```
conda create -n xxxx1 --clone xxxx2
```

- remove the env

```
conda remove -n xxxxx --all
```

- activate the env

```
source activate xxxx
```

- deactivate the env

```
source deactivate xxxx
```

reference: https://blog.csdn.net/menc15/article/details/71477949


## Jupyter

- convert *.ipynb to .py

`jupyter nbconvert --to script *.ipynb`

- start the jupyter with specific port

`nohup jupyter notebook --port=7141 --ip=0.0.0.0 --allow-root --notebook-dir='~/' &`

- create a passowrd to avoid using TOKEN to log in

check if the config file exists or not

`jupyter notebook --generate-config`

generate password

`jupyter notebook password`

**Starting the notebook server before setting up the password**

**After using this way to creat a port, `jupyter notebook stop` command is usually invalid, therefore `kill` is commended**

- stop the jupyter with specific port

`jupyter notebook stop $PORT` where PORT is the port got to stop

- insert true tabs in jupyter editor/notebook

`jupyter --config-dir` to create config file for user

then create `~/.jupyter/nbconfig/edit.json`:
```
{
    "Editor": {
        "codemirror_options": {
            "indentWithTabs": true
        }
    }
}
```

And create `~/.jupyter/nbconfig/notebook.json` with:

```
{
    "CodeCell": {
        "cm_config": {
            "indentWithTabs": 2
        }
    }
}
```

- add conda env

```
python -m ipykernel install --user --name $ENV_NAME --display-name "$DISPLAY_NAME"
```

- remove conda env

```
jupyter kernelspec remove $ENV_NAME
```

- import technique

working directory:
```
WorkingDirectory--
                 |--MyPackage--
                 |            |--__init__.py
                 |            |--module1.py
                 |            |--module2.py
                 |
                 |--notebook.ipynb
```
In `__init__.py`,
```python
import module1
import module2
```
In `notebook.ipynb`,
```python
import sys, os
sys.path.append(os.getcwd())

import MyPackage.modeul1
```

**Note that** there are several reason for `No module named 'XXXX'`, one is that `packages` of `.py` must be in one `utility directory`, 
and `init file` of the package must be built in previous. Another is that `current working directory(cwd)` must be appended to `system path`


## Remote pycharm

- import tf error

```python
print(os.environ.get('LD_LIBRARY_PATH', None))
```

add path of output to env variable in configuration

## Shadowsocks

### Server

- install on Ubuntu

```
apt-get install python-pip python-setuptools
pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

- run in the background

```
ssserver -p $PORT -k $PASSWORD -m aes-256-cfb --user nobody -d start
```
where PORT and PASSWORD are refered to the ones of the server.

- stop the ss

```
ssserver -d stop
```

- check the log

```
less /var/log/shadowsocks.log
```

### Client

- run in the background

```
nohup sslocal -s $IP -p $SERVER_PORT -k "$PASSWORD" -l $CLIENT_PORT -t 600 -m aes-256-cfb &
```
where $SERVER_IP and $PASSWORD are refered to the ones of the server and $CLIENT_PORT is refered to the one of the client.  

- check running status

```
systemctl status rc-local.service
```

- use shadowsockssr source code to run

```
git clone https://github.com/shadowsocksrr/shadowsocksr
vim config.json
cd shadowsocks/
./local.py -c ../config.json
```

## Apache

### installation

- install the apache package

`apt install apache2`

- adjust the firewall, check the available ufw application profiles

`ufw app list`

- Check your Web Server, Check with the systemd init system to make sure the service is running by typing:

`systemctl status apache2`

### config your own index.html

- build an index.html file in anywhere, take '/home/test_html/' as an example

`vim /home/test_html/index.html`

add the below in the file

```html
<body>
  Hello world!
</body>
```

- append the specified path to the virtualHost statement

`vim /etc/apache2/sites-available/000-default.conf`

modify the listen port into `7146` in the first line as an example

`<VirtualHost *:7146>`

change the virtualHost root to the specified path in `DocumentRoot`

for the above example, we modify the below,

`DocumentRoot /home/test_html`

- config the port.conf to add listen port

`vim /etc/apache2/ports.conf`

then add Listen ports into the config file

- modify the main config file to add your own web file

for the default security mode, apache doesn't allow access outside of `/usr/share` and `/var/www`.

therefore we should add an item to allow the specified path to be accessed.

`vim /etc/apache2/apache2.conf`

for the above example, we add the below,

```
<Directory /home/test_html/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
</Directory>
```

- then restart the apache service after modify any config files

`systemctl restart apache2`

- view apache log, if there is something wrong

`tail -f /var/log/apache2/error.log`

