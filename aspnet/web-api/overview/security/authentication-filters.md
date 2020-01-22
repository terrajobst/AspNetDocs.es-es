---
uid: web-api/overview/security/authentication-filters
title: Filtros de autenticación en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Un filtro de autenticación es un componente que autentica una solicitud HTTP. Tanto web API 2 como MVC 5 admiten filtros de autenticación, pero difieren ligeramente...
ms.author: riande
ms.date: 09/25/2014
ms.assetid: b9882e53-b3ca-4def-89b0-322846973ccb
msc.legacyurl: /web-api/overview/security/authentication-filters
msc.type: authoredcontent
ms.openlocfilehash: b6815baf05303d5f47a14ee5fe0fdfc2836c1868
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519380"
---
# <a name="authentication-filters-in-aspnet-web-api-2"></a>Filtros de autenticación en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

> Un filtro de autenticación es un componente que autentica una solicitud HTTP. Tanto web API 2 como MVC 5 admiten filtros de autenticación, pero difieren ligeramente, principalmente en las convenciones de nomenclatura para la interfaz de filtro. En este tema se describen los filtros de autenticación de API Web.

Los filtros de autenticación permiten establecer un esquema de autenticación para cada uno de los controladores o acciones. De este modo, la aplicación puede admitir distintos mecanismos de autenticación para distintos recursos HTTP.

En este artículo, mostraré el código del ejemplo de [autenticación básica](http://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) en [https://github.com/aspnet/samples](https://github.com/aspnet/samples). En el ejemplo se muestra un filtro de autenticación que implementa el esquema de autenticación de acceso básico HTTP (RFC 2617). El filtro se implementa en una clase denominada `IdentityBasicAuthenticationAttribute`. No se muestra todo el código del ejemplo, solo las partes que muestran cómo escribir un filtro de autenticación.

## <a name="setting-an-authentication-filter"></a>Establecer un filtro de autenticación

Al igual que otros filtros, los filtros de autenticación se pueden aplicar por controlador, por acción o globalmente a todos los controladores de API Web.

Para aplicar un filtro de autenticación a un controlador, decore la clase de controlador con el atributo de filtro. El código siguiente establece el filtro de `[IdentityBasicAuthentication]` en una clase de controlador, que habilita la autenticación básica para todas las acciones del controlador.

[!code-csharp[Main](authentication-filters/samples/sample1.cs)]

Para aplicar el filtro a una acción, decore la acción con el filtro. El código siguiente establece el filtro de `[IdentityBasicAuthentication]` en el método de `Post` del controlador.

[!code-csharp[Main](authentication-filters/samples/sample2.cs)]

Para aplicar el filtro a todos los controladores de la API Web, agréguelo a **GlobalConfiguration. filters**.

[!code-csharp[Main](authentication-filters/samples/sample3.cs)]

## <a name="implementing-a-web-api-authentication-filter"></a>Implementación de un filtro de autenticación de API Web

En Web API, los filtros de autenticación implementan la interfaz [System. Web. http. filters. IAuthenticationFilter](https://msdn.microsoft.com/library/system.web.http.filters.iauthenticationfilter.aspx) . También deben heredar de **System. Attribute**para aplicarlos como atributos.

La interfaz **IAuthenticationFilter** tiene dos métodos:

- **AuthenticateAsync** autentica la solicitud mediante la validación de las credenciales en la solicitud, si está presente.
- **ChallengeAsync** agrega un desafío de autenticación a la respuesta http, si es necesario.

Estos métodos se corresponden con el flujo de autenticación definido en [rfc 2612](http://tools.ietf.org/html/rfc2616) y [RFC 2617](http://tools.ietf.org/html/rfc2617):

1. El cliente envía las credenciales en el encabezado de autorización. Esto suele suceder después de que el cliente reciba una respuesta 401 (no autorizada) del servidor. Sin embargo, un cliente puede enviar credenciales con cualquier solicitud, no solo después de obtener un 401.
2. Si el servidor no acepta las credenciales, devuelve una respuesta 401 (no autorizada). La respuesta incluye un encabezado WWW-Authenticate que contiene uno o más desafíos. Cada desafío especifica un esquema de autenticación reconocido por el servidor.

El servidor también puede devolver 401 desde una solicitud anónima. De hecho, esto suele ser el modo en que se inicia el proceso de autenticación:

1. El cliente envía una solicitud anónima.
2. El servidor devuelve 401.
3. Los clientes reenvían la solicitud con credenciales.

En este flujo se incluyen los pasos de *autenticación* y *autorización* .

- La autenticación demuestra la identidad del cliente.
- La autorización determina si el cliente puede tener acceso a un recurso determinado.

En Web API, los filtros de autenticación controlan la autenticación, pero no la autorización. La autorización debe realizarse mediante un filtro de autorización o dentro de la acción del controlador.

Este es el flujo en la canalización Web API 2:

1. Antes de invocar una acción, la API Web crea una lista de los filtros de autenticación para esa acción. Esto incluye filtros con el ámbito de la acción, el ámbito del controlador y el ámbito global.
2. La API Web llama a **AuthenticateAsync** en todos los filtros de la lista. Cada filtro puede validar las credenciales de la solicitud. Si algún filtro valida correctamente las credenciales, el filtro crea un **IPrincipal** y lo adjunta a la solicitud. Un filtro también puede desencadenar un error en este momento. Si es así, el resto de la canalización no se ejecuta.
3. Suponiendo que no hay ningún error, la solicitud fluye a través del resto de la canalización.
4. Por último, la API Web llama a cada método **ChallengeAsync** de un filtro de autenticación. Los filtros utilizan este método para agregar un desafío a la respuesta, si es necesario. Normalmente (pero no siempre) que se produciría en respuesta a un error 401.

En los diagramas siguientes se muestran dos casos posibles. En el primero, el filtro de autenticación autentica la solicitud correctamente, un filtro de autorización autoriza la solicitud y la acción del controlador devuelve 200 (correcto).

![](authentication-filters/_static/image1.png)

En el segundo ejemplo, el filtro de autenticación autentica la solicitud, pero el filtro de autorización devuelve 401 (no autorizado). En este caso, no se invoca la acción del controlador. El filtro de autenticación agrega un encabezado WWW-Authenticate a la respuesta.

![](authentication-filters/_static/image2.png)

Otras combinaciones son posibles&mdash;por ejemplo, si la acción del controlador permite solicitudes anónimas, es posible que tenga un filtro de autenticación pero sin autorización.

## <a name="implementing-the-authenticateasync-method"></a>Implementar el método AuthenticateAsync

El método **AuthenticateAsync** intenta autenticar la solicitud. Esta es la firma del método:

[!code-csharp[Main](authentication-filters/samples/sample4.cs)]

El método **AuthenticateAsync** debe realizar una de las siguientes acciones:

1. Nada (no-OP).
2. Cree un **IPrincipal** y establézcalo en la solicitud.
3. Establezca un resultado de error.

La opción (1) significa que la solicitud no tenía ninguna credencial que el filtro entienda. La opción (2) significa que el filtro autenticó correctamente la solicitud. La opción (3) significa que la solicitud tenía credenciales no válidas (por ejemplo, contraseña incorrecta), lo que desencadena una respuesta de error.

A continuación se muestra una descripción general de la implementación de **AuthenticateAsync**.

1. Busque las credenciales en la solicitud.
2. Si no hay ninguna credencial, no haga nada y devuelva (no-OP).
3. Si hay credenciales pero el filtro no reconoce el esquema de autenticación, no haga nada y devuelva (no-OP). Otro filtro de la canalización puede comprender el esquema.
4. Si hay credenciales que el filtro entiende, intente autenticarlas.
5. Si las credenciales son incorrectas, devuelva 401 estableciendo `context.ErrorResult`.
6. Si las credenciales son válidas, cree una **IPrincipal** y establezca `context.Principal`.

En el código siguiente se muestra el método **AuthenticateAsync** del ejemplo de [autenticación básica](http://github.com/aspnet/samples/tree/master/samples/aspnet/WebApi/BasicAuthentication) . Los comentarios indican cada paso. El código muestra varios tipos de error: un encabezado de autorización sin credenciales, credenciales con formato incorrecto y un nombre de usuario o contraseña incorrectos.

[!code-csharp[Main](authentication-filters/samples/sample5.cs)]

## <a name="setting-an-error-result"></a>Establecer un resultado de error

Si las credenciales no son válidas, el filtro debe establecer `context.ErrorResult` en un **IHttpActionResult** que crea una respuesta de error. Para obtener más información sobre **IHttpActionResult**, consulte [resultados de la acción en Web API 2](../getting-started-with-aspnet-web-api/action-results.md).

El ejemplo de autenticación básica incluye una clase `AuthenticationFailureResult` que es adecuada para este fin.

[!code-csharp[Main](authentication-filters/samples/sample6.cs)]

## <a name="implementing-challengeasync"></a>Implementación de ChallengeAsync

El propósito del método **ChallengeAsync** es agregar desafíos de autenticación a la respuesta, si es necesario. Esta es la firma del método:

[!code-csharp[Main](authentication-filters/samples/sample7.cs)]

Se llama al método en todos los filtros de autenticación de la canalización de solicitudes.

Es importante comprender que se llama a **ChallengeAsync** *antes* de que se cree la respuesta HTTP y, posiblemente, incluso antes de que se ejecute la acción del controlador. Cuando se llama a **ChallengeAsync** , `context.Result` contiene un **IHttpActionResult**, que se usa más adelante para crear la respuesta http. Por tanto, cuando se llama a **ChallengeAsync** , no se sabe nada sobre la respuesta http todavía. El método **ChallengeAsync** debe reemplazar el valor original de `context.Result` por un nuevo **IHttpActionResult**. Este **IHttpActionResult** debe contener el `context.Result`original.

![](authentication-filters/_static/image3.png)

Llamaré al **IHttpActionResult** original el *resultado interno*y el nuevo **IHttpActionResult** el *resultado externo*. El resultado externo debe hacer lo siguiente:

1. Invoque el resultado interno para crear la respuesta HTTP.
2. Examine la respuesta.
3. Agregue un desafío de autenticación a la respuesta, si es necesario.

El ejemplo siguiente se toma del ejemplo de autenticación básica. Define un **IHttpActionResult** para el resultado externo.

[!code-csharp[Main](authentication-filters/samples/sample8.cs)]

La propiedad `InnerResult` contiene el **IHttpActionResult**interno. La propiedad `Challenge` representa un encabezado www-Authentication. Observe que **ExecuteAsync** llama primero a `InnerResult.ExecuteAsync` para crear la respuesta HTTP y, a continuación, agrega el desafío si es necesario.

Compruebe el código de respuesta antes de agregar el desafío. La mayoría de los esquemas de autenticación solo agregan un desafío si la respuesta es 401, como se muestra aquí. Sin embargo, algunos esquemas de autenticación agregan un desafío a una respuesta correcta. Por ejemplo, consulte [Negotiate](http://tools.ietf.org/html/rfc4559#section-5) (RFC 4559).

Dada la clase `AddChallengeOnUnauthorizedResult`, el código real de **ChallengeAsync** es sencillo. Solo tiene que crear el resultado y adjuntarlo a `context.Result`.

[!code-csharp[Main](authentication-filters/samples/sample9.cs)]

Nota: el ejemplo de autenticación básica abstrae esta lógica un poco, colocándolo en un método de extensión.

## <a name="combining-authentication-filters-with-host-level-authentication"></a>Combinación de filtros de autenticación con autenticación de nivel de host

"Autenticación de nivel de host" es la autenticación realizada por el host (como IIS), antes de que la solicitud alcance el marco de la API Web.

A menudo, es posible que quiera habilitar la autenticación de nivel de host para el resto de la aplicación, pero deshabilitarla para los controladores de la API Web. Por ejemplo, un escenario típico es habilitar la autenticación de formularios en el nivel de host, pero usar la autenticación basada en tokens para la API Web.

Para deshabilitar la autenticación de nivel de host dentro de la canalización de API Web, llame a `config.SuppressHostPrincipal()` en su configuración. Esto hace que la API Web Quite el **IPrincipal** de cualquier solicitud que entra en la canalización de la API Web. De hecho, &quot;anula la autenticación&quot; la solicitud.

[!code-csharp[Main](authentication-filters/samples/sample10.cs)]

## <a name="additional-resources"></a>Recursos adicionales

[ASP.net web API filtros de seguridad](https://msdn.microsoft.com/magazine/dn781361.aspx) (MSDN Magazine)
