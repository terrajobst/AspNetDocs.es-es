---
uid: signalr/overview/older-versions/introduction-to-security
title: Introducción a la seguridad de Signalr (Signalr 1. x) | Microsoft Docs
author: bradygaster
description: Describe los problemas de seguridad que debe tener en cuenta al desarrollar una aplicación Signalr.
ms.author: bradyg
ms.date: 10/17/2013
ms.assetid: 715a4059-d307-4631-abbb-c789c95d6eb4
msc.legacyurl: /signalr/overview/older-versions/introduction-to-security
msc.type: authoredcontent
ms.openlocfilehash: 34172c0a2a15a7ab0d782704d5831ce236f5c989
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78431353"
---
# <a name="introduction-to-signalr-security-signalr-1x"></a>Introducción a la seguridad de SignalR (SignalR 1.x)

por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este artículo se describen los problemas de seguridad que debe tener en cuenta al desarrollar una aplicación Signalr.

## <a name="overview"></a>Información general

Este documento contiene las siguientes secciones:

- [Conceptos de seguridad de signalr](#concepts)

    - [Autenticación y autorización](#authentication)
    - [Token de conexión](#connectiontoken)
    - [Volver a unir grupos cuando se vuelve a conectar](#rejoingroup)
- [Cómo Signalr evita la falsificación de solicitudes entre sitios](#csrf)
- [Recomendaciones de seguridad de signalr](#recommendations)

    - [Protocolo de capas de sockets seguros (SSL)](#ssl)
    - [No usar grupos como mecanismo de seguridad](#groupsecurity)
    - [Controlar de forma segura la entrada desde clientes](#input)
    - [Reconciliar un cambio en el estado de usuario con una conexión activa](#reconcile)
    - [Archivos de proxy de JavaScript generados automáticamente](#autogen)
    - [Excepciones](#exceptions)

<a id="concepts"></a>

## <a name="signalr-security-concepts"></a>Conceptos de seguridad de signalr

<a id="authentication"></a>

### <a name="authentication-and-authorization"></a>Autenticación y autorización

Signalr está diseñado para integrarse en la estructura de autenticación existente de una aplicación. No proporciona ninguna característica para autenticar a los usuarios. En su lugar, debe autenticar a los usuarios como lo haría normalmente en la aplicación y, a continuación, trabajar con los resultados de la autenticación en el código de Signalr. Por ejemplo, puede autenticar a los usuarios con la autenticación de formularios de ASP.NET y, a continuación, en el concentrador, exigir qué usuarios o roles están autorizados a llamar a un método. En el centro, también puede pasar información de autenticación, como el nombre de usuario o si un usuario pertenece a un rol, al cliente.

Signalr proporciona el atributo [Authorize](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.authorizeattribute(v=vs.111).aspx) para especificar qué usuarios tienen acceso a un concentrador o método. El atributo Authorize se aplica a un concentrador o a métodos concretos de un concentrador. Sin el atributo Authorize, todos los métodos públicos del centro están disponibles para un cliente que está conectado al centro. Para obtener más información sobre los concentradores, consulte [autenticación y autorización de los concentradores de signalr](../security/hub-authorization.md).

El atributo `Authorize` se usa solo con hubs. Para aplicar las reglas de autorización cuando se usa un `PersistentConnection` debe invalidar el método `AuthorizeRequest`. Para obtener más información acerca de las conexiones persistentes, consulte [autenticación y autorización para las conexiones persistentes de signalr](../security/persistent-connection-authorization.md).

<a id="connectiontoken"></a>

### <a name="connection-token"></a>Token de conexión

Signalr mitiga el riesgo de ejecutar comandos malintencionados mediante la validación de la identidad del remitente. Un token de conexión que contiene el identificador de conexión y el nombre de usuario para los usuarios autenticados se pasa entre el cliente y el servidor para cada solicitud. El identificador de conexión es un identificador único que el servidor genera aleatoriamente cuando se crea una nueva conexión y se conserva mientras dure la conexión. El nombre de usuario lo proporciona el mecanismo de autenticación para la aplicación Web. El token de conexión está protegido con cifrado y una firma digital.

![](introduction-to-security/_static/image2.png)

Para cada solicitud, el servidor valida el contenido del token para asegurarse de que la solicitud proviene del usuario especificado. El nombre de usuario debe corresponder al identificador de conexión. Al validar el identificador de conexión y el nombre de usuario, Signalr impide que un usuario malintencionado suplante a otro usuario fácilmente. Si el servidor no puede validar el token de conexión, se produce un error en la solicitud.

![](introduction-to-security/_static/image4.png)

Dado que el identificador de conexión forma parte del proceso de comprobación, no debe revelar el identificador de conexión de un usuario a otros usuarios ni almacenar el valor en el cliente, como en una cookie.

<a id="rejoingroup"></a>

### <a name="rejoining-groups-when-reconnecting"></a>Volver a unir grupos cuando se vuelve a conectar

De forma predeterminada, la aplicación Signalr vuelve a asignar automáticamente un usuario a los grupos adecuados cuando se vuelve a conectar desde una interrupción temporal, como cuando una conexión se quita y se vuelve a establecer antes de que se agote el tiempo de espera de la conexión. Al volver a conectarse, el cliente pasa un token de grupo que incluye el identificador de conexión y los grupos asignados. El token de grupo está firmado digitalmente y cifrado. El cliente conserva el mismo identificador de conexión después de una reconexión; por lo tanto, el identificador de conexión que se pasa desde el cliente reconectado debe coincidir con el identificador de conexión anterior usado por el cliente. Esta comprobación impide que un usuario malintencionado pase solicitudes para unirse a grupos no autorizados al volver a conectar.

Sin embargo, es importante tener en cuenta que el token de grupo no expira. Si un usuario pertenecía a un grupo en el pasado, pero se ha vetado de ese grupo, es posible que el usuario pueda imitar un token de grupo que incluya el grupo prohibido. Si necesita administrar de forma segura los usuarios que pertenecen a cada grupo, debe almacenar los datos en el servidor, como en una base de datos de. A continuación, agregue lógica a la aplicación que se comprueba en el servidor si un usuario pertenece a un grupo. Para obtener un ejemplo de comprobación de la pertenencia a grupos, consulte [trabajar con grupos](../guide-to-the-api/working-with-groups.md).

La reunión automática de grupos solo se aplica cuando se vuelve a conectar una conexión después de una interrupción temporal. Si un usuario se desconecta navegando fuera de la aplicación o se reinicia la aplicación, la aplicación debe controlar cómo agregar ese usuario a los grupos correctos. Para obtener más información, vea [trabajar con grupos](../guide-to-the-api/working-with-groups.md).

<a id="csrf"></a>

## <a name="how-signalr-prevents-cross-site-request-forgery"></a>Cómo Signalr evita la falsificación de solicitudes entre sitios

La falsificación de solicitudes entre sitios (CSRF) es un ataque en el que un sitio malintencionado envía una solicitud a un sitio vulnerable en el que el usuario ha iniciado sesión actualmente. Signalr impide CSRF, lo que hace que sea muy improbable que un sitio malintencionado cree una solicitud válida para la aplicación Signalr.

### <a name="description-of-csrf-attack"></a>Descripción del ataque CSRF

A continuación se muestra un ejemplo de un ataque CSRF:

1. Un usuario inicia sesión en `www.example.com`mediante la autenticación de formularios.
2. El servidor autentica al usuario. La respuesta del servidor incluye una cookie de autenticación.
3. Sin cerrar la sesión, el usuario visita un sitio Web malintencionado. Este sitio malintencionado contiene el siguiente formulario HTML: 

    [!code-html[Main](introduction-to-security/samples/sample1.html)]

   Tenga en cuenta que la acción del formulario se publica en el sitio vulnerable, no en el sitio malintencionado. Esta es la parte "entre sitios" de CSRF.
4. El usuario hace clic en el botón Enviar. El explorador incluye la cookie de autenticación con la solicitud.
5. La solicitud se ejecuta en el servidor de example.com con el contexto de autenticación del usuario y puede hacer todo lo que se permite a un usuario autenticado.

Aunque este ejemplo requiere que el usuario haga clic en el botón de formulario, la página malintencionada podría ejecutar fácilmente un script que envíe una solicitud AJAX a la aplicación Signalr. Además, el uso de SSL no evita un ataque CSRF, ya que el sitio malintencionado puede enviar una solicitud "https://".

Normalmente, los ataques CSRF son posibles en sitios web que usan cookies para la autenticación, ya que los exploradores envían todas las cookies pertinentes al sitio web de destino. Sin embargo, los ataques CSRF no se limitan a aprovechar las cookies. Por ejemplo, la autenticación básica e implícita también son vulnerables. Después de que un usuario inicie sesión con la autenticación básica o implícita, el explorador enviará automáticamente las credenciales hasta que finalice la sesión.

### <a name="csrf-mitigations-taken-by-signalr"></a>Mitigaciones de CSRF tomadas por Signalr

Signalr realiza los pasos siguientes para evitar que un sitio malintencionado cree solicitudes válidas para la aplicación Signalr. Estos pasos se realizan de forma predeterminada y no requieren ninguna acción en el código.

- **Deshabilitar solicitudes entre dominios**  
 De forma predeterminada, las solicitudes entre dominios están deshabilitadas en una aplicación de Signalr para impedir que los usuarios llamen a un punto de conexión de Signalr desde un dominio externo. Cualquier solicitud que procede de un dominio externo se considera automáticamente no válida y está bloqueada. Se recomienda mantener este comportamiento predeterminado; de lo contrario, un sitio malintencionado podría engañar a los usuarios para que envíen comandos al sitio. Si necesita usar solicitudes entre dominios, vea [cómo establecer una conexión entre dominios](../guide-to-the-api/hubs-api-guide-javascript-client.md#crossdomain) .
- **Pasar el token de conexión en la cadena de consulta, no en la cookie**  
 Signalr pasa el token de conexión como un valor de cadena de consulta, en lugar de como una cookie. Al no almacenar el token de conexión como una cookie, el explorador no reenvía involuntariamente el token de conexión cuando se encuentra código malintencionado. Además, el token de conexión no se conserva más allá de la conexión actual. Por lo tanto, un usuario malintencionado no puede hacer una solicitud con las credenciales de autenticación de otro usuario.
- **Comprobar el token de conexión**  
 Como se describe en la sección [token de conexión](#connectiontoken) , el servidor sabe qué identificador de conexión está asociado a cada usuario autenticado. El servidor no procesa ninguna solicitud de un identificador de conexión que no coincida con el nombre de usuario. Es improbable que un usuario malintencionado pueda adivinar una solicitud válida porque el usuario malintencionado tendría que conocer el nombre de usuario y el identificador de conexión generado aleatoriamente actual. Ese identificador de conexión deja de ser válido en cuanto finaliza la conexión. Los usuarios anónimos no deben tener acceso a información confidencial.

<a id="recommendations"></a>

## <a name="signalr-security-recommendations"></a>Recomendaciones de seguridad de signalr

<a id="ssl"></a>

### <a name="secure-socket-layers-ssl-protocol"></a>Protocolo de capas de sockets seguros (SSL)

El protocolo SSL usa el cifrado para proteger el transporte de datos entre un cliente y un servidor. Si la aplicación Signalr transmite información confidencial entre el cliente y el servidor, use SSL para el transporte. Para obtener más información acerca de cómo configurar SSL, consulte Configuración de [SSL en IIS 7](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis).

<a id="groupsecurity"></a>

### <a name="do-not-use-groups-as-a-security-mechanism"></a>No usar grupos como mecanismo de seguridad

Los grupos son una forma cómoda de recopilar usuarios relacionados, pero no son un mecanismo seguro para limitar el acceso a información confidencial. Esto es especialmente cierto cuando los usuarios pueden volver a unirse automáticamente a los grupos durante una reconexión. En su lugar, considere la posibilidad de agregar usuarios con privilegios a un rol y limitar el acceso a un método de concentrador solo a los miembros de ese rol. Para obtener un ejemplo de restricción del acceso basado en un rol, consulte [autenticación y autorización para los concentradores de signalr](../security/hub-authorization.md). Para ver un ejemplo de cómo comprobar el acceso de usuario a los grupos cuando se vuelve a conectar, consulte [trabajar con grupos](../guide-to-the-api/working-with-groups.md).

<a id="input"></a>

### <a name="safely-handling-input-from-clients"></a>Controlar de forma segura la entrada desde clientes

Todas las entradas de clientes que están destinadas a la difusión a otros clientes se deben codificar para asegurarse de que un usuario malintencionado no envía un script a otros usuarios. Es mejor codificar los mensajes en los clientes receptores en lugar del servidor, ya que la aplicación Signalr puede tener muchos tipos diferentes de clientes. Por lo tanto, la codificación HTML funciona para un cliente web, pero no para otros tipos de clientes. Por ejemplo, un método de cliente web para mostrar un mensaje de chat administraría de forma segura el nombre de usuario y el mensaje mediante una llamada a la función `html()`.

[!code-html[Main](introduction-to-security/samples/sample2.html?highlight=3-4)]

<a id="reconcile"></a>

### <a name="reconciling-a-change-in-user-status-with-an-active-connection"></a>Reconciliar un cambio en el estado de usuario con una conexión activa

Si el estado de autenticación de un usuario cambia mientras existe una conexión activa, el usuario recibirá un error que indica que la identidad del usuario no puede cambiar durante una conexión de Signalr activa. En ese caso, la aplicación debe volver a conectarse al servidor para asegurarse de que el identificador de conexión y el nombre de usuario se coordinan. Por ejemplo, si la aplicación permite al usuario cerrar sesión mientras existe una conexión activa, el nombre de usuario de la conexión ya no coincidirá con el nombre que se pasa para la solicitud siguiente. Querrá detener la conexión antes de que el usuario cierre la sesión y reiniciarla.

Sin embargo, es importante tener en cuenta que la mayoría de las aplicaciones no tendrán que detener e iniciar la conexión manualmente. Si la aplicación redirige a los usuarios a una página independiente después de cerrar la sesión, como el comportamiento predeterminado en una aplicación de formularios Web Forms o una aplicación MVC, o actualiza la página actual después de cerrar la sesión, la conexión activa se desconecta automáticamente y no requerir cualquier acción adicional.

En el ejemplo siguiente se muestra cómo detener e iniciar una conexión cuando el estado del usuario ha cambiado.

[!code-html[Main](introduction-to-security/samples/sample3.html)]

O bien, el estado de autenticación del usuario puede cambiar si el sitio usa una expiración variable con autenticación de formularios y no hay ninguna actividad para mantener la cookie de autenticación válida. En ese caso, se cerrará la sesión del usuario y el nombre de usuario ya no coincidirá con el nombre de usuario en el token de conexión. Puede corregir este problema agregando algún script que solicite periódicamente un recurso en el servidor web para mantener la cookie de autenticación válida. En el ejemplo siguiente se muestra cómo solicitar un recurso cada 30 minutos.

[!code-javascript[Main](introduction-to-security/samples/sample4.js)]

<a id="autogen"></a>

### <a name="automatically-generated-javascript-proxy-files"></a>Archivos de proxy de JavaScript generados automáticamente

Si no desea incluir todos los concentradores y métodos en el archivo de proxy de JavaScript para cada usuario, puede deshabilitar la generación automática del archivo. Puede elegir esta opción si tiene varios concentradores y métodos, pero no desea que todos los usuarios tengan en cuenta todos los métodos. Para deshabilitar la generación automática, establezca **EnableJavaScriptProxies** en **false**.

[!code-csharp[Main](introduction-to-security/samples/sample5.cs)]

Para obtener más información acerca de los archivos de proxy de JavaScript, consulte [el proxy generado y lo que hace](../guide-to-the-api/hubs-api-guide-javascript-client.md#genproxy). <a id="exceptions"></a>

### <a name="exceptions"></a>Excepciones

Debe evitar pasar objetos de excepción a los clientes, ya que los objetos pueden exponer información confidencial a los clientes. En su lugar, llame a un método en el cliente que muestre el mensaje de error correspondiente.

[!code-csharp[Main](introduction-to-security/samples/sample6.cs)]
