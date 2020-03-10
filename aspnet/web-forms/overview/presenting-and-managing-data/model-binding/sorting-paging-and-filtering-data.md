---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Ordenar, paginar y filtrar datos con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441067"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a><span data-ttu-id="66287-104">Ordenar, paginar y filtrar datos con el enlace de modelos y formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="66287-104">Sorting, paging, and filtering data with model binding and web forms</span></span>

<span data-ttu-id="66287-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="66287-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="66287-106">En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="66287-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="66287-107">El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="66287-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="66287-108">Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="66287-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="66287-109">En este tutorial se muestra cómo agregar ordenación, paginación y filtrado de los datos a través del enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="66287-109">This tutorial shows how to add sorting, paging, and filtering of the data through model binding.</span></span>
> 
> <span data-ttu-id="66287-110">Este tutorial se basa en el proyecto creado en la primera [parte](retrieving-data.md) de la serie.</span><span class="sxs-lookup"><span data-stu-id="66287-110">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="66287-111">Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="66287-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="66287-112">El código descargable funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="66287-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="66287-113">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="66287-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="66287-114">Lo que va a compilar</span><span class="sxs-lookup"><span data-stu-id="66287-114">What you'll build</span></span>

<span data-ttu-id="66287-115">En este tutorial, hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="66287-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="66287-116">Habilitar la ordenación y paginación de los datos</span><span class="sxs-lookup"><span data-stu-id="66287-116">Enable sorting and paging of the data</span></span>
2. <span data-ttu-id="66287-117">Habilitar el filtrado de los datos en función de una selección por parte del usuario</span><span class="sxs-lookup"><span data-stu-id="66287-117">Enable filtering of the data based on a selection by the user</span></span>

## <a name="add-sorting"></a><span data-ttu-id="66287-118">Adición de ordenación</span><span class="sxs-lookup"><span data-stu-id="66287-118">Add sorting</span></span>

<span data-ttu-id="66287-119">Habilitar la ordenación en GridView es muy fácil.</span><span class="sxs-lookup"><span data-stu-id="66287-119">Enabling sorting in the GridView is very easy.</span></span> <span data-ttu-id="66287-120">En el archivo Student. aspx, simplemente establezca **AllowSorting** en **true** en GridView.</span><span class="sxs-lookup"><span data-stu-id="66287-120">In the Student.aspx file, simply set **AllowSorting** to **true** in the GridView.</span></span> <span data-ttu-id="66287-121">No es necesario establecer un valor de **SortExpression** para cada columna, ya que el campo de campos se usa automáticamente.</span><span class="sxs-lookup"><span data-stu-id="66287-121">You do not need to set a **SortExpression** value for each column as the DataField is automatically used.</span></span> <span data-ttu-id="66287-122">GridView modifica la consulta para incluir el orden de los datos por el valor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="66287-122">The GridView modifies the query to include ordering the data by the selected value.</span></span> <span data-ttu-id="66287-123">En el código resaltado siguiente se muestra la adición que debe hacer para habilitar la ordenación.</span><span class="sxs-lookup"><span data-stu-id="66287-123">The highlighted code below shows the addition you need to make to enable sorting.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

<span data-ttu-id="66287-124">Ejecutar la aplicación web y probar la ordenación de los registros de estudiante por los valores de columnas diferentes.</span><span class="sxs-lookup"><span data-stu-id="66287-124">Run the web application, and test sorting student records by the values in different columns.</span></span>

![ordenar estudiantes](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a><span data-ttu-id="66287-126">Adición de paginación</span><span class="sxs-lookup"><span data-stu-id="66287-126">Add paging</span></span>

<span data-ttu-id="66287-127">También es muy fácil habilitar la paginación.</span><span class="sxs-lookup"><span data-stu-id="66287-127">Enabling paging is also very easy.</span></span> <span data-ttu-id="66287-128">En GridView, establezca la propiedad **AllowPaging** en **true** y establezca la propiedad **pageSize** en el número de registros que desea mostrar en cada página.</span><span class="sxs-lookup"><span data-stu-id="66287-128">In the GridView, set the **AllowPaging** property to **true** and set the **PageSize** property to the number of records you wish to display on each page.</span></span> <span data-ttu-id="66287-129">En este tutorial, puede establecerlo en 4.</span><span class="sxs-lookup"><span data-stu-id="66287-129">In this tutorial, you can set it to 4.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

<span data-ttu-id="66287-130">Ejecute la aplicación web y observe que ahora los registros se dividen en varias páginas con un máximo de 4 registros en una sola página.</span><span class="sxs-lookup"><span data-stu-id="66287-130">Run the web application, and notice that now the records are divided over multiple pages with no more than 4 records displayed on a single page.</span></span>

![Agregar paginación](sorting-paging-and-filtering-data/_static/image4.png)

<span data-ttu-id="66287-132">La ejecución diferida de consultas mejora la eficacia de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="66287-132">Deferred query execution improves the application efficiency.</span></span> <span data-ttu-id="66287-133">En lugar de recuperar todo el conjunto de datos, GridView modifica la consulta para recuperar solo los registros de la página actual.</span><span class="sxs-lookup"><span data-stu-id="66287-133">Instead of retrieving the entire data set, the GridView modifies the query to retrieve only the records for the current page.</span></span>

## <a name="filter-records-by-user-selection"></a><span data-ttu-id="66287-134">Filtrar registros por selección de usuario</span><span class="sxs-lookup"><span data-stu-id="66287-134">Filter records by user selection</span></span>

<span data-ttu-id="66287-135">El enlace de modelos agrega varios atributos que le permiten designar cómo establecer el valor de un parámetro en un método de enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="66287-135">Model binding adds several attributes which enable you to designate how to set the value for a parameter in a model binding method.</span></span> <span data-ttu-id="66287-136">Estos atributos están en el espacio de nombres **System. Web. ModelBinding** .</span><span class="sxs-lookup"><span data-stu-id="66287-136">These attributes are in the **System.Web.ModelBinding** namespace.</span></span> <span data-ttu-id="66287-137">Son los siguientes:</span><span class="sxs-lookup"><span data-stu-id="66287-137">They include:</span></span>

- <span data-ttu-id="66287-138">Control</span><span class="sxs-lookup"><span data-stu-id="66287-138">Control</span></span>
- <span data-ttu-id="66287-139">Cookie</span><span class="sxs-lookup"><span data-stu-id="66287-139">Cookie</span></span>
- <span data-ttu-id="66287-140">Form</span><span class="sxs-lookup"><span data-stu-id="66287-140">Form</span></span>
- <span data-ttu-id="66287-141">Perfil</span><span class="sxs-lookup"><span data-stu-id="66287-141">Profile</span></span>
- <span data-ttu-id="66287-142">QueryString</span><span class="sxs-lookup"><span data-stu-id="66287-142">QueryString</span></span>
- <span data-ttu-id="66287-143">RouteData</span><span class="sxs-lookup"><span data-stu-id="66287-143">RouteData</span></span>
- <span data-ttu-id="66287-144">Sesión</span><span class="sxs-lookup"><span data-stu-id="66287-144">Session</span></span>
- <span data-ttu-id="66287-145">Perfil</span><span class="sxs-lookup"><span data-stu-id="66287-145">UserProfile</span></span>
- <span data-ttu-id="66287-146">ViewState</span><span class="sxs-lookup"><span data-stu-id="66287-146">ViewState</span></span>

<span data-ttu-id="66287-147">En este tutorial, usará el valor de un control para filtrar los registros que se muestran en GridView.</span><span class="sxs-lookup"><span data-stu-id="66287-147">In this tutorial, you will use a control's value to filter which records are displayed in the GridView.</span></span> <span data-ttu-id="66287-148">Agregará el atributo de **control** al método de consulta que creó anteriormente.</span><span class="sxs-lookup"><span data-stu-id="66287-148">You will add the **Control** attribute to the query method you had created earlier.</span></span> <span data-ttu-id="66287-149">En un tutorial [posterior](using-query-string-values-to-retrieve-data.md) , se aplicará el atributo **QueryString** a un parámetro para especificar que el valor del parámetro proviene de un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="66287-149">In a [later](using-query-string-values-to-retrieve-data.md) tutorial, you will apply the **QueryString** attribute to a parameter to specify that the parameter value comes from a query string value.</span></span>

<span data-ttu-id="66287-150">En primer lugar, sobre ValidationSummary, agregue una lista desplegable para filtrar los estudiantes que se muestran.</span><span class="sxs-lookup"><span data-stu-id="66287-150">First, above the ValidationSummary, add a drop down list for filtering which students are shown.</span></span>

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

<span data-ttu-id="66287-151">En el archivo de código subyacente, modifique el método SELECT para recibir un valor del control y establezca el nombre del parámetro en el nombre del control que proporciona el valor.</span><span class="sxs-lookup"><span data-stu-id="66287-151">In the code-behind file, modify the select method to receive a value from the control, and set the name of the parameter to the name of the control that provides the value.</span></span>

<span data-ttu-id="66287-152">Debe agregar una instrucción **using** para el espacio de nombres **System. Web. ModelBinding** para resolver el atributo de control.</span><span class="sxs-lookup"><span data-stu-id="66287-152">You must add a **using** statement for the **System.Web.ModelBinding** namespace to resolve the Control attribute.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

<span data-ttu-id="66287-153">En el código siguiente se muestra que el método Select ha vuelto a trabajar para filtrar los datos devueltos en función del valor de la lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="66287-153">The following code shows the select method re-worked to filter the returned data based on the value of the drop down list.</span></span> <span data-ttu-id="66287-154">Al agregar un atributo de control antes de un parámetro, se especifica que el valor de este parámetro proviene de un control con el mismo nombre.</span><span class="sxs-lookup"><span data-stu-id="66287-154">Adding a control attribute before a parameter specifies that the value for this parameter comes from a control with the same name.</span></span>

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

<span data-ttu-id="66287-155">Ejecute la aplicación web y seleccione valores diferentes en la lista desplegable para filtrar la lista de alumnos.</span><span class="sxs-lookup"><span data-stu-id="66287-155">Run the web application and select different values from the drop down list to filter the list of students.</span></span>

![filtrar estudiantes](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a><span data-ttu-id="66287-157">Conclusión</span><span class="sxs-lookup"><span data-stu-id="66287-157">Conclusion</span></span>

<span data-ttu-id="66287-158">En este tutorial, ha habilitado la ordenación y la paginación de los datos.</span><span class="sxs-lookup"><span data-stu-id="66287-158">In this tutorial, you enabled sorting and paging of the data.</span></span> <span data-ttu-id="66287-159">También ha habilitado el filtrado de los datos por el valor de un control.</span><span class="sxs-lookup"><span data-stu-id="66287-159">You also enabled filtering the data by the value of a control.</span></span>

<span data-ttu-id="66287-160">En el siguiente [tutorial](integrating-jquery-ui.md) , mejorará la interfaz de usuario mediante la integración de un widget de jQuery UI en la plantilla de datos dinámicos.</span><span class="sxs-lookup"><span data-stu-id="66287-160">In the next [tutorial](integrating-jquery-ui.md) you will enhance the UI by integrating a JQuery UI widget into the dynamic data template.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="66287-161">[Anterior](updating-deleting-and-creating-data.md)
> [Siguiente](integrating-jquery-ui.md)</span><span class="sxs-lookup"><span data-stu-id="66287-161">[Previous](updating-deleting-and-creating-data.md)
[Next](integrating-jquery-ui.md)</span></span>
