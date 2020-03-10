---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: Use OWIN para autohospedar ASP.NET Web API ASP.NET 4. x
author: rick-anderson
description: Tutorial con código que muestra cómo hospedar ASP.NET Web API en una aplicación de consola.
ms.author: riande
ms.date: 07/09/2013
ms.custom: seoapril2019
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 872b931391a63ef82b96e5b264c070c0b5e9605d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78448333"
---
# <a name="use-owin-to-self-host-aspnet-web-api"></a>Usar OWIN para autohospedar ASP.NET Web API 

> En este tutorial se muestra cómo hospedar ASP.NET Web API en una aplicación de consola mediante OWIN para autohospedar el marco de la API Web.
>
> [Open Web Interface for .net](http://owin.org) (OWIN) define una abstracción entre los servidores Web .net y las aplicaciones Web. OWIN desacopla la aplicación web del servidor, lo que hace que OWIN sea ideal para hospedar automáticamente una aplicación web en su propio proceso, fuera de IIS.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
>
>
> - [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) 
> - API Web 5.2.7

> [!NOTE]
> Puede encontrar el código fuente completo de este tutorial en [github.com/ASPNET/samples](https://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/OwinSelfhostSample).

## <a name="create-a-console-application"></a>Creación de una aplicación de consola

En el menú **archivo** , haga clic en **nuevo**y seleccione **proyecto**. En **instalado**, en **Visual C#** , seleccione **escritorio de Windows** y, a continuación, seleccione **aplicación de consola (.NET Framework)** . Asigne al proyecto el nombre "OwinSelfhostSample" y seleccione **Aceptar**.

[![](use-owin-to-self-host-web-api/_static/image7.png)](use-owin-to-self-host-web-api/_static/image7.png)

## <a name="add-the-web-api-and-owin-packages"></a>Incorporación de la API Web y los paquetes OWIN

En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana Package Manager Console, escriba el siguiente comando:

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

Se instalará el paquete de WebAPI OWIN selfhost y todos los paquetes OWIN necesarios.

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Configuración de la API Web para el autohospedaje

En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase. Asigne a la clase el nombre `Startup`.

![](use-owin-to-self-host-web-api/_static/image5.png)

Reemplace todo el código reutilizable de este archivo por lo siguiente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Incorporación de un controlador de API Web

A continuación, agregue una clase de controlador de API Web. En Explorador de soluciones, haga clic con el botón secundario en el proyecto y seleccione **agregar** / **clase** para agregar una nueva clase. Asigne a la clase el nombre `ValuesController`.

Reemplace todo el código reutilizable de este archivo por lo siguiente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-with-httpclient"></a>Inicio del host OWIN y solicitud con HttpClient

Reemplace todo el código reutilizable en el archivo Program.cs por lo siguiente:

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="run-the-application"></a>Ejecutar la aplicación

Para ejecutar la aplicación, presione F5 en Visual Studio. La salida debe tener un aspecto parecido al siguiente:

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>Recursos adicionales

[Información general del proyecto Katana](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[ASP.NET Web API host en un rol de trabajo de Azure](host-aspnet-web-api-in-an-azure-worker-role.md)
