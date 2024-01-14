---
layout: post
title: HTTP Response Splitting
tags: oscp ewpt ctf PoC httprs bugbounty xss
categories: vulnerabilities
---

# Descripción

Este tipo de ataque de inyección de headers ocurre cuando un atacante envía datos arbitrarios al servidor y estos datos se devuelven como parte de la respuesta HTTP.

Como siguiere el nombre, el objetivo HTTP Response Splitting (ataque de divisoón de respuesta HTTP) es dividir la respuesta en dos partes por el medio de caracteres especiales como:

- CR (Carriage Return) \r
- LF (Line Feed) \n

## Header Injection Attack v/s HTTP Response Splitting

La diferencia entre un Header Injection Attack y un HTTP Response Splitting es que en el segundo no solo estamos inyectando datos en los Headers, sino que también estamos creando un bloque de respuesta HTTP completamente nuevo con nuevos Headers.

# PoC

Se encuentra destacado en rojo la entrada reflejada por el usuario.

![Alt text](/assets/17/1.png)

A continuación se muestra la explotación inyectando contenido adicional es cual es reflejado en la respuesta del servidor.

![Alt text](/assets/17/2.png)


# XSS Throught HTTP Response Splitting

En el siguiente ejemplo, realizaremos un ataque XSS a través de un HTTP Response Splitting. La repuesta contiene el siguiente código:

~~~ js
<script>
    alert(document.cookie);
</script>
~~~

![Alt text](/assets/17/3.png)

Request:

![Alt text](/assets/17/4.png)

Response:

![Alt text](/assets/17/5.png)