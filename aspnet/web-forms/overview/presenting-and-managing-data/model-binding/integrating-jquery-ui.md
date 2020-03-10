---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integración del DatePicker de JQuery UI con el enlace de modelos y formularios Web Forms | Microsoft Docs
author: Rick-Anderson
description: En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET. El enlace de modelos hace que la interacción de datos sea más recta-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78521983"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a><span data-ttu-id="bf8e7-104">Integración del DatePicker de JQuery UI con el enlace de modelos y formularios Web Forms</span><span class="sxs-lookup"><span data-stu-id="bf8e7-104">Integrating JQuery UI Datepicker with model binding and web forms</span></span>

<span data-ttu-id="bf8e7-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bf8e7-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bf8e7-106">En esta serie de tutoriales se muestran los aspectos básicos del uso del enlace de modelos con un proyecto de formularios Web Forms ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-106">This tutorial series demonstrates basic aspects of using model binding with an ASP.NET Web Forms project.</span></span> <span data-ttu-id="bf8e7-107">El enlace de modelos hace que la interacción de datos sea más directa que tratar con objetos de origen de datos (como ObjectDataSource o SqlDataSource).</span><span class="sxs-lookup"><span data-stu-id="bf8e7-107">Model binding makes data interaction more straight-forward than dealing with data source objects (such as ObjectDataSource or SqlDataSource).</span></span> <span data-ttu-id="bf8e7-108">Esta serie comienza con material introductorio y se traslada a conceptos más avanzados en los tutoriales posteriores.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-108">This series starts with introductory material and moves to more advanced concepts in later tutorials.</span></span>
> 
> <span data-ttu-id="bf8e7-109">En este tutorial se muestra cómo agregar el [Widget DatePicker](http://jqueryui.com/datepicker/) de la interfaz de usuario de jQuery a un formulario web y cómo usar el enlace de modelos para actualizar la base de datos con el valor seleccionado.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-109">This tutorial shows how to add the JQuery UI [Datepicker widget](http://jqueryui.com/datepicker/) to a Web Form, and use model binding to update the database with the selected value.</span></span>
> 
> <span data-ttu-id="bf8e7-110">Este tutorial se basa en el proyecto creado en la [primera](retrieving-data.md) y [segunda](updating-deleting-and-creating-data.md) parte de la serie.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-110">This tutorial builds on the project created in the [first](retrieving-data.md) and [second](updating-deleting-and-creating-data.md) parts of the series.</span></span>
> 
> <span data-ttu-id="bf8e7-111">Puede [Descargar](https://go.microsoft.com/fwlink/?LinkId=286116) el proyecto completo en C# o VB.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-111">You can [download](https://go.microsoft.com/fwlink/?LinkId=286116) the complete project in C# or VB.</span></span> <span data-ttu-id="bf8e7-112">El código descargable funciona con Visual Studio 2012 o Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-112">The downloadable code works with either Visual Studio 2012 or Visual Studio 2013.</span></span> <span data-ttu-id="bf8e7-113">Usa la plantilla de Visual Studio 2012, que es ligeramente diferente de la plantilla de Visual Studio 2013 que se muestra en este tutorial.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-113">It uses the Visual Studio 2012 template, which is slightly different than the Visual Studio 2013 template shown in this tutorial.</span></span>

## <a name="what-youll-build"></a><span data-ttu-id="bf8e7-114">Lo que va a compilar</span><span class="sxs-lookup"><span data-stu-id="bf8e7-114">What you'll build</span></span>

<span data-ttu-id="bf8e7-115">En este tutorial, hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bf8e7-115">In this tutorial, you'll:</span></span>

1. <span data-ttu-id="bf8e7-116">Agregar una propiedad al modelo para registrar la fecha de inscripción del estudiante</span><span class="sxs-lookup"><span data-stu-id="bf8e7-116">Add a property to your model to record the student's enrollment date</span></span>
2. <span data-ttu-id="bf8e7-117">Permitir al usuario seleccionar la fecha de inscripción mediante el widget DatePicker de JQuery UI</span><span class="sxs-lookup"><span data-stu-id="bf8e7-117">Enable the user to select the enrollment date using the JQuery UI Datepicker widget</span></span>
3. <span data-ttu-id="bf8e7-118">Aplicar reglas de validación para la fecha de inscripción</span><span class="sxs-lookup"><span data-stu-id="bf8e7-118">Enforce validation rules for the enrollment date</span></span>

<span data-ttu-id="bf8e7-119">El widget DatePicker de la interfaz de usuario de JQuery permite a los usuarios seleccionar fácilmente una fecha de un calendario que aparece cuando el usuario interactúa con el campo.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-119">The JQuery UI Datepicker widget enables users to easily select a date from a calendar that pops up when the user interacts with the field.</span></span> <span data-ttu-id="bf8e7-120">El uso de este widget puede ser más cómodo para los usuarios que escribir manualmente una fecha.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-120">Using this widget can be more convenient for users than manually typing a date.</span></span> <span data-ttu-id="bf8e7-121">La integración del widget DatePicker en una página que usa el enlace de modelos para las operaciones de datos requiere solo una pequeña cantidad de trabajo adicional.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-121">Integrating the Datepicker widget into a page that uses model binding for data operations requires only a small amount of additional work.</span></span>

## <a name="add-a-new-property-to-the-model"></a><span data-ttu-id="bf8e7-122">Agregar una nueva propiedad al modelo</span><span class="sxs-lookup"><span data-stu-id="bf8e7-122">Add a new property to the model</span></span>

<span data-ttu-id="bf8e7-123">En primer lugar, agregará una propiedad **DateTime** a su modelo Student y migrará ese cambio a la base de datos.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-123">First, you will add a **Datetime** property to your Student model and migrate that change to the database.</span></span> <span data-ttu-id="bf8e7-124">Abra **UniversityModels.CS**y agregue el código resaltado al modelo Student.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-124">Open **UniversityModels.cs**, and add the highlighted code to the Student model.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

<span data-ttu-id="bf8e7-125">**RangeAttribute** se incluye para aplicar reglas de validación para la propiedad.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-125">The **RangeAttribute** is included to enforce validation rules for the property.</span></span> <span data-ttu-id="bf8e7-126">En este tutorial, supondremos que contoso University se fundó el 1 de enero de 2013 y, por lo tanto, las fechas de inscripción anteriores no son válidas.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-126">For this tutorial, we will assume that Contoso University was founded on January 1st, 2013 and therefore earlier enrollment dates are not valid.</span></span>

<span data-ttu-id="bf8e7-127">En la ventana de Administración de paquetes, agregue una migración ejecutando el comando **Add-Migration AddEnrollmentDate**.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-127">In the Package Management window, add a migration by running the command **add-migration AddEnrollmentDate**.</span></span> <span data-ttu-id="bf8e7-128">Tenga en cuenta que el código de migración agrega la nueva columna de fecha y hora a la tabla Student.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-128">Notice that the migration code adds the new Datetime column to the Student table.</span></span> <span data-ttu-id="bf8e7-129">Para que coincida con el valor especificado en RangeAttribute, agregue un valor predeterminado para la nueva columna, tal como se muestra en el código resaltado a continuación.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-129">To match the value you specified in the RangeAttribute, add a default value for the new column, as shown in the highlighted code below.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

<span data-ttu-id="bf8e7-130">Guarde el cambio en el archivo de migración.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-130">Save your change to the migration file.</span></span>

<span data-ttu-id="bf8e7-131">No es necesario volver a inicializar los datos.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-131">You do not need to seed the data again.</span></span> <span data-ttu-id="bf8e7-132">Por lo tanto, Abra **Configuration.CS** en la carpeta Migrations y quite o comente el código en el método de **inicialización** .</span><span class="sxs-lookup"><span data-stu-id="bf8e7-132">Therefore, open **Configuration.cs** in the Migrations folder and remove or comment out the code in the **Seed** method.</span></span> <span data-ttu-id="bf8e7-133">Guarde y cierre el archivo.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-133">Save and close the file.</span></span>

<span data-ttu-id="bf8e7-134">Ahora, ejecute el comando **Update-Database**.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-134">Now, run the command **update-database**.</span></span> <span data-ttu-id="bf8e7-135">Observe que la columna existe ahora en la base de datos y todos los registros existentes tienen el valor predeterminado para EnrollmentDate.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-135">Notice that the column now exists in the database and all of the existing records have the default value for EnrollmentDate.</span></span>

## <a name="add-dynamic-controls-for-enrollment-date"></a><span data-ttu-id="bf8e7-136">Agregar controles dinámicos para la fecha de inscripción</span><span class="sxs-lookup"><span data-stu-id="bf8e7-136">Add dynamic controls for enrollment date</span></span>

<span data-ttu-id="bf8e7-137">Ahora agregará controles para mostrar y editar la fecha de inscripción.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-137">You will now add controls for displaying and editing the enrollment date.</span></span> <span data-ttu-id="bf8e7-138">En este punto, el valor se edita a través de un cuadro de texto.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-138">At this point, the value is edited through a text box.</span></span> <span data-ttu-id="bf8e7-139">Más adelante en el tutorial, cambiará el cuadro de texto al widget DatePicker de JQuery.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-139">Later in the tutorial, you will change the text box to the JQuery Datepicker widget.</span></span>

<span data-ttu-id="bf8e7-140">En primer lugar, es importante tener en cuenta que no es necesario realizar ningún cambio en el archivo **AddStudent. aspx** .</span><span class="sxs-lookup"><span data-stu-id="bf8e7-140">First, it is important to note that you do not need to make any change to the **AddStudent.aspx** file.</span></span> <span data-ttu-id="bf8e7-141">El control DynamicEntity representará automáticamente la nueva propiedad.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-141">The DynamicEntity control will automatically render the new property.</span></span>

<span data-ttu-id="bf8e7-142">Abra **Students. aspx**y agregue el siguiente código resaltado.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-142">Open **Students.aspx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

<span data-ttu-id="bf8e7-143">Ejecute la aplicación y observe que puede establecer el valor de la fecha de inscripción escribiendo una fecha.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-143">Run the application and notice that you can set the value of the enrollment date by typing a date.</span></span> <span data-ttu-id="bf8e7-144">Al agregar un estudiante nuevo:</span><span class="sxs-lookup"><span data-stu-id="bf8e7-144">When adding a new student:</span></span>

![establecer fecha](integrating-jquery-ui/_static/image1.png)

<span data-ttu-id="bf8e7-146">O bien, editando un valor existente:</span><span class="sxs-lookup"><span data-stu-id="bf8e7-146">Or, editing an existing value:</span></span>

![Editar fecha](integrating-jquery-ui/_static/image2.png)

<span data-ttu-id="bf8e7-148">Escribir la fecha funciona, pero puede que no sea la experiencia del cliente que desea proporcionar.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-148">Typing the date works, but it might not be the customer experience you wish to provide.</span></span> <span data-ttu-id="bf8e7-149">En la siguiente sección, habilitará la selección de una fecha a través de un calendario.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-149">In the next section, you will enable selecting a date through a calendar.</span></span>

## <a name="install-nuget-package-to-work-with-jquery-ui"></a><span data-ttu-id="bf8e7-150">Instalación de un paquete NuGet para trabajar con JQuery UI</span><span class="sxs-lookup"><span data-stu-id="bf8e7-150">Install NuGet package to work with JQuery UI</span></span>

<span data-ttu-id="bf8e7-151">El paquete NuGet de la **interfaz de usuario de zumo** permite integrar fácilmente los widgets de la interfaz de usuario de jQuery en la aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-151">The **Juice UI** NuGet package enables easy integration of the JQuery UI widgets into your web application.</span></span> <span data-ttu-id="bf8e7-152">Para usar este paquete, instálelo a través de NuGet.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-152">To use this package, install it through NuGet.</span></span>

![agregar la interfaz de usuario de zumo](integrating-jquery-ui/_static/image3.png)

<span data-ttu-id="bf8e7-154">La versión de la interfaz de usuario de zumo que instale puede entrar en conflicto con la versión de JQuery en la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-154">The version of Juice UI that you install may conflict with the version of JQuery in your application.</span></span> <span data-ttu-id="bf8e7-155">Antes de continuar con este tutorial, intente ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-155">Before proceeding with this tutorial, try running your application.</span></span> <span data-ttu-id="bf8e7-156">Si encuentra un error de JavaScript, debe conciliar la versión de JQuery.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-156">If you encounter a JavaScript error, you need to reconcile the JQuery version.</span></span> <span data-ttu-id="bf8e7-157">Puede Agregar la versión esperada de JQuery a la carpeta scripts (versión 1.8.2 en el momento de escribir este tutorial) o en site. Master especifique la ruta de acceso al archivo JQuery.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-157">You can either add the expected version of JQuery to your Scripts folder (version 1.8.2 at time of writing this tutorial), or in Site.master specify the path to the JQuery file.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a><span data-ttu-id="bf8e7-158">Personalización de la plantilla de fecha y hora para incluir el widget DatePicker</span><span class="sxs-lookup"><span data-stu-id="bf8e7-158">Customize DateTime template to include Datepicker widget</span></span>

<span data-ttu-id="bf8e7-159">Agregará el widget DatePicker a la plantilla de datos dinámicos para editar un valor DateTime.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-159">You will add the Datepicker widget to the dynamic data template for editing a datetime value.</span></span> <span data-ttu-id="bf8e7-160">Al agregar el widget a la plantilla, se representa automáticamente en el formulario para agregar un nuevo estudiante y en la vista de cuadrícula para editar alumnos.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-160">By adding the widget to the template, it is automatically rendered in both the form for adding a new student and in the grid view for editing students.</span></span> <span data-ttu-id="bf8e7-161">Abra **DateTime\_Edit. ascx**y agregue el siguiente código resaltado.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-161">Open **DateTime\_Edit.ascx**, and add the following highlighted code.</span></span>

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

<span data-ttu-id="bf8e7-162">En el archivo de código subyacente, se establecerán las fechas mínima y máxima del DatePicker.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-162">In the code-behind file, you will set the minimum and maximum dates for the DatePicker.</span></span> <span data-ttu-id="bf8e7-163">Al establecer estos valores, impedirá que los usuarios naveguen a fechas no válidas.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-163">By setting these values, you will prevent users from navigating to invalid dates.</span></span> <span data-ttu-id="bf8e7-164">Recuperará los valores mínimo y máximo de **RangeAttribute** en la propiedad DateTime, si se proporciona uno.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-164">You will retrieve the minimum and maximum values from the **RangeAttribute** on the DateTime property, if one is provided.</span></span> <span data-ttu-id="bf8e7-165">Abra **DateTime\_Edit.ascx.CS**y agregue el siguiente código resaltado a la página\_método Load.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-165">Open **DateTime\_Edit.ascx.cs**, and add the following highlighted code to the Page\_Load method.</span></span>

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

<span data-ttu-id="bf8e7-166">Ejecute la aplicación web y vaya a la página AddStudent.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-166">Run the web application and navigate to the AddStudent page.</span></span> <span data-ttu-id="bf8e7-167">Proporcione valores para los campos y tenga en cuenta que al hacer clic en el cuadro de texto para la fecha de inscripción, se muestra el calendario.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-167">Provide values for the fields and notice that when you click on the text box for Enrollment Date, the calendar is displayed.</span></span>

![Selector de fecha](integrating-jquery-ui/_static/image4.png)

<span data-ttu-id="bf8e7-169">Seleccione una fecha y haga clic en **Insertar**.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-169">Pick a date, and click **Insert**.</span></span> <span data-ttu-id="bf8e7-170">RangeAttribute aplica la validación en el servidor.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-170">The RangeAttribute enforces validation on the server.</span></span> <span data-ttu-id="bf8e7-171">Al establecer la propiedad minDate en DatePicker, también se aplica la validación en el cliente.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-171">By setting the minDate property on the Datepicker, you also apply validation on the client.</span></span> <span data-ttu-id="bf8e7-172">El calendario no permite al usuario desplazarse a una fecha anterior al valor de minDate.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-172">The calendar does not let the user navigate to a date prior to the value of minDate.</span></span>

<span data-ttu-id="bf8e7-173">Al editar un registro en la vista de cuadrícula, también se muestra el calendario.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-173">When you edit a record in the grid view, the calendar is also displayed.</span></span>

![DatePicker en GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a><span data-ttu-id="bf8e7-175">Conclusión</span><span class="sxs-lookup"><span data-stu-id="bf8e7-175">Conclusion</span></span>

<span data-ttu-id="bf8e7-176">En este tutorial, aprendió a incorporar un widget de JQuery a un formulario web que usa el enlace de modelos.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-176">In this tutorial, you learned how to incorporate a JQuery widget into a web form that uses model binding.</span></span>

<span data-ttu-id="bf8e7-177">En el siguiente [tutorial](using-query-string-values-to-retrieve-data.md), usará un valor de cadena de consulta al seleccionar los datos.</span><span class="sxs-lookup"><span data-stu-id="bf8e7-177">In the next [tutorial](using-query-string-values-to-retrieve-data.md), you will use a query string value when selecting data.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bf8e7-178">[Anterior](sorting-paging-and-filtering-data.md)
> [Siguiente](using-query-string-values-to-retrieve-data.md)</span><span class="sxs-lookup"><span data-stu-id="bf8e7-178">[Previous](sorting-paging-and-filtering-data.md)
[Next](using-query-string-values-to-retrieve-data.md)</span></span>
