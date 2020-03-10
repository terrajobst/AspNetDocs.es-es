---
uid: web-api/overview/security/basic-authentication
title: Autenticación básica en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Describe el uso de la autenticación básica en ASP.NET Web API.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447637"
---
# <a name="basic-authentication-in-aspnet-web-api"></a><span data-ttu-id="1a434-103">Autenticación básica en ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="1a434-103">Basic Authentication in ASP.NET Web API</span></span>

<span data-ttu-id="1a434-104">por [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="1a434-104">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="1a434-105">La autenticación básica se define en [RFC 2617, autenticación http: autenticación básica y de acceso implícita](http://www.ietf.org/rfc/rfc2617.txt).</span><span class="sxs-lookup"><span data-stu-id="1a434-105">Basic authentication is defined in [RFC 2617, HTTP Authentication: Basic and Digest Access Authentication](http://www.ietf.org/rfc/rfc2617.txt).</span></span>

<span data-ttu-id="1a434-106">Desventajas</span><span class="sxs-lookup"><span data-stu-id="1a434-106">Disadvantages</span></span>

- <span data-ttu-id="1a434-107">Las credenciales de usuario se envían en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1a434-107">User credentials are sent in the request.</span></span>
- <span data-ttu-id="1a434-108">Las credenciales se envían como texto sin formato.</span><span class="sxs-lookup"><span data-stu-id="1a434-108">Credentials are sent as plaintext.</span></span>
- <span data-ttu-id="1a434-109">Las credenciales se envían con cada solicitud.</span><span class="sxs-lookup"><span data-stu-id="1a434-109">Credentials are sent with every request.</span></span>
- <span data-ttu-id="1a434-110">No es posible cerrar sesión, excepto al finalizar la sesión del explorador.</span><span class="sxs-lookup"><span data-stu-id="1a434-110">No way to log out, except by ending the browser session.</span></span>
- <span data-ttu-id="1a434-111">Vulnerable a la falsificación de solicitudes entre sitios (CSRF); requiere medidas anti-CSRF.</span><span class="sxs-lookup"><span data-stu-id="1a434-111">Vulnerable to cross-site request forgery (CSRF); requires anti-CSRF measures.</span></span>

<span data-ttu-id="1a434-112">Ventajas</span><span class="sxs-lookup"><span data-stu-id="1a434-112">Advantages</span></span>

- <span data-ttu-id="1a434-113">Estándar de Internet.</span><span class="sxs-lookup"><span data-stu-id="1a434-113">Internet standard.</span></span>
- <span data-ttu-id="1a434-114">Compatible con todos los exploradores principales.</span><span class="sxs-lookup"><span data-stu-id="1a434-114">Supported by all major browsers.</span></span>
- <span data-ttu-id="1a434-115">Protocolo relativamente simple.</span><span class="sxs-lookup"><span data-stu-id="1a434-115">Relatively simple protocol.</span></span>

<span data-ttu-id="1a434-116">La autenticación básica funciona de la siguiente manera:</span><span class="sxs-lookup"><span data-stu-id="1a434-116">Basic authentication works as follows:</span></span>

1. <span data-ttu-id="1a434-117">Si una solicitud requiere autenticación, el servidor devuelve 401 (no autorizado).</span><span class="sxs-lookup"><span data-stu-id="1a434-117">If a request requires authentication, the server returns 401 (Unauthorized).</span></span> <span data-ttu-id="1a434-118">La respuesta incluye un encabezado WWW-Authenticate, que indica que el servidor admite la autenticación básica.</span><span class="sxs-lookup"><span data-stu-id="1a434-118">The response includes a WWW-Authenticate header, indicating the server supports Basic authentication.</span></span>
2. <span data-ttu-id="1a434-119">El cliente envía otra solicitud, con las credenciales del cliente en el encabezado de autorización.</span><span class="sxs-lookup"><span data-stu-id="1a434-119">The client sends another request, with the client credentials in the Authorization header.</span></span> <span data-ttu-id="1a434-120">Las credenciales tienen el formato de cadena "nombre: contraseña", codificada en Base64.</span><span class="sxs-lookup"><span data-stu-id="1a434-120">The credentials are formatted as the string "name:password", base64-encoded.</span></span> <span data-ttu-id="1a434-121">Las credenciales no se cifran.</span><span class="sxs-lookup"><span data-stu-id="1a434-121">The credentials are not encrypted.</span></span>

<span data-ttu-id="1a434-122">La autenticación básica se realiza en el contexto de un "dominio".</span><span class="sxs-lookup"><span data-stu-id="1a434-122">Basic authentication is performed within the context of a "realm."</span></span> <span data-ttu-id="1a434-123">El servidor incluye el nombre del dominio Kerberos en el encabezado WWW-Authenticate.</span><span class="sxs-lookup"><span data-stu-id="1a434-123">The server includes the name of the realm in the WWW-Authenticate header.</span></span> <span data-ttu-id="1a434-124">Las credenciales del usuario son válidas dentro de ese territorio.</span><span class="sxs-lookup"><span data-stu-id="1a434-124">The user's credentials are valid within that realm.</span></span> <span data-ttu-id="1a434-125">El servidor define el ámbito exacto de un dominio Kerberos.</span><span class="sxs-lookup"><span data-stu-id="1a434-125">The exact scope of a realm is defined by the server.</span></span> <span data-ttu-id="1a434-126">Por ejemplo, puede definir varios dominios para particionar los recursos.</span><span class="sxs-lookup"><span data-stu-id="1a434-126">For example, you might define several realms in order to partition resources.</span></span>

![](basic-authentication/_static/image1.png)

<span data-ttu-id="1a434-127">Dado que las credenciales se envían sin cifrar, la autenticación básica solo es segura a través de HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1a434-127">Because the credentials are sent unencrypted, Basic authentication is only secure over HTTPS.</span></span> <span data-ttu-id="1a434-128">Consulte [trabajar con SSL en Web API](working-with-ssl-in-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="1a434-128">See [Working with SSL in Web API](working-with-ssl-in-web-api.md).</span></span>

<span data-ttu-id="1a434-129">La autenticación básica también es vulnerable a los ataques CSRF.</span><span class="sxs-lookup"><span data-stu-id="1a434-129">Basic authentication is also vulnerable to CSRF attacks.</span></span> <span data-ttu-id="1a434-130">Después de que el usuario escriba las credenciales, el explorador las enviará automáticamente en las solicitudes posteriores al mismo dominio, mientras dure la sesión.</span><span class="sxs-lookup"><span data-stu-id="1a434-130">After the user enters credentials, the browser automatically sends them on subsequent requests to the same domain, for the duration of the session.</span></span> <span data-ttu-id="1a434-131">Esto incluye las solicitudes AJAX.</span><span class="sxs-lookup"><span data-stu-id="1a434-131">This includes AJAX requests.</span></span> <span data-ttu-id="1a434-132">Consulte [prevención de ataques de falsificación de solicitud entre sitios (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).</span><span class="sxs-lookup"><span data-stu-id="1a434-132">See [Preventing Cross-Site Request Forgery (CSRF) Attacks](preventing-cross-site-request-forgery-csrf-attacks.md).</span></span>

## <a name="basic-authentication-with-iis"></a><span data-ttu-id="1a434-133">Autenticación básica con IIS</span><span class="sxs-lookup"><span data-stu-id="1a434-133">Basic Authentication with IIS</span></span>

<span data-ttu-id="1a434-134">IIS admite la autenticación básica, pero hay una advertencia: el usuario se autentica con sus credenciales de Windows.</span><span class="sxs-lookup"><span data-stu-id="1a434-134">IIS supports Basic authentication, but there is a caveat: The user is authenticated against their Windows credentials.</span></span> <span data-ttu-id="1a434-135">Esto significa que el usuario debe tener una cuenta en el dominio del servidor.</span><span class="sxs-lookup"><span data-stu-id="1a434-135">That means the user must have an account on the server's domain.</span></span> <span data-ttu-id="1a434-136">Para un sitio web de acceso público, normalmente desea autenticarse en un proveedor de pertenencia a ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="1a434-136">For a public-facing web site, you typically want to authenticate against an ASP.NET membership provider.</span></span>

<span data-ttu-id="1a434-137">Para habilitar la autenticación básica mediante IIS, establezca el modo de autenticación en "Windows" en el archivo Web. config del proyecto ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="1a434-137">To enable Basic authentication using IIS, set the authentication mode to "Windows" in the Web.config of your ASP.NET project:</span></span>

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

<span data-ttu-id="1a434-138">En este modo, IIS usa las credenciales de Windows para la autenticación.</span><span class="sxs-lookup"><span data-stu-id="1a434-138">In this mode, IIS uses Windows credentials to authenticate.</span></span> <span data-ttu-id="1a434-139">Además, debe habilitar la autenticación básica en IIS.</span><span class="sxs-lookup"><span data-stu-id="1a434-139">In addition, you must enable Basic authentication in IIS.</span></span> <span data-ttu-id="1a434-140">En el administrador de IIS, vaya a la vista características, seleccione autenticación y habilite la autenticación básica.</span><span class="sxs-lookup"><span data-stu-id="1a434-140">In IIS Manager, go to Features View, select Authentication, and enable Basic authentication.</span></span>

![](basic-authentication/_static/image2.png)

<span data-ttu-id="1a434-141">En el proyecto de API Web, agregue el atributo `[Authorize]` para las acciones de controlador que necesiten autenticación.</span><span class="sxs-lookup"><span data-stu-id="1a434-141">In your Web API project, add the `[Authorize]` attribute for any controller actions that need authentication.</span></span>

<span data-ttu-id="1a434-142">Un cliente se autentica a sí mismo estableciendo el encabezado de autorización en la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1a434-142">A client authenticates itself by setting the Authorization header in the request.</span></span> <span data-ttu-id="1a434-143">Los clientes del explorador realizan este paso automáticamente.</span><span class="sxs-lookup"><span data-stu-id="1a434-143">Browser clients perform this step automatically.</span></span> <span data-ttu-id="1a434-144">Los clientes que no son de explorador deberán establecer el encabezado.</span><span class="sxs-lookup"><span data-stu-id="1a434-144">Nonbrowser clients will need to set the header.</span></span>

## <a name="basic-authentication-with-custom-membership"></a><span data-ttu-id="1a434-145">Autenticación básica con pertenencia personalizada</span><span class="sxs-lookup"><span data-stu-id="1a434-145">Basic Authentication with Custom Membership</span></span>

<span data-ttu-id="1a434-146">Como se mencionó, la autenticación básica integrada en IIS utiliza las credenciales de Windows.</span><span class="sxs-lookup"><span data-stu-id="1a434-146">As mentioned, the Basic Authentication built into IIS uses Windows credentials.</span></span> <span data-ttu-id="1a434-147">Esto significa que necesita crear cuentas para los usuarios en el servidor de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="1a434-147">That means you need to create accounts for your users on the hosting server.</span></span> <span data-ttu-id="1a434-148">Pero para una aplicación de Internet, las cuentas de usuario suelen almacenarse en una base de datos externa.</span><span class="sxs-lookup"><span data-stu-id="1a434-148">But for an internet application, user accounts are typically stored in an external database.</span></span>

<span data-ttu-id="1a434-149">En el código siguiente se describe cómo un módulo HTTP que realiza la autenticación básica.</span><span class="sxs-lookup"><span data-stu-id="1a434-149">The following code how an HTTP module that performs Basic Authentication.</span></span> <span data-ttu-id="1a434-150">Puede conectar fácilmente un proveedor de pertenencia a ASP.NET reemplazando el método `CheckPassword`, que es un método ficticio en este ejemplo.</span><span class="sxs-lookup"><span data-stu-id="1a434-150">You can easily plug in an ASP.NET membership provider by replacing the `CheckPassword` method, which is a dummy method in this example.</span></span>

<span data-ttu-id="1a434-151">En Web API 2, debería considerar la posibilidad de escribir un [filtro de autenticación](authentication-filters.md) o [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md), en lugar de un módulo http.</span><span class="sxs-lookup"><span data-stu-id="1a434-151">In Web API 2, you should consider writing an [authentication filter](authentication-filters.md) or [OWIN middleware](../../../aspnet/overview/owin-and-katana/index.md), instead of an HTTP module.</span></span>

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

<span data-ttu-id="1a434-152">Para habilitar el módulo HTTP, agregue lo siguiente al archivo Web. config en la sección **System. WebServer** :</span><span class="sxs-lookup"><span data-stu-id="1a434-152">To enable the HTTP module, add the following to your web.config file in the **system.webServer** section:</span></span>

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

<span data-ttu-id="1a434-153">Reemplace "YourAssemblyName" por el nombre del ensamblado (sin incluir la extensión "dll").</span><span class="sxs-lookup"><span data-stu-id="1a434-153">Replace "YourAssemblyName" with the name of the assembly (not including the "dll" extension).</span></span>

<span data-ttu-id="1a434-154">Debe deshabilitar otros esquemas de autenticación, como formularios o la autenticación de Windows.</span><span class="sxs-lookup"><span data-stu-id="1a434-154">You should disable other authentication schemes, such as Forms or Windows auth.</span></span>
