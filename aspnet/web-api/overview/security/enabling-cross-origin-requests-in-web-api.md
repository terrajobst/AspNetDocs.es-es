---
uid: web-api/overview/security/enabling-cross-origin-requests-in-web-api
title: Habilitación de solicitudes entre orígenes en ASP.NET Web API 2 | Microsoft Docs
author: MikeWasson
description: Muestra cómo admitir el uso compartido de recursos entre orígenes (CORS) en ASP.NET Web API.
ms.author: riande
ms.date: 01/29/2019
ms.assetid: 9b265a5a-6a70-4a82-adce-2d7c56ae8bdd
msc.legacyurl: /web-api/overview/security/enabling-cross-origin-requests-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9d3016d98fa6c3a55359c6dab0737407b29925f1
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78447619"
---
# <a name="enable-cross-origin-requests-in-aspnet-web-api-2"></a>Habilitar solicitudes entre orígenes en ASP.NET Web API 2

por [Mike Wasson](https://github.com/MikeWasson)

> La seguridad del explorador impide que una página web realice solicitudes AJAX a otro dominio. Esta restricción se conoce como *directiva de mismo origen* y evita que un sitio malintencionado lea información confidencial de otro sitio. Sin embargo, a veces es posible que quiera permitir que otros llamen a su API web.
>
> El [uso compartido de recursos entre orígenes](http://www.w3.org/TR/cors/) (CORS) es un estándar del W3C que permite a un servidor relajar la Directiva del mismo origen. Mediante CORS, un servidor puede permitir explícitamente algunas solicitudes entre orígenes y rechazar otras. CORS es más seguro y más flexible que las técnicas anteriores, como [JSONP](http://en.wikipedia.org/wiki/JSONP). En este tutorial se muestra cómo habilitar CORS en la aplicación de API Web.
>
> ## <a name="software-used-in-the-tutorial"></a>Software usado en el tutorial
>
> - [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)
> - API Web 2,2

## <a name="introduction"></a>Introducción

En este tutorial se muestra la compatibilidad con CORS en ASP.NET Web API. Comenzaremos creando dos proyectos de ASP.NET: uno denominado "WebService", que hospeda un controlador de API Web y el otro denominado "WebClient", que llama a WebService. Dado que las dos aplicaciones se hospedan en dominios diferentes, una solicitud AJAX de WebClient a WebService es una solicitud entre orígenes.

![](enabling-cross-origin-requests-in-web-api/_static/image1.png)

### <a name="what-is-same-origin"></a>¿Qué es el "mismo origen"?

Dos direcciones URL tienen el mismo origen si tienen puertos, hosts y esquemas idénticos. ([RFC 6454](http://tools.ietf.org/html/rfc6454))

Estas dos direcciones URL tienen el mismo origen:

- `http://example.com/foo.html`
- `http://example.com/bar.html`

Estas direcciones URL tienen orígenes diferentes de los dos anteriores:

- `http://example.net`: dominio diferente
- `http://example.com:9000/foo.html`: puerto diferente
- `https://example.com/foo.html`: esquema diferente
- `http://www.example.com/foo.html`-subdominio diferente

> [!NOTE]
> Internet Explorer no tiene en cuenta el puerto al comparar los orígenes.

## <a name="create-the-webservice-project"></a>Crear el proyecto WebService

> [!NOTE]
> En esta sección se da por supuesto que ya sabe cómo crear proyectos de Web API. Si no es así, vea [Introducción con ASP.net web API](../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

1. Inicie Visual Studio y cree un nuevo proyecto de **aplicación Web de ASP.net (.NET Framework)** .
2. En el cuadro de diálogo **nueva aplicación Web de ASP.net** , seleccione la plantilla de proyecto **vacía** . En **Agregar carpetas y referencias principales para**, active la casilla **API Web** .

   ![Cuadro de diálogo nuevo proyecto de ASP.NET en Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog.png)

3. Agregue un controlador de API Web denominado `TestController` con el código siguiente:

   [!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample1.cs)]

4. Puede ejecutar la aplicación de forma local o implementarla en Azure. (Para las capturas de pantallas de este tutorial, la aplicación se implementa en Azure App Service Web Apps). Para comprobar que la API Web funciona, vaya a `http://hostname/api/test/`, donde *hostname* es el dominio en el que ha implementado la aplicación. Debería ver el texto de la respuesta &quot;GET: test Message&quot;.

   ![Explorador Web que muestra el mensaje de prueba](enabling-cross-origin-requests-in-web-api/_static/image4.png)

## <a name="create-the-webclient-project"></a>Crear el proyecto de WebClient

1. Cree otro proyecto de **aplicación Web de ASP.net (.NET Framework)** y seleccione la plantilla de proyecto de **MVC** . Opcionalmente, seleccione **cambiar autenticación** > **sin autenticación**. No necesita autenticación para este tutorial.

   ![Plantilla MVC en el cuadro de diálogo nuevo proyecto de ASP.NET en Visual Studio](enabling-cross-origin-requests-in-web-api/_static/new-web-app-dialog-mvc.png)

2. En **Explorador de soluciones**, abra el archivo *views/home/index. cshtml*. Reemplace el código de este archivo por lo siguiente:

   [!code-cshtml[Main](enabling-cross-origin-requests-in-web-api/samples/sample2.cshtml?highlight=13)]

   Para la variable *serviceUrl* , use el URI de la aplicación WebService.

3. Ejecutar la aplicación WebClient localmente o publicarla en otro sitio Web.

Al hacer clic en el botón "pruébelo", se envía una solicitud AJAX a la aplicación WebService mediante el método HTTP que se muestra en el cuadro desplegable (GET, POST o PUT). Esto le permite examinar diferentes solicitudes entre orígenes. Actualmente, la aplicación WebService no es compatible con CORS, por lo que si hace clic en el botón obtendrá un error.

![Error ' pruébelo ' en el explorador](enabling-cross-origin-requests-in-web-api/_static/image7.png)

> [!NOTE]
> Si observa el tráfico HTTP en una herramienta como [Fiddler](https://www.telerik.com/fiddler), verá que el explorador envía la solicitud GET y la solicitud se realiza correctamente, pero la llamada AJAX devuelve un error. Es importante comprender que la Directiva del mismo origen no impide que el explorador *envíe* la solicitud. En su lugar, impide que la aplicación vea la *respuesta*.

![Depurador Web de Fiddler que muestra solicitudes Web](enabling-cross-origin-requests-in-web-api/_static/image8.png)

## <a name="enable-cors"></a>Habilitación de CORS

Ahora vamos a habilitar CORS en la aplicación WebService. En primer lugar, agregue el paquete NuGet de CORS. En Visual Studio, en el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**. En la ventana de la consola del administrador de paquetes, escriba el siguiente comando:

[!code-powershell[Main](enabling-cross-origin-requests-in-web-api/samples/sample3.ps1)]

Este comando instala el paquete más reciente y actualiza todas las dependencias, incluidas las bibliotecas principales de API Web. Use la marca `-Version` para tener como destino una versión específica. El paquete CORS requiere Web API 2,0 o posterior.

Abra la aplicación de archivo *\_Start/WebApiConfig. CS*. Agregue el código siguiente al método **WebApiConfig. Register** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample4.cs?highlight=9)]

A continuación, agregue el atributo **[EnableCors]** a la clase `TestController`:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample5.cs?highlight=3,7)]

En el caso *del parámetro* Origins, use el URI en el que implementó la aplicación WebClient. Esto permite las solicitudes entre orígenes de WebClient, a la vez que no se permiten todas las demás solicitudes entre dominios. Más adelante, describiré los parámetros de **[EnableCors]** con más detalle.

No incluya una barra diagonal al final de la dirección URL de *origen* .

Vuelva a implementar la aplicación WebService actualizada. No es necesario actualizar WebClient. Ahora la solicitud AJAX de WebClient debe realizarse correctamente. Se permiten los métodos GET, PUT y POST.

![Explorador Web que muestra el mensaje de prueba correcto](enabling-cross-origin-requests-in-web-api/_static/image9.png)

## <a name="how-cors-works"></a>Cómo funciona CORS

En esta sección se describe lo que sucede en una solicitud de CORS, en el nivel de los mensajes HTTP. Es importante comprender cómo funciona CORS, de modo que pueda configurar el atributo **[EnableCors]** correctamente y solucionar los problemas si no funciona como se espera.

La especificación de CORS presenta varios encabezados HTTP nuevos que permiten las solicitudes entre orígenes. Si un explorador admite CORS, establece estos encabezados automáticamente para las solicitudes entre orígenes. no es necesario hacer nada especial en el código de JavaScript.

Este es un ejemplo de una solicitud entre orígenes. El encabezado "Origin" proporciona el dominio del sitio que realiza la solicitud.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample6.cmd?highlight=5)]

Si el servidor permite la solicitud, establece el encabezado Access-Control-Allow-Origin. El valor de este encabezado coincide con el encabezado de origen o es el valor de carácter comodín "\*", lo que significa que se permite cualquier origen.

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample7.cmd?highlight=5)]

Si la respuesta no incluye el encabezado Access-Control-Allow-Origin, se produce un error en la solicitud AJAX. En concreto, el explorador no permite la solicitud. Aunque el servidor devuelva una respuesta correcta, el explorador no pone la respuesta a disposición de la aplicación cliente.

**Solicitudes preparatorias**

En algunas solicitudes de CORS, el explorador envía una solicitud adicional, denominada "solicitud preparatoria", antes de enviar la solicitud real para el recurso.

El explorador puede omitir la solicitud preparatoria si se cumplen las condiciones siguientes:

- El método de solicitud es GET, HEAD o POST, *y*
- La aplicación no establece ningún encabezado de solicitud que no sea Accept, Accept-Language, Content-Language, Content-Type o Last-Event-ID, *y*
- El encabezado Content-Type (si se establece) es uno de los siguientes:

    - application/x-www-form-urlencoded
    - multipart/form-data
    - text/plain

La regla sobre los encabezados de solicitud se aplica a los encabezados que establece la aplicación mediante una llamada a **setRequestHeader** en el objeto **XMLHttpRequest** . (La especificación CORS llama a estos "encabezados de solicitud de autor"). La regla no se aplica a los encabezados que el *Explorador* puede establecer, como el agente de usuario, el host o la longitud del contenido.

Este es un ejemplo de una solicitud preparatoria:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample8.cmd?highlight=4-5)]

La solicitud previa al vuelo usa el método HTTP OPTIONs. Incluye dos encabezados especiales:

- Access-Control-Request-Method: el método HTTP que se usará para la solicitud real.
- Access-Control-request-headers: una lista de encabezados de solicitud que la *aplicación* estableció en la solicitud real. (De nuevo, esto no incluye los encabezados que establece el explorador).

A continuación se muestra una respuesta de ejemplo, suponiendo que el servidor permite la solicitud:

[!code-console[Main](enabling-cross-origin-requests-in-web-api/samples/sample9.cmd?highlight=6-7)]

La respuesta incluye un encabezado Access-Control-Allow-Methods que enumera los métodos permitidos y, opcionalmente, un encabezado Access-Control-Allow-Headers, que muestra los encabezados permitidos. Si la solicitud de comprobaciones preparatorias se realiza correctamente, el explorador envía la solicitud real, como se ha descrito anteriormente.

Las herramientas que se usan normalmente para probar extremos con solicitudes de opciones preparatorias (por ejemplo, [Fiddler](https://www.telerik.com/fiddler) y [Postman](https://www.getpostman.com/)) no envían de forma predeterminada los encabezados de opciones necesarios. Confirme que los encabezados `Access-Control-Request-Method` y `Access-Control-Request-Headers` se envían con la solicitud y que los encabezados de opciones llegan a la aplicación a través de IIS.

Para configurar IIS de forma que permita que una aplicación ASP.NET reciba y controle solicitudes de opción, agregue la siguiente configuración al archivo *Web. config* de la aplicación en la sección `<system.webServer><handlers>`:

```xml
<system.webServer>
  <handlers>
    <remove name="ExtensionlessUrlHandler-Integrated-4.0" />
    <remove name="OPTIONSVerbHandler" />
    <add name="ExtensionlessUrlHandler-Integrated-4.0" path="*." verb="*" type="System.Web.Handlers.TransferRequestHandler" preCondition="integratedMode,runtimeVersionv4.0" />
  </handlers>
</system.webServer>
```

La eliminación de `OPTIONSVerbHandler` impide que IIS controle las solicitudes OPTIONs. El reemplazo de `ExtensionlessUrlHandler-Integrated-4.0` permite que las solicitudes de opciones lleguen a la aplicación porque el registro del módulo predeterminado solo permite solicitudes GET, HEAD, POST y DEBUG con direcciones URL sin extensión.

## <a name="scope-rules-for-enablecors"></a>Reglas de ámbito para [EnableCors]

Puede habilitar CORS por acción, por controlador o globalmente para todos los controladores de la API Web de la aplicación.

**Por acción**

Para habilitar CORS para una única acción, establezca el atributo **[EnableCors]** en el método de acción. En el ejemplo siguiente se habilita CORS solo para el método `GetItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample10.cs)]

**Por controlador**

Si establece **[EnableCors]** en la clase del controlador, se aplica a todas las acciones del controlador. Para deshabilitar CORS para una acción, agregue el atributo **[DisableCors]** a la acción. En el ejemplo siguiente se habilita CORS para cada método excepto `PutItem`.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample11.cs)]

**Globalmente**

Para habilitar CORS para todos los controladores de API Web de la aplicación, pase una instancia de **EnableCorsAttribute** al método **EnableCors** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample12.cs)]

Si establece el atributo en más de un ámbito, el orden de prioridad es:

1. Acción
2. Controlador
3. Global

## <a name="set-the-allowed-origins"></a>Establecer los orígenes permitidos

El *parámetro* Origins del atributo **[EnableCors]** especifica qué orígenes tienen permiso para acceder al recurso. El valor es una lista separada por comas de los orígenes permitidos.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample13.cs)]

También puede usar el valor de carácter comodín "\*" para permitir las solicitudes de cualquier origen.

Considere detenidamente antes de permitir solicitudes de cualquier origen. Esto significa que, literalmente, cualquier sitio web puede realizar llamadas AJAX a la API Web.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample14.cs)]

## <a name="set-the-allowed-http-methods"></a>Establecer los métodos HTTP permitidos

El parámetro *Methods* del atributo **[EnableCors]** especifica qué métodos http pueden tener acceso al recurso. Para permitir todos los métodos, use el valor de carácter comodín "\*". En el ejemplo siguiente solo se permiten las solicitudes GET y POST.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample15.cs)]

## <a name="set-the-allowed-request-headers"></a>Establecer los encabezados de solicitudes permitidos

En este artículo se ha descrito anteriormente cómo una solicitud preparatoria podría incluir un encabezado Access-Control-request-headers, donde se enumeran los encabezados HTTP establecidos por la aplicación (denominados "encabezados de solicitud de autor"). El parámetro *headers* del atributo **[EnableCors]** especifica qué encabezados de solicitud de autor se permiten. Para permitir cualquier encabezado, establezca los *encabezados* en "\*". Para incluir encabezados específicos en la lista de permitidos, establezca los *encabezados* en una lista separada por comas de los encabezados permitidos:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample16.cs)]

Sin embargo, los exploradores no son totalmente coherentes en cómo establecen los encabezados Access-Control-request-. Por ejemplo, Chrome actualmente incluye "Origin". FireFox no incluye encabezados estándar, como "Accept", incluso cuando la aplicación los establece en un script.

Si establece *encabezados* en un valor distinto de "\*", debe incluir al menos "Accept", "Content-Type" y "Origin", además de los encabezados personalizados que desee admitir.

## <a name="set-the-allowed-response-headers"></a>Establecer los encabezados de respuesta permitidos

De forma predeterminada, el explorador no expone todos los encabezados de respuesta a la aplicación. Los encabezados de respuesta que están disponibles de forma predeterminada son:

- Cache-Control
- Content-Language
- Content-Type
- Expires
- Last-Modified
- Omiti

La especificación CORS llama a estos [encabezados de respuesta sencillos](https://dvcs.w3.org/hg/cors/raw-file/tip/Overview.html#simple-response-header). Para que otros encabezados estén disponibles para la aplicación, establezca el parámetro *exposedHeaders* de **[EnableCors]** .

En el ejemplo siguiente, el método `Get` del controlador establece un encabezado personalizado denominado ' X-custom-header '. De forma predeterminada, el explorador no expondrá este encabezado en una solicitud entre orígenes. Para que el encabezado esté disponible, incluya "X-custom-header" en *exposedHeaders*.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample17.cs)]

## <a name="pass-credentials-in-cross-origin-requests"></a>Pasar credenciales en solicitudes entre orígenes

Las credenciales requieren un tratamiento especial en una solicitud de CORS. De forma predeterminada, el explorador no envía ninguna credencial con una solicitud entre orígenes. Las credenciales incluyen cookies y esquemas de autenticación HTTP. Para enviar credenciales con una solicitud entre orígenes, el cliente debe establecer **XMLHttpRequest. withCredentials** en true.

Usar **XMLHttpRequest** directamente:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample18.cs)]

En jQuery:

[!code-javascript[Main](enabling-cross-origin-requests-in-web-api/samples/sample19.js)]

Además, el servidor debe permitir las credenciales. Para permitir las credenciales entre orígenes en Web API, establezca la propiedad **SupportsCredentials** en true en el atributo **[EnableCors]** :

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample20.cs)]

Si esta propiedad es true, la respuesta HTTP incluirá un encabezado Access-Control-Allow-Credentials. Este encabezado indica al explorador que el servidor permite credenciales para una solicitud entre orígenes.

Si el explorador envía credenciales, pero la respuesta no incluye un encabezado Access-Control-Allow-Credentials válido, el explorador no expondrá la respuesta a la aplicación y se producirá un error en la solicitud de AJAX.

Tenga cuidado al establecer **SupportsCredentials** en true, ya que significa que un sitio web en otro dominio puede enviar las credenciales del usuario que ha iniciado sesión a la API Web en nombre del usuario, sin que el usuario sea consciente. La especificación CORS también indica que el establecimiento de *orígenes* en &quot;\*&quot; no es válido si **SupportsCredentials** es true.

## <a name="custom-cors-policy-providers"></a>Proveedores de directivas de CORS personalizados

El atributo **[EnableCors]** implementa la interfaz **ICorsPolicyProvider** . Puede proporcionar su propia implementación creando una clase que derive de **Attribute** e implemente **ICorsPolicyProvider**.

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample21.cs)]

Ahora puede aplicar el atributo a cualquier lugar que colocaría **[EnableCors]** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample22.cs)]

Por ejemplo, un proveedor de directivas de CORS personalizado podría leer la configuración de un archivo de configuración.

Como alternativa al uso de atributos, puede registrar un objeto **ICorsPolicyProviderFactory** que cree objetos **ICorsPolicyProvider** .

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample23.cs)]

Para establecer **ICorsPolicyProviderFactory**, llame al método de extensión **SetCorsPolicyProviderFactory** en el inicio, como se indica a continuación:

[!code-csharp[Main](enabling-cross-origin-requests-in-web-api/samples/sample24.cs)]

## <a name="browser-support"></a>Compatibilidad con exploradores

El paquete CORS de la API Web es una tecnología del lado servidor. El explorador del usuario también debe admitir CORS. Afortunadamente, las versiones actuales de todos los exploradores principales incluyen la [compatibilidad con CORS](http://caniuse.com/cors).
