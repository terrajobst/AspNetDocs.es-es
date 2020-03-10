---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Actualizar, eliminar y crear datos con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78474139"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a><span data-ttu-id="ef5e6-104">Actualizar, eliminar y crear datos con enlace de modelos y formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="ef5e6-104">Updating, deleting, and creating data with model binding and web forms</span></span>

<span data-ttu-id="ef5e6-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="ef5e6-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="ef5e6-106">En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="ef5e6-107">El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="ef5e6-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="ef5e6-108">Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="ef5e6-109">En este tutorial se muestra cómo crear, actualizar y eliminar datos con el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-109">This tutorial shows how to create, update, and delete data with model binding.</span></span> <span data-ttu-id="ef5e6-110">Establecerá las siguientes propiedades:</span><span class="sxs-lookup"><span data-stu-id="ef5e6-110">You will set the following properties:</span></span>
> 
> - <span data-ttu-id="ef5e6-111">DeleteMethod</span><span class="sxs-lookup"><span data-stu-id="ef5e6-111">DeleteMethod</span></span>
> - <span data-ttu-id="ef5e6-112">InsertMethod</span><span class="sxs-lookup"><span data-stu-id="ef5e6-112">InsertMethod</span></span>
> - <span data-ttu-id="ef5e6-113">UpdateMethod</span><span class="sxs-lookup"><span data-stu-id="ef5e6-113">UpdateMethod</span></span>
> 
> <span data-ttu-id="ef5e6-114">Estas propiedades reciben el nombre del método que controla la operación correspondiente.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-114">These properties receive the name of the method that handles the corresponding operation.</span></span> <span data-ttu-id="ef5e6-115">Dentro de ese método, se proporciona la lógica para interactuar con los datos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-115">Within that method, you provide the logic for interacting with the data.</span></span>
> 
> <span data-ttu-id="ef5e6-116">Este tutorial se basa en el proyecto creado en la primera [parte](retrieving-data.md) de la serie.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-116">This tutorial builds on the project created in the first [part](retrieving-data.md) of the series.</span></span>
> 
> <span data-ttu-id="ef5e6-117">Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-117">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="ef5e6-118">El código descargable funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-118">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="ef5e6-119">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-119">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="ef5e6-120">Lo que va a compilar</span><span class="sxs-lookup"><span data-stu-id="ef5e6-120">What you'll build</span></span>

<span data-ttu-id="ef5e6-121">En este tutorial, hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="ef5e6-121">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="ef5e6-122">Agregar plantillas de datos dinámicos</span><span class="sxs-lookup"><span data-stu-id="ef5e6-122">Add dynamic data templates</span></span>
2. <span data-ttu-id="ef5e6-123">Habilitar la actualización y eliminación de datos mediante los métodos de enlace de modelos</span><span class="sxs-lookup"><span data-stu-id="ef5e6-123">Enable updating and deleting data through model binding methods</span></span>
3. <span data-ttu-id="ef5e6-124">Aplicar reglas de validación de datos: habilitar la creación de un nuevo registro en la base de datos</span><span class="sxs-lookup"><span data-stu-id="ef5e6-124">Apply data validation rules - Enable creating a new record in the database</span></span>

## <a name="add-dynamic-data-templates"></a><span data-ttu-id="ef5e6-125">Agregar plantillas de datos dinámicos</span><span class="sxs-lookup"><span data-stu-id="ef5e6-125">Add dynamic data templates</span></span>

<span data-ttu-id="ef5e6-126">Para proporcionar la mejor experiencia de usuario y minimizar la repetición de código, utilizará plantillas de datos dinámicos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-126">To provide the best user experience and minimize code repetition, you will use dynamic data templates.</span></span> <span data-ttu-id="ef5e6-127">Puede integrar fácilmente plantillas de datos dinámicos predefinidas en el sitio existente mediante la instalación de un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-127">You can easily integrate pre-built dynamic data templates into your existing site by installing a NuGet package.</span></span>

<span data-ttu-id="ef5e6-128">En **administrar paquetes NuGet**, instale **DynamicDataTemplatesCS**.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-128">From the **Manage NuGet Packages**, install the **DynamicDataTemplatesCS**.</span></span>

![plantillas de datos dinámicos](updating-deleting-and-creating-data/_static/image1.png)

<span data-ttu-id="ef5e6-130">Observe que el proyecto incluye ahora una carpeta denominada **DynamicData**.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-130">Notice that your project now includes a folder named **DynamicData**.</span></span> <span data-ttu-id="ef5e6-131">En esa carpeta, encontrará las plantillas que se aplican automáticamente a los controles dinámicos de los formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-131">In that folder, you will find the templates that are automatically applied to dynamic controls in your web forms.</span></span>

![carpeta de datos dinámicos](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a><span data-ttu-id="ef5e6-133">Habilitar la actualización y eliminación</span><span class="sxs-lookup"><span data-stu-id="ef5e6-133">Enable updating and deleting</span></span>

<span data-ttu-id="ef5e6-134">Permitir a los usuarios actualizar y eliminar registros en la base de datos es muy similar al proceso de recuperación de datos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-134">Enabling users to update and delete records in the database is very similar to the process for retrieving data.</span></span> <span data-ttu-id="ef5e6-135">En las propiedades **UpdateMethod** y **DeleteMethod** , especifique los nombres de los métodos que realizan esas operaciones.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-135">In the **UpdateMethod** and **DeleteMethod** properties, you specify the names of the methods that perform those operations.</span></span> <span data-ttu-id="ef5e6-136">Con un control GridView, también puede especificar la generación automática de botones de edición y eliminación.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-136">With a GridView control, you can also specify the automatic generation of edit and delete buttons.</span></span> <span data-ttu-id="ef5e6-137">En el código resaltado siguiente se muestran las adiciones al código GridView.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-137">The following highlighted code shows the additions to the GridView code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

<span data-ttu-id="ef5e6-138">En el archivo de código subyacente, agregue una instrucción using para **System. Data. Entity. Infrastructure**.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-138">In the code-behind file, add a using statement for **System.Data.Entity.Infrastructure**.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

<span data-ttu-id="ef5e6-139">A continuación, agregue los siguientes métodos de actualización y eliminación.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-139">Then, add the following update and delete methods.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

<span data-ttu-id="ef5e6-140">El método **TryUpdateModel** aplica los valores enlazados a datos coincidentes del formulario web al elemento de datos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-140">The **TryUpdateModel** method applies the matching data-bound values from the web form to the data item.</span></span> <span data-ttu-id="ef5e6-141">El elemento de datos se recupera según el valor del parámetro id.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-141">The data item is retrieved based on the value of the id parameter.</span></span>

## <a name="enforce-validation-requirements"></a><span data-ttu-id="ef5e6-142">Exigir requisitos de validación</span><span class="sxs-lookup"><span data-stu-id="ef5e6-142">Enforce validation requirements</span></span>

<span data-ttu-id="ef5e6-143">Los atributos de validación que se aplican a las propiedades FirstName, LastName y Year de la clase Student se aplican automáticamente al actualizar los datos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-143">The validation attributes that you applied to the FirstName, LastName, and Year properties in the Student class are automatically enforced when updating the data.</span></span> <span data-ttu-id="ef5e6-144">Los controles DynamicField agregan validadores de cliente y servidor basados en los atributos de validación.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-144">The DynamicField controls add client and server validators based on the validation attributes.</span></span> <span data-ttu-id="ef5e6-145">Las propiedades FirstName y LastName son obligatorias.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-145">The FirstName and LastName properties are both required.</span></span> <span data-ttu-id="ef5e6-146">FirstName no puede tener más de 20 caracteres de longitud y LastName no puede superar los 40 caracteres.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-146">FirstName cannot exceed 20 characters in length, and LastName cannot exceed 40 characters.</span></span> <span data-ttu-id="ef5e6-147">Year debe ser un valor válido para la enumeración AcademicYear.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-147">Year must be a valid value for the AcademicYear enumeration.</span></span>

<span data-ttu-id="ef5e6-148">Si el usuario infringe uno de los requisitos de validación, la actualización no continúa.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-148">If the user violates one of the validation requirements, the update does not proceed.</span></span> <span data-ttu-id="ef5e6-149">Para ver el mensaje de error, agregue un control ValidationSummary sobre GridView.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-149">To see the error message, add a ValidationSummary control above the GridView.</span></span> <span data-ttu-id="ef5e6-150">Para mostrar los errores de validación del enlace de modelos, establezca la propiedad **ShowModelStateErrors** establecida en **true**.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-150">To display the validation errors from model binding, set the **ShowModelStateErrors** property set to **true**.</span></span> 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

<span data-ttu-id="ef5e6-151">Ejecute la aplicación web y actualice y elimine cualquiera de los registros.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-151">Run the web application, and update and delete any of the records.</span></span>

![actualizar datos](updating-deleting-and-creating-data/_static/image3.png)

<span data-ttu-id="ef5e6-153">Observe que, cuando en el modo de edición, el valor de la propiedad Year se representa automáticamente como una lista desplegable.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-153">Notice that when in the edit mode the value for the Year property is automatically rendered as a drop down list.</span></span> <span data-ttu-id="ef5e6-154">La propiedad Year es un valor de enumeración y la plantilla de datos dinámicos para un valor de enumeración especifica una lista desplegable para su edición.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-154">The Year property is an enumeration value, and the dynamic data template for an enumeration value specifies a drop down list for editing.</span></span> <span data-ttu-id="ef5e6-155">Para encontrar esa plantilla, abra la **enumeración\_archivo Edit. ascx** en la carpeta **DynamicData**/**FieldTemplates**</span><span class="sxs-lookup"><span data-stu-id="ef5e6-155">You can find that template by opening the **Enumeration\_Edit.ascx** file in the **DynamicData**/**FieldTemplates** folder.</span></span>

<span data-ttu-id="ef5e6-156">Si proporciona valores válidos, la actualización se completará correctamente.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-156">If you provide valid values, the update completes successfully.</span></span> <span data-ttu-id="ef5e6-157">Si infringe uno de los requisitos de validación, la actualización no continúa y se muestra un mensaje de error sobre la cuadrícula.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-157">If you violate one of the validation requirements, the update does not proceed and an error message is displayed above the grid.</span></span>

![mensaje de error](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a><span data-ttu-id="ef5e6-159">Agregar nuevos registros</span><span class="sxs-lookup"><span data-stu-id="ef5e6-159">Add new records</span></span>

<span data-ttu-id="ef5e6-160">El control GridView no incluye la propiedad **InsertMethod** y, por tanto, no se puede usar para agregar un nuevo registro con el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-160">The GridView control does not include the **InsertMethod** property and therefore cannot be used for adding a new record with model binding.</span></span> <span data-ttu-id="ef5e6-161">Puede encontrar la propiedad InsertMethod en los controles **FormView**, **DetailsView**o **ListView** .</span><span class="sxs-lookup"><span data-stu-id="ef5e6-161">You can find the InsertMethod property in the **FormView**, **DetailsView**, or **ListView** controls.</span></span> <span data-ttu-id="ef5e6-162">En este tutorial, usará un control FormView para agregar un nuevo registro.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-162">In this tutorial, you will use a FormView control to add a new record.</span></span>

<span data-ttu-id="ef5e6-163">En primer lugar, agregue un vínculo a la nueva página que creará para agregar un nuevo registro.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-163">First, add a link to the new page you will create for adding a new record.</span></span> <span data-ttu-id="ef5e6-164">Encima de ValidationSummary, agregue:</span><span class="sxs-lookup"><span data-stu-id="ef5e6-164">Above the ValidationSummary, add:</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

<span data-ttu-id="ef5e6-165">El nuevo vínculo aparecerá en la parte superior del contenido de la página Students.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-165">The new link will appear at the top of the content for the Students page.</span></span>

![nuevo vínculo](updating-deleting-and-creating-data/_static/image5.png)

<span data-ttu-id="ef5e6-167">Después, agregue un nuevo formulario Web Forms mediante una página maestra y asígnele el nombre **AddStudent**.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-167">Then, add a new web form using a master page, and name it **AddStudent**.</span></span> <span data-ttu-id="ef5e6-168">Seleccione site. Master como la página maestra.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-168">Select Site.Master as the master page.</span></span>

<span data-ttu-id="ef5e6-169">Se representarán los campos para agregar un estudiante nuevo mediante un control **DynamicEntity** .</span><span class="sxs-lookup"><span data-stu-id="ef5e6-169">You will render the fields for adding a new student by using a **DynamicEntity** control.</span></span> <span data-ttu-id="ef5e6-170">El control DynamicEntity representa las propiedades editables de la clase especificada en la propiedad ItemType.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-170">The DynamicEntity control renders that editable properties in the class specified in the ItemType property.</span></span> <span data-ttu-id="ef5e6-171">La propiedad StudentID se marcó con el atributo **[ScaffoldColumn (false)]** para que no se represente.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-171">The StudentID property was marked with the **[ScaffoldColumn(false)]** attribute so it is not rendered.</span></span> <span data-ttu-id="ef5e6-172">En el marcador de posición MainContent de la página AddStudent, agregue el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-172">In the MainContent placeholder of the AddStudent page, add the following code.</span></span>

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

<span data-ttu-id="ef5e6-173">En el archivo de código subyacente (AddStudent.aspx.cs), agregue una instrucción **using** para el espacio de nombres **ContosoUniversityModelBinding. Models** .</span><span class="sxs-lookup"><span data-stu-id="ef5e6-173">In the code-behind file (AddStudent.aspx.cs), add a **using** statement for the **ContosoUniversityModelBinding.Models** namespace.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

<span data-ttu-id="ef5e6-174">A continuación, agregue los métodos siguientes para especificar cómo se inserta un nuevo registro y un controlador de eventos para el botón Cancelar.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-174">Then, add the following methods to specify how to insert a new record and an event handler for the cancel button.</span></span>

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

<span data-ttu-id="ef5e6-175">Guarde todos los cambios.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-175">Save all of the changes.</span></span>

<span data-ttu-id="ef5e6-176">Ejecute la aplicación web y cree un estudiante nuevo.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-176">Run the web application and create a new student.</span></span>

![Agregar nuevo estudiante](updating-deleting-and-creating-data/_static/image6.png)

<span data-ttu-id="ef5e6-178">Haga clic en **Insertar** y observe que se ha creado el estudiante nuevo.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-178">Click **Insert** and notice the new student has been created.</span></span>

![Mostrar nuevo estudiante](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a><span data-ttu-id="ef5e6-180">Conclusión</span><span class="sxs-lookup"><span data-stu-id="ef5e6-180">Conclusion</span></span>

<span data-ttu-id="ef5e6-181">En este tutorial, ha habilitado la actualización, la eliminación y la creación de datos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-181">In this tutorial, you enabled updating, deleting, and creating data.</span></span> <span data-ttu-id="ef5e6-182">Las reglas de validación garantizadas se aplican al interactuar con los datos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-182">You ensured validation rules are applied when interacting with the data.</span></span>

<span data-ttu-id="ef5e6-183">En el siguiente [tutorial](sorting-paging-and-filtering-data.md) de esta serie, podrá habilitar la ordenación, la paginación y el filtrado de datos.</span><span class="sxs-lookup"><span data-stu-id="ef5e6-183">In the next [tutorial](sorting-paging-and-filtering-data.md) in this series, you will enable sorting, paging, and filtering data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ef5e6-184">[Anterior](retrieving-data.md)
> [Siguiente](sorting-paging-and-filtering-data.md)</span><span class="sxs-lookup"><span data-stu-id="ef5e6-184">[Previous](retrieving-data.md)
[Next](sorting-paging-and-filtering-data.md)</span></span>
