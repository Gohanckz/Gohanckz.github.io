---
layout: post
title: Protocolo de transferencia de hipertexto (HTTP)
tags: web http burpsuite bugbounty
categories: academy 
---


La comunicación HTTP consta de un cliente y un servidor, donde el cliente solicita un recurso al servidor. El servidor procesa las solicitudes y devuelve el recurso solicitado. El puerto predeterminado para la comunicación HTTP es el puerto 80, aunque se puede cambiar a cualquier otro puerto, dependiendo de la configuración del servidor web. Se utilizan las mismas solicitudes cuando utilizamos Internet para visitar diferentes sitios web. Ingresamos un Fully Qualified Domain Name ( FQDN) como Uniform Resource Locator( URL) para llegar al sitio web deseado, como www.hackthebox.com .


## URL

Se accede a los recursos a través de HTTP a través de un URL, que ofrece muchas más especificacioens que simplemente especificar un sitio web que queremos visitar. Veamos la estructura de una URL:

![](/assets/03/00.png)

Esto es lo que represneta cada componente:

| Componente | Ejemplo | Descripción |
| -----------|---------|-------------|
| Scheme | http:// https:// | Esto se utiliza para identificar el protocolo al que accede el cliente y termina con dos puntos y una doble barra ( ://). |
| User info | admin:password@ | Este es un componente opcional que contiene las credenciales (separadas por dos puntos :) utilizadas para autenticarse en el host y está separado del host con un signo de arroba ( @). |
| Host | inlanefreight.com | El host indica la ubicación del recurso. Puede ser un nombre de host o una dirección IP. |
| Port | :80 | El Portestá separado del Hostpor dos puntos ( :). Si no se especifica ningún puerto, httplos esquemas utilizan de forma predeterminada el puerto 80y httpsel puerto de forma predeterminada443 |
| Path | /dashboard.php | Esto apunta al recurso al que se accede, que puede ser un archivo o una carpeta. Si no se especifica ninguna ruta, el servidor devuelve el índice predeterminado (por ejemplo index.html). |
| Query String | ?login=true | La cadena de consulta comienza con un signo de interrogación ( ?) y consta de un parámetro (p. ej. login) y un valor (p. ej. true). Se pueden separar varios parámetros mediante un signo comercial ( &). |
| Fragments | #status | Los navegadores del lado del cliente procesan los fragmentos para localizar secciones dentro del recurso principal (por ejemplo, un encabezado o sección en la página). |

No todos los componentes son necesarios para acceder a un recurso. Los principales campos obligatorios son el esquema y el host, sin los cuales la solicitud no tendría recurso para solicitar.

## cURL

En este módulo, enviaremos solicitudes web a través de dos de las herramientas más importantes para cualquier evaluador de penetración web, un navegador web, como Chrome o Firefox, y la herramienta cURL de línea de comandos.

cURL (URL del cliente) es una herramienta y biblioteca de línea de comandos que admite principalmente HTTP junto con muchos otros protocolos. Esto lo convierte en un buen candidato para scripts y automatización, lo que lo hace esencial para enviar varios tipos de solicitudes web desde la línea de comandos, lo cual es necesario para muchos tipos de pruebas de penetración web.

- Solicitud HTTP básica

~~~ bash
curl inlanefreight.com

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
...SNIP...
~~~

Vemos que cURL no representa el código HTML/JavaScript/CSS, a diferencia de un navegador web, sino que lo imprime en su formato sin formato. Sin embargo, como Pentesters, estamos interesados ​​principalmente en el contexto de solicitud y respuesta, que generalmente resulta mucho más rápido y conveniente que un navegador web.

También podemos usar cURL para descargar una página o un archivo y generar el contenido en un archivo usando la Flag -O. Si queremos especificar el nombre del archivo de salida, podemos usar la Flag -o y especificar el nombre. De lo contrario, podemos usar -O y cURL usará el nombre del archivo remoto, de la siguiente manera:

~~~ bash
curl -O inlanefreight.com/index.html

# output
index.html
~~~


| **Command** | **Description** |
| --------------|-------------------|
| curl -h | cURL help menu |
| curl inlanefreight.com | Basic GET request |
| curl -s -O inlanefreight.com/index.html | Download file |
| curl -k https://inlanefreight.com | Skip HTTPS (SSL) certificate validation |
| curl inlanefreight.com -v | Print full HTTP request/response details |
| curl -I https://www.inlanefreight.com | Send HEAD request (only prints response headers) |
| curl -i https://www.inlanefreight.com | Print response headers and response body |
| curl https://www.inlanefreight.com -A 'Mozilla/5.0' | Set User-Agent header |
| curl -u admin:admin http://<SERVER_IP>:<PORT>/ | Set HTTP basic authorization credentials |
| curl  http://admin:admin@<SERVER_IP>:<PORT>/ | Pass HTTP basic authorization credentials in the URL |
| curl -H 'Authorization: Basic YWRtaW46YWRtaW4=' http://<SERVER_IP>:<PORT>/ | Set request header |
| curl 'http://<SERVER_IP>:<PORT>/search.php?search=le'` | Pass GET parameters |
| curl -X POST -d 'username=admin&password=admin' http://<SERVER_IP>:<PORT>/ | Send POST request with POST data |
| curl -b 'PHPSESSID=c1nsa6op7vtk7kdis7bcnbadf1' http://<SERVER_IP>:<PORT>/ | Set request cookies |
|curl -X POST -d '{"search":"london"}' -H 'Content-Type: application/json' http://<SERVER_IP>:<PORT>/search.php | Send POST request with JSON data |

## APIs

| Command | Description |
| ---------- | ---------- |
| curl http://<SERVER_IP>:<PORT>/api.php/city/london | Read entry |
| curl -s http://<SERVER_IP>:<PORT>/api.php/city/ \| jq | Read all entries |
| curl -X POST http://<SERVER_IP>:<PORT>/api.php/city/ -d '{"city_name":"HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json' | Create (add) entry |
| curl -X PUT http://<SERVER_IP>:<PORT>/api.php/city/london -d '{"city_name":"New_HTB_City", "country_name":"HTB"}' -H 'Content-Type: application/json' | Update (modify) entry |
| curl -X DELETE http://<SERVER_IP>:<PORT>/api.php/city/New_HTB_City | Delete entry |

## Browser DevTools

| **Shortcut** | **Description** |
| --------------|-------------------|
| [`CTRL+SHIFT+I`] or [`F12`] | Show devtools |
| [`CTRL+SHIFT+E`] | Show Network tab |
| [`CTRL+SHIFT+K`] | Show Console tab |