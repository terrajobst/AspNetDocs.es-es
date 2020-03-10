---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Compilación del capítulo descargas para los tutoriales de EF 5 MVC 4 | Microsoft Docs
author: Rick-Anderson
description: En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 10237af40e3914b65e5181f17555697e86adea4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78485125"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="d922c-103">Compilación del capítulo descargas para los tutoriales de EF 5 MVC 4</span><span class="sxs-lookup"><span data-stu-id="d922c-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>

<span data-ttu-id="d922c-104">por [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d922c-104">by [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[<span data-ttu-id="d922c-105">Descargar proyecto completado</span><span class="sxs-lookup"><span data-stu-id="d922c-105">Download Completed Project</span></span>](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="d922c-106">En la aplicación Web de ejemplo contoso University se muestra cómo crear aplicaciones ASP.NET MVC 4 con el Code First de Entity Framework 5 y Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d922c-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="d922c-107">Para obtener información sobre la serie de tutoriales, consulte [el primer tutorial de la serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span><span class="sxs-lookup"><span data-stu-id="d922c-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>

## <a name="building-the-chapter-downloads"></a><span data-ttu-id="d922c-108">Crear las descargas de capítulos</span><span class="sxs-lookup"><span data-stu-id="d922c-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="d922c-109">Descargue y descomprima el archivo zip de ejemplo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="d922c-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="d922c-110">En el paquete de descarga descomprimido, encontrará archivos zip adicionales, uno para la realización de cada capítulo.</span><span class="sxs-lookup"><span data-stu-id="d922c-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="d922c-111">Haga clic con el botón derecho en el archivo zip deseado, haga clic en **propiedades**y, a continuación, haga clic en el botón **desbloquear** .</span><span class="sxs-lookup"><span data-stu-id="d922c-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="d922c-112">Descomprima el archivo.</span><span class="sxs-lookup"><span data-stu-id="d922c-112">Unzip the file.</span></span>
4. <span data-ttu-id="d922c-113">Haga doble clic en el archivo *CUx. sln* para iniciar Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d922c-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="d922c-114">En el menú **herramientas** , haga clic en **Administrador de paquetes NuGet**y luego en **consola del administrador de paquetes**.</span><span class="sxs-lookup"><span data-stu-id="d922c-114">From the **Tools** menu, click **NuGet Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="d922c-115">En la consola del administrador de paquetes (PMC), haga clic en **restaurar**.</span><span class="sxs-lookup"><span data-stu-id="d922c-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="d922c-116">Salga de Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d922c-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="d922c-117">Reinicie Visual Studio y abra el archivo de solución que cerró en el paso anterior.</span><span class="sxs-lookup"><span data-stu-id="d922c-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="d922c-118">En la consola del administrador de paquetes (PMC), escriba el comando `Update-Database`:</span><span class="sxs-lookup"><span data-stu-id="d922c-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="d922c-119">Si recibe el siguiente error:</span><span class="sxs-lookup"><span data-stu-id="d922c-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="d922c-120">*El término ' Update-Database ' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable. Compruebe la ortografía del nombre, o bien, si se incluyó una ruta de acceso, compruebe que la ruta de acceso es correcta y vuelva a intentarlo.*</span><span class="sxs-lookup"><span data-stu-id="d922c-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="d922c-121">Salga y reinicie Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d922c-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="d922c-122">Cada migración se ejecutará y, a continuación, se ejecutará el método de inicialización.</span><span class="sxs-lookup"><span data-stu-id="d922c-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="d922c-123">Ahora puede ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d922c-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [<span data-ttu-id="d922c-124">Anterior</span><span class="sxs-lookup"><span data-stu-id="d922c-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
