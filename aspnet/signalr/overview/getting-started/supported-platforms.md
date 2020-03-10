---
uid: signalr/overview/getting-started/supported-platforms
title: Plataformas admitidas | Microsoft Docs
author: bradygaster
description: En este artículo se describe qué clientes y servidores son compatibles con Signalr.
ms.author: bradyg
ms.date: 04/18/2018
ms.assetid: eac31beb-0f46-4afa-9def-e80904dea4f0
msc.legacyurl: /signalr/overview/getting-started/supported-platforms
msc.type: authoredcontent
ms.openlocfilehash: 89730e591bb94022d16fe1a78a4350b38e0bf7a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450127"
---
# <a name="supported-platforms"></a>Plataformas compatibles

por [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se describe qué clientes y servidores son compatibles con Signalr. 
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

Signalr es compatible con una variedad de configuraciones de servidor y cliente. Además, cada opción de transporte tiene un conjunto de requisitos propios; Si los requisitos del sistema para un transporte no están disponibles, Signalr realizará correctamente la conmutación por error a otros transportes. Para más información sobre los transportes compatibles con Signalr, consulte [transportes y reservas](introduction-to-signalr.md#transports).

## <a name="server-system-requirements"></a>Requisitos del sistema de servidor de

El componente de servidor de Signalr se puede hospedar en una variedad de configuraciones de servidor. En esta sección se describen las versiones admitidas de los sistemas operativos, .NET Framework, Internet Information Server y otros componentes.

### <a name="supported-server-operating-systems"></a>Sistemas operativos de servidor admitidos

El componente de servidor de Signalr se puede hospedar en los siguientes sistemas operativos de servidor o cliente. Tenga en cuenta que para que Signalr utilice WebSockets, se necesita Windows Server 2012, Windows Server 2016 o Windows 8 (WebSocket se puede usar en sitios web de Windows Azure, siempre y cuando la versión de .NET Framework del sitio esté establecida en 4,5 y los sockets Web estén habilitados en el Página de configuración).

- Windows Server 2016
- Windows Server 2012
- Windows Server 2008 r2
- Windows 10
- Windows 8
- Windows 7
- Microsoft Azure

### <a name="supported-server-net-framework-version"></a>Versión de .NET Framework de servidor admitida

Signalr 2 solo se admite en .NET Framework 4,5. Consulte la sección [actualizaciones recomendadas](#updates) para ver las actualizaciones que mejoran la confiabilidad, la compatibilidad, la estabilidad y el rendimiento.

### <a name="supported-server-iis-versions"></a>Versiones de IIS de servidor admitidas

Cuando Signalr se hospeda en IIS, se admiten las siguientes versiones. Tenga en cuenta que si se usa un sistema operativo de cliente, como para el desarrollo (Windows 8 o Windows 7), no se deben usar las versiones completas de IIS o Cassini, ya que habrá un límite de 10 conexiones simultáneas impuestas, que se alcanzarán muy rápidamente desde las conexiones son transitorios, que se restablecen con frecuencia y no se eliminan inmediatamente cuando ya no se usan. IIS Express debe usarse en los sistemas operativos cliente.

Tenga en cuenta también que para que Signalr utilice WebSocket, se debe usar IIS 8 o IIS 8 Express, el servidor debe usar Windows 8, Windows Server 2012 o posterior, y WebSocket debe estar habilitado en IIS. Para obtener información sobre cómo habilitar WebSocket en IIS, consulte [compatibilidad con el protocolo WebSocket de iis 8,0](https://www.iis.net/learn/get-started/whats-new-in-iis-8/iis-80-websocket-protocol-support).

- IIS 8 o IIS 8 Express.
- IIS 7 y 7,5. Se requiere compatibilidad con [direcciones URL sin extensión](https://support.microsoft.com/kb/980368) .
- IIS debe ejecutarse en modo integrado; no se admite el modo clásico. Es posible que se produzcan retrasos de mensajes de hasta 30 segundos si IIS se ejecuta en modo clásico mediante el transporte de eventos enviados por el servidor.
- La aplicación de hospedaje debe ejecutarse en modo de plena confianza.

## <a name="client-system-requirements"></a>Requisitos del sistema de cliente de

Signalr se puede usar en una variedad de plataformas de cliente. En esta sección se describen los requisitos del sistema para usar Signalr en exploradores Web, aplicaciones de escritorio de Windows, aplicaciones de Silverlight y dispositivos móviles.

### <a name="web-browsers"></a>Exploradores web

Signalr se puede usar en varios exploradores Web, pero normalmente solo se admiten las dos últimas versiones.

Las aplicaciones que usan Signalr en exploradores deben usar jQuery version 1.6.4 o versiones posteriores (como 1.7.2, 1.8.2 o 1.9.1).

Signalr se puede usar en los siguientes exploradores:

- Microsoft Internet Explorer, versiones 8, 9, 10 y 11. Se admiten las versiones modernas, de escritorio y móviles.
- Mozilla Firefox: versión actual-1, ambas versiones de Windows y Mac.
- Google Chrome: versión actual-1, ambas versiones de Windows y Mac.
- Safari: versión actual-1, ambas versiones, Mac e iOS.
- Opera: versión actual: 1, solo Windows.
- Explorador Android

Además de requerir ciertos exploradores, los diversos transportes que Signalr utiliza tienen sus propios requisitos. Los transportes siguientes se admiten en las siguientes configuraciones:

<a id="browser"></a>

**Requisitos de transporte del explorador Web**

| Transporte | Internet Explorer | Chrome (Windows o iOS) | Firefox | Safari (OSX o iOS) | Android |
| --- | --- | --- | --- | --- | --- |
| WebSockets | 10+ | actual-1 | actual-1 | actual-1 | N/D |
| Eventos enviados por el servidor | N/D | actual-1 | actual-1 | actual-1 | N/D |
| ForeverFrame | 8+ | N/D | N/D | N/D | 4.1 |
| Sondeo largo | 8+ | actual-1 | actual-1 | actual-1 | 4.1 |

\*: 6 + necesario para obtener una funcionalidad completa.

#### <a name="unsupported-browsers"></a>Exploradores no admitidos

Aunque Signalr *puede* ejecutarse sin problemas importantes en versiones anteriores del explorador, no se realiza una prueba activa del señalizador en ellos y, por lo general, no se corrigen los errores que pueden aparecer en ellos.

### <a name="windows-desktop-and-silverlight-applications"></a>Escritorio de Windows y aplicaciones de Silverlight

Además de ejecutarse en un explorador Web, Signalr se puede hospedar en aplicaciones de cliente o de Silverlight independientes de Windows. Las aplicaciones de escritorio de Windows y de Windows Signalr tienen los siguientes requisitos del sistema.

- Las aplicaciones que usan .NET 4 se admiten en Windows XP SP3 o posterior.
- Las aplicaciones que usan .NET Framework 4,5 se admiten en Windows Vista o posterior.

Además de los requisitos del sistema operativo y de .NET Framework, los transportes disponibles para Signalr tienen sus propios requisitos. Los transportes siguientes se admiten en las siguientes configuraciones:

**Requisitos de transporte de Windows y escritorio de Windows**

| Transporte | Aplicación .NET | Silverlight |
| --- | --- | --- |
| Web Sockets | Windows 8 + y .NET 4.5 + | N/D |
| Siempre marco | N/D | N/D |
| Eventos enviados por el servidor | .NET 4 + | 5+ |
| Sondeo largo | .NET 4 + | 5+ |

<a id="android"></a>

### <a name="windows-store-and-windows-phone-applications"></a>Aplicaciones de la tienda Windows y Windows Phone

Signalr se puede usar en aplicaciones de la tienda Windows y en aplicaciones de Windows Phone 8. Los transportes siguientes se admiten en las siguientes configuraciones:

**Requisitos de transporte de la tienda Windows y de Windows Phone**

| Transporte | Tienda Windows/.NET | Tienda Windows/JavaScript | Windows Phone/IE | Windows Phone/.NET |
| --- | --- | --- | --- | --- |
| WebSockets | N/D | Win8 + | 8+ | N/D |
| Siempre marco | N/D | Win8 + | 7.5+ | N/D |
| Eventos enviados por el servidor | Win8 + | N/D | N/D | 8+ |
| Sondeo largo | Win8 + | Win8 + | 7.5+ | 8+ |

<a id="updates"></a>

## <a name="recommended-updates"></a>Actualizaciones recomendadas

Se recomiendan las siguientes actualizaciones para los servidores de Signalr:

- Hay disponible una actualización para .NET Framework 4,5 [aquí](https://support.microsoft.com/kb/2750149).
- Microsoft publicará periódicamente las QFE para ASP.NET. Deben aplicarse como disponibles.
