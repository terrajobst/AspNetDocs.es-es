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
# <a name="authentication-and-authorization-for-signalr-hubs"></a>Autenticación y autorización de los concentradores de SignalR

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tema se describe cómo restringir qué usuarios o roles pueden acceder a los métodos de concentrador.
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

## <a name="overview"></a>Información general

Este tema contiene las siguientes secciones:

- [Authorize (atributo)](#authorizeattribute)
- [Requerir autenticación para todos los concentradores](#requireauth)
- [Autorización personalizada](#custom)
- [Pasar información de autenticación a los clientes](#passauth)
- [Opciones de autenticación para clientes de .NET](#authoptions)

    - [Cookie con autenticación de formularios](#cookie)
    - [Autenticación de Windows](#windows)
    - [Encabezado de conexión](#header)
    - [Certificate](#certificate)

<a id="authorizeattribute"></a>

## <a name="authorize-attribute"></a>Authorize (atributo)

Signalr proporciona el atributo [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) para especificar qué usuarios o roles tienen acceso a un concentrador o método. Este atributo se encuentra en el espacio de nombres `Microsoft.AspNet.SignalR`. El atributo `Authorize` se aplica a un concentrador o a métodos concretos de un concentrador. Al aplicar el atributo `Authorize` a una clase de concentrador, el requisito de autorización especificado se aplica a todos los métodos del concentrador. En este tema se proporcionan ejemplos de los diferentes tipos de requisitos de autorización que se pueden aplicar. Sin el atributo `Authorize`, un cliente conectado puede acceder a cualquier método público en el concentrador.

Si ha definido un rol denominado "admin" en la aplicación Web, puede especificar que solo los usuarios de ese rol puedan acceder a un centro con el código siguiente.

[!code-csharp[Main](hub-authorization/samples/sample1.cs)]

O bien, puede especificar que un concentrador contenga un método que esté disponible para todos los usuarios y un segundo método que solo esté disponible para los usuarios autenticados, como se muestra a continuación.

[!code-csharp[Main](hub-authorization/samples/sample2.cs)]

En los siguientes ejemplos se tratan diferentes escenarios de autorización:

- `[Authorize]`: solo usuarios autenticados
- `[Authorize(Roles = "Admin,Manager")]`: solo usuarios autenticados en los roles especificados
- `[Authorize(Users = "user1,user2")]`: solo usuarios autenticados con los nombres de usuario especificados
- `[Authorize(RequireOutgoing=false)]`: solo los usuarios autenticados pueden invocar el concentrador, pero las llamadas desde el servidor de vuelta a los clientes no están limitadas por la autorización, como, cuando solo determinados usuarios pueden enviar un mensaje, pero todos los demás pueden recibir el mensaje. La propiedad RequireOutgoing solo se puede aplicar a todo el concentrador, no a los métodos particulares del centro. Cuando RequireOutgoing no está establecido en false, solo se llama desde el servidor a los usuarios que cumplen el requisito de autorización.

<a id="requireauth"></a>

## <a name="require-authentication-for-all-hubs"></a>Requerir autenticación para todos los concentradores

Puede requerir la autenticación para todos los concentradores y métodos de concentrador en la aplicación llamando al método [RequireAuthentication](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubpipelineextensions.requireauthentication(v=vs.111).aspx) cuando se inicia la aplicación. Puede usar este método si tiene varios centros y desea aplicar un requisito de autenticación para todos ellos. Con este método, no se pueden especificar requisitos para el rol, el usuario o la autorización de salida. Solo puede especificar que el acceso a los métodos de concentrador esté restringido a los usuarios autenticados. Sin embargo, todavía puede aplicar el atributo Authorize a los concentradores o métodos para especificar requisitos adicionales. Cualquier requisito que especifique en un atributo se agrega al requisito básico de autenticación.

En el ejemplo siguiente se muestra un archivo de inicio que restringe todos los métodos de concentrador a usuarios autenticados.

[!code-csharp[Main](hub-authorization/samples/sample3.cs)]

Si llama al método `RequireAuthentication()` una vez que se ha procesado una solicitud de Signalr, Signalr producirá una excepción `InvalidOperationException`. Signalr produce esta excepción porque no se puede Agregar un módulo al HubPipeline una vez que se ha invocado la canalización. En el ejemplo anterior se muestra la llamada al método `RequireAuthentication` en el método `Configuration` que se ejecuta una vez antes de controlar la primera solicitud.

<a id="custom"></a>

## <a name="customized-authorization"></a>Autorización personalizada

Si necesita personalizar el modo en que se determina la autorización, puede crear una clase que derive de `AuthorizeAttribute` e invalide el método [UserAuthorized](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute.userauthorized(v=vs.111).aspx) . Para cada solicitud, Signalr invoca este método para determinar si el usuario está autorizado para completar la solicitud. En el método invalidado, se proporciona la lógica necesaria para el escenario de autorización. En el ejemplo siguiente se muestra cómo aplicar la autorización a través de la identidad basada en notificaciones.

[!code-csharp[Main](hub-authorization/samples/sample4.cs)]

<a id="passauth"></a>

## <a name="pass-authentication-information-to-clients"></a>Pasar información de autenticación a los clientes

Es posible que necesite usar información de autenticación en el código que se ejecuta en el cliente. La información necesaria se pasa cuando se llama a los métodos en el cliente. Por ejemplo, un método de aplicación de chat podría pasar como parámetro el nombre de usuario de la persona que publica un mensaje, como se muestra a continuación.

[!code-csharp[Main](hub-authorization/samples/sample5.cs)]

O bien, puede crear un objeto para representar la información de autenticación y pasar ese objeto como parámetro, como se muestra a continuación.

[!code-csharp[Main](hub-authorization/samples/sample6.cs)]

Nunca debe pasar el identificador de conexión de un cliente a otros clientes, ya que un usuario malintencionado podría usarlo para imitar una solicitud de ese cliente.

<a id="authoptions"></a>

## <a name="authentication-options-for-net-clients"></a>Opciones de autenticación para clientes de .NET

Si tiene un cliente de .NET, como una aplicación de consola, que interactúa con un centro limitado a los usuarios autenticados, puede pasar las credenciales de autenticación en una cookie, en el encabezado de la conexión o en un certificado. En los ejemplos de esta sección se muestra cómo usar los distintos métodos para autenticar a un usuario. No son aplicaciones de Signalr totalmente funcionales. Para obtener más información sobre los clientes de .NET con Signalr, consulte [Guía de la API de hubs: cliente .net](../guide-to-the-api/hubs-api-guide-net-client.md).

<a id="cookie"></a>

### <a name="cookie"></a>Cookie

Cuando el cliente .NET interactúa con un concentrador que usa la autenticación de formularios ASP.NET, deberá establecer manualmente la cookie de autenticación en la conexión. Agregue la cookie a la propiedad `CookieContainer` en el objeto [HubConnection](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.client.hubs.hubconnection(v=vs.111).aspx) . En el ejemplo siguiente se muestra una aplicación de consola que recupera una cookie de autenticación de una página web y agrega esa cookie a la conexión.

[!code-csharp[Main](hub-authorization/samples/sample7.cs)]

La aplicación de consola envía las credenciales a <strong>www.contoso.com/RemoteLogin</strong> , que pueden hacer referencia a una página vacía que contiene el siguiente archivo de código subyacente.

[!code-csharp[Main](hub-authorization/samples/sample8.cs)]

<a id="windows"></a>

### <a name="windows-authentication"></a>Autenticación de Windows

Al usar la autenticación de Windows, puede pasar las credenciales del usuario actual mediante la propiedad [DefaultCredentials](https://msdn.microsoft.com/library/system.net.credentialcache.defaultcredentials.aspx) . Establezca las credenciales para la conexión con el valor de DefaultCredentials.

[!code-csharp[Main](hub-authorization/samples/sample9.cs?highlight=6)]

<a id="header"></a>

### <a name="connection-header"></a>Encabezado de conexión

Si la aplicación no usa cookies, puede pasar la información de usuario en el encabezado de la conexión. Por ejemplo, puede pasar un token en el encabezado de la conexión.

[!code-csharp[Main](hub-authorization/samples/sample10.cs?highlight=6)]

A continuación, en el concentrador, se comprobaría el token del usuario.

<a id="certificate"></a>

### <a name="certificate"></a>Certificado

Puede pasar un certificado de cliente para comprobar el usuario. Agregue el certificado al crear la conexión. En el ejemplo siguiente se muestra cómo agregar un certificado de cliente a la conexión; no se muestra la aplicación de consola completa. Utiliza la clase [X509Certificate](https://msdn.microsoft.com/library/system.security.cryptography.x509certificates.x509certificate.aspx) , que proporciona varias maneras diferentes de crear el certificado.

[!code-csharp[Main](hub-authorization/samples/sample11.cs?highlight=6)]
