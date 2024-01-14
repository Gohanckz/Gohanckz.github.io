---
layout: post
title: Local File Inclusion (LFI)
tags: PoC LFI ewpt bugbounty
categories: vulnerabilities 
---

La vulnerabilidad de Local File Inclusion nos resulta más fácil de entender si nos fijamos en una simple sección de código PHP. Supongamos que la aplicación de destino cambia su contenido según la ubicación del visitante. La URL será algo como esto:

![url](/assets/02/7.png)

Y el código PHP maneja el parámetro de la siguiente manera:

![php code](/assets/02/8.png)

Como puede ver, podemos ingresar cualquier ruta de archivo local válida para que PHP la incluya en la respuesta a nuestro navegador.

![path](/assets/02/9.png)

Ejemplo: La aplicación que se encuentra en http://lfi.site usa el parámetro location para renderizar el contenido al usuario.

![site](/assets/02/10.png)

Si la aplicación es vulnerable a LFI podremos ver lo siguiente:

![exploit](/assets/02/11.png)

El código luce como lo siguiente:
~~~ php
<?php
include($_GET['location'] . "/template.tlp");
?>
~~~

Una explotación válida sería de la siguiente forma:

~~~ shell
index.php?location=../../../etc/passwd%00
~~~

