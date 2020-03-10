---
uid: web-api/overview/security/integrated-windows-authentication
title: Autenticación integrada de Windows | Microsoft Docs
author: MikeWasson
description: Describe el uso de la autenticación integrada de Windows en ASP.NET Web API.
ms.author: riande
ms.date: 12/18/2012
ms.assetid: 71ee4c78-c500-4d1c-b761-b4e161a291b5
msc.legacyurl: /web-api/overview/security/integrated-windows-authentication
msc.type: authoredcontent
ms.openlocfilehash: e4f31f191f3c0fabff308ea5dadb0f1d9ce7d448
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78504211"
---
# <a name="integrated-windows-authentication"></a><span data-ttu-id="c1df5-103">Autenticación integrada de Windows</span><span class="sxs-lookup"><span data-stu-id="c1df5-103">Integrated Windows Authentication</span></span>

<span data-ttu-id="c1df5-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c1df5-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="c1df5-105">La autenticación integrada de Windows permite a los usuarios iniciar sesión con sus credenciales de Windows, mediante Kerberos o NTLM.</span><span class="sxs-lookup"><span data-stu-id="c1df5-105">Integrated Windows authentication enables users to log in with their Windows credentials, using Kerberos or NTLM.</span></span> <span data-ttu-id="c1df5-106">El cliente envía las credenciales en el encabezado de autorización.</span><span class="sxs-lookup"><span data-stu-id="c1df5-106">The client sends credentials in the Authorization header.</span></span> <span data-ttu-id="c1df5-107">La autenticación de Windows es más adecuada para un entorno de intranet.</span><span class="sxs-lookup"><span data-stu-id="c1df5-107">Windows authentication is best suited for an intranet environment.</span></span> <span data-ttu-id="c1df5-108">Para obtener más información, vea [Autenticación de Windows](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span><span class="sxs-lookup"><span data-stu-id="c1df5-108">For more information, see [Windows Authentication](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication).</span></span>

| <span data-ttu-id="c1df5-109">Ventajas</span><span class="sxs-lookup"><span data-stu-id="c1df5-109">Advantages</span></span> | <span data-ttu-id="c1df5-110">Desventajas</span><span class="sxs-lookup"><span data-stu-id="c1df5-110">Disadvantages</span></span> |
| --- | --- |
| <span data-ttu-id="c1df5-111">-Integrado en IIS.</span><span class="sxs-lookup"><span data-stu-id="c1df5-111">- Built into IIS.</span></span> <span data-ttu-id="c1df5-112">: No envía las credenciales de usuario en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="c1df5-112">- Does not send the user credentials in the request.</span></span> <span data-ttu-id="c1df5-113">-Si el equipo cliente pertenece al dominio (por ejemplo, una aplicación de intranet), no es necesario que el usuario escriba las credenciales.</span><span class="sxs-lookup"><span data-stu-id="c1df5-113">- If the client computer belongs to the domain (for example, intranet application), the user does not need to enter credentials.</span></span> | <span data-ttu-id="c1df5-114">: No se recomienda para las aplicaciones de Internet.</span><span class="sxs-lookup"><span data-stu-id="c1df5-114">- Not recommended for Internet applications.</span></span> <span data-ttu-id="c1df5-115">-Requiere compatibilidad con Kerberos o NTLM en el cliente.</span><span class="sxs-lookup"><span data-stu-id="c1df5-115">- Requires Kerberos or NTLM support in the client.</span></span> <span data-ttu-id="c1df5-116">-El cliente debe estar en el dominio de Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c1df5-116">- Client must be in the Active Directory domain.</span></span> |

> [!NOTE]
> <span data-ttu-id="c1df5-117">Si la aplicación se hospeda en Azure y tiene un dominio de Active Directory local, considere la posibilidad de federar su AD local con Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="c1df5-117">If your application is hosted on Azure and you have an on-premise Active Directory domain, consider federating your on-premise AD with Azure Active Directory.</span></span> <span data-ttu-id="c1df5-118">De este modo, los usuarios pueden iniciar sesión con sus credenciales locales, pero la autenticación se realiza mediante Azure AD.</span><span class="sxs-lookup"><span data-stu-id="c1df5-118">That way, users can log in with their on-premise credentials, but the authentication is performed by Azure AD.</span></span> <span data-ttu-id="c1df5-119">Para obtener más información, consulte [autenticación de Azure](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="c1df5-119">For more information, see [Azure Authentication](../../../visual-studio/overview/2012/windows-azure-authentication.md).</span></span>

<span data-ttu-id="c1df5-120">Para crear una aplicación que use la autenticación integrada de Windows, seleccione la plantilla "aplicación de intranet" en el Asistente para proyectos de MVC 4.</span><span class="sxs-lookup"><span data-stu-id="c1df5-120">To create an application that uses Integrated Windows authentication, select the "Intranet Application" template in the MVC 4 project wizard.</span></span> <span data-ttu-id="c1df5-121">Esta plantilla de proyecto coloca la siguiente configuración en el archivo Web. config:</span><span class="sxs-lookup"><span data-stu-id="c1df5-121">This project template puts the following setting in the Web.config file:</span></span>

[!code-xml[Main](integrated-windows-authentication/samples/sample1.xml)]

<span data-ttu-id="c1df5-122">En el lado cliente, la autenticación integrada de Windows funciona con cualquier explorador que admita el esquema de autenticación [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , que incluye la mayoría de los exploradores principales.</span><span class="sxs-lookup"><span data-stu-id="c1df5-122">On the client side, Integrated Windows authentication works with any browser that supports the [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) authentication scheme, which includes most major browsers.</span></span> <span data-ttu-id="c1df5-123">En el caso de las aplicaciones cliente de .NET, la clase **HttpClient** admite la autenticación de Windows:</span><span class="sxs-lookup"><span data-stu-id="c1df5-123">For .NET client applications, the **HttpClient** class supports Windows authentication:</span></span>

[!code-csharp[Main](integrated-windows-authentication/samples/sample2.cs)]

<span data-ttu-id="c1df5-124">La autenticación de Windows es vulnerable a los ataques de falsificación de solicitud entre sitios (CSRF).</span><span class="sxs-lookup"><span data-stu-id="c1df5-124">Windows authentication is vulnerable to cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="c1df5-125">Consulte [prevención de ataques de falsificación de solicitud entre sitios (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="c1df5-125">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>
