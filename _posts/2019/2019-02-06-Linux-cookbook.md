---
layout: article
title: "Linux cookbook"
sidebar:
  nav: Cookbook
aside:
  toc: true
tags:
  - cookbook
key: blog-2019-02-05-Linux-cookbook
date: 2019-02-06
modify_date: 2019-09-06
---

This cookbook is built for quick and easy search for commands and deployments of Linux, Pip, Conda, Jupyter, Remote pycharm, Apache
<!--more-->

## Linux

### Commands

- open HTTP server with specific port

``` shell
python -m SimpleHTTPServer $PORT &
python -m http.server $PORT
```

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

``` shell
nslookup $DOMAIN_ADDRESS
```

- count the number of line of files

``` shell
wc -l $FILE_NAME
```

- change access permission

``` shell
chmod [Option] [ugoa][+-=][rwx] $FILE_NAME
```

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

- convert py2 to py3

``` shell
2to3 [-w] [-n] <file_name>
```

`-w` will write to the origin file, `-n` will save a backup

- change default gpu to run xorg and gnome process

modify `/usr/X11/xorg.conf` and replace BusID:

``` shell
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
    BoardName      "GeForce TitanX"
    BusID          "PCI:2:0:0" # change the BusID to what lspci | grep VGA shown
EndSection
```

### .bashrc file

- `:` is to segregate the `PATH`

``` shell
echo $PATH
>> /home/jiahl/code-server/:/home/jiahl/.linuxbrew/bin
```

- append local executable file to environment variable

`~/.bashrc` is a file to store `alias`, `path` etc.

For example, after appending the following to the `.bashrc` file, everything executable can be executed in any directory.

``` shell
CODE_SERVER_PATH=/home/jiahl/code-server/
PATH=$CODE_SERVER_PATH:$PATH
export CODE_SERVER_PATH PATH
```

**Note that** using `source ~/.bashrc` to make bash resource file effect.

- `-x` make `.sh` file executable

Generally, `.sh` file is not executable execept its mode has been changed:

`chmod a+x <FILE_NAME>.sh`

### less mode

- `k` move upward a line

- `j` move downward a line

- `u` move upward a half page

- `d` move downward a half page

- `b` move upward an whole page

- `f` move downward an whole page

- `=` display the information of the current line

- `\<string>` search forward in the file

- `?<string>` search backward in the file

- `n` search the next occurrence of the string

- `N` search the previous occurrence of the string

### nvidia-driver

- `sudo dpkg -l | grep nvidia` list all drivers relative to nvidia

- `sudo apt remove --purge '*nvidia*'` remove and clear specified package or drivers

- `sudo dpkg --force-all -P <package>` forcely remove the library difficult to deleted in previous step

- `sudo add-apt-repository ppa:graphics-drivers/ppa` add ppa for the installation

- `ubuntu-drivers devices` show available drivers online

- `sudo apt install nvidia-driver-<version> nvidia-settings` install nvidia driver

- `cat /usr/local/cuda/version.txt` or `nvidia-smi` show the version of cuda

- `cat /proc/driver/nvidia/version` or `nvidia-smi` show the version of nvidia-driver

## Ssh

- Automatic log in to remote server

add following to `~/.ssh/config`

``` shell
Host <host_var>
    HostName    <host_name>
    Port        22
    User        <user_name>
```

if public key is appended to the `server:/~/.ssh/authorized_keys`, password won't be needed


## Ruby

### Installation

To install the ruby **without privilege of root**, the third-party package `RVM` is strongly suggested. [reference link](https://blog.codeminer42.com/4-5-ways-to-install-ruby-in-userspace-d26b0ba43610)

- using RVM

RVM installer script uses GPG to check signatures; because of that, the first thing to do is to download the gpg key

``` shell
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
```

then download and execute the script from https://get.rvm.io:

``` shell
curl -sSL https://get.rvm.io | bash -s stable
```

source the file

``` shell
source ~/.rvm/scripts/rvm
```

**disable RVM from trying to install necessary software via apt-get**

``` shell
rvm autolibs disable
```

get the listt of known Ruby versions

``` shell
rvm list known
```

install the latest, eg:

``` shell
rvm install 2.6.3
```


## Shell

- **leave no space**

``` shell
STR = "foo" # wrong
STR="foo" 	# correct
```

- use ```` or `$()` to assign a command

``` shell
DIRS=$(find . * -type d | grep -v "\.")
echo ${DIRS}
```

- calculate and loop

**`assign` must not leave space**
**`expr` must leave space**

``` shell
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
``` shell
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

``` shell
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple $PACKAGE
```

- pip ungrade package

``` shell
pip install $PACKAGE --upgrade
```

## Conda

- check all envs

``` shell
conda info -e
```

- create the env

``` shell
conda create -n xxxx 
```

- create and clone the env

``` shell
conda create -n xxxx1 --clone xxxx2
```

- remove the env

``` shell
conda remove -n xxxxx --all
```

- activate the env

``` shell
source activate xxxx
```

- deactivate the env

``` shell
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

**Warning:** Starting the notebook server before setting up the password
{:.warning}

**Info:** After using this way to creat a port, `jupyter notebook stop` command is usually invalid, therefore `kill` is commended
{:.info}

- stop the jupyter with specific port

`jupyter notebook stop $PORT` where PORT is the port got to stop

- insert true tabs in jupyter editor/notebook

`jupyter --config-dir` to create config file for user

then create `~/.jupyter/nbconfig/edit.json`:

``` json
{
    "Editor": {
        "codemirror_options": {
            "indentWithTabs": true
        }
    }
}
```

And create `~/.jupyter/nbconfig/notebook.json` with:

``` json
{
    "CodeCell": {
        "cm_config": {
            "indentWithTabs": 2
        }
    }
}
```

- add conda env

``` shell
python -m ipykernel install --user --name $ENV_NAME --display-name "$DISPLAY_NAME"
```

- remove conda env

``` shell
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


## Virtualenv

- create virtual environment

``` shell
virtualenv <env_name>
```

- activate the virtualenv

``` shell
source <env_name>/bin/activate
```

- deactivate the virtualenv

``` shell
deactivate
```

## Remote pycharm

- import tf error

``` python
print(os.environ.get('LD_LIBRARY_PATH', None))
```

add path of output to env variable in configuration



## Shadowsocks

### Server

- install on Ubuntu

``` shell
apt-get install python-pip python-setuptools
pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

- run in the background

``` shell
ssserver -p $PORT -k $PASSWORD -m aes-256-cfb --user nobody -d start
```
where PORT and PASSWORD are refered to the ones of the server.

- stop the ss

``` shell
ssserver -d stop
```

- check the log

``` shell
less /var/log/shadowsocks.log
```

### Client

For linux operating system:

- create `SOCKS5` proxy

``` shell
nohup sslocal -s $IP -p $SERVER_PORT -k "$PASSWORD" -l $CLIENT_PORT [-d 0.0.0.0] -t 600 -m aes-256-cfb &
``` 
<a id="sslocal"></a>

where `$SERVER_IP` and `$PASSWORD` are refered to the ones of the server and `$CLIENT_PORT` is refered to the one of the client.  

**Note that** different from `vpn` global proxy,`sslocal`(shadowsocks on linux) creates a tunnel of `SOCKS5` proxy and only works for browsers rather than running files or applications such as `curl`, `wget` or any other commands in bash. 
{: .error}

There are two ways to hook calls from local programs to sockets and redirect it through one or more socks/http proxies.

1. Use `proxychains4` to force applications to go through `SOCKS5` proxy. [link](#Proxychains)

2. Use `privoxy` to convert `SOCKS5` into `HTTP` proxy, and support program using it by exporting the environment variable. [link](https://blog.liuguofeng.com/p/4010) **TO BE DONE BY SELF**

- check running status

``` shell
systemctl status rc-local.service
```

- use shadowsockssr JSON config file to run

``` shell
git clone https://github.com/shadowsocksrr/shadowsocksr
vim config.json
cd shadowsocks/
./local.py -c ../config.json
```

## Proxychains

Here I demonstrate to download a large file from google drive.

### Preliminaries

- installation

For the non-root user of the server, using `Linuxbrew` to install it is recommended

`/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

- modify the config file

`vim ~/.linuxbrew/etc/proxychains.conf`

change the last line of the config file to your specified port with `SOCKS5`, e.g. `socks5  127.0.0.1 $LOCAL_PORT`

where the `$LOCAL_PORT` is same to the one in `SOCKS5` creating command as `sslocal ... $CLIENT_PORT` [link](#sslocal)


### Test

Comparing `curl google.com` to `proxychains4 curl google.com`

### Fetching file

Each file shared on Google drive has a unique id (`$FILE_ID`), like `1UFaRl_3EGK0hSBlmB6AJVeZZ_r-SHQLm` in `https://drive.google.com/uc?id=1UFaRl_3EGK0hSBlmB6AJVeZZ_r-SHQLm&export=download`

There are two ways to download it

- By `wget` command (Recommand)

append proxychains4 as the prefix to any commands

`proxychains4 wget --no-check-certificate -r 'https://docs.google.com/uc?export=download&id=$FILE_ID' -O $FILE_NAME`

where `$FILE_ID` is an identifier to the shared file, and `$FILE_NAME` is the name of the output file.

**Note that** `-r` is crucial and decide `wget` in recursive mode.

- By `gdrive.sh` script

`proxychains4 curl gdrive.sh | proxychains4 bash -s $FILE_ID`

This solution is **unstable** in some cases, the above $FILE_ID mentioned is a case.

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

``` html
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

``` html
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

