---
uid: web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
title: Agregar capa de lógica de negocios a un proyecto que utiliza el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 7ef664b3-1cc8-4cbf-bb18-9f0f3a3ada2b
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/adding-business-logic-layer
msc.type: authoredcontent
ms.openlocfilehash: a824d06d3781e11706f2a48d44ea3ad89bdb7c8b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78515431"
---
# <a name="adding-business-logic-layer-to-a-project-that-uses-model-binding-and-web-forms"></a><span data-ttu-id="efe48-104">Agregar capa de lógica de negocios a un proyecto que usa el enlace de modelos y formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="efe48-104">Adding business logic layer to a project that uses model binding and web forms</span></span>

<span data-ttu-id="efe48-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="efe48-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="efe48-106">En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="efe48-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="efe48-107">El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="efe48-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="efe48-108">Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="efe48-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="efe48-109">En este tutorial se muestra cómo usar el enlace de modelos con una capa de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="efe48-109">This tutorial shows how to use model binding with a business logic layer.</span></span> <span data-ttu-id="efe48-110">Establecerá el miembro OnCallingDataMethods para especificar que un objeto distinto de la página actual se usa para llamar a los métodos de datos.</span><span class="sxs-lookup"><span data-stu-id="efe48-110">You will set the OnCallingDataMethods member to specify that an object other than the current page is used to call the data methods.</span></span>
> 
> <span data-ttu-id="efe48-111">Este tutorial se basa en el proyecto creado en las partes [anteriores](retrieving-data.md) de la serie.</span><span class="sxs-lookup"><span data-stu-id="efe48-111">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="efe48-112">Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="efe48-112">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="efe48-113">El código descargable funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="efe48-113">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="efe48-114">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="efe48-114">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="efe48-115">Lo que va a compilar</span><span class="sxs-lookup"><span data-stu-id="efe48-115">What you'll build</span></span>

<span data-ttu-id="efe48-116">El enlace de modelos permite colocar el código de interacción de datos en el archivo de código subyacente de una página web o en una clase de lógica de negocios independiente.</span><span class="sxs-lookup"><span data-stu-id="efe48-116">Model binding enables you to put your data interaction code in either the code-behind file for a web page or in a separate business logic class.</span></span> <span data-ttu-id="efe48-117">En los tutoriales anteriores se ha mostrado cómo usar los archivos de código subyacente para el código de interacción de datos.</span><span class="sxs-lookup"><span data-stu-id="efe48-117">The previous tutorials have shown how to use the code-behind files for data interaction code.</span></span> <span data-ttu-id="efe48-118">Este enfoque funciona para sitios pequeños, pero puede dar lugar a repeticiones de código y mayor dificultad al mantener un sitio de gran tamaño.</span><span class="sxs-lookup"><span data-stu-id="efe48-118">This approach works for small sites but it can lead to code repetition and greater difficulty when maintaining a large site.</span></span> <span data-ttu-id="efe48-119">También puede ser muy difícil probar mediante programación el código que reside en los archivos de código subyacente, ya que no hay ninguna capa de abstracción.</span><span class="sxs-lookup"><span data-stu-id="efe48-119">It can also be very difficult to programmatically test code that resides in code behind files because there is no abstraction layer.</span></span>

<span data-ttu-id="efe48-120">Para centralizar el código de interacción de datos, puede crear una capa de lógica de negocios que contenga toda la lógica para interactuar con los datos.</span><span class="sxs-lookup"><span data-stu-id="efe48-120">To centralize the data interaction code, you can create a business logic layer that contains all of the logic for interacting with data.</span></span> <span data-ttu-id="efe48-121">A continuación, llame a la capa de lógica de negocios desde las páginas Web.</span><span class="sxs-lookup"><span data-stu-id="efe48-121">You then call the business logic layer from your web pages.</span></span> <span data-ttu-id="efe48-122">En este tutorial se muestra cómo trasladar todo el código que ha escrito en los tutoriales anteriores a una capa de lógica de negocios y, a continuación, usar ese código desde las páginas.</span><span class="sxs-lookup"><span data-stu-id="efe48-122">This tutorial shows how to move all of the code you have written in the previous tutorials into a business logic layer, and then use that code from the pages.</span></span>

<span data-ttu-id="efe48-123">En este tutorial, hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="efe48-123">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="efe48-124">Trasladar el código de los archivos de código subyacente a una capa de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="efe48-124">Move the code from code-behind files to a business logic layer</span></span>
2. <span data-ttu-id="efe48-125">Cambiar los controles enlazados a datos para llamar a los métodos en el nivel de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="efe48-125">Change your data bound controls to call the methods in the business logic layer</span></span>

## <a name="create-business-logic-layer"></a><span data-ttu-id="efe48-126">Crear capa de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="efe48-126">Create business logic layer</span></span>

<span data-ttu-id="efe48-127">Ahora, creará la clase a la que se llama desde las páginas Web.</span><span class="sxs-lookup"><span data-stu-id="efe48-127">Now, you will create the class that is called from the web pages.</span></span> <span data-ttu-id="efe48-128">Los métodos de esta clase tienen un aspecto similar al de los métodos usados en los tutoriales anteriores e incluyen los atributos de proveedor de valores.</span><span class="sxs-lookup"><span data-stu-id="efe48-128">The methods in this class look similar to the methods you used in the previous tutorials and include the value provider attributes.</span></span>

<span data-ttu-id="efe48-129">En primer lugar, agregue una nueva carpeta denominada **BLL**.</span><span class="sxs-lookup"><span data-stu-id="efe48-129">First, add a new folder called **BLL**.</span></span>

![Agregar carpeta](adding-business-logic-layer/_static/image1.png)

<span data-ttu-id="efe48-131">En la carpeta BLL, cree una nueva clase denominada **SchoolBL.CS**.</span><span class="sxs-lookup"><span data-stu-id="efe48-131">In the BLL folder, create a new class named **SchoolBL.cs**.</span></span> <span data-ttu-id="efe48-132">Contendrá todas las operaciones de datos que residían originalmente en los archivos de código subyacente.</span><span class="sxs-lookup"><span data-stu-id="efe48-132">It will contain all of the data operations that originally resided in code-behind files.</span></span> <span data-ttu-id="efe48-133">Los métodos son prácticamente los mismos que los métodos del archivo de código subyacente, pero incluirán algunos cambios.</span><span class="sxs-lookup"><span data-stu-id="efe48-133">The methods are almost the same as the methods in the code-behind file, but will include some changes.</span></span>

<span data-ttu-id="efe48-134">El cambio más importante que hay que tener en cuenta es que ya no se está ejecutando el código desde una instancia de la clase **Page** .</span><span class="sxs-lookup"><span data-stu-id="efe48-134">The most important change to note is that you are no longer executing the code from within an instance of **Page** class.</span></span> <span data-ttu-id="efe48-135">La clase Page contiene el método **TryUpdateModel** y la propiedad **ModelState** .</span><span class="sxs-lookup"><span data-stu-id="efe48-135">The Page class contains the **TryUpdateModel** method and the **ModelState** property.</span></span> <span data-ttu-id="efe48-136">Cuando este código se mueve a una capa de lógica de negocios, ya no tiene una instancia de la clase Page para llamar a estos miembros.</span><span class="sxs-lookup"><span data-stu-id="efe48-136">When this code is moved to a business logic layer, you no longer have an instance of the Page class to call these members.</span></span> <span data-ttu-id="efe48-137">Para solucionar este problema, debe agregar un parámetro **ModelMethodContext** a cualquier método que tenga acceso a TryUpdateModel o ModelState.</span><span class="sxs-lookup"><span data-stu-id="efe48-137">To get around this issue, you must add a **ModelMethodContext** parameter to any method that accesses TryUpdateModel or ModelState.</span></span> <span data-ttu-id="efe48-138">Este parámetro ModelMethodContext se usa para llamar a TryUpdateModel o recuperar ModelState.</span><span class="sxs-lookup"><span data-stu-id="efe48-138">You use this ModelMethodContext parameter to call TryUpdateModel or retrieve ModelState.</span></span> <span data-ttu-id="efe48-139">No es necesario cambiar nada en la página web para tener en cuenta este nuevo parámetro.</span><span class="sxs-lookup"><span data-stu-id="efe48-139">You do not need to change anything in the web page to account for this new parameter.</span></span>

<span data-ttu-id="efe48-140">Reemplace el código de SchoolBL.cs por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="efe48-140">Replace the code in SchoolBL.cs with the following code.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample1.cs)]

## <a name="revise-existing-pages-to-retrieve-data-from-business-logic-layer"></a><span data-ttu-id="efe48-141">Revisar las páginas existentes para recuperar datos de la capa de lógica de negocios</span><span class="sxs-lookup"><span data-stu-id="efe48-141">Revise existing pages to retrieve data from business logic layer</span></span>

<span data-ttu-id="efe48-142">Por último, convertirá las páginas Students. aspx, AddStudent. aspx y Courses. aspx del uso de las consultas del archivo de código subyacente en el uso del nivel de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="efe48-142">Finally, you will convert the pages Students.aspx, AddStudent.aspx, and Courses.aspx from using queries in the code-behind file to using the business logic layer.</span></span>

<span data-ttu-id="efe48-143">En los archivos de código subyacente para estudiantes, AddStudent y cursos, elimine o comente los siguientes métodos de consulta:</span><span class="sxs-lookup"><span data-stu-id="efe48-143">In the code-behind files for Students, AddStudent, and Courses, delete or comment out the following query methods:</span></span>

- <span data-ttu-id="efe48-144">studentsGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="efe48-144">studentsGrid\_GetData</span></span>
- <span data-ttu-id="efe48-145">studentsGrid\_UpdateItem</span><span class="sxs-lookup"><span data-stu-id="efe48-145">studentsGrid\_UpdateItem</span></span>
- <span data-ttu-id="efe48-146">studentsGrid\_DeleteItem</span><span class="sxs-lookup"><span data-stu-id="efe48-146">studentsGrid\_DeleteItem</span></span>
- <span data-ttu-id="efe48-147">addStudentForm\_InsertItem</span><span class="sxs-lookup"><span data-stu-id="efe48-147">addStudentForm\_InsertItem</span></span>
- <span data-ttu-id="efe48-148">coursesGrid\_GetData</span><span class="sxs-lookup"><span data-stu-id="efe48-148">coursesGrid\_GetData</span></span>

<span data-ttu-id="efe48-149">Ahora no debe tener ningún código en el archivo de código subyacente que pertenezca a las operaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="efe48-149">You now should have no code in the code-behind file that pertains to data operations.</span></span>

<span data-ttu-id="efe48-150">El controlador de eventos **OnCallingDataMethods** le permite especificar un objeto que se usará para los métodos de datos.</span><span class="sxs-lookup"><span data-stu-id="efe48-150">The **OnCallingDataMethods** event handler enables you to specify an object to use for the data methods.</span></span> <span data-ttu-id="efe48-151">En Students. aspx, agregue un valor para ese controlador de eventos y cambie los nombres de los métodos de datos a los nombres de los métodos de la clase de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="efe48-151">In Students.aspx, add a value for that event handler and change the names of the data methods to the names of the methods in the business logic class.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample2.aspx?highlight=3-4,8)]

<span data-ttu-id="efe48-152">En el archivo de código subyacente para Students. aspx, defina el controlador de eventos para el evento CallingDataMethods.</span><span class="sxs-lookup"><span data-stu-id="efe48-152">In the code-behind file for Students.aspx, define the event handler for the CallingDataMethods event.</span></span> <span data-ttu-id="efe48-153">En este controlador de eventos, se especifica la clase de lógica de negocios para las operaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="efe48-153">In this event handler, you specify the business logic class for data operations.</span></span>

[!code-csharp[Main](adding-business-logic-layer/samples/sample3.cs)]

<span data-ttu-id="efe48-154">En AddStudent. aspx, realice cambios similares.</span><span class="sxs-lookup"><span data-stu-id="efe48-154">In AddStudent.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample4.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample5.cs)]

<span data-ttu-id="efe48-155">En Courses. aspx, realice cambios similares.</span><span class="sxs-lookup"><span data-stu-id="efe48-155">In Courses.aspx, make similar changes.</span></span>

[!code-aspx[Main](adding-business-logic-layer/samples/sample6.aspx?highlight=3-4)]

[!code-csharp[Main](adding-business-logic-layer/samples/sample7.cs)]

<span data-ttu-id="efe48-156">Ejecute la aplicación y observe que todas las páginas funcionan como tenían anteriormente.</span><span class="sxs-lookup"><span data-stu-id="efe48-156">Run the application and notice that all of the pages function as they had previously.</span></span> <span data-ttu-id="efe48-157">La lógica de validación también funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="efe48-157">The validation logic also works correctly.</span></span>

## <a name="conclusion"></a><span data-ttu-id="efe48-158">Conclusión</span><span class="sxs-lookup"><span data-stu-id="efe48-158">Conclusion</span></span>

<span data-ttu-id="efe48-159">En este tutorial, reestructure la aplicación para usar una capa de acceso a datos y una capa de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="efe48-159">In this tutorial, you re-structured your application to use a data access layer and business logic layer.</span></span> <span data-ttu-id="efe48-160">Especificó que los controles de datos usan un objeto que no es la página actual para las operaciones de datos.</span><span class="sxs-lookup"><span data-stu-id="efe48-160">You specified that the data controls use an object that is not the current page for data operations.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="efe48-161">Anterior</span><span class="sxs-lookup"><span data-stu-id="efe48-161">Previous</span></span>](using-query-string-values-to-retrieve-data.md)
