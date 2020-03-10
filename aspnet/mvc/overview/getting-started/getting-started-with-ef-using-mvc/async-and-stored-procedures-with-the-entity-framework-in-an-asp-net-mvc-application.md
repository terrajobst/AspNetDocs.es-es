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
# <a name="tutorial-use-async-and-stored-procedures-with-ef-in-an-aspnet-mvc-app"></a><span data-ttu-id="3f3b0-103">Tutorial: uso de procedimientos almacenados y asincrónicos con EF en una aplicación ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3f3b0-103">Tutorial: Use async and stored procedures with EF in an ASP.NET MVC App</span></span>

<span data-ttu-id="3f3b0-104">En los tutoriales anteriores, aprendió a leer y actualizar datos mediante el modelo de programación sincrónica.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-104">In earlier tutorials you learned how to read and update data using the synchronous programming model.</span></span> <span data-ttu-id="3f3b0-105">En este tutorial, verá cómo implementar el modelo de programación asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-105">In this tutorial you see how to implement the asynchronous programming model.</span></span> <span data-ttu-id="3f3b0-106">El código asincrónico puede ayudar a que una aplicación funcione mejor porque hace un mejor uso de los recursos del servidor.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-106">Asynchronous code can help an application perform better because it makes better use of server resources.</span></span>

<span data-ttu-id="3f3b0-107">En este tutorial también verá cómo usar procedimientos almacenados para las operaciones de inserción, actualización y eliminación en una entidad.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-107">In this tutorial you also see how to use stored procedures for insert, update, and delete operations on an entity.</span></span>

<span data-ttu-id="3f3b0-108">Por último, vuelva a implementar la aplicación en Azure, junto con todos los cambios de base de datos que ha implementado desde la primera vez que implementó.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-108">Finally, you redeploy the application to Azure, along with all of the database changes that you've implemented since the first time you deployed.</span></span>

<span data-ttu-id="3f3b0-109">En las ilustraciones siguientes se muestran algunas de las páginas con las que va a trabajar.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-109">The following illustrations show some of the pages that you'll work with.</span></span>

![Página departamentos](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Crear Departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

<span data-ttu-id="3f3b0-112">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-112">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3f3b0-113">Más información sobre el código asincrónico</span><span class="sxs-lookup"><span data-stu-id="3f3b0-113">Learn about asynchronous code</span></span>
> * <span data-ttu-id="3f3b0-114">Crear un controlador de Departamento</span><span class="sxs-lookup"><span data-stu-id="3f3b0-114">Create a Department controller</span></span>
> * <span data-ttu-id="3f3b0-115">Usar procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="3f3b0-115">Use stored procedures</span></span>
> * <span data-ttu-id="3f3b0-116">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="3f3b0-116">Deploy to Azure</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f3b0-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="3f3b0-117">Prerequisites</span></span>

* [<span data-ttu-id="3f3b0-118">Actualización de datos relacionados</span><span class="sxs-lookup"><span data-stu-id="3f3b0-118">Updating Related Data</span></span>](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="why-use-asynchronous-code"></a><span data-ttu-id="3f3b0-119">Por qué usar código asincrónico</span><span class="sxs-lookup"><span data-stu-id="3f3b0-119">Why use asynchronous code</span></span>

<span data-ttu-id="3f3b0-120">Un servidor web tiene un número limitado de subprocesos disponibles y, en situaciones de carga alta, es posible que todos los subprocesos disponibles estén en uso.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-120">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="3f3b0-121">Cuando esto ocurre, el servidor no puede procesar nuevas solicitudes hasta que los subprocesos se liberen.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-121">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="3f3b0-122">Con el código sincrónico, se pueden acumular muchos subprocesos mientras no estén realizando ningún trabajo porque están a la espera de que finalice la E/S.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-122">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="3f3b0-123">Con el código asincrónico, cuando un proceso está a la espera de que finalice la E/S, se libera su subproceso para el que el servidor lo use para el procesamiento de otras solicitudes.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-123">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="3f3b0-124">Como resultado, el código asincrónico permite el uso más eficaz de los recursos del servidor y el servidor está habilitado para administrar más tráfico sin retrasos.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-124">As a result, asynchronous code enables server resources to be use more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="3f3b0-125">En versiones anteriores de .NET, escribir y probar código asincrónico era complejo, propenso a errores y difícil de depurar.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-125">In earlier versions of .NET, writing and testing asynchronous code was complex, error prone, and hard to debug.</span></span> <span data-ttu-id="3f3b0-126">En .NET 4,5, escribir, probar y depurar código asincrónico es mucho más fácil de escribir en general, a menos que tenga un motivo que no sea.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-126">In .NET 4.5, writing, testing, and debugging asynchronous code is so much easier that you should generally write asynchronous code unless you have a reason not to.</span></span> <span data-ttu-id="3f3b0-127">El código asincrónico introduce una pequeña cantidad de sobrecarga, pero para situaciones de tráfico reducido, la disminución del rendimiento es insignificante, mientras que en situaciones de tráfico elevado, la posible mejora del rendimiento es considerable.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-127">Asynchronous code does introduce a small amount of overhead, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="3f3b0-128">Para obtener más información sobre la programación asincrónica, vea [uso de la compatibilidad asincrónica de .net 4.5 para evitar llamadas de bloqueo](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span><span class="sxs-lookup"><span data-stu-id="3f3b0-128">For more information about asynchronous programming, see [Use .NET 4.5's async support to avoid blocking calls](../../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices.md#async).</span></span>

## <a name="create-department-controller"></a><span data-ttu-id="3f3b0-129">Crear controlador de Departamento</span><span class="sxs-lookup"><span data-stu-id="3f3b0-129">Create Department controller</span></span>

<span data-ttu-id="3f3b0-130">Cree un controlador de Departamento de la misma manera que hizo con los controladores anteriores, pero esta vez Active la casilla **usar acciones de controlador asincrónico** .</span><span class="sxs-lookup"><span data-stu-id="3f3b0-130">Create a Department controller the same way you did the earlier controllers, except this time select the **Use async controller actions** check box.</span></span>

<span data-ttu-id="3f3b0-131">Los siguientes resaltados muestran lo que se agregó al código sincrónico para el método `Index` para que sea asincrónico:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-131">The following highlights show what was added to the synchronous code for the `Index` method to make it asynchronous:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=1,4)]

<span data-ttu-id="3f3b0-132">Se aplicaron cuatro cambios para permitir que la consulta de base de datos de Entity Framework se ejecute de forma asincrónica:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-132">Four changes were applied to enable the Entity Framework database query to execute asynchronously:</span></span>

- <span data-ttu-id="3f3b0-133">El método se marca con la palabra clave `async`, que indica al compilador que genere devoluciones de llamada para partes del cuerpo del método y que cree automáticamente el objeto `Task<ActionResult>` que se devuelve.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-133">The method is marked with the `async` keyword, which tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<ActionResult>` object that is returned.</span></span>
- <span data-ttu-id="3f3b0-134">El tipo de valor devuelto cambió de `ActionResult` a `Task<ActionResult>`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-134">The return type was changed from `ActionResult` to `Task<ActionResult>`.</span></span> <span data-ttu-id="3f3b0-135">El tipo de `Task<T>` representa el trabajo en curso con un resultado de tipo `T`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-135">The `Task<T>` type represents ongoing work with a result of type `T`.</span></span>
- <span data-ttu-id="3f3b0-136">La palabra clave `await` se aplicó a la llamada al servicio Web.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-136">The `await` keyword was applied to the web service call.</span></span> <span data-ttu-id="3f3b0-137">Cuando el compilador ve esta palabra clave, en segundo plano divide el método en dos partes.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-137">When the compiler sees this keyword, behind the scenes it splits the method into two parts.</span></span> <span data-ttu-id="3f3b0-138">La primera parte finaliza con la operación que se inicia de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-138">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="3f3b0-139">La segunda parte se coloca en un método de devolución de llamada al que se llama cuando se completa la operación.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-139">The second part is put into a callback method that is called when the operation completes.</span></span>
- <span data-ttu-id="3f3b0-140">Se llamó a la versión asincrónica del método de extensión `ToList`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-140">The asynchronous version of the `ToList` extension method was called.</span></span>

<span data-ttu-id="3f3b0-141">¿Por qué se ha modificado la instrucción `departments.ToList` pero no la instrucción `departments = db.Departments`?</span><span class="sxs-lookup"><span data-stu-id="3f3b0-141">Why is the `departments.ToList` statement modified but not the `departments = db.Departments` statement?</span></span> <span data-ttu-id="3f3b0-142">La razón es que solo se ejecutan de forma asincrónica las instrucciones que hacen que las consultas o los comandos se envíen a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-142">The reason is that only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="3f3b0-143">La instrucción `departments = db.Departments` configura una consulta, pero la consulta no se ejecuta hasta que se llama al método `ToList`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-143">The `departments = db.Departments` statement sets up a query but the query is not executed until the `ToList` method is called.</span></span> <span data-ttu-id="3f3b0-144">Por lo tanto, solo se ejecuta de forma asincrónica el método `ToList`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-144">Therefore, only the `ToList` method is executed asynchronously.</span></span>

<span data-ttu-id="3f3b0-145">En el método `Details` y los métodos `Edit` y `Delete` `HttpGet`, el método `Find` es el que hace que se envíe una consulta a la base de datos, por lo que es el método que se ejecuta de forma asincrónica:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-145">In the `Details` method and the `HttpGet` `Edit` and `Delete` methods, the `Find` method is the one that causes a query to be sent to the database, so that's the method that gets executed asynchronously:</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs?highlight=7)]

<span data-ttu-id="3f3b0-146">En los métodos `Create`, `HttpPost Edit`y `DeleteConfirmed`, es la llamada al método `SaveChanges` que hace que se ejecute un comando, no instrucciones como `db.Departments.Add(department)` que solo hacen que se modifiquen las entidades de la memoria.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-146">In the `Create`, `HttpPost Edit`, and `DeleteConfirmed` methods, it is the `SaveChanges` method call that causes a command to be executed, not statements such as `db.Departments.Add(department)` which only cause entities in memory to be modified.</span></span>

[!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs?highlight=6)]

<span data-ttu-id="3f3b0-147">Abra *Views\Department\Index.cshtml*y reemplace el código de plantilla por el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-147">Open *Views\Department\Index.cshtml*, and replace the template code with the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=3,5,20-22,36-38)]

<span data-ttu-id="3f3b0-148">Este código cambia el título de index a departments, mueve el nombre del administrador a la derecha y proporciona el nombre completo del administrador.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-148">This code changes the title from Index to Departments, moves the Administrator name to the right, and provides the full name of the administrator.</span></span>

<span data-ttu-id="3f3b0-149">En las vistas crear, eliminar, detalles y editar, cambie el título del campo `InstructorID` a "administrador" de la misma manera que cambió el campo Nombre del Departamento a "Departamento" en las vistas del curso.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-149">In the Create, Delete, Details, and Edit views, change the caption for the `InstructorID` field to "Administrator" the same way you changed the department name field to "Department" in the Course views.</span></span>

<span data-ttu-id="3f3b0-150">En las vistas crear y editar, use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-150">In the Create and Edit views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml)]

<span data-ttu-id="3f3b0-151">En las vistas eliminar y detalles, use el código siguiente:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-151">In the Delete and Details views use the following code:</span></span>

[!code-cshtml[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cshtml)]

<span data-ttu-id="3f3b0-152">Ejecute la aplicación y haga clic en la pestaña **departments** .</span><span class="sxs-lookup"><span data-stu-id="3f3b0-152">Run the application, and click the **Departments** tab.</span></span>

<span data-ttu-id="3f3b0-153">Todo funciona igual que en los demás controladores, pero en este controlador todas las consultas SQL se ejecutan de forma asincrónica.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-153">Everything works the same as in the other controllers, but in this controller all of the SQL queries are executing asynchronously.</span></span>

<span data-ttu-id="3f3b0-154">Algunos aspectos que se deben tener en cuenta cuando se usa la programación asincrónica con la Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-154">Some things to be aware of when you are using asynchronous programming with the Entity Framework:</span></span>

- <span data-ttu-id="3f3b0-155">El código asincrónico no es seguro para subprocesos.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-155">The async code is not thread safe.</span></span> <span data-ttu-id="3f3b0-156">En otras palabras, en otras palabras, no intente realizar varias operaciones en paralelo con la misma instancia de contexto.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-156">In other words, in other words, don't try to do multiple operations in parallel using the same context instance.</span></span>
- <span data-ttu-id="3f3b0-157">Si quiere aprovechar las ventajas de rendimiento del código asincrónico, asegúrese de que en los paquetes de biblioteca que use (por ejemplo para paginación), también se usa async si llaman a cualquier método de Entity Framework que haga que las consultas se envíen a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-157">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

## <a name="use-stored-procedures"></a><span data-ttu-id="3f3b0-158">Usar procedimientos almacenados</span><span class="sxs-lookup"><span data-stu-id="3f3b0-158">Use stored procedures</span></span>

<span data-ttu-id="3f3b0-159">Algunos desarrolladores y DBA prefieren usar procedimientos almacenados para el acceso a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-159">Some developers and DBAs prefer to use stored procedures for database access.</span></span> <span data-ttu-id="3f3b0-160">En versiones anteriores de Entity Framework se pueden recuperar datos mediante un procedimiento almacenado mediante [la ejecución de una consulta SQL sin formato](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), pero no se puede indicar a EF que use procedimientos almacenados para las operaciones de actualización.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-160">In earlier versions of Entity Framework you can retrieve data using a stored procedure by [executing a raw SQL query](advanced-entity-framework-scenarios-for-an-mvc-web-application.md), but you can't instruct EF to use stored procedures for update operations.</span></span> <span data-ttu-id="3f3b0-161">En EF 6 es fácil configurar Code First para usar procedimientos almacenados.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-161">In EF 6 it's easy to configure Code First to use stored procedures.</span></span>

1. <span data-ttu-id="3f3b0-162">En *DAL\SchoolContext.CS*, agregue el código resaltado al método `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-162">In *DAL\SchoolContext.cs*, add the highlighted code to the `OnModelCreating` method.</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=9)]

    <span data-ttu-id="3f3b0-163">Este código indica a Entity Framework que utilice procedimientos almacenados para las operaciones de inserción, actualización y eliminación en la entidad `Department`.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-163">This code instructs Entity Framework to use stored procedures for insert, update, and delete operations on the `Department` entity.</span></span>
2. <span data-ttu-id="3f3b0-164">En la consola de administración de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-164">In Package Manage Console, enter the following command:</span></span>

    `add-migration DepartmentSP`

    <span data-ttu-id="3f3b0-165">Abra *migraciones\\&lt;timestamp&gt;\_DepartmentSP.CS* para ver el código en el método `Up` que crea procedimientos almacenados de inserción, actualización y eliminación:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-165">Open *Migrations\\&lt;timestamp&gt;\_DepartmentSP.cs* to see the code in the `Up` method that creates Insert, Update, and Delete stored procedures:</span></span>

    [!code-csharp[Main](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs?highlight=3-4,26-27,42-43)]
3. <span data-ttu-id="3f3b0-166">En la consola de administración de paquetes, escriba el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-166">In Package Manage Console, enter the following command:</span></span>

     `update-database`
4. <span data-ttu-id="3f3b0-167">Ejecute la aplicación en modo de depuración, haga clic en la pestaña **departamentos** y, a continuación, haga clic en **crear nuevo**.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-167">Run the application in debug mode, click the **Departments** tab, and then click **Create New**.</span></span>
5. <span data-ttu-id="3f3b0-168">Escriba los datos de un nuevo departamento y, a continuación, haga clic en **crear**.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-168">Enter data for a new department, and then click **Create**.</span></span>

6. <span data-ttu-id="3f3b0-169">En Visual Studio, examine los registros de la ventana de **salida** para ver que se ha utilizado un procedimiento almacenado para insertar la nueva fila Department.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-169">In Visual Studio, look at the logs in the **Output** window to see that a stored procedure was used to insert the new Department row.</span></span>

     ![Proveedor de inserción de Departamento](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="3f3b0-171">Code First crea nombres de procedimientos almacenados predeterminados.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-171">Code First creates default stored procedure names.</span></span> <span data-ttu-id="3f3b0-172">Si está utilizando una base de datos existente, es posible que tenga que personalizar los nombres de los procedimientos almacenados para poder utilizar los procedimientos almacenados ya definidos en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-172">If you are using an existing database, you might need to customize the stored procedure names in order to use stored procedures already defined in the database.</span></span> <span data-ttu-id="3f3b0-173">Para obtener información sobre cómo hacerlo, vea [Entity Framework Code First procedimientos almacenados INSERT, Update y DELETE](https://msdn.microsoft.com/data/dn468673).</span><span class="sxs-lookup"><span data-stu-id="3f3b0-173">For information about how to do that, see [Entity Framework Code First Insert/Update/Delete Stored Procedures](https://msdn.microsoft.com/data/dn468673).</span></span>

<span data-ttu-id="3f3b0-174">Si desea personalizar los procedimientos almacenados generados, puede editar el código con scaffolding para el método Migrations `Up` que crea el procedimiento almacenado.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-174">If you want to customize what generated stored procedures do, you can edit the scaffolded code for the migrations `Up` method that creates the stored procedure.</span></span> <span data-ttu-id="3f3b0-175">De este modo, los cambios se reflejarán siempre que se ejecute la migración y se apliquen a la base de datos de producción cuando las migraciones se ejecuten automáticamente en producción después de la implementación.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-175">That way your changes are reflected whenever that migration is run and will be applied to your production database when migrations runs automatically in production after deployment.</span></span>

<span data-ttu-id="3f3b0-176">Si desea cambiar un procedimiento almacenado existente creado en una migración anterior, puede usar el comando Add-Migration para generar una migración en blanco y, a continuación, escribir manualmente el código que llama al método [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) .</span><span class="sxs-lookup"><span data-stu-id="3f3b0-176">If you want to change an existing stored procedure that was created in a previous migration, you can use the Add-Migration command to generate a blank migration, and then manually write code that calls the [AlterStoredProcedure](https://msdn.microsoft.com/library/system.data.entity.migrations.dbmigration.alterstoredprocedure.aspx) method.</span></span>

## <a name="deploy-to-azure"></a><span data-ttu-id="3f3b0-177">Implementar en Azure</span><span class="sxs-lookup"><span data-stu-id="3f3b0-177">Deploy to Azure</span></span>

<span data-ttu-id="3f3b0-178">En esta sección, es necesario haber completado la sección implementación opcional de **la aplicación en Azure** en el tutorial sobre [migraciones e implementación](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) de esta serie.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-178">This section requires you to have completed the optional **Deploying the app to Azure** section in the [Migrations and Deployment](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application.md) tutorial of this series.</span></span> <span data-ttu-id="3f3b0-179">Si tuviera errores de migración que resolvió eliminando la base de datos en el proyecto local, omita esta sección.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-179">If you had migrations errors that you resolved by deleting the database in your local project, skip this section.</span></span>

1. <span data-ttu-id="3f3b0-180">En Visual Studio, haga clic con el botón derecho en el proyecto, en el **Explorador de soluciones** y seleccione **Publicar** en el menú contextual.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-180">In Visual Studio, right-click the project in **Solution Explorer** and select **Publish** from the context menu.</span></span>
2. <span data-ttu-id="3f3b0-181">Haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-181">Click **Publish**.</span></span>

    <span data-ttu-id="3f3b0-182">Visual Studio implementa la aplicación en Azure y la aplicación se abre en el explorador predeterminado, que se ejecuta en Azure.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-182">Visual Studio deploys the application to Azure, and the application opens in your default browser, running in Azure.</span></span>
3. <span data-ttu-id="3f3b0-183">Pruebe la aplicación para comprobar que funciona.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-183">Test the application to verify it's working.</span></span>

    <span data-ttu-id="3f3b0-184">La primera vez que se ejecuta una página que tiene acceso a la base de datos, el Entity Framework ejecuta todas las migraciones `Up` métodos necesarios para actualizar la base de datos con el modelo de datos actual.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-184">The first time you run a page that accesses the database, the Entity Framework runs all of the migrations `Up` methods required to bring the database up to date with the current data model.</span></span> <span data-ttu-id="3f3b0-185">Ahora puede usar todas las páginas web que agregó desde la última vez que implementó, incluidas las páginas de departamento que agregó en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-185">You can now use all of the web pages that you added since the last time you deployed, including the Department pages that you added in this tutorial.</span></span>

## <a name="get-the-code"></a><span data-ttu-id="3f3b0-186">Obtención del código</span><span class="sxs-lookup"><span data-stu-id="3f3b0-186">Get the code</span></span>

[<span data-ttu-id="3f3b0-187">Descargar el proyecto completado</span><span class="sxs-lookup"><span data-stu-id="3f3b0-187">Download the Completed Project</span></span>](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a><span data-ttu-id="3f3b0-188">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="3f3b0-188">Additional resources</span></span>

<span data-ttu-id="3f3b0-189">Los vínculos a otros recursos de Entity Framework pueden encontrarse en el [acceso a datos ASP.net: recursos recomendados](../../../../whitepapers/aspnet-data-access-content-map.md).</span><span class="sxs-lookup"><span data-stu-id="3f3b0-189">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3f3b0-190">Pasos siguientes</span><span class="sxs-lookup"><span data-stu-id="3f3b0-190">Next steps</span></span>

<span data-ttu-id="3f3b0-191">En este tutorial va a:</span><span class="sxs-lookup"><span data-stu-id="3f3b0-191">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="3f3b0-192">Información sobre el código asincrónico</span><span class="sxs-lookup"><span data-stu-id="3f3b0-192">Learned about asynchronous code</span></span>
> * <span data-ttu-id="3f3b0-193">Creación de un controlador de Departamento</span><span class="sxs-lookup"><span data-stu-id="3f3b0-193">Created a Department controller</span></span>
> * <span data-ttu-id="3f3b0-194">Procedimientos almacenados usados</span><span class="sxs-lookup"><span data-stu-id="3f3b0-194">Used stored procedures</span></span>
> * <span data-ttu-id="3f3b0-195">Implementado en Azure</span><span class="sxs-lookup"><span data-stu-id="3f3b0-195">Deployed to Azure</span></span>

<span data-ttu-id="3f3b0-196">Avance al siguiente artículo para obtener información sobre cómo controlar los conflictos cuando varios usuarios actualizan la misma entidad al mismo tiempo.</span><span class="sxs-lookup"><span data-stu-id="3f3b0-196">Advance to the next article to learn how to handle conflicts when multiple users update the same entity at the same time.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="3f3b0-197">Controlar la simultaneidad</span><span class="sxs-lookup"><span data-stu-id="3f3b0-197">Handling concurrency</span></span>](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application.md)
