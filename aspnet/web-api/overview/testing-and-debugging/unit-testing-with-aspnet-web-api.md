---
uid: web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
title: Pruebas unitarias ASP.NET Web API 2 | Microsoft Docs
author: Rick-Anderson
description: En esta guía y en la aplicación se muestra cómo crear pruebas unitarias simples para la aplicación web API 2. En este tutorial se muestra cómo incluir un proj de prueba unitaria...
ms.author: riande
ms.date: 06/05/2014
ms.assetid: bf20f78d-ff91-48be-abd1-88e23dcc70ba
msc.legacyurl: /web-api/overview/testing-and-debugging/unit-testing-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: f2d60b977475e048a3a74aabff4adc768ee22baf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446989"
---
# <a name="unit-testing-aspnet-web-api-2"></a><span data-ttu-id="4abd3-104">Pruebas unitarias ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="4abd3-104">Unit Testing ASP.NET Web API 2</span></span>

<span data-ttu-id="4abd3-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="4abd3-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[<span data-ttu-id="4abd3-106">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="4abd3-106">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11)

> <span data-ttu-id="4abd3-107">En esta guía y en la aplicación se muestra cómo crear pruebas unitarias simples para la aplicación web API 2.</span><span class="sxs-lookup"><span data-stu-id="4abd3-107">This guidance and application demonstrate how to create simple unit tests for your Web API 2 application.</span></span> <span data-ttu-id="4abd3-108">En este tutorial se muestra cómo incluir un proyecto de prueba unitaria en la solución y cómo escribir métodos de prueba que comprueben los valores devueltos de un método de controlador.</span><span class="sxs-lookup"><span data-stu-id="4abd3-108">This tutorial shows how to include a unit test project in your solution, and write test methods that check the returned values from a controller method.</span></span>
>
> <span data-ttu-id="4abd3-109">En este tutorial se da por supuesto que está familiarizado con los conceptos básicos de ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="4abd3-109">This tutorial assumes you are familiar with the basic concepts of ASP.NET Web API.</span></span> <span data-ttu-id="4abd3-110">Para ver un tutorial introductorio, consulte [Introducción con ASP.net web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="4abd3-110">For an introductory tutorial, see [Getting Started with ASP.NET Web API 2](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).</span></span>
>
> <span data-ttu-id="4abd3-111">Las pruebas unitarias de este tema se limitan intencionalmente a escenarios de datos simples.</span><span class="sxs-lookup"><span data-stu-id="4abd3-111">The unit tests in this topic are intentionally limited to simple data scenarios.</span></span> <span data-ttu-id="4abd3-112">Para realizar pruebas unitarias de escenarios de datos más avanzados, consulte [Entity Framework ficticios cuando las pruebas unitarias ASP.net web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="4abd3-112">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="4abd3-113">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="4abd3-113">Software versions used in the tutorial</span></span>
>
> - [<span data-ttu-id="4abd3-114">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="4abd3-114">Visual Studio 2017</span></span>](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - <span data-ttu-id="4abd3-115">API Web 2</span><span class="sxs-lookup"><span data-stu-id="4abd3-115">Web API 2</span></span>

## <a name="in-this-topic"></a><span data-ttu-id="4abd3-116">En este tema</span><span class="sxs-lookup"><span data-stu-id="4abd3-116">In this topic</span></span>

<span data-ttu-id="4abd3-117">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="4abd3-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="4abd3-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4abd3-118">Prerequisites</span></span>](#prereqs)
- [<span data-ttu-id="4abd3-119">Descargar código</span><span class="sxs-lookup"><span data-stu-id="4abd3-119">Download code</span></span>](#download)
- [<span data-ttu-id="4abd3-120">Crear aplicación con proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="4abd3-120">Create application with unit test project</span></span>](#appwithunittest)
    - [<span data-ttu-id="4abd3-121">Agregar proyecto de prueba unitaria al crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="4abd3-121">Add unit test project when creating the application</span></span>](#whencreate)
    - [<span data-ttu-id="4abd3-122">Agregar un proyecto de prueba unitaria a una aplicación existente</span><span class="sxs-lookup"><span data-stu-id="4abd3-122">Add unit test project to an existing application</span></span>](#addtoexisting)
- [<span data-ttu-id="4abd3-123">Configuración de la aplicación web API 2</span><span class="sxs-lookup"><span data-stu-id="4abd3-123">Set up the Web API 2 application</span></span>](#setupproject)
- [<span data-ttu-id="4abd3-124">Instalación de paquetes NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="4abd3-124">Install NuGet packages in test project</span></span>](#testpackages)
- [<span data-ttu-id="4abd3-125">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="4abd3-125">Create tests</span></span>](#tests)
- [<span data-ttu-id="4abd3-126">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="4abd3-126">Run tests</span></span>](#runtests)

<a id="prereqs"></a>
## <a name="prerequisites"></a><span data-ttu-id="4abd3-127">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="4abd3-127">Prerequisites</span></span>

<span data-ttu-id="4abd3-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional o Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="4abd3-128">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional or Enterprise edition</span></span>

<a id="download"></a>
## <a name="download-code"></a><span data-ttu-id="4abd3-129">Descarga de código</span><span class="sxs-lookup"><span data-stu-id="4abd3-129">Download code</span></span>

<span data-ttu-id="4abd3-130">Descargue el [proyecto completado](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span><span class="sxs-lookup"><span data-stu-id="4abd3-130">Download the [completed project](https://code.msdn.microsoft.com/Unit-Testing-with-ASPNET-1374bc11).</span></span> <span data-ttu-id="4abd3-131">El proyecto descargable incluye el código de prueba unitaria de este tema y el [Entity Framework ficticio cuando se ASP.net web API tema de pruebas unitarias](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) .</span><span class="sxs-lookup"><span data-stu-id="4abd3-131">The downloadable project includes unit test code for this topic and for the [Mocking Entity Framework when Unit Testing ASP.NET Web API](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md) topic.</span></span>

<a id="appwithunittest"></a>
## <a name="create-application-with-unit-test-project"></a><span data-ttu-id="4abd3-132">Crear aplicación con proyecto de prueba unitaria</span><span class="sxs-lookup"><span data-stu-id="4abd3-132">Create application with unit test project</span></span>

<span data-ttu-id="4abd3-133">Puede crear un proyecto de prueba unitaria al crear la aplicación o agregar un proyecto de prueba unitaria a una aplicación existente.</span><span class="sxs-lookup"><span data-stu-id="4abd3-133">You can either create a unit test project when creating your application or add a unit test project to an existing application.</span></span> <span data-ttu-id="4abd3-134">En este tutorial se muestran los dos métodos para crear un proyecto de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="4abd3-134">This tutorial shows both methods for creating a unit test project.</span></span> <span data-ttu-id="4abd3-135">Para seguir este tutorial, puede usar cualquiera de estos enfoques.</span><span class="sxs-lookup"><span data-stu-id="4abd3-135">To follow this tutorial, you can use either approach.</span></span>

<a id="whencreate"></a>
### <a name="add-unit-test-project-when-creating-the-application"></a><span data-ttu-id="4abd3-136">Agregar proyecto de prueba unitaria al crear la aplicación</span><span class="sxs-lookup"><span data-stu-id="4abd3-136">Add unit test project when creating the application</span></span>

<span data-ttu-id="4abd3-137">Cree una nueva aplicación Web de ASP.NET denominada **StoreApp**.</span><span class="sxs-lookup"><span data-stu-id="4abd3-137">Create a new ASP.NET Web Application named **StoreApp**.</span></span>

![creación de proyecto](unit-testing-with-aspnet-web-api/_static/image1.png)

<span data-ttu-id="4abd3-139">En las nuevas ventanas del proyecto ASP.NET, seleccione la plantilla **vacía** y agregue las carpetas y las referencias básicas para la API Web.</span><span class="sxs-lookup"><span data-stu-id="4abd3-139">In the New ASP.NET Project windows, select the **Empty** template and add folders and core references for Web API.</span></span> <span data-ttu-id="4abd3-140">Seleccione la opción **Agregar pruebas unitarias** .</span><span class="sxs-lookup"><span data-stu-id="4abd3-140">Select the **Add unit tests** option.</span></span> <span data-ttu-id="4abd3-141">El proyecto de prueba unitaria se denomina automáticamente **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="4abd3-141">The unit test project is automatically named **StoreApp.Tests**.</span></span> <span data-ttu-id="4abd3-142">Puede mantener este nombre.</span><span class="sxs-lookup"><span data-stu-id="4abd3-142">You can keep this name.</span></span>

![crear proyecto de prueba unitaria](unit-testing-with-aspnet-web-api/_static/image2.png)

<span data-ttu-id="4abd3-144">Después de crear la aplicación, verá que contiene dos proyectos.</span><span class="sxs-lookup"><span data-stu-id="4abd3-144">After creating the application, you will see it contains two projects.</span></span>

![dos proyectos](unit-testing-with-aspnet-web-api/_static/image3.png)

<a id="addtoexisting"></a>
### <a name="add-unit-test-project-to-an-existing-application"></a><span data-ttu-id="4abd3-146">Agregar un proyecto de prueba unitaria a una aplicación existente</span><span class="sxs-lookup"><span data-stu-id="4abd3-146">Add unit test project to an existing application</span></span>

<span data-ttu-id="4abd3-147">Si no creó el proyecto de prueba unitaria al crear la aplicación, puede Agregar una en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="4abd3-147">If you did not create the unit test project when you created your application, you can add one at any time.</span></span> <span data-ttu-id="4abd3-148">Por ejemplo, supongamos que ya tiene una aplicación denominada StoreApp y desea agregar pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="4abd3-148">For example, suppose you already have an application named StoreApp, and you want to add unit tests.</span></span> <span data-ttu-id="4abd3-149">Para agregar un proyecto de prueba unitaria, haga clic con el botón derecho en la solución y seleccione **Agregar** y **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="4abd3-149">To add a unit test project, right-click your solution and select **Add** and **New Project**.</span></span>

![Agregar nuevo proyecto a la solución](unit-testing-with-aspnet-web-api/_static/image4.png)

<span data-ttu-id="4abd3-151">Seleccione **prueba** en el panel izquierdo y, a continuación, seleccione **proyecto de prueba unitaria** para el tipo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="4abd3-151">Select **Test** in the left pane, and select **Unit Test Project** for the project type.</span></span> <span data-ttu-id="4abd3-152">Asigne al proyecto el nombre **StoreApp. tests**.</span><span class="sxs-lookup"><span data-stu-id="4abd3-152">Name the project **StoreApp.Tests**.</span></span>

![Agregar proyecto de prueba unitaria](unit-testing-with-aspnet-web-api/_static/image5.png)

<span data-ttu-id="4abd3-154">Verá el proyecto de prueba unitaria en la solución.</span><span class="sxs-lookup"><span data-stu-id="4abd3-154">You will see the unit test project in your solution.</span></span>

<span data-ttu-id="4abd3-155">En el proyecto de prueba unitaria, agregue una referencia de proyecto al proyecto original.</span><span class="sxs-lookup"><span data-stu-id="4abd3-155">In the unit test project, add a project reference to the original project.</span></span>

<a id="setupproject"></a>
## <a name="set-up-the-web-api-2-application"></a><span data-ttu-id="4abd3-156">Configuración de la aplicación web API 2</span><span class="sxs-lookup"><span data-stu-id="4abd3-156">Set up the Web API 2 application</span></span>

<span data-ttu-id="4abd3-157">En el proyecto StoreApp, agregue un archivo de clase a la carpeta **Models** denominada **product.CS**.</span><span class="sxs-lookup"><span data-stu-id="4abd3-157">In your StoreApp project, add a class file to the **Models** folder named **Product.cs**.</span></span> <span data-ttu-id="4abd3-158">Reemplace el contenido del archivo por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="4abd3-158">Replace the contents of the file with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="4abd3-159">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="4abd3-159">Build the solution.</span></span>

<span data-ttu-id="4abd3-160">Haga clic con el botón secundario en la carpeta Controllers y seleccione **Agregar** y **nuevo elemento con scaffolding**.</span><span class="sxs-lookup"><span data-stu-id="4abd3-160">Right-click the Controllers folder and select **Add** and **New Scaffolded Item**.</span></span> <span data-ttu-id="4abd3-161">Seleccione **controlador de Web API 2-vacío**.</span><span class="sxs-lookup"><span data-stu-id="4abd3-161">Select **Web API 2 Controller - Empty**.</span></span>

![Agregar nuevo controlador](unit-testing-with-aspnet-web-api/_static/image6.png)

<span data-ttu-id="4abd3-163">Establezca el nombre del controlador en **SimpleProductController**y haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="4abd3-163">Set the controller name to **SimpleProductController**, and click **Add**.</span></span>

![especificar controlador](unit-testing-with-aspnet-web-api/_static/image7.png)

<span data-ttu-id="4abd3-165">Reemplace el código existente por el siguiente código.</span><span class="sxs-lookup"><span data-stu-id="4abd3-165">Replace the existing code with the following code.</span></span> <span data-ttu-id="4abd3-166">Para simplificar este ejemplo, los datos se almacenan en una lista en lugar de en una base de datos.</span><span class="sxs-lookup"><span data-stu-id="4abd3-166">To simplify this example, the data is stored in a list rather than a database.</span></span> <span data-ttu-id="4abd3-167">La lista definida en esta clase representa los datos de producción.</span><span class="sxs-lookup"><span data-stu-id="4abd3-167">The list defined in this class represents the production data.</span></span> <span data-ttu-id="4abd3-168">Observe que el controlador incluye un constructor que toma como parámetro una lista de objetos product.</span><span class="sxs-lookup"><span data-stu-id="4abd3-168">Notice that the controller includes a constructor that takes as a parameter a list of Product objects.</span></span> <span data-ttu-id="4abd3-169">Este constructor le permite pasar datos de prueba al realizar pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="4abd3-169">This constructor enables you to pass test data when unit testing.</span></span> <span data-ttu-id="4abd3-170">El controlador también incluye dos métodos **asincrónicos** para ilustrar métodos asincrónicos de pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="4abd3-170">The controller also includes two **async** methods to illustrate unit testing asynchronous methods.</span></span> <span data-ttu-id="4abd3-171">Estos métodos asincrónicos se implementan mediante una llamada a **Task. FromResult** para minimizar el código extraño, pero normalmente los métodos incluyen operaciones que consumen muchos recursos.</span><span class="sxs-lookup"><span data-stu-id="4abd3-171">These async methods were implemented by calling **Task.FromResult** to minimize extraneous code, but normally the methods would include resource-intensive operations.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="4abd3-172">El método GetProduct devuelve una instancia de la interfaz **IHttpActionResult** .</span><span class="sxs-lookup"><span data-stu-id="4abd3-172">The GetProduct method returns an instance of the **IHttpActionResult** interface.</span></span> <span data-ttu-id="4abd3-173">IHttpActionResult es una de las nuevas características de Web API 2 y simplifica el desarrollo de pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="4abd3-173">IHttpActionResult is one of the new features in Web API 2, and it simplifies unit test development.</span></span> <span data-ttu-id="4abd3-174">Las clases que implementan la interfaz IHttpActionResult se encuentran en el espacio de nombres [System. Web. http. Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) .</span><span class="sxs-lookup"><span data-stu-id="4abd3-174">Classes that implement the IHttpActionResult interface are found in the [System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx) namespace.</span></span> <span data-ttu-id="4abd3-175">Estas clases representan las posibles respuestas de una solicitud de acción y se corresponden con códigos de Estado HTTP.</span><span class="sxs-lookup"><span data-stu-id="4abd3-175">These classes represent possible responses from an action request, and they correspond to HTTP status codes.</span></span>

<span data-ttu-id="4abd3-176">Compile la solución.</span><span class="sxs-lookup"><span data-stu-id="4abd3-176">Build the solution.</span></span>

<span data-ttu-id="4abd3-177">Ahora está listo para configurar el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="4abd3-177">You are now ready to set up the test project.</span></span>

<a id="testpackages"></a>
## <a name="install-nuget-packages-in-test-project"></a><span data-ttu-id="4abd3-178">Instalación de paquetes NuGet en el proyecto de prueba</span><span class="sxs-lookup"><span data-stu-id="4abd3-178">Install NuGet packages in test project</span></span>

<span data-ttu-id="4abd3-179">Cuando se usa la plantilla vacía para crear una aplicación, el proyecto de prueba unitaria (StoreApp. tests) no incluye ningún paquete NuGet instalado.</span><span class="sxs-lookup"><span data-stu-id="4abd3-179">When you use the Empty template to create an application, the unit test project (StoreApp.Tests) does not include any installed NuGet packages.</span></span> <span data-ttu-id="4abd3-180">Otras plantillas, como la plantilla de API Web, incluyen algunos paquetes NuGet en el proyecto de prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="4abd3-180">Other templates, such as the Web API template, include some NuGet packages in the unit test project.</span></span> <span data-ttu-id="4abd3-181">En este tutorial, debe incluir el paquete de Microsoft ASP.NET Web API 2 Core en el proyecto de prueba.</span><span class="sxs-lookup"><span data-stu-id="4abd3-181">For this tutorial, you must include the Microsoft ASP.NET Web API 2 Core package to the test project.</span></span>

<span data-ttu-id="4abd3-182">Haga clic con el botón derecho en el proyecto StoreApp. tests y seleccione **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="4abd3-182">Right-click the StoreApp.Tests project and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="4abd3-183">Debe seleccionar el proyecto StoreApp. tests para agregar los paquetes a ese proyecto.</span><span class="sxs-lookup"><span data-stu-id="4abd3-183">You must select the StoreApp.Tests project to add the packages to that project.</span></span>

![administrar paquetes](unit-testing-with-aspnet-web-api/_static/image8.png)

<span data-ttu-id="4abd3-185">Busque e instale Microsoft ASP.NET paquete principal de Web API 2.</span><span class="sxs-lookup"><span data-stu-id="4abd3-185">Find and install Microsoft ASP.NET Web API 2 Core package.</span></span>

![instalación del paquete principal de Web API](unit-testing-with-aspnet-web-api/_static/image9.png)

<span data-ttu-id="4abd3-187">Cierre la ventana administrar paquetes NuGet.</span><span class="sxs-lookup"><span data-stu-id="4abd3-187">Close the Manage NuGet Packages window.</span></span>

<a id="tests"></a>
## <a name="create-tests"></a><span data-ttu-id="4abd3-188">Crear pruebas</span><span class="sxs-lookup"><span data-stu-id="4abd3-188">Create tests</span></span>

<span data-ttu-id="4abd3-189">De forma predeterminada, el proyecto de prueba incluye un archivo de prueba vacío denominado UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="4abd3-189">By default, your test project includes an empty test file named UnitTest1.cs.</span></span> <span data-ttu-id="4abd3-190">Este archivo muestra los atributos que se usan para crear métodos de prueba.</span><span class="sxs-lookup"><span data-stu-id="4abd3-190">This file shows the attributes you use to create test methods.</span></span> <span data-ttu-id="4abd3-191">En el caso de las pruebas unitarias, puede usar este archivo o crear su propio archivo.</span><span class="sxs-lookup"><span data-stu-id="4abd3-191">For your unit tests, you can either use this file or create your own file.</span></span>

![UnitTest1](unit-testing-with-aspnet-web-api/_static/image10.png)

<span data-ttu-id="4abd3-193">En este tutorial, creará su propia clase de prueba.</span><span class="sxs-lookup"><span data-stu-id="4abd3-193">For this tutorial, you will create your own test class.</span></span> <span data-ttu-id="4abd3-194">Puede eliminar el archivo UnitTest1.cs.</span><span class="sxs-lookup"><span data-stu-id="4abd3-194">You can delete the UnitTest1.cs file.</span></span> <span data-ttu-id="4abd3-195">Agregue una clase denominada **TestSimpleProductController.CS**y reemplace el código por el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="4abd3-195">Add a class named **TestSimpleProductController.cs**, and replace the code with the following code.</span></span>

[!code-csharp[Main](unit-testing-with-aspnet-web-api/samples/sample3.cs)]

<a id="runtests"></a>
## <a name="run-tests"></a><span data-ttu-id="4abd3-196">Ejecutar pruebas</span><span class="sxs-lookup"><span data-stu-id="4abd3-196">Run tests</span></span>

<span data-ttu-id="4abd3-197">Ahora está listo para ejecutar las pruebas.</span><span class="sxs-lookup"><span data-stu-id="4abd3-197">You are now ready to run the tests.</span></span> <span data-ttu-id="4abd3-198">Se probará todo el método marcado con el atributo **TestMethod** .</span><span class="sxs-lookup"><span data-stu-id="4abd3-198">All of the method that are marked with the **TestMethod** attribute will be tested.</span></span> <span data-ttu-id="4abd3-199">En el elemento de menú **prueba** , ejecute las pruebas.</span><span class="sxs-lookup"><span data-stu-id="4abd3-199">From the **Test** menu item, run the tests.</span></span>

![ejecución de pruebas](unit-testing-with-aspnet-web-api/_static/image11.png)

<span data-ttu-id="4abd3-201">Abra la ventana **Explorador de pruebas** y observe los resultados de las pruebas.</span><span class="sxs-lookup"><span data-stu-id="4abd3-201">Open the **Test Explorer** window, and notice the results of the tests.</span></span>

![resultados de pruebas](unit-testing-with-aspnet-web-api/_static/image12.png)

## <a name="summary"></a><span data-ttu-id="4abd3-203">Resumen</span><span class="sxs-lookup"><span data-stu-id="4abd3-203">Summary</span></span>

<span data-ttu-id="4abd3-204">Ha completado este tutorial.</span><span class="sxs-lookup"><span data-stu-id="4abd3-204">You have completed this tutorial.</span></span> <span data-ttu-id="4abd3-205">Los datos de este tutorial se han simplificado intencionalmente para centrarse en las condiciones de las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="4abd3-205">The data in this tutorial was intentionally simplified to focus on unit testing conditions.</span></span> <span data-ttu-id="4abd3-206">Para realizar pruebas unitarias de escenarios de datos más avanzados, consulte [Entity Framework ficticios cuando las pruebas unitarias ASP.net web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span><span class="sxs-lookup"><span data-stu-id="4abd3-206">For unit testing more advanced data scenarios, see [Mocking Entity Framework when Unit Testing ASP.NET Web API 2](mocking-entity-framework-when-unit-testing-aspnet-web-api-2.md).</span></span>
