---
layout: post
title: HTML Injection
tags: web HTMLi burpsuite bugbounty CBBH PoC
categories: vulnerabilities 
---

La inyección de HTML se produce cuando la entrada del usuario sin filtrar se muestra en la página. Esto puede realizarse recuperando el código enviado previamente, como recuperar un comentario de usuario de la base de datos del back-end, o mostrando directamente la entrada del usuario sin filtrar en el front-end JavaScript.

Cuando un usuario tiene control total de cómo se mostrará su entrada, puede enviar código HTML y el navegador puede mostrarlo como parte de la página. Esto puede incluir un código malicioso HTML, como un formulario de inicio de sesión externo, que puede usarse para engañar a los usuarios para que inicien sesión mientras en realidad envían sus credenciales de inicio de sesión a un servidor malicioso para que las recopile para otros ataques.

Otro ejemplo HTML Injectiones la desfiguración de páginas web. Consiste en inyectar nuevo código HTML para cambiar la apariencia de la página web, insertar anuncios maliciosos o incluso cambiar completamente la página. Este tipo de ataque puede provocar graves daños a la reputación de la empresa que aloja la aplicación web.

## PoC HTML Injection

El siguiente ejemplo es una página web muy básica con un solo botón " Click to enter your name." Cuando hacemos clic en el botón, nos solicita que ingresemos nuestro nombre y luego muestra nuestro nombre como " Your name is ...":

![](/assets/08/00.png)

Si no se implementa una desinfección de los insumos, este es potencialmente un blanco fácil para ataques HTML Injection y Cross-Site Scripting (XSS). Echamos un vistazo al código fuente de la página y no vemos ningún saneamiento de entrada, ya que la página toma la entrada del usuario y la muestra directamente:

~~~ bash
<!DOCTYPE html>
<html>

<body>
    <button onclick="inputFunction()">Click to enter your name</button>
    <p id="output"></p>

    <script>
        function inputFunction() {
            var input = prompt("Please enter your name", "");

            if (input != null) {
                document.getElementById("output").innerHTML = "Your name is " + input;
            }
        }
    </script>
</body>

</html>
~~~

Para probar HTML Injection, simplemente podemos ingresar un pequeño fragmento de código HTML como nuestro nombre y ver si se muestra como parte de la página. Probaremos el siguiente código, que cambia la imagen de fondo de la página web:

~~~ html
<style> body { background-image: url('https://academy.hackthebox.com/images/logo.svg'); } </style>
~~~

![](/assets/08/01.png)

**Nota**:En este ejemplo, como todo se lleva a cabo en el front-end, actualizar la página web restablecería todo a la normalidad.