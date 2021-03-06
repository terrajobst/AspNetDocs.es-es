---
uid: signalr/overview/older-versions/persistent-connection-authorization
title: Autenticación y autorización para las conexiones persistentes de Signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: En este tema se describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de la seguridad en una aplicación Signalr,...
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: c34bc627-41af-4c21-a817-e97a19a7f252
msc.legacyurl: /signalr/overview/older-versions/persistent-connection-authorization
msc.type: authoredcontent
ms.openlocfilehash: 9ccc59e3ea502daf12ce82382ab30ca73ca0f9b5
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431191"
---
# <a name="authentication-and-authorization-for-signalr-persistent-connections-signalr-1x"></a>Autenticación y autorización para las conexiones persistentes de SignalR (SignalR 1.x)

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tema se describe cómo aplicar la autorización en una conexión persistente. Para obtener información general sobre la integración de la seguridad en una aplicación de Signalr, consulte [Introducción a la seguridad](index.md).

## <a name="enforce-authorization"></a>Exigir autorización

Para aplicar reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , debe invalidar el método `AuthorizeRequest`. No se puede usar el atributo `Authorize` con conexiones persistentes. Signalr Framework llama al método `AuthorizeRequest` antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada. No se llama al método `AuthorizeRequest` desde el cliente; en su lugar, se autentica al usuario a través del mecanismo de autenticación estándar de la aplicación.

En el ejemplo siguiente se muestra cómo limitar las solicitudes a los usuarios autenticados.

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest. por ejemplo, comprobar si un usuario pertenece a un rol determinado.
