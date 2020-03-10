---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-3
title: Usar Migraciones de Code First para inicializar la base de datos | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 76e2013a-65b7-488c-834d-9448ecea378e
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-3
msc.type: authoredcontent
ms.openlocfilehash: 257bd06848adb949330856cc71eeb3d685e9d036
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449119"
---
# <a name="use-code-first-migrations-to-seed-the-database"></a>Usar Migraciones de Code First para inicializar la base de datos

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, usará [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) en EF para inicializar la base de datos con datos de prueba.

En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana Package Manager Console, escriba el siguiente comando:

[!code-console[Main](part-3/samples/sample1.cmd)]

Este comando agrega una carpeta denominada Migrations al proyecto, además de un archivo de código denominado Configuration.cs en la carpeta Migrations.

![](part-3/_static/image1.png)

Abra el archivo Configuration.cs. Agregue la siguiente instrucción **using** .

[!code-csharp[Main](part-3/samples/sample2.cs)]

Después, agregue el código siguiente al método **Configuration. SEED** :

[!code-csharp[Main](part-3/samples/sample3.cs)]

En la ventana de la consola del administrador de paquetes, escriba los siguientes comandos:

[!code-console[Main](part-3/samples/sample4.cmd)]

El primer comando genera código que crea la base de datos y el segundo comando ejecuta ese código. La base de datos se crea localmente mediante [LocalDB](https://msdn.microsoft.com/library/hh510202.aspx).

![](part-3/_static/image2.png)

## <a name="explore-the-api-optional"></a>Explorar la API (opcional)

Presione F5 para ejecutar la aplicación en modo de depuración. Visual Studio inicia IIS Express y ejecuta la aplicación Web. A continuación, Visual Studio inicia un explorador y abre la Página principal de la aplicación.

Cuando Visual Studio ejecuta un proyecto Web, asigna un número de puerto. En la imagen siguiente, el número de puerto es 50524. Al ejecutar la aplicación, verá un número de puerto diferente.

![](part-3/_static/image3.png)

La Página principal se implementa mediante ASP.NET MVC. En la parte superior de la página, hay un vínculo que dice "API". Este vínculo le lleva a una página de ayuda generada automáticamente para la API Web. (Para obtener información sobre cómo se genera esta página de ayuda y cómo puede agregar su propia documentación a la página, consulte [crear páginas de ayuda para ASP.net web API](../../getting-started-with-aspnet-web-api/creating-api-help-pages.md)). Puede hacer clic en los vínculos de la página de ayuda para ver detalles sobre la API, incluido el formato de solicitud y respuesta.

![](part-3/_static/image4.png)

La API habilita las operaciones CRUD en la base de datos. A continuación se resume la API.

| Authors |  |
| --- | -- |
| GET api/authors | Obtiene todos los autores. |
| GET api/authors/{id} | Obtener un autor por identificador. |
| POST /api/authors | Cree un nuevo autor. |
| PUT /api/authors/{id} | Actualice un autor existente. |
| DELETE /api/authors/{id} | Eliminar un autor. |

| Libros |  |
| --- | -- |
| GET /api/books | Obtener todos los libros. |
| GET /api/books/{id} | Obtiene un libro por identificador. |
| POST /api/books | Cree un nuevo libro. |
| PUT /api/books/{id} | Actualizar un libro existente. |
| DELETE /api/books/{id} | Eliminar un libro. |

## <a name="view-the-database-optional"></a>Ver la base de datos (opcional)

Cuando ejecutó el comando UPDATE-Database, EF creó la base de datos y llamó al método `Seed`. Cuando se ejecuta la aplicación de forma local, EF usa [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). Puede ver la base de datos en Visual Studio. En el menú **Ver**, seleccione **Explorador de objetos de SQL Server**.

![](part-3/_static/image5.png)

En el cuadro de diálogo **conectar con el servidor** , en el cuadro de edición **nombre del servidor** , escriba "(LocalDB) \v11.0". Deje la opción de **autenticación** como "autenticación de Windows". Haga clic en **Conectar**.

![](part-3/_static/image6.png)

Visual Studio se conecta a LocalDB y muestra las bases de datos existentes en la ventana Explorador de objetos de SQL Server. Puede expandir los nodos para ver las tablas que ha creado EF.

![](part-3/_static/image7.png)

Para ver los datos, haga clic con el botón derecho en una tabla y seleccione **ver datos**.

![](part-3/_static/image8.png)

En la captura de pantalla siguiente se muestran los resultados de la tabla books. Observe que EF rellenó la base de datos con los datos de inicialización y la tabla contiene la clave externa de la tabla authors.

![](part-3/_static/image9.png)

> [!div class="step-by-step"]
> [Anterior](part-2.md)
> [Siguiente](part-4.md)
