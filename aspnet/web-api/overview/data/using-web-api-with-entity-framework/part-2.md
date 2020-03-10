---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-2
title: Agregar modelos y controladores | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 88908ff8-51a9-40eb-931c-a8139128b680
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-2
msc.type: authoredcontent
ms.openlocfilehash: 57dacda421968f341284d89c9a3ad80040c16e25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78449191"
---
# <a name="add-models-and-controllers"></a>Agregar modelos y controladores

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://github.com/MikeWasson/BookService)

En esta sección, agregará clases de modelo que definen las entidades de base de datos. A continuación, agregará controladores de API Web que realizan operaciones CRUD en esas entidades.

## <a name="add-model-classes"></a>Agregar clases de modelo

En este tutorial, crearemos la base de datos con el enfoque "Code First" para Entity Framework (EF). Con Code First, escribe C# las clases que corresponden a las tablas de base de datos y EF crea la base de datos. (Para obtener más información, vea [métodos de desarrollo de Entity Framework](https://msdn.microsoft.com/library/ms178359%28v=vs.110%29.aspx#dbfmfcf)).

Comenzaremos definiendo nuestros objetos de dominio como POCO (objetos CLR antiguos sin formato). Se crearán los siguientes POCO:

- Autor
- Book

En Explorador de soluciones, haga clic con el botón secundario en la carpeta modelos. Seleccione **Agregar**y, a continuación, seleccione **clase**. Asigne a la clase el nombre `Author`.

![](part-2/_static/image1.png)

Reemplace todo el código reutilizable de Author.cs por el código siguiente.

[!code-csharp[Main](part-2/samples/sample1.cs)]

Agregue otra clase denominada `Book`, con el código siguiente.

[!code-csharp[Main](part-2/samples/sample2.cs)]

Entity Framework usará estos modelos para crear tablas de base de datos. Para cada modelo, la propiedad `Id` se convertirá en la columna de clave principal de la tabla de base de datos.

En la clase Book, el `AuthorId` define una clave externa en la tabla `Author`. (Por motivos de simplicidad, supongo que cada libro tiene un solo autor). La clase Book también contiene una propiedad de navegación a la `Author`relacionada. Puede usar la propiedad de navegación para tener acceso al `Author` relacionado en el código. Me digo más sobre las propiedades de navegación en la parte 4, el control de las [relaciones de entidad](part-4.md).

## <a name="add-web-api-controllers"></a>Agregar controladores de API Web

En esta sección, agregaremos controladores de API Web que admitan las operaciones CRUD (crear, leer, actualizar y eliminar). Los controladores utilizarán Entity Framework para comunicarse con el nivel de base de datos.

En primer lugar, puede eliminar los controladores de archivos/clase valuescontroller. cs. Este archivo contiene un controlador de API Web de ejemplo, pero no lo necesita para este tutorial.

![](part-2/_static/image2.png)

A continuación, compile el proyecto. El scaffolding de API Web utiliza la reflexión para buscar las clases de modelo, por lo que necesita el ensamblado compilado.

En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers. Seleccione **Agregar**y, a continuación, seleccione **controlador**.

![](part-2/_static/image3.png)

En el cuadro de diálogo **Agregar scaffold** , seleccione "controlador Web API 2 con acciones, usando Entity Framework". Haga clic en **Agregar**.

![](part-2/_static/image4.png)

En el cuadro de diálogo **Agregar controlador** , haga lo siguiente:

1. En el menú desplegable **clase de modelo** , seleccione la clase `Author`. (Si no aparece en la lista desplegable, asegúrese de que ha compilado el proyecto).
2. Active "usar acciones de controlador Async".
3. Deje el nombre del controlador como &quot;AuthorsController&quot;.
4. Haga clic en el botón más (+) situado junto a **clase de contexto de datos**.

![](part-2/_static/image5.png)

En el cuadro de diálogo **nuevo contexto de datos** , deje el nombre predeterminado y haga clic en **Agregar**.

![](part-2/_static/image6.png)

Haga clic en **Agregar** para completar el cuadro de diálogo **Agregar controlador** . El cuadro de diálogo agrega dos clases al proyecto:

- `AuthorsController` define un controlador de API Web. El controlador implementa la API de REST que usan los clientes para realizar operaciones CRUD en la lista de autores.
- `BookServiceContext` administra objetos entidad durante el tiempo de ejecución, lo que incluye llenar objetos con datos de una base de datos, realizar un seguimiento de los cambios y conservar los datos en la base de datos. Hereda de `DbContext`.

![](part-2/_static/image7.png)

En este punto, vuelva a compilar el proyecto. Ahora siga los mismos pasos para agregar un controlador de API para `Book` entidades. Esta vez, seleccione `Book` para la clase de modelo y seleccione la clase de `BookServiceContext` existente para la clase de contexto de datos. (No cree un nuevo contexto de datos). Haga clic en **Agregar** para agregar el controlador.

![](part-2/_static/image8.png)

> [!div class="step-by-step"]
> [Anterior](part-1.md)
> [Siguiente](part-3.md)
