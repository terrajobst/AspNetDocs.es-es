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
# <a name="authentication-and-authorization-for-signalr-persistent-connections"></a><span data-ttu-id="c84e2-104">Autenticación y autorización para las conexiones persistentes de SignalR</span><span class="sxs-lookup"><span data-stu-id="c84e2-104">Authentication and Authorization for SignalR Persistent Connections</span></span>

<span data-ttu-id="c84e2-105">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="c84e2-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="c84e2-106">En este tema se describe cómo aplicar la autorización en una conexión persistente.</span><span class="sxs-lookup"><span data-stu-id="c84e2-106">This topic describes how to enforce authorization on a persistent connection.</span></span> <span data-ttu-id="c84e2-107">Para obtener información general sobre la integración de la seguridad en una aplicación de Signalr, consulte [Introducción a la seguridad](introduction-to-security.md).</span><span class="sxs-lookup"><span data-stu-id="c84e2-107">For general information about integrating security into a SignalR application, see [Introduction to Security](introduction-to-security.md).</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="c84e2-108">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="c84e2-108">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="c84e2-109">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="c84e2-109">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="c84e2-110">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="c84e2-110">.NET 4.5</span></span>
> - <span data-ttu-id="c84e2-111">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="c84e2-111">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="c84e2-112">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="c84e2-112">Previous versions of this topic</span></span>
>
> <span data-ttu-id="c84e2-113">Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="c84e2-113">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="c84e2-114">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="c84e2-114">Questions and comments</span></span>
>
> <span data-ttu-id="c84e2-115">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="c84e2-115">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="c84e2-116">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="c84e2-116">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="enforce-authorization"></a><span data-ttu-id="c84e2-117">Exigir autorización</span><span class="sxs-lookup"><span data-stu-id="c84e2-117">Enforce authorization</span></span>

<span data-ttu-id="c84e2-118">Para aplicar reglas de autorización cuando se usa un [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) , debe invalidar el método `AuthorizeRequest`.</span><span class="sxs-lookup"><span data-stu-id="c84e2-118">To enforce authorization rules when using a [PersistentConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.persistentconnection(v=vs.111).aspx) you must override the `AuthorizeRequest` method.</span></span> <span data-ttu-id="c84e2-119">No se puede usar el atributo `Authorize` con conexiones persistentes.</span><span class="sxs-lookup"><span data-stu-id="c84e2-119">You cannot use the `Authorize` attribute with persistent connections.</span></span> <span data-ttu-id="c84e2-120">Signalr Framework llama al método `AuthorizeRequest` antes de cada solicitud para comprobar que el usuario está autorizado para realizar la acción solicitada.</span><span class="sxs-lookup"><span data-stu-id="c84e2-120">The `AuthorizeRequest` method is called by the SignalR Framework before every request to verify that the user is authorized to perform the requested action.</span></span> <span data-ttu-id="c84e2-121">No se llama al método `AuthorizeRequest` desde el cliente; en su lugar, se autentica al usuario a través del mecanismo de autenticación estándar de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c84e2-121">The `AuthorizeRequest` method is not called from the client; instead, you authenticate the user through your application's standard authentication mechanism.</span></span>

<span data-ttu-id="c84e2-122">En el ejemplo siguiente se muestra cómo limitar las solicitudes a los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="c84e2-122">The example below shows how to limit requests to authenticated users.</span></span>

[!code-csharp[Main](persistent-connection-authorization/samples/sample1.cs)]

<span data-ttu-id="c84e2-123">Puede agregar cualquier lógica de autorización personalizada en el método AuthorizeRequest. por ejemplo, comprobar si un usuario pertenece a un rol determinado.</span><span class="sxs-lookup"><span data-stu-id="c84e2-123">You can add any customized authorization logic in the AuthorizeRequest method; such as, checking whether a user belongs to a particular role.</span></span>
