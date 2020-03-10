---
uid: mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
title: Información general sobre las vistasC#de MVC de ASP.net () | Microsoft Docs
author: StephenWalther
description: ¿Qué es una vista de ASP.NET MVC y en qué se diferencia de una página HTML? En este tutorial, Stephen Walther le presenta las vistas y demuestra cómo puede...
ms.author: riande
ms.date: 02/16/2008
ms.assetid: 152ab1e5-aec2-4ea7-b8cc-27a24dd9acb8
msc.legacyurl: /mvc/overview/older-versions-1/views/asp-net-mvc-views-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: b3f44aa9654a2a718381eaf9c856ca3e15ed1e27
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485857"
---
# <a name="aspnet-mvc-views-overview-c"></a>Información general sobre las vistas de ASP.NET MVC (C#)

por [Stephen Walther](https://github.com/StephenWalther)

> ¿Qué es una vista de ASP.NET MVC y en qué se diferencia de una página HTML? En este tutorial, Stephen Walther le presenta las vistas y muestra cómo puede aprovechar las ventajas de ver datos y aplicaciones auxiliares HTML dentro de una vista.

El propósito de este tutorial es proporcionar una breve introducción a las vistas de ASP.NET MVC, ver datos y aplicaciones auxiliares HTML. Al final de este tutorial, debe comprender cómo crear nuevas vistas, pasar datos de un controlador a una vista y usar aplicaciones auxiliares HTML para generar contenido en una vista.

## <a name="understanding-views"></a>Descripción de vistas

En el caso de las páginas ASP.NET o Active Server, ASP.NET MVC no incluye nada que se corresponda directamente con una página. En una aplicación ASP.NET MVC, no hay una página en disco que se corresponda con la ruta de acceso en la dirección URL que se escribe en la barra de direcciones del explorador. Lo más parecido a una página en una aplicación ASP.NET MVC es algo denominado *vista*.

En una aplicación ASP.NET MVC, las solicitudes del explorador entrante se asignan a las acciones del controlador. Una acción del controlador puede devolver una vista. Sin embargo, una acción de controlador podría realizar algún otro tipo de acción, como redirigirle a otra acción de controlador.

La lista 1 contiene un controlador simple denominado HomeController. El HomeController expone dos acciones de controlador denominadas index () y Details ().

**Lista 1-HomeController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample1.cs)]

Puede invocar la primera acción, la acción index (), escribiendo la siguiente dirección URL en la barra de direcciones del explorador:

/Home/Index

Puede invocar la segunda acción, la acción Details (), escribiendo esta dirección en el explorador:

/Home/Details

La acción index () devuelve una vista. La mayoría de las acciones que cree devolverán vistas. Sin embargo, una acción puede devolver otros tipos de resultados de acción. Por ejemplo, la acción Details () devuelve un RedirectToActionResult que redirige la solicitud entrante a la acción index ().

La acción index () contiene la siguiente línea de código:

Vista ();

Esta línea de código devuelve una vista que se debe encontrar en la siguiente ruta de acceso del servidor Web:

\Views\Home\Index.aspx

La ruta de acceso a la vista se deduce del nombre del controlador y el nombre de la acción del controlador.

Si lo prefiere, puede ser explícito acerca de la vista. La siguiente línea de código devuelve una vista denominada Fred:

Vista (Fred);

Cuando se ejecuta esta línea de código, se devuelve una vista desde la ruta de acceso siguiente:

\Views\Home\Fred.aspx

> [!NOTE] 
> 
> Si tiene previsto crear pruebas unitarias para la aplicación ASP.NET MVC, se recomienda ser explícita sobre los nombres de vista. De este modo, puede crear una prueba unitaria para comprobar que una acción de controlador devolvió la vista esperada.

## <a name="adding-content-to-a-view"></a>Agregar contenido a una vista

Una vista es un documento HTML estándar (X) que puede contener scripts. Los scripts se usan para agregar contenido dinámico a una vista.

Por ejemplo, la vista de la lista 2 muestra la fecha y la hora actuales.

**Lista 2-\Views\Home\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample2.aspx)]

Observe que el cuerpo de la página HTML de la lista 2 contiene el siguiente script:

&lt;% Response. Write (DateTime. Now);%&gt;

Use los delimitadores de script &lt;% y%&gt; para marcar el principio y el final de un script. Este script está escrito en C#. Muestra la fecha y hora actuales llamando al método Response. Write () para representar el contenido en el explorador. Los delimitadores de script &lt;% y%&gt; se pueden usar para ejecutar una o más instrucciones.

Dado que llama a Response. Write () con tanta frecuencia, Microsoft le proporciona un acceso directo para llamar al método Response. Write (). La vista de la lista 3 usa los delimitadores &lt;% = y%&gt; como un acceso directo para llamar a Response. Write ().

**Lista 3: Views\Home\Index2.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample3.aspx)]

Puede usar cualquier lenguaje .NET para generar contenido dinámico en una vista. Normalmente, usará Visual Basic .NET o C# para escribir los controladores y las vistas.

## <a name="using-html-helpers-to-generate-view-content"></a>Usar aplicaciones auxiliares HTML para generar el contenido de la vista

Para que sea más fácil agregar contenido a una vista, puede aprovechar algo denominado *aplicación auxiliar HTML*. Una aplicación auxiliar HTML, normalmente, es un método que genera una cadena. Puede usar aplicaciones auxiliares HTML para generar elementos HTML estándar como cuadros de texto, vínculos, listas desplegables y cuadros de lista.

Por ejemplo, la vista de la lista 4 aprovecha las ventajas de tres aplicaciones auxiliares HTML: las aplicaciones auxiliares (), el cuadro de texto () y la contraseña () para generar un formulario de inicio de sesión (consulte la figura 1).

**Lista 4: \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample4.aspx)]

[![el cuadro de diálogo nuevo proyecto](asp-net-mvc-views-overview-cs/_static/image1.jpg)](asp-net-mvc-views-overview-cs/_static/image1.png)

**Figura 01**: formulario de inicio de sesión estándar ([haga clic para ver la imagen de tamaño completo](asp-net-mvc-views-overview-cs/_static/image2.png))

Se llama a todos los métodos auxiliares HTML en la propiedad HTML de la vista. Por ejemplo, si se representa un cuadro de texto, se llama al método html. TextBox ().

Tenga en cuenta que usa los delimitadores de script &lt;% = y%&gt; al llamar a las aplicaciones auxiliares HTML. TextBox () y HTML. Password (). Estas aplicaciones auxiliares simplemente devuelven una cadena. Debe llamar a Response. Write () para representar la cadena en el explorador.

El uso de métodos auxiliares HTML es opcional. Facilitan su vida reduciendo la cantidad de HTML y el script que necesita escribir. La vista de la lista 5 representa el mismo formulario exacto que la vista de la lista 4 sin usar aplicaciones auxiliares HTML.

**Lista 5: \Views\Home\Login.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample5.aspx)]

También tiene la opción de crear sus propias aplicaciones auxiliares HTML. Por ejemplo, puede crear un método auxiliar GridView () que muestre automáticamente un conjunto de registros de base de datos en una tabla HTML. Exploramos este tema en el tutorial **creación de aplicaciones auxiliares HTML personalizadas**.

## <a name="using-view-data-to-pass-data-to-a-view"></a>Usar los datos de vista para pasar datos a una vista

Utilice los datos de vista para pasar datos de un controlador a una vista. Considere la visualización de datos como un paquete que se envía a través del correo. Todos los datos pasados desde un controlador a una vista se deben enviar mediante este paquete. Por ejemplo, el controlador de la lista 6 agrega un mensaje para ver los datos.

**Listado 6-ProductController.cs**

[!code-csharp[Main](asp-net-mvc-views-overview-cs/samples/sample6.cs)]

La propiedad ViewData del controlador representa una colección de pares de nombre y valor. En la enumeración 6, el método index () agrega un elemento a la colección de datos de vista denominada Message con el valor Hola mundo!. Cuando el método index () devuelve la vista, los datos de la vista se pasan automáticamente a la vista.

La vista de la lista 7 recupera el mensaje de los datos de la vista y representa el mensaje en el explorador.

**Listado 7: \Views\Product\Index.aspx**

[!code-aspx[Main](asp-net-mvc-views-overview-cs/samples/sample7.aspx)]

Tenga en cuenta que la vista aprovecha el método auxiliar HTML. encode () HTML al representar el mensaje. La aplicación auxiliar html html. encode () codifica caracteres especiales como &lt; y &gt; en caracteres que se pueden mostrar de forma segura en una página web. Siempre que represente el contenido que un usuario envía a un sitio web, debe codificar el contenido para evitar ataques por inyección de código JavaScript.

(Dado que hemos creado el mensaje en ProductController, realmente no es necesario codificar el mensaje. Sin embargo, es un buen hábito llamar siempre al método html. encode () al mostrar el contenido recuperado de los datos de vista en una vista.

En el listado 7, aprovechamos los datos de la vista para pasar un mensaje de cadena simple de un controlador a una vista. También puede usar ver datos para pasar otros tipos de datos, como una colección de registros de base de datos, de un controlador a una vista. Por ejemplo, si desea mostrar el contenido de la tabla de la base de datos Products en una vista, pasaría la colección de registros de base de datos en los datos de la vista.

También tiene la opción de pasar datos de vista fuertemente tipados de un controlador a una vista. Exploramos este tema en el tutorial **Descripción de las vistas y los datos de vista fuertemente tipados**.

## <a name="summary"></a>Resumen

En este tutorial se ha proporcionado una breve introducción a las vistas de MVC de ASP.NET, los datos de vista y las aplicaciones auxiliares HTML. En la primera sección, aprendió a agregar nuevas vistas al proyecto. Aprendió que debe agregar una vista a la carpeta correcta para poder llamarla desde un controlador determinado. A continuación, se describe el tema de las aplicaciones auxiliares HTML. Ha aprendido cómo las aplicaciones auxiliares HTML le permiten generar fácilmente contenido HTML estándar. Por último, aprendió a sacar provecho de los datos de vista para pasar datos de un controlador a una vista.

> [!div class="step-by-step"]
> [Siguiente](creating-custom-html-helpers-cs.md)
