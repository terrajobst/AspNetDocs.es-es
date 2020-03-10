---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
title: Introducción a ASP.NET MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Una versión actualizada si este tutorial está disponible aquí mediante Visual Studio 2013. En el nuevo tutorial se usa ASP.NET MVC 5, que proporciona muchas mejoras sobre t...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: ed66530a-04d5-49eb-b76a-85be1f57c437
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: 51709a9c6ddb39b8fcd1cd94cd08d530a595825a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485281"
---
# <a name="intro-to-aspnet-mvc-4"></a>Introducción a ASP.NET MVC 4

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Una versión actualizada si este tutorial está disponible [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). En el nuevo tutorial se usa ASP.NET MVC 5, que proporciona muchas mejoras en este tutorial.
>
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC 4 con Microsoft [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual Web Developer 2010 Express Service Pack 1. Se recomienda Visual Studio 2012, por lo que no tendrá que instalar nada para completar el tutorial. Si usa Visual Studio 2010, debe instalar los componentes siguientes. Para instalarlos todos, haga clic en los vínculos siguientes:
>
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Instalador de WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)
> - [LocalDB](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLLocalDBOnly_11_0)
> - [SSDT](https://blogs.msdn.com/b/rickandy/archive/2012/08/02/installing-and-using-sql-server-data-tools-ssdt-on-visual-studio-2010-and-vwd.aspx)
>
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale el [instalador de WPI para ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392) y los [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack) .
>
> Hay disponible un proyecto de Visual C# Web Developer con código fuente para acompañar este tema. [Descargue la C# versión](https://code.msdn.microsoft.com/Intro-to-ASPNET-MVC-4-61d0219d/file/114480/1/MvcMovie.zip).
>
> En el tutorial, ejecute la aplicación en Visual Studio. También puede hacer que la aplicación esté disponible a través de Internet si la implementa en un proveedor de hospedaje. Microsoft ofrece hospedaje web gratuito para un máximo de 10 sitios web en una [cuenta de prueba gratuita de Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Para obtener información sobre cómo implementar un proyecto Web de Visual Studio en un sitio web de Windows Azure, vea [crear e implementar un sitio web de ASP.net y SQL Database con Visual Studio](https://docs.microsoft.com/dotnet/azure/). En este tutorial también se muestra cómo usar Migraciones de Entity Framework Code First para implementar la base de datos de SQL Server en Windows Azure SQL Database (anteriormente SQL Azure).
>
> Este tutorial fue escrito por Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ).

## <a name="what-youll-build"></a>Lo que creará

> [!NOTE]
> Una versión actualizada si este tutorial está disponible [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013). En el nuevo tutorial se usa ASP.NET MVC 5, que proporciona muchas mejoras en este tutorial.

Implementará una sencilla aplicación de lista de películas que admita la creación, edición, búsqueda y enumeración de películas de una base de datos. A continuación se muestran dos capturas de pantallas de la aplicación que va a compilar. Incluye una página que muestra una lista de películas de una base de datos:

![](intro-to-aspnet-mvc-4/_static/image1.png)

La aplicación también le permite agregar, editar y eliminar películas, además de ver los detalles de cada uno de ellos. Todos los escenarios de entrada de datos incluyen la validación para asegurarse de que los datos almacenados en la base de datos son correctos.

![](intro-to-aspnet-mvc-4/_static/image2.png)

## <a name="getting-started"></a>Introducción

Para empezar, ejecute Visual Studio Express 2012 o Visual Web Developer 2010 Express. La mayoría de las capturas de pantalla de esta serie usan Visual Studio Express 2012, pero puede completar este tutorial con Visual Studio 2010/SP1, Visual Studio 2012, Visual Studio Express 2012 o Visual Web Developer 2010 Express. Seleccione **nuevo proyecto** en la página de **Inicio** .

Visual Studio es un IDE o un entorno de desarrollo integrado. Del mismo modo que usa Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones. En Visual Studio, hay una barra de herramientas en la parte superior que muestra varias opciones disponibles. También hay un menú que proporciona otra manera de realizar tareas en el IDE. (Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde la página **Inicio** , puede usar el menú y seleccionar **archivo** &gt; **nuevo proyecto**).

![](intro-to-aspnet-mvc-4/_static/image3.png)

## <a name="creating-your-first-application"></a>Crear la primera aplicación

Puede crear aplicaciones mediante Visual Basic o visual C# como lenguaje de programación. Seleccione visual C# a la izquierda y, a continuación, seleccione **aplicación Web de ASP.NET MVC 4**. Asigne al proyecto el nombre &quot;MvcMovie&quot; y, a continuación, haga clic en **Aceptar**.

![](intro-to-aspnet-mvc-4/_static/image4.png)

En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione **aplicación de Internet**. Deje **Razor** como el motor de vista predeterminado.

![](intro-to-aspnet-mvc-4/_static/image5.png)

Haga clic en **Aceptar**. Visual Studio usó una plantilla predeterminada para el proyecto ASP.NET MVC que acaba de crear, por lo que tiene una aplicación en funcionamiento en este momento sin hacer nada. Este es un Hola mundo de &quot;sencillo.&quot; proyecto y es un buen lugar para iniciar la aplicación.

![](intro-to-aspnet-mvc-4/_static/image6.png)

En el menú **Depurar**, seleccione **Iniciar depuración**.

![](intro-to-aspnet-mvc-4/_static/image7.png)

Observe que el método abreviado de teclado para iniciar la depuración es F5.

F5 hace que Visual Studio se inicie IIS Express y ejecute la aplicación Web. A continuación, Visual Studio inicia un explorador y abre la Página principal de la aplicación. Tenga en cuenta que la barra de direcciones del explorador indica `localhost` y no algo como `example.com`. Esto se debe a que `localhost` siempre apunta a su propio equipo local, que en este caso está ejecutando la aplicación que acaba de crear. Cuando Visual Studio ejecuta un proyecto Web, se usa un puerto aleatorio para el servidor Web. En la imagen siguiente, el número de puerto es 41788. Al ejecutar la aplicación, es probable que vea un número de puerto diferente.

![](intro-to-aspnet-mvc-4/_static/image8.png)

De forma predeterminada, esta plantilla predeterminada le proporciona Inicio, contacto y acerca de las páginas. También proporciona soporte para registrarse e iniciar sesión, y vínculos a Facebook y Twitter. El siguiente paso es cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC. Cierre el explorador y cambie parte del código.

> [!div class="step-by-step"]
> [Siguiente](adding-a-controller.md)
