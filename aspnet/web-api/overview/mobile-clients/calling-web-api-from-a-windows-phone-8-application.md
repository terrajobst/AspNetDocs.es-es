---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Llamada a la API Web desde una aplicación Windows PhoneC#8 ()-ASP.net 4. x
author: rmcmurray
description: 'Tutorial con código: cree una aplicación ASP.NET Web API en ASP.NET 4. x que proporciona un catálogo de libros para una aplicación Windows Phone 8.'
ms.author: riande
ms.date: 10/09/2013
ms.custom: seoapril2019
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: c5da14a6856f551343b6fb14f0aedc659e792f6b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78498217"
---
# <a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="8fe67-103">Llamar a Web API desde una aplicación de Windows Phone 8 (C#)</span><span class="sxs-lookup"><span data-stu-id="8fe67-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>

<span data-ttu-id="8fe67-104">por [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="8fe67-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="8fe67-105">En este tutorial, aprenderá a crear un escenario completo de un extremo a otro que consiste en una aplicación ASP.NET Web API que proporciona un catálogo de libros para una aplicación Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="8fe67-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="8fe67-106">Información general</span><span class="sxs-lookup"><span data-stu-id="8fe67-106">Overview</span></span>

<span data-ttu-id="8fe67-107">Los servicios RESTful como ASP.NET Web API simplifican la creación de aplicaciones basadas en HTTP para desarrolladores mediante la abstracción de la arquitectura para las aplicaciones del lado servidor y del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="8fe67-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="8fe67-108">En lugar de crear un protocolo basado en sockets propietario para la comunicación, los desarrolladores de la API Web simplemente necesitan publicar los métodos HTTP necesarios para su aplicación (por ejemplo: GET, POST, PUT, DELETE) y los desarrolladores de aplicaciones cliente solo necesitan consumir los métodos HTTP necesarios para su aplicación.</span><span class="sxs-lookup"><span data-stu-id="8fe67-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="8fe67-109">En este tutorial de un extremo a otro, aprenderá a usar la API Web para crear los siguientes proyectos:</span><span class="sxs-lookup"><span data-stu-id="8fe67-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="8fe67-110">En la [primera parte de este tutorial](#STEP1), creará una aplicación ASP.net web API que admita todas las operaciones de creación, lectura, actualización y eliminación (CRUD) para administrar un catálogo de libros.</span><span class="sxs-lookup"><span data-stu-id="8fe67-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="8fe67-111">Esta aplicación usará el [archivo XML de ejemplo (Books. xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) de MSDN.</span><span class="sxs-lookup"><span data-stu-id="8fe67-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="8fe67-112">En la [segunda parte de este tutorial](#STEP2), creará una aplicación interactiva Windows Phone 8 que recupera los datos de la aplicación de API Web.</span><span class="sxs-lookup"><span data-stu-id="8fe67-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="8fe67-113">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="8fe67-113">Prerequisites</span></span>

- <span data-ttu-id="8fe67-114">Visual Studio 2013 con el SDK de Windows Phone 8 instalado</span><span class="sxs-lookup"><span data-stu-id="8fe67-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="8fe67-115">Windows 8 o posterior en un sistema de 64 bits con Hyper-V instalado</span><span class="sxs-lookup"><span data-stu-id="8fe67-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="8fe67-116">Para obtener una lista de requisitos adicionales, consulte la sección *requisitos del sistema* en la página de descarga del [SDK de Windows Phone 8,0](https://www.microsoft.com/download/details.aspx?id=35471) .</span><span class="sxs-lookup"><span data-stu-id="8fe67-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="8fe67-117">Si va a probar la conectividad entre los proyectos de API Web y Windows Phone 8 en el sistema local, deberá seguir las instrucciones del artículo *[conexión del emulador de Windows Phone 8 a aplicaciones de API Web en un equipo local](https://go.microsoft.com/fwlink/?LinkId=324014)* para configurar el entorno de pruebas.</span><span class="sxs-lookup"><span data-stu-id="8fe67-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="8fe67-118">Paso 1: creación del proyecto de librería de API Web</span><span class="sxs-lookup"><span data-stu-id="8fe67-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="8fe67-119">El primer paso de este tutorial de un extremo a otro es crear un proyecto de API Web que admita todas las operaciones CRUD; Tenga en cuenta que agregará el proyecto de aplicación de Windows Phone a esta solución en el [paso 2](#STEP2) de este tutorial.</span><span class="sxs-lookup"><span data-stu-id="8fe67-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="8fe67-120">Abra **Visual Studio 2013**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="8fe67-121">Haga clic en **archivo**, **nuevo**y **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="8fe67-122">Cuando se muestre el cuadro de diálogo **nuevo proyecto** , expanda **instalado**, **plantillas**, **Visual C#** y, a continuación, **Web**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="8fe67-123">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="8fe67-123">Click image to expand</span></span>                                                                |

4. <span data-ttu-id="8fe67-124">Resalte la **aplicación Web ASP.net**, escriba **BookStore** como nombre del proyecto y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="8fe67-125">Cuando se muestre el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **API Web** y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>

   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="8fe67-126">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="8fe67-126">Click image to expand</span></span>                                                                |

6. <span data-ttu-id="8fe67-127">Cuando se abra el proyecto de API Web, quite el controlador de ejemplo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="8fe67-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="8fe67-128">Expanda la carpeta **Controladores** en el explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="8fe67-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="8fe67-129">Haga clic con el botón secundario en el archivo **ValuesController.CS** y, a continuación, haga clic en **eliminar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="8fe67-130">Haga clic en **Aceptar** cuando se le pida que confirme la eliminación.</span><span class="sxs-lookup"><span data-stu-id="8fe67-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="8fe67-131">Agregar un archivo de datos XML al proyecto de API Web; este archivo incluye el contenido del catálogo de la librería:</span><span class="sxs-lookup"><span data-stu-id="8fe67-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="8fe67-132">Haga clic con el botón secundario en la carpeta **App\_Data** del explorador de soluciones, haga clic en **Agregar**y, a continuación, haga clic en **nuevo elemento**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="8fe67-133">Cuando se muestre el cuadro de diálogo **Agregar nuevo elemento** , resalte la plantilla de **archivo XML** .</span><span class="sxs-lookup"><span data-stu-id="8fe67-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="8fe67-134">Asigne al archivo el nombre **books. XML**y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="8fe67-135">Cuando se abra el archivo **books. XML** , reemplace el código del archivo por el XML del archivo **books. XML** de ejemplo en MSDN:</span><span class="sxs-lookup"><span data-stu-id="8fe67-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="8fe67-136">Guarde y cierre el archivo XML.</span><span class="sxs-lookup"><span data-stu-id="8fe67-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="8fe67-137">Agregue el modelo Bookstore al proyecto de API Web. Este modelo contiene la lógica de creación, lectura, actualización y eliminación (CRUD) de la aplicación Bookstore:</span><span class="sxs-lookup"><span data-stu-id="8fe67-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="8fe67-138">Haga clic con el botón secundario en la carpeta **modelos** en el explorador de soluciones, haga clic en **Agregar**y, a continuación, en **clase**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="8fe67-139">Cuando se muestre el cuadro de diálogo **Agregar nuevo elemento** , asigne un nombre al archivo de clase **BookDetails.CS**y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="8fe67-140">Cuando se abra el archivo **BookDetails.CS** , reemplace el código del archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8fe67-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="8fe67-141">Guarde y cierre el archivo **BookDetails.CS** .</span><span class="sxs-lookup"><span data-stu-id="8fe67-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="8fe67-142">Agregue el controlador de la librería al proyecto de API Web:</span><span class="sxs-lookup"><span data-stu-id="8fe67-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="8fe67-143">Haga clic con el botón secundario en la carpeta **Controllers** del explorador de soluciones, haga clic en **Agregar**y, a continuación, en **controlador**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="8fe67-144">Cuando aparezca el cuadro de diálogo **Agregar scaffold** , resalte el **controlador de Web API 2-vacío**y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="8fe67-145">Cuando se muestre el cuadro de diálogo **Agregar controlador** , asigne al controlador el nombre **BooksController**y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="8fe67-146">Cuando se abra el archivo **BooksController.CS** , reemplace el código del archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8fe67-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="8fe67-147">Guarde y cierre el archivo **BooksController.CS** .</span><span class="sxs-lookup"><span data-stu-id="8fe67-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="8fe67-148">Cree la aplicación de API Web para comprobar si hay errores.</span><span class="sxs-lookup"><span data-stu-id="8fe67-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="8fe67-149">Paso 2: agregar el proyecto de catálogo de la librería de Windows Phone 8</span><span class="sxs-lookup"><span data-stu-id="8fe67-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="8fe67-150">El siguiente paso de este escenario de un extremo a otro es crear la aplicación de catálogo para Windows Phone 8.</span><span class="sxs-lookup"><span data-stu-id="8fe67-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="8fe67-151">Esta aplicación usará el Windows Phone plantilla de *aplicación de enlace* de datos para la interfaz de usuario predeterminada y usará la aplicación de API Web que creó en el [paso 1](#STEP1) de este tutorial como origen de datos.</span><span class="sxs-lookup"><span data-stu-id="8fe67-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="8fe67-152">Haga clic con el botón secundario en la solución **BookStore** en el explorador de soluciones, haga clic en **Agregar**y, a continuación, en **nuevo proyecto**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="8fe67-153">Cuando se muestre el cuadro de diálogo **nuevo proyecto** , expanda **instalado**, luego **Visual C#** y, a continuación, **Windows Phone**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="8fe67-154">Resalte **Windows Phone aplicación DataBound**, escriba **BookCatalog** para el nombre y, a continuación, haga clic en **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="8fe67-155">Agregue el paquete NuGet Json.NET al proyecto **BookCatalog** :</span><span class="sxs-lookup"><span data-stu-id="8fe67-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="8fe67-156">Haga clic con el botón derecho en **referencias** para el proyecto **BookCatalog** en el explorador de soluciones y, a continuación, haga clic en **administrar paquetes NuGet**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="8fe67-157">Cuando aparezca el cuadro de diálogo **administrar paquetes NuGet** , expanda la sección **en línea** y resalte **Nuget.org**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="8fe67-158">Escriba **JSON.net** en el campo de búsqueda y haga clic en el icono de búsqueda.</span><span class="sxs-lookup"><span data-stu-id="8fe67-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="8fe67-159">Resalte **JSON.net** en los resultados de la búsqueda y, a continuación, haga clic en **instalar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="8fe67-160">Una vez finalizada la instalación, haga clic en **cerrar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="8fe67-161">Agregue el modelo **BookDetails** al proyecto **BookCatalog** ; contiene un modelo genérico de la clase Bookstore:</span><span class="sxs-lookup"><span data-stu-id="8fe67-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="8fe67-162">Haga clic con el botón secundario en el proyecto **BookCatalog** en el explorador de soluciones, haga clic en **Agregar**y, a continuación, en **nueva carpeta**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="8fe67-163">Asigne un nombre a los nuevos **modelos**de carpeta.</span><span class="sxs-lookup"><span data-stu-id="8fe67-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="8fe67-164">Haga clic con el botón secundario en la carpeta **modelos** en el explorador de soluciones, haga clic en **Agregar**y, a continuación, en **clase**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="8fe67-165">Cuando se muestre el cuadro de diálogo **Agregar nuevo elemento** , asigne un nombre al archivo de clase **BookDetails.CS**y, a continuación, haga clic en **Agregar**.</span><span class="sxs-lookup"><span data-stu-id="8fe67-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="8fe67-166">Cuando se abra el archivo **BookDetails.CS** , reemplace el código del archivo por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8fe67-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="8fe67-167">Guarde y cierre el archivo **BookDetails.CS** .</span><span class="sxs-lookup"><span data-stu-id="8fe67-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="8fe67-168">Actualice la clase **MainViewModel.CS** para incluir la funcionalidad para comunicarse con la aplicación de API Web de BookStore:</span><span class="sxs-lookup"><span data-stu-id="8fe67-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="8fe67-169">Expanda la carpeta **ViewModels** en el explorador de soluciones y, a continuación, haga doble clic en el archivo **MainViewModel.CS** .</span><span class="sxs-lookup"><span data-stu-id="8fe67-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="8fe67-170">Cuando se abra el archivo **MainViewModel.CS** , reemplace el código del archivo por lo siguiente: Tenga en cuenta que tendrá que actualizar el valor de la constante `apiUrl` con la dirección URL real de la API Web:</span><span class="sxs-lookup"><span data-stu-id="8fe67-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="8fe67-171">Guarde y cierre el archivo **MainViewModel.CS** .</span><span class="sxs-lookup"><span data-stu-id="8fe67-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="8fe67-172">Actualice el archivo **mainpage. Xaml** para personalizar el nombre de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="8fe67-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="8fe67-173">Haga doble clic en el archivo **mainpage. Xaml** en el explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="8fe67-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="8fe67-174">Cuando se abra el archivo **mainpage. Xaml** , busque las siguientes líneas de código:</span><span class="sxs-lookup"><span data-stu-id="8fe67-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="8fe67-175">Reemplace esas líneas por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8fe67-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="8fe67-176">Guarde y cierre el archivo **mainpage. Xaml** .</span><span class="sxs-lookup"><span data-stu-id="8fe67-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="8fe67-177">Actualice el archivo **DetailsPage. Xaml** para personalizar los elementos mostrados:</span><span class="sxs-lookup"><span data-stu-id="8fe67-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="8fe67-178">Haga doble clic en el archivo **DetailsPage. Xaml** en el explorador de soluciones.</span><span class="sxs-lookup"><span data-stu-id="8fe67-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="8fe67-179">Cuando se abra el archivo **DetailsPage. Xaml** , busque las siguientes líneas de código:</span><span class="sxs-lookup"><span data-stu-id="8fe67-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="8fe67-180">Reemplace esas líneas por lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="8fe67-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="8fe67-181">Guarde y cierre el archivo **DetailsPage. Xaml** .</span><span class="sxs-lookup"><span data-stu-id="8fe67-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="8fe67-182">Compile la aplicación de Windows Phone para comprobar si hay errores.</span><span class="sxs-lookup"><span data-stu-id="8fe67-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="8fe67-183">Paso 3: probar la solución de un extremo a otro</span><span class="sxs-lookup"><span data-stu-id="8fe67-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="8fe67-184">Como se mencionó en la sección de *requisitos previos* de este tutorial, al probar la conectividad entre la API Web y los proyectos de Windows Phone 8 en el sistema local, deberá seguir las instrucciones del artículo *[conexión del emulador de Windows Phone 8 a aplicaciones de API Web en un equipo local](https://go.microsoft.com/fwlink/?LinkId=324014)* para configurar el entorno de pruebas.</span><span class="sxs-lookup"><span data-stu-id="8fe67-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="8fe67-185">Una vez configurado el entorno de pruebas, tendrá que establecer la aplicación Windows Phone como el proyecto de inicio.</span><span class="sxs-lookup"><span data-stu-id="8fe67-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="8fe67-186">Para ello, resalte la aplicación **BookCatalog** en el explorador de soluciones y, a continuación, haga clic en **establecer como proyecto de inicio**:</span><span class="sxs-lookup"><span data-stu-id="8fe67-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="8fe67-187">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="8fe67-187">Click image to expand</span></span> |

<span data-ttu-id="8fe67-188">Al presionar F5, Visual Studio iniciará el emulador de Windows Phone, que mostrará una &quot;espere&quot; mensaje mientras se recuperan los datos de la aplicación de la API Web:</span><span class="sxs-lookup"><span data-stu-id="8fe67-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="8fe67-189">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="8fe67-189">Click image to expand</span></span> |

<span data-ttu-id="8fe67-190">Si todo se realiza correctamente, debería ver el catálogo mostrado:</span><span class="sxs-lookup"><span data-stu-id="8fe67-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="8fe67-191">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="8fe67-191">Click image to expand</span></span> |

<span data-ttu-id="8fe67-192">Si puntea en cualquier título de libro, la aplicación mostrará la descripción del libro:</span><span class="sxs-lookup"><span data-stu-id="8fe67-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="8fe67-193">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="8fe67-193">Click image to expand</span></span> |

<span data-ttu-id="8fe67-194">Si la aplicación no puede comunicarse con la API Web, se mostrará un mensaje de error:</span><span class="sxs-lookup"><span data-stu-id="8fe67-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="8fe67-195">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="8fe67-195">Click image to expand</span></span> |

<span data-ttu-id="8fe67-196">Si puntea en el mensaje de error, se mostrarán los detalles adicionales sobre el error:</span><span class="sxs-lookup"><span data-stu-id="8fe67-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="8fe67-197">Haga clic en la imagen para expandir</span><span class="sxs-lookup"><span data-stu-id="8fe67-197">Click image to expand</span></span>                                                                 |
