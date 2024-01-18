---
layout: post
title: Proxychains
tags: tool proxy proxychain
categories: tool 
---

Proxychains enruta todo el tráfico procedente de cualquier herramienta de consola a cualquier proxy que especifiquemos. Proxychains agrega un proxy a cualquier herramienta de comandos, y por lo tanto, es el método más sencillo para enrutar el tráfico web a través de nuestros servidores proxy web.

Para usar proxychains, primero tenemos que editar "/etc/proxychains.conf", se debe comentar la línea final y agregar la siguiente línea.

~~~ bash
#socks4         127.0.0.1 9050
http 127.0.0.1 8080
~~~

También deberíamos permitir Quiet Mode para reducir el ruido descomentando quiet_mode. Una vez hecho esto, podemos anteponer proxychains cualquier comando y el tráfico de ese comando debe enrutarse a través de proxychains(es decir, nuestro proxy web). Por ejemplo, intentemos usar cURL:

![](/assets/11/00.png)

Vemos que funcionó como lo haría normalmente, con la línea adicional  ProxyChains al principio, para notar que se está enrutando a través de ProxyChains. Si volvemos a nuestro proxy web (Burp en este caso), veremos que efectivamente la solicitud ha pasado por él:

![](/assets/11/01.png)