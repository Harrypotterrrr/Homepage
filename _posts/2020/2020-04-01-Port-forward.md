---
layout: article
title: "Port forward"
sidebar:
  nav: Cookbook
aside:
  toc: true
tags:
  - cookbook
key: blog-2020-04-01-Port-forward
date: 2020-04-01
modify_date: 2020-07-15
---

Given a internal server without public IP, this cookbook aims to instruct how to use port forward techniques to access resources of it with the help of the outside server with public IP.
<!--more-->

## Preparation

- One internal server without public IP, has an account named `innerServer`. This server has resources you want to access remotely.
- One global server with public IP `xx.xx.xx.xx`, has an account named `publicServer`. This server is used to forward ports.
- Personal PC, named `personalPC`. This one is the only PC around you, enabling you to fetch data from remote nodes.

## Target

Manipulate `personalPC` to access remote resources from `innerServer` through `publicServer`.


## Procedure

### Forward the app port to the public server

**1\.** Remote port forwarding to enable `publicServer` to listen applications residing in `innerServer`. Run the command on `innerServer`:
   
``` shell
ssh -NfR <remote_port>:localhost:<inner_port> publicServer@xx.xx.xx.xx
```

where `<remote_port>` is the port specified as the listening port on `publicServer`, and `inner_port` is the port of the running application on `innerServer`, e.g. `inner_port` sets to 22 as ssh service is to be forwarded.

If the error `ssh : Permission denied (publickey,gssapi-with-mic)` appears, please check [FAQ](#faq).

It is also helpful to check whether the ports are being listened at backend both on two server:

``` shell
netstat -nultp | grep <port>
```

**2\.** Test on `publicServer` to check if port forwarding works. Run command on `publicServer`:
   
``` shell
ssh -p <remote_port> innerServer@localhost
```
Note that the innerServer is the account name of `innerServer`.

Then there are **two ways** to fetch data to your local PC.

### Forward the listening port to the local server

**3\.** Local port forwarding let a connection from a local computer to another server. Run the command on `personalPC`:
 
``` shell
ssh -NfL <local_port>:localhost:<remote_port> publicServer@xx.xx.xx.xx
```

where `remote_port` is the same to the previous setting in remote forwarding step, and the `local_port` is an available port on your personal PC.

**Info:** If the SSH connection and the port forwarding both go in the same direction, then it is said to be a local forwarding, otherwise remote forwarding.
{:.info}

**4\.** Test connection on your personal PC. Execute the command following or use the third-party app like MobaXTerm to log in the `innerServer`:

``` shell
ssh -p <local_port> innerServer@xx.xx.xx.xx
```

While it would be convenient to fetch remotely in this way, every time local forwarding the port from the local PC to the public server is not handy. Then comes another way.

### Access resources directly through visiting the remote server

Without step 3 and 4, you directly access the listening port of the `publicServer`. Try the following command on `personalPC`:

``` shell
ssh -p <remote_port> innerServer@xx.xx.xx.xx
```

If time out comes out or the connection failed, it is time to check the firewall checking of the `publicServer`. The following steps are helpful to the public server deployed on google cloud.

## Firewall setting

Google cloud has a series of extremely strict regulations and mechanism to prevent from DDOS and other sorts of malevolent attacks. Thus the available port on the google engine is limited and have to be appended whenever it needed. Google issues a useful firewall documentation to manage ports. Check this [link](https://cloud.google.com/sdk/gcloud/reference/compute/firewall-rules/create) for more info.

Shortly, you could firstly try to find out where Firewall Rules option is available by going to look at **VPC Networking** title of the google cloud console. Then in the **firewall** tab, all port rules are listed.

It is a good way to append personal rules by click **CREATE FIREWALL RULE**, but it is more recommended to use commands on Google Engine:

**1\.** First authorize gcloud to access the Cloud Platform with personal credentials:

``` shell
gcloud auth login
```

filling in the verification code through the link command line provides.

**2\.** Append the port through the following command:

``` shell
gcloud compute firewall-rules create <rule_name> --allow tcp:<remote_port>
```

where the `rule_name` should satisfy the naming rule, and `remote_port` is the same to the above specified remote port.

Then you could return to the **VPC Networking** website to check if the rule is appended. Now it is time to test if you could access the `innerServer` directly from you `personalPC` through this `remote_port` on `publicServer`

## Advanced

The connection frequently invalid and too tricky to deal with the disconnection?
Here is a more convenient way to automatically check the tunnel you built between two server.

### Waive password to log in

**1\.** Build public&private ssh key on `innerServer`, execute:

``` shell
ssh-keygen
```
then `id_rsa`, `id_rsa.pub` and `known_hosts` three files are generated in `~/.ssh`

**2\.** Add ssh key to the public server to authorize identity, then no more password needed next time when log in. Here are two ways to reach this:

- copy the content of `id_rsa.pub` of `innerServer` to `~/.ssh/authorized_keys` of `publicServer`.
- resort to `ssh-copy-id` command: `ssh-copy-id <publicServer>@xx.xx.xx.xx`, which will automatically copy the public key to the authorized list.

### Auto reconnect the broken ssh

Specify a new port `<listen_port>` to listen the state of the ssh connection and replace `ssh` with `autossh` instructions:

``` shell
autossh -M <listen_port> -NfR <remote_port>:localhost:<inner_port> publicServer@xx.xx.xx.xx
```

**warning:** The `-f` flag of `autossh` is different with the one in `ssh`, which means `autossh` will run in the background without a chance to input password. Thus, it is preferable to append `id_rsa.pub` of `InnerServer` to `authorized_keys` of `publicServer` before using `autossh -f`
{:.warning}

### More extention

Fill the above instructions in the startup setting in terms of the system version so as to launch the autossh service whenever the system starts up:

```
SysV：/etc/inid.d/autossh
Upstart: /etc/init/autossh.conf
systemd: /usr/lib/systemd/system/autossh.service
```

### Startup script example

Here is an example of a `xxx.service` file as Ubuntu startup script under `/etc/systemd/systme/`:

```
[Unit]
Description=Port forward
After=network.target syslog.target
Wants=network.target default.target

[Service]
Type=forking
User=lz
Group=lz
ExecStart=/bin/bash /home/lz/potter/scripts/portf.sh
ExecReload=/bin/pkill autossh && /home/lz/potter/scripts/portf.sh
ExecStop=/bin/pkill autossh

[Install]
WantedBy=default.target
```

`portf.sh` script is to describe which port to forward under specified path:

```
autossh -M 9230 -NfR 9220:localhost:22 root@xx.xx.xx.xx
autossh -M 8895 -NfR 8894:localhost:8894 root@xx.xx.xx.xx
```

then refresh the system daemon and start your service:

``` bash
sudo systemctl daemon-reload
systemctl start xxx.service
```


## FAQ

### SSH Permission Denied

It is common problem happens on the newly deployed server.
Solved it by amending in `/etc/ssh/sshd_config`:
`PasswordAuthentication yes`, `GatewayPorts yes` and also `PermitRootLogin yes` if you use root identity to log in, then re-started the service using `service sshd restart`.


## Reference

- [SSH反向连接及Autossh](https://www.cnblogs.com/irockcode/p/6629526.html)
- [使用 ssh 端口转发实现登陆内网主机](https://blog.csdn.net/bgao86/article/details/80233913)
- [port forward sketches](https://unix.stackexchange.com/questions/115897/whats-ssh-port-forwarding-and-whats-the-difference-between-ssh-local-and-remot)
- [端口转发详解及实例](https://www.cnblogs.com/keerya/p/7612715.html)