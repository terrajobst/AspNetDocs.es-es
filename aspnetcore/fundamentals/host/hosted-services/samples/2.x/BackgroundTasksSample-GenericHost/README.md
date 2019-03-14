---
ms.openlocfilehash: 6b2bc386ec179e786de205af0ca6dbd610e000d9
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/01/2019
ms.locfileid: "57056492"
---
# <a name="aspnet-core-background-tasks-sample-generic-host"></a><span data-ttu-id="641ae-101">Ejemplo de tareas en segundo plano de ASP.NET Core (host genérico)</span><span class="sxs-lookup"><span data-stu-id="641ae-101">ASP.NET Core Background Tasks Sample (Generic Host)</span></span>

<span data-ttu-id="641ae-102">En este ejemplo se muestra el uso de [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="641ae-102">This sample illustrates the use of [IHostedService](https://docs.microsoft.com/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span></span> <span data-ttu-id="641ae-103">En este ejemplo se muestran las características descritas en el tema [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) (Tareas en segundo plano con servicios hospedados en ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="641ae-103">This sample demonstrates the features described in the [Background tasks with hosted services in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="641ae-104">Al ejecutar el ejemplo en [Visual Studio Code](https://code.visualstudio.com/), establezca el valor de **console** de la configuración de la consola en *.vscode/launch.json* como `externalTerminal` o `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="641ae-104">When running the sample in [Visual Studio Code](https://code.visualstudio.com/), set the **console** value of the console configuration in *.vscode/launch.json* to either `externalTerminal` or `integratedTerminal`.</span></span> <span data-ttu-id="641ae-105">El uso de la `internalConsole` no es compatible con la entrada de pulsación de tecla de la consola que utiliza la aplicación para poner en cola elementos de trabajo en segundo plano.</span><span class="sxs-lookup"><span data-stu-id="641ae-105">Use of the `internalConsole` is incompatible with console keystroke input that the app uses to enqueue background work items.</span></span>
