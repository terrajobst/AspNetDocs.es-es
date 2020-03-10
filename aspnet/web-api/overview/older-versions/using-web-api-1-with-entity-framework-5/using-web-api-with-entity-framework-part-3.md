---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
title: 'Parte 3: creación de un controlador de administración | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 6b9ae3c4-0274-4170-a1bb-9df9c546b2a9
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-3
msc.type: authoredcontent
ms.openlocfilehash: f39be7a84e85db93487d246e9f8cb59c401fe5ce
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447913"
---
# <a name="part-3-creating-an-admin-controller"></a>Parte 3: creación de un controlador de administración

por [Mike Wasson](https://github.com/MikeWasson)

[Descargar proyecto completado](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-controller"></a>Agregar un controlador de administración

En esta sección, agregaremos un controlador de API Web que admita operaciones CRUD (creación, lectura, actualización y eliminación) en productos. El controlador usará Entity Framework para comunicarse con el nivel de base de datos. Solo los administradores podrán usar este controlador. Los clientes tendrán acceso a los productos a través de otro controlador.

En Explorador de soluciones, haga clic con el botón secundario en la carpeta Controllers. Seleccione **Agregar** y después **controlador**.

![](using-web-api-with-entity-framework-part-3/_static/image1.png)

En el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre `AdminController`. En **plantilla**, seleccione &quot;controlador de API con acciones de lectura y escritura con Entity Framework&quot;. En **clase de modelo**, seleccione "Product (ProductStore. Models)". En **contexto de datos**, seleccione "&lt;nuevo contexto de datos&gt;".

![](using-web-api-with-entity-framework-part-3/_static/image2.png)

> [!NOTE]
> Si la lista desplegable **clase de modelo** no muestra ninguna clase de modelo, asegúrese de que compiló el proyecto. Entity Framework usa la reflexión, por lo que necesita el ensamblado compilado.

Al seleccionar "&lt;nuevo contexto de datos&gt;", se abrirá el cuadro de diálogo **nuevo contexto de datos** . Asigne un nombre al `ProductStore.Models.OrdersContext`de contexto de datos.

![](using-web-api-with-entity-framework-part-3/_static/image3.png)

Haga clic en **Aceptar** para descartar el cuadro de diálogo **nuevo contexto de datos** . En el cuadro de diálogo **Agregar controlador** , haga clic en **Agregar**.

Esto es lo que se ha agregado al proyecto:

- Una clase denominada `OrdersContext` que se deriva de **DbContext**. Esta clase proporciona el pegado entre los modelos POCO y la base de datos.
- Un controlador de API Web denominado `AdminController`. Este controlador admite operaciones CRUD en instancias de `Product`. Usa la clase `OrdersContext` para comunicarse con Entity Framework.
- Una nueva cadena de conexión de base de datos en el archivo Web. config.

![](using-web-api-with-entity-framework-part-3/_static/image4.png)

Abra el archivo OrdersContext.cs. Observe que el constructor especifica el nombre de la cadena de conexión de base de datos. Este nombre hace referencia a la cadena de conexión que se ha agregado a Web. config.

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample1.cs)]

Agregue las propiedades siguientes a la clase `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample2.cs)]

Un **DbSet** representa un conjunto de entidades que se pueden consultar. Esta es la lista completa de la clase `OrdersContext`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample3.cs)]

La clase `AdminController` define cinco métodos que implementan la funcionalidad CRUD básica. Cada método corresponde a un URI que el cliente puede invocar:

| Método de controlador | Description | Identificador URI | Método HTTP |
| --- | --- | --- | --- |
| GetProducts | Obtiene todos los productos. | API/productos | GET |
| GetProduct | Busca un producto por identificador. | API/productos/*ID.* | GET |
| PutProduct | Actualiza un producto. | API/productos/*ID.* | PUT |
| Postproducto | Crea un producto nuevo. | API/productos | EXPONER |
| DeleteProduct | Elimina un producto. | API/productos/*ID.* | SUPRIMIR |

Cada método llama a `OrdersContext` para consultar la base de datos. Los métodos que modifican la colección (PUT, POST y DELETE) llaman a `db.SaveChanges` para conservar los cambios en la base de datos. Los controladores se crean por solicitud HTTP y, a continuación, se eliminan, por lo que es necesario conservar los cambios antes de que se devuelva un método.

## <a name="add-a-database-initializer"></a>Agregar un inicializador de base de datos

Entity Framework tiene una característica agradable que le permite rellenar la base de datos al iniciarse y volver a crear automáticamente la base de datos cada vez que cambien los modelos. Esta característica es útil durante el desarrollo, porque siempre tiene algunos datos de prueba, aunque cambie los modelos.

En Explorador de soluciones, haga clic con el botón secundario en la carpeta modelos y cree una nueva clase denominada `OrdersContextInitializer`. Pegue la siguiente implementación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample4.cs)]

Al heredar de la clase **DropCreateDatabaseIfModelChanges** , estamos indicando Entity Framework a quitar la base de datos siempre que se modifiquen las clases del modelo. Cuando Entity Framework crea (o vuelve a crear) la base de datos, llama al método de **inicialización** para rellenar las tablas. Usamos el método **SEED** para agregar algunos productos de ejemplo más un orden de ejemplo.

Esta característica es ideal para las pruebas, pero no se usa la clase **DropCreateDatabaseIfModelChanges** en producción,, porque se podrían perder los datos si alguien cambia una clase de modelo.

A continuación, abra global. asax y agregue el código siguiente al método de **Inicio de\_** de la aplicación:

[!code-csharp[Main](using-web-api-with-entity-framework-part-3/samples/sample5.cs)]

## <a name="send-a-request-to-the-controller"></a>Enviar una solicitud al controlador

En este momento, no hemos escrito ningún código de cliente, pero puede invocar la API Web mediante un explorador Web o una herramienta de depuración HTTP como [Fiddler](http://www.fiddler2.com/fiddler2/). En Visual Studio, presione F5 para iniciar la depuración. El explorador Web se abrirá en `http://localhost:*portnum*/`, donde *portnum* es un número de puerto.

Envíe una solicitud HTTP a "`http://localhost:*portnum*/api/admin`. Es posible que la primera solicitud tarde en completarse, porque Entity Framework necesita crear e inicializar la base de datos. La respuesta debe ser similar a la siguiente:

[!code-console[Main](using-web-api-with-entity-framework-part-3/samples/sample6.cmd)]

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-2.md)
> [Siguiente](using-web-api-with-entity-framework-part-4.md)
