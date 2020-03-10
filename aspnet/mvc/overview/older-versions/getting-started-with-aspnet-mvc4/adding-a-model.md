---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Agregar un modelo | Microsoft Docs
author: Rick-Anderson
description: 'Nota: hay disponible una versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78434419"
---
# <a name="adding-a-model"></a>Agregar un modelo

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > [Hay disponible una](../../getting-started/introduction/getting-started.md) versión actualizada de este tutorial que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro, mucho más fácil de seguir y demuestra más características.

En esta sección, agregará algunas clases para administrar películas en una base de datos de. Estas clases serán el modelo de &quot;&quot; parte de la aplicación ASP.NET MVC.

Usará una .NET Framework tecnología de acceso a datos conocida como [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) para definir y trabajar con estas clases de modelo. El Entity Framework (a menudo denominado EF) admite un paradigma de desarrollo denominado *code First*. Code First permite crear objetos de modelo escribiendo clases simples. (También se conocen como clases POCO, desde &quot;objetos CLR antiguos sin formato.&quot;) Después, puede hacer que la base de datos se cree sobre la marcha desde las clases, lo que permite un flujo de trabajo de desarrollo rápido y muy limpio.

## <a name="adding-model-classes"></a>Agregar clases de modelo

En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *modelos* , seleccione **Agregar**y, a continuación, seleccione **clase**.

![](adding-a-model/_static/image1.png)

Escriba el nombre de *clase* &quot;película&quot;.

Agregue las cinco propiedades siguientes a la clase `Movie`:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Usaremos la clase `Movie` para representar películas en una base de datos. Cada instancia de un objeto de `Movie` se corresponderá con una fila de una tabla de base de datos y cada propiedad de la clase `Movie` se asignará a una columna de la tabla.

En el mismo archivo, agregue la siguiente `MovieDBContext` clase:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

La clase `MovieDBContext` representa el contexto de la base de datos Entity Framework Movie, que controla la captura, el almacenamiento y la actualización de instancias de clase `Movie` en una base de datos. El `MovieDBContext` deriva de la clase base `DbContext` proporcionada por el Entity Framework.

Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente instrucción `using` en la parte superior del archivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

A continuación se muestra el archivo *Movie.CS* completo. (Se han quitado varias instrucciones Using que no son necesarias).

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Crear una cadena de conexión y trabajar con SQL Server LocalDB

La clase `MovieDBContext` que ha creado controla la tarea de conectar con la base de datos y asignar `Movie` objetos a los registros de la base de datos. Sin embargo, una pregunta que podría preguntar es cómo especificar a qué base de datos se conectará. Para ello, agregue la información de conexión en el archivo *Web. config* de la aplicación.

Abra el archivo *Web. config* raíz de la aplicación. (No el archivo *Web. config* en la carpeta *views* ). Abra el archivo *Web. config* que se describe en rojo.

![](adding-a-model/_static/image2.png)

Agregue la siguiente cadena de conexión al elemento `<connectionStrings>` en el archivo *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

En el ejemplo siguiente se muestra una parte del archivo *Web. config* con la nueva cadena de conexión agregada:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Esta pequeña cantidad de código y XML es todo lo que necesita escribir para representar y almacenar los datos de la película en una base de datos.

A continuación, creará una nueva clase de `MoviesController` que puede usar para mostrar los datos de la película y permitir que los usuarios creen nuevas listas de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)
