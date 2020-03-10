---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Usar el calendario emergente del DatePicker de la interfaz de usuario de HTML5 y jQuery con ASP.NET MVC, parte 1 | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en un ASP.NET...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: c1c2380f24c72f6aabaaacaf975e95288a384ff1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78433291"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Usar el calendario emergente DatePicker de HTML5 y jQuery UI con ASP.NET MVC, parte 1

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el calendario emergente de DatePicker de la interfaz de usuario de jQuery en una aplicación web MVC de ASP.NET.

Este tutorial le enseñará los aspectos básicos de cómo trabajar con plantillas de editor, plantillas de pantalla y el [calendario emergente de DatePicker](http://plugins.jquery.com/project/datepicker) de la interfaz de usuario de jQuery en una aplicación web MVC de ASP.net. En este tutorial, puede usar Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), que es una versión gratuita de Microsoft Visual Studio, o puede usar Visual Studio 2010 SP1 si ya lo tiene.

Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente el software necesario mediante los siguientes vínculos:

- [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)

Si usa Visual Studio 2010 en lugar de Visual Web Developer, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

En este tutorial se supone que ha completado el tutorial [de introducción con MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o que está familiarizado con el desarrollo de ASP.NET MVC. Este tutorial comienza con el proyecto completado del tutorial [Introducción con MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) .

En este tutorial se muestra C#el código de. Sin embargo, el [proyecto de inicio](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) y el proyecto completado también están disponibles en Visual Basic.

Un proyecto de Visual Studio C# con y Visual Basic código fuente está disponible para acompañar este tema: [Descargar](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Lo que creará

Agregará plantillas (específicamente, plantillas de edición y visualización) a la aplicación de lista de películas sencilla que se creó en el tutorial [Introducción con MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) . También agregará un calendario emergente DatePicker de la [interfaz de usuario de jQuery](http://jqueryui.com/demos/datepicker/) para simplificar el proceso de escritura de fechas. En la captura de pantalla siguiente se muestra la aplicación modificada con el calendario emergente DatePicker de jQuery UI mostrado.

![jQuery terminado](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Habilidades que aprenderá

Aprenderá lo siguiente:

- Cómo usar los atributos del espacio de nombres [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) para controlar el formato de los datos cuando se muestran y cuando están en modo de edición.
- Cómo crear plantillas (editar y mostrar plantillas) para controlar el formato de los datos.
- Cómo agregar el DatePicker de la [interfaz de usuario de jQuery](http://jqueryui.com/demos/datepicker/) como una manera de especificar los campos de fecha.

### <a name="getting-started"></a>Introducción

Si aún no tiene la aplicación de lista de películas del proyecto de inicio, descárguelo: 

* [Descargar](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).
* En el explorador de Windows, haga clic con el botón secundario en el archivo *MvcMovie. zip* y seleccione **propiedades**. 
* En el cuadro de diálogo **propiedades de MvcMovie. zip** , seleccione **desbloquear**. (El desbloqueo evita recibir una advertencia de seguridad que se produce al intentar usar un archivo *.zip* que ha descargado de la web).

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Haga clic con el botón secundario en el archivo *MvcMovie. zip* y seleccione **extraer todo** para descomprimir el archivo. En Visual Web Developer o Visual Studio 2010, abra el archivo *MvcMovieCS\_MA. sln* .

En **Explorador de soluciones**, haga doble clic en el *\\Views\Shared _Layout. cshtml* para abrirlo. Cambie el encabezado de `H1` de la **aplicación de película MVC** a **jQuery jQuery**. Presione CTRL + F5 para ejecutar la aplicación y haga clic en la pestaña **Inicio** , que le llevará al método `Index` del controlador de película. Para probar la aplicación, seleccione el vínculo **Editar** y el vínculo **detalles** de una de las películas. Tenga en cuenta que en las vistas de índice, edición y detalles, la fecha y el precio de lanzamiento tienen un formato correcto:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

El formato de la fecha y el precio es el resultado de utilizar el atributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) en las propiedades de la clase `Movie`.

Abra el archivo *Movie.CS* y comente el atributo `DisplayFormat` en las propiedades `ReleaseDate` y `Price`. La clase `Movie` resultante tiene el siguiente aspecto:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Presione CTRL + F5 de nuevo para ejecutar la aplicación y seleccione la pestaña **Inicio** para ver la lista de películas. Esta vez, la fecha de lanzamiento muestra la fecha y la hora, y el campo precio ya no muestra el símbolo de moneda. El cambio en la clase `Movie` ha deshecho el bonito formato que vio anteriormente, pero lo corregiremos en un momento.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Usar el atributo DataType de DataAnnotations para especificar el tipo de datos

Reemplace el atributo `DisplayFormat` comentado para la propiedad `ReleaseDate` por el atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , mediante la enumeración `Date`. Reemplace el atributo `DisplayFormat` de la propiedad `Price` por el atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) de nuevo, pero esta vez con la enumeración `Currency`. Este es el aspecto del código completado:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Ejecute la aplicación. Ahora, las propiedades fecha de lanzamiento y precio tienen el formato correcto (es decir, con los formatos de fecha y moneda adecuados). El atributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) proporciona metadatos de tipo para las plantillas ASP.NET MVC integradas para que los campos se representen en el formato correcto. El uso del atributo `DataType` es preferible al uso del atributo `DisplayFormat` que estaba originalmente en el código, ya que el atributo `DataType` hace que el modelo sea más limpio y más flexible para propósitos como la internacionalización.

En la sección siguiente, verá cómo crear plantillas personalizadas para mostrar los campos de fecha.

> [!div class="step-by-step"]
> [Siguiente](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
