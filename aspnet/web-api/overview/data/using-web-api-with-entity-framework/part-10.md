---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publicar la aplicación en Azure Azure App Service | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: a9a7b74a07c71b47253c0af304c7a26ffa4eaae5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504763"
---
# <a name="publish-the-app-to-azure-azure-app-service"></a><span data-ttu-id="3605d-102">Publicación de la aplicación en Azure Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3605d-102">Publish the App to Azure Azure App Service</span></span>

<span data-ttu-id="3605d-103">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3605d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3605d-104">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="3605d-104">Download Completed Project</span></span>](https://github.com/MikeWasson/BookService)

<span data-ttu-id="3605d-105">Como último paso, publicará la aplicación en Azure.</span><span class="sxs-lookup"><span data-stu-id="3605d-105">As the last step, you will publish the application to Azure.</span></span> <span data-ttu-id="3605d-106">En Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="3605d-106">In Solution Explorer, right-click the project and select **Publish**.</span></span>

![](part-10/_static/image1.png)

<span data-ttu-id="3605d-107">Al hacer clic en **publicar** se invoca el cuadro de diálogo **publicación web** .</span><span class="sxs-lookup"><span data-stu-id="3605d-107">Clicking **Publish** invokes the **Publish Web** dialog.</span></span> <span data-ttu-id="3605d-108">Si seleccionó **hospedar en la nube** cuando creó el proyecto por primera vez, la conexión y la configuración ya están configuradas.</span><span class="sxs-lookup"><span data-stu-id="3605d-108">If you checked **Host in Cloud** when you first created the project, then the connection and settings are already configured.</span></span> <span data-ttu-id="3605d-109">En ese caso, solo tiene que hacer clic en la pestaña **configuración** y comprobar &quot;ejecutar migraciones de Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="3605d-109">In that case, just click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="3605d-110">(Si no seleccionó **host en la nube** al principio, siga los pasos descritos en la [sección siguiente](#new-website)).</span><span class="sxs-lookup"><span data-stu-id="3605d-110">(If you didn't check **Host in Cloud** at the beginning, then follow the steps in the [next section](#new-website).)</span></span>

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

<span data-ttu-id="3605d-111">Para implementar la aplicación, haga clic en **publicar**.</span><span class="sxs-lookup"><span data-stu-id="3605d-111">To deploy the app, click **Publish**.</span></span> <span data-ttu-id="3605d-112">Puede ver el progreso de la publicación en la ventana **actividad de publicación web** .</span><span class="sxs-lookup"><span data-stu-id="3605d-112">You can view the publishing progress in the **Web Publish Activity** window.</span></span> <span data-ttu-id="3605d-113">(En el menú **Ver** , seleccione **otras ventanas**y, a continuación, seleccione **actividad de publicación web**).</span><span class="sxs-lookup"><span data-stu-id="3605d-113">(From the **View** menu, select **Other Windows**, then select **Web Publish Activity**.)</span></span>

![](part-10/_static/image4.png)

<span data-ttu-id="3605d-114">Cuando Visual Studio finaliza la implementación de la aplicación, el explorador predeterminado se abre automáticamente en la dirección URL del sitio Web implementado y la aplicación que ha creado se ejecuta ahora en la nube.</span><span class="sxs-lookup"><span data-stu-id="3605d-114">When Visual Studio finishes deploying the app, the default browser automatically opens to the URL of the deployed website, and the application that you created is now running in the cloud.</span></span> <span data-ttu-id="3605d-115">La dirección URL de la barra de direcciones del explorador muestra que el sitio se está cargando desde Internet.</span><span class="sxs-lookup"><span data-stu-id="3605d-115">The URL in the browser address bar shows that the site is being loaded from the Internet.</span></span>

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a><span data-ttu-id="3605d-116">Implementación en un sitio web nuevo</span><span class="sxs-lookup"><span data-stu-id="3605d-116">Deploying to a New Website</span></span>

<span data-ttu-id="3605d-117">Si no ha activado **host en la nube** cuando creó el proyecto por primera vez, puede configurar ahora una nueva aplicación Web.</span><span class="sxs-lookup"><span data-stu-id="3605d-117">If you did not check **Host in Cloud** when you first created the project, you can configure a new web app now.</span></span> <span data-ttu-id="3605d-118">En Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **publicar**.</span><span class="sxs-lookup"><span data-stu-id="3605d-118">In Solution Explorer, right-click the project and select **Publish**.</span></span> <span data-ttu-id="3605d-119">Seleccione la pestaña **perfil** y haga clic en **Microsoft Azure websites**.</span><span class="sxs-lookup"><span data-stu-id="3605d-119">Select the **Profile** tab and click **Microsoft Azure Websites**.</span></span> <span data-ttu-id="3605d-120">Si no ha iniciado sesión actualmente en Azure, se le pedirá que inicie sesión.</span><span class="sxs-lookup"><span data-stu-id="3605d-120">If you aren't currently signed in to Azure, you will be prompted to sign in.</span></span>

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

<span data-ttu-id="3605d-121">En el cuadro de diálogo **sitios Web existentes** , haga clic en **nuevo**.</span><span class="sxs-lookup"><span data-stu-id="3605d-121">In the **Existing Websites** dialog, click **New**.</span></span>

![](part-10/_static/image9.png)

<span data-ttu-id="3605d-122">Escriba un nombre de sitio.</span><span class="sxs-lookup"><span data-stu-id="3605d-122">Enter a site name.</span></span> <span data-ttu-id="3605d-123">Seleccione su suscripción de Azure y la región.</span><span class="sxs-lookup"><span data-stu-id="3605d-123">Select your Azure subscription and the region.</span></span> <span data-ttu-id="3605d-124">En **servidor de base de datos**, seleccione **crear nuevo servidor**o seleccione un servidor existente.</span><span class="sxs-lookup"><span data-stu-id="3605d-124">Under **Database server**, select **Create New Server**, or select an existing server.</span></span> <span data-ttu-id="3605d-125">Haga clic en **Crear**.</span><span class="sxs-lookup"><span data-stu-id="3605d-125">Click **Create**.</span></span>

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

<span data-ttu-id="3605d-126">Haga clic en la pestaña **configuración** y compruebe &quot;ejecutar migraciones de Code First&quot;.</span><span class="sxs-lookup"><span data-stu-id="3605d-126">Click the **Settings** tab and check &quot;Execute Code First Migrations&quot;.</span></span> <span data-ttu-id="3605d-127">A continuación, haga clic en **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="3605d-127">Then click **Publish**.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="3605d-128">Anterior</span><span class="sxs-lookup"><span data-stu-id="3605d-128">Previous</span></span>](part-9.md)
