---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Ejemplos de Katana | Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 01/17/2014
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 1238f7d09492a6856d49dece5de75184ccfa4838
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78472351"
---
# <a name="katana-samples"></a>Ejemplos de Katana

por [Microsoft](https://github.com/microsoft)

## <a name="katana-samples"></a>Ejemplos de Katana

[Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/AspNetRoutes) de | de **rutas de ASP.net de ejemplo**  
En algunas aplicaciones, querrá enlazar componentes OWIN en la tabla de rutas Asp.Net en paralelo con los componentes no OWIN. En este ejemplo se muestra cómo usar los métodos de extensión RouteCollection MapOwinPath y MapOwinRoute proporcionados por Microsoft. Owin. host. SystemWeb.

**Ejemplo de canalizaciones de bifurcación** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/BranchingPipelines)  
Las canalizaciones de procesamiento de solicitudes OWIN no necesitan ser lineales, sino que se pueden bifurcar para procesar solicitudes de diferentes maneras. En este ejemplo se muestra cómo construir una canalización de bifurcación basada en rutas de acceso de solicitud u otros datos de solicitud, como encabezados. Estos componentes están disponibles en el paquete Nuget Microsoft. Owin. Mapping.

**Ejemplo de servidor personalizado** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/CustomServer)   
Muestra cómo usar un servidor OWIN personalizado cuando se autohospeda OWIN.

[Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/Embedded) de **ejemplo | incrustado**  
Algunos servidores OWIN pueden ejecutarse dentro de su propio proceso (&quot;autohospedado&quot;). En este ejemplo se muestra cómo iniciar una aplicación OWIN con las herramientas proporcionadas por el paquete Nuget Microsoft. Owin. Hosting.

**Ejemplo HelloWorld** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorld)  
OWIN es una abstracción de API de servidor HTTP que permite la portabilidad de aplicaciones entre varios servidores. En este ejemplo se muestra cómo escribir una aplicación Hola mundo mediante algunos **contenedores simples** en torno a la abstracción OWIN sin procesar y ejecutarla en un servidor web como ASP.net.

Hola mundo el [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/HelloWorldRawOwin) del **ejemplo OWIN sin procesar** |   
En este ejemplo se muestra cómo escribir una aplicación Hola mundo con la abstracción OWIN **sin procesar** y ejecutarla en un servidor web como ASP.net.

[Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/SignalR) de **ejemplo de signalr** |   
Muestra cómo autohospedar Signalr mediante OWIN/Katana. Para obtener más información sobre Signalr autohospedado, consulte [Tutorial: Autohospedaje de signalr](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).

**Ejemplo de archivos estáticos** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/StaticFilesSample)   
Muestra cómo admitir solicitudes HTTP para archivos estáticos mediante OWIN/Katana.

[Código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebApi) de | de **API Web**   
En este ejemplo se muestra cómo hospedar OWIN en IIS y agregar la API Web a la canalización OWIN.

**Ejemplo de socket Web** | [código fuente](https://github.com/aspnet/samples/tree/master/samples/aspnet/Katana/WebSocketSample)   
Muestra cómo admitir Sockets Web en OWIN mediante la clase [System .net. WebSockets. WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) .
