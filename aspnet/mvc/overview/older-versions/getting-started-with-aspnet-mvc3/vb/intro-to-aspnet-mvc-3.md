---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
title: Introducción a ASP.NET MVC 3 (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: a1b3d789-93b4-487f-b90d-80c9c9b4f8fa
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/intro-to-aspnet-mvc-3
msc.type: authoredcontent
ms.openlocfilehash: 24f7de303ef7f5a457bd509ecc6bd0e3be7e3d9d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456350"
---
# <a name="intro-to-aspnet-mvc-3-vb"></a>Introducción a ASP.NET MVC 3 (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de VB.NET está disponible para acompañar este tema. [Descargue la versión de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [ C# versión](../cs/intro-to-aspnet-mvc-3.md) de este tutorial.

Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:

- [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)

Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Hay disponible un proyecto de Visual Web Developer con código fuente de VB para acompañar este tema. [Descargue la versión de VB aquí](https://code.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=14824). Si prefiere CSharp, cambie a la [versión de CSharp](../cs/intro-to-aspnet-mvc-3.md) de este tutorial.

## <a name="what-youll-build"></a>Lo que creará

Implementará una sencilla aplicación de lista de películas que admita la creación, edición y enumeración de películas de una base de datos. A continuación se muestran dos capturas de pantallas de la aplicación que va a compilar. Incluye una página que muestra una lista de películas de una base de datos:

[![MoviesWithVariousSm](intro-to-aspnet-mvc-3/_static/image2.png)](intro-to-aspnet-mvc-3/_static/image1.png)

La aplicación también le permite agregar, editar y eliminar películas, además de ver los detalles de cada uno de ellos. Todos los escenarios de entrada de datos incluyen la validación para asegurarse de que los datos almacenados en la base de datos son correctos.

[![CreateFormSo](intro-to-aspnet-mvc-3/_static/image4.png)](intro-to-aspnet-mvc-3/_static/image3.png)

## <a name="skills-youll-learn"></a>Habilidades que aprenderá

Aprenderá lo siguiente:

- Cómo crear un nuevo proyecto de ASP.NET MVC
- Cómo crear una nueva base de datos con Entity Framework First Code
- Cómo crear controladores y vistas de ASP.NET MVC
- Cómo recuperar y Mostrar datos
- Cómo editar datos y habilitar la validación de datos

## <a name="getting-started"></a>Introducción

Para empezar, ejecute Visual Web Developer 2010 Express ("vWD existente" para abreviar) y seleccione **nuevo proyecto** en la página de **Inicio** .

Visual Web Developer es un IDE o un entorno de desarrollo integrado. Del mismo modo que usa Microsoft Word para escribir documentos, usará un IDE para crear aplicaciones. En Visual Web Developer, hay una barra de herramientas en la parte superior que muestra varias opciones disponibles. También hay un menú que proporciona otra manera de realizar tareas en el IDE. (Por ejemplo, en lugar de seleccionar **nuevo proyecto** desde la página **Inicio** , puede usar el menú y seleccionar **archivo** &gt; **nuevo proyecto**).

[![](intro-to-aspnet-mvc-3/_static/image6.png)](intro-to-aspnet-mvc-3/_static/image5.png)

## <a name="creating-your-first-application"></a>Crear la primera aplicación

Puede crear aplicaciones mediante la elección de Visual Basic o visual C# como lenguaje de programación. En este tutorial, seleccione Visual Basic a la izquierda y, a continuación, seleccione **aplicación web MVC 3 ASP.net**. Asigne al proyecto el nombre "MvcMovie" y, a continuación, haga clic en **Aceptar**.

![1NewMVCproj_sm](intro-to-aspnet-mvc-3/_static/image7.png)

En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 3** , seleccione **aplicación de Internet**. Deje **Razor** como el motor de vista predeterminado.

![1InternetAppRazor_SM](intro-to-aspnet-mvc-3/_static/image8.png)

Haga clic en **OK**. Visual Web Developer usó una plantilla predeterminada para el proyecto ASP.NET MVC que acaba de crear, por lo que tiene una aplicación en funcionamiento en este momento sin hacer nada. Se trata de una "Hola mundo" simple. Project, y es un buen lugar para iniciar la aplicación.

[![](intro-to-aspnet-mvc-3/_static/image10.png)](intro-to-aspnet-mvc-3/_static/image9.png)

En el menú **Depurar**, seleccione **Iniciar depuración**.

![](intro-to-aspnet-mvc-3/_static/image11.png)

Observe que el método abreviado de teclado para iniciar la depuración es F5.

F5 hace que Visual Web Developer inicie un servidor Web de desarrollo y ejecute la aplicación Web. A continuación, vWD existente inicia un explorador y abre la Página principal de la aplicación. Tenga en cuenta que la barra de direcciones del explorador indica `localhost` y no algo como `example.com`. Esto se debe a que `localhost` siempre apunta a su propio equipo local, que en este caso está ejecutando la aplicación que acaba de crear. Cuando vWD existente ejecuta un proyecto Web, se usa un puerto aleatorio para el proyecto. En la imagen siguiente, el número de Puerto aleatorio es 43246. El proyecto probablemente usará un número de puerto diferente.

![](intro-to-aspnet-mvc-3/_static/image12.png)

De forma predeterminada, esta plantilla predeterminada proporciona dos páginas para visitar y una página de inicio de sesión básica. Vamos a cambiar el funcionamiento de esta aplicación y aprender un poco sobre ASP.NET MVC en el proceso. Cierre el explorador y cambie parte del código.

> [!div class="step-by-step"]
> [Siguiente](adding-a-controller.md)
