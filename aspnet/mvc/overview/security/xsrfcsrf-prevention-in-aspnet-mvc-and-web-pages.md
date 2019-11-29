---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: Prevención de XSRF/CSRF en ASP.NET MVC y Web pages | Microsoft Docs
author: Rick-Anderson
description: La falsificación de solicitudes entre sitios (también conocida como XSRF o CSRF) es un ataque contra aplicaciones hospedadas en Web, por lo que un sitio Web malintencionado puede influir en la interacción...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: fb7e76101cbe6a874ddf5b3429ca2dc6d474334b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595758"
---
# <a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>Prevención de XSRF/CSRF en ASP.NET MVC y Web Pages

por [Rick Anderson]((https://twitter.com/RickAndMSFT))

> La falsificación de solicitudes entre sitios (también conocida como XSRF o CSRF) es un ataque contra aplicaciones hospedadas en Web, por lo que un sitio Web malintencionado puede influir en la interacción entre un explorador cliente y un sitio web de confianza para ese explorador. Estos ataques son posibles porque los exploradores Web enviarán tokens de autenticación automáticamente con cada solicitud a un sitio Web. El ejemplo canónico es una cookie de autenticación, como ASP. Vale de autenticación de formularios de la red. Sin embargo, los sitios web que utilizan cualquier mecanismo de autenticación persistente (como la autenticación de Windows, básica, etc.) pueden ser objeto de estos ataques.
> 
> Un ataque de XSRF es distinto de un ataque de suplantación de identidad (phishing). Los ataques de suplantación de identidad requieren la interacción de la víctima. En un ataque de suplantación de identidad (phishing), un sitio Web malintencionado imitará el sitio web de destino y se le engañará a proporcionar información confidencial al atacante. En un ataque XSRF, a menudo no hay ninguna interacción necesaria para el sujeto. En su lugar, el atacante confía en el explorador enviando automáticamente todas las cookies pertinentes al sitio web de destino.
> 
> Para obtener más información, vea el [proyecto Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)).

## <a name="anatomy-of-an-attack"></a>Anatomía de un ataque

Para recorrer un ataque de XSRF, considere la posibilidad de que un usuario desee realizar algunas transacciones bancarias en línea. Este usuario visita por primera vez WoodgroveBank.com e inicia sesión, momento en el que el encabezado de respuesta contendrá su cookie de autenticación:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

Dado que la cookie de autenticación es una cookie de sesión, el explorador la borrará automáticamente cuando se cierre el proceso del explorador. Sin embargo, hasta ese momento, el explorador incluirá automáticamente la cookie con cada solicitud en WoodgroveBank.com. El usuario ahora quiere transferir $1000 a otra cuenta, por lo que rellena un formulario en el sitio de banca y el explorador realiza esta solicitud al servidor:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

Dado que esta operación tiene un efecto secundario (inicia una transacción monetaria), el sitio de banca ha elegido requerir una solicitud HTTP POST para iniciar esta operación. El servidor lee el token de autenticación de la solicitud, busca el número de cuenta del usuario actual, comprueba que existen fondos suficientes y, a continuación, inicia la transacción en la cuenta de destino.

Su banca en línea se completa, el usuario navega fuera del sitio de banca y visita otras ubicaciones en la Web. Uno de esos sitios: fabrikam.com: incluye el marcado siguiente en una página insertada dentro de un &lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

Esto hace que el explorador realice esta solicitud:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

El atacante está aprovechando el hecho de que el usuario todavía puede tener un token de autenticación válido para el sitio web de destino y usa un pequeño fragmento de código de JavaScript para hacer que el explorador realice automáticamente una solicitud HTTP POST al sitio de destino. Si el token de autenticación sigue siendo válido, el sitio de banca iniciará una transferencia de $250 a la cuenta del que elija el atacante.

### <a name="ineffective-mitigations"></a>Mitigaciones ineficaces

Es interesante tener en cuenta que en el escenario anterior, el hecho de que WoodgroveBank.com se accediera a través de SSL y tenía una cookie de autenticación solo de SSL era insuficiente para frustrar el ataque. El atacante puede especificar el esquema de [URI](http://en.wikipedia.org/wiki/URI_scheme) (https) en su &lt;formulario&gt; elemento y el explorador seguirá enviando cookies no caducadas al sitio de destino siempre y cuando dichas cookies sean coherentes con el esquema de URI del destino previsto.

Se puede argumentar que el usuario simplemente no debe visitar sitios que no son de confianza, ya que solo los sitios de confianza pueden estar seguros en línea. Esto es cierto, pero desgraciadamente este Consejo no siempre es práctico. Quizás el usuario "confíe" en el sitio de noticias local ConsolidatedMessenger. ConsolidatedMessenger.com y va a visitar ese sitio en su lugar, pero ese sitio tiene una vulnerabilidad de XSS que permite a un atacante insertar el mismo fragmento de código que se estaba ejecutando en fabrikam.com.

Puede comprobar que las solicitudes entrantes tienen un [encabezado de referencia](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) que hace referencia a su dominio. Esto detendrá las solicitudes que se envían involuntariamente desde un dominio de terceros. Sin embargo, algunas personas deshabilitan el encabezado de referencia del explorador por motivos de privacidad y, a veces, los atacantes pueden suplantar ese encabezado si el sujeto tiene instalado cierto software no seguro. La comprobación del [encabezado de referencia](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14) no se considera un enfoque seguro para evitar ataques de XSRF.

## <a name="web-stack-runtime-xsrf-mitigations"></a>Mitigaciones de XSRF en tiempo de ejecución de pila Web

El tiempo de ejecución de la pila Web de ASP.NET utiliza una variante del [patrón de token del sincronizador](https://cheatsheetseries.owasp.org/cheatsheets/Cross-Site_Request_Forgery_Prevention_Cheat_Sheet.html#synchronizer-token-pattern) para defenderse frente a ataques de XSRF. La forma general del patrón de token del sincronizador es que se envían dos tokens antixsrf al servidor con cada HTTP POST (además del token de autenticación): un token como una cookie y el otro como valor de formulario. Los valores de token generados por el tiempo de ejecución de ASP.NET no son deterministas o predecibles por un atacante. Cuando se envían los tokens, el servidor permitirá que la solicitud continúe solo si ambos tokens pasan una comprobación de comparación.

El token de la *sesión* de comprobación de solicitudes de XSRF se almacena como una cookie HTTP y actualmente contiene la siguiente información en su carga:

- Un token de seguridad, que consta de un identificador aleatorio de 128 bits.   
 En la imagen siguiente se muestra el token de sesión de comprobación de solicitud de XSRF mostrado con las herramientas de desarrollo F12 de Internet Explorer: (tenga en cuenta que esta es la implementación actual y está sujeta, incluso probablemente, para cambiar).

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

El *token de campo* se almacena como un `<input type="hidden" />` y contiene la siguiente información en su carga:

- El nombre de usuario del usuario que ha iniciado sesión (si está autenticado).
- Los datos adicionales proporcionados por un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx).

Las cargas de los tokens anti-XSRF se cifran y firman, por lo que no se puede ver el nombre de usuario al usar las herramientas de para examinar los tokens. Cuando la aplicación web está destinada a ASP.NET 4,0, los servicios criptográficos se proporcionan mediante la rutina [MachineKey. encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx) . Cuando la aplicación web está destinada a ASP.NET 4,5 o posterior, los servicios criptográficos se proporcionan mediante la rutina [MachineKey. Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110)) , que ofrece un mejor rendimiento, extensibilidad y seguridad. Consulte las siguientes entradas de blog para obtener más detalles:

- [Mejoras criptográficas en ASP.NET 4,5, PT. 1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [Mejoras criptográficas en ASP.NET 4,5, PT. 2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [Mejoras criptográficas en ASP.NET 4,5, PT. 3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>Generación de tokens

Para generar los tokens anti-XSRF, llame al método [@Html.AntiForgeryToken](https://msdn.microsoft.com/library/dd470175.aspx) desde una vista de MVC o @AntiForgery.GetHtml() desde una página de Razor. A continuación, el tiempo de ejecución llevará a cabo los siguientes pasos:

1. Si la solicitud HTTP actual ya contiene un token de sesión anti-XSRF (la cookie anti-XSRF \_\_RequestVerificationToken), se extrae el token de seguridad de él. Si la solicitud HTTP no contiene un token de sesión anti-XSRF o si se produce un error en la extracción del token de seguridad, se generará un nuevo token anti-XSRF aleatorio.
2. Un token de campo anti-XSRF se genera mediante el token de seguridad del paso (1) anterior y la identidad del usuario que ha iniciado la sesión actual. (Para obtener más información sobre cómo determinar la identidad del usuario, consulte la sección **[escenarios con compatibilidad especial](#_Scenarios_with_special)** más adelante). Además, si se configura un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx) , el tiempo de ejecución llamará a su método [GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx) e incluirá la cadena devuelta en el token de campo. (Consulte la sección **[configuración y extensibilidad](#_Configuration_and_extensibility)** para obtener más información).
3. Si se generó un nuevo token anti-XSRF en el paso (1), se creará un nuevo token de sesión que lo incluirá y se agregará a la colección de cookies HTTP salientes. El token de campo del paso (2) se ajustará en un elemento `<input type="hidden" />`, y este marcado HTML será el valor devuelto de `Html.AntiForgeryToken()` o `AntiForgery.GetHtml()`.

## <a name="validating-the-tokens"></a>Validar los tokens

Para validar los tokens anti-XSRF entrantes, el desarrollador incluye un atributo [ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx) en su controlador o acción de MVC, o bien llama a `@AntiForgery.Validate()` desde su página de Razor. El tiempo de ejecución llevará a cabo los pasos siguientes:

1. El token de la sesión entrante y el token de campo se leen y el token anti-XSRF extraído de cada. Los tokens anti-XSRF deben ser idénticos según el paso (2) en la rutina de generación.
2. Si el usuario actual está autenticado, su nombre de usuario se compara con el nombre de usuario almacenado en el token del campo. Los nombres de usuario deben coincidir.
3. Si se configura un [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) , el tiempo de ejecución llama a su método *ValidateAdditionalData* . El método debe devolver el valor booleano *true*.

Si la validación se realiza correctamente, la solicitud puede continuar. Si se produce un error en la validación, el marco de trabajo producirá una *HttpAntiForgeryException*.

## <a name="failure-conditions"></a>Condiciones de error

A partir de ASP.NET Web Stack Runtime V2, cualquier *HttpAntiForgeryException* que se inicie durante la validación contendrá información detallada sobre lo que salió mal. Las condiciones de error definidas actualmente son:

- El token de la sesión o el token del formulario no está presente en la solicitud.
- El token de sesión o el token de formulario son ilegibles. La causa más probable es que una granja ejecute versiones no coincidentes del tiempo de ejecución de la pila de Web ASP.NET o una granja en la que el elemento &lt;machineKey&gt; de Web. config difiere entre equipos. Puede usar una herramienta como Fiddler para forzar esta excepción mediante la manipulación de tokens antixsrf.
- El token de sesión y el token de campo se intercambiaron.
- El token de sesión y el token de campo contienen tokens de seguridad no coincidentes.
- El nombre de usuario incrustado en el token de campo no coincide con el nombre de usuario del usuario que ha iniciado la sesión actual.
- El método *[IAntiForgeryAdditionalDataProvider. ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* devolvió *false*.

Los servicios antixsrf también pueden realizar comprobaciones adicionales durante la generación o la validación de tokens, y los errores que se produzcan durante estas comprobaciones pueden dar lugar a que se inicien excepciones. Consulte las secciones sobre la autenticación y la **[configuración y la extensibilidad](#_Configuration_and_extensibility)** [basadas en WIF/ACS/notificaciones](#_WIF_ACS) para obtener más información.

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>Escenarios con compatibilidad especial

### <a name="anonymous-authentication"></a>Autenticación anónima

El sistema anti-XSRF contiene compatibilidad especial con usuarios anónimos, donde "Anonymous" se define como un usuario en el que la propiedad *IIdentity. IsAuthenticated* devuelve *false*. Los escenarios incluyen la protección de XSRF en la página de inicio de sesión (antes de que se autentique el usuario) y los esquemas de autenticación personalizados en los que la aplicación utiliza un mecanismo distinto de *IIdentity* para identificar a los usuarios.

Para admitir estos escenarios, recuerde que los tokens de sesión y de campo están Unidos por un token de seguridad, que es un identificador opaco generado aleatoriamente de 128 bits. Este token de seguridad se usa para realizar el seguimiento de la sesión de un usuario individual a medida que navega por el sitio, por lo que sirve de forma eficaz el propósito de un identificador anónimo. Se utiliza una cadena vacía en lugar del nombre de usuario para las rutinas de generación y validación descritas anteriormente.

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF/ACS/autenticación basada en notificaciones

Normalmente, las clases *IIdentity* integradas en el .NET Framework tienen la propiedad de que *IIdentity.Name* es suficiente para identificar de forma única a un usuario determinado dentro de una aplicación determinada. Por ejemplo, *FormsIdentity.Name* devuelve el nombre de usuario almacenado en la base de datos de pertenencia (que es único para todas las aplicaciones en función de esa base de datos), *WindowsIdentity.Name* devuelve la identidad calificada del dominio del usuario, etc. Estos sistemas proporcionan no solo autenticación; también *identifican* a los usuarios de una aplicación.

La autenticación basada en notificaciones, por otro lado, no requiere necesariamente la identificación de un usuario determinado. En su lugar, los tipos *ClaimsPrincipal* y *ClaimsIdentity* están asociados a un conjunto de instancias de *notificación* , donde las notificaciones individuales pueden ser "más de 18 años de edad" o "es un administrador". Como el usuario no se ha identificado necesariamente, el tiempo de ejecución no puede usar la propiedad *ClaimsIdentity.Name* como identificador único para este usuario concreto. El equipo ha detectado ejemplos del mundo real en los que *ClaimsIdentity.Name* devuelve *null*, devuelve un nombre descriptivo (Mostrar) o, de lo contrario, devuelve una cadena que no es adecuada para su uso como identificador único para el usuario.

Muchas de las implementaciones que usan la autenticación basada en notificaciones usan [Azure Access Control Service](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx) (ACS) en concreto. ACS permite al desarrollador configurar proveedores de *identidades* individuales (por ejemplo, ADFS, el proveedor de cuentas Microsoft, los proveedores de OpenID como Yahoo!, etc.) y los proveedores de identidades devuelven los *identificadores de nombre*. Estos identificadores de nombre pueden contener información de identificación personal (PII) como una dirección de correo electrónico, o bien se anónimos como un identificador personal privado (PPID). Sin tener en consideración, la tupla (proveedor de identidades, identificador de nombre) actúa suficientemente como un token de seguimiento adecuado para un usuario determinado mientras está explorando el sitio, por lo que el tiempo de ejecución de la pila Web ASP.NET puede usar la tupla en lugar del nombre de usuario al generar y validando los tokens de campo anti-XSRF. Los URI específicos para el proveedor de identidades y el identificador de nombre son:

- `https://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(consulte esta [Página de documento de ACS](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx) para obtener más información).

Al generar o validar un token, el tiempo de ejecución de la pila de Web ASP.NET en tiempo de ejecución intenta enlazar a los tipos:

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` (para el SDK de WIF).
- `System.Security.Claims.ClaimsIdentity` (para .NET 4,5).

Si estos tipos existen, y si el *IIIIdentity* del usuario actual implementa o subclase de uno de estos tipos, el servicio anti-XSRF usará la tupla (proveedor de identidades, identificador de nombre) en lugar del nombre de usuario al generar y validar los tokens. Si no hay ninguna tupla de este tipo, se producirá un error en la solicitud y se describirá al desarrollador cómo configurar el sistema anti-XSRF para comprender el mecanismo de autenticación específico basado en notificaciones en uso. Vea la sección **[configuración y extensibilidad](#_Configuration_and_extensibility)** para obtener más información.

### <a name="oauth--openid-authentication"></a>Autenticación de OAuth/OpenID

Por último, la instalación de anti-XSRF tiene compatibilidad especial con las aplicaciones que usan la autenticación de OAuth o OpenID. Esta compatibilidad se basa en la heurística: Si el *IIdentity.Name* actual comienza con http://o https://, las comparaciones de nombre de usuario se realizarán mediante un comparador ordinal en lugar del comparador de OrdinalIgnoreCase predeterminado.

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>Configuración y extensibilidad

En ocasiones, es posible que los desarrolladores quieran un mayor control sobre los comportamientos de la generación y validación de XSRF. Por ejemplo, es posible que el comportamiento predeterminado de las aplicaciones web y MVC para agregar cookies HTTP a la respuesta no sea deseable y que el desarrollador desee conservar los tokens en otra parte. Existen dos API que le servirán de ayuda:

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

El método *GetTokens* toma como entrada un token de sesión de comprobación de solicitud de XSRF existente (que puede ser null) y genera como resultado un nuevo token de sesión de comprobación de solicitud de XSRF y un token de campo. Los tokens son simplemente cadenas opacas sin decoración. el valor de *formToken* para la instancia no se encapsulará en una etiqueta de entrada &lt;&gt;. El valor de *newCookieToken* puede ser null; Si esto ocurre, el valor *oldCookieToken* sigue siendo válido y no es necesario establecer ninguna cookie de respuesta nueva. El autor de la llamada de *GetTokens* es responsable de conservar las cookies de respuesta necesarias o de generar cualquier marcado necesario; el método *GetTokens* en sí no modificará la respuesta como efecto secundario. El método *Validate* toma la sesión entrante y los tokens de campo, y ejecuta la lógica de validación mencionada anteriormente.

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

El desarrollador puede configurar el sistema anti-XSRF desde el inicio de la aplicación\_. La configuración es mediante programación. A continuación se describen las propiedades del tipo *AntiForgeryConfig* estático. La mayoría de los usuarios que usan notificaciones querrán establecer la propiedad UniqueClaimTypeIdentifier.

| **Property** | **Descripción** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) que proporciona datos adicionales durante la generación de tokens y consume datos adicionales durante la validación de tokens. El valor predeterminado es *null*. Para obtener más información, vea la sección [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) . |
| **CookieName** | Una cadena que proporciona el nombre de la cookie HTTP que se utiliza para almacenar el token de sesión anti-XSRF. Si no se establece este valor, se generará automáticamente un nombre en función de la ruta de acceso virtual implementada de la aplicación. El valor predeterminado es *null*. |
| **RequireSsl** | Un valor booleano que indica si se requiere que los tokens antixsrf se envíen a través de un canal seguro para SSL. Si este valor es *true*, todas las cookies generadas automáticamente tendrán establecida la marca "Secure" y las API antixsrf se iniciarán si se llama desde dentro de una solicitud que no se envía a través de SSL. El valor predeterminado es *false*. |
| **SuppressIdentityHeuristicChecks** | Un valor booleano que indica si el sistema anti-XSRF debe desactivar su compatibilidad con identidades basadas en notificaciones. Si este valor es *true*, el sistema asumirá que *IIdentity.Name* es adecuado para su uso como un identificador único por usuario y no intentará *IClaimsIdentity* o *ClClaimsIdentity* especial como se describe en [WIF/ACS/ sección de autenticación basada en notificaciones](#_WIF_ACS) . El valor predeterminado es `false`. |
| **UniqueClaimTypeIdentifier** | Una cadena que indica qué tipo de notificaciones es adecuado para su uso como un identificador único por usuario. Si se establece este valor y el *IIdentity* actual está basado en notificaciones, el sistema intentará extraer una notificación del tipo especificado por *UniqueClaimTypeIdentifier*, y se usará el valor correspondiente en lugar del nombre de usuario del usuario cuando generar el token de campo. Si no se encuentra el tipo de demanda, el sistema producirá un error en la solicitud. El valor predeterminado es *null*, lo que indica que el sistema debe usar la tupla (proveedor de identidades, identificador de nombre) tal y como se ha descrito anteriormente en lugar del nombre de usuario del usuario. |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

El tipo *[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* permite a los desarrolladores ampliar el comportamiento del sistema anti-XSRF mediante el recorrido de ida y vuelta por los datos adicionales de cada token. Se llama al método *GetAdditionalData* cada vez que se genera un token de campo y el valor devuelto se incrusta en el token generado. Un implementador podría devolver una marca de tiempo, un valor de seguridad (nonce) o cualquier otro valor que desee de este método.

Del mismo modo, se llama al método *ValidateAdditionalData* cada vez que se valida un token de campo y la cadena de "datos adicionales" que se incrustó en el token se pasa al método. La rutina de validación podría implementar un tiempo de espera (comprobando la hora actual con la hora que se almacenó cuando se creó el token), una rutina de comprobación de nonce o cualquier otra lógica deseada.

## <a name="design-decisions-and-security-considerations"></a>Decisiones de diseño y consideraciones de seguridad

Técnicamente, el token de seguridad que vincula los tokens de sesión y de campo solo es necesario cuando se intenta proteger usuarios anónimos o no autenticados contra ataques de XSRF. Cuando se autentica el usuario, el propio token de autenticación (supuestamente enviado en forma de cookie) podría usarse como una mitad de un par de tokens de sincronizador. Sin embargo, hay escenarios válidos para proteger las páginas de inicio de sesión de los usuarios no autenticados, y la lógica anti-XSRF se hizo más sencilla generando y validando siempre el token de seguridad, incluso para los usuarios autenticados. También proporciona protección adicional en caso de que un atacante haya puesto en peligro un token de campo, ya que el establecimiento o la adivinación del token de sesión sería otro obstáculo para que el atacante lo solucione.

Los desarrolladores deben tener cuidado cuando se hospeden varias aplicaciones en un único dominio. Por ejemplo, aunque *example1.CloudApp.net* y *example2.CloudApp.net* son hosts diferentes, hay una relación de confianza implícita entre todos los hosts del dominio *\*. CloudApp.net* . Esta relación de confianza implícita [permite que los hosts potencialmente que no son de confianza afecten a las cookies de los demás](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) (las directivas del mismo origen que rigen las solicitudes Ajax no se aplican necesariamente a las cookies http). El tiempo de ejecución de la pila Web de ASP.NET proporciona una mitigación en que el nombre de usuario se inserta en el token de campo, por lo que incluso si un subdominio malintencionado puede sobrescribir un token de sesión, no podrá generar un token de campo válido para el usuario. Sin embargo, cuando se hospeda en este tipo de entorno, las rutinas antixsrf integradas todavía no pueden defenderse contra el secuestro de sesión o el inicio de sesión de XSRF.

Las rutinas anti-XSRF no se defienden actualmente contra [el secuestro](https://www.owasp.org/index.php/Clickjacking). Las aplicaciones que quieran defenderse frente a el secuestro pueden hacerlo fácilmente mediante el envío de un encabezado X-Frame-Options: SAMEORIGIN con cada respuesta. Este encabezado es compatible con todos los exploradores recientes. Para obtener más información, consulte el [blog de IE](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx), el blog de [SDL](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)y [OWASP](https://www.owasp.org/index.php/Clickjacking). En algunas versiones futuras, el tiempo de ejecución de la pila Web de ASP.NET puede hacer que las aplicaciones de la aplicación Web de MVC y de las páginas web configuran automáticamente este encabezado para que las aplicaciones se protejan automáticamente contra este ataque.

Los desarrolladores web deben continuar para asegurarse de que su sitio no sea vulnerable a los ataques XSS. Los ataques XSS son muy eficaces y un ataque correcto también interrumpiría las defensas en tiempo de ejecución de la pila Web ASP.NET frente a ataques XSRF.

## <a name="acknowledgment"></a>Confirmación

[@LeviBroderick](https://twitter.com/LeviBroderick), que escribió gran parte del código de seguridad de ASP.net en la mayor parte de esta información.
