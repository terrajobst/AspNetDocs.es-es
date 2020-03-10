---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controladores | Microsoft Docs'
author: jongalloway
description: En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 2 cubre los controladores.
ms.author: riande
ms.date: 04/21/2011
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9dc2226f4951d4bed122df37d35bbb94730a00ad
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78451195"
---
# <a name="part-2-controllers"></a>Parte 2: Controladores

por [Jon Galloway](https://github.com/jongalloway)

> El almacén de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> MVC Music Store es una implementación ligera de almacén de ejemplo que vende álbumes musicales en línea e implementa la funcionalidad básica de administración de sitios, Inicio de sesión de usuario y carro de la compra.  
>   
> En esta serie de tutoriales se detallan todos los pasos realizados para compilar la aplicación de ejemplo ASP.NET MVC Music Store. La parte 2 cubre los controladores.

Con los marcos web tradicionales, las direcciones URL de entrada se asignan normalmente a archivos en disco. Por ejemplo: una solicitud para una dirección URL como "/Products.aspx" o "/Products.php" puede ser procesada por un archivo "Products. aspx" o "Products. php".

Los marcos MVC basados en Web asignan direcciones URL al código del servidor de una manera ligeramente diferente. En lugar de asignar las direcciones URL entrantes a los archivos, asignan direcciones URL a métodos en las clases. Estas clases se denominan "controladores" y son responsables del procesamiento de solicitudes HTTP entrantes, el control de la entrada del usuario, la recuperación y el almacenamiento de datos, y la determinación de la respuesta que se devuelve al cliente (mostrar HTML, descargar un archivo, redirigir a otro URL, etc.).

## <a name="adding-a-homecontroller"></a>Agregar un HomeController

Comenzaremos nuestra aplicación MVC Music Store agregando una clase de controlador que controlará las direcciones URL de la Página principal de nuestro sitio. Seguiremos las convenciones de nomenclatura predeterminadas de ASP.NET MVC y lo llamarán HomeController.

Haga clic con el botón secundario en la carpeta "Controllers" dentro del Explorador de soluciones, seleccione "agregar" y, a continuación, el "controlador..." Command

![](mvc-music-store-part-2/_static/image1.jpg)

Se abrirá el cuadro de diálogo "agregar controlador". Asigne al controlador el nombre "HomeController" y presione el botón Agregar.

![](mvc-music-store-part-2/_static/image1.png)

Se creará un nuevo archivo, HomeController.cs, con el código siguiente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Para empezar lo más fácilmente posible, reemplace el método index por un método simple que simplemente devuelva una cadena. Realizaremos dos cambios:

- Cambie el método para que devuelva una cadena en lugar de un ActionResult
- Cambie la instrucción return para que devuelva "Hello From Home"

El método debería tener ahora el siguiente aspecto:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Ejecutar la aplicación

Ahora vamos a ejecutar el sitio. Podemos iniciar nuestro Web-Server y probar el sitio con cualquiera de los siguientes:

- Elija el elemento de menú Depurar ⇨ iniciar depuración
- Haga clic en el botón de flecha verde de la barra de herramientas ![](mvc-music-store-part-2/_static/image2.jpg)
- Use el método abreviado de teclado, F5.

Con cualquiera de los pasos anteriores se compilará el proyecto y, a continuación, se iniciará el Servidor de desarrollo de ASP.NET integrado en Visual Web Developer. Aparecerá una notificación en la esquina inferior de la pantalla para indicar que el Servidor de desarrollo de ASP.NET se ha iniciado y mostrará el número de puerto en el que se está ejecutando.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer abrirá automáticamente una ventana del explorador cuya dirección URL señala a nuestro servidor Web. Esto nos permitirá probar rápidamente nuestra aplicación web:

![](mvc-music-store-part-2/_static/image3.png)

Bien, eso era bastante rápido: hemos creado un nuevo sitio web, hemos agregado una función de tres líneas y hemos incluido texto en un explorador. No se trata de la ciencia, pero es un principio.

*Nota: Visual Web Developer incluye el Servidor de desarrollo de ASP.NET, que ejecutará el sitio web en un número de "puerto" gratuito aleatorio. En la captura de pantalla anterior, el sitio se ejecuta en `http://localhost:26641/`, por lo que usa el puerto 26641. El número de puerto será diferente. Cuando hablemos de la dirección URL como/Store/Browse en este tutorial, irá después del número de puerto. Suponiendo un número de puerto de 26641, la exploración a/Store/Browse significará que se va a `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Adición de un StoreController

Hemos agregado un HomeController sencillo que implementa la Página principal de nuestro sitio. Ahora vamos a agregar otro controlador que usaremos para implementar la funcionalidad de exploración de nuestra tienda de música. Nuestro controlador de tienda será compatible con tres escenarios:

- Página de lista de los géneros musicales de nuestra tienda de música
- Página de exploración en la que se muestran todos los álbumes de música de un género determinado.
- Una página de detalles que muestra información sobre un álbum de música específico

Comenzaremos agregando una nueva clase StoreController.. Si todavía no lo ha hecho, detenga la ejecución de la aplicación; para ello, cierre el explorador o seleccione el elemento de menú Depurar ⇨ detener depuración.

Ahora, agregue un nuevo StoreController. Al igual que hicimos con HomeController, haremos esto haciendo clic con el botón derecho en la carpeta "Controllers" en el Explorador de soluciones y eligiendo el elemento de menú controlador de Add-&gt;.

![](mvc-music-store-part-2/_static/image4.png)

Nuestro nuevo StoreController ya tiene un método "índice". Usaremos este método de "índice" para implementar la página de anuncios en la que se enumeran todos los géneros de nuestra tienda de música. También agregaremos dos métodos adicionales para implementar los otros dos escenarios que queremos que StoreController administre: examinar y detalles.

Estos métodos (índice, exploración y detalles) en nuestro controlador se denominan "acciones del controlador" y, como ya se ha visto con el método de acción HomeController. index (), su trabajo consiste en responder a las solicitudes URL y, por lo general, determinar qué contenido debe volver a enviarse al explorador o al usuario que invocó la dirección URL.

Comenzaremos nuestra implementación de StoreController cambiando el método index () para que devuelva la cadena "Hello from Store. index ()" y agregaremos métodos similares para Browse () y Details ():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Vuelva a ejecutar el proyecto y examine las direcciones URL siguientes:

- /Store
- /Store/Browse
- /Store/Details

El acceso a estas direcciones URL invocará los métodos de acción dentro de nuestro controlador y devolverá respuestas de cadena:

![](mvc-music-store-part-2/_static/image5.png)

Eso es excelente, pero se trata simplemente de cadenas constantes. Vamos a hacerlos dinámicos, de modo que tomen información de la dirección URL y la muestren en el resultado de la página.

En primer lugar, vamos a cambiar el método de acción de exploración para recuperar un valor de cadena de consulta de la dirección URL. Podemos hacer esto agregando un parámetro "género" a nuestro método de acción. Cuando hacemos esto, ASP.NET MVC pasará automáticamente los parámetros QueryString o post de formulario denominados "género" a nuestro método de acción cuando se invoque.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Nota: estamos usando el método de la utilidad HttpUtility. HtmlEncode para corregir la entrada del usuario. Esto impide que los usuarios inserten JavaScript en nuestra vista con un vínculo como/Store/Browse? Genre =&lt;script&gt;Window. Location = 'http://hackersite.com'&lt;/script&gt;.*

Ahora vamos a examinar/Store/Browse? Género = disco

![](mvc-music-store-part-2/_static/image6.png)

A continuación, vamos a cambiar la acción de detalles para leer y mostrar un parámetro de entrada denominado ID. A diferencia de nuestro método anterior, no vamos a insertar el valor de identificador como parámetro QueryString. En su lugar, lo insertaremos directamente dentro de la propia dirección URL. Por ejemplo:/Store/Details/5.

ASP.NET MVC nos permite hacer esto fácilmente sin tener que configurar nada. La Convención de enrutamiento predeterminada de MVC de ASP.NET consiste en tratar el segmento de una dirección URL después del nombre del método de acción como un parámetro denominado "ID". Si el método de acción tiene un parámetro denominado ID, ASP.NET MVC le pasará automáticamente el segmento de dirección URL como parámetro.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Ejecute la aplicación y vaya a/Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Vamos a resumir lo que hemos hecho hasta ahora:

- Hemos creado un nuevo proyecto de ASP.NET MVC en Visual Web Developer
- Hemos analizado la estructura de carpetas básica de una aplicación ASP.NET MVC
- Hemos aprendido a ejecutar nuestro sitio web mediante el Servidor de desarrollo de ASP.NET
- Hemos creado dos clases de controlador: HomeController y StoreController
- Hemos agregado métodos de acción a los controladores que responden a las solicitudes URL y devuelven texto al explorador.

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-1.md)
> [Siguiente](mvc-music-store-part-3.md)
