---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-1
title: Uso de Web API 2 con Entity Framework 6 | Microsoft Docs
author: MikeWasson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación web con un ASP.NET Web API back-end. En el tutorial se usa Entity Framework 6 para los datos...
ms.author: riande
ms.date: 01/17/2019
ms.assetid: e879487e-dbcd-4b33-b092-d67c37ae768c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-1
msc.type: authoredcontent
ms.openlocfilehash: 0f5dc960f494af5bd4ce87863a510d1892319908
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504841"
---
# <a name="using-web-api-2-with-entity-framework-6"></a><span data-ttu-id="a6cd6-104">Usar Web API 2 con Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a6cd6-104">Using Web API 2 with Entity Framework 6</span></span>

[<span data-ttu-id="a6cd6-105">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="a6cd6-105">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

> <span data-ttu-id="a6cd6-106">Este tutorial le enseña los aspectos básicos de la creación de una aplicación web con un back-end ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-106">This tutorial teaches you the basics of creating a web application with an ASP.NET Web API back end.</span></span> <span data-ttu-id="a6cd6-107">En el tutorial se usa Entity Framework 6 para la capa de datos y Knockout. js para la aplicación JavaScript del lado cliente.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-107">The tutorial uses Entity Framework 6 for the data layer, and Knockout.js for the client-side JavaScript application.</span></span> <span data-ttu-id="a6cd6-108">En el tutorial también se muestra cómo implementar la aplicación en Azure App Service Web Apps.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-108">The tutorial also shows how to deploy the app to Azure App Service Web Apps.</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a6cd6-109">Versiones de software usadas en el tutorial</span><span class="sxs-lookup"><span data-stu-id="a6cd6-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="a6cd6-110">API Web 2,1</span><span class="sxs-lookup"><span data-stu-id="a6cd6-110">Web API 2.1</span></span>
> - <span data-ttu-id="a6cd6-111">Visual Studio 2017 (Descargue Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="a6cd6-111">Visual Studio 2017 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="a6cd6-112">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a6cd6-112">Entity Framework 6</span></span>
> - <span data-ttu-id="a6cd6-113">.NET 4.7</span><span class="sxs-lookup"><span data-stu-id="a6cd6-113">.NET 4.7</span></span>
> - <span data-ttu-id="a6cd6-114">[Knockout. js](http://knockoutjs.com/) 3,1</span><span class="sxs-lookup"><span data-stu-id="a6cd6-114">[Knockout.js](http://knockoutjs.com/) 3.1</span></span>

<span data-ttu-id="a6cd6-115">En este tutorial se usa ASP.NET Web API 2 con Entity Framework 6 para crear una aplicación web que manipula una base de datos back-end.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-115">This tutorial uses ASP.NET Web API 2 with Entity Framework 6 to create a web application that manipulates a back-end database.</span></span> <span data-ttu-id="a6cd6-116">Esta es una captura de pantalla de la aplicación que creará.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-116">Here is a screen shot of the application that you will create.</span></span>

[![](part-1/_static/image2.png)](part-1/_static/image1.png)

<span data-ttu-id="a6cd6-117">La aplicación usa un diseño de aplicación de una sola página (SPA).</span><span class="sxs-lookup"><span data-stu-id="a6cd6-117">The app uses a single-page application (SPA) design.</span></span> <span data-ttu-id="a6cd6-118">"Aplicación de una sola página" es el término general para una aplicación web que carga una sola página HTML y, a continuación, actualiza la página dinámicamente, en lugar de cargar nuevas páginas.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-118">"Single-page application" is the general term for a web application that loads a single HTML page and then updates the page dynamically, instead of loading new pages.</span></span> <span data-ttu-id="a6cd6-119">Después de la carga de página inicial, la aplicación se comunica con el servidor a través de solicitudes AJAX.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-119">After the initial page load, the app talks with the server through AJAX requests.</span></span> <span data-ttu-id="a6cd6-120">Las solicitudes AJAX devuelven datos JSON, que la aplicación usa para actualizar la interfaz de usuario.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-120">The AJAX requests return JSON data, which the app uses to update the UI.</span></span>

<span data-ttu-id="a6cd6-121">AJAX no es nuevo, pero hoy en día hay marcos de JavaScript que facilitan la compilación y el mantenimiento de una aplicación de SPA sofisticada.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-121">AJAX isn't new, but today there are JavaScript frameworks that make it easier to build and maintain a large sophisticated SPA application.</span></span> <span data-ttu-id="a6cd6-122">En este tutorial se usa [Knockout. js](http://knockoutjs.com/), pero puede usar cualquier marco de cliente de JavaScript.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-122">This tutorial uses [Knockout.js](http://knockoutjs.com/), but you can use any JavaScript client framework.</span></span>

<span data-ttu-id="a6cd6-123">Estos son los principales bloques de creación para esta aplicación:</span><span class="sxs-lookup"><span data-stu-id="a6cd6-123">Here are the main building blocks for this app:</span></span>

- <span data-ttu-id="a6cd6-124">ASP.NET MVC crea la página HTML.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-124">ASP.NET MVC creates the HTML page.</span></span>
- <span data-ttu-id="a6cd6-125">ASP.NET Web API controla las solicitudes AJAX y devuelve datos JSON.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-125">ASP.NET Web API handles the AJAX requests and returns JSON data.</span></span>
- <span data-ttu-id="a6cd6-126">Datos de Knockout. js: enlaza los elementos HTML a los datos JSON.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-126">Knockout.js data-binds the HTML elements to the JSON data.</span></span>
- <span data-ttu-id="a6cd6-127">Entity Framework se comunica con la base de datos.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-127">Entity Framework talks to the database.</span></span>

## <a name="see-this-app-running-on-azure"></a><span data-ttu-id="a6cd6-128">Vea esta aplicación en ejecución en Azure</span><span class="sxs-lookup"><span data-stu-id="a6cd6-128">See this app running on Azure</span></span>

<span data-ttu-id="a6cd6-129">¿Desea ver el sitio terminado que se ejecuta como una aplicación Web activa?</span><span class="sxs-lookup"><span data-stu-id="a6cd6-129">Would you like to see the finished site running as a live web app?</span></span> <span data-ttu-id="a6cd6-130">Puede implementar una versión completa de la aplicación en su cuenta de Azure; para ello, seleccione el botón siguiente.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-130">You can deploy a complete version of the app to your Azure account by selecting the following button.</span></span>

[![](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?WT.mc_id=deploy_azure_aspnet&repository=https://github.com/tfitzmac/BookService)

<span data-ttu-id="a6cd6-131">Necesita una cuenta de Azure para implementar esta solución en Azure.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-131">You need an Azure account to deploy this solution to Azure.</span></span> <span data-ttu-id="a6cd6-132">Si aún no tiene una cuenta, tiene las siguientes opciones:</span><span class="sxs-lookup"><span data-stu-id="a6cd6-132">If you do not already have an account, you have the following options:</span></span>

- <span data-ttu-id="a6cd6-133">[Abra una cuenta de Azure gratis](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) : obtiene créditos que puede usar para probar los servicios de Azure de pago y, incluso después de que se agoten, puede mantener la cuenta y usar los servicios gratuitos de Azure.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-133">[Open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services.</span></span>
- <span data-ttu-id="a6cd6-134">[Activar las ventajas de suscriptor de MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) : su suscripción a MSDN le proporciona créditos todos los meses que puede usar para los servicios de Azure de pago.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-134">[Activate MSDN subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604) - Your MSDN subscription gives you credits every month that you can use for paid Azure services.</span></span>

## <a name="create-the-project"></a><span data-ttu-id="a6cd6-135">Crear el proyecto</span><span class="sxs-lookup"><span data-stu-id="a6cd6-135">Create the project</span></span>

<span data-ttu-id="a6cd6-136">Abra Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-136">Open Visual Studio.</span></span> <span data-ttu-id="a6cd6-137">En el menú **archivo** , seleccione **nuevo**y seleccione **proyecto**.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-137">From the **File** menu, select **New**, then select **Project**.</span></span> <span data-ttu-id="a6cd6-138">(O seleccione **nuevo proyecto** en la página de inicio).</span><span class="sxs-lookup"><span data-stu-id="a6cd6-138">(Or select **New Project** on the Start page.)</span></span>

<span data-ttu-id="a6cd6-139">En el cuadro de diálogo **nuevo proyecto** , seleccione **Web** en el panel izquierdo y **ASP.NET Web Application (.NET Framework)** en el panel central.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-139">In the **New Project** dialog, select **Web** in the left pane and **ASP.NET Web Application (.NET Framework)** in the middle pane.</span></span> <span data-ttu-id="a6cd6-140">Asigne al proyecto el nombre **BookService** y seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-140">Name the project **BookService** and select **OK**.</span></span>

[![](part-1/_static/image11.png)](part-1/_static/image11.png)

<span data-ttu-id="a6cd6-141">En el cuadro de diálogo **nuevo proyecto ASP.net** , seleccione la plantilla **API Web** .</span><span class="sxs-lookup"><span data-stu-id="a6cd6-141">In the **New ASP.NET Project** dialog, select the **Web API** template.</span></span>

[![](part-1/_static/image12.png)](part-1/_static/image12.png)

<span data-ttu-id="a6cd6-142">Seleccione **Aceptar** para crear el proyecto.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-142">Select **OK** to create the project.</span></span>

## <a name="configure-azure-settings-optional"></a><span data-ttu-id="a6cd6-143">Configuración de Azure (opcional)</span><span class="sxs-lookup"><span data-stu-id="a6cd6-143">Configure Azure settings (optional)</span></span>

<span data-ttu-id="a6cd6-144">Después de crear el proyecto, puede optar por implementarlo en Azure App Service Web Apps en cualquier momento.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-144">After you create the project, you can choose to deploy to Azure App Service Web Apps at any time.</span></span> 

1. <span data-ttu-id="a6cd6-145">En Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-145">In Solution Explorer, right-click on your project and select **Publish**.</span></span>

2. <span data-ttu-id="a6cd6-146">En la ventana que aparece, seleccione **iniciar**.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-146">In the window that appears, select **Start**.</span></span> <span data-ttu-id="a6cd6-147">Aparece el cuadro de diálogo **seleccionar un destino de publicación** .</span><span class="sxs-lookup"><span data-stu-id="a6cd6-147">The **Pick a publish target** dialog box appears.</span></span>

   [![](part-1/_static/image14.png)](part-1/_static/image14.png)

3. <span data-ttu-id="a6cd6-148">Seleccione **Crear perfil**.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-148">Select **Create Profile**.</span></span> <span data-ttu-id="a6cd6-149">Aparecerá el cuadro de diálogo **Crear servicio de aplicaciones**.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-149">The **Create App Service** dialog box appears.</span></span>

   [![](part-1/_static/image15.png)](part-1/_static/image15.png)

   <span data-ttu-id="a6cd6-150">Acepte los valores predeterminados o escriba valores diferentes para el nombre de la aplicación, el grupo de recursos, el plan de hospedaje, la suscripción de Azure y la región geográfica.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-150">Accept the defaults, or enter different values for the application name, resource group, hosting plan, Azure subscription, and geographical region.</span></span> 

4. <span data-ttu-id="a6cd6-151">Seleccione **crear una base de datos SQL**.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-151">Select **Create a SQL database**.</span></span> <span data-ttu-id="a6cd6-152">Aparece el cuadro de diálogo **configurar SQL Server** .</span><span class="sxs-lookup"><span data-stu-id="a6cd6-152">The **Configure SQL Server** dialog box appears.</span></span> 

   [![](part-1/_static/image16.png)](part-1/_static/image16.png)

   <span data-ttu-id="a6cd6-153">Acepte los valores predeterminados o escriba valores distintos.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-153">Accept the defaults or enter different values.</span></span> <span data-ttu-id="a6cd6-154">Escriba un **nombre de usuario de administrador** y una **contraseña de administrador** para la nueva base de datos.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-154">Enter an **Administrator Username** and **Administrator Password** for your new database.</span></span> <span data-ttu-id="a6cd6-155">Seleccione **Aceptar** cuando haya terminado.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-155">Select **OK** when you're done.</span></span> <span data-ttu-id="a6cd6-156">Vuelve a aparecer la página **crear App Service** .</span><span class="sxs-lookup"><span data-stu-id="a6cd6-156">The **Create App Service** page reappears.</span></span>

5. <span data-ttu-id="a6cd6-157">Seleccione **crear** para crear el perfil.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-157">Select **Create** to create your profile.</span></span> <span data-ttu-id="a6cd6-158">Aparece un mensaje en la esquina inferior derecha que indica que la implementación está en curso.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-158">A message appears in the lower-right corner indicating that deployment is in progress.</span></span> <span data-ttu-id="a6cd6-159">Tras unos instantes, la ventana de **publicación** vuelve a aparecer.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-159">After a short while, the **Publish** window reappears.</span></span>

    [![](part-1/_static/image17.png)](part-1/_static/image17.png)
   
    <span data-ttu-id="a6cd6-160">El perfil que ha creado para implementar la aplicación ya está disponible.</span><span class="sxs-lookup"><span data-stu-id="a6cd6-160">The profile you created to deploy the app is now available.</span></span> 

> [!div class="step-by-step"]
> [<span data-ttu-id="a6cd6-161">Siguiente</span><span class="sxs-lookup"><span data-stu-id="a6cd6-161">Next</span></span>](part-2.md)
