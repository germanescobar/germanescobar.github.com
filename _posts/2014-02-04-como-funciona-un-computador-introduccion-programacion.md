---
layout: post
title: ¿Cómo funciona y qué puede hacer un computador? Una introducción a la programación
---

# ¿Cómo funciona y qué puede hacer un computador[^1]? Una introducción a la programación

Este post es una introducción para aquellas personas que saben cómo usar un computador pero quieren entender un poco más cómo funciona. Y aunque los computadores pueden parecer muy complejos, la realidad es que trabajan con unos patrones muy simples que nos van a permitir entender qué puede y qué no puede hacer un computador.

El objetivo de los computadores, en sus inicios, era **realizar cálculos matemáticos** (sí, la gente los hacía a mano!). Los primeros computadores no tenían pantallas, todo se tenía que imprimir. Pero eventualmente aparecieron las pantallas, primero con una línea de comandos (la consola) y luego con las interfaces gráficas que hoy conocemos.

Pero además de hacer cálculos matemáticos, los computadores evolucionaron en un modo que fue posible **almacenar información persistente**, es decir, información que sobrevive los reinicios del computador.

Hoy también es posible **transmitir información de un computador a otro** (p.e. a través de Internet). 

En resumen, los computadores pueden:
1. Hacer cálculos matemáticos.
2. Recibir y mostrar información (p.e. recibir información de un teclado y mostrarla en la pantalla o en una impresora).
3. Almacenar información.
4. Enviar y recibir información a otro computador.

Los últimos 3 puntos se pueden consolidar en uno solo: **realizar operaciones de entrada y salida (E/S), en inglés input y output (I/O)**. Para un computador da igual que la información llegue de un teclado, un sensor, una memoria USB, o a través de una red. También da lo mismo que la información sea enviada a la pantalla, a una impresora, a una memoria USB, o a una red. Para un computador todos son dispositivos de entrada y salida. Por otro lado, a las aplicaciones (el software) sí le interesa saber de dónde viene y hacia dónde va la información.

Veamos un ejemplo concreto. Cuando conectamos nuestra cámara digital a través de USB, generalmente también tenemos que instalar un software que sabe interpretar la información que viene de la cámara, y sabe cómo enviar información que la cámara entienda[^2]. 

De nuevo, al computador no le importa qué conectemos en los puertos USB (o en cualquier otro puerto de I/O), al software sí.

La programación nos permite orquestar las dos acciones que acabamos de mencionar (hacer cálculos y operaciones I/O) para crear aplicaciones que generen valor, ya sea para automatizar procesos, organizar/visualizar información, o mejorar la comunicación, entre otros.


## El procesador (CPU)

El procesador es un componente físico que ejecuta las instrucciones de un programa, ya sea haciendo cálculos u operaciones de I/O. Entre más rápido es el procesador, mayor cantidad de operaciones puede hacer en un determinado tiempo.


## Disco duro y memoria RAM

Existen dos áreas de almacenamiento en un computador. La primera es el disco duro que nos permite almacenar información de forma persistente. La segunda es la memoria RAM, que es mucho más rápida de acceder que el disco duro pero es volátil, no sobrevive los reinicios del computador.

Uno de los mayores retos de los programadores ha sido encontrar formas para almacenar la información de modo eficiente para que sea fácil de encontrar y ocupe el menor espacio en disco.

Supongamos que vamos a hacer un sistema que almacena los pedidos de una empresa. ¿Cómo y dónde almacenamos esa información? Una opción es crear un archivo de texto llamado <code>pedidos.txt</code> y guardar, en cada línea del archivo, la información de un pedido incluyendo los datos completos del cliente como se muestra a continuación:

```
1,Pedro Perez,Cll 2 # 4 - 23,5993377,3 rosas,23000,1
2,Arturo Paez,Cra 1 # 8 - 65,5993665,8 rosas,42000,0
3,Pedro Perez,Cll 2 # 4 - 23,5993377,7 rosas,35000,1
```

Lo primero que podemos observar en el ejemplo es que la información está separada por comas (podríamos haber utilizado cualquier otro carácter como * o -). Es responsabilidad de nuestra aplicación entender qué significa cada columna (p.e. que la tercera columna es la dirección y que en la última, uno significa "pagado" y cero "sin pagar").

El problema de guardar toda la información en el mismo archivo es que si un mismo cliente hace varios pedidos, su información se repetiría varias veces, como ocurre en el ejemplo anterior en el primer y tercer pedido. Una posible solución sería crear otro archivo llamado <code>clientes.txt</code> y guardar allí la información de los clientes. Cada línea en <code>clientes.txt</code> tendría un número de identificación único que utilizaríamos como referencia en el archivo de <code>pedidos.txt</code>.

Nuestro ejemplo se puede volver cada vez más complejo: ¿qué pasa si un pedido se debe dividir en varios productos? ¿qué pasa si queremos almacenar la cédula de los clientes? ¿qué pasa si nos piden un informe que muestre el total de ventas por cliente?

Almacenar una base de datos (como la que acabamos de describir) en archivos de texto es incómodo y a medida que los archivos crecen, buscar la información se vuelve cada vez más difícil y lento. Afortunadamente, existen unas aplicaciones llamadas **motores de bases de datos** que nos permiten organizar la información en tablas, crear relaciones entre esas tablas y encontrar información rápidamente. Además, muchos motores de bases de datos son gratis. [MySQL](http://www.mysql.com/) y [PostgreSQL](http://www.postgresql.org/) son dos ejemplos que son ampliamente usados por empresas como Google y Facebook. En [elibom.com](http://www.elibom.com/) utilizamos [MySQL](http://www.mysql.com/).

Aprender a manejar un motor de base de datos es importante cuando queremos empezar a crear nuestras propias aplicaciones. Sin embargo, no olvidemos que, en ocasiones, un archivo de texto es todo lo que necesitamos.


## Transmisión de datos entre computadores

Aunque existen muchas aplicaciones que crean valor sin necesidad de comunicarse con otros computadores, las redes han creado nuevas posibilidades que no tendrían sentido de otra forma: chats, juegos en línea, compras físicas y digitales, sistemas de información en línea de aerolíneas, bancos, aseguradoras, etc., son solo algunos ejemplos.

Sin embargo, esas posibilidades también crean retos para los programadores. Ya no es suficiente almacenar y visualizar información en un computador, ahora es necesario aprender cómo intercambiar información con otros computadores.

Una de las primeras aplicaciones de las redes fue el intercambio de mensajes, conocido como correo electrónico, que sigue siendo muy popular. Otra aplicación fue la de publicar documentos de texto que otras personas podían consultar a través de un navegador, conocido como la World Wide Web (WWW). No pasó mucho tiempo para que fuera posible también incluir imágenes y video en los documentos. Ahora es posible crear documentos dinámicos que cambian cada vez que alguien los accede.

Desafortunadamente, muchas personas creen que Internet es solo la WWW (acceder a documentos a través de un navegador). Pero Internet es mucho más que eso. Pensemos por un momento en algunas de las aplicaciones que tenemos en nuestros teléfonos que utilizan Internet sin un navegador: Whatsapp, Waze, Spotify, Dropbox, etc. Esas aplicaciones también están haciendo uso de Internet para conectarse con otros computadores y transmitir información.

Dos tecnologías sobresalen cuando hablamos de Internet: HTTP y HTML. Cuando ingresamos a través de un navegador a una página como facebook.com, eltiempo.com, etc., estamos utilizando las dos tecnologías. HTTP se utiliza cuando digitas "http://facebook.com/". (En muchos navegadores ya no es necesario digitar la dirección completa, es decir, podemos digitar "facebook.com" y el navegador automáticamente agrega el "http://", pero es importante entender que igual se necesita ese prefijo para que los demás computadores de la red entiendan que el protocolo que se está utilizando es HTTP).

Cuando Facebook le responde al navegador, en el cuerpo del mensaje HTTP llega un documento HTML, que el navegador interpreta y muestra. Si pudiéramos ver lo que el navegador recibe cuando accedemos a una página, veríamos algo parecido a lo siguiente:

```
HTTP/1.1 200 OK
Content-Type: text/html; charset=utf-8
Date: ...

<html>
  <head>
    <title>...</title>
  </head>
  <body>
    ...
  </body>
</html>
```

Analicemos la respuesta. La primera línea le está diciendo al navegador que están utilizando el protocolo HTTP y que todo salió bien (OK). Después hay unos encabezados que le dicen que el contenido que viene en la respuesta es HTML (text/html para ser exactos). Eso quiere decir que después de la línea en blanco viene un documento HTML y que el navegador lo tiene que interpretar y presentárselo al usuario.

¿Podemos recibir otros tipos de documentos por HTTP que no sean HTML? ¡Claro! Podemos recibir imágenes, vídeo, audio, archivos, etc. (en estos casos algunos navegadores nos piden confirmar en dónde queremos guardar el archivo, otros intentan mostrar el archivo en el mismo navegador). También podemos recibir información plana y en ese caso el explorador la muestra como viene, sin interpretarla.

Para resumir, HTTP es un protocolo que nos permite solicitar documentos a otro computador y HTML es un tipo de documento con una estructura que el navegador interpreta y nos presenta con encabezados, colores y demás adornos que ahora tienen las páginas Web.

Para entender cómo es que el navegador interpreta HTML, podemos realizar el siguiente ejercicio. Abre un editor de texto (p.e. bloc de notas) y pega el código HTML que se presenta a continuación en el archivo. Finalmente, guárdalo con el nombre <code>prueba.html</code> en alguna carpeta que puedas encontrar fácilmente.

```
<html>
  <head>
    <title>Un titulo</title>
  </head>
  <body>
    <h1>Que hago hoy?</h1>
    <img src="http://bit.ly/1am6z8s" width="200px">
  </body>
</html>
```
Abre la carpeta en la que quedó guardado el documento y haz doble click sobre él para abrirlo. Se deberá abrir un navegador y mostrar un título y una imagen.

Aprender lo básico de HTML es importante si queremos desarrollar páginas y aplicaciones Web. Un buen recurso para empezar es el [tutorial de W3Schools](http://www.w3schools.com/html/default.asp). Más adelante, con programación, podemos hacer que esas páginas HTML muestren información dinámica, por ejemplo para listar los pedidos que hicimos en la sección anterior.

Un último componente que es importante entender si queremos crear nuestras aplicaciones Web es cómo funciona un **Servidor Web**.

Un Servidor Web es una aplicación que se instala en un computador y que recibe todas las peticiones HTTP que llegan por la red. Existe una carpeta en el computador (la ubicación de la carpeta la determina el servidor Web) en donde podemos almacenar todos los archivos (HTML, imágenes, video) que queremos publicar en la Web. Cada vez que el servidor Web recibe una petición HTTP, verifica si ese archivo solicitado existe en la carpeta. Si es así, lo devuelve en la respuesta del HTTP. De lo contrario, devuelve un error de "página no encontrada". Con programación, también es posible devolver archivos generados dinámicamente (que cambian cada vez que son solicitados).

Ejemplos de servidores Web incluyen [Apache](http://httpd.apache.org/) y [nginx](http://wiki.nginx.org/Main). Mi recomendación para empezar sería [Apache](http://httpd.apache.org/)[^3].

---

Espero que esta explicación les de una idea general de lo que puede hacer un computador. Recuerden que los computadores pueden recibir información de diferentes medios (el teclado, la red, la cámara, etc.), procesar u organizar esa información, almacenarla, mostrarla en la pantalla, imprimirla, o publicarla a través de Internet. ¿Qué aplicación podrían construir entendiendo eso?

En un próximo post voy a mostrar cómo diseñar una aplicación, es decir, cómo aterrizar la idea.


[^1]: El español, o castellano, le asigna  a cada sustantivo un género: "la casa" o "el lápiz". Eso ha creado una confusión con la traducción de la palabra "computer", que no tiene genero. En Colombia lo llamamos el computador, en el resto de Latinoamérica  la computadora. En España es el ordenador.

[^2]: Con las cámaras más recientes ya no es necesario instalar ningún software porque los fabricantes se dieron cuenta que las cámaras no funcionaban muy diferente a una memoria USB.

[^3]: Esto no significa que uno vaya a alojar sus páginas web en su propio computador, para eso existen los servicios de hosting. Pero igual es bueno saber instalar y probar un servidor Web en nuestro propio computador.