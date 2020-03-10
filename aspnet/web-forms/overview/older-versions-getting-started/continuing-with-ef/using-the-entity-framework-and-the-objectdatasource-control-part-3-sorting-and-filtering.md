---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
title: 'Usar el Entity Framework 4,0 y el control ObjectDataSource, parte 3: ordenación y filtrado | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales se basa en la aplicación web contoso University que crea el Introducción con la serie de tutoriales Entity Framework 4,0. I...
ms.author: riande
ms.date: 01/26/2011
ms.assetid: 2990bd10-590d-43d5-9529-6b503ce5455d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering
msc.type: authoredcontent
ms.openlocfilehash: 603120864528b9a5ff81214270eb9a7f1b68b347
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78512719"
---
# <a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-3-sorting-and-filtering"></a><span data-ttu-id="16466-104">Usar el Entity Framework 4,0 y el control ObjectDataSource, parte 3: ordenación y filtrado</span><span class="sxs-lookup"><span data-stu-id="16466-104">Using the Entity Framework 4.0 and the ObjectDataSource Control, Part 3: Sorting and Filtering</span></span>

<span data-ttu-id="16466-105">por [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="16466-105">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="16466-106">Esta serie de tutoriales se basa en la aplicación web contoso University que crea el [Introducción con la](https://asp.net/entity-framework/tutorials#Getting%20Started) serie de tutoriales Entity Framework 4,0.</span><span class="sxs-lookup"><span data-stu-id="16466-106">This tutorial series builds on the Contoso University web application that is created by the [Getting Started with the Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started) tutorial series.</span></span> <span data-ttu-id="16466-107">Si no completó los tutoriales anteriores, como punto de partida para este tutorial, puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) que habría creado.</span><span class="sxs-lookup"><span data-stu-id="16466-107">If you didn't complete the earlier tutorials, as a starting point for this tutorial you can [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) that you would have created.</span></span> <span data-ttu-id="16466-108">También puede [descargar la aplicación](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) que se crea en la serie completa de tutoriales.</span><span class="sxs-lookup"><span data-stu-id="16466-108">You can also [download the application](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) that is created by the complete tutorial series.</span></span> <span data-ttu-id="16466-109">Si tiene alguna pregunta sobre los tutoriales, puede publicarlos en el [Foro de Entity Framework de ASP.net](https://forums.asp.net/1227.aspx).</span><span class="sxs-lookup"><span data-stu-id="16466-109">If you have questions about the tutorials, you can post them to the [ASP.NET Entity Framework forum](https://forums.asp.net/1227.aspx).</span></span>

<span data-ttu-id="16466-110">En el tutorial anterior, implementó el patrón del repositorio en una aplicación Web de n niveles que utiliza el Entity Framework y el control `ObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="16466-110">In the previous tutorial you implemented the repository pattern in an n-tier web application that uses the Entity Framework and the `ObjectDataSource` control.</span></span> <span data-ttu-id="16466-111">En este tutorial se muestra cómo realizar tareas de ordenación y filtrado y cómo controlar los escenarios de detalles principales.</span><span class="sxs-lookup"><span data-stu-id="16466-111">This tutorial shows how to do sorting and filtering and handle master-detail scenarios.</span></span> <span data-ttu-id="16466-112">Agregará las siguientes mejoras a la página *departments. aspx* :</span><span class="sxs-lookup"><span data-stu-id="16466-112">You'll add the following enhancements to the *Departments.aspx* page:</span></span>

- <span data-ttu-id="16466-113">Un cuadro de texto para permitir que los usuarios seleccionen departamentos por nombre.</span><span class="sxs-lookup"><span data-stu-id="16466-113">A text box to allow users to select departments by name.</span></span>
- <span data-ttu-id="16466-114">Una lista de cursos para cada departamento que se muestra en la cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="16466-114">A list of courses for each department that's shown in the grid.</span></span>
- <span data-ttu-id="16466-115">La capacidad de ordenar haciendo clic en los encabezados de columna.</span><span class="sxs-lookup"><span data-stu-id="16466-115">The ability to sort by clicking column headings.</span></span>

<span data-ttu-id="16466-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="16466-116">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image1.png)</span></span>

## <a name="adding-the-ability-to-sort-gridview-columns"></a><span data-ttu-id="16466-117">Agregar la capacidad de ordenar las columnas de GridView</span><span class="sxs-lookup"><span data-stu-id="16466-117">Adding the Ability to Sort GridView Columns</span></span>

<span data-ttu-id="16466-118">Abra la página *departments. aspx* y agregue un atributo `SortParameterName="sortExpression"` al control `ObjectDataSource` denominado `DepartmentsObjectDataSource`.</span><span class="sxs-lookup"><span data-stu-id="16466-118">Open the *Departments.aspx* page and add a `SortParameterName="sortExpression"` attribute to the `ObjectDataSource` control named `DepartmentsObjectDataSource`.</span></span> <span data-ttu-id="16466-119">(Más adelante creará un método `GetDepartments` que toma un parámetro denominado `sortExpression`). El marcado para la etiqueta de apertura del control ahora es similar al ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="16466-119">(Later you'll create a `GetDepartments` method that takes a parameter named `sortExpression`.) The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample1.aspx)]

<span data-ttu-id="16466-120">Agregue el atributo `AllowSorting="true"` a la etiqueta de apertura del control `GridView`.</span><span class="sxs-lookup"><span data-stu-id="16466-120">Add the `AllowSorting="true"` attribute to the opening tag of the `GridView` control.</span></span> <span data-ttu-id="16466-121">El marcado para la etiqueta de apertura del control ahora es similar al ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="16466-121">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample2.aspx)]

<span data-ttu-id="16466-122">En *departments.aspx.CS*, establezca el criterio de ordenación predeterminado llamando al método `Sort` del control `GridView` desde el método `Page_Load`:</span><span class="sxs-lookup"><span data-stu-id="16466-122">In *Departments.aspx.cs*, set the default sort order by calling the `GridView` control's `Sort` method from the `Page_Load` method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample3.cs)]

<span data-ttu-id="16466-123">Puede agregar código que ordena o filtra en la clase de lógica de negocios o en la clase de repositorio.</span><span class="sxs-lookup"><span data-stu-id="16466-123">You can add code that sorts or filters in either the business logic class or the repository class.</span></span> <span data-ttu-id="16466-124">Si lo hace en la clase de lógica de negocios, el trabajo de ordenación o filtrado se realizará después de que se recuperen los datos de la base de datos, porque la clase de lógica de negocios está trabajando con un `IEnumerable` objeto devuelto por el repositorio.</span><span class="sxs-lookup"><span data-stu-id="16466-124">If you do it in the business logic class, the sorting or filtering work will be done after the data is retrieved from the database, because the business logic class is working with an `IEnumerable` object returned by the repository.</span></span> <span data-ttu-id="16466-125">Si agrega código de ordenación y filtrado en la clase de repositorio y lo hace antes de que una expresión LINQ o una consulta de objeto se haya convertido en un objeto `IEnumerable`, los comandos se pasarán a la base de datos para su procesamiento, que suele ser más eficaz.</span><span class="sxs-lookup"><span data-stu-id="16466-125">If you add sorting and filtering code in the repository class and you do it before a LINQ expression or object query has been converted to an `IEnumerable` object, your commands will be passed through to the database for processing, which is typically more efficient.</span></span> <span data-ttu-id="16466-126">En este tutorial, va a implementar la ordenación y el filtrado de una manera que hace que la base de datos realice el procesamiento, es decir, en el repositorio.</span><span class="sxs-lookup"><span data-stu-id="16466-126">In this tutorial you'll implement sorting and filtering in a way that causes the processing to be done by the database — that is, in the repository.</span></span>

<span data-ttu-id="16466-127">Para agregar capacidad de ordenación, debe agregar un nuevo método a la interfaz del repositorio y a las clases de repositorio, así como a la clase de lógica de negocios.</span><span class="sxs-lookup"><span data-stu-id="16466-127">To add sorting capability, you must add a new method to the repository interface and repository classes as well as to the business logic class.</span></span> <span data-ttu-id="16466-128">En el archivo *ISchoolRepository.CS* , agregue un nuevo método de `GetDepartments` que toma un parámetro `sortExpression` que se utilizará para ordenar la lista de departamentos devueltos:</span><span class="sxs-lookup"><span data-stu-id="16466-128">In the *ISchoolRepository.cs* file, add a new `GetDepartments` method that takes a `sortExpression` parameter that will be used to sort the list of departments that's returned:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample4.cs)]

<span data-ttu-id="16466-129">El parámetro `sortExpression` especificará la columna por la que se va a ordenar y la dirección de ordenación.</span><span class="sxs-lookup"><span data-stu-id="16466-129">The `sortExpression` parameter will specify the column to sort on and the sort direction.</span></span>

<span data-ttu-id="16466-130">Agregue el código para el nuevo método al archivo *SchoolRepository.CS* :</span><span class="sxs-lookup"><span data-stu-id="16466-130">Add code for the new method to the *SchoolRepository.cs* file:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample5.cs)]

<span data-ttu-id="16466-131">Cambie el método de `GetDepartments` sin parámetros existente para llamar al nuevo método:</span><span class="sxs-lookup"><span data-stu-id="16466-131">Change the existing parameterless `GetDepartments` method to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample6.cs)]

<span data-ttu-id="16466-132">En el proyecto de prueba, agregue el siguiente método nuevo a *MockSchoolRepository.CS*:</span><span class="sxs-lookup"><span data-stu-id="16466-132">In the test project, add the following new method to *MockSchoolRepository.cs*:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample7.cs)]

<span data-ttu-id="16466-133">Si va a crear cualquier prueba unitaria que dependa de este método para devolver una lista ordenada, deberá ordenar la lista antes de devolverla.</span><span class="sxs-lookup"><span data-stu-id="16466-133">If you were going to create any unit tests that depended on this method returning a sorted list, you would need to sort the list before returning it.</span></span> <span data-ttu-id="16466-134">No creará pruebas como esta en este tutorial, por lo que el método solo puede devolver la lista sin ordenar de departamentos.</span><span class="sxs-lookup"><span data-stu-id="16466-134">You won't be creating tests like that in this tutorial, so the method can just return the unsorted list of departments.</span></span>

<span data-ttu-id="16466-135">En el archivo *SchoolBL.CS* , agregue el siguiente método nuevo a la clase de lógica de negocios:</span><span class="sxs-lookup"><span data-stu-id="16466-135">In the *SchoolBL.cs* file, add the following new method to the business logic class:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample8.cs)]

<span data-ttu-id="16466-136">Este código pasa el parámetro de ordenación al método de repositorio.</span><span class="sxs-lookup"><span data-stu-id="16466-136">This code passes the sort parameter to the repository method.</span></span>

<span data-ttu-id="16466-137">Ejecute la página *departments. aspx* .</span><span class="sxs-lookup"><span data-stu-id="16466-137">Run the *Departments.aspx* page.</span></span>

<span data-ttu-id="16466-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="16466-138">[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image3.png)</span></span>

<span data-ttu-id="16466-139">Ahora puede hacer clic en cualquier encabezado de columna para ordenar por esa columna.</span><span class="sxs-lookup"><span data-stu-id="16466-139">You can now click any column heading to sort by that column.</span></span> <span data-ttu-id="16466-140">Si la columna ya está ordenada, al hacer clic en el encabezado se invierte la dirección de ordenación.</span><span class="sxs-lookup"><span data-stu-id="16466-140">If the column is already sorted, clicking the heading reverses the sort direction.</span></span>

## <a name="adding-a-search-box"></a><span data-ttu-id="16466-141">Agregar un cuadro de búsqueda</span><span class="sxs-lookup"><span data-stu-id="16466-141">Adding a Search Box</span></span>

<span data-ttu-id="16466-142">En esta sección, agregará un cuadro de texto de búsqueda, lo vinculará al control `ObjectDataSource` mediante un parámetro de control y agregará un método a la clase de lógica de negocios para admitir el filtrado.</span><span class="sxs-lookup"><span data-stu-id="16466-142">In this section you'll add a search text box, link it to the `ObjectDataSource` control using a control parameter, and add a method to the business logic class to support filtering.</span></span>

<span data-ttu-id="16466-143">Abra la página *departments. aspx* y agregue el siguiente marcado entre el encabezado y el primer control `ObjectDataSource`:</span><span class="sxs-lookup"><span data-stu-id="16466-143">Open the *Departments.aspx* page and add the following markup between the heading and the first `ObjectDataSource` control:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample9.aspx)]

<span data-ttu-id="16466-144">En el control `ObjectDataSource` denominado `DepartmentsObjectDataSource`, haga lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="16466-144">In the `ObjectDataSource` control named `DepartmentsObjectDataSource`, do the following:</span></span>

- <span data-ttu-id="16466-145">Agregue un elemento `SelectParameters` para un parámetro denominado `nameSearchString` que obtiene el valor especificado en el control `SearchTextBox`.</span><span class="sxs-lookup"><span data-stu-id="16466-145">Add a `SelectParameters` element for a parameter named `nameSearchString` that gets the value entered in the `SearchTextBox` control.</span></span>
- <span data-ttu-id="16466-146">Cambie el valor del atributo `SelectMethod` a `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="16466-146">Change the `SelectMethod` attribute value to `GetDepartmentsByName`.</span></span> <span data-ttu-id="16466-147">(Creará este método más adelante).</span><span class="sxs-lookup"><span data-stu-id="16466-147">(You'll create this method later.)</span></span>

<span data-ttu-id="16466-148">El marcado del control `ObjectDataSource` ahora es similar al ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="16466-148">The markup for the `ObjectDataSource` control now resembles the following example:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample10.aspx)]

<span data-ttu-id="16466-149">En *ISchoolRepository.CS*, agregue un método `GetDepartmentsByName` que toma los parámetros `sortExpression` y `nameSearchString`:</span><span class="sxs-lookup"><span data-stu-id="16466-149">In *ISchoolRepository.cs*, add a `GetDepartmentsByName` method that takes both `sortExpression` and `nameSearchString` parameters:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample11.cs)]

<span data-ttu-id="16466-150">En *SchoolRepository.CS*, agregue el siguiente método nuevo:</span><span class="sxs-lookup"><span data-stu-id="16466-150">In *SchoolRepository.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample12.cs)]

<span data-ttu-id="16466-151">Este código usa un método `Where` para seleccionar los elementos que contienen la cadena de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="16466-151">This code uses a `Where` method to select items that contain the search string.</span></span> <span data-ttu-id="16466-152">Si la cadena de búsqueda está vacía, se seleccionarán todos los registros.</span><span class="sxs-lookup"><span data-stu-id="16466-152">If the search string is empty, all records will be selected.</span></span> <span data-ttu-id="16466-153">Tenga en cuenta que cuando se especifican llamadas a métodos juntas en una instrucción como esta (`Include`, a continuación, se `OrderBy``Where`), el método de `Where` siempre debe ser el último.</span><span class="sxs-lookup"><span data-stu-id="16466-153">Note that when you specify method calls together in one statement like this (`Include`, then `OrderBy`, then `Where`), the `Where` method must always be last.</span></span>

<span data-ttu-id="16466-154">Cambie el método de `GetDepartments` existente que toma un parámetro `sortExpression` para llamar al nuevo método:</span><span class="sxs-lookup"><span data-stu-id="16466-154">Change the existing `GetDepartments` method that takes a `sortExpression` parameter to call the new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample13.cs)]

<span data-ttu-id="16466-155">En *MockSchoolRepository.CS* en el proyecto de prueba, agregue el siguiente método nuevo:</span><span class="sxs-lookup"><span data-stu-id="16466-155">In *MockSchoolRepository.cs* in the test project, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample14.cs)]

<span data-ttu-id="16466-156">En *SchoolBL.CS*, agregue el siguiente método nuevo:</span><span class="sxs-lookup"><span data-stu-id="16466-156">In *SchoolBL.cs*, add the following new method:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample15.cs)]

<span data-ttu-id="16466-157">Ejecute la página *departments. aspx* y escriba una cadena de búsqueda para asegurarse de que la lógica de selección funciona.</span><span class="sxs-lookup"><span data-stu-id="16466-157">Run the *Departments.aspx* page and enter a search string to make sure that the selection logic works.</span></span> <span data-ttu-id="16466-158">Deje vacío el cuadro de texto e intente realizar una búsqueda para asegurarse de que se devuelven todos los registros.</span><span class="sxs-lookup"><span data-stu-id="16466-158">Leave the text box empty and try a search to make sure that all records are returned.</span></span>

<span data-ttu-id="16466-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="16466-159">[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image5.png)</span></span>

## <a name="adding-a-details-column-for-each-grid-row"></a><span data-ttu-id="16466-160">Agregar una columna de detalles para cada fila de la cuadrícula</span><span class="sxs-lookup"><span data-stu-id="16466-160">Adding a Details Column for Each Grid Row</span></span>

<span data-ttu-id="16466-161">A continuación, desea ver todos los cursos de cada departamento mostrados en la celda derecha de la cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="16466-161">Next, you want to see all of the courses for each department displayed in the right-hand cell of the grid.</span></span> <span data-ttu-id="16466-162">Para ello, utilizará un control `GridView` anidado y lo enlazará a los datos de la propiedad de navegación `Courses` de la entidad `Department`.</span><span class="sxs-lookup"><span data-stu-id="16466-162">To do this, you'll use a nested `GridView` control and databind it to data from the `Courses` navigation property of the `Department` entity.</span></span>

<span data-ttu-id="16466-163">Abra *departments. aspx* y, en el marcado del control `GridView`, especifique un controlador para el evento `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="16466-163">Open *Departments.aspx* and in the markup for the `GridView` control, specify a handler for the `RowDataBound` event.</span></span> <span data-ttu-id="16466-164">El marcado para la etiqueta de apertura del control ahora es similar al ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="16466-164">The markup for the opening tag of the control now resembles the following example.</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample16.aspx)]

<span data-ttu-id="16466-165">Agregue un nuevo elemento `TemplateField` después del campo de plantilla `Administrator`:</span><span class="sxs-lookup"><span data-stu-id="16466-165">Add a new `TemplateField` element after the `Administrator` template field:</span></span>

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample17.aspx)]

<span data-ttu-id="16466-166">Este marcado crea un control `GridView` anidado que muestra el número de curso y el título de una lista de cursos.</span><span class="sxs-lookup"><span data-stu-id="16466-166">This markup creates a nested `GridView` control that shows the course number and title of a list of courses.</span></span> <span data-ttu-id="16466-167">No especifica ningún origen de datos porque lo enlazará en el código del controlador de `RowDataBound`.</span><span class="sxs-lookup"><span data-stu-id="16466-167">It does not specify a data source because you'll databind it in code in the `RowDataBound` handler.</span></span>

<span data-ttu-id="16466-168">Abra *departments.aspx.CS* y agregue el siguiente controlador para el evento `RowDataBound`:</span><span class="sxs-lookup"><span data-stu-id="16466-168">Open *Departments.aspx.cs* and add the following handler for the `RowDataBound` event:</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample18.cs)]

<span data-ttu-id="16466-169">Este código obtiene la entidad `Department` de los argumentos del evento, convierte la propiedad de navegación `Courses` en una colección `List` y enlaza el `GridView` anidado a la colección.</span><span class="sxs-lookup"><span data-stu-id="16466-169">This code gets the `Department` entity from the event arguments, converts the `Courses` navigation property to a `List` collection, and databinds the nested `GridView` to the collection.</span></span>

<span data-ttu-id="16466-170">Abra el archivo *SchoolRepository.CS* y especifique la carga diligente para la propiedad de navegación `Courses` llamando al método `Include` en la consulta de objeto que cree en el método `GetDepartmentsByName`.</span><span class="sxs-lookup"><span data-stu-id="16466-170">Open the *SchoolRepository.cs* file and specify eager loading for the `Courses` navigation property by calling the `Include` method in the object query that you create in the `GetDepartmentsByName` method.</span></span> <span data-ttu-id="16466-171">La instrucción `return` del método `GetDepartmentsByName` ahora es similar al ejemplo siguiente.</span><span class="sxs-lookup"><span data-stu-id="16466-171">The `return` statement in the `GetDepartmentsByName` method now resembles the following example.</span></span>

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/samples/sample19.cs)]

<span data-ttu-id="16466-172">Ejecute la página.</span><span class="sxs-lookup"><span data-stu-id="16466-172">Run the page.</span></span> <span data-ttu-id="16466-173">Además de la funcionalidad de ordenación y filtrado que agregó anteriormente, el control GridView ahora muestra detalles de los cursos anidados para cada departamento.</span><span class="sxs-lookup"><span data-stu-id="16466-173">In addition to the sorting and filtering capability that you added earlier, the GridView control now shows nested course details for each department.</span></span>

<span data-ttu-id="16466-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="16466-174">[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering/_static/image7.png)</span></span>

<span data-ttu-id="16466-175">Esto completa la introducción a los escenarios de ordenación, filtrado y detalle principal.</span><span class="sxs-lookup"><span data-stu-id="16466-175">This completes the introduction to sorting, filtering, and master-detail scenarios.</span></span> <span data-ttu-id="16466-176">En el siguiente tutorial, verá cómo controlar la simultaneidad.</span><span class="sxs-lookup"><span data-stu-id="16466-176">In the next tutorial, you'll see how to handle concurrency.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="16466-177">[Anterior](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
> [Siguiente](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span><span class="sxs-lookup"><span data-stu-id="16466-177">[Previous](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
[Next](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application.md)</span></span>
