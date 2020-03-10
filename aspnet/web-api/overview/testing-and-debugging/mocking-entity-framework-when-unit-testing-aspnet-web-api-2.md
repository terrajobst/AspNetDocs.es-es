---
uid: web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
title: Entity Framework ficticios cuando las pruebas unitarias ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: Esta guía y la aplicación demuestran cómo crear pruebas unitarias para la aplicación web API 2 que usa el Entity Framework. Muestra cómo modificar...
ms.author: riande
ms.date: 12/13/2013
ms.assetid: cd844025-ccad-41ce-8694-595f1022a49f
msc.legacyurl: /web-api/overview/testing-and-debugging/mocking-entity-framework-when-unit-testing-aspnet-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 258450107ee7443c4efd43a3b8e4851249745227
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447091"
---
# <a name="mocking-entity-framework-when-unit-testing-aspnet-web-api-2"></a><span data-ttu-id="b030e-104">Entity Framework ficticios cuando las pruebas unitarias ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="b030e-104">Mocking Entity Framework when Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="b030e-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="b030e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="b030e-106">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="b030e-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="b030e-107">Esta guía y la aplicación demuestran cómo crear pruebas unitarias para la aplicación web API 2 que usa el Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b030e-107">This guidance and application demonstrate how to create unit tests for your Web API 2 application that uses the Entity Framework.</span></span> <span data-ttu-id="b030e-108">Muestra cómo modificar el controlador scaffolding para habilitar el paso de un objeto de contexto para las pruebas y cómo crear objetos de prueba que funcionen con Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b030e-108">It shows how to modify the scaffolded controller to enable passing a context object for testing, and how to create test objects that work with Entity Framework.</span></span>
>
> <span data-ttu-id="b030e-109">Para ver una introducción a las pruebas unitarias con ASP.NET Web API, consulte [pruebas unitarias con ASP.net web API 2](unit-testing-with-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b030e-109">For an introduction to unit testing with ASP.NET Web API, see [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md).</span></span>
>
> <span data-ttu-id="b030e-110">En este tutorial se da por supuesto que está familiarizado con los conceptos básicos de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="b030e-110">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="b030e-111">Para ver un tutorial introductorio, consulte [Introducción con ASP.net web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="b030e-111">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="b030e-112">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="b030e-112">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="b030e-113">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="b030e-113">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="b030e-114">API Web 2</span><span class="sxs-lookup"><span data-stu-id="b030e-114">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="b030e-115">En este tema</span><span class="sxs-lookup"><span data-stu-id="b030e-115">In this topic</span></span>

<span data-ttu-id="b030e-116">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="b030e-116">This topic contains the following sections:</span></span>

- [<span data-ttu-id="b030e-117">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b030e-117">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="b030e-118">Descargar código</span><span class="sxs-lookup"><span data-stu-id="b030e-118">Download code</span></span>](#download)
- [<span data-ttu-id="b030e-119">Crear aplicación con proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="b030e-119">Create application with unit test project</span></span>](#appwithunittest)
- [<span data-ttu-id="b030e-120">Crear la clase de modelo</span><span class="sxs-lookup"><span data-stu-id="b030e-120">Create the model class</span></span>](#modelclass)
- [<span data-ttu-id="b030e-121">Agregar el controlador</span><span class="sxs-lookup"><span data-stu-id="b030e-121">Add the controller</span></span>](#controller)
- [<span data-ttu-id="b030e-122">Agregar inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="b030e-122">Add dependency injection</span></span>](#dependency)
- [<span data-ttu-id="b030e-123">Instalación de paquetes NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="b030e-123">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="b030e-124">Crear contexto de prueba</span><span class="sxs-lookup"><span data-stu-id="b030e-124">Create test context</span></span>](#testcontext)
- [<span data-ttu-id="b030e-125">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="b030e-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="b030e-126">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="b030e-126">Run tests</span></span>](#runtests)

<span data-ttu-id="b030e-127">Si ya ha completado los pasos de las [pruebas unitarias con ASP.net web API 2](unit-testing-with-aspnet-web-api.md), puede ir directamente a la sección [Agregar el controlador](#controller).</span><span class="sxs-lookup"><span data-stu-id="b030e-127">If you have already completed the steps in [Unit Testing with ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md), you can skip to the section [Add the controller](#controller).</span></span>

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="b030e-128">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="b030e-128">Prerequisites</span></span>

<span data-ttu-id="b030e-129">Visual Studio 2017 Community, Professional o Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="b030e-129">Visual Studio 2017 Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="b030e-130">Descarga de código</span><span class="sxs-lookup"><span data-stu-id="b030e-130">Download code</span></span>

<span data-ttu-id="b030e-131">Descargue el [proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="b030e-131">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="b030e-132">El proyecto descargable incluye el código de prueba unitaria de este tema y el tema [ASP.net web API 2 de las pruebas unitarias](unit-testing-with-aspnet-web-api.md) .</span><span class="sxs-lookup"><span data-stu-id="b030e-132">The downloadable project includes unit test code for this topic and for the [Unit Testing ASP.NET Web API 2](unit-testing-with-aspnet-web-api.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="b030e-133">Crear aplicación con proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="b030e-133">Create application with unit test project</span></span>

<span data-ttu-id="b030e-134">Puede crear un proyecto de prueba unitaria al crear la aplicación o agregar un proyecto de prueba unitaria a una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="b030e-134">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="b030e-135">En este tutorial se muestra cómo crear un proyecto de prueba unitaria al crear la aplicación.</span><span class="sxs-lookup"><span data-stu-id="b030e-135">This tutorial shows creating a unit test project when creating the application.</span></span>

<span data-ttu-id="b030e-136">Cree una nueva aplicación Web de ASP.NET denominada **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="b030e-136">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

<span data-ttu-id="b030e-137">En las nuevas ventanas del proyecto ASP.NET, seleccione la plantilla **vacía** y agregue las carpetas y las referencias básicas para la API Web.</span><span class="sxs-lookup"><span data-stu-id="b030e-137">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="b030e-138">Seleccione la opción **Agregar pruebas unitarias** .</span><span class="sxs-lookup"><span data-stu-id="b030e-138">Select the **Add unit tests** option.</span></span> <span data-ttu-id="b030e-139">El proyecto de prueba unitaria se denomina automáticamente **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="b030e-139">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="b030e-140">Puede mantener este nombre.</span><span class="sxs-lookup"><span data-stu-id="b030e-140">You can keep this name.</span></span>

![crear proyecto de prueba unitaria](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image1.png)

<span data-ttu-id="b030e-142">Después de crear la aplicación, verá que contiene dos proyectos: **StoreApp** y **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="b030e-142">After creating the application, you will see it contains two projects - **StoreApp** and **StoreApp.Tests**.</span></span>

<a id="modelclass"></a>
## <a name="create-the-model-class"></a><span data-ttu-id="b030e-143">Crear la clase de modelo</span><span class="sxs-lookup"><span data-stu-id="b030e-143">Create the model class</span></span>

<span data-ttu-id="b030e-144">En el proyecto StoreApp, agregue un archivo de clase a la carpeta **Models** denominada **product.CS**.</span><span class="sxs-lookup"><span data-stu-id="b030e-144">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="b030e-145">Reemplace el contenido del archivo por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="b030e-145">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample1.cs)]

<span data-ttu-id="b030e-146">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="b030e-146">Build the solution.</span></span>

<a id="controller"></a>
## <a name="add-the-controller"></a><span data-ttu-id="b030e-147">Agregar el controlador</span><span class="sxs-lookup"><span data-stu-id="b030e-147">Add the controller</span></span>

<span data-ttu-id="b030e-148">Haga clic con el botón secundario en la carpeta Controllers y seleccione **Agregar** y **nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="b030e-148">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="b030e-149">Seleccione controlador de Web API 2 con acciones mediante Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="b030e-149">Select Web API 2 Controller with actions, using Entity Framework.</span></span>

![Agregar nuevo controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image2.png)

<span data-ttu-id="b030e-151">Establezca los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="b030e-151">Set the following values:</span></span>

- <span data-ttu-id="b030e-152">Nombre del controlador: **ProductController**</span><span class="sxs-lookup"><span data-stu-id="b030e-152">Controller name: **ProductController**</span></span>
- <span data-ttu-id="b030e-153">Clase de modelo: **producto**</span><span class="sxs-lookup"><span data-stu-id="b030e-153">Model class: **Product**</span></span>
- <span data-ttu-id="b030e-154">Clase de contexto de datos: [Seleccione el botón **nuevo contexto de datos** que rellenará los valores que se muestran a continuación]</span><span class="sxs-lookup"><span data-stu-id="b030e-154">Data context class: [Select **New data context** button which fills in the values seen below]</span></span>

![especificar controlador](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image3.png)

<span data-ttu-id="b030e-156">Haga clic en **Agregar** para crear el controlador con código generado automáticamente.</span><span class="sxs-lookup"><span data-stu-id="b030e-156">Click **Add** to create the controller with automatically-generated code.</span></span> <span data-ttu-id="b030e-157">El código incluye métodos para crear, recuperar, actualizar y eliminar instancias de la clase product.</span><span class="sxs-lookup"><span data-stu-id="b030e-157">The code includes methods for creating, retrieving, updating and deleting instances of the Product class.</span></span> <span data-ttu-id="b030e-158">En el código siguiente se muestra el método para agregar un producto.</span><span class="sxs-lookup"><span data-stu-id="b030e-158">The following code shows the method for add a Product.</span></span> <span data-ttu-id="b030e-159">Observe que el método devuelve una instancia de **IHttpActionResult**.</span><span class="sxs-lookup"><span data-stu-id="b030e-159">Notice that the method returns an instance of **IHttpActionResult**.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample2.cs)]

<span data-ttu-id="b030e-160">IHttpActionResult es una de las nuevas características de Web API 2 y simplifica el desarrollo de pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="b030e-160">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span>

<span data-ttu-id="b030e-161">En la siguiente sección, personalizará el código generado para facilitar el paso de objetos de prueba al controlador.</span><span class="sxs-lookup"><span data-stu-id="b030e-161">In the next section, you will customize the generated code to facilitate passing test objects to the controller.</span></span>

<a id="dependency"></a>
## <a name="add-dependency-injection"></a><span data-ttu-id="b030e-162">Agregar inyección de dependencia</span><span class="sxs-lookup"><span data-stu-id="b030e-162">Add dependency injection</span></span>

<span data-ttu-id="b030e-163">Actualmente, la clase ProductController está codificada de forma rígida para usar una instancia de la clase StoreAppContext.</span><span class="sxs-lookup"><span data-stu-id="b030e-163">Currently, the ProductController class is hard-coded to use an instance of the StoreAppContext class.</span></span> <span data-ttu-id="b030e-164">Usará un patrón denominado inyección de dependencia para modificar la aplicación y quitar esa dependencia codificada de forma rígida.</span><span class="sxs-lookup"><span data-stu-id="b030e-164">You will use a pattern called dependency injection to modify your application and remove that hard-coded dependency.</span></span> <span data-ttu-id="b030e-165">Al interrumpir esta dependencia, puede pasar un objeto ficticio al realizar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="b030e-165">By breaking this dependency, you can pass in a mock object when testing.</span></span>

<span data-ttu-id="b030e-166">Haga clic con el botón secundario en la carpeta **modelos** y agregue una nueva interfaz denominada **IStoreAppContext**.</span><span class="sxs-lookup"><span data-stu-id="b030e-166">Right-click the **Models** folder, and add a new interface named **IStoreAppContext**.</span></span>

<span data-ttu-id="b030e-167">Reemplace el código por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="b030e-167">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample3.cs)]

<span data-ttu-id="b030e-168">Abra el archivo StoreAppContext.cs y realice los siguientes cambios resaltados.</span><span class="sxs-lookup"><span data-stu-id="b030e-168">Open the StoreAppContext.cs file and make the following highlighted changes.</span></span> <span data-ttu-id="b030e-169">Los cambios importantes que se deben tener en cuenta son:</span><span class="sxs-lookup"><span data-stu-id="b030e-169">The important changes to note are:</span></span>

- <span data-ttu-id="b030e-170">La clase StoreAppContext implementa la interfaz IStoreAppContext</span><span class="sxs-lookup"><span data-stu-id="b030e-170">StoreAppContext class implements IStoreAppContext interface</span></span>
- <span data-ttu-id="b030e-171">El método MarkAsModified está implementado</span><span class="sxs-lookup"><span data-stu-id="b030e-171">MarkAsModified method is implemented</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample4.cs?highlight=6,14-17)]

<span data-ttu-id="b030e-172">Abra el archivo ProductController.cs.</span><span class="sxs-lookup"><span data-stu-id="b030e-172">Open the ProductController.cs file.</span></span> <span data-ttu-id="b030e-173">Cambie el código existente para que coincida con el código resaltado.</span><span class="sxs-lookup"><span data-stu-id="b030e-173">Change the existing code to match the highlighted code.</span></span> <span data-ttu-id="b030e-174">Estos cambios interrumpen la dependencia de StoreAppContext y permiten que otras clases pasen un objeto diferente para la clase de contexto.</span><span class="sxs-lookup"><span data-stu-id="b030e-174">These changes break the dependency on StoreAppContext and enable other classes to pass in a different object for the context class.</span></span> <span data-ttu-id="b030e-175">Este cambio le permitirá pasar un contexto de prueba durante las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="b030e-175">This change will enable you to pass in a test context during unit tests.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample5.cs?highlight=4,7-12)]

<span data-ttu-id="b030e-176">Hay un cambio más que debe hacer en ProductController.</span><span class="sxs-lookup"><span data-stu-id="b030e-176">There is one more change you must make in ProductController.</span></span> <span data-ttu-id="b030e-177">En el método **PutProduct** , reemplace la línea que establece el estado de la entidad a Modified con una llamada al método MarkAsModified.</span><span class="sxs-lookup"><span data-stu-id="b030e-177">In the **PutProduct** method, replace the line that sets the entity state to modified with a call to the MarkAsModified method.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample6.cs?highlight=14-15)]

<span data-ttu-id="b030e-178">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="b030e-178">Build the solution.</span></span>

<span data-ttu-id="b030e-179">Ahora está listo para configurar el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="b030e-179">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="b030e-180">Instalación de paquetes NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="b030e-180">Install NuGet packages in test project</span></span>

<span data-ttu-id="b030e-181">Cuando se usa la plantilla vacía para crear una aplicación, el proyecto de prueba unitaria (StoreApp. tests) no incluye ningún paquete NuGet instalado.</span><span class="sxs-lookup"><span data-stu-id="b030e-181">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="b030e-182">Otras plantillas, como la plantilla de API Web, incluyen algunos paquetes NuGet en el proyecto de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="b030e-182">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="b030e-183">En este tutorial, debe incluir el paquete de Entity Framework y el paquete de Microsoft ASP.NET Web API 2 Core en el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="b030e-183">For this tutorial, you must include the Entity Framework package and the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="b030e-184">Haga clic con el botón derecho en el proyecto StoreApp. tests y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="b030e-184">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="b030e-185">Debe seleccionar el proyecto StoreApp. tests para agregar los paquetes a ese proyecto.</span><span class="sxs-lookup"><span data-stu-id="b030e-185">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![administrar paquetes](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image4.png)

<span data-ttu-id="b030e-187">En los paquetes en línea, busque e instale el paquete EntityFramework (versión 6,0 o posterior).</span><span class="sxs-lookup"><span data-stu-id="b030e-187">From the Online packages, find and install the EntityFramework package (version 6.0 or later).</span></span> <span data-ttu-id="b030e-188">Si parece que el paquete EntityFramework ya está instalado, es posible que haya seleccionado el proyecto StoreApp en lugar del proyecto StoreApp. tests.</span><span class="sxs-lookup"><span data-stu-id="b030e-188">If it appears that the EntityFramework package is already installed, you may have selected the StoreApp project instead of the StoreApp.Tests project.</span></span>

![Agregar Entity Framework](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image5.png)

<span data-ttu-id="b030e-190">Busque e instale Microsoft ASP.NET paquete principal de Web API 2.</span><span class="sxs-lookup"><span data-stu-id="b030e-190">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![instalación del paquete principal de Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image6.png)

<span data-ttu-id="b030e-192">Cierre la ventana administrar paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="b030e-192">Close the Manage NuGet Packages window.</span></span>

<a id="testcontext"></a>
## <a name="create-test-context"></a><span data-ttu-id="b030e-193">Crear contexto de prueba</span><span class="sxs-lookup"><span data-stu-id="b030e-193">Create test context</span></span>

<span data-ttu-id="b030e-194">Agregue una clase denominada **TestDbSet** al proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="b030e-194">Add a class named **TestDbSet** to the test project.</span></span> <span data-ttu-id="b030e-195">Esta clase actúa como la clase base para el conjunto de datos de prueba.</span><span class="sxs-lookup"><span data-stu-id="b030e-195">This class serves as the base class for your test data set.</span></span> <span data-ttu-id="b030e-196">Reemplace el código por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="b030e-196">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample7.cs)]

<span data-ttu-id="b030e-197">Agregue una clase denominada **TestProductDbSet** al proyecto de prueba que contiene el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="b030e-197">Add a class named **TestProductDbSet** to the test project which contains the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample8.cs)]

<span data-ttu-id="b030e-198">Agregue una clase denominada **TestStoreAppContext** y reemplace el código existente por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="b030e-198">Add a class named **TestStoreAppContext** and replace the existing code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample9.cs)]

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="b030e-199">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="b030e-199">Create tests</span></span>

<span data-ttu-id="b030e-200">De forma predeterminada, el proyecto de prueba incluye un archivo de prueba vacío denominado **UnitTest1.CS**.</span><span class="sxs-lookup"><span data-stu-id="b030e-200">By default, your test project includes an empty test file named **UnitTest1.cs**.</span></span> <span data-ttu-id="b030e-201">Este archivo muestra los atributos que se usan para crear métodos de prueba.</span><span class="sxs-lookup"><span data-stu-id="b030e-201">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="b030e-202">En este tutorial, puede eliminar este archivo porque agregará una nueva clase de prueba.</span><span class="sxs-lookup"><span data-stu-id="b030e-202">For this tutorial, you can delete this file because you will add a new test class.</span></span>

<span data-ttu-id="b030e-203">Agregue una clase denominada **TestProductController** al proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="b030e-203">Add a class named **TestProductController** to the test project.</span></span> <span data-ttu-id="b030e-204">Reemplace el código por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="b030e-204">Replace the code with the following code.</span></span>

[!code-csharp[Main](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/samples/sample10.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="b030e-205">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="b030e-205">Run tests</span></span>

<span data-ttu-id="b030e-206">Ahora está listo para ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="b030e-206">You are now ready to run the tests.</span></span> <span data-ttu-id="b030e-207">Se probará todo el método marcado con el atributo **TestMethod** .</span><span class="sxs-lookup"><span data-stu-id="b030e-207">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="b030e-208">En el elemento de menú **prueba** , ejecute las pruebas.</span><span class="sxs-lookup"><span data-stu-id="b030e-208">From the **Test** menu item, run the tests.</span></span>

![ejecución de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image7.png)

<span data-ttu-id="b030e-210">Abra la ventana **Explorador de pruebas** y observe los resultados de las pruebas.</span><span class="sxs-lookup"><span data-stu-id="b030e-210">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![resultados de pruebas](mocking-entity-framework-when-unit-testing-aspnet-web-api-2/_static/image8.png)
