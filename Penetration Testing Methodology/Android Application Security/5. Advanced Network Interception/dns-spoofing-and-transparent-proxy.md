# DNS Spoofing and Transparent Proxy

* We can also manipulate and intercept network traffic going to a web address by modifying DNS servers and then changing DNS record of that particular web address.
* We have to setup our own DNS server.
* I am using dnsmasq to create our own dns servers. If i (future me) have better alternative use it.

#### dnsmasq setup

* Custom configuration for dnsmasq - `dnsmasq.conf`

```xml
address=/hextree.io/192.168.178.37
address=/ht-api-mocks-lcfc4kr5oa-uc.a.run.app/192.168.178.37
log-queries
```

* Running dnsmasq with docker

```xml
docker pull andyshinn/dnsmasq
docker run --name my-dnsmasq --rm -it -p 0.0.0.0:53:53/udp \\
 -v <LOCAL_dnsmasq.conf_FILE_PATH>:/etc/dnsmasq.conf andyshinn/dnsmasq.conf andyshinn/dnsmasq
```

#### Configuring DNS server in Android

* We can use either privet DNS or change the wifi settings to static and configure our own DNS but both the method is not that useful private DNS only works with hostname and not IP and apps like chrome uses DNS over https so it ignores the wifi DNS setting.
* So here the best way is to use a vpn to force app to use custom DNS for that i am again using rethink-app.
* Configuring rethink app (Here we will be only changing the DNS and not configuring any vpn)
  * Change DNS settings to “Other DNS”
  * Select “Proxy DNS”
  * Create a new entry pointing at your local DNS server Host

#### Invisible proxy setup

* Invisible proxy just work as a middleman after configuring the DNS server it will send the request of [example.com](http://example.com) to 192.168.29.1 and we setup an invisible proxy in burp so it will first go to burp and then burp will send it where it is supposed to go.
* Steps to setup Invisible proxy:-
  * In the proxy setting set the proxy listener bind address to all interface
  * Then form request handling tab turn on support invisible proxying.

> **Make sure you have invisible proxy listener on both port `443` and `80` .**

***
