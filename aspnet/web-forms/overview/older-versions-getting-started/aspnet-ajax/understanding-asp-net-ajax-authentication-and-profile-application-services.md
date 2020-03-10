---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Descripción de la autenticación y el Servicios de aplicación de perfiles de ASP.NET AJAX | Microsoft Docs
author: scottcate
description: El servicio de autenticación permite a los usuarios proporcionar credenciales para recibir una cookie de autenticación y es el servicio de puerta de enlace para permitir el usuario personalizado...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: cab9acb1ffd75cca87f6c575a6abdd000235828e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520315"
---
# <a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Descripción de la autenticación de ASP.NET AJAX y los servicios de aplicaciones de perfiles

por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> El servicio de autenticación permite a los usuarios proporcionar credenciales para recibir una cookie de autenticación y es el servicio de puerta de enlace para permitir perfiles de usuario personalizados proporcionados por ASP.NET. El uso del servicio de autenticación de AJAX de ASP.NET es compatible con la autenticación de formularios estándar de ASP.NET, por lo que las aplicaciones que usan actualmente la autenticación de formularios (por ejemplo, con el control de inicio de sesión) no se interrumpirán actualizando al servicio de autenticación de AJAX.

## <a name="introduction"></a>Introducción

Como parte de la .NET Framework 3,5, Microsoft ofrece una actualización de entorno de tamaño ajustable. no solo hay disponible un nuevo entorno de desarrollo, pero las nuevas características de Language-Integrated Query (LINQ) y otras mejoras de lenguaje están disponibles. Además, algunas características conocidas de otros conjuntos de herramientas, sobre todo las extensiones AJAX de ASP.NET, se incluyen como miembros de primera clase de la biblioteca de clases base .NET Framework. Estas extensiones permiten muchas características nuevas de cliente enriquecidas, incluida la representación parcial de páginas sin necesidad de una actualización de página completa, la capacidad de obtener acceso a los servicios web a través de scripts de cliente (incluida la API de generación de perfiles de ASP.NET) y una amplia API del lado cliente. diseñado para reflejar muchos de los esquemas de control que se han detectado en el conjunto de controles del servidor de ASP.NET.

En estas notas del producto se examina la implementación y el uso de los servicios de generación de perfiles de ASP.NET y de autenticación de formularios, ya que los Microsoft ASP.NET AJAX ExtensionsThe Ajax Extensions hacen que la autenticación de formularios sea increíblemente fácil de admitir, como esto (así como el servicio de generación de perfiles) se expone a través de un script de proxy de servicio Web. Las extensiones AJAX también admiten la autenticación personalizada a través de la clase AuthenticationServiceManager.

Estas notas del producto se basan en la versión beta 2 de Visual Studio 2008 y el .NET Framework 3,5. En estas notas del producto también se da por supuesto que va a trabajar con Visual Studio 2008 beta 2, no con Visual Web Developer Express, y proporcionará tutoriales de acuerdo con la interfaz de usuario de Visual Studio. Algunos ejemplos de código pueden emplear plantillas de proyecto que no están disponibles en Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Perfiles y autenticación*

Los servicios de autenticación y perfiles de Microsoft ASP.NET los proporciona el sistema de autenticación de formularios ASP.NET y son componentes estándar de ASP.NET. Las extensiones de AJAX de ASP.NET proporcionan acceso de script a estos servicios a través de proxies de scripts, a través de un modelo bastante sencillo en el espacio de nombres sys. Services de la biblioteca AJAX de cliente.

El servicio de autenticación permite a los usuarios proporcionar credenciales para recibir una cookie de autenticación y es el servicio de puerta de enlace para permitir perfiles de usuario personalizados proporcionados por ASP.NET. El uso del servicio de autenticación de AJAX de ASP.NET es compatible con la autenticación de formularios estándar de ASP.NET, por lo que las aplicaciones que usan actualmente la autenticación de formularios (por ejemplo, con el control de inicio de sesión) no se interrumpirán actualizando al servicio de autenticación de AJAX.

El servicio de perfiles permite la integración automática y el almacenamiento de datos de usuario en función de la pertenencia proporcionada por el servicio de autenticación. Los datos almacenados se especifican mediante el archivo Web. config y los diversos proveedores de servicios de generación de perfiles controlan la administración de datos. Al igual que con el servicio de autenticación, el servicio de perfiles de AJAX es compatible con el servicio de Perfil de ASP.NET estándar, por lo que las páginas que actualmente incorporan características del servicio de Perfil de ASP.NET no deben romperse incluyendo la compatibilidad con AJAX.

La incorporación de los servicios de autenticación y generación de perfiles de ASP.NET en una aplicación está fuera del ámbito de esta documentación. Para obtener más información sobre el tema, consulte el artículo de referencia de MSDN Library administrar usuarios mediante la pertenencia a [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET también incluye una utilidad para configurar automáticamente la pertenencia a un SQL Server, que es el proveedor de servicios de autenticación predeterminado para la pertenencia a ASP.NET. Para obtener más información, vea el artículo ASP.NET SQL Server registration Tool (ASPNET\_regsql. exe) en [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Usar el servicio de autenticación de AJAX de ASP.NET*

El servicio de autenticación de AJAX de ASP.NET debe estar habilitado en el archivo Web. config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

El servicio de autenticación requiere que se habilite la autenticación de formularios ASP.NET y requiere que se habiliten las cookies en el explorador del cliente (un script no puede habilitar una sesión sin cookies, ya que las sesiones sin cookies requieren parámetros de dirección URL).

Una vez habilitado y configurado el servicio de autenticación de AJAX, el script de cliente puede aprovechar de inmediato el objeto sys. Services. AuthenticationService. Principalmente, el script de cliente querrá aprovechar las ventajas del método `login` y de `isLoggedIn` propiedad. Existen varias propiedades que proporcionan valores predeterminados para el método login, que puede aceptar un gran número de parámetros.

*Sys. Services. AuthenticationService (miembros)*

*método de inicio de sesión:*

El método login () comienza una solicitud para autenticar las credenciales del usuario. Este método es asincrónico y no bloquea la ejecución.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| userName | Obligatorio. Nombre de usuario que se va a autenticar. |
| contraseña | Opcional (el valor predeterminado es null). Contraseña del usuario. |
| isPersistent | Opcional (el valor predeterminado es false). Si la cookie de autenticación del usuario debe persistir entre sesiones. Si es false, el usuario cerrará la sesión cuando se cierre el explorador o expire la sesión. |
| redirectUrl | Opcional (el valor predeterminado es null). Dirección URL a la que se redirigirá el explorador tras una autenticación correcta. Si este parámetro es null o una cadena vacía, no se produce ninguna redirección. |
| customInfo | Opcional (el valor predeterminado es null). Este parámetro no se usa actualmente y se reserva para uso futuro. |
| loginCompletedCallback | Opcional (el valor predeterminado es null). Función a la que se va a llamar cuando el inicio de sesión se haya completado correctamente. Si se especifica, este parámetro reemplaza la propiedad defaultLoginCompleted. |
| failedCallback | Opcional (el valor predeterminado es null). Función a la que se llamará cuando se haya producido un error en el inicio de sesión. Si se especifica, este parámetro invalida la propiedad defaultFailedCallback. |
| userContext | Opcional (el valor predeterminado es null). Datos de contexto de usuario personalizados que se deben pasar a las funciones de devolución de llamada. |

*Return Value* (Valor devuelto):

Esta función no incluye un valor devuelto. Sin embargo, se incluyen varios comportamientos al finalizar una llamada a esta función:

- La página actual se actualizará o se cambiará si el parámetro `redirectUrl` no era null ni una cadena vacía.
- Sin embargo, si el parámetro era null o una cadena vacía, se llama al parámetro `loginCompletedCallback` o a `defaultLoginCompletedCallback` propiedad.
- Si se produce un error en la llamada al servicio Web, se llama al parámetro `failedCallback` de `defaultFailedCallback` propiedad.

*método de cierre de sesión:*

El método Logout () quita la cookie de credenciales y cierra la sesión del usuario actual de la aplicación Web.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| redirectUrl | Opcional (el valor predeterminado es null). Dirección URL a la que se redirigirá el explorador tras una autenticación correcta. Si este parámetro es null o una cadena vacía, no se produce ninguna redirección. |
| logoutCompletedCallback | Opcional (el valor predeterminado es null). Función a la que se va a llamar cuando se haya completado correctamente el cierre de sesión. Si se especifica, este parámetro reemplaza la propiedad defaultLogoutCompleted. |
| failedCallback | Opcional (el valor predeterminado es null). Función a la que se llamará cuando se haya producido un error en el inicio de sesión. Si se especifica, este parámetro invalida la propiedad defaultFailedCallback. |
| userContext | Opcional (el valor predeterminado es null). Datos de contexto de usuario personalizados que se deben pasar a las funciones de devolución de llamada. |

*Return Value* (Valor devuelto):

Esta función no incluye un valor devuelto. Sin embargo, se incluyen varios comportamientos al finalizar una llamada a esta función:

- La página actual se actualizará o se cambiará si el parámetro `redirectUrl` no era null ni una cadena vacía.
- Sin embargo, si el parámetro era null o una cadena vacía, se llama al parámetro `logoutCompletedCallback` o a `defaultLogoutCompletedCallback` propiedad.
- Si se produce un error en la llamada al servicio Web, se llama al parámetro `failedCallback` de `defaultFailedCallback` propiedad.

*propiedad defaultFailedCallback (GET, Set):*

Esta propiedad especifica una función a la que se debe llamar si se produce un error al comunicar con el servicio Web. Debe recibir un delegado (o una referencia de función).

La referencia a la función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| error | Especifica la información del error. |
| userContext | Especifica la información de contexto de usuario proporcionada al llamar a la función login o logout. |
| methodName | Nombre del método de llamada. |

*propiedad defaultLoginCompletedCallback (GET, Set):*

Esta propiedad especifica una función a la que se debe llamar cuando se haya completado la llamada al servicio Web de inicio de sesión. Debe recibir un delegado (o una referencia de función).

La referencia a la función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| validCredentials | Especifica si el usuario proporcionó credenciales válidas. `true` si el usuario inició sesión correctamente; de lo contrario `false`. |
| userContext | Especifica la información de contexto del usuario que se proporciona cuando se llamó a la función login. |
| methodName | Nombre del método de llamada. |

*propiedad defaultLogoutCompletedCallback (GET, Set):*

Esta propiedad especifica una función a la que se debe llamar cuando se haya completado la llamada al servicio Web de cierre de sesión. Debe recibir un delegado (o una referencia de función).

La referencia a la función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| result | Este parámetro siempre será `null`; está reservado para un uso futuro. |
| userContext | Especifica la información de contexto del usuario que se proporciona cuando se llamó a la función login. |
| methodName | Nombre del método de llamada. |

*propiedad isLoggedIn (GET):*

Esta propiedad obtiene el estado de autenticación actual del usuario; lo establece el objeto ScriptManager durante una solicitud de página.

Esta propiedad devuelve `true` si el usuario ha iniciado sesión actualmente. de lo contrario, devuelve `false`.

*propiedad Path (GET, Set):*

Esta propiedad determina la ubicación del servicio Web de autenticación mediante programación. Se puede usar para invalidar el proveedor de autenticación predeterminado, así como un conjunto mediante declaración en la propiedad path del nodo secundario de AuthenticationService del control ScriptManager (para obtener más información, consulte el uso de un proveedor de servicios de autenticación personalizado). en el tema siguiente).

Tenga en cuenta que la ubicación del servicio de autenticación predeterminado no cambia. Sin embargo, ASP.NET AJAX le permite especificar la ubicación de un servicio Web que proporciona la misma interfaz de clase que el proxy del servicio de autenticación AJAX de ASP.NET.

Tenga en cuenta también que esta propiedad no se debe establecer en un valor que dirija la solicitud de script fuera del sitio actual. Dado que la aplicación actual no recibirá las credenciales de autenticación, sería inútil; Además, la tecnología AJAX subyacente no debe publicar solicitudes entre sitios y puede generar una excepción de seguridad en el explorador del cliente.

Esta propiedad es un objeto `String` que representa la ruta de acceso al servicio Web de autenticación.

*propiedad timeout (GET, Set):*

Esta propiedad determina el período de tiempo de espera para el servicio de autenticación antes de que se haya producido un error en la solicitud de inicio de sesión. Si el tiempo de espera expira mientras espera a que se complete una llamada, se llamará a la devolución de llamada de solicitud-error y no se completará la llamada.

Esta propiedad es un objeto `Number` que representa el número de milisegundos de espera de los resultados del servicio de autenticación.

*Ejemplo de código: Inicio de sesión en el servicio de autenticación*

El marcado siguiente es un ejemplo de página ASP.NET con una llamada de script simple a los métodos login y logout de la clase AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Obtener acceso a los datos de generación de perfiles ASP.NET a través de AJAX

El servicio de generación de perfiles ASP.NET también se expone a través de las extensiones AJAX de ASP.NET. Dado que el servicio de generación de perfiles ASP.NET proporciona una API enriquecida y granular para almacenar y recuperar datos de usuario, puede ser una excelente herramienta de productividad.

El servicio de perfiles debe estar habilitado en Web. config; no está de forma predeterminada. Para ello, asegúrese de que el elemento secundario `profileService` tenga habilitado = true en el archivo Web. config y de que haya especificado qué propiedades se pueden leer o escribir de la siguiente manera:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

También se debe configurar el servicio de perfiles. Aunque la configuración del servicio de generación de perfiles está fuera del ámbito de este documento, merece la pena tener en cuenta que los grupos definidos en los valores de configuración del perfil serán accesibles como subpropiedades del nombre del grupo. Por ejemplo, con la siguiente sección de perfil especificada:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

El script de cliente podría tener acceso a Name, Address. línea1, Address. línea2, Address. City, Address. State, Address. zip y BackgroundColor como propiedades del campo Properties de la clase ProfileService.

Una vez configurado el servicio de generación de perfiles de AJAX, estará inmediatamente disponible en las páginas; sin embargo, tendrá que cargarse una vez antes del uso.

*Sys. Services. ProfileService (miembros)*

*campo de propiedades:*

El campo de propiedades expone todos los datos de perfil configurados como propiedades secundarias a las que se puede hacer referencia mediante la Convención de nombre-operador-punto. Las propiedades que son elementos secundarios de los grupos de propiedades se denominan GroupName. PropertyName. En la configuración de Perfil de ejemplo mostrada anteriormente, para obtener el estado del usuario, podría usar el siguiente identificador:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Load (método):*

Carga una lista seleccionada o todas las propiedades del servidor.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (el valor predeterminado es null). Propiedades que se van a cargar desde el servidor. |
| loadCompletedCallback | Opcional (el valor predeterminado es null). Función a la que se va a llamar cuando se complete la carga. |
| failedCallback | Opcional (el valor predeterminado es null). Función a la que se llama si se produce un error. |
| userContext | Opcional (el valor predeterminado es null). Información de contexto que se va a pasar a la función de devolución de llamada. |

La función Load no tiene un valor devuelto. Si la llamada se ha realizado correctamente, llamará al parámetro `loadCompletedCallback` o a `defaultLoadCompletedCallback` propiedad. Si se produjo un error en la llamada o se agotó el tiempo de espera, se llamará al parámetro `failedCallback` o a la propiedad `defaultFailedCallback`.

Si no se proporciona el parámetro `propertyNames`, todas las propiedades configuradas por lectura se recuperan del servidor.

*método Save:*

El método Save () guarda la lista de propiedades especificada (o todas las propiedades) en el perfil ASP.NET del usuario.

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| propertyNames | Opcional (el valor predeterminado es null). Propiedades que se van a guardar en el servidor. |
| saveCompletedCallback | Opcional (el valor predeterminado es null). Función a la que se llama cuando se ha completado el guardado. |
| failedCallback | Opcional (el valor predeterminado es null). Función a la que se llama si se produce un error. |
| userContext | Opcional (el valor predeterminado es null). Información de contexto que se va a pasar a la función de devolución de llamada. |

La función Save no tiene un valor devuelto. Si la llamada se completa correctamente, llamará al parámetro `saveCompletedCallback` o `defaultSaveCompletedCallback` propiedad. Si se produjo un error en la llamada o se agotó el tiempo de espera, se llamará a la propiedad `failedCallback` o `defaultFailedCallback`.

Si el parámetro `propertyNames` es null, todas las propiedades de perfil se enviarán al servidor y el servidor decidirá qué propiedades se pueden guardar y cuáles no.

*propiedad defaultFailedCallback (GET, Set):*

Esta propiedad especifica una función a la que se debe llamar si se produce un error al comunicar con el servicio Web. Debe recibir un delegado (o una referencia de función).

La referencia a la función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| Error | Especifica la información del error. |
| userContext | Especifica la información de contexto de usuario proporcionada al llamar a la función de carga o guardado. |
| methodName | Nombre del método de llamada. |

*propiedad defaultSaveCompleted (GET, Set):*

Esta propiedad especifica una función a la que se debe llamar cuando se complete la operación de guardar los datos de perfil del usuario. Debe recibir un delegado (o una referencia de función).

La referencia a la función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| numPropsSaved | Especifica el número de propiedades que se guardaron. |
| userContext | Especifica la información de contexto de usuario proporcionada al llamar a la función de carga o guardado. |
| methodName | Nombre del método de llamada. |

*propiedad defaultLoadCompleted (GET, Set):*

Esta propiedad especifica una función a la que se debe llamar cuando se complete la carga de los datos de perfil del usuario. Debe recibir un delegado (o una referencia de función).

La referencia a la función especificada por esta propiedad debe tener la siguiente firma:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parámetros:*

| **Nombre de parámetro** | **Significado** |
| --- | --- |
| numPropsLoaded | Especifica el número de propiedades cargadas. |
| userContext | Especifica la información de contexto de usuario proporcionada al llamar a la función de carga o guardado. |
| methodName | Nombre del método de llamada. |

*propiedad Path (GET, Set):*

Esta propiedad determina la ubicación del servicio Web de perfil mediante programación. Se puede usar para invalidar el proveedor de servicios de perfil predeterminado, así como un conjunto mediante declaración en la propiedad path del nodo secundario ProfileService del control ScriptManager.

Tenga en cuenta que la ubicación del servicio de perfiles predeterminado no cambia. Sin embargo, ASP.NET AJAX le permite especificar la ubicación de un servicio Web que proporciona la misma interfaz de clase que el proxy del servicio de autenticación AJAX de ASP.NET.

Tenga en cuenta también que esta propiedad no se debe establecer en un valor que dirija la solicitud de script fuera del sitio actual. La tecnología AJAX subyacente no debe publicar solicitudes entre sitios y puede generar una excepción de seguridad en el explorador del cliente.

Esta propiedad es un objeto `String` que representa la ruta de acceso al servicio Web de perfil.

*propiedad timeout (GET, Set):*

Esta propiedad determina el período de tiempo de espera para el servicio de perfil antes de suponer que se ha producido un error en la solicitud de carga o guardado. Si el tiempo de espera expira mientras espera a que se complete una llamada, se llamará a la devolución de llamada de solicitud-error y no se completará la llamada.

Esta propiedad es un objeto `Number` que representa el número de milisegundos de espera de los resultados del servicio de perfil.

*Ejemplo de código: cargar datos de perfil en la carga de página*

El código siguiente comprobará si un usuario está autenticado y, en caso afirmativo, cargará el color de fondo preferido del usuario como la de la página.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Uso de un proveedor de servicios de autenticación personalizado*

Las extensiones de AJAX de ASP.NET permiten crear un proveedor de servicios de autenticación de scripts personalizados mediante la exposición de la funcionalidad a través de un servicio web personalizado. Para que se pueda usar, el servicio Web debe exponer dos métodos, `Login` y `Logout`; y estos métodos deben especificarse con las mismas firmas de método que el servicio Web de autenticación AJAX predeterminado de ASP.NET.

Una vez que haya creado el servicio web personalizado, deberá especificar la ruta de acceso, ya sea mediante declaración en la página, mediante programación en código o mediante el script de cliente.

*Para establecer la ruta de acceso mediante declaración:*

Para establecer la ruta de acceso mediante declaración, incluya el elemento secundario AuthenticationService del objeto ScriptManager en la página ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Para establecer la ruta de acceso en el código:*

Para establecer la ruta de acceso mediante programación, especifique la ruta de acceso a través de la instancia del administrador de scripts:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Para establecer la ruta de acceso en el script:*

Para establecer la ruta de acceso mediante programación en script, use la propiedad `path` de la clase AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Servicio Web de ejemplo para la autenticación personalizada*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Resumen

ASP.NET Services: en concreto, los servicios de generación de perfiles, pertenencia y autenticación, se exponen fácilmente a JavaScript en el explorador del cliente. Esto permite a los desarrolladores integrar el código del lado cliente con el mecanismo de autenticación sin problemas, sin depender de controles como UpdatePanel para realizar el trabajo pesado. También se pueden proteger los datos de perfil del cliente mediante la utilización de la configuración Web; no hay datos disponibles de forma predeterminada, y los desarrolladores deben participar en las propiedades del perfil.

Además, al crear implementaciones de servicio Web simplificadas con firmas de método equivalentes, los desarrolladores pueden crear proveedores de scripts personalizados para estos servicios intrínsecos de ASP.NET. La compatibilidad con estas técnicas simplifica el desarrollo de aplicaciones cliente enriquecidas, a la vez que proporciona a los desarrolladores una amplia gama de flexibilidad para satisfacer las necesidades específicas.

## <a name="bio"></a>*Disponibilidad*

Scott Cate ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el Presidente de myKB.com ([www.myKB.com](http://www.myKB.com)), donde se especializa en escribir aplicaciones basadas en ASP.net centradas en las soluciones de software de la base de conocimiento. Puede ponerse en contacto con Scott por correo electrónico en [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Siguiente](understanding-asp-net-ajax-localization.md)
