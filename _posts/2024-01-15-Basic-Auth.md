---
layout: post
title: Basic Auth HTTP
tags: web http burpsuite bugbounty CBBH MethodsHTTP BasicAuth
categories: academy 
---

Cuando visitamos el ejercicio que se encuentra al final de esta sección, nos solicita que ingresemos un nombre de usuario y una contraseña. A diferencia de los formularios de inicio de sesión habituales, que utilizan parámetros HTTP para validar las credenciales del usuario (por ejemplo, solicitud POST), este tipo de autenticación utiliza un archivo basic HTTP authentication, que es manejado directamente por el servidor web para proteger una página/directorio específico, sin interactuar directamente con la aplicación web. .

Para acceder a la página, debemos ingresar un par de credenciales válidas, que son admin: admin en este caso:

![](/assets/05/00.png)

Una vez que ingresemos las credenciales, obtendremos acceso a la página:

![](/assets/05/01.png)

Intentemos acceder a la página con cURL y agregaremos -i para ver los encabezados de respuesta:

![](/assets/05/02.png)

Como podemos ver, llegamos Access denied al cuerpo de la respuesta y también Basic realm="Access denied" al WWW-Authenticateencabezado, lo que confirma que esta página efectivamente usa basic HTTP auth, como se analiza en la sección Encabezados. Para proporcionar las credenciales a través de cURL, podemos usar la flag -u, de la siguiente manera:

![](/assets/05/03.png)

Esta vez obtenemos la página en la respuesta. Existe otro método por el que podemos proporcionar las basic HTTP auth credenciales, que es directamente a través de la URL como ( username:password@URL), como comentamos en la primera sección. Si intentamos lo mismo con cURL o nuestro navegador, también obtendremos acceso a la página:

![](/assets/05/04.png)

## Encabezado de autorización HTTP

Si agregamos la Flag -v a cualquiera de nuestros comandos cURL anteriores:

![](/assets/05/05.png)

Mientras usamos basic HTTP auth, vemos que nuestra solicitud HTTP establece el Header Authorization en Basic YWRtaW46YWRtaW4=, que es el valor codificado en base64 de admin:admin. Si estuviéramos usando un método moderno de autenticación (por ejemplo JWT), Authorizationsería de tipo Bearery contendría un token cifrado más largo.

Intentemos configurar manualmente Authorization, sin proporcionar las credenciales, para ver si nos permite acceder a la página. Podemos configurar el Header con la Flag -H y usaremos el mismo valor de la solicitud HTTP anterior. Podemos agregar la Flag -H varias veces para especificar múltiples encabezados:

![](/assets/05/06.png)

Como vemos, esto también nos dio acceso a la página. Estos son algunos métodos que podemos utilizar para autenticarnos en la página. La mayoría de las aplicaciones web modernas utilizan formularios de inicio de sesión creados con el lenguaje de secuencias de comandos back-end (por ejemplo, PHP), que utilizan solicitudes HTTP POST para autenticar a los usuarios y luego devolver una cookie para mantener su autenticación.

