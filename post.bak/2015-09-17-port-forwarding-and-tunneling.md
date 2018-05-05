---
title: Port forwarding and tunneling
tags:
- ssh
- post exploitation
- pivot
see_also:
  -
    link: Hak5 videos
    url: https://www.youtube.com/watch?v=Wuu_n0Pvbsk
  -
    link: Port forwarding tutoriel on linux-france (fr)
    url: http://www.linux-france.org/prj/edu/archinet/systeme/ch13s04.html
---
As a pentester, you might be able to take control of systems that have a direct access but you also might be able to test the internal network and check the machine who are inside a subnetwork. 

For that task you'll have to use an already compromised machine as a bridge/gateway, this technic is called "pivot". 
Depending of the context, different solutions exist to perform that task.

## Rinetd

The easiest one. First you need to install Rinetd:

```bash
aptitude search rinetd
p   rinetd                                     - Internet TCP redirection server</pre>
```

Then edit the `/etc/rinetd.conf` file:

```
# bindadress    bindport  connectaddress  connectport
192.168.0.10    80        91.121.139.22   8080
```

Restart Rinetd and from now, all incoming traffic on `192.168.0.10` on port `80` will be redirected to `91.121.139.22` on port `8080`. 
This can be usefull if a firewall is restricting outbound traffic on certain port.

<!--more-->

## SSH

### Local port forwarding

`ssh -L <local port to listen>:<remote host>:<remote port> <gateway>`

Similar to port forwarding with Rinetd, this technic still have some tints. The traffic is encrypted but only between the local machine and the gateway. 
If the remote host is `localhost` then it refers to the gateway. Example:

```
ssh -L 8080:192.168.1.25:80 bob@192.168.0.10
```

This open a tunnel between the local machine on port `8080` to `192.168.1.25` on port `80` trough the ssh server `192.168.0.10` connected with user `bob`. 
Connexion from other machines are not accepted by default, to enable this feature you have to use the `-g` option.

### Remote port forwarding

`ssh -R <remote port to bind>:<local host>:<local port> <gateway>`

Note that this must be launched on the already compromised machine ! In this case if the local host is `localhost` then it refers to the local machine. Example:

```
ssh -R 1234:192.168.1.25:80 bob@192.168.0.10
```

This pop a reverse shell on `192.168.0.10` connected with user `bob` and create a tunnel on port `1234` wich will receive all traffic from `192.168.1.25` on port `80`.

## ProxyChains

As a standalone, Proxychains is mainly used to anonymize traffic but combined with SSH it can be used to perform dynamic port forwarding.

```
$ aptitude search proxychains
p   libproxychains-dev                - proxy chains -- shared library (development)
p   libproxychains3                   - proxy chains -- shared library (runtime)
p   proxychains                       - proxy chains - redirect connections through proxy servers</pre>
```

When the install is finished, edit `/etc/proxychains.conf` as here:

```
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
socks4  127.0.0.1 9050
```

Then we can create a tunnel wich will forward all incoming traffic to any host in the internal network trough the compromised machine which runs the ssh server. Syntax:

`ssh -D <local proxy> <target>`

Example:

```
$ ssh -D 127.0.0.1:9050 bob@192.168.0.10
```

From now we can perform scans or anything else on every port on every machine in the DMZ with Proxychains:

```
$ proxychains nmap -p 139,445 192.168.1.100-200
```
