---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
title: Agregar una vista (C#) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: abc7c78d-cb09-4a4c-a887-61bc401d40e3
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-view
msc.type: authoredcontent
ms.openlocfilehash: b4a1316feb8d9b7f3ef5ca4755bf1cc5b23e6ef9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498847"
---
# <a name="adding-a-view-c"></a>Agregar una vista (C#)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Hay disponible un proyecto de Visual C# Web Developer con código fuente para acompañar este tema. [Descargue la C# versión](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.

En esta sección, va a modificar la clase `HelloWorldController` para usar los archivos de plantilla de vista con el fin de encapsular correctamente el proceso de generación de respuestas HTML a un cliente.

Creará un archivo de plantilla de vista mediante el nuevo [motor de vista de Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx) que se presentó con ASP.NET MVC 3. Las plantillas de vista basadas en Razor tienen una extensión de archivo *. cshtml* y proporcionan una forma elegante de crear la C#salida HTML mediante. Razor minimiza el número de caracteres y pulsaciones de teclas necesarios al escribir una plantilla de vista y habilita un flujo de trabajo de codificación de fluido rápido.

Empiece usando una plantilla de vista con el método `Index` en la clase `HelloWorldController`. Actualmente, el método `Index` devuelve una cadena con un mensaje que está codificado de forma rígida en la clase de controlador. Cambie el método de `Index` para que devuelva un objeto `View`, como se muestra a continuación:

[!code-csharp[Main](adding-a-view/samples/sample1.cs)]

Este código usa una plantilla de vista para generar una respuesta HTML al explorador. En el proyecto, agregue una plantilla de vista que pueda usar con el método `Index`. Para ello, haga clic con el botón secundario dentro del método `Index` y haga clic en **Agregar vista**.

![](adding-a-view/_static/image1.png)

Aparece el cuadro de diálogo **Agregar vista** . Deje los valores predeterminados tal y como están y haga clic en el botón **Agregar** :

![](adding-a-view/_static/image2.png)

Se crean la carpeta *MvcMovie\Views\HelloWorld* y el archivo *MvcMovie\Views\HelloWorld\Index.cshtml* . Puede verlos en **Explorador de soluciones**:

![](adding-a-view/_static/image3.png)

A continuación se muestra el archivo *index. cshtml* que se creó:

[![HelloWorldIndex](adding-a-view/_static/image5.png)](adding-a-view/_static/image4.png)

Agregue cierto código HTML bajo la etiqueta `<h2>`. A continuación se muestra el archivo *MvcMovie\Views\HelloWorld\Index.cshtml* modificado.

[!code-cshtml[Main](adding-a-view/samples/sample2.cshtml)]

Ejecute la aplicación y vaya al controlador de `HelloWorld` (`http://localhost:xxxx/HelloWorld`). El método `Index` del controlador no hace mucho trabajo; simplemente ejecutó la instrucción `return View()`, que especificaba que el método debe utilizar un archivo de plantilla de vista para representar una respuesta al explorador. Dado que no especificó explícitamente el nombre del archivo de plantilla de vista que se va a usar, ASP.NET MVC tenía como valor predeterminado el archivo de vista *index. cshtml* en la carpeta *\Views\HelloWorld* . En la imagen siguiente se muestra la cadena codificada de forma rígida en la vista.

![](adding-a-view/_static/image6.png)

Parece bastante bueno. Sin embargo, tenga en cuenta que la barra de título del explorador indica "índice" y el título grande de la página indica "mi aplicación MVC". Vamos a cambiarlos.

## <a name="changing-views-and-layout-pages"></a>Cambiar vistas y páginas de diseño

En primer lugar, desea cambiar el título "mi aplicación MVC" en la parte superior de la página. Ese texto es común a todas las páginas. En realidad, se implementa en un solo lugar del proyecto, aunque aparezca en todas las páginas de la aplicación. Vaya a la carpeta */views/Shared* en **Explorador de soluciones** y abra el archivo *\_layout. cshtml* . Este archivo se denomina *Página de diseño* y es el "Shell" compartido que usan todas las demás páginas.

[![_LayoutCshtml](adding-a-view/_static/image8.png)](adding-a-view/_static/image7.png)

Las plantillas de diseño permiten especificar el diseño del contenedor HTML del sitio en un solo lugar y, después, aplicarlo en varias páginas del sitio. Observe la línea `@RenderBody()` situada cerca de la parte inferior del archivo. `RenderBody` es un marcador de posición en el que se muestran todas las páginas específicas de la vista que se crean, "encapsuladas" en la página de diseño. Cambie el título del título en la plantilla de diseño de "My MVC Application" a "MVC Movie app".

[!code-cshtml[Main](adding-a-view/samples/sample3.cshtml)]

Ejecute la aplicación y observe que ahora dice "MVC Movie app". Haga clic en el vínculo **About (acerca de** ) y verá cómo la página muestra también "MVC Movie app". Pudimos realizar el cambio una vez en la plantilla de diseño y hacer que todas las páginas del sitio reflejen el nuevo título.

![](adding-a-view/_static/image9.png)

A continuación se muestra el archivo completo *\_layout. cshtml* :

[!code-cshtml[Main](adding-a-view/samples/sample4.cshtml)]

Ahora, vamos a cambiar el título de la página de índice (ver).

Abra *MvcMovie\Views\HelloWorld\Index.cshtml*. Hay dos lugares para hacer un cambio: en primer lugar, el texto que aparece en el título del explorador y, a continuación, en el encabezado secundario (el elemento `<h2>`). Haremos que sean algo diferentes para que pueda ver qué parte del código cambia cada área de la aplicación.

[!code-cshtml[Main](adding-a-view/samples/sample5.cshtml)]

Para indicar el título HTML que se va a mostrar, el código anterior establece una propiedad `Title` del objeto `ViewBag` (que se encuentra en la plantilla de vista *index. cshtml* ). Si vuelve a examinar el código fuente de la plantilla de diseño, observará que la plantilla usa este valor en el elemento `<title>` como parte de la `<head>` sección del código HTML. Con este enfoque, puede pasar fácilmente otros parámetros entre la plantilla de vista y el archivo de diseño.

Ejecute la aplicación y vaya a `http://localhost:xx/HelloWorld`. Tenga en cuenta que el título del explorador, el encabezado principal y los encabezados secundarios han cambiado. (Si no ve los cambios en el explorador, es posible que esté viendo contenido almacenado en caché. Presione Ctrl+F5 en el explorador para forzar que se cargue la respuesta del servidor).

Observe también cómo el contenido de la plantilla de vista *index. cshtml* se combinó con la plantilla de vista *\_layout. cshtml* y se envió una única respuesta HTML al explorador. Con las plantillas de diseño es realmente fácil hacer cambios para que se apliquen en todas las páginas de la aplicación.

![](adding-a-view/_static/image10.png)

Nuestra pequeña cantidad de "datos", en este caso, el mensaje "Hello from our View Template!" (Hola desde nuestra plantilla de vista), están codificados de forma rígida. La aplicación de MVC tiene una "V" (vista) y ha obtenido una "C" (controlador), pero todavía no tiene una "M" (modelo). En breve, veremos cómo crear una base de datos y recuperar los datos del modelo.

## <a name="passing-data-from-the-controller-to-the-view"></a>Pasar datos del controlador a la vista

Sin embargo, antes de ir a una base de datos y hablar acerca de los modelos, vamos a hablar primero sobre cómo pasar información desde el controlador a una vista. Las clases de controlador se invocan en respuesta a una solicitud de dirección URL entrante. Una clase de controlador es donde se escribe el código que controla los parámetros de entrada, recupera datos de una base de datos y, en última instancia, decide qué tipo de respuesta devolver al explorador. Las plantillas de vista se pueden usar desde un controlador para generar y dar formato a una respuesta HTML en el explorador.

Los controladores son responsables de proporcionar los datos u objetos necesarios para que una plantilla de vista represente una respuesta al explorador. Una plantilla de vista nunca debe realizar la lógica de negocios ni interactuar directamente con una base de datos. En su lugar, debería funcionar solo con los datos proporcionados por el controlador. Mantener esta "separación de preocupaciones" ayuda a mantener el código limpio y más fácil de mantener.

Actualmente, el método de acción `Welcome` de la clase `HelloWorldController` toma un `name` y un parámetro `numTimes` y, a continuación, genera los valores directamente en el explorador. En lugar de hacer que el controlador represente esta respuesta como una cadena, vamos a cambiar el controlador para usar una plantilla de vista en su lugar. La plantilla de vista generará una respuesta dinámica, lo que significa que debe pasar las partes de datos adecuadas desde el controlador a la vista para que se genere la respuesta. Para ello, haga que el controlador Coloque los datos dinámicos que necesita la plantilla de vista en un objeto `ViewBag` al que pueda tener acceso la plantilla de vista.

Vuelva al archivo *HelloWorldController.CS* y cambie el método `Welcome` para agregar un `Message` y `NumTimes` valor al objeto `ViewBag`. `ViewBag` es un objeto dinámico, lo que significa que puede colocar todo lo que desee en él. el objeto `ViewBag` no tiene ninguna propiedad definida hasta que coloque algo dentro de ella. El archivo *HelloWorldController.cs* completo tiene este aspecto:

[!code-csharp[Main](adding-a-view/samples/sample6.cs)]

Ahora, el objeto de `ViewBag` contiene datos que se pasarán a la vista automáticamente.

A continuación, necesitará una plantilla de vista de bienvenida. En el menú **depurar** , seleccione compilar **MvcMovie** para asegurarse de que se compila el proyecto.

[![BuildHelloWorld](adding-a-view/_static/image12.png)](adding-a-view/_static/image11.png)

A continuación, haga clic con el botón secundario dentro del método `Welcome` y haga clic en **Agregar vista**. Este es el aspecto del cuadro de diálogo **Agregar vista** :

![](adding-a-view/_static/image13.png)

Haga clic en **Agregar**y, a continuación, agregue el siguiente código en el elemento `<h2>` del nuevo archivo *welcome. cshtml* . Creará un bucle que dice "Hello" tantas veces como indique el usuario. A continuación se muestra el archivo *welcome. cshtml* completo.

[!code-cshtml[Main](adding-a-view/samples/sample7.cshtml)]

Ejecute la aplicación y vaya a la siguiente dirección URL:

`http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4`

Ahora los datos se toman de la dirección URL y se pasan al controlador automáticamente. El controlador empaqueta los datos en un objeto `ViewBag` y pasa ese objeto a la vista. A continuación, la vista muestra los datos como HTML al usuario.

![](adding-a-view/_static/image14.png)

Bueno, todo esto era un tipo de "M" para el modelo, pero no el tipo de base de datos. Vamos a aprovechar lo que hemos aprendido para crear una base de datos de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-controller.md)
> [Siguiente](adding-a-model.md)
