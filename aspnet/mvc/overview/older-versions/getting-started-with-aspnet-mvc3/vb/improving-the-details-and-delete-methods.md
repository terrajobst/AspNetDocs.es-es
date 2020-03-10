---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
title: Mejorar los métodos details y Delete (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: c5c14ef0-c128-4dc1-8c01-7f0fdb09e411
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 08d80cac071907e927bb30df53c6f84a28f53156
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485545"
---
# <a name="improving-the-details-and-delete-methods-vb"></a>Mejorar los métodos Details y Delete (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de VB.NET está disponible para acompañar este tema. [Descargue la versión de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [ C# versión](../cs/improving-the-details-and-delete-methods.md) de este tutorial.

En esta parte del tutorial, realizará algunas mejoras en los métodos `Details` y `Delete` generados automáticamente. Estos cambios no son necesarios, pero con unos pocos fragmentos de código pequeños, puede mejorar fácilmente la aplicación.

## <a name="improving-the-details-and-delete-methods"></a>Mejorar los métodos details y DELETE

Al aplicar scaffolding al controlador de `Movie`, ASP.NET MVC generó código que funcionó bien, pero que se puede hacer más robusto con solo unos pocos cambios pequeños.

Abra el controlador de `Movie` y modifique el método `Details` devolviendo `HttpNotFound` cuando no se encuentra una película. También debe modificar el método `Details` para establecer un valor predeterminado para el identificador que se le pasa. (Se realizaron cambios similares en el método `Edit` en la [parte 6](examining-the-edit-methods-and-edit-view.md) de este tutorial). Sin embargo, debe cambiar el tipo de valor devuelto del método `Details` de `ViewResult` a `ActionResult`, porque el método `HttpNotFound` no devuelve un objeto `ViewResult`. En el ejemplo siguiente se muestra el método de `Details` modificado.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample1.vb)]

Code First facilita la búsqueda de datos mediante el método `Find`. Una característica de seguridad importante que integramos en el método es que el código comprueba que el método `Find` ha encontrado una película antes de que el código intente hacer nada con ella. Por ejemplo, un pirata informático podría introducir errores en el sitio cambiando la dirección URL creada por los vínculos de `http://localhost:xxxx/Movies/Details/1` a algo parecido a `http://localhost:xxxx/Movies/Details/12345` (o a algún otro valor que no represente una película real). Si no se comprueba una película nula, podría producirse un error de base de datos.

Del mismo modo, cambie los métodos `Delete` y `DeleteConfirmed` para especificar un valor predeterminado para el parámetro ID y para devolver `HttpNotFound` cuando no se encuentre una película. A continuación se muestran los métodos de `Delete` actualizados en el controlador de `Movie`.

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample2.vb)]

Tenga en cuenta que el método `Delete` no elimina los datos. La acción de efectuar una operación de eliminación en respuesta a una solicitud GET (o con este propósito efectuar una operación de edición, creación o cualquier otra operación que modifique los datos) genera una vulnerabilidad de seguridad. Para obtener más información, consulte la entrada de blog de Stephen Walther [ASP.NET MVC Tip #46: no use los vínculos de eliminación porque crean carencias de seguridad](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

El método `HttpPost` que elimina los datos se denomina `DeleteConfirmed` para proporcionar al método HTTP POST una firma o nombre únicos. Las dos firmas de método se muestran a continuación:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample3.vb)]

El Common Language Runtime (CLR) requiere que los métodos sobrecargados tengan una firma única (el mismo nombre, una lista diferente de parámetros). Sin embargo, aquí necesita dos métodos de eliminación: uno para GET y otro para POST, que ambos requieren la misma firma. (ambos deben aceptar un número entero como parámetro).

Para ordenar esto, puede hacer un par de cosas. Uno es asignar nombres distintos a los métodos. Eso es lo que hicimos en el ejemplo anterior. Pero esto implica un pequeño problema: ASP.NET asigna segmentos de una dirección URL a los métodos de acción por nombre y, si cambia el nombre de un método, normalmente el enrutamiento no podría encontrar ese método. La solución es la que ve en el ejemplo, que consiste en agregar el atributo `ActionName("Delete")` al método `DeleteConfirmed`. De este modo, se realiza la asignación del sistema de enrutamiento de forma eficaz para que una dirección URL que incluya <em>/Delete/</em>para una solicitud post busque el método `DeleteConfirmed`.

Otra manera de evitar un problema con los métodos que tienen nombres y firmas idénticos es cambiar artificialmente la firma del método POST para incluir un parámetro no utilizado. Por ejemplo, algunos desarrolladores agregan un tipo de parámetro `FormCollection` que se pasa al método POST y, a continuación, simplemente no usan el parámetro:

[!code-vb[Main](improving-the-details-and-delete-methods/samples/sample4.vb)]

## <a name="wrapping-up"></a>Ajuste

Ahora tiene una aplicación ASP.NET MVC completa que almacena los datos en una base de datos SQL Server Compact. Puede crear, leer, actualizar, eliminar y buscar películas.

![](improving-the-details-and-delete-methods/_static/image1.png)

En este tutorial básico se ha empezado a crear controladores, a asociarlos con vistas y a pasar datos codificados de forma rígida. A continuación, creó y diseñó un modelo de datos. Entity Framework Code First crear una base de datos a partir del modelo de datos sobre la marcha, y el sistema de scaffolding de ASP.NET MVC generó automáticamente los métodos de acción y las vistas para las operaciones CRUD básicas. Después, ha agregado un formulario de búsqueda que permite a los usuarios buscar en la base de datos. Ha cambiado la base de datos para incluir una nueva columna de datos y, a continuación, ha actualizado dos páginas para crear y mostrar estos datos nuevos. Ha agregado la validación marcando el modelo de datos con los atributos del espacio de nombres `DataAnnotations`. La validación resultante se ejecuta en el cliente y en el servidor.

Si desea implementar la aplicación, es útil probar primero la aplicación en el servidor IIS 7 local. Puede usar este vínculo del [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) para habilitar la configuración de IIS para aplicaciones ASP.net. Vea los siguientes vínculos de implementación:

- [Mapa de contenido de implementación de ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Habilitar IIS 7. x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Implementación de proyectos de aplicación Web](https://msdn.microsoft.com/library/dd394698.aspx)

Ahora le animamos a pasar a nuestro nivel intermedio creación de [un modelo de datos Entity Framework para una aplicación ASP.NET MVC](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) y tutoriales de [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) , para explorar los [artículos de ASP.net en MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)y para consultar los numerosos vídeos y recursos en [https://asp.net/mvc](https://asp.net/mvc) para aprender más sobre ASP.NET MVC. Los [foros de ASP.NET MVC](https://forums.asp.net/1146.aspx) son un excelente lugar para formular preguntas.

Disfrútelo.

: Scott Hanselman ([http://hanselman.com](http://hanselman.com) y [@shanselman](http://twitter.com/shanselman) en Twitter) y Rick Anderson [blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Anterior](adding-validation-to-the-model.md)
