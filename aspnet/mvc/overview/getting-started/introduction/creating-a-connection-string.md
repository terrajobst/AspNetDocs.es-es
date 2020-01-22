---
uid: mvc/overview/getting-started/introduction/creating-a-connection-string
title: Crear una cadena de conexión y trabajar con SQL Server LocalDB | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 10/17/2013
ms.assetid: 6127804d-c1a9-414d-8429-7f3dd0f56e97
msc.legacyurl: /mvc/overview/getting-started/introduction/creating-a-connection-string
msc.type: authoredcontent
ms.openlocfilehash: d3c6e736c5dcf4a3615e3c72cfc033effc7cc8e6
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519315"
---
# <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Crear una cadena de conexión y trabajar con SQL Server LocalDB

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](index.md)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Crear una cadena de conexión y trabajar con SQL Server LocalDB

La clase `MovieDBContext` que ha creado controla la tarea de conectar con la base de datos y asignar `Movie` objetos a los registros de la base de datos. Sin embargo, una pregunta que podría preguntar es cómo especificar a qué base de datos se conectará. En realidad, no tiene que especificar la base de datos que se va a usar, Entity Framework usará de forma predeterminada [LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb). En esta sección, agregaremos explícitamente una cadena de conexión en el archivo *Web. config* de la aplicación.

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-2016-express-localdb) es una versión ligera de la SQL Server Express motor de base de datos que se inicia a petición y se ejecuta en modo de usuario. LocalDB se ejecuta en un modo de ejecución especial de SQL Server Express que le permite trabajar con bases de datos como archivos *. MDF* . Normalmente, los archivos de base de datos LocalDB se guardan en la carpeta *App\_Data* de un proyecto Web.

No se recomienda el uso de SQL Server Express en aplicaciones Web de producción. En concreto, LocalDB no debe usarse para producción con una aplicación web porque no está diseñado para funcionar con IIS. Sin embargo, una base de datos de LocalDB se puede migrar fácilmente a SQL Server o SQL Azure.

En Visual Studio 2017, LocalDB se instala de forma predeterminada con Visual Studio.

De forma predeterminada, la Entity Framework busca una cadena de conexión denominada igual que la clase de contexto del objeto (`MovieDBContext` para este proyecto). Para obtener más información, vea [SQL Server cadenas de conexión para las aplicaciones Web de ASP.net](https://msdn.microsoft.com/library/jj653752.aspx).

Abra el archivo *Web. config* raíz de la aplicación que se muestra a continuación. (No el archivo *Web. config* en la carpeta *views* ).

![](creating-a-connection-string/_static/image1.png)

Busque el elemento `<connectionStrings>`:

![](creating-a-connection-string/_static/image2.png)

Agregue la siguiente cadena de conexión al elemento `<connectionStrings>` en el archivo *Web. config* .

[!code-xml[Main](creating-a-connection-string/samples/sample1.xml)]

En el ejemplo siguiente se muestra una parte del archivo *Web. config* con la nueva cadena de conexión agregada:

[!code-xml[Main](creating-a-connection-string/samples/sample2.xml)]

Las dos cadenas de conexión son muy similares. La primera cadena de conexión se denomina `DefaultConnection` y se utiliza para que la base de datos de pertenencia controle quién puede tener acceso a la aplicación. La cadena de conexión que ha agregado especifica una base de datos LocalDB denominada *Movie. MDF* ubicada en la carpeta *App\_Data* . No usaremos la base de datos de pertenencia en este tutorial. para obtener más información sobre la pertenencia, la autenticación y la seguridad, consulte mi tutorial [creación de una aplicación de ASP.NET MVC con autenticación y base de datos SQL e implementación en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).

El nombre de la cadena de conexión debe coincidir con el nombre de la clase [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) .

[!code-csharp[Main](creating-a-connection-string/samples/sample3.cs?highlight=15)]

En realidad, no es necesario agregar la cadena de conexión de `MovieDBContext`. Si no especifica una cadena de conexión, Entity Framework creará una base de datos LocalDB en el directorio usuarios con el nombre completo de la clase [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx) (en este caso `MvcMovie.Models.MovieDBContext`). Puede asignar a la base de datos el nombre que desee, siempre y cuando tenga el *. Sufijo MDF* . Por ejemplo, podríamos asignar un nombre a la base de datos *. MDF*.

A continuación, creará una nueva clase de `MoviesController` que puede usar para mostrar los datos de la película y permitir que los usuarios creen nuevas listas de películas.

> [!div class="step-by-step"]
> [Anterior](adding-a-model.md)
> [Siguiente](accessing-your-models-data-from-a-controller.md)
