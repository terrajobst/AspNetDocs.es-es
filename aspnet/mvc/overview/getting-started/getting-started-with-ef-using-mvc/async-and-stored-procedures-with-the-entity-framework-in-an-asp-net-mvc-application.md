---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
title: 'Tutorial: uso de procedimientos almacenados y asincrónicos con EF en una aplicación ASP.NET MVC'
description: En este tutorial, verá cómo implementar el modelo de programación asincrónica y cómo usar procedimientos almacenados.
author: tdykstra
ms.author: riande
ms.date: 01/18/2019
ms.topic: tutorial
ms.assetid: 27d110fc-d1b7-4628-a763-26f1e6087549
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 5612f2f25d06feb904a205505ed8f048d2263266
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471391"
---
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a>Tutorial: uso de procedimientos almacenados y asincrónicos con EF en una aplicación ASP.NET MVC

En los tutoriales anteriores, aprendió a leer y actualizar datos mediante el modelo de programación sincrónica. En este tutorial, verá cómo implementar el modelo de programación asincrónica. El código asincrónico puede ayudar a que una aplicación funcione mejor porque hace un mejor uso de los recursos del servidor.

En este tutorial también verá cómo usar procedimientos almacenados para las operaciones de inserción, actualización y eliminación en una entidad.

Por último, vuelva a implementar la aplicación en Azure, junto con todos los cambios de base de datos que ha implementado desde la primera vez que implementó.

En las ilustraciones siguientes se muestran algunas de las páginas con las que va a trabajar.

![Página departamentos](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Crear Departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

En este tutorial va a:

> [!div class="checklist"]
> * Más información sobre el código asincrónico
> * Crear un controlador de Departamento
> * Usar procedimientos almacenados
> * Implementar en Azure

## <a name="prerequisites"></a>Requisitos previos

* [Actualización de datos relacionados](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a>Por qué usar código asincrónico

Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso. Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen. Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S. Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes. Como resultado, el código asincrónico permite el uso más eficaz de los recursos del servidor y el servidor está habilitado para administrar más tráfico sin retrasos.

En versiones anteriores de .NET, escribir y probar código asincrónico era complejo, propenso a errores y difícil de depurar. En .NET 4,5, escribir, probar y depurar código asincrónico es mucho más fácil de escribir en general, a menos que tenga un motivo que no sea. El código asincrónico introduce una pequeña cantidad de sobrecarga, pero para situaciones de tráfico reducido, la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, la posible mejora del rendimiento es considerable.

Para obtener más información sobre la programación asincrónica, vea [uso de la compatibilidad asincrónica de .net 4.5 para evitar llamadas de bloqueo](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).

## <a name="create-department-controller"></a>Crear controlador de Departamento

Cree un controlador de Departamento de la misma manera que hizo con los controladores anteriores, pero esta vez Active la casilla **usar acciones de controlador asincrónico** .

Los siguientes resaltados muestran lo que se agregó al código sincrónico para el método `Index` para que sea asincrónico:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

Se aplicaron cuatro cambios para permitir que la consulta de base de datos de Entity Framework se ejecute de forma asincrónica:

- El método se marca con la palabra clave `async`, que indica al compilador que genere devoluciones de llamada para partes del cuerpo del método y que cree automáticamente el objeto `Task<ActionResult>` que se devuelve.
- El tipo de valor devuelto cambió de `ActionResult` a `Task<ActionResult>`. El tipo de `Task<T>` representa el trabajo en curso con un resultado de tipo `T`.
- La palabra clave `await` se aplicó a la llamada al servicio Web. Cuando el compilador ve esta palabra clave, en segundo plano divide el método en dos partes. La primera parte finaliza con la operación que se inicia de forma asincrónica. La segunda parte se coloca en un método de devolución de llamada al que se llama cuando se completa la operación.
- Se llamó a la versión asincrónica del método de extensión `ToList`.

¿Por qué se ha modificado la instrucción `departments.ToList` pero no la instrucción `departments = db.Departments`? La razón es que solo se ejecutan de forma asincrónica las instrucciones que hacen que las consultas o los comandos se envíen a la base de datos. La instrucción `departments = db.Departments` configura una consulta, pero la consulta no se ejecuta hasta que se llama al método `ToList`. Por lo tanto, solo se ejecuta de forma asincrónica el método `ToList`.

En el método `Details` y los métodos `Edit` y `Delete` `HttpGet`, el método `Find` es el que hace que se envíe una consulta a la base de datos, por lo que es el método que se ejecuta de forma asincrónica:

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

En los métodos `Create`, `HttpPost Edit`y `DeleteConfirmed`, es la llamada al método `SaveChanges` que hace que se ejecute un comando, no instrucciones como `db.Departments.Add(department)` que solo hacen que se modifiquen las entidades de la memoria.

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

Abra *Views\Department\Index.cshtml*y reemplace el código de plantilla por el código siguiente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

Este código cambia el título de index a departments, mueve el nombre del administrador a la derecha y proporciona el nombre completo del administrador.

En las vistas crear, eliminar, detalles y editar, cambie el título del campo `InstructorID` a "administrador" de la misma manera que cambió el campo Nombre del Departamento a "Departamento" en las vistas del curso.

En las vistas crear y editar, use el código siguiente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

En las vistas eliminar y detalles, use el código siguiente:

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

Ejecute la aplicación y haga clic en la pestaña **departments** .

Todo funciona igual que en los demás controladores, pero en este controlador todas las consultas SQL se ejecutan de forma asincrónica.

Algunos aspectos que se deben tener en cuenta cuando se usa la programación asincrónica con la Entity Framework:

- El código asincrónico no es seguro para subprocesos. En otras palabras, en otras palabras, no intente realizar varias operaciones en paralelo con la misma instancia de contexto.
- Si quiere aprovechar las ventajas de rendimiento del código asincrónico, asegúrese de que en los paquetes de biblioteca que use (por ejemplo para paginación), también se usa async si llaman a cualquier método de Entity Framework que haga que las consultas se envíen a la base de datos.

## <a name="use-stored-procedures"></a>Usar procedimientos almacenados

Algunos desarrolladores y DBA prefieren usar procedimientos almacenados para el acceso a la base de datos. En versiones anteriores de Entity Framework se pueden recuperar datos mediante un procedimiento almacenado mediante [la ejecución de una consulta SQL sin formato](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), pero no se puede indicar a EF que use procedimientos almacenados para las operaciones de actualización. En EF 6 es fácil configurar Code First para usar procedimientos almacenados.

1. En *DAL\SchoolContext.CS*, agregue el código resaltado al método `OnModelCreating`.

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    Este código indica a Entity Framework que utilice procedimientos almacenados para las operaciones de inserción, actualización y eliminación en la entidad `Department`.
2. En la consola de administración de paquetes, escriba el siguiente comando:

    `add-migration DepartmentSP`

    Abra *migraciones\\&lt;timestamp&gt;\_DepartmentSP.CS* para ver el código en el método `Up` que crea procedimientos almacenados de inserción, actualización y eliminación:

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. En la consola de administración de paquetes, escriba el siguiente comando:

     `update-database`
4. Ejecute la aplicación en modo de depuración, haga clic en la pestaña **departamentos** y, a continuación, haga clic en **crear nuevo**.
5. Escriba los datos de un nuevo departamento y, a continuación, haga clic en **crear**.

6. En Visual Studio, examine los registros de la ventana de **salida** para ver que se ha utilizado un procedimiento almacenado para insertar la nueva fila Department.

     ![Proveedor de inserción de Departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

Code First crea nombres de procedimientos almacenados predeterminados. Si está utilizando una base de datos existente, es posible que tenga que personalizar los nombres de los procedimientos almacenados para poder utilizar los procedimientos almacenados ya definidos en la base de datos. Para obtener información sobre cómo hacerlo, vea [Entity Framework Code First procedimientos almacenados INSERT, Update y DELETE](https://msdn.microsoft.com/data/dn468673).

Si desea personalizar los procedimientos almacenados generados, puede editar el código con scaffolding para el método Migrations `Up` que crea el procedimiento almacenado. De este modo, los cambios se reflejarán siempre que se ejecute la migración y se apliquen a la base de datos de producción cuando las migraciones se ejecuten automáticamente en producción después de la implementación.

Si desea cambiar un procedimiento almacenado existente creado en una migración anterior, puede usar el comando Add-Migration para generar una migración en blanco y, a continuación, escribir manualmente el código que llama al método [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) .

## <a name="deploy-to-azure"></a>Implementar en Azure

En esta sección, es necesario haber completado la sección implementación opcional de **la aplicación en Azure** en el tutorial sobre [migraciones e implementación](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) de esta serie. Si tuviera errores de migración que resolvió eliminando la base de datos en el proyecto local, omita esta sección.

1. En Visual Studio, haga clic con el botón derecho en el proyecto, en el **Explorador de soluciones** y seleccione **Publicar** en el menú contextual.
2. Haga clic en **Publicar**.

    Visual Studio implementa la aplicación en Azure y la aplicación se abre en el explorador predeterminado, que se ejecuta en Azure.
3. Pruebe la aplicación para comprobar que funciona.

    La primera vez que se ejecuta una página que tiene acceso a la base de datos, el Entity Framework ejecuta todas las migraciones `Up` métodos necesarios para actualizar la base de datos con el modelo de datos actual. Ahora puede usar todas las páginas web que agregó desde la última vez que implementó, incluidas las páginas de departamento que agregó en este tutorial.

## <a name="get-the-code"></a>Obtención del código

[Descargar el proyecto completado](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>Recursos adicionales

Los vínculos a otros recursos de Entity Framework pueden encontrarse en el [acceso a datos ASP.net: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial va a:

> [!div class="checklist"]
> * Información sobre el código asincrónico
> * Creación de un controlador de Departamento
> * Procedimientos almacenados usados
> * Implementado en Azure

Avance al siguiente artículo para obtener información sobre cómo controlar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo.
> [!div class="nextstepaction"]
> [Controlar la simultaneidad](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
