---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Agregar un modelo | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 0d926c7a8bd99c56820208921c10e609da56d236
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519042"
---
# <a name="adding-a-model"></a>Agregar un modelo

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

En esta sección, agregará algunas clases para administrar películas en una base de datos de. Estas clases serán el modelo de &quot;&quot; parte de la aplicación ASP.NET MVC.

Usará una .NET Framework tecnología de acceso a datos conocida como [Entity Framework](https://docs.microsoft.com/ef/) para definir y trabajar con estas clases de modelo. El Entity Framework (a menudo denominado EF) admite un paradigma de desarrollo denominado *code First*. Code First permite crear objetos de modelo escribiendo clases simples. (También se conocen como clases POCO, desde &quot;objetos CLR antiguos sin formato.&quot;) Después, puede hacer que la base de datos se cree sobre la marcha desde las clases, lo que permite un flujo de trabajo de desarrollo rápido y muy limpio. Si necesita crear primero la base de datos, puede seguir este tutorial para obtener información sobre el desarrollo de aplicaciones MVC y EF. Después, puede seguir el tutorial de [scaffolding Fizmakens ASP.net](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) , que cubre el primer enfoque de la base de datos.

## <a name="adding-model-classes"></a>Agregar clases de modelo

En **Explorador de soluciones**, haga clic con el botón derecho en la carpeta *modelos* , seleccione **Agregar**y, a continuación, seleccione **clase**.

![](adding-a-model/_static/image1.png)

Escriba el nombre de *clase* &quot;película&quot;.

Agregue las cinco propiedades siguientes a la clase `Movie`:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Usaremos la clase `Movie` para representar películas en una base de datos. Cada instancia de un objeto de `Movie` se corresponderá con una fila de una tabla de base de datos y cada propiedad de la clase `Movie` se asignará a una columna de la tabla.

Nota: para poder usar System. Data. Entity y la clase relacionada, debe instalar el [paquete NuGet de Entity Framework](https://www.nuget.org/packages/EntityFramework/). Siga el vínculo para obtener más instrucciones.

En el mismo archivo, agregue la siguiente `MovieDBContext` clase:

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

La clase `MovieDBContext` representa el contexto de la base de datos Entity Framework Movie, que controla la captura, el almacenamiento y la actualización de instancias de clase `Movie` en una base de datos. El `MovieDBContext` deriva de la clase base `DbContext` proporcionada por el Entity Framework.

Para poder hacer referencia a `DbContext` y `DbSet`, debe agregar la siguiente instrucción `using` en la parte superior del archivo:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Para ello, agregue manualmente la instrucción using, o bien, puede mantener el mouse sobre las líneas onduladas rojas, hacer clic en `Show potential fixes` y hacer clic en `using System.Data.Entity;`

![](adding-a-model/_static/image2.png)

Nota: se han quitado varias instrucciones `using` sin usar. Visual Studio mostrará las dependencias no usadas en gris. Para quitar las dependencias no utilizadas, mantenga el mouse sobre las dependencias grises, haga clic en `Show potential fixes` y haga clic en quitar las utilizaciones **no utilizadas.**

![](adding-a-model/_static/image3.png)

Por último, hemos agregado un modelo (la M en MVC). En la siguiente sección, trabajará con la cadena de conexión de base de datos.

> [!div class="step-by-step"]
> [Anterior](adding-a-view.md)
> [Siguiente](creating-a-connection-string.md)
