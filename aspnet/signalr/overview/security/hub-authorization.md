---
uid: signalr/overview/security/hub-authorization
title: Autenticación y autorización de los concentradores de Signalr | Microsoft Docs
author: bradygaster
description: En este tema se describe cómo restringir qué usuarios o roles pueden acceder a los métodos de concentrador. Versiones de software usadas en este tema Visual Studio 2013 .NET 4,5 Signalr...
ms.author: bradyg
ms.date: 01/05/2015
ms.assetid: a610c796-c131-473c-baef-2e6c568cb2a2
msc.legacyurl: /signalr/overview/security/hub-authorization
msc.type: authoredcontent
ms.openlocfilehash: 5006af5e623da6958a6d59949c6f2cf776c77fc3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467515"
---
# <a name="authentication-and-authorization-for-signalr-hubs"></a><span data-ttu-id="bda2d-104">Autenticación y autorización de los concentradores de SignalR</span><span class="sxs-lookup"><span data-stu-id="bda2d-104">Authentication and Authorization for SignalR Hubs</span></span>

<span data-ttu-id="bda2d-105">por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bda2d-105">by [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> <span data-ttu-id="bda2d-106">En este tema se describe cómo restringir qué usuarios o roles pueden acceder a los métodos de concentrador.</span><span class="sxs-lookup"><span data-stu-id="bda2d-106">This topic describes how to restrict which users or roles can access hub methods.</span></span>
>
> ## <a name="software-versions-used-in-this-topic"></a><span data-ttu-id="bda2d-107">Versiones de software utilizadas en este tema</span><span class="sxs-lookup"><span data-stu-id="bda2d-107">Software versions used in this topic</span></span>
>
>
> - [<span data-ttu-id="bda2d-108">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="bda2d-108">Visual Studio 2013</span></span>](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - <span data-ttu-id="bda2d-109">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="bda2d-109">.NET 4.5</span></span>
> - <span data-ttu-id="bda2d-110">Signalr versión 2</span><span class="sxs-lookup"><span data-stu-id="bda2d-110">SignalR version 2</span></span>
>
>
>
> ## <a name="previous-versions-of-this-topic"></a><span data-ttu-id="bda2d-111">Versiones anteriores de este tema</span><span class="sxs-lookup"><span data-stu-id="bda2d-111">Previous versions of this topic</span></span>
>
> <span data-ttu-id="bda2d-112">Para obtener información sobre las versiones anteriores de Signalr, consulte [versiones anteriores de signalr](../older-versions/index.md).</span><span class="sxs-lookup"><span data-stu-id="bda2d-112">For information about earlier versions of SignalR, see [SignalR Older Versions](../older-versions/index.md).</span></span>
>
> ## <a name="questions-and-comments"></a><span data-ttu-id="bda2d-113">Preguntas y comentarios</span><span class="sxs-lookup"><span data-stu-id="bda2d-113">Questions and comments</span></span>
>
> <span data-ttu-id="bda2d-114">Deje comentarios sobre cómo le gustó este tutorial y lo que podríamos mejorar en los comentarios en la parte inferior de la página.</span><span class="sxs-lookup"><span data-stu-id="bda2d-114">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="bda2d-115">Si tiene preguntas que no están directamente relacionadas con el tutorial, puede publicarlas en el foro de [ASP.net signalr](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [stackoverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="bda2d-115">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>

## <a name="overview"></a><span data-ttu-id="bda2d-116">Información general</span><span class="sxs-lookup"><span data-stu-id="bda2d-116">Overview</span></span>

<span data-ttu-id="bda2d-117">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="bda2d-117">This topic contains the following sections:</span></span>

- [<span data-ttu-id="bda2d-118">Authorize (atributo)</span><span class="sxs-lookup"><span data-stu-id="bda2d-118">Authorize attribute</span></span>](#authorizeattribute)
- [<span data-ttu-id="bda2d-119">Requerir autenticación para todos los concentradores</span><span class="sxs-lookup"><span data-stu-id="bda2d-119">Require authentication for all hubs</span></span>](#requireauth)
- [<span data-ttu-id="bda2d-120">Autorización personalizada</span><span class="sxs-lookup"><span data-stu-id="bda2d-120">Customized authorization</span></span>](#custom)
- [<span data-ttu-id="bda2d-121">Pasar información de autenticación a los clientes</span><span class="sxs-lookup"><span data-stu-id="bda2d-121">Pass authentication information to clients</span></span>](#passauth)
- [<span data-ttu-id="bda2d-122">Opciones de autenticación para clientes de .NET</span><span class="sxs-lookup"><span data-stu-id="bda2d-122">Authentication options for .NET clients</span></span>](#authoptions)

    - [<span data-ttu-id="bda2d-123">Cookie con autenticación de formularios</span><span class="sxs-lookup"><span data-stu-id="bda2d-123">Cookie with Forms Authentication</span></span>](#cookie)
    - [<span data-ttu-id="bda2d-124">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="bda2d-124">Windows Authentication</span></span>](#windows)
    - [<span data-ttu-id="bda2d-125">Encabezado de conexión</span><span class="sxs-lookup"><span data-stu-id="bda2d-125">Connection header</span></span>](#header)
    - [<span data-ttu-id="bda2d-126">Certificate</span><span class="sxs-lookup"><span data-stu-id="bda2d-126">Certificate</span></span>](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a><span data-ttu-id="bda2d-127">Authorize (atributo)</span><span class="sxs-lookup"><span data-stu-id="bda2d-127">Authorize attribute</span></span>

<span data-ttu-id="bda2d-128">Signalr proporciona el atributo [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) para especificar qué usuarios o roles tienen acceso a un concentrador o método.</span><span class="sxs-lookup"><span data-stu-id="bda2d-128">SignalR provides the [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) attribute to specify which users or roles have access to a hub or method.</span></span> <span data-ttu-id="bda2d-129">Este atributo se encuentra en el espacio de nombres `Microsoft.AspNet.SignalR`.</span><span class="sxs-lookup"><span data-stu-id="bda2d-129">This attribute is located in the `Microsoft.AspNet.SignalR` namespace.</span></span> <span data-ttu-id="bda2d-130">El atributo `Authorize` se aplica a un concentrador o a métodos concretos de un concentrador.</span><span class="sxs-lookup"><span data-stu-id="bda2d-130">You apply the `Authorize` attribute to either a hub or particular methods in a hub.</span></span> <span data-ttu-id="bda2d-131">Al aplicar el atributo `Authorize` a una clase de concentrador, el requisito de autorización especificado se aplica a todos los métodos del concentrador.</span><span class="sxs-lookup"><span data-stu-id="bda2d-131">When you apply the `Authorize` attribute to a hub class, the specified authorization requirement is applied to all of the methods in the hub.</span></span> <span data-ttu-id="bda2d-132">En este tema se proporcionan ejemplos de los diferentes tipos de requisitos de autorización que se pueden aplicar.</span><span class="sxs-lookup"><span data-stu-id="bda2d-132">This topic provides examples of the different types of authorization requirements that you can apply.</span></span> <span data-ttu-id="bda2d-133">Sin el atributo `Authorize`, un cliente conectado puede acceder a cualquier método público en el concentrador.</span><span class="sxs-lookup"><span data-stu-id="bda2d-133">Without the `Authorize` attribute, a connected client can access any public method on the hub.</span></span>

<span data-ttu-id="bda2d-134">Si ha definido un rol denominado "admin" en la aplicación Web, puede especificar que solo los usuarios de ese rol puedan acceder a un centro con el código siguiente.</span><span class="sxs-lookup"><span data-stu-id="bda2d-134">If you have defined a role named "Admin" in your web application, you could specify that only users in that role can access a hub with the following code.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

<span data-ttu-id="bda2d-135">O bien, puede especificar que un concentrador contenga un método que esté disponible para todos los usuarios y un segundo método que solo esté disponible para los usuarios autenticados, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="bda2d-135">Or, you can specify that a hub contains one method that is available to all users, and a second method that is only available to authenticated users, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

<span data-ttu-id="bda2d-136">En los siguientes ejemplos se tratan diferentes escenarios de autorización:</span><span class="sxs-lookup"><span data-stu-id="bda2d-136">The following examples address different authorization scenarios:</span></span>

- <span data-ttu-id="bda2d-137">`[Authorize]`: solo usuarios autenticados</span><span class="sxs-lookup"><span data-stu-id="bda2d-137">`[Authorize]` – only authenticated users</span></span>
- <span data-ttu-id="bda2d-138">`[Authorize(Roles = "Admin,Manager")]`: solo usuarios autenticados en los roles especificados</span><span class="sxs-lookup"><span data-stu-id="bda2d-138">`[Authorize(Roles = "Admin,Manager")]` – only authenticated users in the specified roles</span></span>
- <span data-ttu-id="bda2d-139">`[Authorize(Users = "user1,user2")]`: solo usuarios autenticados con los nombres de usuario especificados</span><span class="sxs-lookup"><span data-stu-id="bda2d-139">`[Authorize(Users = "user1,user2")]` – only authenticated users with the specified user names</span></span>
- <span data-ttu-id="bda2d-140">`[Authorize(RequireOutgoing=false)]`: solo los usuarios autenticados pueden invocar el concentrador, pero las llamadas desde el servidor de vuelta a los clientes no están limitadas por la autorización, como, cuando solo determinados usuarios pueden enviar un mensaje, pero todos los demás pueden recibir el mensaje.</span><span class="sxs-lookup"><span data-stu-id="bda2d-140">`[Authorize(RequireOutgoing=false)]` – only authenticated users can invoke the hub, but calls from the server back to clients are not limited by authorization, such as, when only certain users can send a message but all others can receive the message.</span></span> <span data-ttu-id="bda2d-141">La propiedad RequireOutgoing solo se puede aplicar a todo el concentrador, no a los métodos particulares del centro.</span><span class="sxs-lookup"><span data-stu-id="bda2d-141">The RequireOutgoing property can only be applied to the entire hub, not on individuals methods within the hub.</span></span> <span data-ttu-id="bda2d-142">Cuando RequireOutgoing no está establecido en false, solo se llama desde el servidor a los usuarios que cumplen el requisito de autorización.</span><span class="sxs-lookup"><span data-stu-id="bda2d-142">When RequireOutgoing is not set to false, only users that meet the authorization requirement are called from the server.</span></span>

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a><span data-ttu-id="bda2d-143">Requerir autenticación para todos los concentradores</span><span class="sxs-lookup"><span data-stu-id="bda2d-143">Require authentication for all hubs</span></span>

<span data-ttu-id="bda2d-144">Puede requerir la autenticación para todos los concentradores y métodos de concentrador en la aplicación llamando al método [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) cuando se inicia la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bda2d-144">You can require authentication for all hubs and hub methods in your application by calling the [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) method when the application starts.</span></span> <span data-ttu-id="bda2d-145">Puede usar este método si tiene varios centros y desea aplicar un requisito de autenticación para todos ellos.</span><span class="sxs-lookup"><span data-stu-id="bda2d-145">You might use this method when you have multiple hubs and want to enforce an authentication requirement for all of them.</span></span> <span data-ttu-id="bda2d-146">Con este método, no se pueden especificar requisitos para el rol, el usuario o la autorización de salida.</span><span class="sxs-lookup"><span data-stu-id="bda2d-146">With this method, you cannot specify requirements for role, user, or outgoing authorization.</span></span> <span data-ttu-id="bda2d-147">Solo puede especificar que el acceso a los métodos de concentrador esté restringido a los usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="bda2d-147">You can only specify that access to the hub methods is restricted to authenticated users.</span></span> <span data-ttu-id="bda2d-148">Sin embargo, todavía puede aplicar el atributo Authorize a los concentradores o métodos para especificar requisitos adicionales.</span><span class="sxs-lookup"><span data-stu-id="bda2d-148">However, you can still apply the Authorize attribute to hubs or methods to specify additional requirements.</span></span> <span data-ttu-id="bda2d-149">Cualquier requisito que especifique en un atributo se agrega al requisito básico de autenticación.</span><span class="sxs-lookup"><span data-stu-id="bda2d-149">Any requirement you specify in an attribute is added to the basic requirement of authentication.</span></span>

<span data-ttu-id="bda2d-150">En el ejemplo siguiente se muestra un archivo de inicio que restringe todos los métodos de concentrador a usuarios autenticados.</span><span class="sxs-lookup"><span data-stu-id="bda2d-150">The following example shows a Startup file which restricts all hub methods to authenticated users.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

<span data-ttu-id="bda2d-151">Si llama al método `RequireAuthentication()` una vez que se ha procesado una solicitud de Signalr, Signalr producirá una excepción `InvalidOperationException`.</span><span class="sxs-lookup"><span data-stu-id="bda2d-151">If you call the `RequireAuthentication()` method after a SignalR request has been processed, SignalR will throw a `InvalidOperationException` exception.</span></span> <span data-ttu-id="bda2d-152">Signalr produce esta excepción porque no se puede Agregar un módulo al HubPipeline una vez que se ha invocado la canalización.</span><span class="sxs-lookup"><span data-stu-id="bda2d-152">SignalR throws this exception because you cannot add a module to the HubPipeline after the pipeline has been invoked.</span></span> <span data-ttu-id="bda2d-153">En el ejemplo anterior se muestra la llamada al método `RequireAuthentication` en el método `Configuration` que se ejecuta una vez antes de controlar la primera solicitud.</span><span class="sxs-lookup"><span data-stu-id="bda2d-153">The previous example shows calling the `RequireAuthentication` method in the `Configuration` method which is executed one time prior to handling the first request.</span></span>

<a id="custom"></a>

## <a name="customized-authorization"></a><span data-ttu-id="bda2d-154">Autorización personalizada</span><span class="sxs-lookup"><span data-stu-id="bda2d-154">Customized authorization</span></span>

<span data-ttu-id="bda2d-155">Si necesita personalizar el modo en que se determina la autorización, puede crear una clase que derive de `AuthorizeAttribute` e invalide el método [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="bda2d-155">If you need to customize how authorization is determined, you can create a class that derives from `AuthorizeAttribute` and override the [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) method.</span></span> <span data-ttu-id="bda2d-156">Para cada solicitud, Signalr invoca este método para determinar si el usuario está autorizado para completar la solicitud.</span><span class="sxs-lookup"><span data-stu-id="bda2d-156">For each request, SignalR invokes this method to determine whether the user is authorized to complete the request.</span></span> <span data-ttu-id="bda2d-157">En el método invalidado, se proporciona la lógica necesaria para el escenario de autorización.</span><span class="sxs-lookup"><span data-stu-id="bda2d-157">In the overridden method, you provide the necessary logic for your authorization scenario.</span></span> <span data-ttu-id="bda2d-158">En el ejemplo siguiente se muestra cómo aplicar la autorización a través de la identidad basada en notificaciones.</span><span class="sxs-lookup"><span data-stu-id="bda2d-158">The following example shows how to enforce authorization through claims-based identity.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a><span data-ttu-id="bda2d-159">Pasar información de autenticación a los clientes</span><span class="sxs-lookup"><span data-stu-id="bda2d-159">Pass authentication information to clients</span></span>

<span data-ttu-id="bda2d-160">Es posible que necesite usar información de autenticación en el código que se ejecuta en el cliente.</span><span class="sxs-lookup"><span data-stu-id="bda2d-160">You may need to use authentication information in the code that runs on the client.</span></span> <span data-ttu-id="bda2d-161">La información necesaria se pasa cuando se llama a los métodos en el cliente.</span><span class="sxs-lookup"><span data-stu-id="bda2d-161">You pass the required information when calling the methods on the client.</span></span> <span data-ttu-id="bda2d-162">Por ejemplo, un método de aplicación de chat podría pasar como parámetro el nombre de usuario de la persona que publica un mensaje, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="bda2d-162">For example, a chat application method could pass as a parameter the user name of the person posting a message, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

<span data-ttu-id="bda2d-163">O bien, puede crear un objeto para representar la información de autenticación y pasar ese objeto como parámetro, como se muestra a continuación.</span><span class="sxs-lookup"><span data-stu-id="bda2d-163">Or, you can create an object to represent the authentication information and pass that object as a parameter, as shown below.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

<span data-ttu-id="bda2d-164">Nunca debe pasar el identificador de conexión de un cliente a otros clientes, ya que un usuario malintencionado podría usarlo para imitar una solicitud de ese cliente.</span><span class="sxs-lookup"><span data-stu-id="bda2d-164">You should never pass one client's connection id to other clients, as a malicious user could use it to mimic a request from that client.</span></span>

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a><span data-ttu-id="bda2d-165">Opciones de autenticación para clientes de .NET</span><span class="sxs-lookup"><span data-stu-id="bda2d-165">Authentication options for .NET clients</span></span>

<span data-ttu-id="bda2d-166">Si tiene un cliente de .NET, como una aplicación de consola, que interactúa con un centro limitado a los usuarios autenticados, puede pasar las credenciales de autenticación en una cookie, en el encabezado de la conexión o en un certificado.</span><span class="sxs-lookup"><span data-stu-id="bda2d-166">When you have a .NET client, such as a console app, which interacts with a hub that is limited to authenticated users, you can pass the authentication credentials in a cookie, the connection header, or a certificate.</span></span> <span data-ttu-id="bda2d-167">En los ejemplos de esta sección se muestra cómo usar los distintos métodos para autenticar a un usuario.</span><span class="sxs-lookup"><span data-stu-id="bda2d-167">The examples in this section show how to use those different methods for authenticating a user.</span></span> <span data-ttu-id="bda2d-168">No son aplicaciones de Signalr totalmente funcionales.</span><span class="sxs-lookup"><span data-stu-id="bda2d-168">They are not fully-functional SignalR apps.</span></span> <span data-ttu-id="bda2d-169">Para obtener más información sobre los clientes de .NET con Signalr, consulte [Guía de la API de hubs: cliente .net](../guide-to-the-api/hubs-api-guide-net-client.md).</span><span class="sxs-lookup"><span data-stu-id="bda2d-169">For more information about .NET clients with SignalR, see [Hubs API Guide - .NET Client](../guide-to-the-api/hubs-api-guide-net-client.md).</span></span>

<a id="cookie"></a>

### <a name="cookie"></a><span data-ttu-id="bda2d-170">Cookie</span><span class="sxs-lookup"><span data-stu-id="bda2d-170">Cookie</span></span>

<span data-ttu-id="bda2d-171">Cuando el cliente .NET interactúa con un concentrador que usa la autenticación de formularios ASP.NET, deberá establecer manualmente la cookie de autenticación en la conexión.</span><span class="sxs-lookup"><span data-stu-id="bda2d-171">When your .NET client interacts with a hub that uses ASP.NET Forms Authentication, you will need to manually set the authentication cookie on the connection.</span></span> <span data-ttu-id="bda2d-172">Agregue la cookie a la propiedad `CookieContainer` en el objeto [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) .</span><span class="sxs-lookup"><span data-stu-id="bda2d-172">You add the cookie to the `CookieContainer` property on the [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) object.</span></span> <span data-ttu-id="bda2d-173">En el ejemplo siguiente se muestra una aplicación de consola que recupera una cookie de autenticación de una página web y agrega esa cookie a la conexión.</span><span class="sxs-lookup"><span data-stu-id="bda2d-173">The following example shows a console app that retrieves an authentication cookie from a web page and adds that cookie to the connection.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

<span data-ttu-id="bda2d-174">La aplicación de consola envía las credenciales a <strong>www.contoso.com/RemoteLogin</strong> , que pueden hacer referencia a una página vacía que contiene el siguiente archivo de código subyacente.</span><span class="sxs-lookup"><span data-stu-id="bda2d-174">The console app posts the credentials to <strong>www.contoso.com/RemoteLogin</strong> which could refer to an empty page that contains the following code-behind file.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a><span data-ttu-id="bda2d-175">Autenticación de Windows</span><span class="sxs-lookup"><span data-stu-id="bda2d-175">Windows authentication</span></span>

<span data-ttu-id="bda2d-176">Al usar la autenticación de Windows, puede pasar las credenciales del usuario actual mediante la propiedad [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) .</span><span class="sxs-lookup"><span data-stu-id="bda2d-176">When using Windows authentication, you can pass the current user's credentials by using the [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) property.</span></span> <span data-ttu-id="bda2d-177">Establezca las credenciales para la conexión con el valor de DefaultCredentials.</span><span class="sxs-lookup"><span data-stu-id="bda2d-177">You set the credentials for the connection to the value of the DefaultCredentials.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a><span data-ttu-id="bda2d-178">Encabezado de conexión</span><span class="sxs-lookup"><span data-stu-id="bda2d-178">Connection header</span></span>

<span data-ttu-id="bda2d-179">Si la aplicación no usa cookies, puede pasar la información de usuario en el encabezado de la conexión.</span><span class="sxs-lookup"><span data-stu-id="bda2d-179">If your application is not using cookies, you can pass user information in the connection header.</span></span> <span data-ttu-id="bda2d-180">Por ejemplo, puede pasar un token en el encabezado de la conexión.</span><span class="sxs-lookup"><span data-stu-id="bda2d-180">For example, you can pass a token in the connection header.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

<span data-ttu-id="bda2d-181">A continuación, en el concentrador, se comprobaría el token del usuario.</span><span class="sxs-lookup"><span data-stu-id="bda2d-181">Then, in the hub, you would verify the user's token.</span></span>

<a id="certificate"></a>

### <a name="certificate"></a><span data-ttu-id="bda2d-182">Certificado</span><span class="sxs-lookup"><span data-stu-id="bda2d-182">Certificate</span></span>

<span data-ttu-id="bda2d-183">Puede pasar un certificado de cliente para comprobar el usuario.</span><span class="sxs-lookup"><span data-stu-id="bda2d-183">You can pass a client certificate to verify the user.</span></span> <span data-ttu-id="bda2d-184">Agregue el certificado al crear la conexión.</span><span class="sxs-lookup"><span data-stu-id="bda2d-184">You add the certificate when creating the connection.</span></span> <span data-ttu-id="bda2d-185">En el ejemplo siguiente se muestra cómo agregar un certificado de cliente a la conexión; no se muestra la aplicación de consola completa.</span><span class="sxs-lookup"><span data-stu-id="bda2d-185">The following example shows only how to add a client certificate to the connection; it does not show the full console app.</span></span> <span data-ttu-id="bda2d-186">Utiliza la clase [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) , que proporciona varias maneras diferentes de crear el certificado.</span><span class="sxs-lookup"><span data-stu-id="bda2d-186">It uses the [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) class which provides several different ways to create the certificate.</span></span>

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
