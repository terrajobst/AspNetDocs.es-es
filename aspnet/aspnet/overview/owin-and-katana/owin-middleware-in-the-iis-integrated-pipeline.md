---
uid: aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
title: Middleware de OWIN en la canalización integrada de IIS | Microsoft Docs
author: Praburaj
description: En este artículo se muestra cómo ejecutar componentes de middleware OWIN (OMCs) en la canalización integrada de IIS y cómo establecer el evento de canalización en el que se ejecuta un OMC. Debe...
ms.author: riande
ms.date: 11/07/2013
ms.assetid: d031c021-33c2-45a5-bf9f-98f8fa78c2ab
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-middleware-in-the-iis-integrated-pipeline
msc.type: authoredcontent
ms.openlocfilehash: 7d157fb6bd9e2ae9b55af41ef06c1eb5e6310ce1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78500263"
---
# <a name="owin-middleware-in-the-iis-integrated-pipeline"></a>Middleware de OWIN en la canalización integrada de IIS

por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://twitter.com/RickAndMSFT)

> En este artículo se muestra cómo ejecutar componentes de middleware OWIN (OMCs) en la canalización integrada de IIS y cómo establecer el evento de canalización en el que se ejecuta un OMC. Antes de leer este tutorial, debe revisar una introducción a la detección de la clase [de inicio de Project Katana](an-overview-of-project-katana.md) y [OWIN](owin-startup-class-detection.md) . Este tutorial fue escrito por Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Chris Ross, Praburaj Thiagarajan y Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).

Aunque los componentes de middleware [OWIN](an-overview-of-project-katana.md) (OMCs) están diseñados principalmente para ejecutarse en una canalización independiente del servidor, es posible ejecutar un OMC también en la canalización integrada de IIS ( ***no* se admite el modo clásico**). Se puede realizar una OMC para trabajar en la canalización integrada de IIS mediante la instalación del paquete siguiente desde la consola del administrador de paquetes (PMC):

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample1.cmd)]

Esto significa que todos los marcos de aplicaciones, incluso aquellos que todavía no se pueden ejecutar fuera de IIS y System. Web, pueden beneficiarse de los componentes de middleware OWIN existentes. 

> [!NOTE]
> Todos los paquetes de `Microsoft.Owin.Security.*` que se envían con el nuevo sistema de identidad en Visual Studio 2013 (por ejemplo: cookies, cuenta de Microsoft, Google, Facebook, Twitter, [token de portador](http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html), OAuth, servidor de autorización, JWT, Azure Active Directory y servicios de Federación de Active Directory) se crean como OMCs y se pueden usar en escenarios autohospedados y hospedados en IIS.

## <a name="how-owin-middleware-executes-in-the-iis-integrated-pipeline"></a>Cómo se ejecuta el middleware OWIN en la canalización integrada de IIS

En el caso de las aplicaciones de consola OWIN, la canalización de la aplicación compilada con la [configuración de inicio](owin-startup-class-detection.md) se establece mediante el orden en que se agregan los componentes mediante el método `IAppBuilder.Use`. Es decir, la canalización OWIN en el tiempo de ejecución de [Katana](an-overview-of-project-katana.md) procesará OMCs en el orden en el que se registraron mediante `IAppBuilder.Use`. En la canalización integrada de IIS, la canalización de solicitudes consta de [httpModules](https://msdn.microsoft.com/library/ms178468(v=vs.85).aspx) suscrito a un conjunto predefinido de eventos de canalización como [BeginRequest](https://msdn.microsoft.com/library/system.web.httpapplication.beginrequest.aspx), [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), [AuthorizeRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx), etc.

Si comparamos un OMC con el de un [HttpModule](https://msdn.microsoft.com/library/zec9k340(v=vs.85).aspx) en el mundo de ASP.net, se debe registrar un OMC en el evento de canalización predefinido correcto. Por ejemplo, el `MyModule` HttpModule se invocará cuando llegue una solicitud a la fase [AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx) de la canalización:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample2.cs?highlight=10)]

Para que un OMC participe en este mismo orden de ejecución basado en eventos, el código en tiempo de ejecución de [Katana](an-overview-of-project-katana.md) examina la [configuración de inicio](owin-startup-class-detection.md) y suscribe cada uno de los componentes de middleware a un evento de canalización integrada. Por ejemplo, el siguiente código de registro y OMC le permite ver el registro de eventos predeterminado de los componentes de middleware. (Para obtener instrucciones más detalladas sobre cómo crear una clase de inicio de OWIN, vea [detección de clases de inicio de Owin](owin-startup-class-detection.md)).

1. Cree un proyecto de aplicación Web vacío y asígnele el nombre **owin2**.
2. En la consola del administrador de paquetes (PMC), ejecute el siguiente comando: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample3.cmd)]
3. Agregue un `OWIN Startup Class` y asígnele el nombre `Startup`. Reemplace el código generado por lo siguiente (se resaltan los cambios):  

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample4.cs?highlight=5-7,15-36)]
4. Presione F5 para ejecutar la aplicación.

La configuración de inicio configura una canalización con tres componentes de middleware, los dos primeros muestran información de diagnóstico y el último que responde a los eventos (y también muestra información de diagnóstico). El método `PrintCurrentIntegratedPipelineStage` muestra el evento de canalización integrada en el que se invoca a este middleware y un mensaje. Las ventanas de salida muestran lo siguiente:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample5.cmd)]

El tiempo de ejecución de Katana asignó cada uno de los componentes de middleware OWIN a [PreExecuteRequestHandler](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx) de forma predeterminada, que corresponde al evento de canalización de IIS [PreRequestHandlerExecute](https://msdn.microsoft.com/library/system.web.httpapplication.prerequesthandlerexecute.aspx).

## <a name="stage-markers"></a>Marcadores de fase

Puede marcar OMCs para que se ejecute en fases específicas de la canalización mediante el método de extensión `IAppBuilder UseStageMarker()`. Para ejecutar un conjunto de componentes de middleware durante una fase determinada, inserte un marcador de fase justo después de que el último componente sea el establecido durante el registro. Hay reglas en qué fase de la canalización se puede ejecutar el software intermedio y se deben ejecutar los componentes de pedido (las reglas se explican más adelante en el tutorial). Agregue el método `UseStageMarker` al código de `Configuration` como se muestra a continuación:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample6.cs?highlight=13,19)]

La llamada `app.UseStageMarker(PipelineStage.Authenticate)` configura todos los componentes de middleware previamente registrados (en este caso, nuestros dos componentes de diagnóstico) para que se ejecuten en la fase de autenticación de la canalización. El último componente middleware (que muestra diagnósticos y responde a las solicitudes) se ejecutará en la `ResolveCache` fase (el evento [ResolveRequestCache](https://msdn.microsoft.com/library/system.web.httpapplication.resolverequestcache.aspx) ).

Presione F5 para ejecutar la aplicación. La ventana salida muestra lo siguiente:

[!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample7.cmd)]

## <a name="stage-marker-rules"></a>Reglas de marcador de fase

Los componentes de middleware Owin (OMC) se pueden configurar para que se ejecuten en los siguientes eventos de fase de canalización OWIN:

[!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample8.cs)]

1. De forma predeterminada, OMCs se ejecuta en el último evento (`PreHandlerExecute`). Este es el motivo por el que el primer código de ejemplo muestra "PreExecuteRequestHandler".
2. Puede usar el método a `app.UseStageMarker` para registrar un OMC para que se ejecute antes, en cualquier fase de la canalización OWIN indicada en la enumeración `PipelineStage`.
3. La canalización OWIN y la canalización IIS están ordenadas, por lo que las llamadas a `app.UseStageMarker` deben estar en orden. No se puede establecer el controlador de eventos en un evento que precede al último evento registrado en para `app.UseStageMarker`. Por ejemplo, *después* de llamar a:

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample9.cmd)]

   no se atenderán las llamadas a `app.UseStageMarker` que pasen `Authenticate` o `PostAuthenticate`, y no se producirá ninguna excepción. OMCs se ejecuta en la fase más reciente, que de forma predeterminada es `PreHandlerExecute`. Los marcadores de fase se usan para que se ejecuten antes. Si especifica marcadores de fase fuera de orden, se redondea al marcador anterior. En otras palabras, la adición de un marcador de fase indica "ejecución no posterior a la fase X". OMC se ejecuta en el marcador de fase más antiguo que se agregó después de ellos en la canalización OWIN.
4. La primera fase de llamadas a `app.UseStageMarker` WINS. Por ejemplo, si cambia el orden de las llamadas `app.UseStageMarker` del ejemplo anterior:

    [!code-csharp[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample10.cs?highlight=13,19)]

   Se mostrará la ventana de salida: 

    [!code-console[Main](owin-middleware-in-the-iis-integrated-pipeline/samples/sample11.cmd)]

   El OMCs se ejecuta en la fase `AuthenticateRequest`, porque el último OMC registrado con el evento `Authenticate` y el evento `Authenticate` precede a todos los demás eventos.
