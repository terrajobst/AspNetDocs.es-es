---
uid: web-forms/overview/older-versions-security/membership/user-based-authorization-vb
title: Autorización basada en usuario (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo limitar el acceso a las páginas y restringir la funcionalidad de nivel de página a través de diversas técnicas.
ms.author: riande
ms.date: 01/18/2008
ms.assetid: bc937e9d-5c14-4fc4-aec7-440da924dd18
msc.legacyurl: /web-forms/overview/older-versions-security/membership/user-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: dfac0c6fa955e59c6ea996533f2447e89ec8d468
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74587863"
---
# <a name="user-based-authorization-vb"></a>Autorización basada en usuario (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_07_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial07_UserAuth_vb.pdf)

> En este tutorial, veremos cómo limitar el acceso a las páginas y restringir la funcionalidad de nivel de página a través de diversas técnicas.

## <a name="introduction"></a>Introducción

La mayoría de las aplicaciones web que ofrecen cuentas de usuario lo hacen en parte para impedir que determinados visitantes tengan acceso a determinadas páginas del sitio. En la mayoría de los sitios de Messageboard en línea, por ejemplo, todos los usuarios (anónimos y autenticados) pueden ver las entradas de Messageboard, pero solo los usuarios autenticados pueden visitar la página web para crear una nueva publicación. Y puede haber páginas administrativas que solo sean accesibles para un usuario determinado (o un conjunto determinado de usuarios). Además, la funcionalidad en el nivel de página puede variar en función del usuario. Al ver una lista de entradas, se muestra a los usuarios autenticados una interfaz para clasificar cada entrada, mientras que esta interfaz no está disponible para los visitantes anónimos.

ASP.NET facilita la definición de reglas de autorización basadas en el usuario. Con tan solo un poco de marcado en `Web.config`, las páginas web específicas o los directorios completos se pueden bloquear para que solo se pueda tener acceso a ellas en un subconjunto especificado de usuarios. La funcionalidad en el nivel de página puede activarse o desactivarse en función del usuario que ha iniciado sesión actualmente a través de medios mediante programación y declarativos.

En este tutorial, veremos cómo limitar el acceso a las páginas y restringir la funcionalidad de nivel de página a través de diversas técnicas. Comencemos.

## <a name="a-look-at-the-url-authorization-workflow"></a>Un vistazo al flujo de trabajo de autorización de URL

Como se describe en el tutorial [*de información general sobre la autenticación de formularios*](../introduction/an-overview-of-forms-authentication-vb.md) , cuando el tiempo de ejecución de ASP.net procesa una solicitud de un recurso ASP.net, la solicitud genera una serie de eventos durante su ciclo de vida. Los *módulos http* son clases administradas cuyo código se ejecuta como respuesta a un evento determinado en el ciclo de vida de la solicitud. ASP.NET se suministra con varios módulos HTTP que realizan tareas esenciales en segundo plano.

Uno de estos módulos HTTP es [`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx). Como se explicó en los tutoriales anteriores, la función principal de la `FormsAuthenticationModule` es determinar la identidad de la solicitud actual. Esto se logra mediante la inspección del vale de autenticación de formularios, que se encuentra en una cookie o se inserta en la dirección URL. Esta identificación tiene lugar durante el [evento`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Otro módulo HTTP importante es el [`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx), que se genera en respuesta al [evento`AuthorizeRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authorizerequest.aspx) (que se produce después del evento `AuthenticateRequest`). El `UrlAuthorizationModule` examina el marcado de configuración en `Web.config` para determinar si la identidad actual tiene autoridad para visitar la página especificada. Este proceso se conoce como *autorización de dirección URL*.

Examinaremos la sintaxis de las reglas de autorización de URL en el paso 1, pero primero veamos lo que hace el `UrlAuthorizationModule` en función de si la solicitud está autorizada o no. Si el `UrlAuthorizationModule` determina que la solicitud está autorizada, no hace nada y la solicitud continúa a lo largo de su ciclo de vida. Sin embargo, si la solicitud *no* está autorizada, la `UrlAuthorizationModule` anula el ciclo de vida e indica al objeto `Response` que devuelva un estado [no autorizado http 401](http://www.checkupdown.com/status/E401.html) . Al utilizar la autenticación de formularios, este estado HTTP 401 nunca se devuelve al cliente porque si el `FormsAuthenticationModule` detecta un Estado HTTP 401, lo modifica en una [redirección http 302](http://www.checkupdown.com/status/E302.html) a la página de inicio de sesión.

La figura 1 muestra el flujo de trabajo de la canalización ASP.NET, el `FormsAuthenticationModule`y el `UrlAuthorizationModule` cuando llega una solicitud no autorizada. En concreto, la ilustración 1 muestra una solicitud por parte de un visitante anónimo de `ProtectedPage.aspx`, que es una página que deniega el acceso a los usuarios anónimos. Puesto que el visitante es anónimo, el `UrlAuthorizationModule` anula la solicitud y devuelve un estado no autorizado HTTP 401. A continuación, el `FormsAuthenticationModule` convierte el estado 401 en un redireccionamiento de 302 a la página de inicio de sesión. Una vez que el usuario se ha autenticado a través de la página de inicio de sesión, se le redirige a `ProtectedPage.aspx`. Esta vez, el `FormsAuthenticationModule` identifica al usuario en función de su vale de autenticación. Ahora que el visitante está autenticado, el `UrlAuthorizationModule` permite el acceso a la página.

[![el flujo de trabajo de autenticación de formularios y autorización de URL](user-based-authorization-vb/_static/image2.png)](user-based-authorization-vb/_static/image1.png)

**Figura 1**: flujo de trabajo de autenticación de formularios y autorización de URL ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image3.png))

La figura 1 muestra la interacción que se produce cuando un visitante anónimo intenta tener acceso a un recurso que no está disponible para los usuarios anónimos. En tal caso, el visitante anónimo se redirige a la página de inicio de sesión con la página que ha intentado visitar especificado en la cadena de consulta. Una vez que el usuario haya iniciado sesión correctamente, se le redirigirá automáticamente al recurso que estaba intentando ver inicialmente.

Cuando un usuario anónimo realiza la solicitud no autorizada, este flujo de trabajo es sencillo y es fácil para el visitante comprender lo que ha sucedido y por qué. Sin embargo, tenga en cuenta que el `FormsAuthenticationModule` *redirigirá a los usuarios* no autorizados a la página de inicio de sesión, aunque la solicitud la realice un usuario autenticado. Esto puede dar lugar a una experiencia de usuario confusa si un usuario autenticado intenta visitar una página para la que carece de autoridad.

Imagine que el sitio web tenía sus reglas de autorización de URL configuradas de forma que la `OnlyTito.aspx` página ASP.NET solo era accesible a Tito. Ahora, Imagine que Sam visita el sitio, inicia sesión y, a continuación, intenta visitar `OnlyTito.aspx`. El `UrlAuthorizationModule` detendrá el ciclo de vida de la solicitud y devolverá un estado no autorizado HTTP 401, que el `FormsAuthenticationModule` detectará y redirigirá a Sam a la página de inicio de sesión. A pesar de que Sam ya ha iniciado sesión, es posible que se pregunte por qué se ha enviado a la página de inicio de sesión. Es posible que tenga en que las credenciales de inicio de sesión se han perdido de algún modo o que ha especificado credenciales no válidas. Si Sam vuelve a escribir sus credenciales desde la página de inicio de sesión, se iniciará sesión (de nuevo) y se le redirigirá a `OnlyTito.aspx`. El `UrlAuthorizationModule` detectará que Sam no puede visitar esta página y se le devolverá a la página de inicio de sesión.

En la ilustración 2 se muestra este flujo de trabajo confuso.

[![el flujo de trabajo predeterminado puede conducir a un ciclo confuso](user-based-authorization-vb/_static/image5.png)](user-based-authorization-vb/_static/image4.png)

**Figura 2**: el flujo de trabajo predeterminado puede conducir a un ciclo confuso ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image6.png))

El flujo de trabajo que se muestra en la figura 2 puede befuddle rápidamente incluso el visitante más experimentado. Veremos maneras de evitar este ciclo confuso en el paso 2.

> [!NOTE]
> ASP.NET usa dos mecanismos para determinar si el usuario actual puede tener acceso a una página web determinada: autorización de URL y autorización de archivo. La autorización de archivos se implementa mediante el [`FileAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx), que determina la autoridad mediante la consulta de las ACL de los archivos solicitados. La autorización de archivos se usa normalmente con la autenticación de Windows, ya que las ACL son permisos que se aplican a las cuentas de Windows. Al utilizar la autenticación de formularios, todas las solicitudes de sistema operativo y de nivel de sistema de archivos se ejecutan con la misma cuenta de Windows, independientemente de que el usuario visite el sitio. Dado que esta serie de tutoriales se centra en la autenticación de formularios, no hablaremos de la autorización de archivos.

### <a name="the-scope-of-url-authorization"></a>El ámbito de autorización de URL

El `UrlAuthorizationModule` es código administrado que forma parte del tiempo de ejecución de ASP.NET. Antes de la versión 7 del servidor Web de [Internet Information Services (IIS)](https://www.iis.net/) de Microsoft, había una barrera distinta entre la CANALización http de IIS y la canalización del tiempo de ejecución de ASP.net. En Resumen, en IIS 6 y versiones anteriores, ASP. La `UrlAuthorizationModule` de red solo se ejecuta cuando se delega una solicitud desde IIS al tiempo de ejecución de ASP.NET. De forma predeterminada, IIS procesa contenido estático en sí mismo, como páginas HTML y archivos CSS, JavaScript y de imagen, y solo entrega solicitudes al tiempo de ejecución de ASP.NET cuando se solicita una página con una extensión de `.aspx`, `.asmx`o `.ashx`.

Sin embargo, IIS 7 permite canalizaciones integradas de IIS y ASP.NET. Con algunas opciones de configuración, puede configurar IIS 7 para que invoque el `UrlAuthorizationModule` para *todas* las solicitudes, lo que significa que se pueden definir reglas de autorización de URL para los archivos de cualquier tipo. Además, IIS 7 incluye su propio motor de autorización de URL. Para obtener más información acerca de la integración de ASP.NET y la funcionalidad de autorización de URL nativa de IIS 7, consulte [Understanding IIS7 URL Authorization](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization). Para obtener una visión más detallada de la integración de ASP.NET e IIS 7, seleccione una copia del libro de Shahram Khosravi, *Professional IIS 7 y ASP.net Integrated Programming* (ISBN: 978-0470152539).

En pocas palabras, en las versiones anteriores a IIS 7, las reglas de autorización de direcciones URL solo se aplican a los recursos administrados por el tiempo de ejecución de ASP.NET. Pero con IIS 7 es posible usar la característica de autorización de URL nativa de IIS o integrar ASP. La `UrlAuthorizationModule` de red en la canalización HTTP de IIS, con lo que se amplía esta funcionalidad a todas las solicitudes.

> [!NOTE]
> Hay algunas diferencias sutiles pero importantes en el modo en que ASP. La característica de autorización de URL de la `UrlAuthorizationModule` de red y de IIS 7 procesa las reglas de autorización. En este tutorial no se examina la funcionalidad de autorización de URL de IIS 7 ni las diferencias en la forma en que analiza las reglas de autorización en comparación con la `UrlAuthorizationModule`. Para obtener más información sobre estos temas, consulte la documentación de IIS 7 en MSDN o en [www.IIS.net](https://www.iis.net/).

## <a name="step-1-defining-url-authorization-rules-inwebconfig"></a>Paso 1: definición de las reglas de autorización de URL en`Web.config`

El `UrlAuthorizationModule` determina si se debe conceder o denegar el acceso a un recurso solicitado para una identidad determinada en función de las reglas de autorización de URL definidas en la configuración de la aplicación. Las reglas de autorización se escriben en el [elemento`<authorization>`](https://msdn.microsoft.com/library/8d82143t.aspx) en forma de `<allow>` y `<deny>` elementos secundarios. Cada `<allow>` y `<deny>` elemento secundario pueden especificar:

- Un usuario determinado
- Una lista delimitada por comas de usuarios
- Todos los usuarios anónimos, indicados por un signo de interrogación (?)
- Todos los usuarios, indicados por un asterisco (\*)

En el marcado siguiente se muestra cómo usar las reglas de autorización de URL para permitir a los usuarios Tito y Scott y denegar todos los demás:

[!code-xml[Main](user-based-authorization-vb/samples/sample1.xml)]

El elemento `<allow>` define qué usuarios están permitidos-Tito y Scott-mientras que el elemento `<deny>` indica que se deniegan *todos* los usuarios.

> [!NOTE]
> Los elementos `<allow>` y `<deny>` también pueden especificar las reglas de autorización para los roles. Examinaremos la autorización basada en roles en un tutorial futuro.

La configuración siguiente concede acceso a cualquier persona que no sea Sam (incluidos los visitantes anónimos):

[!code-xml[Main](user-based-authorization-vb/samples/sample2.xml)]

Para permitir solo usuarios autenticados, use la configuración siguiente, que deniega el acceso a todos los usuarios anónimos:

[!code-xml[Main](user-based-authorization-vb/samples/sample3.xml)]

Las reglas de autorización se definen en el elemento `<system.web>` de `Web.config` y se aplican a todos los recursos ASP.NET de la aplicación Web. A menudo, una aplicación tiene distintas reglas de autorización para diferentes secciones. Por ejemplo, en un sitio de comercio electrónico, todos los visitantes pueden consultar los productos, ver las revisiones del producto, buscar en el catálogo, etc. Sin embargo, solo los usuarios autenticados pueden tener acceso a la desprotección o las páginas para administrar el historial de envíos de un. Además, puede haber partes del sitio a las que solo puedan acceder los usuarios seleccionados, como los administradores del sitio.

ASP.NET facilita la definición de distintas reglas de autorización para diferentes archivos y carpetas en el sitio. Las reglas de autorización especificadas en el archivo de `Web.config` de la carpeta raíz se aplican a todos los recursos de ASP.NET del sitio. Sin embargo, estos valores de autorización predeterminados se pueden invalidar para una carpeta determinada agregando una `Web.config` con una `<authorization>` sección.

Vamos a actualizar nuestro sitio web para que solo los usuarios autenticados puedan visitar las páginas de ASP.NET en la carpeta `Membership`. Para ello, es necesario agregar un archivo de `Web.config` a la carpeta de `Membership` y establecer su configuración de autorización para denegar a los usuarios anónimos. Haga clic con el botón secundario en la carpeta `Membership` del Explorador de soluciones, elija el menú Agregar nuevo elemento en el menú contextual y agregue un nuevo archivo de configuración Web denominado `Web.config`.

[![agregar un archivo Web. config a la carpeta de pertenencia](user-based-authorization-vb/_static/image8.png)](user-based-authorization-vb/_static/image7.png)

**Figura 3**: agregar un archivo de `Web.config` a la carpeta `Membership` ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image9.png))

En este momento, el proyecto debe contener dos archivos de `Web.config`: uno en el directorio raíz y otro en la carpeta `Membership`.

[![la aplicación ahora debe contener dos archivos Web. config](user-based-authorization-vb/_static/image11.png)](user-based-authorization-vb/_static/image10.png)

**Figura 4**: la aplicación ahora debe contener dos archivos de `Web.config` ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image12.png))

Actualice el archivo de configuración en la carpeta `Membership` para que prohíba el acceso a los usuarios anónimos.

[!code-xml[Main](user-based-authorization-vb/samples/sample4.xml)]

Así de simple.

Para probar este cambio, visite la Página principal en un explorador y asegúrese de que ha cerrado la sesión. Dado que el comportamiento predeterminado de una aplicación ASP.NET es permitir a todos los visitantes, y como no realizamos ninguna modificación de autorización en el archivo `Web.config` del directorio raíz, podemos visitar los archivos del directorio raíz como visitante anónimo.

Haga clic en el vínculo crear cuentas de usuario que se encuentra en la columna izquierda. Esto le llevará a la `~/Membership/CreatingUserAccounts.aspx`. Puesto que el archivo de `Web.config` de la carpeta `Membership` define reglas de autorización para prohibir el acceso anónimo, el `UrlAuthorizationModule` anula la solicitud y devuelve un estado no autorizado HTTP 401. El `FormsAuthenticationModule` lo modifica en un estado de redireccionamiento 302 y lo envía a la página de inicio de sesión. Tenga en cuenta que la página a la que intentamos acceder (`CreatingUserAccounts.aspx`) se pasa a la página de inicio de sesión a través del parámetro QueryString `ReturnUrl`.

[![dado que las reglas de autorización de URL prohíben el acceso anónimo, se le redirigirá a la página de inicio de sesión.](user-based-authorization-vb/_static/image14.png)](user-based-authorization-vb/_static/image13.png)

**Figura 5**: dado que las reglas de autorización de URL prohíben el acceso anónimo, se le redirigirá a la página de inicio de sesión ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image15.png))

Tras iniciar sesión correctamente, se le redirigirá a la página `CreatingUserAccounts.aspx`. Esta vez, el `UrlAuthorizationModule` permite el acceso a la página porque ya no somos anónimos.

### <a name="applying-url-authorization-rules-to-a-specific-location"></a>Aplicar reglas de autorización de URL a una ubicación específica

Los valores de autorización definidos en la sección `<system.web>` de `Web.config` se aplican a todos los recursos de ASP.NET de ese directorio y sus subdirectorios (hasta que otro archivo de `Web.config` invalida de otro modo). Sin embargo, en algunos casos, es posible que quiera que todos los recursos de ASP.NET de un directorio determinado tengan una configuración de autorización determinada, excepto para una o dos páginas específicas. Esto puede lograrse agregando un elemento de `<location>` en `Web.config`, apuntando al archivo cuyas reglas de autorización difieren y definiendo las reglas de autorización únicas que contiene.

Para ilustrar el uso del elemento `<location>` para invalidar la configuración de un recurso específico, vamos a personalizar la configuración de autorización para que solo Tito pueda visitar `CreatingUserAccounts.aspx`. Para ello, agregue un elemento `<location>` al archivo `Web.config` de la carpeta de `Membership` y actualice su marcado para que tenga el aspecto siguiente:

[!code-xml[Main](user-based-authorization-vb/samples/sample5.xml)]

El elemento `<authorization>` de `<system.web>` define las reglas de autorización de direcciones URL predeterminadas para los recursos de ASP.NET en la carpeta `Membership` y sus subcarpetas. El elemento `<location>` nos permite invalidar estas reglas para un recurso determinado. En el marcado anterior, el elemento `<location>` hace referencia a la página `CreatingUserAccounts.aspx` y especifica sus reglas de autorización, como permitir Tito, pero denegar a todos los demás.

Para probar este cambio de autorización, empiece por visitar el sitio web como usuario anónimo. Si intenta visitar cualquier página de la carpeta `Membership`, como `UserBasedAuthorization.aspx`, el `UrlAuthorizationModule` denegará la solicitud y se le redirigirá a la página de inicio de sesión. Después de iniciar sesión como, por ejemplo, Scott, puede visitar cualquier página de la carpeta `Membership`, *excepto* `CreatingUserAccounts.aspx`. Si intenta visitar `CreatingUserAccounts.aspx` sesión iniciada como cualquiera pero Tito, se producirá un intento de acceso no autorizado y se le redirigirá a la página de inicio de sesión.

> [!NOTE]
> El elemento `<location>` debe aparecer fuera del elemento `<system.web>` de la configuración. Debe usar un elemento de `<location>` independiente para cada recurso cuya configuración de autorización desea reemplazar.

### <a name="a-look-at-how-theurlauthorizationmoduleuses-the-authorization-rules-to-grant-or-deny-access"></a>Un vistazo a cómo el`UrlAuthorizationModule`utiliza las reglas de autorización para conceder o denegar el acceso

El `UrlAuthorizationModule` determina si se va a autorizar una identidad determinada para una dirección URL determinada mediante el análisis de las reglas de autorización de dirección URL de una en una, empezando por la primera y trabajando hasta el final. En cuanto se encuentra una coincidencia, se concede o se deniega el acceso al usuario, en función de si se encontró la coincidencia en un elemento `<allow>` o `<deny>`. <strong>Si no se encuentra ninguna coincidencia, se concede al usuario acceso.</strong> Por consiguiente, si desea restringir el acceso, es imperativo que use un elemento `<deny>` como el último elemento en la configuración de autorización de la dirección URL. <strong>Si omite un</strong> <strong>elemento`<deny>`, se concederá acceso a todos los usuarios.</strong>

Para entender mejor el proceso utilizado por el `UrlAuthorizationModule` para determinar la autoridad, tenga en cuenta las reglas de autorización de URL de ejemplo que hemos visto anteriormente en este paso. La primera regla es un elemento `<allow>` que permite el acceso a Tito y a Scott. La segunda regla es un elemento `<deny>` que deniega el acceso a todos los usuarios. Si un usuario anónimo visita, el `UrlAuthorizationModule` empieza por preguntar, ¿es anónimo Scott o Tito? Obviamente, la respuesta es no, por lo que pasa a la segunda regla. ¿Es anónimo en el conjunto de todos? Como la respuesta aquí es sí, la regla de `<deny>` se aplica y el visitante se redirige a la página de inicio de sesión. Del mismo modo, si Jisun está visitando, el `UrlAuthorizationModule` empieza por preguntar, ¿Jisun Scott o Tito? Como no es así, el `UrlAuthorizationModule` continúa con la segunda pregunta, ¿está Jisun en el conjunto de todos? También se le deniega el acceso a ella. Por último, si Tito visita, la primera pregunta planteada por el `UrlAuthorizationModule` es una respuesta afirmativa, de modo que se concede el acceso a Tito.

Dado que el `UrlAuthorizationModule` procesa las reglas de autorización de arriba abajo y se detiene en cualquier coincidencia, es importante tener las reglas más específicas antes que las menos específicas. Es decir, para definir reglas de autorización que prohíban a los usuarios de Jisun y anónimos, pero que permitan a todos los demás usuarios autenticados, comenzaría con la regla más específica (la que afectó a Jisun) y, a continuación, continuaría con las reglas menos específicas, que permitían todas las demás usuarios autenticados, pero denegar a todos los usuarios anónimos. Las siguientes reglas de autorización de dirección URL implementan esta directiva denegando primero Jisun y denegando después cualquier usuario anónimo. Se concederá acceso a cualquier usuario autenticado que no sea JISUN, ya que ninguna de estas instrucciones `<deny>` coincidirá.

[!code-xml[Main](user-based-authorization-vb/samples/sample6.xml)]

## <a name="step-2-fixing-the-workflow-for-unauthorized-authenticated-users"></a>Paso 2: corregir el flujo de trabajo de usuarios autenticados y no autorizados

Como se explicó anteriormente en este tutorial, en la sección un vistazo al flujo de trabajo de autorización de URL, cada vez que se transcurrió una solicitud no autorizada, la `UrlAuthorizationModule` anula la solicitud y devuelve un estado no autorizado HTTP 401. El estado 401 lo modifica el `FormsAuthenticationModule` en un estado de redirección de 302 que envía al usuario a la página de inicio de sesión. Este flujo de trabajo se produce en cualquier solicitud no autorizada, incluso si el usuario está autenticado.

Es probable que la devolución de un usuario autenticado a la página de inicio de sesión se confunda, ya que ya han iniciado sesión en el sistema. Con un poco de trabajo, podemos mejorar este flujo de trabajo redirigiendo a los usuarios autenticados que realizan solicitudes no autorizadas a una página en la que se explica que han intentado tener acceso a una página restringida.

Empiece por crear una nueva página de ASP.NET en la carpeta raíz de la aplicación web denominada `UnauthorizedAccess.aspx`; no olvide asociar esta página con la página maestra de `Site.master`. Después de crear esta página, quite el control de contenido que hace referencia a la `LoginContent` ContentPlaceHolder para que se muestre el contenido predeterminado de la página maestra. Después, agregue un mensaje que explique la situación, es decir, que el usuario intentó tener acceso a un recurso protegido. Después de agregar este tipo de mensaje, el marcado declarativo de la página `UnauthorizedAccess.aspx` debe ser similar al siguiente:

[!code-aspx[Main](user-based-authorization-vb/samples/sample7.aspx)]

Ahora es necesario modificar el flujo de trabajo para que, si un usuario autenticado realiza una solicitud no autorizada, se envíe a la página `UnauthorizedAccess.aspx` en lugar de a la página de inicio de sesión. La lógica que redirige las solicitudes no autorizadas a la página de inicio de sesión está oculta dentro de un método privado de la clase `FormsAuthenticationModule`, por lo que no se puede personalizar este comportamiento. Sin embargo, lo que podemos hacer es agregar nuestra propia lógica a la página de inicio de sesión que redirige al usuario a `UnauthorizedAccess.aspx`, si es necesario.

Cuando el `FormsAuthenticationModule` redirige un visitante no autorizado a la página de inicio de sesión, anexa la dirección URL solicitada y no autorizada a la cadena de entrada con el nombre `ReturnUrl`. Por ejemplo, si un usuario no autorizado intenta visitar `OnlyTito.aspx`, el `FormsAuthenticationModule` los redirigirá a `Login.aspx?ReturnUrl=OnlyTito.aspx`. Por lo tanto, si un usuario autenticado obtiene acceso a la página de inicio de sesión con una cadena de consulta que incluye el parámetro `ReturnUrl`, sabemos que este usuario no autenticado acaba de intentar visitar una página que no está autorizado a ver. En tal caso, queremos redirigirlo a `UnauthorizedAccess.aspx`.

Para ello, agregue el código siguiente al controlador de eventos `Page_Load` de la página de inicio de sesión:

[!code-vb[Main](user-based-authorization-vb/samples/sample8.vb)]

En el código anterior se redirige a los usuarios autenticados y no autorizados a la página `UnauthorizedAccess.aspx`. Para ver esta lógica en acción, visite el sitio como visitante anónimo y haga clic en el vínculo crear cuentas de usuario en la columna izquierda. Esto le llevará a la página `~/Membership/CreatingUserAccounts.aspx`, que en el paso 1 se configuró para permitir solo el acceso a Tito. Dado que los usuarios anónimos están prohibidos, el `FormsAuthenticationModule` redirige US de nuevo a la página de inicio de sesión.

En este momento, somos anónimos, por lo que `Request.IsAuthenticated` devuelve `False` y no se le redirigirá a `UnauthorizedAccess.aspx`. En su lugar, se muestra la página de inicio de sesión. Inicie sesión como un usuario que no sea Tito, como Bruce. Después de escribir las credenciales adecuadas, la página de inicio de sesión redirigirá a `~/Membership/CreatingUserAccounts.aspx`. Sin embargo, dado que esta página solo es accesible para Tito, no se autoriza para verla y se devuelve rápidamente a la página de inicio de sesión. Sin embargo, esta vez `Request.IsAuthenticated` devuelve `True` (y el parámetro `ReturnUrl` QueryString existe), por lo que se le redirigirá a la página de `UnauthorizedAccess.aspx`.

[![autenticado, los usuarios no autorizados se redirigen a UnauthorizedAccess. aspx.](user-based-authorization-vb/_static/image17.png)](user-based-authorization-vb/_static/image16.png)

**Figura 6**: los usuarios autenticados y no autorizados se redirigen a `UnauthorizedAccess.aspx` ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image18.png))

Este flujo de trabajo personalizado presenta una experiencia de usuario más sensata y sencilla al cortocircuitar el ciclo representado en la figura 2.

## <a name="step-3-limiting-functionality-based-on-the-currently-logged-in-user"></a>Paso 3: limitar la funcionalidad basada en el usuario que ha iniciado sesión actualmente

La autorización de URL facilita la especificación de reglas de autorización generales. Como vimos en el paso 1, con la autorización de dirección URL podemos indicar brevemente qué identidades se permiten y cuáles se deniegan para ver una página determinada o todas las páginas de una carpeta. Sin embargo, en algunos escenarios, es posible que desee permitir que todos los usuarios visiten una página, pero limiten la funcionalidad de la página en función del usuario que la visite.

Considere el caso de un sitio web de comercio electrónico que permita a los visitantes autenticados revisar sus productos. Cuando un usuario anónimo visita la página de un producto, verá solo la información del producto y no se le ofrecerá la oportunidad de salir de una revisión. Sin embargo, un usuario autenticado que visita la misma página verá la interfaz de revisión. Si el usuario autenticado todavía no ha revisado este producto, la interfaz les permitiría enviar una revisión; de lo contrario, se mostrarán las revisiones enviadas previamente. Para llevar a cabo este escenario un paso más, la página del producto podría mostrar información adicional y ofrecer características ampliadas para los usuarios que trabajan para la empresa de comercio electrónico. Por ejemplo, la página del producto podría mostrar el inventario en existencias e incluir opciones para editar el precio y la descripción del producto cuando lo visite un empleado.

Estas reglas de autorización detalladas se pueden implementar de forma declarativa o mediante programación (o a través de alguna combinación de los dos). En la sección siguiente, veremos cómo implementar la autorización específica a través del control LoginView. A continuación, exploraremos las técnicas de programación. Sin embargo, antes de que podamos ver cómo aplicar reglas de autorización detalladas, primero debemos crear una página cuya funcionalidad depende del usuario que la visita.

Vamos a crear una página que muestra los archivos de un directorio determinado dentro de un control GridView. Además de mostrar el nombre, el tamaño y otra información de cada archivo, GridView incluirá dos columnas de LinkButtons: una denominada vista y otra titulada eliminar. Si se hace clic en la vista de LinkButton, se mostrará el contenido del archivo seleccionado; Si se hace clic en la eliminación de LinkButton, se eliminará el archivo. Vamos a crear inicialmente esta página de forma que su funcionalidad de vista y eliminación esté disponible para todos los usuarios. En el uso del control LoginView y la limitación mediante programación de las secciones de funcionalidad, veremos cómo habilitar o deshabilitar estas características en función del usuario que visita la página.

> [!NOTE]
> La página ASP.NET que se va a compilar usa un control GridView para mostrar una lista de archivos. Dado que esta serie de tutoriales se centra en la autenticación de formularios, la autorización, las cuentas de usuario y los roles, no deseo gastar demasiado tiempo en hablar sobre el funcionamiento interno del control GridView. Aunque en este tutorial se proporcionan instrucciones paso a paso específicas para configurar esta página, no profundiza en los detalles del motivo por el que se realizaron ciertas opciones o el efecto que tienen determinadas propiedades en la salida representada. Para un examen exhaustivo del control GridView, consulte mi *[trabajo con datos en la serie de tutoriales de ASP.NET 2,0](../../data-access/index.md)* .

Comience abriendo el archivo `UserBasedAuthorization.aspx` en la carpeta `Membership` y agregando un control GridView a la página denominada `FilesGrid`. En la etiqueta inteligente de GridView, haga clic en el vínculo editar columnas para abrir el cuadro de diálogo campos. Desde aquí, desactive la casilla generar automáticamente campos en la esquina inferior izquierda. Después, agregue un botón seleccionar, un botón eliminar y dos BoundFields desde la esquina superior izquierda (los botones seleccionar y eliminar se pueden encontrar en el tipo CommandField). Establezca la propiedad `SelectText` del botón seleccionar en ver y las propiedades `HeaderText` y `DataField` del primer BoundField en Name. Establezca la propiedad `HeaderText` de la segunda BoundField en size en bytes, su propiedad `DataField` en length, su propiedad `DataFormatString` en {0:N0} y su propiedad `HtmlEncode` en false.

Después de configurar las columnas de GridView, haga clic en Aceptar para cerrar el cuadro de diálogo campos. En el ventana Propiedades, establezca la propiedad `DataKeyNames` de GridView en `FullName`. En este momento, el marcado declarativo de GridView debería tener un aspecto similar al siguiente:

[!code-aspx[Main](user-based-authorization-vb/samples/sample9.aspx)]

Con el marcado de GridView creado, estamos preparados para escribir el código que recuperará los archivos en un directorio determinado y enlazarlos a GridView. Agregue el código siguiente al controlador de eventos de `Page_Load` de la página:

[!code-vb[Main](user-based-authorization-vb/samples/sample10.vb)]

En el código anterior se usa la [clase`DirectoryInfo`](https://msdn.microsoft.com/library/system.io.directoryinfo.aspx) para obtener una lista de los archivos de la carpeta raíz de la aplicación. El [método`GetFiles()`](https://msdn.microsoft.com/library/system.io.directoryinfo.getfiles.aspx) devuelve todos los archivos del directorio como una matriz de [objetos`FileInfo`](https://msdn.microsoft.com/library/system.io.fileinfo.aspx), que después se enlaza a GridView. El objeto `FileInfo` tiene una serie de propiedades, como `Name`, `Length`y `IsReadOnly`, entre otras. Como puede ver en su marcado declarativo, GridView muestra solo las propiedades `Name` y `Length`.

> [!NOTE]
> Las clases `DirectoryInfo` y `FileInfo` se encuentran en el [espacio de nombres`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). Por lo tanto, tendrá que anteponer estos nombres de clase con sus nombres de espacio de nombres o tener el espacio de nombres importado en el archivo de clase (a través de `Imports System.IO`).

Dedique un momento a visitar esta página a través de un explorador. Se mostrará la lista de archivos que residen en el directorio raíz de la aplicación. Al hacer clic en cualquiera de las LinkButtons de vista o de eliminación, se producirá un postback, pero no se producirá ninguna acción porque todavía se han creado los controladores de eventos necesarios.

[![GridView muestra los archivos en el directorio raíz de la aplicación Web](user-based-authorization-vb/_static/image20.png)](user-based-authorization-vb/_static/image19.png)

**Figura 7**: el control GridView muestra los archivos en el directorio raíz de la aplicación web ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image21.png))

Necesitamos un medio para mostrar el contenido del archivo seleccionado. Vuelva a Visual Studio y agregue un cuadro de texto denominado `FileContents` sobre GridView. Establezca su propiedad `TextMode` en `MultiLine` y sus propiedades `Columns` y `Rows` en 95% y 10, respectivamente.

[!code-aspx[Main](user-based-authorization-vb/samples/sample11.aspx)]

A continuación, cree un controlador de eventos para el [evento de`SelectedIndexChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindexchanged.aspx) de GridView y agregue el código siguiente:

[!code-vb[Main](user-based-authorization-vb/samples/sample12.vb)]

Este código usa la propiedad `SelectedValue` de GridView para determinar el nombre de archivo completo del archivo seleccionado. Internamente, se hace referencia a la colección de `DataKeys` para obtener el `SelectedValue`, por lo que es imperativo establecer la propiedad `DataKeyNames` de GridView en Name, como se describió anteriormente en este paso. La [clase`File`](https://msdn.microsoft.com/library/system.io.file.aspx) se utiliza para leer el contenido del archivo seleccionado en una cadena, que se asigna a la propiedad `Text` del cuadro de `FileContents`, con lo que se muestra el contenido del archivo seleccionado en la página.

[![el contenido del archivo seleccionado se muestra en el cuadro de texto](user-based-authorization-vb/_static/image23.png)](user-based-authorization-vb/_static/image22.png)

**Figura 8**: el contenido del archivo seleccionado se muestra en el cuadro de texto ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image24.png))

> [!NOTE]
> Si ve el contenido de un archivo que contiene marcado HTML y, a continuación, intenta ver o eliminar un archivo, recibirá un error de `HttpRequestValidationException`. Esto se debe a que, en el postback, el contenido del cuadro de texto se envía de vuelta al servidor Web. De forma predeterminada, ASP.NET genera un error `HttpRequestValidationException` cada vez que se detecta contenido de postback potencialmente peligroso, como el marcado HTML. Para impedir que se produzca este error, desactive la validación de solicitudes de la página agregando `ValidateRequest="false"` a la Directiva `@Page`. Para obtener más información sobre las ventajas de la validación de solicitudes, así como sobre las precauciones que debe tomar al deshabilitarla, consulte [validación de solicitudes de lectura: prevención de ataques de scripts](https://asp.net/learn/whitepapers/request-validation/).

Por último, agregue un controlador de eventos con el siguiente código para el [evento`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx)de GridView:

[!code-vb[Main](user-based-authorization-vb/samples/sample13.vb)]

El código simplemente muestra el nombre completo del archivo que se va a eliminar en el cuadro de texto `FileContents` *sin* eliminar realmente el archivo.

[![hacer clic en el botón eliminar no elimina realmente el archivo](user-based-authorization-vb/_static/image26.png)](user-based-authorization-vb/_static/image25.png)

**Figura 9**: al hacer clic en el botón eliminar, realmente no se elimina el archivo ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image27.png))

En el paso 1, se han configurado las reglas de autorización de URL para impedir que los usuarios anónimos vean las páginas de la carpeta `Membership`. Con el fin de mejorar la autenticación precisa, vamos a permitir a los usuarios anónimos visitar la página `UserBasedAuthorization.aspx`, pero con una funcionalidad limitada. Para abrir esta página hasta que todos los usuarios puedan tener acceso a ella, agregue el siguiente elemento de `<location>` al archivo de `Web.config` en la carpeta `Membership`:

[!code-xml[Main](user-based-authorization-vb/samples/sample14.xml)]

Después de agregar este elemento de `<location>`, pruebe las nuevas reglas de autorización de URL cerrando el sitio. Como usuario anónimo, debe tener permiso para visitar la página `UserBasedAuthorization.aspx`.

Actualmente, cualquier usuario autenticado o anónimo puede visitar la página `UserBasedAuthorization.aspx` y ver o eliminar archivos. Vamos a hacerlo para que solo los usuarios autenticados puedan ver el contenido de un archivo y solo Tito puedan eliminar un archivo. Estas reglas de autorización específicas se pueden aplicar mediante declaración, mediante programación o a través de una combinación de ambos métodos. Vamos a usar el enfoque declarativo para limitar quién puede ver el contenido de un archivo. usaremos el enfoque de programación para limitar quién puede eliminar un archivo.

### <a name="using-the-loginview-control"></a>Usar el control LoginView

Como hemos visto en los tutoriales anteriores, el control LoginView es útil para mostrar diferentes interfaces para usuarios autenticados y anónimos, y ofrece una manera sencilla de ocultar funciones que no son accesibles para los usuarios anónimos. Puesto que los usuarios anónimos no pueden ver ni eliminar archivos, solo es necesario mostrar el cuadro de texto `FileContents` cuando un usuario autenticado visita la página. Para ello, agregue un control LoginView a la página, asígnele el nombre `LoginViewForFileContentsTextBox`y mueva el marcado declarativo del cuadro de texto `FileContents` al `LoggedInTemplate`del control LoginView.

[!code-aspx[Main](user-based-authorization-vb/samples/sample15.aspx)]

Los controles Web de las plantillas de LoginView ya no son accesibles directamente desde la clase de código subyacente. Por ejemplo, la `FilesGrid` los controladores de eventos `SelectedIndexChanged` y `RowDeleting` de GridView hacen referencia actualmente al control TextBox `FileContents` con código como el siguiente:

[!code-aspx[Main](user-based-authorization-vb/samples/sample16.aspx)]

Sin embargo, este código ya no es válido. Al mover el cuadro de texto `FileContents` al `LoggedInTemplate` no se puede tener acceso directamente al cuadro de texto. En su lugar, se debe usar el método `FindControl("controlId")` para hacer referencia al control mediante programación. Actualice los controladores de eventos `FilesGrid` para hacer referencia al cuadro de texto de la manera siguiente:

[!code-vb[Main](user-based-authorization-vb/samples/sample17.vb)]

Después de mover el cuadro de texto a la `LoggedInTemplate` de LoginView y actualizar el código de la página para hacer referencia al cuadro de texto mediante el patrón de `FindControl("controlId")`, visite la página como un usuario anónimo. Como se muestra en la figura 10, no se muestra el cuadro de texto `FileContents`. Sin embargo, se sigue mostrando la vista LinkButton.

[![el control LoginView solo representa el cuadro de texto FileContents para los usuarios autenticados](user-based-authorization-vb/_static/image29.png)](user-based-authorization-vb/_static/image28.png)

**Figura 10**: el control LoginView solo representa el cuadro de texto `FileContents` para los usuarios autenticados ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image30.png))

Una manera de ocultar el botón ver para los usuarios anónimos es convertir el campo GridView en TemplateField. Se generará una plantilla que contiene el marcado declarativo para la vista LinkButton. A continuación, podemos agregar un control LoginView a TemplateField y colocar el control LinkButton en el `LoggedInTemplate`de LoginView, ocultando así el botón ver de los visitantes anónimos. Para ello, haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView para abrir el cuadro de diálogo campos. A continuación, seleccione el botón seleccionar de la lista en la esquina inferior izquierda y, a continuación, haga clic en el vínculo convertir este campo en un TemplateField. Al hacerlo, se modificará el marcado declarativo del campo desde:

[!code-aspx[Main](user-based-authorization-vb/samples/sample18.aspx)]

 A: 

[!code-aspx[Main](user-based-authorization-vb/samples/sample19.aspx)]

 En este momento, podemos agregar un LoginView a TemplateField. El marcado siguiente muestra la vista LinkButton solo para los usuarios autenticados. 

[!code-aspx[Main](user-based-authorization-vb/samples/sample20.aspx)]

Como se muestra en la figura 11, el resultado final no es que, al igual que la columna de vista, se sigue mostrando aunque se oculte la vista LinkButtons dentro de la columna. Veremos cómo ocultar la columna de GridView completa (y no solo el control LinkButton) en la sección siguiente.

[![el control LoginView oculta la vista LinkButtons para los visitantes anónimos](user-based-authorization-vb/_static/image32.png)](user-based-authorization-vb/_static/image31.png)

**Figura 11**: el control LoginView oculta la vista LinkButtons para los visitantes anónimos ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Limitar la funcionalidad mediante programación

En algunas circunstancias, las técnicas declarativas no son suficientes para limitar la funcionalidad a una página. Por ejemplo, la disponibilidad de determinada funcionalidad de página puede depender de criterios que van más allá de si el usuario que visita la página es anónimo o autenticado. En tales casos, los distintos elementos de la interfaz de usuario se pueden mostrar u ocultar a través de promedios mediante programación.

Para limitar la funcionalidad mediante programación, es necesario realizar dos tareas:

1. Determine si el usuario que visita la página puede acceder a la funcionalidad y
2. Modifique mediante programación la interfaz de usuario en función de si el usuario tiene acceso a la funcionalidad en cuestión.

Para mostrar la aplicación de estas dos tareas, vamos a permitir que Tito elimine archivos de GridView. La primera tarea, a continuación, es determinar si está Tito visitando la página. Una vez que se ha determinado, es necesario ocultar (o mostrar) la columna de eliminación de GridView. Se puede tener acceso a las columnas de GridView a través de su propiedad `Columns`; una columna solo se representa si su propiedad `Visible` está establecida en `True` (valor predeterminado).

Agregue el código siguiente al controlador de eventos `Page_Load` antes de enlazar los datos a GridView:

[!code-vb[Main](user-based-authorization-vb/samples/sample21.vb)]

Como se explicó en el tutorial [*de introducción a la autenticación de formularios*](../introduction/an-overview-of-forms-authentication-vb.md) , `User.Identity.Name` devuelve el nombre de la identidad. Esto corresponde al nombre de usuario especificado en el control de inicio de sesión. Si Tito visita la página, la propiedad `Visible` de la segunda columna de GridView está establecida en `True`; de lo contrario, se establece en `False`. El resultado neto es que cuando alguien que no sea Tito visita la página, ya sea otro usuario autenticado o un usuario anónimo, la columna eliminar no se representa (vea la figura 12). sin embargo, cuando Tito visita la página, la columna eliminar está presente (vea la figura 13).

[![la columna eliminar no se representará cuando la visiten otros usuarios que no sean Tito (como Bruce)](user-based-authorization-vb/_static/image35.png)](user-based-authorization-vb/_static/image34.png)

**Figura 12**: la columna eliminar no se representa cuando la visita alguien que no sea Tito (por ejemplo, Bruce) ([haga clic para ver la imagen a tamaño completo](user-based-authorization-vb/_static/image36.png))

[![se representa la columna delete para Tito](user-based-authorization-vb/_static/image38.png)](user-based-authorization-vb/_static/image37.png)

**Figura 13**: la columna Delete se representa para Tito ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image39.png))

## <a name="step-4-applying-authorization-rules-to-classes-and-methods"></a>Paso 4: aplicar reglas de autorización a clases y métodos

En el paso 3, no permitimos que los usuarios anónimos puedan ver el contenido de un archivo y que todos los usuarios se Tito de eliminar archivos. Esto se logra ocultando los elementos de la interfaz de usuario asociados a los visitantes no autorizados a través de técnicas declarativas y de programación. En nuestro sencillo ejemplo, la ocultación correcta de los elementos de la interfaz de usuario era sencilla, pero ¿qué ocurre con sitios más complejos en los que puede haber muchas formas diferentes de realizar la misma funcionalidad? Al limitar esa funcionalidad a usuarios no autorizados, ¿qué ocurre si se olvida de ocultar o deshabilitar todos los elementos de la interfaz de usuario aplicables?

Una manera sencilla de asegurarse de que un usuario no autorizado no pueda acceder a una parte determinada de la funcionalidad es decorar esa clase o método con el [`PrincipalPermission` atributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Cuando el tiempo de ejecución de .NET usa una clase o ejecuta uno de sus métodos, comprueba para asegurarse de que el contexto de seguridad actual tiene permiso para usar la clase o ejecutar el método. El atributo `PrincipalPermission` proporciona un mecanismo a través del cual se pueden definir estas reglas.

Vamos a mostrar el uso del atributo `PrincipalPermission` en los controladores de eventos `SelectedIndexChanged` y `RowDeleting` de GridView para prohibir la ejecución por parte de usuarios anónimos y usuarios que no sean Tito, respectivamente. Lo único que debemos hacer es agregar el atributo adecuado sobre cada definición de función:

[!code-vb[Main](user-based-authorization-vb/samples/sample22.vb)]

El atributo del controlador de eventos `SelectedIndexChanged` determina que solo los usuarios autenticados pueden ejecutar el controlador de eventos, donde como el atributo del controlador de eventos `RowDeleting` limita la ejecución a Tito.

> [!NOTE]
> Un atributo se puede aplicar a una clase, método, propiedad o evento. Al agregar un atributo, debe formar parte de la instrucción de declaración de clase, método, propiedad o evento. Como Visual Basic usa saltos de línea como delimitadores de instrucciones, los atributos deben aparecer en la misma línea que la declaración o directamente encima de él con un carácter de continuación de línea (el carácter de subrayado). En el fragmento de código anterior, el carácter de continuación de línea se usa para colocar el atributo en una línea y la declaración del método en otro.

Si, de algún modo, un usuario que no sea Tito intenta ejecutar el controlador de eventos `RowDeleting` o un usuario no autenticado intenta ejecutar el controlador de eventos `SelectedIndexChanged`, el tiempo de ejecución de .NET generará una `SecurityException`.

[![si el contexto de seguridad no está autorizado para ejecutar el método, se produce una excepción SecurityException](user-based-authorization-vb/_static/image41.png)](user-based-authorization-vb/_static/image40.png)

**Figura 14**: Si el contexto de seguridad no está autorizado para ejecutar el método, se produce una `SecurityException` ([haga clic para ver la imagen de tamaño completo](user-based-authorization-vb/_static/image42.png))

> [!NOTE]
> Para permitir que varios contextos de seguridad tengan acceso a una clase o método, decore la clase o el método con un atributo `PrincipalPermission` para cada contexto de seguridad. Es decir, para permitir que Tito y Bruce ejecuten el controlador de eventos `RowDeleting`, agregue *dos* atributos `PrincipalPermission`:

[!code-vb[Main](user-based-authorization-vb/samples/sample23.vb)]

Además de las páginas de ASP.NET, muchas aplicaciones también tienen una arquitectura que incluye varias capas, como las capas de lógica de negocios y de acceso a datos. Estas capas se implementan normalmente como bibliotecas de clases y ofrecen clases y métodos para realizar la lógica empresarial y la funcionalidad relacionada con los datos. El atributo `PrincipalPermission` es útil para aplicar reglas de autorización a estas capas.

Para obtener más información sobre el uso del atributo `PrincipalPermission` para definir reglas de autorización en clases y métodos, consulte la entrada de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/)sobre cómo [agregar reglas de autorización a las capas de datos y empresariales mediante `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumen

En este tutorial, hemos examinado cómo aplicar reglas de autorización basadas en el usuario. Comenzamos con una mirada a ASP. Marco de autorización de dirección URL de la red. En cada solicitud, el `UrlAuthorizationModule` del motor de ASP.NET inspecciona las reglas de autorización de URL definidas en la configuración de la aplicación para determinar si la identidad está autorizada para tener acceso al recurso solicitado. En Resumen, la autorización de direcciones URL facilita la especificación de reglas de autorización para una página específica o para todas las páginas de un directorio determinado.

El marco de autorización de URL aplica las reglas de autorización cada página. Con la autorización de URL, la identidad solicitante está autorizada para tener acceso a un recurso determinado o no. Sin embargo, muchos escenarios llaman a para las reglas de autorización más granulares. En lugar de definir quién tiene permiso para acceder a una página, es posible que necesitemos permitir que todos tengan acceso a una página, pero para Mostrar datos diferentes o para ofrecer una funcionalidad diferente en función del usuario que visita la página. La autorización en el nivel de página normalmente implica Ocultar elementos específicos de la interfaz de usuario para evitar que usuarios no autorizados tengan acceso a la funcionalidad prohibida. Además, es posible utilizar atributos para restringir el acceso a las clases y la ejecución de sus métodos para determinados usuarios.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Adición de reglas de autorización a las capas de negocio y de datos con `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Autorización de ASP.NET](https://msdn.microsoft.com/library/wce3kxhd.aspx)
- [Cambios entre la seguridad de IIS6 y IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Configuración de archivos y subdirectorios específicos](https://msdn.microsoft.com/library/6hbkh9s7.aspx)
- [Limitar la funcionalidad de modificación de datos basada en el usuario](../../data-access/editing-inserting-and-deleting-data/limiting-data-modification-functionality-based-on-the-user-vb.md)
- [Inicios rápidos del control LoginView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/loginview.aspx)
- [Descripción de la autorización de URL de IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization)
- [`UrlAuthorizationModule` documentación técnica](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)
- [Trabajar con datos en ASP.NET 2,0](../../data-access/index.md)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](validating-user-credentials-against-the-membership-user-store-vb.md)
> [Siguiente](storing-additional-user-information-vb.md)
