---
uid: signalr/overview/security/persistent-connection-authorization
title: Autenticación y autorización para las conexiones persistentes de Signalr | Microsoft Docs
author: bradygaster
description: En este tema se describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de la seguridad en una aplicación Signalr,...
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: e264677b-9c01-47ec-94f9-3cd8f08f94af
msc.legacyurl: /signalr/overview/security/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 7e69d3c1a18f1239142891734ba58cd2b0078f84
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467479"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a>Autenticación y autorización para las conexiones persistentes de SignalR

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tema se describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de la seguridad en una aplicación de Signalr, consulte [Introducción a la seguridad](introduction-to-security.md).
>
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software utilizadas en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Signalr versión 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
>
> Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).

## <a name="enforce-authorization"></a>Exigir autorización

Para aplicar reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , debe invalidar el método `AuthorizeRequest`. No se puede usar el atributo `Authorize` con conexiones persistentes. Signalr Framework llama al método `AuthorizeRequest` antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada. No se llama al método `AuthorizeRequest` desde el cliente; en su lugar, se autentica al usuario a través del mecanismo de autenticación estándar de la aplicación.

En el ejemplo siguiente se muestra cómo limitar las solicitudes a los usuarios autenticados.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest. por ejemplo, comprobar si un usuario pertenece a un rol determinado.
