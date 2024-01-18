---
layout: post
title: Burpsuite - Fuzzing Cookie
tags: web tool burpsuite bugbounty CBBH PoC cookie
categories: tools 
---

En muchas ocasiones los sitios webs trabajan con cookies generadas a partir de un nombre de usuario. Para este ejemplo, intentaremos verificar si es posible tener acceso a un recurso distinto de un usuario "guest" realizando fuzzing en la cookie.

Suponemos que tenemos acceso al siguiente recurso como invitados (guest).

![](/assets/12/00.png)

La cookie corresponde al MD5 del usuario guest.

~~~ MD5
guest:084e0343a0486ff05530df6c705c8bb4
~~~

Para poder realizar el procesamiento de los nombres de usuarios e ir modificando la cookie en tiempo real, primero interceptaremos un request en donde se utilice la cookie.

![](/assets/12/01.png)

Una vez seleccionada la cookie a modificar, en el apartado de Payloads cargaremos una posible lista de usuarios.

![](/assets/12/02.png)

Si bien tenemos una lista de usuarios cargadas, aún no están procesados para poder convertirse en cookies, por lo que, ahora en el apartado "Payload proccesing"  Seleccionamos "Add" y marcamos el tipo Hash en MD5. y presionamos en OK.

![](/assets/12/03.png)

Ahora cada vez que se vaya a utilizar uno de los nombres de usuarios facilitados por la lista, estos serán transformados a MD5 antes de pasar al request.

![](/assets/12/04.png)

Realizando Fuzzing a la cookie, se logró obtener acceso a información adicional correspondiente a otro usuario.

