---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
title: Agregar una vista (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: d3633f64-5d3c-45c9-ae4b-cb1563e3739f
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: fa200935d83bb26c07b302449a6eba6fd67b5322
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434521"
---
# <a name="adding-a-view-vb"></a>Agregar una vista (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de VB.NET está disponible para acompañar este tema. [Descargue la versión de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [ C# versión](../cs/adding-a-view.md) de este tutorial.

En esta sección vamos a modificar la clase `HelloWorldController` para que use un archivo de plantilla de vista para encapsular correctamente el proceso de generación de respuestas HTML a un cliente.

Comencemos usando una plantilla de vista con el método `Index` en la clase `HelloWorldController`. Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida dentro de la clase de controlador. Cambie el método de `Index` para que devuelva un objeto `View`, como se muestra a continuación:

[!code-vb[Main](adding-a-view/samples/sample1.vb)]

Ahora vamos a agregar una plantilla de vista a nuestro proyecto que se puede invocar con el método `Index`. Para ello, haga clic con el botón secundario dentro del método `Index` y haga clic en **Agregar vista**.

[![IndexAddView](adding-a-view/_static/image2.png "IndexAddView")](adding-a-view/_static/image1.png)

Aparece el cuadro de diálogo **Agregar vista** . Deje las entradas predeterminadas y haga clic en el botón **Agregar** .

[![3addView](adding-a-view/_static/image4.png "3addView")](adding-a-view/_static/image3.png)

Se crean la carpeta *MvcMovie\Views\HelloWorld* y el archivo *MvcMovie\Views\HelloWorld\Index.vbhtml* . Puede verlos en **Explorador de soluciones**:

[![SolnExpHelloWorldIndx](adding-a-view/_static/image6.png "SolnExpHelloWorldIndx")](adding-a-view/_static/image5.png)

Agregue cierto código HTML bajo la etiqueta `<h2>`. A continuación se muestra el archivo *MvcMovie\Views\HelloWorld\Index.vbhtml* modificado.

[!code-vbhtml[Main](adding-a-view/samples/sample2.vbhtml)]

Ejecute la aplicación y vaya al controlador de&quot; de &quot;Hola mundo (`http://localhost:xxxx/HelloWorld`). El método `Index` del controlador no hace mucho trabajo; simplemente ejecutó la instrucción `return View()`, lo que indica que queríamos usar un archivo de plantilla de vista para presentar una respuesta al cliente. Dado que no especificamos explícitamente el nombre del archivo de plantilla de vista que se va a usar, ASP.NET MVC tenía como valor predeterminado el archivo de vista *index. vbhtml* dentro de la carpeta *\Views\HelloWorld* . En la imagen siguiente se muestra la cadena codificada de forma rígida en la vista.

[![3HelloWorld](adding-a-view/_static/image8.png "3HelloWorld")](adding-a-view/_static/image7.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que la barra de título del explorador indica &quot;&quot; de índice y el título grande de la página indica &quot;mi aplicación MVC.&quot; vamos a cambiarlas.

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, vamos a cambiar el texto &quot;mi aplicación MVC.&quot; que el texto se comparta y aparezca en todas las páginas. Realmente aparece solo en un lugar de nuestro proyecto, aunque esté en cada página de la aplicación. Vaya a la carpeta */views/Shared* en **Explorador de soluciones** y abra el archivo *\_layout. vbhtml* . Este archivo se denomina página de diseño y es el shell de &quot;compartido&quot; que usan todas las demás páginas.

Observe la línea de código `@RenderBody()` situada cerca de la parte inferior del archivo. `RenderBody` es un marcador de posición en el que se muestran todas las páginas que se crean, &quot;&quot; ajustadas en la página de diseño. Cambie el encabezado `<h1>` de **&quot;** &quot; de aplicación mvc a &quot;aplicación de películas MVC&quot;.

[!code-html[Main](adding-a-view/samples/sample3.html)]

Ejecute la aplicación y observe que ahora dice &quot;aplicación de películas MVC&quot;. Haga clic en el vínculo **About (acerca** de) y esa página muestra también &quot;aplicación de películas MVC&quot;.

A continuación se muestra el archivo completo *\_layout. vbhtml* :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Ahora, vamos a cambiar el título de la página de índice (ver).

[!code-vbhtml[Main](adding-a-view/samples/sample5.vbhtml)]

Abra *MvcMovie\Views\HelloWorld\Index.vbhtml*. Hay dos lugares para hacer un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el elemento `<h2>`). Haremos ligeramente diferentes para que pueda ver qué fragmento de código cambia la parte de la aplicación.

Ejecute la aplicación y vaya a`http://localhost:xx/HelloWorld`. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. Es fácil realizar cambios importantes en la aplicación con pequeños cambios en una vista. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor).

[![3_MyMovieList](adding-a-view/_static/image10.png "3_MyMovieList")](adding-a-view/_static/image9.png)

No obstante, nuestro pequeño bit de&quot; de datos de &quot;(en este caso, el &quot;mensaje Hola mundo!&quot;) está codificado de forma rígida. Nuestra aplicación MVC tiene V (views) y tenemos C (controladores), pero todavía no hay M (modelo). En breve, veremos cómo crear una base de datos y recuperar los datos del modelo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Sin embargo, antes de ir a una base de datos y hablar acerca de los modelos, vamos a hablar primero sobre cómo pasar información desde el controlador a una vista. Queremos pasar lo que requiere una plantilla de vista para presentar una respuesta HTML a un cliente. Normalmente, estos objetos se crean y pasan por una clase de controlador a una plantilla de vista, y solo deben contener los datos que requiere la plantilla de vista, y no más.

Anteriormente, con la clase `HelloWorldController`, el método de acción `Welcome` tomó un `name` y un parámetro `numTimes` y, a continuación, generaba los valores de los parámetros en el explorador. En lugar de hacer que el controlador siga representando esta respuesta directamente, vamos a colocar los datos en un contenedor para la vista. Los controladores y las vistas pueden utilizar un `ViewBag` objeto para almacenar los datos. Que se pasará a una plantilla de vista automáticamente y se usará para representar la respuesta HTML usando el contenido del contenedor como datos. De este modo, el controlador se preocupa de una cosa y de la plantilla de vista con otra, lo que nos permite mantener limpio &quot;la separación de preocupaciones&quot; dentro de la aplicación.

Como alternativa, podríamos definir una clase personalizada y, a continuación, crear una instancia de ese objeto en nuestro propio, rellenarla con datos y pasarla a la vista. Esto se suele denominar ViewModel, ya que es un modelo personalizado para la vista. Sin embargo, para pequeñas cantidades de datos, ViewBag funciona bien.

Vuelva al archivo *HelloWorldController. VB* cambie el método `Welcome` dentro del controlador para colocar el mensaje y NumTimes en ViewBag. ViewBag es un objeto dinámico. Esto significa que puede colocar todo lo que desee en él. ViewBag no tiene ninguna propiedad definida hasta que coloque algo dentro de ella.

El `HelloWorldController.vb` completo con la nueva clase en el mismo archivo.

[!code-vb[Main](adding-a-view/samples/sample6.vb)]

Ahora nuestro ViewBag contiene datos que se pasarán a la vista automáticamente. De nuevo, también podríamos haber pasado nuestro propio objeto, como esto, si le gusta:

[!code-csharp[Main](adding-a-view/samples/sample7.cs)]

Ahora necesitamos una plantilla `WelcomeView`. Ejecute la aplicación para compilar el nuevo código. Cierre el explorador, haga clic con el botón secundario dentro del método `Welcome` y, a continuación, haga clic en **Agregar vista**.

Este es el aspecto que tiene el cuadro de diálogo **Agregar vista** .

[![3AddWelcomeView](adding-a-view/_static/image12.png "3AddWelcomeView")](adding-a-view/_static/image11.png)

Agregue el siguiente código bajo el elemento `<h2>` en la nueva <em>Página principal.</em> archivo vbhtml. Haremos un bucle y digamos &quot;Hello&quot; tantas veces como indique el usuario.

[!code-vbhtml[Main](adding-a-view/samples/sample8.vbhtml)]

Ejecute la aplicación y vaya a `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora los datos se toman de la dirección URL y se pasan al controlador automáticamente. El controlador empaqueta los datos en un objeto `Model` y pasa ese objeto a la vista. La vista que muestra los datos como HTML para el usuario.

[![3Hello_Scott_4](adding-a-view/_static/image14.png "3Hello_Scott_4")](adding-a-view/_static/image13.png)

Bueno, esto era un tipo de &quot;M&quot; para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
