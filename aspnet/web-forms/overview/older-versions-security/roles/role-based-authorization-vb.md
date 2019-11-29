---
uid: web-forms/overview/older-versions-security/roles/role-based-authorization-vb
title: Autorización basada en roles (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial se empieza con una visión de cómo el marco de trabajo de roles asocia los roles de un usuario con su contexto de seguridad. A continuación, examina cómo aplicar la dirección URL basada en roles...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 83b4f5a4-4f5a-4380-ba33-f0b5c5ac6a75
msc.legacyurl: /web-forms/overview/older-versions-security/roles/role-based-authorization-vb
msc.type: authoredcontent
ms.openlocfilehash: feb3e5eb992284033853e67bfab3872243cefe39
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74570634"
---
# <a name="role-based-authorization-vb"></a>Autorización basada en roles (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.11.zip) o [Descargar PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial11_RoleAuth_vb.pdf)

> En este tutorial se empieza con una visión de cómo el marco de trabajo de roles asocia los roles de un usuario con su contexto de seguridad. A continuación, examina cómo aplicar reglas de autorización de URL basadas en roles. A continuación, veremos el uso de métodos declarativos y de programación para modificar los datos mostrados y la funcionalidad que ofrece una página de ASP.NET.

## <a name="introduction"></a>Introducción

En el <a id="_msoanchor_1"> </a>tutorial de [*autorización basada*](../membership/user-based-authorization-vb.md) en el usuario vimos cómo usar la autorización de URL para especificar qué usuarios pueden visitar un conjunto determinado de páginas. Con tan solo un poco de marcado en `Web.config`, podríamos indicar a ASP.NET que solo permitiera a los usuarios autenticados visitar una página. O bien, podríamos obligar a que solo se permitieran los usuarios Tito y Bob, o que indiquen que se permitían todos los usuarios autenticados excepto Sam.

Además de la autorización de URL, también hemos examinado las técnicas declarativas y de programación para controlar los datos que se muestran y la funcionalidad que ofrece una página basada en el usuario que visita. En concreto, hemos creado una página que muestra el contenido del directorio actual. Cualquier persona podría visitar esta página, pero solo los usuarios autenticados podrían ver el contenido de los archivos y solo Tito podrían eliminarlos.

La aplicación de reglas de autorización para cada usuario puede aumentar la pesadilla. Un enfoque más fácil de mantener es usar la autorización basada en roles. La buena noticia es que las herramientas de nuestra disposición para aplicar reglas de autorización funcionan igual de bien con roles que para las cuentas de usuario. Las reglas de autorización de URL pueden especificar roles en lugar de usuarios. El control LoginView, que representa una salida diferente para los usuarios autenticados y anónimos, se puede configurar para mostrar contenido diferente en función de los roles del usuario que ha iniciado sesión. Y la API de roles incluye métodos para determinar los roles del usuario que ha iniciado sesión.

En este tutorial se empieza con una visión de cómo el marco de trabajo de roles asocia los roles de un usuario con su contexto de seguridad. A continuación, examina cómo aplicar reglas de autorización de URL basadas en roles. A continuación, veremos el uso de métodos declarativos y de programación para modificar los datos mostrados y la funcionalidad que ofrece una página de ASP.NET. Comencemos.

## <a name="understanding-how-roles-are-associated-with-a-users-security-context"></a>Descripción de cómo se asocian los roles con el contexto de seguridad de un usuario

Cada vez que una solicitud entra en la canalización ASP.NET, se asocia a un contexto de seguridad, que incluye información que identifica al solicitante. Al utilizar la autenticación de formularios, se usa un vale de autenticación como un token de identidad. Como se explicó en los <a id="_msoanchor_2"> </a>tutoriales [*información general de la autenticación de formularios*](../introduction/an-overview-of-forms-authentication-vb.md) y <a id="_msoanchor_3"> </a>la [*configuración de autenticación de formularios y temas avanzados*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) , el `FormsAuthenticationModule` es responsable de determinar la identidad del solicitante, lo que hace durante el [evento`AuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx).

Si se encuentra un vale de autenticación no expirado válido, el `FormsAuthenticationModule` lo descodifica para determinar la identidad del solicitante. Crea un nuevo objeto `GenericPrincipal` y lo asigna al objeto `HttpContext.User`. El propósito de una entidad de seguridad, como `GenericPrincipal`, es identificar el nombre del usuario autenticado y los roles a los que pertenece. Este propósito es evidente por el hecho de que todos los objetos de entidad de seguridad tienen una propiedad `Identity` y un método `IsInRole(roleName)`. Sin embargo, el `FormsAuthenticationModule`no está interesado en registrar información de roles y el objeto de `GenericPrincipal` que crea no especifica ningún rol.

Si el marco de trabajo de roles está habilitado, el módulo HTTP [`RoleManagerModule`](https://msdn.microsoft.com/library/system.web.security.rolemanagermodule.aspx) pasos en después del `FormsAuthenticationModule` e identifica los roles del usuario autenticado durante el [evento`PostAuthenticateRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), que se desencadena después del evento de `AuthenticateRequest`. Si la solicitud proviene de un usuario autenticado, el `RoleManagerModule` sobrescribe el `GenericPrincipal` objeto creado por el `FormsAuthenticationModule` y lo reemplaza por un [objeto de`RolePrincipal`](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx). La clase `RolePrincipal` usa la API de roles para determinar los roles a los que pertenece el usuario.

La figura 1 muestra el flujo de trabajo de la canalización ASP.NET cuando se usa la autenticación de formularios y el marco de trabajo de roles. El `FormsAuthenticationModule` se ejecuta primero, identifica al usuario a través de su vale de autenticación y crea un nuevo objeto `GenericPrincipal`. A continuación, el `RoleManagerModule` pasos en y sobrescribe el objeto `GenericPrincipal` con un objeto `RolePrincipal`.

Si un usuario anónimo visita el sitio, ni el `FormsAuthenticationModule` ni el `RoleManagerModule` crea un objeto de entidad de seguridad.

[![los eventos de canalización ASP.NET para un usuario autenticado cuando se usa la autenticación de formularios y el marco de trabajo de roles](role-based-authorization-vb/_static/image2.png)](role-based-authorization-vb/_static/image1.png)

**Figura 1**: eventos de canalización ASP.net para un usuario autenticado cuando se usa la autenticación de formularios y el marco de trabajo de roles ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image3.png))

### <a name="caching-role-information-in-a-cookie"></a>Almacenar en caché información de roles en una cookie

El método de `IsInRole(roleName)` del objeto `RolePrincipal` llama a `Roles`.`GetRolesForUser` para obtener los roles para el usuario con el fin de determinar si el usuario es un miembro de *roleName*. Al utilizar el `SqlRoleProvider`, esto da como resultado una consulta a la base de datos del almacén de roles. Al usar las reglas de autorización de URL basada en roles, se llamará al método `IsInRole` del `RolePrincipal`en cada solicitud a una página protegida por las reglas de autorización de la dirección URL basada en roles. En lugar de tener que buscar la información de roles en la base de datos en cada solicitud, el marco de `Roles` incluye una opción para almacenar en caché los roles del usuario en una cookie.

Si el marco de trabajo de roles está configurado para almacenar en caché los roles del usuario en una cookie, el `RoleManagerModule` crea la cookie durante el [evento`EndRequest`](https://msdn.microsoft.com/library/system.web.httpapplication.endrequest.aspx)de la canalización ASP.net. Esta cookie se usa en las solicitudes posteriores en el `PostAuthenticateRequest`, que es cuando se crea el objeto `RolePrincipal`. Si la cookie es válida y no ha expirado, los datos de la cookie se analizan y se utilizan para rellenar los roles del usuario, con lo que se evita que el `RolePrincipal` tenga que realizar una llamada a la clase `Roles` para determinar los roles del usuario. En la ilustración 2 se muestra este flujo de trabajo.

[![la información de rol del usuario se puede almacenar en una cookie para mejorar el rendimiento](role-based-authorization-vb/_static/image5.png)](role-based-authorization-vb/_static/image4.png)

**Figura 2**: la información de rol del usuario se puede almacenar en una cookie para mejorar el rendimiento ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image6.png))

De forma predeterminada, el mecanismo de cookies de la caché de rol está deshabilitado. Se puede habilitar mediante el `<roleManager>`; marcado de configuración en `Web.config`. Hemos explicado cómo usar el [elemento`<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx) para especificar proveedores de <a id="_msoanchor_4"> </a>roles en el tutorial [*creación y administración de roles*](creating-and-managing-roles-vb.md) , por lo que ya debe tener este elemento en el archivo de `Web.config` de la aplicación. La configuración de cookies de la caché de rol se especifica como atributos de la `<roleManager>`; y se resumen en la tabla 1.

> [!NOTE]
> Los valores de configuración que aparecen en la tabla 1 especifican las propiedades de la cookie de caché de rol resultante. Para obtener más información sobre las cookies, cómo funcionan y sus distintas propiedades, lea [este tutorial de cookies](http://www.quirksmode.org/js/cookies.html).

| <strong>Property</strong> |                                                                                                                                                                                                                                                                                                                                                         <strong>Descripción</strong>                                                                                                                                                                                                                                                                                                                                                          |
|---------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   `cacheRolesInCookie`    |                                                                                                                                                                                                                                                                                                                              Valor booleano que indica si se utiliza el almacenamiento en caché de cookies. Tiene como valor predeterminado `false`.                                                                                                                                                                                                                                                                                                                              |
|       `cookieName`        |                                                                                                                                                                                                                                                                                                                                     Nombre de la cookie de caché de rol. El valor predeterminado es ". ASPXROLES ".                                                                                                                                                                                                                                                                                                                                     |
|       `cookiePath`        |                                                                                                                                                                                                                                La ruta de acceso de la cookie de nombre de roles. El atributo path permite a un programador limitar el ámbito de una cookie a una jerarquía de directorios determinada. El valor predeterminado es "/", que informa al explorador de que envíe la cookie de vale de autenticación a cualquier solicitud realizada al dominio.                                                                                                                                                                                                                                 |
|    `cookieProtection`     |                                                                                                                                                               Indica las técnicas que se usan para proteger la cookie de la caché de roles. Los valores permitidos son: `All` (valor predeterminado); `Encryption`; `None`; y `Validation`. Consulte el <a id="_anchor_5"> </a>paso 3 del tutorial [*configuración de autenticación de formularios y temas avanzados*](../introduction/forms-authentication-configuration-and-advanced-topics-vb.md) para obtener más información sobre estos niveles de protección.                                                                                                                                                                |
|    `cookieRequireSSL`     |                                                                                                                                                                                                                                                                                                   Valor booleano que indica si se requiere una conexión SSL para transmitir la cookie de autenticación. El valor predeterminado es `false`.                                                                                                                                                                                                                                                                                                   |
| `cookieSlidingExpiration` |                                                                                                                                                                                                                                                  Valor booleano que indica si el tiempo de espera de la cookie se restablece cada vez que el usuario visita el sitio durante una sola sesión. El valor predeterminado es `false`. Este valor solo es relevante cuando `createPersistentCookie` está establecido en `true`.                                                                                                                                                                                                                                                  |
|      `cookieTimeout`      |                                                                                                                                                                                                                                                                         Especifica el tiempo, en minutos, después del cual expira la cookie del vale de autenticación. El valor predeterminado es `30`. Este valor solo es relevante cuando `createPersistentCookie` está establecido en `true`.                                                                                                                                                                                                                                                                         |
| `createPersistentCookie`  |                                                                                                                                                                   Valor booleano que especifica si la cookie de caché de rol es una cookie de sesión o una cookie persistente. Si `false` (valor predeterminado), se usa una cookie de sesión, que se elimina cuando se cierra el explorador. Si `true`, se usa una cookie persistente; expira `cookieTimeout` número de minutos después de que se haya creado o después de la visita anterior, en función del valor de `cookieSlidingExpiration`.                                                                                                                                                                    |
|         `domain`          |                                                                                                                                                 Especifica el valor de dominio de la cookie. El valor predeterminado es una cadena vacía, que hace que el explorador use el dominio desde el que se emitió (por ejemplo, www.yourdomain.com). En este caso, la cookie <strong>no</strong> se enviará al realizar solicitudes a subdominios, como admin.yourdomain.com. Si desea que la cookie se pase a todos los subdominios, debe personalizar el `domain` atributo, estableciéndolo en "yourdomain.com".                                                                                                                                                 |
|    `maxCachedResults`     | Especifica el número máximo de nombres de rol que se almacenan en caché en la cookie. El valor predeterminado es 25. El `RoleManagerModule` no crea una cookie para los usuarios que pertenecen a más de `maxCachedResults` roles. Por consiguiente, el método `IsInRole` del objeto `RolePrincipal` utilizará la clase `Roles` para determinar los roles del usuario. La razón `maxCachedResults` existe es porque muchos agentes de usuario no admiten cookies de más de 4.096 bytes. Por lo tanto, este extremo está pensado para reducir la probabilidad de superar este límite de tamaño. Si tiene nombres de rol muy largos, puede que desee considerar la posibilidad de especificar un valor de `maxCachedResults` menor; contrariwise, si tiene nombres de rol muy cortos, probablemente pueda aumentar este valor. |

**Tabla 1**: opciones de configuración de cookies de la caché de rol

Vamos a configurar nuestra aplicación para que use cookies de caché de rol no persistente. Para ello, actualice el elemento `<roleManager>` en `Web.config` para incluir los siguientes atributos relacionados con las cookies:

[!code-xml[Main](role-based-authorization-vb/samples/sample1.xml)]

He actualizado el `<roleManager>`; elemento agregando tres atributos: `cacheRolesInCookie`, `createPersistentCookie`y `cookieProtection`. Al establecer `cacheRolesInCookie` en `true`, el `RoleManagerModule` almacenará automáticamente en caché los roles del usuario en una cookie en lugar de tener que buscar la información del rol del usuario en cada solicitud. Estableció explícitamente los atributos `createPersistentCookie` y `cookieProtection` en `false` y `All`, respectivamente. Técnicamente, no era necesario especificar valores para estos atributos, ya que simplemente se les ha asignado a sus valores predeterminados, pero los pongo aquí para que queden claramente claros que no utilizo cookies persistentes y que la cookie se cifra y valida.

Así de simple. En adelante, el marco de trabajo de roles almacenará en la caché los roles de los usuarios en cookies. Si el explorador del usuario no admite cookies, o si sus cookies se eliminan o se pierden, de algún modo no es importante: el objeto de `RolePrincipal` simplemente usará la clase `Roles` en caso de que no haya ninguna cookie (o una no válida o expirada).

> [!NOTE]
> El grupo Patterns &amp; Practices de Microsoft no recomienda el uso de cookies de caché de rol persistentes. Puesto que la posesión de la cookie de caché de rol es suficiente para demostrar la pertenencia al rol, si un pirata informático puede obtener acceso de algún modo a la cookie de un usuario válido, puede suplantar a ese usuario. La probabilidad de que esto suceda aumenta si la cookie se conserva en el explorador del usuario. Para obtener más información sobre esta recomendación de seguridad, así como otros problemas de seguridad, consulte la [lista de preguntas de seguridad de ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx).

## <a name="step-1-defining-role-based-url-authorization-rules"></a>Paso 1: definición de las reglas de autorización de URL basada en roles

Tal y como se <a id="_msoanchor_6"> </a>describe en el tutorial de [*autorización basada*](../membership/user-based-authorization-vb.md) en el usuario, la autorización de direcciones URL ofrece un medio para restringir el acceso a un conjunto de páginas para cada usuario o función. Las reglas de autorización de URL se escriben en `Web.config` mediante el [elemento`<authorization>`](https://msdn.microsoft.com/library/8d82143t.aspx) con `<allow>` y `<deny>` elementos secundarios. Además de las reglas de autorización relacionadas con el usuario descritas en los tutoriales anteriores, cada `<allow>` y `<deny>` elemento secundario también puede incluir:

- Un rol determinado
- Una lista delimitada por comas de roles

Por ejemplo, las reglas de autorización de URL conceden acceso a los usuarios de los roles administradores y supervisores, pero deniegan el acceso a todos los demás:

[!code-xml[Main](role-based-authorization-vb/samples/sample2.xml)]

El elemento `<allow>` en el marcado anterior indica que se permiten los roles administradores y supervisores; `<deny>`; el elemento indica que se deniegan *todos* los usuarios.

Vamos a configurar nuestra aplicación para que las páginas `ManageRoles.aspx`, `UsersAndRoles.aspx`y `CreateUserWizardWithRoles.aspx` solo sean accesibles para los usuarios del rol administradores, mientras que la página de `RoleBasedAuthorization.aspx` permanece accesible para todos los visitantes.

Para ello, empiece agregando un archivo `Web.config` a la carpeta `Roles`.

[![agregar un archivo Web. config al directorio de roles](role-based-authorization-vb/_static/image8.png)](role-based-authorization-vb/_static/image7.png)

**Figura 3**: agregar un archivo de `Web.config` al directorio `Roles` ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image9.png))

A continuación, agregue el siguiente marcado de configuración a `Web.config`:

[!code-xml[Main](role-based-authorization-vb/samples/sample3.xml)]

El elemento `<authorization>` de la sección `<system.web>` indica que solo los usuarios del rol administradores pueden acceder a los recursos de ASP.NET en el directorio `Roles`. El elemento `<location>` define un conjunto alternativo de reglas de autorización de URL para la página `RoleBasedAuthorization.aspx`, lo que permite que todos los usuarios visiten la página.

Después de guardar los cambios en `Web.config`, inicie sesión como un usuario que no está en el rol administradores y, a continuación, intente visitar una de las páginas protegidas. El `UrlAuthorizationModule` detectará que no tiene permiso para visitar el recurso solicitado. por lo tanto, el `FormsAuthenticationModule` le redirigirá a la página de inicio de sesión. La página de inicio de sesión le redirigirá a la página `UnauthorizedAccess.aspx` (consulte la figura 4). Esta redirección final de la página de inicio de sesión a `UnauthorizedAccess.aspx` se produce debido al código que hemos agregado a la página de <a id="_msoanchor_7"> </a>inicio de sesión en el paso 2 del tutorial de [*autorización basada*](../membership/user-based-authorization-vb.md) en el usuario. En concreto, la página de inicio de sesión redirige automáticamente cualquier usuario autenticado a `UnauthorizedAccess.aspx` si la cadena de consulta contiene un parámetro `ReturnUrl`, ya que este parámetro indica que el usuario llegó a la página de inicio de sesión después de intentar ver una página que no estaba autorizado a ver.

[![solo los usuarios del rol administradores pueden ver las páginas protegidas](role-based-authorization-vb/_static/image11.png)](role-based-authorization-vb/_static/image10.png)

**Figura 4**: solo los usuarios del rol administradores pueden ver las páginas protegidas ([haga clic para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image12.png))

Cierre la sesión y, a continuación, inicie sesión como un usuario con el rol administradores. Ahora debería poder ver las tres páginas protegidas.

[![Tito puede visitar la página UsersAndRoles. aspx porque tiene el rol administradores](role-based-authorization-vb/_static/image14.png)](role-based-authorization-vb/_static/image13.png)

**Figura 5**: Tito puede visitar la página `UsersAndRoles.aspx` porque tiene el rol administradores ([haga clic para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image15.png))

> [!NOTE]
> Al especificar las reglas de autorización de URL (para roles o usuarios) es importante tener en cuenta que las reglas se analizan de una en una, de arriba abajo. En cuanto se encuentra una coincidencia, se concede o se deniega el acceso al usuario, en función de si se encontró la coincidencia en un elemento `<allow>` o `<deny>`. **Si no se encuentra ninguna coincidencia, se concede al usuario acceso.** Por lo tanto, si desea restringir el acceso a una o varias cuentas de usuario, es imperativo que use un elemento `<deny>` como el último elemento en la configuración de autorización de la dirección URL. **Si las reglas de autorización de URL no incluyen un** **elemento`<deny>`, se concederá acceso a todos los usuarios.** Para obtener una explicación más detallada sobre cómo se analizan las reglas de autorización de URL, consulte la sección "un vistazo a cómo utiliza la `UrlAuthorizationModule` las reglas de autorización para conceder o denegar el acceso" del tutorial de <a id="_msoanchor_8"> </a> [*autorización basada en usuario*](../membership/user-based-authorization-vb.md) .

## <a name="step-2-limiting-functionality-based-on-the-currently-logged-in-users-roles"></a>Paso 2: limitar la funcionalidad en función de los roles del usuario que ha iniciado sesión actualmente

La autorización de direcciones URL facilita la especificación de reglas de autorización generales que indican qué identidades se permiten y cuáles se deniegan para ver una página determinada (o todas las páginas de una carpeta y sus subcarpetas). Sin embargo, en algunos casos, es posible que desee permitir que todos los usuarios visiten una página, pero limiten la funcionalidad de la página en función de los roles del usuario visitante. Esto puede implicar la visualización u ocultación de los datos en función del rol del usuario o la oferta de funcionalidad adicional a los usuarios que pertenecen a un rol determinado.

Estas reglas de autorización basadas en roles de grano se pueden implementar de forma declarativa o mediante programación (o a través de alguna combinación de los dos). En la sección siguiente, veremos cómo implementar la autorización específica declarativo a través del control LoginView. A continuación, exploraremos las técnicas de programación. Sin embargo, antes de que podamos ver la aplicación de reglas de autorización de grano fino, primero tenemos que crear una página cuya funcionalidad depende del rol del usuario que la visita.

Vamos a crear una página que enumera todas las cuentas de usuario del sistema en un control GridView. GridView incluirá el nombre de usuario, la dirección de correo electrónico, la fecha del último inicio de sesión y los comentarios del usuario. Además de mostrar la información de cada usuario, el control GridView incluirá capacidades de edición y eliminación. Esta página se creará inicialmente con la funcionalidad de edición y eliminación disponible para todos los usuarios. En las secciones "uso del control LoginView" y "limitar la funcionalidad mediante programación", veremos cómo habilitar o deshabilitar estas características en función del rol del usuario que ha visitado.

> [!NOTE]
> La página ASP.NET que se va a compilar usa un control GridView para mostrar las cuentas de usuario. Dado que esta serie de tutoriales se centra en la autenticación de formularios, la autorización, las cuentas de usuario y los roles, no deseo gastar demasiado tiempo en hablar sobre el funcionamiento interno del control GridView. Aunque en este tutorial se proporcionan instrucciones paso a paso específicas para configurar esta página, no profundiza en los detalles del motivo por el que se realizaron ciertas opciones o el efecto que tienen determinadas propiedades en la salida representada. Para un examen exhaustivo del control GridView, consulte mi *[trabajo con datos en la serie de tutoriales de ASP.NET 2,0](../../data-access/index.md)* .

Para empezar, abra la página `RoleBasedAuthorization.aspx` de la carpeta `Roles`. Arrastre un control GridView desde la página hasta el diseñador y establezca su `ID` en `UserGrid`. En un momento, se escribirá código que llama al `Membership`.`GetAllUsers` y enlaza el objeto de `MembershipUserCollection` resultante a GridView. El `MembershipUserCollection` contiene un objeto `MembershipUser` para cada cuenta de usuario del sistema; `MembershipUser` objetos tienen propiedades como `UserName`,`Email`,`LastLoginDate` etc.

Antes de escribir el código que enlaza las cuentas de usuario a la cuadrícula, vamos a definir primero los campos de GridView. En la etiqueta inteligente de GridView, haga clic en el vínculo "editar columnas" para iniciar el cuadro de diálogo campos (vea la figura 6). Desde aquí, desactive la casilla "generar automáticamente campos" en la esquina inferior izquierda. Como queremos que este GridView incluya funcionalidades de edición y eliminación, agregue CommandField y establezca sus propiedades `ShowEditButton` y `ShowDeleteButton` en true. A continuación, agregue cuatro campos para mostrar las propiedades `UserName`, `Email`, `LastLoginDate`y `Comment`. Use un BoundField para las dos propiedades de solo lectura (`UserName` y `LastLoginDate`) y TemplateFields para los dos campos modificables (`Email` y `Comment`).

Haga que el primer BoundField muestre la propiedad `UserName`; Establezca sus propiedades `HeaderText` y `DataField` en "UserName". Este campo no se podrá modificar, así que establezca su propiedad `ReadOnly` en true. Configure el `LastLoginDate` BoundField estableciendo su `HeaderText` en "último inicio de sesión" y su `DataField` en "LastLoginDate". Vamos a dar formato a la salida de este BoundField para que solo se muestre la fecha (en lugar de la fecha y la hora). Para ello, establezca la propiedad `HtmlEncode` de BoundField en false y su propiedad `DataFormatString` en "{0:d}". Establezca también la propiedad `ReadOnly` en true.

Establezca las propiedades del `HeaderText` de los dos TemplateFields en "email" y "comment".

[![los campos de GridView se pueden configurar mediante el cuadro de diálogo campos](role-based-authorization-vb/_static/image17.png)](role-based-authorization-vb/_static/image16.png)

**Figura 6**: los campos de GridView se pueden configurar mediante el cuadro de diálogo campos ([haga clic para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image18.png))

Ahora tenemos que definir el `ItemTemplate` y `EditItemTemplate` para el "correo electrónico" y "comentario" TemplateFields. Agregue un control Web de etiqueta a cada una de las `ItemTemplates` y enlace sus propiedades de `Text` a las propiedades `Email` y `Comment`, respectivamente.

En el caso de "email", agregue un cuadro de texto denominado `Email` a su `EditItemTemplate` y enlace su propiedad `Text` a la propiedad `Email` mediante el enlace de los dos sentidos. Agregue un RequiredFieldValidator y un RegularExpressionValidator al `EditItemTemplate` para asegurarse de que un visitante que edita la propiedad de correo electrónico ha escrito una dirección de correo electrónico válida. En el caso del "comentario", agregue un cuadro de texto de varias líneas denominado `Comment` a su `EditItemTemplate`. Establezca las propiedades `Columns` y `Rows` del cuadro de texto en 40 y 4 respectivamente y, a continuación, enlace su propiedad `Text` a la propiedad `Comment` mediante el enlace de los dos sentidos.

Después de configurar estos TemplateFields, su marcado declarativo debe ser similar al siguiente:

[!code-aspx[Main](role-based-authorization-vb/samples/sample4.aspx)]

Al editar o eliminar una cuenta de usuario, es necesario saber que el valor de la propiedad `UserName` del usuario. Establezca la propiedad `DataKeyNames` de GridView en "UserName" para que esta información esté disponible a través de la colección `DataKeys` de GridView.

Por último, agregue un control ValidationSummary a la página y establezca su propiedad `ShowMessageBox` en true y su propiedad `ShowSummary` en false. Con esta configuración, ValidationSummary mostrará una alerta del lado cliente si el usuario intenta editar una cuenta de usuario con una dirección de correo electrónico que falta o no es válida.

[!code-aspx[Main](role-based-authorization-vb/samples/sample5.aspx)]

Ahora hemos completado el marcado declarativo de esta página. La siguiente tarea consiste en enlazar el conjunto de cuentas de usuario a GridView. Agregue un método denominado `BindUserGrid` a la clase de código subyacente de la página de `RoleBasedAuthorization.aspx` que enlaza el `MembershipUserCollection` devuelto por `Membership.GetAllUsers` al `UserGrid` GridView. Llame a este método desde el controlador de eventos `Page_Load` en la primera visita de la página.

[!code-vb[Main](role-based-authorization-vb/samples/sample6.vb)]

Con este código en su lugar, visite la página a través de un explorador. Como se muestra en la figura 7, debería ver una información de lista de GridView acerca de cada cuenta de usuario del sistema.

[![UserGrid GridView muestra información sobre cada usuario del sistema](role-based-authorization-vb/_static/image20.png)](role-based-authorization-vb/_static/image19.png)

**Figura 7**: el `UserGrid` GridView muestra información sobre cada usuario del sistema ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image21.png))

> [!NOTE]
> El `UserGrid` GridView enumera todos los usuarios de una interfaz no paginada. Esta interfaz de cuadrícula simple no es adecuada para escenarios en los que hay varias docenas o más usuarios. Una opción consiste en configurar GridView para habilitar la paginación. El método `Membership.GetAllUsers` tiene dos sobrecargas: una que no acepta parámetros de entrada y devuelve todos los usuarios y uno que toma valores enteros para el índice de página y el tamaño de página, y devuelve solo el subconjunto especificado de los usuarios. La segunda sobrecarga se puede utilizar para paginar más eficazmente a través de los usuarios, ya que devuelve solo el subconjunto preciso de cuentas de usuario en lugar de *todas* ellas. Si tiene miles de cuentas de usuario, puede considerar la posibilidad de usar una interfaz basada en filtros, una que solo muestra los usuarios cuyo nombre de usuario comienza por un carácter seleccionado, por ejemplo. El [método`Membership.FindUsersByName`](https://msdn.microsoft.com/library/system.web.security.membership.findusersbyname.aspx) es ideal para crear una interfaz de usuario basada en filtros. Veremos cómo crear una interfaz de este tipo en un tutorial futuro.

El control GridView ofrece compatibilidad integrada de edición y eliminación cuando el control está enlazado a un control de origen de datos configurado correctamente, como SqlDataSource o ObjectDataSource. Sin embargo, el `UserGrid` GridView tiene sus datos enlazados mediante programación; por lo tanto, debemos escribir código para realizar estas dos tareas. En concreto, es necesario crear controladores de eventos para los eventos `RowEditing`, `RowCancelingEdit`, `RowUpdating`y `RowDeleting` de GridView, que se activan cuando un visitante hace clic en los botones editar, cancelar, actualizar o eliminar de GridView.

Empiece por crear los controladores de eventos para los eventos `RowEditing`, `RowCancelingEdit`y `RowUpdating` de GridView y, a continuación, agregue el código siguiente:

[!code-vb[Main](role-based-authorization-vb/samples/sample7.vb)]

Los controladores de eventos `RowEditing` y `RowCancelingEdit` simplemente establecen la propiedad `EditIndex` de GridView y, a continuación, vuelve a enlazar la lista de cuentas de usuario a la cuadrícula. Las cosas interesantes se producen en el controlador de eventos `RowUpdating`. Este controlador de eventos se inicia asegurándose de que los datos son válidos y, a continuación, obtiene el valor `UserName` de la cuenta de usuario modificada de la colección `DataKeys`. A continuación, se hace referencia mediante programación a los cuadros de texto `Email` y `Comment` de los dos TemplateFields ' `EditItemTemplate` s. Sus propiedades de `Text` contienen la dirección de correo electrónico y el comentario editados.

Para actualizar una cuenta de usuario a través de la API de pertenencia, necesitamos obtener primero la información del usuario, que hacemos a través de una llamada a `Membership.GetUser(userName)`. Las propiedades `Email` y `Comment` del objeto devuelto `MembershipUser` se actualizan con los valores especificados en los dos cuadros de texto de la interfaz de edición. Por último, estas modificaciones se guardan con una llamada a [`Membership.UpdateUser`](https://msdn.microsoft.com/library/system.web.security.membership.updateuser.aspx). El controlador de eventos `RowUpdating` finaliza revirtiendo GridView a su interfaz de edición previa.

A continuación, cree el controlador de eventos `RowDeleting` RowDeleting y, a continuación, agregue el siguiente código:

[!code-vb[Main](role-based-authorization-vb/samples/sample8.vb)]

El controlador de eventos anterior se inicia capturando el valor `UserName` de la colección `DataKeys` de GridView; Este valor `UserName` se pasa a continuación en el [método`DeleteUser`](https://msdn.microsoft.com/library/system.web.security.membership.deleteuser.aspx)de la clase de pertenencia. El método `DeleteUser` elimina la cuenta de usuario del sistema, incluidos los datos de pertenencia relacionados (como los roles a los que pertenece este usuario). Después de eliminar el usuario, el `EditIndex` de la cuadrícula se establece en-1 (en caso de que el usuario haga clic en eliminar mientras otra fila estaba en modo de edición) y se llama al método `BindUserGrid`.

> [!NOTE]
> El botón eliminar no requiere ningún tipo de confirmación por parte del usuario antes de eliminar la cuenta de usuario. Le recomiendo que agregue algún tipo de confirmación de usuario para reducir la posibilidad de que una cuenta se elimine accidentalmente. Una de las formas más sencillas de confirmar una acción es a través de un cuadro de diálogo de confirmación en el lado cliente. Para obtener más información sobre esta técnica, consulte [Agregar confirmación del lado cliente al eliminar](https://asp.net/learn/data-access/tutorial-42-vb.aspx).

Compruebe que esta página funciona según lo previsto. Debe poder editar la dirección de correo electrónico y el comentario de cualquier usuario, así como eliminar cualquier cuenta de usuario. Puesto que todos los usuarios pueden tener acceso a la página `RoleBasedAuthorization.aspx`, cualquier usuario, incluso los visitantes anónimos, puede visitar esta página y editar y eliminar cuentas de usuario. Vamos a actualizar esta página para que solo los usuarios de los roles supervisores y administradores puedan editar la dirección de correo electrónico y el comentario de un usuario, y solo los administradores puedan eliminar una cuenta de usuario.

En la sección "usar el control LoginView" se examina el uso del control LoginView para mostrar instrucciones específicas del rol del usuario. Si una persona del rol administradores visita esta página, se mostrarán instrucciones sobre cómo editar y eliminar usuarios. Si un usuario del rol supervisores llega a esta página, se mostrarán instrucciones sobre cómo editar usuarios. Y si el visitante es anónimo o no está en el rol supervisores o administradores, se mostrará un mensaje que explica que no puede editar ni eliminar la información de la cuenta de usuario. En la sección "limitar la funcionalidad mediante programación", se escribirá código que muestra u oculta mediante programación los botones editar y eliminar según el rol del usuario.

### <a name="using-the-loginview-control"></a>Usar el control LoginView

Como hemos visto en los tutoriales anteriores, el control LoginView es útil para mostrar diferentes interfaces para usuarios autenticados y anónimos, pero el control LoginView también se puede usar para mostrar un marcado diferente en función de los roles del usuario. Vamos a usar un control LoginView para mostrar diferentes instrucciones basadas en el rol del usuario que ha visitado.

Empiece agregando un LoginView sobre el `UserGrid` GridView. Como hemos explicado anteriormente, el control LoginView tiene dos plantillas integradas: `AnonymousTemplate` y `LoggedInTemplate`. Escriba un mensaje breve en ambas plantillas que informe al usuario de que no puede editar ni eliminar información de usuario.

[!code-aspx[Main](role-based-authorization-vb/samples/sample9.aspx)]

Además de los `AnonymousTemplate` y `LoggedInTemplate`, el control LoginView puede incluir *RoleGroups*, que son plantillas específicas del rol. Cada RoleGroup contiene una sola propiedad, `Roles`, que especifica los roles a los que se aplica el RoleGroup. La propiedad `Roles` se puede establecer en un único rol (por ejemplo, "administradores") o en una lista delimitada por comas de roles (como "administradores, supervisores").

Para administrar el RoleGroups, haga clic en el vínculo "editar RoleGroups" de la etiqueta inteligente del control para abrir el editor de la colección RoleGroup. Agregue dos nuevos RoleGroups. Establezca la propiedad `Roles` de la primera RoleGroup en "Administrators" y la segunda en "supervisores".

[![administrar las plantillas específicas de roles de LoginView mediante el editor de colección RoleGroup](role-based-authorization-vb/_static/image23.png)](role-based-authorization-vb/_static/image22.png)

**Figura 8**: administración de las plantillas específicas de roles de LoginView mediante el editor de la colección RoleGroup ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image24.png))

Haga clic en Aceptar para cerrar el editor de la colección RoleGroup; Esto actualiza el marcado declarativo de LoginView para incluir una `<RoleGroups>` sección con un `<asp:RoleGroup>` elemento secundario para cada RoleGroup definido en el editor de la colección RoleGroup. Además, la lista desplegable "vistas" de la etiqueta inteligente de LoginView, que inicialmente solo mostraba el `AnonymousTemplate` y el `LoggedInTemplate`, ahora incluye también el RoleGroups agregado.

Edite RoleGroups para que los usuarios del rol supervisores muestren instrucciones sobre cómo editar cuentas de usuario, mientras que los usuarios del rol administradores reciben instrucciones para editar y eliminar. Después de realizar estos cambios, el marcado declarativo de LoginView debe ser similar al siguiente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample10.aspx)]

Después de realizar estos cambios, guarde la página y, a continuación, visítela a través de un explorador. En primer lugar, visite la página como usuario anónimo. Debe mostrarse el mensaje "no ha iniciado sesión en el sistema. Por lo tanto, no puede editar ni eliminar información de usuario ". A continuación, inicie sesión como usuario autenticado, pero uno que no esté en el rol supervisores ni administradores. Esta vez debería ver el mensaje "no es miembro de los roles supervisores o administradores. Por lo tanto, no puede editar ni eliminar información de usuario ".

A continuación, inicie sesión como un usuario que sea miembro del rol supervisores. Esta vez debería ver el mensaje específico del rol supervisores (consulte la figura 9). Y si inicia sesión como usuario en el rol administradores, debería ver el mensaje específico del rol administradores (consulte la figura 10).

[![Bruce se muestra el mensaje específico del rol supervisores](role-based-authorization-vb/_static/image26.png)](role-based-authorization-vb/_static/image25.png)

**Figura 9**: Bruce se muestra el mensaje específico del rol supervisores ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image27.png))

[![Tito se muestra el mensaje específico de la función Administrators](role-based-authorization-vb/_static/image29.png)](role-based-authorization-vb/_static/image28.png)

**Figura 10**: Tito se muestra el mensaje específico del rol administradores ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image30.png))

Como muestran las capturas de pantalla en las figuras 9 y 10, la LoginView solo representa una plantilla, incluso si se aplican varias plantillas. Bruce y Tito están registrados en los usuarios, pero el LoginView solo representa el RoleGroup coincidente y no el `LoggedInTemplate`. Además, Tito pertenece a los roles Administrators y supervisores, pero el control LoginView representa la plantilla específica de la función Administrators en lugar de los supervisores.

En la figura 11 se muestra el flujo de trabajo que usa el control LoginView para determinar la plantilla que se va a representar. Tenga en cuenta que si se especifica más de un RoleGroup, la plantilla LoginView representa el *primer* RoleGroup que coincide con. En otras palabras, si se hubieran colocado los supervisores RoleGroup como primer RoleGroup y los administradores como segundo, cuando Tito visite esta página, verá el mensaje supervisores.

[![el flujo de trabajo del control LoginView para determinar qué plantilla se va a representar](role-based-authorization-vb/_static/image32.png)](role-based-authorization-vb/_static/image31.png)

**Figura 11**: flujo de trabajo del control LoginView para determinar qué plantilla se va a representar ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image33.png))

### <a name="programmatically-limiting-functionality"></a>Limitar la funcionalidad mediante programación

Aunque el control LoginView muestra distintas instrucciones basadas en el rol del usuario que visita la página, los botones editar y cancelar permanecen visibles para todo. Es necesario ocultar mediante programación los botones editar y eliminar para los visitantes anónimos y los usuarios que no están en el rol supervisores ni administradores. Necesitamos ocultar el botón Eliminar para todos los usuarios que no sean administradores. Para lograr esto, escribiremos un fragmento de código que haga referencia mediante programación a la LinkButtons de edición y eliminación de CommandField y establezca sus propiedades de `Visible` en `False`, si es necesario.

La manera más sencilla de hacer referencia mediante programación a los controles en un CommandField es convertirlo primero en una plantilla. Para ello, haga clic en el vínculo "editar columnas" de la etiqueta inteligente de GridView, seleccione CommandField en la lista de campos actuales y haga clic en el vínculo "convertir este campo en TemplateField". Esto convierte CommandField en TemplateField con un `ItemTemplate` y `EditItemTemplate`. El `ItemTemplate` contiene el LinkButtons de edición y eliminación mientras el `EditItemTemplate` hospeda la actualización y cancela LinkButtons.

[![convertir CommandField en TemplateField](role-based-authorization-vb/_static/image35.png)](role-based-authorization-vb/_static/image34.png)

**Figura 12**: conversión de CommandField en TemplateField ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image36.png))

Actualice el LinkButtons de edición y eliminación en el `ItemTemplate`, estableciendo sus propiedades `ID` en valores de `EditButton` y `DeleteButton`, respectivamente.

[!code-aspx[Main](role-based-authorization-vb/samples/sample11.aspx)]

Cada vez que se enlazan datos a GridView, GridView enumera los registros de su propiedad `DataSource` y genera un objeto `GridViewRow` correspondiente. A medida que se crea cada objeto de `GridViewRow`, se desencadena el evento `RowCreated`. Para ocultar los botones de edición y eliminación de usuarios no autorizados, es necesario crear un controlador de eventos para este evento y hacer referencia mediante programación a las LinkButtons de edición y eliminación, estableciendo sus propiedades `Visible` en consecuencia.

Cree un controlador de eventos `RowCreated` evento y, a continuación, agregue el siguiente código:

[!code-vb[Main](role-based-authorization-vb/samples/sample12.vb)]

Tenga en cuenta que el evento `RowCreated` se desencadena para *todas* las filas de GridView, incluidos el encabezado, el pie de página, la interfaz de buscapersonas, etc. Solo queremos hacer referencia mediante programación a las LinkButtons de edición y eliminación si se trata de una fila de datos que no está en modo de edición (puesto que la fila del modo de edición tiene botones actualizar y cancelar en lugar de editar y eliminar). La instrucción `If` controla esta comprobación.

Si se trata de una fila de datos que no está en modo de edición, se hace referencia a los LinkButtons de edición y eliminación, y sus propiedades de `Visible` se establecen en función de los valores booleanos devueltos por el método `IsInRole(roleName)` del objeto `User`. El objeto `User` hace referencia a la entidad de seguridad creada por el `RoleManagerModule`; por consiguiente, el método `IsInRole(roleName)` usa la API de roles para determinar si el visitante actual pertenece a *roleName*.

> [!NOTE]
> Podríamos haber usado la clase de roles directamente, reemplazando la llamada a `User.IsInRole(roleName)` por una llamada al [método`Roles.IsUserInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx). Decidí usar el método `IsInRole(roleName)` del objeto principal en este ejemplo porque es más eficaz que usar directamente la API de roles. Anteriormente en este tutorial, se configuró el administrador de roles para almacenar en caché los roles del usuario en una cookie. Estos datos de cookies almacenados en caché solo se usan cuando se llama al método `IsInRole(roleName)` de la entidad de seguridad; las llamadas directas a la API de roles siempre implican un viaje al almacén de roles. Incluso si los roles no se almacenan en la memoria caché en una cookie, la llamada al método `IsInRole(roleName)` del objeto principal suele ser más eficaz porque cuando se llama por primera vez durante una solicitud, almacena en memoria caché los resultados. Por otro lado, la API de roles no realiza ningún almacenamiento en caché. Dado que el evento de `RowCreated` se desencadena una vez para cada fila de GridView, el uso de `User.IsInRole(roleName)` implica un solo viaje al almacén de roles, mientras que `Roles.IsUserInRole(roleName)` requiere *n* viajes, donde *N* es el número de cuentas de usuario que se muestran en la cuadrícula.

La propiedad `Visible` del botón Editar está establecida en `True` si el usuario que visita esta página está en el rol administradores o supervisores; en caso contrario, se establece en `False`. La propiedad `Visible` del botón Eliminar está establecida en `True` solo si el usuario tiene el rol administradores.

Pruebe esta página a través de un explorador. Si visita la página como visitante anónimo o como usuario que no es supervisor ni administrador, el CommandField está vacío. todavía existe, pero como una astilla fina sin los botones editar o eliminar.

> [!NOTE]
> Es posible ocultar por completo el CommandField cuando un usuario que no es supervisor y no administrador visita la página. Dejo esto como un ejercicio para el lector.

[![los botones editar y eliminar están ocultos para los usuarios que no son supervisores y no son administradores](role-based-authorization-vb/_static/image38.png)](role-based-authorization-vb/_static/image37.png)

**Figura 13**: los botones editar y eliminar están ocultos para los usuarios que no son supervisores y no son administradores ([haga clic para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image39.png))

Si un usuario que pertenece al rol supervisores (pero no al rol administradores) visita, solo ve el botón Editar.

[![mientras el botón Editar está disponible para los supervisores, el botón Eliminar está oculto](role-based-authorization-vb/_static/image41.png)](role-based-authorization-vb/_static/image40.png)

**Figura 14**: mientras el botón Editar está disponible para los supervisores, el botón Eliminar está oculto ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image42.png))

Y si un administrador visita, tiene acceso a los botones editar y eliminar.

[![los botones editar y eliminar solo están disponibles para los administradores](role-based-authorization-vb/_static/image44.png)](role-based-authorization-vb/_static/image43.png)

**Figura 15**: los botones editar y eliminar solo están disponibles para los administradores ([haga clic para ver la imagen a tamaño completo](role-based-authorization-vb/_static/image45.png))

## <a name="step-3-applying-role-based-authorization-rules-to-classes-and-methods"></a>Paso 3: aplicar reglas de autorización basadas en roles a clases y métodos

En el paso 2, limitamos las capacidades de edición a los usuarios de los roles supervisores y administradores, y eliminamos las capacidades solo para los administradores. Esto se logra ocultando los elementos de la interfaz de usuario asociados a usuarios no autorizados mediante técnicas de programación. Estas medidas no garantizan que un usuario no autorizado no pueda realizar una acción privilegiada. Puede haber elementos de la interfaz de usuario que se agreguen más adelante o que se haya olvidado de ocultar a usuarios no autorizados. O bien, un pirata informático puede detectar otra manera de obtener la página ASP.NET para ejecutar el método deseado.

Una manera sencilla de asegurarse de que un usuario no autorizado no pueda acceder a una parte determinada de la funcionalidad es decorar esa clase o método con el [`PrincipalPermission` atributo](https://msdn.microsoft.com/library/system.security.permissions.principalpermissionattribute.aspx). Cuando el tiempo de ejecución de .NET usa una clase o ejecuta uno de sus métodos, comprueba para asegurarse de que el contexto de seguridad actual tiene permiso. El atributo `PrincipalPermission` proporciona un mecanismo a través del cual se pueden definir estas reglas.

Analizamos el uso del `PrincipalPermission` atributo de nuevo en <a id="_msoanchor_9"> </a>el tutorial de [*autorización basada*](../membership/user-based-authorization-vb.md) en el usuario. En concreto, vimos cómo decorar el `SelectedIndexChanged` de GridView y `RowDeleting` controlador de eventos para que solo los usuarios autenticados y Tito, respectivamente puedan ejecutarlos. El atributo `PrincipalPermission` funciona igual que los roles.

Vamos a mostrar el uso del atributo `PrincipalPermission` en los controladores de eventos `RowUpdating` y `RowDeleting` de GridView para prohibir la ejecución de usuarios no autorizados. Lo único que debemos hacer es agregar el atributo adecuado sobre cada definición de función:

[!code-vb[Main](role-based-authorization-vb/samples/sample13.vb)]

El atributo del controlador de eventos `RowUpdating` determina que solo los usuarios de los roles administradores o supervisores pueden ejecutar el controlador de eventos, donde el atributo del controlador de eventos `RowDeleting` limita la ejecución a los usuarios del rol administradores.

> [!NOTE]
> El atributo `PrincipalPermission` se representa como una clase en el espacio de nombres `System.Security.Permissions`. Asegúrese de agregar una instrucción `Imports System.Security.Permissions` en la parte superior del archivo de clase de código subyacente para importar este espacio de nombres.

Si, de algún modo, un usuario no administrador intenta ejecutar el controlador de eventos `RowDeleting` o si un usuario que no es supervisor o no administrador intenta ejecutar el controlador de eventos `RowUpdating`, el tiempo de ejecución de .NET generará una `SecurityException`.

[![si el contexto de seguridad no está autorizado para ejecutar el método, se produce una excepción SecurityException](role-based-authorization-vb/_static/image47.png)](role-based-authorization-vb/_static/image46.png)

**Figura 16**: Si el contexto de seguridad no está autorizado para ejecutar el método, se produce una `SecurityException` ([haga clic para ver la imagen de tamaño completo](role-based-authorization-vb/_static/image48.png))

Además de las páginas de ASP.NET, muchas aplicaciones también tienen una arquitectura que incluye varias capas, como las capas de lógica de negocios y de acceso a datos. Estas capas se implementan normalmente como bibliotecas de clases y ofrecen clases y métodos para realizar la lógica empresarial y la funcionalidad relacionada con los datos. El atributo `PrincipalPermission` es útil para aplicar reglas de autorización a estas capas también.

Para obtener más información sobre el uso del atributo `PrincipalPermission` para definir reglas de autorización en clases y métodos, consulte la entrada de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/)sobre cómo [agregar reglas de autorización a las capas de datos y empresariales mediante `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx).

## <a name="summary"></a>Resumen

En este tutorial, hemos examinado cómo especificar reglas de autorización generales y granulares basadas en los roles del usuario. ASP. La característica de autorización de URL de la red permite a un desarrollador de páginas especificar las identidades a las que se les permite o deniega el acceso a las páginas. Como hemos visto en el tutorial <a id="_msoanchor_10"> </a>de [*autorización basada*](../membership/user-based-authorization-vb.md) en el usuario, las reglas de autorización de URL se pueden aplicar de forma individual para cada usuario. También se pueden aplicar roles por rol, como vimos en el paso 1 de este tutorial.

Las reglas de autorización de grano fino se pueden aplicar de forma declarativa o mediante programación. En el paso 2, analizamos el uso de la característica RoleGroups del control LoginView para representar una salida diferente en función de los roles del usuario visitante. También hemos analizado las maneras de determinar mediante programación si un usuario pertenece a un rol específico y cómo ajustar la funcionalidad de la página en consecuencia.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Adición de reglas de autorización a las capas de negocio y de datos con `PrincipalPermissionAttributes`](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)
- [Examen de la pertenencia, los roles y el perfil de ASP.NET 2.0: trabajar con roles](http://aspnet.4guysfromrolla.com/articles/121405-1.aspx)
- [Lista de preguntas de seguridad para ASP.NET 2,0](https://msdn.microsoft.com/library/ms998375.aspx)
- [Documentación técnica del elemento `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial...

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial incluyen Banerjee y Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](assigning-roles-to-users-vb.md)
