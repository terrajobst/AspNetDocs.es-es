---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
title: Agregar un modelo (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b3aa7720-5c78-4ca2-baef-9a52234fb7ce
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: e69d59aed4d74f08f1c653c4965b128c4dbe20ff
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457250"
---
# <a name="adding-a-model-vb"></a>Agregar un modelo (VB)

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación. Para instalar todos ellos, haga clic en el vínculo siguiente: [instalador de plataforma web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos con los siguientes vínculos:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(compatibilidad con herramientas y tiempo de ejecución)
> 
> Si utiliza Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos haciendo clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de VB.NET está disponible para acompañar este tema. [Descargue la versión de VB.net](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [ C# versión](../cs/adding-a-model.md) de este tutorial.

## <a name="adding-a-model"></a>Agregar un modelo

En esta sección, agregará algunas clases para administrar películas en una base de datos de. Estas clases serán la parte "modelo" de la aplicación ASP.NET MVC.

Usará una .NET Framework tecnología de acceso a datos conocida como Entity Framework para definir y trabajar con estas clases de modelo. El Entity Framework (a menudo denominado EF) admite un paradigma de desarrollo denominado *code First*. Code First permite crear objetos de modelo escribiendo clases simples. (También se conocen como clases POCO, de "objetos CLR antiguos sin formato"). Después, puede hacer que la base de datos se cree sobre la marcha desde las clases, lo que permite un flujo de trabajo de desarrollo rápido y muy limpio.

## <a name="adding-model-classes"></a>Agregar clases de modelo

En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *modelos* , seleccione **Agregar**y, a continuación, seleccione **clase**.

![](adding-a-model/_static/image1.png)

Asigne a la clase el nombre "Movie".

Agregue las cinco propiedades siguientes a la clase `Movie`:

[!code-vb[Main](adding-a-model/samples/sample1.vb)]

Usaremos la clase `Movie` para representar películas en una base de datos. Cada instancia de un objeto de `Movie` se corresponderá con una fila de una tabla de base de datos y cada propiedad de la clase `Movie` se asignará a una columna de la tabla.

En el mismo archivo, agregue la siguiente `MovieDBContext` clase:

[!code-vb[Main](adding-a-model/samples/sample2.vb)]

La clase `MovieDBContext` representa el contexto de la base de datos Entity Framework Movie, que controla la captura, el almacenamiento y la actualización de instancias de clase `Movie` en una base de datos. El `MovieDBContext` deriva de la clase base `DbContext` proporcionada por el Entity Framework. Para obtener más información sobre `DbContext` y `DbSet`, vea [mejoras de productividad para el Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0).

Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente instrucción `imports` en la parte superior del archivo:

[!code-vb[Main](adding-a-model/samples/sample3.vb)]

El archivo *Movie. VB* completo se muestra a continuación.

[!code-vb[Main](adding-a-model/samples/sample4.vb)]

## <a name="creating-a-connection-string-and-working-with-sql-server-compact"></a>Crear una cadena de conexión y trabajar con SQL Server Compact

La clase `MovieDBContext` que ha creado controla la tarea de conectar con la base de datos y asignar `Movie` objetos a los registros de la base de datos. Sin embargo, una pregunta que podría preguntar es cómo especificar a qué base de datos se conectará. Para ello, agregue la información de conexión en el archivo *Web. config* de la aplicación.

Abra el archivo *Web. config* raíz de la aplicación. (No el archivo *Web. config* en la carpeta *views* ). En la imagen siguiente se muestran los archivos *Web. config.* Abra el archivo *Web. config* en un círculo rojo.

![](adding-a-model/_static/image2.png)

Agregue la siguiente cadena de conexión al elemento `<connectionStrings>` en el archivo *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

En el ejemplo siguiente se muestra una parte del archivo *Web. config* con la nueva cadena de conexión agregada:

[!code-xml[Main](adding-a-model/samples/sample6.xml)]

Esta pequeña cantidad de código y XML es todo lo que necesita escribir para representar y almacenar los datos de la película en una base de datos.

A continuación, creará una nueva clase de `MoviesController` que puede usar para mostrar los datos de la película y permitir que los usuarios creen nuevas listas de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)
