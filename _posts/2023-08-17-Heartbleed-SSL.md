---
layout: post
title: Heartbleed (SSL)
tags: PoC SSL bugbounty
categories: vulnerabilities 
---

### Descripción

Heartbleed fue una vulnerabilidad de seguridad informática que se descubrió en abril de 2014 en la implementación de la extensión TLS (Transport Layer Security) llamada OpenSSL. OpenSSL es una biblioteca de software ampliamente utilizada para implementar protocolos de seguridad en las conexiones de red, como HTTPS, que es esencial para la seguridad de las comunicaciones en línea.

### Impacto

La vulnerabilidad Heartbleed permitió a los atacantes acceder a la memoria de un servidor vulnerable, que podría incluir datos confidenciales como contraseñas, claves privadas, tokens de autenticación y otros tipos de información confidencial. Esto se debió a un error en la implementación de la extensión Heartbeat en OpenSSL, que permitía a un atacante solicitar información adicional de la memoria del servidor sin ser detectado.

### Detectando Heartbleed

#### sslscan

Al usar la herramienta de sslscan solo debemos indicar la dirección IP en conjunto del puerto del servicio que se está revisando.

~~~ shell
sslscan <ip:port>
~~~

![sslscan-heartbleed](/assets/00/image.png)

#### nmap

Podemos revisar si la vulnerabilidad de Heartbleed utilizando nmap de la siguiente forma.

~~~ shell
nmap --script ssl-heartbleed -p<port> <IP>
~~~

![ssl-heartbleed-nmap](/assets/00/1.pngg)

### Dump memory con ssltest.py

Para ver los datos filtrados en la memoria, usamos el script "ssltest.py" que indica la dirección IP de la víctima junto con el puerto en el que se ejecuta el servicio.

~~~ shell
python3 ssltest.py <ip> -p <port>
~~~

Ejemplo:

~~~ shell
python3 ssltest.py 127.0.0.1 -p 8443
~~~

![dump](/assets/00/2.png)


