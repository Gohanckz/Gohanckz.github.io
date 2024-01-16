---
layout: post
title: Headers HTTP
tags: web http burpsuite bugbounty CBBH header
categories: academy 
---

Los Headers HTTP pueden tener uno o varios valores, añadidos después del nombre del encabezado y separados por dos puntos. Podemos dividir las cabeceras en las siguientes categorías:
- General Headers.
- Entity Headers.
- Request Headers.
- Response Headers.
- Security Headers.


## General Headers

Los encabezados generales se utilizan tanto en solicitudes como en respuestas HTTP. Son contextuales y están acostumbrados a describir el mensaje en lugar de su contenido.

| Header | Ejemplo | Descripción |
| ------ | ------ |-------- |
| Date | Date: Wed, 16 Feb 2024 10:38:44 GMT | Contiene la fecha y hora en que se originó el mensaje. Es preferible convertir la hora a la zona horaria UTC estándar .|
| Connection | Connection: close |  Dicta si la conexión de red actual debe permanecer activa después de que finalice la solicitud. Dos valores comúnmente utilizados para este encabezado son close y keep-alive. El valor close del cliente o del servidor significa que les gustaría finalizar la conexión, mientras que el header keep-alive indica que la conexión debe permanecer abierta para recibir más datos y entradas. |

## Entity Headers

De manera similar a los encabezados generales, los encabezados de entidad pueden ser comunes tanto a la solicitud como a la respuesta. Estos encabezados se utilizan para describir el contenido(entidad) transferir mediante un mensaje. Generalmente se encuentran en respuestas y solicitudes POST o PUT.

| Header | Ejemplo | Descripción |
| ------ | ------ |-------- |
| Content-Type | Content-Type: text/html | Se utiliza para describir el tipo de recurso que se transfiere. Los navegadores agregan automáticamente el valor en el lado del cliente y lo devuelven en la respuesta del servidor. El campo charset indica el estándar de codificación, como UTF-8 . |
| Media-Type | Media-Type: application/pdf | media-type es similar a Content-Type y describe los datos que se transfieren. Este encabezado puede desempeñar un papel crucial a la hora de hacer que el servidor interprete nuestra entrada. El campo charset también se puede utilizar con este encabezado. |
| Boundary | boundary="b4e4fbd93540" | Actúa como marcador para separar contenidos cuando hay más de uno en un mismo mensaje. Por ejemplo,  dentro de los datos de un formulario, este límite --b4e4fbd93540 se utiliza para separar diferentes partes del formulario. |
| Content-Length | Content-Length: 385 | Mantiene el tamaño de la entidad que se pasa. Este encabezado es necesario ya que el servidor lo usa para leer datos del cuerpo del mensaje y el navegador y herramientas como cURL lo generan automáticamente. |
| Content-Encoding | Content-Encoding: gzip | Los datos pueden sufrir múltiples transformaciones antes de pasarse. Por ejemplo, se pueden comprimir grandes cantidades de datos para reducir el tamaño del mensaje. El tipo de codificación que se utiliza debe especificarse mediante el Header Content-Encoding. |

## Requests Headers

El cliente envía encabezados de solicitud en una transacción HTTP. Estos encabezados son utilizados en una solicitud HTTP y no se relaciona con el contenido. del mensaje. Los siguientes encabezados se ven comúnmente en solicitudes HTTP.

| Header | Ejemplo | Descripción |
| ------ | ------ |-------- |
|  Host | Host: www.inlanefreight.com | Se utiliza para especificar el host al que se consulta el recurso. Puede ser un nombre de dominio o una dirección IP. Los servidores HTTP se pueden configurar para alojar diferentes sitios web, que se revelan según el nombre del host. Esto hace que el encabezado del host sea un objetivo de enumeración importante, ya que puede indicar la existencia de otros hosts en el servidor de destino. |
| User-Agent | User-Agent: curl/7.77.0 | El Header User-Agent se utiliza para describir el cliente que solicita recursos. Este encabezado puede revelar mucho sobre el cliente, como el navegador, su versión y el sistema operativo. |
| Referer | Referer: http://www.inlanefreight.com/ | Indica de dónde proviene la solicitud actual. Por ejemplo, hacer clic en un enlace de los resultados de búsqueda de Google crearía el referente https://google.com. Confiar en este encabezado puede ser peligroso, ya que puede manipularse fácilmente y tener consecuencias no deseadas. |
| Accept | Accept *\/* | El Header Accept describe qué tipos de medios puede comprender el cliente. Puede contener varios tipos de medios separados por comas. El  valor */* significa que se aceptan todos los tipos de medios. |
| Cookie | Cookie: PHPSESSID=b4e4fbd93540 | Contiene pares de valor de cookie en el formato name=value. Una cookie es un dato almacenado en el lado del cliente y en el servidor, que actúa como un identificador. Estos se pasan al servidor por solicitud, manteniendo así el acceso del cliente. Las cookies también pueden tener otros fines, como guardar las preferencias del usuario o el seguimiento de la sesión. Puede haber varias cookies en un único encabezado separadas por un punto y coma. |
| Authorization | Authorization: BASIC cGFzc3dvcmQK | Otro método para que el servidor identifique clientes. Después de una autenticación exitosa, el servidor devuelve un token exclusivo para el cliente. A diferencia de las cookies, los tokens se almacenan únicamente en el lado del cliente y el servidor los recupera por solicitud. Existen varios tipos de autenticación según el servidor web y el tipo de aplicación utilizado. |

Puede encontrar una lista completa de encabezados de solicitud y su uso [aqui](https://datatracker.ietf.org/doc/html/rfc7231#section-5) .


## Response Header

Los encabezados de respuesta pueden ser se utilizados en una respuesta HTTP y no se relacionan con el contenido. Ciertos encabezados de respuesta, como Age, Locationy, Serverse utilizan para proporcionar más contexto sobre la respuesta. Los siguientes encabezados se ven comúnmente en las respuestas HTTP.

| Header | Ejemplo | Descripción |
| ------ | ------ |-------- |
| Server | Server: Apache/2.2.14 (Win32) | Contiene información sobre el servidor HTTP que procesó la solicitud. Se puede utilizar para obtener información sobre el servidor, como su versión, y enumerarla más. |
| Set-Cookie | Set-Cookie: PHPSESSID=b4e4fbd93540 | Contiene las cookies necesarias para la identificación del cliente. Los navegadores analizan las cookies y las almacenan para futuras solicitudes. Este encabezado sigue el mismo formato que el Header Cookiee de la solicitud. |
| WWW-Authenticate | WWW-Authenticate: BASIC realm="localhost" | Notifica al cliente sobre el tipo de autenticación requerida para acceder al recurso solicitado. |


## Security Header

Finalmente, tenemos encabezados de seguridad . Con el aumento de la variedad de navegadores y ataques basados ​​en web, era necesario definir ciertos encabezados que mejoraran la seguridad. una clase de encabezados de respuesta utilizados para especificar ciertas reglas y políticas El navegador debe seguir los encabezados de seguridad HTTP al acceder al sitio web.

| Header | Ejemplo | Descripción |
| ------ | ------ |-------- |
| Content-Security-Policy | Content-Security-Policy: script-src 'self' | Dicta la política del sitio web hacia los recursos inyectados externamente. Podría tratarse de código JavaScript o de recursos de script. Este encabezado indica al navegador que acepte recursos solo de ciertos dominios confiables, evitando así ataques como secuencias de comandos entre sitios (XSS) . |
| Strict-Transport-Security | Strict-Transport-Security: max-age=31536000 | Impide que el navegador acceda al sitio web a través del protocolo HTTP de texto sin formato y obliga a que todas las comunicaciones se realicen a través del protocolo HTTPS seguro. Esto evita que los atacantes rastreen el tráfico web y accedan a información protegida, como contraseñas u otros datos confidenciales. |
| Referrer-Policy | Referrer-Policy: origin | Dicta si el navegador debe incluir el valor especificado a través del Header Referer o no. Puede ayudar a evitar la divulgación de información y URL confidenciales mientras navega por el sitio web. |