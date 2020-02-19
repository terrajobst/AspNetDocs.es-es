---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Agregar un nuevo campo a la tabla y al modelo deC#películas () | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web MVC de ASP.NET con Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 40b02a2f608f07091ce6b5339688a1e6290e2e37
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457468"
---
# <a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Agregar un nuevo campo a la tabla y modelo de películas (C#)

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

En esta sección, realizará algunos cambios en las clases del modelo y aprenderá a actualizar el esquema de la base de datos para que coincida con los cambios del modelo.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Comience agregando una nueva propiedad `Rating` a la clase `Movie` existente. Abra el archivo *Movie.CS* y agregue la propiedad `Rating` como esta:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

La clase `Movie` completa ahora es similar al código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Vuelva a compilar la aplicación mediante el comando de menú **Depurar** &gt;**compilar película** .

Ahora que ha actualizado la clase `Model`, también debe actualizar las plantillas de vista *\Views\Movies\Index.cshtml* y *\Views\Movies\Create.cshtml* para admitir la nueva propiedad `Rating`.

Abra el archivo *\Views\Movies\Index.cshtml* y agregue un encabezado de columna `<th>Rating</th>` justo después de la columna **Price** . A continuación, agregue una `<td>` columna cerca del final de la plantilla para representar el valor `@item.Rating`. A continuación se muestra el aspecto de la plantilla de vista *index. cshtml* actualizada:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

A continuación, abra el archivo *\Views\Movies\Create.cshtml* y agregue el siguiente marcado cerca del final del formulario. Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una nueva película.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Administrar las diferencias de los esquemas de base de datos y modelo

Ahora ha actualizado el código de aplicación para admitir la nueva propiedad `Rating`.

Ahora ejecute la aplicación y vaya a la dirección URL de */Movies* . Sin embargo, cuando lo haga, verá el siguiente error:

![](adding-a-new-field/_static/image1.png)

Está viendo este error porque la clase del modelo de `Movie` actualizada en la aplicación es ahora diferente del esquema de la tabla de `Movie` de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).

De forma predeterminada, cuando se utiliza Entity Framework Code First para crear automáticamente una base de datos, como hizo anteriormente en este tutorial, Code First agrega una tabla a la base de datos para realizar un seguimiento de si el esquema de la base de datos está sincronizado con las clases de modelo desde las que se generó. Si no están sincronizados, el Entity Framework produce un error. Esto facilita el seguimiento de los problemas en el tiempo de desarrollo que, de otra forma, podría encontrar (mediante errores ocultos) en tiempo de ejecución. La característica de comprobación de sincronización es lo que hace que se muestre el mensaje de error que acaba de ver.

Existen dos métodos para resolver el error:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque es muy práctico cuando se realiza el desarrollo activo en una base de datos de prueba, ya que permite desarrollar rápidamente el modelo y el esquema de la base de datos juntos. Sin embargo, el inconveniente es que se pierden los datos existentes en la base de datos, por lo que *no* se desea usar este enfoque en una base de datos de producción.
2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.

En este tutorial, usaremos el primer enfoque: tendrá el Entity Framework Code First volver a crear automáticamente la base de datos siempre que cambie el modelo.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Volver a crear automáticamente la base de datos al cambiar el modelo

Vamos a actualizar la aplicación para que Code First quita y vuelve a crear la base de datos automáticamente cada vez que cambia el modelo de la aplicación.

> [!NOTE] 
> 
> **ADVERTENCIA** de Debe habilitar este método para quitar y volver a crear automáticamente la base de datos solo cuando se utiliza una base de datos de desarrollo o de prueba, y *nunca* en una base de datos de producción que contiene datos reales. Su uso en un servidor de producción puede provocar la pérdida de datos.

En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *modelos* , seleccione **Agregar**y, a continuación, seleccione **clase**.

![](adding-a-new-field/_static/image2.png)

Asigne a la clase el nombre "MovieInitializer". Actualice la clase `MovieInitializer` para que contenga el código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

La clase `MovieInitializer` especifica que la base de datos utilizada por el modelo debe quitarse y volver a crearse automáticamente si las clases del modelo cambian. El código incluye un método `Seed` para especificar algunos datos predeterminados que se agregarán automáticamente a la base de datos cada vez que se cree (o se vuelva a crear). Esto proporciona una manera útil de rellenar la base de datos con algunos datos de ejemplo, sin necesidad de rellenarla manualmente cada vez que realice un cambio en el modelo.

Ahora que ha definido la clase `MovieInitializer`, querrá conectarla para que cada vez que se ejecute la aplicación, compruebe si las clases de modelo son diferentes del esquema de la base de datos. Si es así, puede ejecutar el inicializador para volver a crear la base de datos para que coincida con el modelo y, a continuación, rellenar la base de datos con los datos de ejemplo.

Abra el archivo *global. asax* que se encuentra en la raíz del proyecto `MvcMovies`:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

El archivo *global. asax* contiene la clase que define la aplicación completa del proyecto y contiene un controlador de eventos `Application_Start` que se ejecuta cuando la aplicación se inicia por primera vez.

Vamos a agregar dos instrucciones Using a la parte superior del archivo. El primero hace referencia al espacio de nombres Entity Framework y el segundo hace referencia al espacio de nombres en el que vive nuestra clase `MovieInitializer`:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

A continuación, busque el método `Application_Start` y agregue una llamada a `Database.SetInitializer` al principio del método, como se muestra a continuación:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

La instrucción `Database.SetInitializer` que acaba de agregar indica que la base de datos utilizada por la instancia de `MovieDBContext` debe eliminarse y volver a crearse automáticamente si el esquema y la base de datos no coinciden. Y como vio, también rellenará la base de datos con los datos de ejemplo que se especifican en la clase `MovieInitializer`.

Cierre el archivo *global. asax* .

Vuelva a ejecutar la aplicación y vaya a la dirección URL de */Movies* . Cuando se inicia la aplicación, detecta que la estructura del modelo ya no coincide con el esquema de la base de datos. Vuelve a crear automáticamente la base de datos para que coincida con la nueva estructura del modelo y rellena la base de datos con las películas de ejemplo:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Haga clic en el vínculo **crear nuevo** para agregar una nueva película. Tenga en cuenta que puede Agregar una clasificación.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Haga clic en **Crear**. La nueva película, incluida la clasificación, se muestra ahora en la lista de películas:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

En esta sección vio cómo puede modificar objetos de modelo y mantener la base de datos sincronizada con los cambios. También aprendió una manera de rellenar una base de datos recién creada con datos de ejemplo para que pueda probar escenarios. A continuación, echemos un vistazo a cómo puede Agregar una lógica de validación más enriquecida a las clases de modelo y permitir que se apliquen algunas reglas de negocios.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-validation-to-the-model.md)
