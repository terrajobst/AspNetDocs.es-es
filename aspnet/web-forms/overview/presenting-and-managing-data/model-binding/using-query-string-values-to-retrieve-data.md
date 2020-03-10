---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Usar valores de cadena de consulta para filtrar datos con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519085"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a><span data-ttu-id="c537e-104">Usar valores de cadena de consulta para filtrar datos con el enlace de modelos y formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="c537e-104">Using query string values to filter data with model binding and web forms</span></span>

<span data-ttu-id="c537e-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c537e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="c537e-106">En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="c537e-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="c537e-107">El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="c537e-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="c537e-108">Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="c537e-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="c537e-109">En este tutorial se muestra cómo pasar un valor en la cadena de consulta y usar ese valor para recuperar datos a través del enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="c537e-109">This tutorial shows how to pass a value in the query string and use that value to retrieve data through model binding.</span></span>
> 
> <span data-ttu-id="c537e-110">Este tutorial se basa en el proyecto creado en las partes [anteriores](retrieving-data.md) de la serie.</span><span class="sxs-lookup"><span data-stu-id="c537e-110">This tutorial builds on the project created in the [earlier](retrieving-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="c537e-111">Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="c537e-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="c537e-112">El código descargable funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="c537e-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="c537e-113">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="c537e-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="c537e-114">Lo que va a compilar</span><span class="sxs-lookup"><span data-stu-id="c537e-114">What you'll build</span></span>

<span data-ttu-id="c537e-115">En este tutorial, hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="c537e-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="c537e-116">Agregar una nueva página para mostrar los cursos inscritos de un estudiante</span><span class="sxs-lookup"><span data-stu-id="c537e-116">Add a new page to show the enrolled courses for a student</span></span>
2. <span data-ttu-id="c537e-117">Recuperar los cursos inscritos del estudiante seleccionado en función de un valor de la cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="c537e-117">Retrieve the enrolled courses for the selected student based on a value in the query string</span></span>
3. <span data-ttu-id="c537e-118">Agregar un hipervínculo con un valor de cadena de consulta de la vista de cuadrícula a la nueva página</span><span class="sxs-lookup"><span data-stu-id="c537e-118">Add a hyperlink with a query string value from the grid view to the new page</span></span>

<span data-ttu-id="c537e-119">Los pasos de este tutorial son bastante similares a lo que hizo en el [tutorial](sorting-paging-and-filtering-data.md) anterior para filtrar los estudiantes mostrados en función de la selección del usuario en una lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="c537e-119">The steps in this tutorial are fairly similar to what you did in the earlier [tutorial](sorting-paging-and-filtering-data.md) to filter the displayed students based on the user selection in a drop down list.</span></span> <span data-ttu-id="c537e-120">En ese tutorial, ha usado el atributo **control** en el método SELECT para especificar que el valor del parámetro procede de un control.</span><span class="sxs-lookup"><span data-stu-id="c537e-120">In that tutorial, you used the **Control** attribute in the select method to specify that the parameter value comes from a control.</span></span> <span data-ttu-id="c537e-121">En este tutorial, usará el atributo **QueryString** en el método SELECT para especificar que el valor del parámetro procede de la cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="c537e-121">In this tutorial, you'll use the **QueryString** attribute in the select method to specify that the parameter value comes from the query string.</span></span>

## <a name="add-new-page-for-displaying-a-students-courses"></a><span data-ttu-id="c537e-122">Agregar una nueva página para mostrar los cursos de un estudiante</span><span class="sxs-lookup"><span data-stu-id="c537e-122">Add new page for displaying a student's courses</span></span>

<span data-ttu-id="c537e-123">Agregue un nuevo formulario Web Forms que use la página maestra site. Master y asigne el nombre **cursos**a la página.</span><span class="sxs-lookup"><span data-stu-id="c537e-123">Add a new web form that uses the Site.master master page, and name the page **Courses**.</span></span>

<span data-ttu-id="c537e-124">En el archivo **Courses. aspx** , agregue una vista de cuadrícula para mostrar los cursos del alumno seleccionado.</span><span class="sxs-lookup"><span data-stu-id="c537e-124">In the **Courses.aspx** file, add a grid view to display the courses for the selected student.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a><span data-ttu-id="c537e-125">Definir el método Select</span><span class="sxs-lookup"><span data-stu-id="c537e-125">Define the select method</span></span>

<span data-ttu-id="c537e-126">En **Courses.aspx.CS**, agregará el método Select con el nombre especificado en la propiedad **SelectMethod** de la vista de cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="c537e-126">In **Courses.aspx.cs**, you will add the select method with the name you specified in the grid view's **SelectMethod** property.</span></span> <span data-ttu-id="c537e-127">En ese método, definirá la consulta para recuperar los cursos de un estudiante y especificará que el parámetro proviene de un valor de cadena de consulta con el mismo nombre que el parámetro.</span><span class="sxs-lookup"><span data-stu-id="c537e-127">In that method, you'll define the query for retrieving a student's courses, and specify that the parameter comes from a query string value with the same name as the parameter.</span></span>

<span data-ttu-id="c537e-128">En primer lugar, debe agregar las siguientes instrucciones **using** .</span><span class="sxs-lookup"><span data-stu-id="c537e-128">First, you must add the following **using** statements.</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

<span data-ttu-id="c537e-129">A continuación, agregue el código siguiente a Courses.aspx.cs:</span><span class="sxs-lookup"><span data-stu-id="c537e-129">Then, add the following code to Courses.aspx.cs:</span></span>

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

<span data-ttu-id="c537e-130">El atributo QueryString significa que un valor de cadena de consulta denominado StudentID se asigna automáticamente al parámetro de este método.</span><span class="sxs-lookup"><span data-stu-id="c537e-130">The QueryString attribute means that a query string value named StudentID is automatically assigned to the parameter in this method.</span></span>

## <a name="add-hyperlink-with-query-string-value"></a><span data-ttu-id="c537e-131">Agregar hipervínculo con valor de cadena de consulta</span><span class="sxs-lookup"><span data-stu-id="c537e-131">Add hyperlink with query string value</span></span>

<span data-ttu-id="c537e-132">En la vista de cuadrícula de Students. aspx, agregará un campo de hipervínculo que se vincula a la página nuevos cursos.</span><span class="sxs-lookup"><span data-stu-id="c537e-132">In the grid view on Students.aspx, you will add a hyperlink field that links to your new Courses page.</span></span> <span data-ttu-id="c537e-133">El hipervínculo incluirá un valor de cadena de consulta con el identificador del alumno.</span><span class="sxs-lookup"><span data-stu-id="c537e-133">The hyperlink will include a query string value with the student's id.</span></span>

<span data-ttu-id="c537e-134">En Students. aspx, agregue el siguiente campo a las columnas de la vista de cuadrícula justo debajo del campo para el total de créditos.</span><span class="sxs-lookup"><span data-stu-id="c537e-134">In Students.aspx, add the following field to the grid view columns just below the field for Total Credits.</span></span>

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

<span data-ttu-id="c537e-135">Ejecute la aplicación y observe que la vista de cuadrícula incluye ahora el vínculo cursos.</span><span class="sxs-lookup"><span data-stu-id="c537e-135">Run the application and notice that the grid view now includes the Courses link.</span></span>

![Agregar hipervínculo](using-query-string-values-to-retrieve-data/_static/image1.png)

<span data-ttu-id="c537e-137">Al hacer clic en uno de los vínculos, verá los cursos inscritos de los estudiantes.</span><span class="sxs-lookup"><span data-stu-id="c537e-137">When you click one of the links, you'll see that student's enrolled courses.</span></span>

![Mostrar cursos](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="c537e-139">Conclusión</span><span class="sxs-lookup"><span data-stu-id="c537e-139">Conclusion</span></span>

<span data-ttu-id="c537e-140">En este tutorial, ha agregado un vínculo con un valor de cadena de consulta.</span><span class="sxs-lookup"><span data-stu-id="c537e-140">In this tutorial, you added a link with a query string value.</span></span> <span data-ttu-id="c537e-141">Usó ese valor de cadena de consulta para el valor del parámetro en el método Select.</span><span class="sxs-lookup"><span data-stu-id="c537e-141">You used that query string value for the parameter value in the select method.</span></span>

<span data-ttu-id="c537e-142">En el siguiente [tutorial](adding-business-logic-layer.md), moverá el código de los archivos de código subyacente a una capa de lógica de negocios y una capa de acceso a datos.</span><span class="sxs-lookup"><span data-stu-id="c537e-142">In the next [tutorial](adding-business-logic-layer.md), you will move the code from the code-behind files into a business logic layer and a data access layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c537e-143">[Anterior](integrating-jquery-ui.md)
> [Siguiente](adding-business-logic-layer.md)</span><span class="sxs-lookup"><span data-stu-id="c537e-143">[Previous](integrating-jquery-ui.md)
[Next](adding-business-logic-layer.md)</span></span>
