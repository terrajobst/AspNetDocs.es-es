---
uid: web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
title: Autenticación y autorización en ASP.NET Web API | Microsoft Docs
author: MikeWasson
description: Proporciona una descripción general de la autenticación y autorización en ASP.NET Web API.
ms.author: riande
ms.date: 11/27/2012
ms.assetid: 6dfb51ea-9f4d-4e70-916c-8ef8344a88d6
msc.legacyurl: /web-api/overview/security/authentication-and-authorization-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 368d2b9456d12b2bb4063a23333e5c8837faa3b8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78484423"
---
# <a name="authentication-and-authorization-in-aspnet-web-api"></a>Autenticación y autorización en ASP.NET Web API

por [Mike Wasson](https://github.com/MikeWasson)

Ha creado una API Web, pero ahora desea controlar el acceso a ella. En esta serie de artículos, veremos algunas opciones para proteger una API Web de usuarios no autorizados. Esta serie cubrirá la autenticación y la autorización.

- La *autenticación* está conociendo la identidad del usuario. Por ejemplo, Alice inicia sesión con su nombre de usuario y contraseña, y el servidor usa la contraseña para autenticar a Alice.
- La *autorización* es decidir si un usuario puede realizar una acción. Por ejemplo, Alicia tiene permiso para obtener un recurso, pero no para crear un recurso.

En el primer artículo de la serie se ofrece una introducción general a la autenticación y autorización en ASP.NET Web API. En otros temas se describen los escenarios comunes de autenticación de Web API.

> [!NOTE]
> Gracias a las personas que revisaron esta serie y nos proporcionaban comentarios valiosos: Rick Anderson, Broderick, Barry Dorrans, Tom Dykstra, Hongmei GE, David Paspartúson, Daniel Roth, Tim Teebken.

## <a name="authentication"></a>Autenticación

Web API supone que la autenticación se produce en el host. Para el hospedaje Web, el host es IIS, que usa los módulos HTTP para la autenticación. Puede configurar el proyecto para que use cualquiera de los módulos de autenticación integrados en IIS o ASP.NET, o escribir su propio módulo HTTP para realizar la autenticación personalizada.

Cuando el host autentica al usuario, crea una *entidad*de seguridad, que es un objeto [IPrincipal](https://msdn.microsoft.com/library/System.Security.Principal.IPrincipal.aspx) que representa el contexto de seguridad en el que se ejecuta el código. El host asocia la entidad de seguridad al subproceso actual estableciendo **Thread. CurrentPrincipal**. La entidad de seguridad contiene un objeto de **identidad** asociado que contiene información sobre el usuario. Si el usuario está autenticado, la propiedad **Identity. IsAuthenticated** devuelve **true**. En el caso de las solicitudes anónimas, **IsAuthenticated** devuelve **false**. Para obtener más información acerca de las entidades de seguridad, consulte [seguridad basada en roles](https://msdn.microsoft.com/library/shz8h065.aspx).

### <a name="http-message-handlers-for-authentication"></a>Controladores de mensajes HTTP para la autenticación

En lugar de usar el host para la autenticación, puede poner lógica de autenticación en un [controlador de mensajes http](../advanced/http-message-handlers.md). En ese caso, el controlador de mensajes examina la solicitud HTTP y establece la entidad de seguridad.

¿Cuándo se deben usar los controladores de mensajes para la autenticación? Estos son algunos de los inconvenientes:

- Un módulo HTTP ve todas las solicitudes que pasan a través de la canalización ASP.NET. Un controlador de mensajes solo ve las solicitudes que se enrutan a la API Web.
- Puede establecer controladores de mensajes por ruta, que permite aplicar un esquema de autenticación a una ruta específica.
- Los módulos HTTP son específicos de IIS. Los controladores de mensajes son independientes del host, por lo que se pueden usar tanto con el hospedaje web como con el autohospedaje.
- Los módulos HTTP participan en el registro de IIS, la auditoría, etc.
- Los módulos HTTP se ejecutan antes en la canalización. Si controla la autenticación en un controlador de mensajes, la entidad de seguridad no se establece hasta que se ejecuta el controlador. Además, la entidad de seguridad vuelve a la entidad de seguridad anterior cuando la respuesta deja el controlador de mensajes.

Por lo general, si no necesita admitir el autohospedaje, un módulo HTTP es una opción mejor. Si necesita admitir el autohospedaje, considere la posibilidad de usar un controlador de mensajes.

### <a name="setting-the-principal"></a>Establecimiento de la entidad de seguridad

Si la aplicación realiza una lógica de autenticación personalizada, debe establecer la entidad de seguridad en dos lugares:

- **Thread. CurrentPrincipal**. Esta propiedad es la manera estándar de establecer la entidad de seguridad del subproceso en .NET.
- **HttpContext. Current. User**. Esta propiedad es específica de ASP.NET.

En el código siguiente se muestra cómo establecer la entidad de seguridad:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample1.cs)]

Para el hospedaje Web, debe establecer la entidad de seguridad en ambos lugares; en caso contrario, el contexto de seguridad puede ser incoherente. No obstante, para el autohospedaje, **HttpContext. Current** es NULL. Para asegurarse de que el código sea independiente del host, por lo tanto, compruebe si hay valores NULL antes de asignarlos a **HttpContext. Current**, como se muestra.

## <a name="authorization"></a>Autorización

La autorización se produce más adelante en la canalización, más cerca del controlador. Esto le permite tomar decisiones más pormenorizadas al conceder acceso a los recursos.

- Los *filtros de autorización* se ejecutan antes de la acción de controlador. Si la solicitud no está autorizada, el filtro devuelve una respuesta de error y no se invoca la acción.
- Dentro de una acción del controlador, puede obtener la entidad de seguridad actual de la propiedad **ApiController. User** . Por ejemplo, puede filtrar una lista de recursos basándose en el nombre de usuario y devolver solo los recursos que pertenecen a ese usuario.

![](authentication-and-authorization-in-aspnet-web-api/_static/image1.png)

<a id="auth3"></a>
### <a name="using-the-authorize-attribute"></a>Usar el atributo [Authorize]

Web API proporciona un filtro de autorización integrado, [AuthorizeAttribute](https://msdn.microsoft.com/library/system.web.http.authorizeattribute.aspx). Este filtro comprueba si el usuario está autenticado. De lo contrario, devuelve el código de Estado HTTP 401 (no autorizado), sin invocar la acción.

Puede aplicar el filtro globalmente, en el nivel de controlador o en el nivel de acciones individuales.

**Globalmente**: para restringir el acceso para cada controlador de API Web, agregue el filtro **AuthorizeAttribute** a la lista de filtros globales:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample2.cs)]

**Controlador**: para restringir el acceso a un controlador específico, agregue el filtro como atributo al controlador:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample3.cs)]

**Acción**: para restringir el acceso a acciones específicas, agregue el atributo al método de acción:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample4.cs)]

Como alternativa, puede restringir el controlador y, a continuación, permitir el acceso anónimo a acciones específicas mediante el `[AllowAnonymous]` atributo. En el ejemplo siguiente, el método `Post` está restringido, pero el método `Get` permite el acceso anónimo.

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample5.cs)]

En los ejemplos anteriores, el filtro permite que cualquier usuario autenticado tenga acceso a los métodos restringidos; solo se conservan los usuarios anónimos. También puede limitar el acceso a usuarios específicos o a usuarios en roles específicos:

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample6.cs)]

> [!NOTE]
> El filtro **AuthorizeAttribute** para los controladores de API Web se encuentra en el espacio de nombres **System. Web. http** . Hay un filtro similar para los controladores MVC en el espacio de nombres **System. Web. Mvc** , que no es compatible con los controladores de API Web.

### <a name="custom-authorization-filters"></a>Filtros de autorización personalizados

Para escribir un filtro de autorización personalizado, derive de uno de estos tipos:

- **AuthorizeAttribute**. Extienda esta clase para realizar la lógica de autorización basada en el usuario actual y en los roles del usuario.
- **AuthorizationFilterAttribute**. Extienda esta clase para realizar la lógica de autorización sincrónica que no se basa necesariamente en el usuario o el rol actual.
- **IAuthorizationFilter**. Implemente esta interfaz para realizar la lógica de autorización asincrónica; por ejemplo, si la lógica de autorización realiza llamadas asincrónicas de e/s o de red. (Si la lógica de autorización está enlazada a la CPU, es más fácil derivar de **AuthorizationFilterAttribute**, porque después no es necesario escribir un método asincrónico).

En el diagrama siguiente se muestra la jerarquía de clases de la clase **AuthorizeAttribute** .

![](authentication-and-authorization-in-aspnet-web-api/_static/image2.png)

### <a name="authorization-inside-a-controller-action"></a>Autorización dentro de una acción de controlador

En algunos casos, puede permitir que una solicitud continúe, pero cambiar el comportamiento en función de la entidad de seguridad. Por ejemplo, la información que devuelva podría cambiar en función del rol del usuario. Dentro de un método de controlador, puede obtener la entidad de seguridad actual de la propiedad **ApiController. User** .

[!code-csharp[Main](authentication-and-authorization-in-aspnet-web-api/samples/sample7.cs)]
