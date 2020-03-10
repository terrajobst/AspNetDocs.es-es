---
uid: signalr/overview/older-versions/hub-authorization
title: Autenticación y autorización de los concentradores de Signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: En este tema se describe cómo restringir qué usuarios o roles pueden acceder a los métodos de concentrador.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 3d2dfc0e-eac2-4076-a468-325d3d01cc7b
msc.legacyurl: /signalr/overview/older-versions/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 8182677c8931f060d98d17008b16ad545bee4e69
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78450043"
---
# <a name="authentication-and-authorization-for-signalr-hubs-signalr-1x"></a><span data-ttu-id="bfa49-103">Autenticación y autorización de los concentradores de SignalR (SignalR 1.x)</span><span class="sxs-lookup"><span data-stu-id="bfa49-103">Authentication and Authorization for SignalR Hubs (SignalR 1.x)</span></span>

<span data-ttu-id="bfa49-104">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bfa49-104">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="bfa49-105">En este tema se describe cómo restringir qué usuarios o roles pueden acceder a los métodos de concentrador.</span><span class="sxs-lookup"><span data-stu-id="bfa49-105">This topic describes how to restrict which users or roles can access hub methods.</span></span>

## <a name="overview"></a><span data-ttu-id="bfa49-106">Información general</span><span class="sxs-lookup"><span data-stu-id="bfa49-106">Overview</span></span>

<span data-ttu-id="bfa49-107">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="bfa49-107">This topic contains the following sections:</span></span>

- [<span data-ttu-id="bfa49-108">Authorize (atributo)</span><span class="sxs-lookup"><span data-stu-id="bfa49-108">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="bfa49-109">Requerir autenticación para todos los concentradores</span><span class="sxs-lookup"><span data-stu-id="bfa49-109">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="bfa49-110">Autorización personalizada</span><span class="sxs-lookup"><span data-stu-id="bfa49-110">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="bfa49-111">Pasar información de autenticación a los clientes</span><span class="sxs-lookup"><span data-stu-id="bfa49-111">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="bfa49-112">Opciones de autenticación para clientes de .NET</span><span class="sxs-lookup"><span data-stu-id="bfa49-112">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="bfa49-113">Cookie con autenticación de formularios</span><span class="sxs-lookup"><span data-stu-id="bfa49-113">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="bfa49-114">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="bfa49-114">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="bfa49-115">Encabezado de conexión</span><span class="sxs-lookup"><span data-stu-id="bfa49-115">Connection header</span></span>](#header)
    - [<span data-ttu-id="bfa49-116">Certificate</span><span class="sxs-lookup"><span data-stu-id="bfa49-116">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="bfa49-117">Authorize (atributo)</span><span class="sxs-lookup"><span data-stu-id="bfa49-117">Authorize attribute</span></span>

<span data-ttu-id="bfa49-118">Signalr proporciona el atributo [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) para especificar qué usuarios o roles tienen acceso a un concentrador o método.</span><span class="sxs-lookup"><span data-stu-id="bfa49-118">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="bfa49-119">Este atributo se encuentra en el espacio de nombres `Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="bfa49-119">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="bfa49-120">El atributo `Authorize` se aplica a un concentrador o a métodos concretos de un concentrador.</span><span class="sxs-lookup"><span data-stu-id="bfa49-120">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="bfa49-121">Al aplicar el atributo `Authorize` a una clase de concentrador, el requisito de autorización especificado se aplica a todos los métodos del concentrador.</span><span class="sxs-lookup"><span data-stu-id="bfa49-121">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="bfa49-122">A continuación se muestran los diferentes tipos de requisitos de autorización que se pueden aplicar.</span><span class="sxs-lookup"><span data-stu-id="bfa49-122">The different types of authorization requirements that you can apply are shown below.</span></span> <span data-ttu-id="bfa49-123">Sin el atributo `Authorize`, todos los métodos públicos del centro están disponibles para un cliente que está conectado al centro.</span><span class="sxs-lookup"><span data-stu-id="bfa49-123">Without the `Authorize` attribute, all public methods on the hub are available to a client that is connected to the hub.</span></span>

<span data-ttu-id="bfa49-124">Si ha definido un rol denominado "admin" en la aplicación Web, puede especificar que solo los usuarios de ese rol puedan acceder a un centro con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="bfa49-124">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="bfa49-125">O bien, puede especificar que un concentrador contenga un método que esté disponible para todos los usuarios y un segundo método que solo esté disponible para los usuarios autenticados, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="bfa49-125">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="bfa49-126">En los siguientes ejemplos se tratan diferentes escenarios de autorización:</span><span class="sxs-lookup"><span data-stu-id="bfa49-126">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="bfa49-127">`[Authorize]`: solo usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="bfa49-127">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="bfa49-128">`[Authorize(Roles = "Admin,Manager")]`: solo usuarios autenticados en los roles especificados</span><span class="sxs-lookup"><span data-stu-id="bfa49-128">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="bfa49-129">`[Authorize(Users = "user1,user2")]`: solo usuarios autenticados con los nombres de usuario especificados</span><span class="sxs-lookup"><span data-stu-id="bfa49-129">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="bfa49-130">`[Authorize(RequireOutgoing=false)]`: solo los usuarios autenticados pueden invocar el concentrador, pero las llamadas desde el servidor de vuelta a los clientes no están limitadas por la autorización, como, cuando solo determinados usuarios pueden enviar un mensaje, pero todos los demás pueden recibir el mensaje.</span><span class="sxs-lookup"><span data-stu-id="bfa49-130">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="bfa49-131">La propiedad RequireOutgoing solo se puede aplicar a todo el concentrador, no a los métodos particulares del centro.</span><span class="sxs-lookup"><span data-stu-id="bfa49-131">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="bfa49-132">Cuando RequireOutgoing no está establecido en false, solo se llama desde el servidor a los usuarios que cumplen el requisito de autorización.</span><span class="sxs-lookup"><span data-stu-id="bfa49-132">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="bfa49-133">Requerir autenticación para todos los concentradores</span><span class="sxs-lookup"><span data-stu-id="bfa49-133">Require authentication for all hubs</span></span>

<span data-ttu-id="bfa49-134">Puede requerir la autenticación para todos los concentradores y métodos de concentrador en la aplicación llamando al método [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bfa49-134">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="bfa49-135">Puede usar este método si tiene varios centros y desea aplicar un requisito de autenticación para todos ellos.</span><span class="sxs-lookup"><span data-stu-id="bfa49-135">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="bfa49-136">Con este método, no se puede especificar el rol, el usuario o la autorización de salida.</span><span class="sxs-lookup"><span data-stu-id="bfa49-136">With this method, you cannot specify role, user, or outgoing authorization.</span></span> <span data-ttu-id="bfa49-137">Solo puede especificar que el acceso a los métodos de concentrador esté restringido a los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="bfa49-137">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="bfa49-138">Sin embargo, todavía puede aplicar el atributo Authorize a los concentradores o métodos para especificar requisitos adicionales.</span><span class="sxs-lookup"><span data-stu-id="bfa49-138">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="bfa49-139">Cualquier requisito que especifique en los atributos se aplica además del requisito básico de autenticación.</span><span class="sxs-lookup"><span data-stu-id="bfa49-139">Any requirement you specify in attributes is applied in addition to the basic requirement of authentication.</span></span>

<span data-ttu-id="bfa49-140">En el ejemplo siguiente se muestra un archivo global. asax que restringe todos los métodos de concentrador a usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="bfa49-140">The following example shows a Global.asax file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="bfa49-141">Si llama al método `RequireAuthentication()` una vez que se ha procesado una solicitud de Signalr, Signalr producirá una excepción `InvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="bfa49-141">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="bfa49-142">Esta excepción se produce porque no se puede Agregar un módulo al HubPipeline una vez que se ha invocado la canalización.</span><span class="sxs-lookup"><span data-stu-id="bfa49-142">This exception is thrown because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="bfa49-143">En el ejemplo anterior se muestra la llamada al método `RequireAuthentication` en el método `Application_Start` que se ejecuta una vez antes de controlar la primera solicitud.</span><span class="sxs-lookup"><span data-stu-id="bfa49-143">The previous example shows calling the `RequireAuthentication` method in the `Application_Start` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="bfa49-144">Autorización personalizada</span><span class="sxs-lookup"><span data-stu-id="bfa49-144">Customized authorization</span></span>

<span data-ttu-id="bfa49-145">Si necesita personalizar el modo en que se determina la autorización, puede crear una clase que derive de `AuthorizeAttribute` e invalide el método [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="bfa49-145">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="bfa49-146">Se llama a este método para cada solicitud con el fin de determinar si el usuario está autorizado para completar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="bfa49-146">This method is called for each request to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="bfa49-147">En el método invalidado, se proporciona la lógica necesaria para el escenario de autorización.</span><span class="sxs-lookup"><span data-stu-id="bfa49-147">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="bfa49-148">En el ejemplo siguiente se muestra cómo aplicar la autorización a través de la identidad basada en notificaciones.</span><span class="sxs-lookup"><span data-stu-id="bfa49-148">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="bfa49-149">Pasar información de autenticación a los clientes</span><span class="sxs-lookup"><span data-stu-id="bfa49-149">Pass authentication information to clients</span></span>

<span data-ttu-id="bfa49-150">Es posible que necesite usar información de autenticación en el código que se ejecuta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="bfa49-150">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="bfa49-151">La información necesaria se pasa cuando se llama a los métodos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="bfa49-151">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="bfa49-152">Por ejemplo, un método de aplicación de chat podría pasar como parámetro el nombre de usuario de la persona que publica un mensaje, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="bfa49-152">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="bfa49-153">O bien, puede crear un objeto para representar la información de autenticación y pasar ese objeto como parámetro, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="bfa49-153">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="bfa49-154">Nunca debe pasar el identificador de conexión de un cliente a otros clientes, ya que un usuario malintencionado podría usarlo para imitar una solicitud de ese cliente.</span><span class="sxs-lookup"><span data-stu-id="bfa49-154">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="bfa49-155">Opciones de autenticación para clientes de .NET</span><span class="sxs-lookup"><span data-stu-id="bfa49-155">Authentication options for .NET clients</span></span>

<span data-ttu-id="bfa49-156">Si tiene un cliente de .NET, como una aplicación de consola, que interactúa con un centro limitado a los usuarios autenticados, puede pasar las credenciales de autenticación en una cookie, en el encabezado de la conexión o en un certificado.</span><span class="sxs-lookup"><span data-stu-id="bfa49-156">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="bfa49-157">En los ejemplos de esta sección se muestra cómo usar los distintos métodos para autenticar a un usuario.</span><span class="sxs-lookup"><span data-stu-id="bfa49-157">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="bfa49-158">No son aplicaciones de Signalr totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="bfa49-158">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="bfa49-159">Para obtener más información sobre los clientes de .NET con Signalr, consulte [Guía de la API de hubs: cliente .net](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="bfa49-159">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="bfa49-160">Cookie</span><span class="sxs-lookup"><span data-stu-id="bfa49-160">Cookie</span></span>

<span data-ttu-id="bfa49-161">Cuando el cliente .NET interactúa con un concentrador que usa la autenticación de formularios ASP.NET, deberá establecer manualmente la cookie de autenticación en la conexión.</span><span class="sxs-lookup"><span data-stu-id="bfa49-161">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="bfa49-162">Agregue la cookie a la propiedad `CookieContainer` en el objeto [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="bfa49-162">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="bfa49-163">En el ejemplo siguiente se muestra una aplicación de consola que recupera una cookie de autenticación de una página web y agrega esa cookie a la conexión.</span><span class="sxs-lookup"><span data-stu-id="bfa49-163">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span> <span data-ttu-id="bfa49-164">La dirección URL `https://www.contoso.com/RemoteLogin` en el ejemplo apunta a una página web que se debe crear.</span><span class="sxs-lookup"><span data-stu-id="bfa49-164">The URL `https://www.contoso.com/RemoteLogin` in the example points to a web page that you would need to create.</span></span> <span data-ttu-id="bfa49-165">La página recuperaría el nombre de usuario y la contraseña publicados e intentará iniciar la sesión del usuario con las credenciales.</span><span class="sxs-lookup"><span data-stu-id="bfa49-165">The page would retrieve the posted user name and password, and attempt to log in the user with the credentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="bfa49-166">La aplicación de consola envía las credenciales a www.contoso.com/RemoteLogin, que pueden hacer referencia a una página vacía que contiene el siguiente archivo de código subyacente.</span><span class="sxs-lookup"><span data-stu-id="bfa49-166">The console app posts the credentials to www.contoso.com/RemoteLogin which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="bfa49-167">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="bfa49-167">Windows authentication</span></span>

<span data-ttu-id="bfa49-168">Al usar la autenticación de Windows, puede pasar las credenciales del usuario actual mediante la propiedad [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) .</span><span class="sxs-lookup"><span data-stu-id="bfa49-168">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="bfa49-169">Establezca las credenciales para la conexión con el valor de DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="bfa49-169">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="bfa49-170">Encabezado de conexión</span><span class="sxs-lookup"><span data-stu-id="bfa49-170">Connection header</span></span>

<span data-ttu-id="bfa49-171">Si la aplicación no usa cookies, puede pasar la información de usuario en el encabezado de la conexión.</span><span class="sxs-lookup"><span data-stu-id="bfa49-171">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="bfa49-172">Por ejemplo, puede pasar un token en el encabezado de la conexión.</span><span class="sxs-lookup"><span data-stu-id="bfa49-172">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="bfa49-173">A continuación, en el concentrador, se comprobaría el token del usuario.</span><span class="sxs-lookup"><span data-stu-id="bfa49-173">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="bfa49-174">Certificado</span><span class="sxs-lookup"><span data-stu-id="bfa49-174">Certificate</span></span>

<span data-ttu-id="bfa49-175">Puede pasar un certificado de cliente para comprobar el usuario.</span><span class="sxs-lookup"><span data-stu-id="bfa49-175">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="bfa49-176">Agregue el certificado al crear la conexión.</span><span class="sxs-lookup"><span data-stu-id="bfa49-176">You add the certificate when creating the connection.</span></span> <span data-ttu-id="bfa49-177">En el ejemplo siguiente se muestra cómo agregar un certificado de cliente a la conexión; no se muestra la aplicación de consola completa.</span><span class="sxs-lookup"><span data-stu-id="bfa49-177">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="bfa49-178">Utiliza la clase [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) , que proporciona varias maneras diferentes de crear el certificado.</span><span class="sxs-lookup"><span data-stu-id="bfa49-178">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
