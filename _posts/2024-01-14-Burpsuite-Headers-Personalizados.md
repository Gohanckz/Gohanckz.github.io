---
layout: post
title: Burpsuite - Headers Personalizados
tags: web tool burpsuite bugbounty
categories: tools 
---

Para agregar Headers Personalizados en cada uno de nuestros requests de forma automatizada debemos realizar los siguientes pasos:

1. Abrimos nuestro Burpsuite.

![](/assets/01/00.png)

2. Nos dirigimos a Settings y desde la categoría "Proxy" debemos bajar hasta "Match and replace rules".

![](/assets/01/01.png)

3. Hacemos click en "add", seleccionamos el tipo de Header para agregar (En esta ocasión se utilizó Request header) y en el apartado "Repleace" agregamos nuestro Header personalizado.

![](/assets/01/02.png)

4. Ahora cada vez que hagamos un nuevo request, nuestro Header personalizado irá incluido.

![](/assets/01/03.png)