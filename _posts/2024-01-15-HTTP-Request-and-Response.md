---
layout: post
title: HTTP Requests and Responses
tags: web http burpsuite bugbounty CBBH
categories: academy 
---

Las comunicaciones HTTP constan principalmente de una solicitud HTTP y una respuesta HTTP. El cliente (por ejemplo, cURL/navegador) realiza una solicitud HTTP y el servidor (por ejemplo, servidor web) la procesa. Las solicitudes contienen todos los detalles que requerimos del servidor, incluido el recurso (por ejemplo, URL, ruta, parámetros), cualquier dato de solicitud, encabezados u opciones que especifiquemos y muchas otras opcioneS.

Una vez que el servidor recibe la solicitud HTTP, la procesa y responde enviando la respuesta HTTP, que contiene el código de respuesta, como se analiza en una sección posterior, y puede contener los datos del recurso si el solicitante tiene acceso a ellos.

## Request HTTP

Comencemos examinando el siguiente ejemplo de solicitud HTTP:

![](/assets/04/00.png)

La imagen de arriba muestra una solicitud HTTP GET a la URL:

~~~ bash
http://inlanefreight.com/users/login.html
~~~

La primera línea de cualquier solicitud HTTP contiene tres campos principales 'separados por espacios':

| Campo | Ejemplo | Descripción |
|-------|---------|-------------|
| Method | GET | El método o verbo HTTP, que especifica el tipo de acción a realizar. |
| Path | /users/login.html | La ruta al recurso al que se accede. Este campo también puede tener como sufijo una cadena de consulta (por ejemplo, ?username=user). |
| Version | HTTP/1.1 | El tercer y último campo se utiliza para indicar la versión HTTP. |


El siguiente conjunto de líneas contiene pares de valores de encabezado HTTP, como Host, User-Agent, Cookie y muchos otros encabezados posibles. Estos encabezados se utilizan para especificar varios atributos de una solicitud. Los encabezados terminan con una nueva línea, que es necesaria para que el servidor valide la solicitud. Finalmente, una solicitud puede terminar con el cuerpo y los datos de la solicitud.

**Nota**: HTTP versión 1.X envía solicitudes como texto sin cifrar y utiliza un carácter de nueva línea para separar diferentes campos y diferentes solicitudes. La versión HTTP 2.X, por otro lado, envía solicitudes como datos binarios en forma de diccionario.

## Response HTTP

Una vez que el servidor procesa nuestra solicitud, envía su respuesta. El siguiente es un ejemplo de respuesta HTTP:

![](/assets/04/01.png)

La primera línea de una respuesta HTTP contiene dos campos separados por espacios. El primero es HTTP version (por ejemplo HTTP/1.1), y el segundo denota HTTP response code (por ejemplo 200 OK).

Los códigos de respuesta se utilizan para determinar el estado de la solicitud, como se explicará en una sección posterior. Después de la primera línea, la respuesta enumera sus encabezados, similar a una solicitud HTTP. Tanto los encabezados de solicitud como los de respuesta se analizan en la siguiente sección.

Finalmente, la respuesta puede terminar con un cuerpo de respuesta, que está separado por una nueva línea después de los encabezados. El cuerpo de la respuesta suele definirse como código HTML. Sin embargo, también puede responder con otros tipos de código como recursos JSON del sitio web como imágenes, hojas de estilo o scripts, o incluso un documento como un documento PDF alojado en el servidor web.

## Browser DevTools

La mayoría de los navegadores web modernos vienen con herramientas de desarrollo integradas ( DevTools), que están destinadas principalmente a que los desarrolladores prueben sus aplicaciones web. Sin embargo, como Pentesters web, estas herramientas pueden ser un activo vital en cualquier evaluación web que realicemos, ya que un navegador (y sus DevTools) se encuentran entre los activos que es más probable que tengamos en cada ejercicio de evaluación web. En este módulo, también discutiremos cómo utilizar algunas de las herramientas de desarrollo básicas del navegador para evaluar y monitorear diferentes tipos de solicitudes web.

Cada vez que visitamos un sitio web o accedemos a una aplicación web, nuestro navegador envía múltiples solicitudes web y maneja múltiples respuestas HTTP para representar la vista final que vemos en la ventana del navegador. Para abrir las herramientas de desarrollo del navegador en Chrome o Firefox, podemos hacer clic en [ CTRL+SHIFT+I] o simplemente hacer clic en [ F12]. Las herramientas de desarrollo contienen varias pestañas, cada una de las cuales tiene su propio uso. Nos centraremos principalmente en la Networkpestaña de este módulo, ya que es responsable de las solicitudes web.

Si hacemos clic en la pestaña Red y actualizamos la página, deberíamos poder ver la lista de solicitudes enviadas por la página:

![](/assets/04/02.png)

Como podemos ver, las herramientas de desarrollo nos muestran de un vistazo el estado de la respuesta (es decir, el código de respuesta), el método de solicitud utilizado ( GET), el recurso solicitado (es decir, URL/dominio), junto con la ruta solicitada. Además, podemos utilizarlo Filter URLs para buscar una solicitud específica, en caso de que el sitio web cargue demasiadas para procesarlas.