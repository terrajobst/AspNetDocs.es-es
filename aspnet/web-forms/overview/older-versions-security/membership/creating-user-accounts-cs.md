---
uid: web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
title: Creación de cuentas deC#usuario () | Microsoft Docs
author: rick-anderson
description: En este tutorial, exploraremos el uso del marco de pertenencia (a través de SqlMembershipProvider) para crear nuevas cuentas de usuario. Veremos cómo crear una nueva...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: f175278c-6079-4d91-b9b4-2493ed43d9ec
msc.legacyurl: /web-forms/overview/older-versions-security/membership/creating-user-accounts-cs
msc.type: authoredcontent
ms.openlocfilehash: 955592320e7d36c7ae3b9c03a361bee2183f1776
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74625478"
---
# <a name="creating-user-accounts-c"></a>Crear cuentas de usuario (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_05_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial05_CreatingUsers_cs.pdf)

> En este tutorial, exploraremos el uso del marco de pertenencia (a través de SqlMembershipProvider) para crear nuevas cuentas de usuario. Veremos cómo crear nuevos usuarios mediante programación y a través de ASP. Control CreateUserWizard integrado de la red.

## <a name="introduction"></a>Introducción

En el <a id="_msoanchor_1"> </a> [tutorial anterior](creating-the-membership-schema-in-sql-server-cs.md) se instaló el esquema de servicios de aplicación en una base de datos, que agrega las tablas, vistas y procedimientos almacenados necesarios para el `SqlMembershipProvider` y `SqlRoleProvider`. Esto creó la infraestructura que se necesitará para el resto de los tutoriales de esta serie. En este tutorial, exploraremos el uso del marco de pertenencia (a través de la `SqlMembershipProvider`) para crear nuevas cuentas de usuario. Veremos cómo crear nuevos usuarios mediante programación y a través de ASP. Control CreateUserWizard integrado de la red.

Además de aprender a crear nuevas cuentas de usuario, también tendrá que actualizar el sitio web de demostración que creamos primero en la *<a id="_msoanchor_2"></a>[Introducción al tutorial de autenticación de formularios](../introduction/an-overview-of-forms-authentication-cs.md)* y, a continuación, mejorado en el tutorial configuración de la *<a id="https://www.asp.net/learn/security/tutorial-03-cs.aspx"></a>autenticación de formularios y temas avanzados*. Nuestra aplicación Web de demostración tiene una página de inicio de sesión que valida las credenciales de los usuarios con los pares de nombre de usuario y contraseña codificados de forma rígida. Además, `Global.asax` incluye código que crea objetos personalizados `IPrincipal` y `IIdentity` para los usuarios autenticados. Actualizaremos la página de inicio de sesión para validar las credenciales de los usuarios en el marco de pertenencia y quitar la lógica de identidad y entidad de seguridad personalizada.

Comencemos.

## <a name="the-forms-authentication-and-membership-checklist"></a>La lista de comprobación de pertenencia y autenticación de formularios

Antes de empezar a trabajar con el marco de pertenencia, dedique un momento a revisar los pasos importantes que hemos tomado para llegar a este punto. Al usar el marco de pertenencia con el `SqlMembershipProvider` en un escenario de autenticación basada en formularios, es necesario realizar los siguientes pasos antes de implementar la funcionalidad de pertenencia en la aplicación web:

1. **Habilitar la autenticación basada en formularios.** Como se explicó en *<a id="_msoanchor_4"></a>[Información general sobre la autenticación de formularios](../introduction/an-overview-of-forms-authentication-cs.md)* , la autenticación mediante formularios se habilita editando `Web.config` y estableciendo el atributo `mode` del elemento `<authentication>` en `Forms`. Con la autenticación de formularios habilitada, cada solicitud entrante se examina para un *vale de autenticación de formularios*, que, si está presente, identifica al solicitante.
2. **Agregue el esquema de servicios de aplicación a la base de datos adecuada.** Al usar el `SqlMembershipProvider` es necesario instalar el esquema de servicios de aplicación en una base de datos. Normalmente, este esquema se agrega a la misma base de datos que contiene el modelo de datos de la aplicación. En el *<a id="_msoanchor_5"></a>[tutorial creación del esquema de pertenencia en SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* se ha examinado el uso de la herramienta de `aspnet_regsql.exe` para lograrlo.
3. **Personalice la configuración de la aplicación web para hacer referencia a la base de datos del paso 2.** En el tutorial *crear el esquema de pertenencia en SQL Server* se mostraron dos maneras de configurar la aplicación web para que el `SqlMembershipProvider` use la base de datos seleccionada en el paso 2: modificando el nombre de la cadena de conexión de `LocalSqlServer`; o bien agregando un nuevo proveedor registrado a la lista de proveedores de marcos de pertenencia y personalizando ese nuevo proveedor para usar la base de datos del paso 2.

Al compilar una aplicación web que usa la `SqlMembershipProvider` y la autenticación basada en formularios, deberá realizar estos tres pasos antes de usar la clase `Membership` o los controles Web de inicio de sesión ASP.NET. Dado que ya hemos realizado estos pasos en los tutoriales anteriores, estamos preparados para empezar a usar el marco de pertenencia.

## <a name="step-1-adding-new-aspnet-pages"></a>Paso 1: Agregar nuevas páginas de ASP.NET

En este tutorial y en los tres próximos, examinaremos diversas funciones y funcionalidades relacionadas con la pertenencia. Se necesitará una serie de páginas de ASP.NET para implementar los temas examinados en estos tutoriales. Vamos a crear esas páginas y, a continuación, un archivo de mapa del sitio `(Web.sitemap)`.

Empiece por crear una nueva carpeta en el proyecto denominado `Membership`. A continuación, agregue cinco páginas nuevas de ASP.NET a la carpeta `Membership`, vinculando cada página con la página maestra de `Site.master`. Asigne un nombre a las páginas:

- `CreatingUserAccounts.aspx`
- `UserBasedAuthorization.aspx`
- `EnhancedCreateUserWizard.aspx`
- `AdditionalUserInfo.aspx`
- `Guestbook.aspx`

En este momento el Explorador de soluciones del proyecto debe ser similar a la captura de pantalla que se muestra en la figura 1.

[![se han agregado cinco nuevas páginas a la carpeta de pertenencia](creating-user-accounts-cs/_static/image2.png)](creating-user-accounts-cs/_static/image1.png)

**Figura 1**: Se han agregado cinco nuevas páginas a la carpeta `Membership` ([haga clic para ver la imagen a tamaño completo](creating-user-accounts-cs/_static/image3.png))

En este punto, cada página debe tener los dos controles de contenido, uno para cada una de las ContentPlaceHolders: `MainContent` y `LoginContent`de la página maestra.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample1.aspx)]

Recuerde que el marcado predeterminado de `LoginContent` ContentPlaceHolder muestra un vínculo para iniciar sesión o cerrar sesión en el sitio, en función de si el usuario está autenticado. La presencia del control de contenido de `Content2`, sin embargo, invalida el marcado predeterminado de la página maestra. Como se explicó en *<a id="_msoanchor_6"></a>[Información general sobre el tutorial de autenticación de formularios](../introduction/an-overview-of-forms-authentication-cs.md)* , esto resulta útil en las páginas en las que no queremos mostrar opciones relacionadas con el inicio de sesión en la columna izquierda.

Sin embargo, para estas cinco páginas, queremos mostrar el marcado predeterminado de la página maestra para el `LoginContent` ContentPlaceHolder. Por lo tanto, quite el marcado declarativo para el control de contenido `Content2`. Después de hacerlo, cada una de las marcas de cinco páginas debe contener solo un control de contenido.

## <a name="step-2-creating-the-site-map"></a>Paso 2: Crear el mapa del sitio

Todos los sitios web, salvo los más triviales, deben implementar alguna forma de interfaz de usuario de navegación. La interfaz de usuario de navegación puede ser una lista sencilla de vínculos a las distintas secciones del sitio. Como alternativa, estos vínculos se pueden organizar en menús o vistas de árbol. Como desarrollador de páginas, la creación de la interfaz de usuario de navegación es solo la mitad de la historia. También necesitamos algunos medios para definir la estructura lógica del sitio de manera fácil de mantener y actualizable. A medida que se agregan nuevas páginas o se quitan páginas existentes, queremos poder actualizar un solo origen: el mapa del sitio y hacer que las modificaciones se reflejen en la interfaz de usuario de navegación del sitio.

Estas dos tareas, que definen el mapa del sitio e implementan una interfaz de usuario de navegación basada en el mapa del sitio, son fáciles de lograr gracias al marco del mapa del sitio y a los controles Web de navegación agregados en la versión 2,0 de ASP.NET. El marco de trabajo del mapa del sitio permite a un desarrollador definir un mapa del sitio y, a continuación, acceder a él a través de una API de programación (la [clase`SiteMap`](https://msdn.microsoft.com/library/system.web.sitemap.aspx)). Los controles Web de navegación integrados incluyen un [control de menú](https://msdn.microsoft.com/library/bz09dy46.aspx), el [control TreeView](https://msdn.microsoft.com/library/3eafky27.aspx)y el [control SiteMapPath](https://msdn.microsoft.com/library/3eafky27.aspx).

Al igual que los marcos de pertenencia y roles, el marco del mapa del sitio se crea sobre el [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx). El trabajo de la clase del proveedor del mapa del sitio es generar la estructura en memoria utilizada por la clase `SiteMap` desde un almacén de datos persistente, como un archivo XML o una tabla de base de datos. El .NET Framework se suministra con un proveedor del mapa del sitio predeterminado que lee los datos del mapa del sitio desde un archivo XML ([`XmlSiteMapProvider`](https://msdn.microsoft.com/library/system.web.xmlsitemapprovider.aspx)) y este es el proveedor que se va a usar en este tutorial. Para algunas implementaciones alternativas del proveedor del mapa del sitio, consulte la sección lecturas adicionales al final de este tutorial.

El proveedor del mapa del sitio predeterminado espera que un archivo XML con el formato correcto denominado `Web.sitemap` exista en el directorio raíz. Como usamos este proveedor predeterminado, es necesario agregar este tipo de archivo y definir la estructura del mapa del sitio en el formato XML adecuado. Para agregar el archivo, haga clic con el botón derecho en el nombre del proyecto en Explorador de soluciones y elija Agregar nuevo elemento. En el cuadro de diálogo, opte por agregar un archivo de tipo mapa del sitio denominado `Web.sitemap`.

[![agregar un archivo denominado Web. sitemap al directorio raíz del proyecto](creating-user-accounts-cs/_static/image5.png)](creating-user-accounts-cs/_static/image4.png)

**Figura 2**: Agregue un archivo denominado `Web.sitemap` al directorio raíz del proyecto ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image6.png))

El archivo de mapa del sitio XML define la estructura del sitio web como una jerarquía. Esta relación jerárquica se modela en el archivo XML a través de la ascendencia de los elementos `<siteMapNode>`. El `Web.sitemap` debe comenzar con un `<siteMap>` nodo primario que tenga exactamente un `<siteMapNode>` secundario. Este elemento `<siteMapNode>` de nivel superior representa la raíz de la jerarquía y puede tener un número arbitrario de nodos descendientes. Cada `<siteMapNode>` elemento debe incluir un atributo `title` y, opcionalmente, incluir `url` y `description` atributos, entre otros; cada atributo de `url` que no esté vacío debe ser único.

Escriba el siguiente código XML en el archivo de `Web.sitemap`:

[!code-xml[Main](creating-user-accounts-cs/samples/sample2.xml)]

El marcado del mapa del sitio anterior define la jerarquía que se muestra en la figura 3.

[![el mapa del sitio representa una estructura jerárquica de navegación](creating-user-accounts-cs/_static/image8.png)](creating-user-accounts-cs/_static/image7.png)

**Figura 3**: El mapa del sitio representa una estructura jerárquica de navegación ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image9.png))

## <a name="step-3-updating-the-master-page-to-include-a-navigational-user-interface"></a>Paso 3: Actualización de la página maestra para incluir una interfaz de usuario de navegación

ASP.NET incluye una serie de controles Web relacionados con la navegación para diseñar una interfaz de usuario. Entre ellos se incluyen el menú, la vista de árbol y los controles SiteMapPath. Los controles de menú y TreeView representan la estructura del mapa del sitio en un menú o árbol, respectivamente, mientras que el control SiteMapPath muestra una ruta de navegación que muestra el nodo actual que se está visitando y sus antecesores. Los datos del mapa del sitio se pueden enlazar a otros controles Web de datos mediante SiteMapDataSource y se puede tener acceso a ellos mediante programación a través de la clase `SiteMap`.

Dado que una explicación detallada del marco del mapa del sitio y de los controles de navegación está fuera del ámbito de esta serie de tutoriales, en lugar de dedicar tiempo a crear nuestra propia interfaz de usuario de navegación, en su lugar, se utiliza un control Repeater para mostrar una lista con varias viñetas de vínculos de navegación, como se muestra en *[2,0](../../data-access/index.md)* la figura 4.

### <a name="adding-a-two-level-list-of-links-in-the-left-column"></a>Agregar una lista de dos niveles de vínculos en la columna izquierda

Para crear esta interfaz, agregue el siguiente marcado declarativo a la columna izquierda de la página maestra de `Site.master` en la que el texto "TODO: El menú irá aquí... " reside actualmente.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample3.aspx)]

El marcado anterior enlaza un control Repeater denominado `menu` a un SiteMapDataSource, que devuelve la jerarquía del mapa del sitio definida en `Web.sitemap`. Dado que la [propiedad`ShowStartingNode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sitemapdatasource.showstartingnode.aspx) del control SiteMapDataSource está establecida en false, comienza a devolver la jerarquía del mapa del sitio a partir de los descendientes del nodo "Inicio". El repetidor muestra cada uno de estos nodos (actualmente solo "pertenencia") en un elemento `<li>`. Otro, el repetidor interno, después muestra los elementos secundarios del nodo actual en una lista sin ordenar anidada.

En la figura 4 se muestra la salida representada de la marca anterior con la estructura del mapa del sitio que creamos en el paso 2. El repetidor representa el marcado de la lista sin ordenar de vainilla; las reglas de hojas de estilos en cascada definidas en `Styles.css` son responsables del diseño estéticamente agradable. Para obtener una descripción más detallada de cómo funciona el marcado anterior, consulte las [páginas maestras y](https://asp.net/learn/data-access/tutorial-03-cs.aspx) el tutorial de navegación del sitio.

[![la interfaz de usuario de navegación se representa utilizando listas no ordenadas anidadas](creating-user-accounts-cs/_static/image11.png)](creating-user-accounts-cs/_static/image10.png)

**Figura 4**: La interfaz de usuario de navegación se representa mediante listas no ordenadas anidadas ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image12.png))

### <a name="adding-breadcrumb-navigation"></a>Agregar una ruta de navegación

Además de la lista de vínculos de la columna izquierda, cada página también muestra una [ruta de navegación](http://en.wikipedia.org/wiki/Breadcrumb_%28navigation%29). Una ruta de navegación es un elemento de la interfaz de usuario de navegación que muestra rápidamente a los usuarios su posición actual dentro de la jerarquía del sitio. El control SiteMapPath usa el marco del mapa del sitio para determinar la ubicación de la página actual en el mapa del sitio y, a continuación, muestra una ruta de navegación basada en esta información.

En concreto, agregue un elemento `<span>` al encabezado `<div>` elemento de la página maestra y establezca el atributo `class` del nuevo elemento `<span>` en "ruta de navegación". (La clase `Styles.css` contiene una regla para una clase de "ruta de navegación"). A continuación, agregue un elemento SiteMapPath a este nuevo elemento `<span>`.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample4.aspx)]

En la figura 5 se muestra la salida de SiteMapPath al visitar `~/Membership/CreatingUserAccounts.aspx`.

[![la ruta de navegación muestra la página actual y sus antecesores en el mapa del sitio](creating-user-accounts-cs/_static/image14.png)](creating-user-accounts-cs/_static/image13.png)

**Figura 5**: La ruta de navegación muestra la página actual y sus antecesores en el mapa del sitio ([haga clic para ver la imagen a tamaño completo](creating-user-accounts-cs/_static/image15.png))

## <a name="step-4-removing-the-custom-principal-and-identity-logic"></a>Paso 4: Quitar la lógica de identidad y entidad de seguridad personalizada

En el tutorial configuración de la *<a id="_msoanchor_7"></a>[autenticación de formularios y temas avanzados](../introduction/forms-authentication-configuration-and-advanced-topics-cs.md)* , vimos cómo asociar objetos principales y de identidad personalizados al usuario autenticado. Para ello, se crea un controlador de eventos en `Global.asax` para el evento de `PostAuthenticateRequest` de la aplicación, que se desencadena después de que el `FormsAuthenticationModule` haya autenticado al usuario. En este controlador de eventos se reemplazaron los objetos `GenericPrincipal` y `FormsIdentity` agregados por el `FormsAuthenticationModule` con los objetos `CustomPrincipal` y `CustomIdentity` creados en ese tutorial.

Aunque los objetos principales y de identidad personalizados son útiles en ciertos escenarios, en la mayoría de los casos los objetos `GenericPrincipal` y `FormsIdentity` son suficientes. Por lo tanto, creo que sería rentable volver al comportamiento predeterminado. Realice este cambio quitando o marcando como comentario el controlador de eventos `PostAuthenticateRequest` o eliminando el archivo de `Global.asax` por completo.

## <a name="step-5-programmatically-creating-a-new-user"></a>Paso 5: Crear un nuevo usuario mediante programación

Para crear una nueva cuenta de usuario a través del marco de pertenencia, use el [método`CreateUser`](https://msdn.microsoft.com/library/system.web.security.membership.createuser.aspx)de la clase `Membership`. Este método tiene parámetros de entrada para el nombre de usuario, la contraseña y otros campos relacionados con el usuario. Al invocar, delega la creación de la nueva cuenta de usuario en el proveedor de pertenencia configurado y, a continuación, devuelve un [objeto`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx) que representa la cuenta de usuario que se acaba de crear.

El método `CreateUser` tiene cuatro sobrecargas, cada una de las cuales acepta un número diferente de parámetros de entrada:

- [`CreateUser(username, password)`](https://msdn.microsoft.com/library/d8t4h2es.aspx)
- [`CreateUser(username, password, email)`](https://msdn.microsoft.com/library/t8yy6w3h.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, MembershipCreateStatus)`](https://msdn.microsoft.com/library/82xx2e62.aspx)
- [`CreateUser(username, password, email, passwordQuestion, passwordAnswer, isApproved, providerUserKey, MembershipCreateStatus)`](https://msdn.microsoft.com/library/ms152012.aspx)

Estas cuatro sobrecargas se diferencian en la cantidad de información que se recopila. La primera sobrecarga, por ejemplo, solo requiere el nombre de usuario y la contraseña de la nueva cuenta de usuario, mientras que la segunda también requiere la dirección de correo electrónico del usuario.

Estas sobrecargas existen porque la información necesaria para crear una nueva cuenta de usuario depende de las opciones de configuración del proveedor de pertenencia. En el *<a id="_msoanchor_8"></a>[Tutorial crear el esquema de pertenencia en SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* examinamos especificando los valores de configuración del proveedor de pertenencia en `Web.config`. En la tabla 2 se incluye una lista completa de las opciones de configuración.

Una de estas opciones de configuración del proveedor de pertenencia que afecta a lo que se pueden usar sobrecargas de `CreateUser` es el valor de `requiresQuestionAndAnswer`. Si `requiresQuestionAndAnswer` se establece en `true` (valor predeterminado), al crear una nueva cuenta de usuario se debe especificar una pregunta y respuesta de seguridad. Esta información se utiliza posteriormente si el usuario necesita restablecer o cambiar su contraseña. En concreto, en ese momento se muestra la pregunta de seguridad y deben escribir la respuesta correcta para restablecer o cambiar su contraseña. Por lo tanto, si la `requiresQuestionAndAnswer` se establece en `true` después de llamar a cualquiera de las dos primeras sobrecargas de `CreateUser` se produce una excepción porque falta la pregunta y respuesta de seguridad. Dado que nuestra aplicación está configurada actualmente para requerir una pregunta y respuesta de seguridad, tendremos que usar una de las dos últimas sobrecargas al crear el usuario mediante programación.

Para ilustrar el uso del método `CreateUser`, vamos a crear una interfaz de usuario en la que pedimos al usuario su nombre, contraseña, correo electrónico y una respuesta a una pregunta de seguridad predefinida. Abra la página `CreatingUserAccounts.aspx` en la carpeta `Membership` y agregue los siguientes controles Web al control de contenido:

- Un cuadro de texto denominado `Username`
- Un cuadro de texto denominado `Password`, cuya propiedad `TextMode` está establecida en `Password`
- Un cuadro de texto denominado `Email`
- Una etiqueta llamada `SecurityQuestion` con su propiedad `Text` desactivada
- Un cuadro de texto denominado `SecurityAnswer`
- Un botón denominado `CreateAccountButton` cuya propiedad Text está establecida en "crear la cuenta de usuario"
- Un control etiqueta denominado `CreateAccountResults` con su propiedad `Text` desactivada

En este punto, la pantalla debe ser similar a la captura de pantalla que se muestra en la figura 6.

[![agregar los distintos controles Web a la página CreatingUserAccounts. aspx](creating-user-accounts-cs/_static/image17.png)](creating-user-accounts-cs/_static/image16.png)

**Ilustración 6**: Agregue los distintos controles Web a la página `CreatingUserAccounts.aspx` ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image18.png))

Los `SecurityQuestion` etiqueta y `SecurityAnswer` cuadro de texto están diseñados para mostrar una pregunta de seguridad predefinida y recopilar la respuesta del usuario. Tenga en cuenta que la pregunta y la respuesta de seguridad se almacenan de forma individual para cada usuario, por lo que es posible permitir que cada usuario defina su propia pregunta de seguridad. Sin embargo, en este ejemplo he decidido usar una pregunta de seguridad universal, a saber: "¿Cuál es su color favorito?"

Para implementar esta pregunta de seguridad predefinida, agregue una constante a la clase de código subyacente de la página denominada `passwordQuestion`, asignando la pregunta de seguridad. A continuación, en el controlador de eventos `Page_Load`, asigne esta constante a la propiedad `Text` de la etiqueta de `SecurityQuestion`:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample5.cs)]

A continuación, cree un controlador de eventos para el evento de `Click` del `CreateAccountButton`y agregue el código siguiente:

[!code-csharp[Main](creating-user-accounts-cs/samples/sample6.cs)]

El controlador de eventos `Click` se inicia definiendo una variable denominada `createStatus` de tipo [`MembershipCreateStatus`](https://msdn.microsoft.com/library/system.web.security.membershipcreatestatus.aspx). `MembershipCreateStatus` es una enumeración que indica el estado de la operación de `CreateUser`. Por ejemplo, si la cuenta de usuario se crea correctamente, la instancia de `MembershipCreateStatus` resultante se establecerá en un valor de `Success`; por otro lado, si se produce un error en la operación porque ya existe un usuario con el mismo nombre de usuario, se establecerá en un valor de `DuplicateUserName`. En la `CreateUser` sobrecarga que usamos, es necesario pasar una instancia de `MembershipCreateStatus` al método como un parámetro `out`. Este parámetro se establece en el valor adecuado en el método `CreateUser` y se puede examinar su valor después de la llamada al método para determinar si la cuenta de usuario se ha creado correctamente.

Después de llamar a `CreateUser`, pasando `createStatus`, se usa una instrucción `switch` para generar un mensaje apropiado según el valor asignado a `createStatus`. En las figuras 7 se muestra el resultado cuando se ha creado un nuevo usuario correctamente. En las figuras 8 y 9 se muestra el resultado cuando no se crea la cuenta de usuario. En la figura 8, el visitante escribió una contraseña de cinco letras, que no cumple los requisitos de seguridad de la contraseña escritos en los valores de configuración del proveedor de pertenencia. En la figura 9, el visitante intenta crear una cuenta de usuario con un nombre de usuario existente (el que se creó en la figura 7).

[![se creó correctamente una nueva cuenta de usuario](creating-user-accounts-cs/_static/image20.png)](creating-user-accounts-cs/_static/image19.png)

**Figura 7**: Se creó correctamente una nueva cuenta de usuario ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image21.png))

[![no se creó la cuenta de usuario porque la contraseña proporcionada es demasiado débil](creating-user-accounts-cs/_static/image23.png)](creating-user-accounts-cs/_static/image22.png)

**Figura 8**: No se creó la cuenta de usuario porque la contraseña proporcionada es demasiado débil ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image24.png))

[![no se creó la cuenta de usuario porque el nombre de usuario ya está en uso](creating-user-accounts-cs/_static/image26.png)](creating-user-accounts-cs/_static/image25.png)

**Ilustración 9**: No se creó la cuenta de usuario porque el nombre de usuario ya está en uso ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image27.png))

> [!NOTE]
> Es posible que se pregunte cómo determinar si se ha realizado correctamente o no cuando se usa una de las dos primeras sobrecargas del método `CreateUser`, ninguno de los cuales tiene un parámetro de tipo `MembershipCreateStatus`. Estas dos primeras sobrecargas inician una [excepción`MembershipCreateUserException`](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.aspx) ante un error, que incluye una [propiedad`StatusCode`](https://msdn.microsoft.com/library/system.web.security.membershipcreateuserexception.statuscode.aspx) de tipo `MembershipCreateStatus`.

Después de crear algunas cuentas de usuario, compruebe que las cuentas se han creado enumerando el contenido de las tablas `aspnet_Users` y `aspnet_Membership` en la base de datos `SecurityTutorials.mdf`. Como se muestra en la figura 10, he agregado dos usuarios a través de la página `CreatingUserAccounts.aspx`: Tito y Bruce.

[![hay dos usuarios en el almacén de usuarios de pertenencia: Tito y Bruce](creating-user-accounts-cs/_static/image29.png)](creating-user-accounts-cs/_static/image28.png)

**Figura 10**: Hay dos usuarios en el almacén de usuarios de pertenencia: Tito y Bruce ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image30.png))

Aunque el almacén de usuarios de pertenencia ahora incluye la información de la cuenta de Bruce y Tito, todavía hemos implementado la funcionalidad que permite que Bruce o Tito inicien sesión en el sitio. Actualmente, `Login.aspx` valida las credenciales del usuario con un conjunto codificado de pares de nombre de usuario/contraseña, *no* valida las credenciales proporcionadas en el marco de pertenencia. Por ahora, ver las nuevas cuentas de usuario en el `aspnet_Users` y `aspnet_Membership` tablas tendrán que ser suficientes. En el siguiente tutorial, *<a id="_msoanchor_9"></a>[Validación de las credenciales de usuario en el almacén de usuarios de pertenencia](validating-user-credentials-against-the-membership-user-store-cs.md)* , actualizaremos la página de inicio de sesión para validar con el almacén de pertenencia.

> [!NOTE]
> Si no ve ningún usuario en la base de datos de `SecurityTutorials.mdf`, puede deberse a que la aplicación Web usa el proveedor de pertenencia predeterminado, `AspNetSqlMembershipProvider`, que usa la base de datos de `ASPNETDB.mdf` como su almacén de usuario. Para determinar si este es el problema, haga clic en el botón actualizar en el Explorador de soluciones. Si se ha agregado una base de datos denominada `ASPNETDB.mdf` a la carpeta `App_Data`, este es el problema. Vuelva al paso 4 del tutorial *<a id="_msoanchor_10"></a>[crear el esquema de pertenencia en SQL Server](creating-the-membership-schema-in-sql-server-cs.md)* para obtener instrucciones sobre cómo configurar correctamente el proveedor de pertenencia.

En la mayoría de los escenarios de creación de cuentas de usuario, al visitante se le presenta alguna interfaz para especificar su nombre de usuario, contraseña, correo electrónico y otra información esencial, momento en el que se crea una nueva cuenta. En este paso, analizamos la creación de una interfaz de este tipo a mano y, a continuación, vimos cómo usar el método `Membership.CreateUser` para agregar mediante programación la nueva cuenta de usuario basada en las entradas del usuario. Sin embargo, nuestro código acaba de crear la nueva cuenta de usuario. No realizaba ninguna acción de seguimiento, como el inicio de sesión del usuario en el sitio en la cuenta de usuario recién creada o el envío de un correo electrónico de confirmación al usuario. Estos pasos adicionales requerirían código adicional en el controlador de eventos `Click` del botón.

ASP.NET se incluye con el control CreateUserWizard, que está diseñado para controlar el proceso de creación de cuentas de usuario, desde la representación de una interfaz de usuario para crear nuevas cuentas de usuario, hasta la creación de la cuenta en el marco de pertenencia y la realización de la cuenta posterior. tareas de creación, como el envío de un correo electrónico de confirmación y el inicio de sesión del usuario que acaba de crear en el sitio. El uso del control CreateUserWizard es tan sencillo como arrastrar el control CreateUserWizard desde el cuadro de herramientas a una página y, a continuación, establecer algunas propiedades. En la mayoría de los casos, no tendrá que escribir una sola línea de código. Analizaremos este ingenioso control en el paso 6.

Si las cuentas de usuario nuevas solo se crean mediante una típica página web crear cuenta, es poco probable que tenga que escribir código que use el método `CreateUser`, ya que el control CreateUserWizard probablemente satisfaga sus necesidades. Sin embargo, el método `CreateUser` es útil en escenarios en los que se necesita una experiencia de usuario de creación de cuenta muy personalizada o cuando se necesita crear mediante programación nuevas cuentas de usuario a través de una interfaz alternativa. Por ejemplo, puede tener una página que permita a un usuario cargar un archivo XML que contiene información de usuario de otra aplicación. La página puede analizar el contenido del archivo XML cargado y crear una nueva cuenta para cada usuario representado en el XML llamando al método `CreateUser`.

## <a name="step-6-creating-a-new-user-with-the-createuserwizard-control"></a>Paso 6: Crear un nuevo usuario con el control CreateUserWizard

ASP.NET se suministra con varios controles Web de inicio de sesión. Estos controles ayudan en muchos escenarios comunes relacionados con la cuenta de usuario y el inicio de sesión. El [control CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx) es un control de este tipo diseñado para presentar una interfaz de usuario para agregar una nueva cuenta de usuario al marco de pertenencia.

Al igual que muchos de los demás controles Web relacionados con el inicio de sesión, se puede usar CreateUserWizard sin escribir una sola línea de código. Proporciona de forma intuitiva una interfaz de usuario basada en la configuración del proveedor de pertenencia y llama internamente al método `CreateUser` de la clase `Membership` después de que el usuario escriba la información necesaria y haga clic en el botón "crear usuario". El control CreateUserWizard es sumamente personalizable. Hay un host de eventos que se activan durante varias fases del proceso de creación de la cuenta. Podemos crear controladores de eventos, según sea necesario, para insertar lógica personalizada en el flujo de trabajo de creación de cuentas. Además, la apariencia de CreateUserWizard es muy flexible. Hay una serie de propiedades que definen la apariencia de la interfaz predeterminada; Si es necesario, el control se puede convertir en una plantilla o se puede Agregar "pasos" adicionales de registro del usuario.

Comencemos con un vistazo al uso de la interfaz y el comportamiento predeterminados del control CreateUserWizard. A continuación, exploraremos cómo personalizar la apariencia a través de las propiedades y eventos del control.

### <a name="examining-the-createuserwizards-default-interface-and-behavior"></a>Examinar la interfaz y el comportamiento predeterminados de CreateUserWizard

Vuelva a la página `CreatingUserAccounts.aspx` de la carpeta `Membership`, cambie al modo de diseño o de división y, a continuación, agregue un control CreateUserWizard a la parte superior de la página. El control CreateUserWizard se archiva en la sección de controles de inicio de sesión del cuadro de herramientas. Después de agregar el control, establezca su propiedad `ID` en `RegisterUser`. Como muestra la captura de pantalla de la figura 11, el control CreateUserWizard representa una interfaz con cuadros de texto para el nombre de usuario, la contraseña, la dirección de correo electrónico y la pregunta y respuesta de seguridad.

[![el control CreateUserWizard representa una interfaz de usuario de creación genérica](creating-user-accounts-cs/_static/image32.png)](creating-user-accounts-cs/_static/image31.png)

**Figura 11**: El control CreateUserWizard representa una interfaz de usuario de creación genérica ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image33.png))

Dedique un momento a comparar la interfaz de usuario predeterminada generada por el control CreateUserWizard con la interfaz que creamos en el paso 5. En el caso de los iniciadores, el control CreateUserWizard permite al visitante especificar la pregunta y la respuesta de seguridad, mientras que nuestra interfaz creada manualmente usó una pregunta de seguridad predefinida. La interfaz del control CreateUserWizard también incluye controles de validación, mientras que todavía hemos implementado la validación en los campos de formulario de la interfaz. Y la interfaz del control CreateUserWizard incluye un cuadro de texto "Confirmar contraseña" (junto con un control CompareValidator para asegurarse de que el texto escrito en los cuadros de texto "contraseña" y "comparar contraseña" son iguales).

Lo interesante es que el control CreateUserWizard consulta las opciones de configuración del proveedor de pertenencia al representar su interfaz de usuario. Por ejemplo, los cuadros de texto pregunta y respuesta de seguridad solo se muestran si `requiresQuestionAndAnswer` está establecido en true. Del mismo modo, CreateUserWizard agrega automáticamente un control RegularExpressionValidator para asegurarse de que se cumplen los requisitos de seguridad de la contraseña y establece sus propiedades `ErrorMessage` y `ValidationExpression` basadas en los valores de configuración `minRequiredPasswordLength`, `minRequiredNonalphanumericCharacters`y `passwordStrengthRegularExpression`.

El control CreateUserWizard, como su nombre implica, se deriva del [control del asistente](https://msdn.microsoft.com/library/s2etd1ek.aspx). Los controles de asistente están diseñados para proporcionar una interfaz para completar las tareas de varios pasos. Un control de asistente puede tener un número arbitrario de `WizardSteps`, cada una de las cuales es una plantilla que define los controles HTML y web para ese paso. El control del asistente muestra inicialmente el primer `WizardStep`, junto con los controles de navegación que permiten al usuario continuar desde un paso hasta el siguiente, o volver a los pasos anteriores.

Como se muestra en el marcado declarativo de la figura 11, la interfaz predeterminada del control CreateUserWizard incluye dos `WizardSteps:`

- [`CreateUserWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizardstep.aspx) : representa la interfaz para recopilar información para crear la nueva cuenta de usuario. Este es el paso que se muestra en la figura 11.
- [`CompleteWizardStep`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.completewizardstep.aspx) : representa un mensaje que indica que la cuenta se ha creado correctamente.

La apariencia y el comportamiento de CreateUserWizard se pueden modificar convirtiendo cualquiera de estos pasos en plantillas o agregando su propia `WizardSteps`. Veremos cómo agregar una `WizardStep` a la interfaz de registro en el tutorial *almacenamiento de información de usuario adicional* .

Vamos a ver el control CreateUserWizard en acción. Visite la página `CreatingUserAccounts.aspx` a través de un explorador. Comience escribiendo algunos valores no válidos en la interfaz de CreateUserWizard. Intente escribir una contraseña que no se ajuste a los requisitos de seguridad de la contraseña o dejar vacío el cuadro de texto "nombre de usuario". CreateUserWizard mostrará un mensaje de error adecuado. En la ilustración 12 se muestra el resultado cuando se intenta crear un usuario con una contraseña insuficientemente segura.

[![CreateUserWizard inserta automáticamente controles de validación](creating-user-accounts-cs/_static/image35.png)](creating-user-accounts-cs/_static/image34.png)

**Figura 12**: CreateUserWizard inserta automáticamente controles de validación ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image36.png))

A continuación, escriba los valores adecuados en CreateUserWizard y haga clic en el botón "crear usuario". Suponiendo que se han especificado los campos obligatorios y que la seguridad de la contraseña es suficiente, CreateUserWizard creará una nueva cuenta de usuario a través del marco de pertenencia y, a continuación, mostrará la interfaz del `CompleteWizardStep`(consulte la figura 13). En segundo plano, CreateUserWizard llama al método `Membership.CreateUser`, al igual que hicimos en el paso 5.

[![se ha creado correctamente una nueva cuenta de usuario](creating-user-accounts-cs/_static/image38.png)](creating-user-accounts-cs/_static/image37.png)

**Figura 13**: Se ha creado correctamente una nueva cuenta de usuario ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image39.png))

> [!NOTE]
> Como se muestra en la figura 13, la interfaz del `CompleteWizardStep`incluye un botón continuar. Sin embargo, en este punto, al hacer clic en él solo se realiza un postback, lo que sale del visitante de la misma página. En la sección "personalización de la apariencia y el comportamiento de CreateUserWizard a través de sus propiedades", veremos cómo puede hacer que este botón envíe el visitante a `Default.aspx` (o a alguna otra página).

Después de crear una nueva cuenta de usuario, vuelva a Visual Studio y examine las tablas `aspnet_Users` y `aspnet_Membership` como hicimos en la figura 10 para comprobar que la cuenta se ha creado correctamente.

### <a name="customizing-the-createuserwizards-behavior-and-appearance-through-its-properties"></a>Personalizar el comportamiento y la apariencia de CreateUserWizard a través de sus propiedades

CreateUserWizard puede personalizarse de varias formas, a través de propiedades, `WizardSteps`y controladores de eventos. En esta sección, veremos cómo personalizar la apariencia del control a través de sus propiedades. en la siguiente sección se examina la extensión del comportamiento del control a través de los controladores de eventos.

Prácticamente todo el texto que se muestra en la interfaz de usuario predeterminada del control CreateUserWizard se puede personalizar a través de su gran cantidad de propiedades. Por ejemplo, las propiedades "nombre de usuario", "contraseña", "Confirmar contraseña", "correo electrónico", "pregunta de seguridad" y "respuesta de seguridad" que se muestran a la izquierda de los cuadros de texto se pueden personalizar mediante las propiedades [`UserNameLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.usernamelabeltext.aspx), [`PasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.passwordlabeltext.aspx), [`ConfirmPasswordLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.confirmpasswordlabeltext.aspx), [`EmailLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.emaillabeltext.aspx), [`QuestionLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.questionlabeltext.aspx)y [`AnswerLabelText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.answerlabeltext.aspx) , respectivamente. Del mismo modo, hay propiedades para especificar el texto para los botones "crear usuario" y "continuar" en el `CreateUserWizardStep` y `CompleteWizardStep`, así como si estos botones se representan como botones, LinkButtons o ImageButtons.

Los colores, bordes, fuentes y otros elementos visuales se pueden configurar a través de un host de propiedades de estilo. El propio control CreateUserWizard tiene las propiedades de estilo de control Web comunes: `BackColor`, `BorderStyle`, `CssClass`, `Font`, etc., y hay una serie de propiedades de estilo para definir la apariencia de determinadas secciones de la interfaz de CreateUserWizard. La [propiedad`TextBoxStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.textboxstyle.aspx), por ejemplo, define los estilos para los cuadros de texto en el `CreateUserWizardStep`, mientras que la [propiedad`TitleTextStyle`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.titletextstyle.aspx) define el estilo para el título ("Regístrese para obtener la nueva cuenta").

Además de las propiedades relacionadas con la apariencia, hay una serie de propiedades que afectan al comportamiento del control CreateUserWizard. La [propiedad`DisplayCancelButton`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.displaycancelbutton.aspx), si se establece en true, muestra un botón Cancelar junto al botón "crear usuario" (el valor predeterminado es false). Si se muestra el botón Cancelar, asegúrese de establecer también la [propiedad`CancelDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx), que especifica la página a la que se envía el usuario después de hacer clic en Cancelar. Como se indicó en la sección anterior, el botón continuar de la interfaz del `CompleteWizardStep`produce un postback, pero deja al visitante en la misma página. Para enviar el visitante a otra página después de hacer clic en el botón continuar, solo tiene que especificar la dirección URL en la [propiedad`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx).

Vamos a actualizar el control CreateUserWizard `RegisterUser` para mostrar un botón Cancelar y enviar el visitante a `Default.aspx` cuando se haga clic en los botones cancelar o continuar. Para ello, establezca la propiedad `DisplayCancelButton` en true y las propiedades `CancelDestinationPageUrl` y `ContinueDestinationPageUrl` en "~/default.aspx". En la figura 14 se muestra el CreateUserWizard actualizado cuando se ve a través de un explorador.

[![el CreateUserWizardStep incluye un botón Cancelar](creating-user-accounts-cs/_static/image41.png)](creating-user-accounts-cs/_static/image40.png)

**Figura 14**: El `CreateUserWizardStep` incluye un botón Cancelar ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image42.png))

Cuando un visitante escribe un nombre de usuario, una contraseña, una dirección de correo electrónico y una pregunta y respuesta de seguridad, y hace clic en "crear usuario", se crea una nueva cuenta de usuario y el visitante inicia sesión como el usuario recién creado. Suponiendo que la persona que visita la página está creando una cuenta nueva para sí misma, es probable que sea el comportamiento deseado. Sin embargo, es posible que desee permitir a los administradores agregar nuevas cuentas de usuario. Al hacerlo, se creará la cuenta de usuario, pero el administrador permanecerá conectado como administrador (y no como cuenta recién creada). Este comportamiento se puede modificar mediante la [propiedad booleana`LoginCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.logincreateduser.aspx).

Las cuentas de usuario del marco de pertenencia contienen una marca aprobada; los usuarios que no estén aprobados no podrán iniciar sesión en el sitio. De forma predeterminada, una cuenta recién creada se marca como aprobada, lo que permite al usuario iniciar sesión en el sitio inmediatamente. No obstante, es posible que las cuentas de usuario nuevas estén marcadas como no aprobadas. Quizás quiera que un administrador apruebe manualmente los nuevos usuarios antes de que puedan iniciar sesión. o quizás quiera comprobar que la dirección de correo electrónico especificada en la suscripción es válida antes de permitir que un usuario inicie sesión. Sea cual sea el caso, puede hacer que la cuenta de usuario recién creada esté marcada como no aprobada estableciendo la [propiedad`DisableCreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.disablecreateduser.aspx) del control CreateUserWizard en true (el valor predeterminado es false).

Otras propiedades relacionadas con el comportamiento de Note incluyen `AutoGeneratePassword` y `MailDefinition`. Si la [propiedad`AutoGeneratePassword`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.autogeneratepassword.aspx) está establecida en true, el `CreateUserWizardStep` no muestra los cuadros de texto "contraseña" y "Confirmar contraseña". en su lugar, la contraseña del usuario recién creada se genera automáticamente mediante el [método`GeneratePassword`](https://msdn.microsoft.com/library/system.web.security.membership.generatepassword.aspx)de la clase `Membership`. El método `GeneratePassword` crea una contraseña de una longitud especificada y con un número suficiente de caracteres no alfanuméricos para satisfacer los requisitos de seguridad de contraseña configurados.

La [propiedad`MailDefinition`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.maildefinition.aspx) es útil si desea enviar un correo electrónico a la dirección de correo electrónico especificada durante el proceso de creación de la cuenta. La propiedad `MailDefinition` incluye una serie de subpropiedades para definir información sobre el mensaje de correo electrónico construido. Estas subpropiedades incluyen opciones como `Subject`, `Priority`, `IsBodyHtml`, `From`, `CC`y `BodyFileName`. La [propiedad`BodyFileName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.maildefinition.bodyfilename.aspx) apunta a un archivo de texto o HTML que contiene el cuerpo del mensaje de correo electrónico. El cuerpo admite dos marcadores de posición predefinidos: `<%UserName%>` y `<%Password%>`. Estos marcadores de posición, si están presentes en el archivo `BodyFileName`, se reemplazarán por el nombre y la contraseña del usuario que se acaba de crear.

> [!NOTE]
> La propiedad `MailDefinition` del control `CreateUserWizard` solo especifica detalles sobre el mensaje de correo electrónico que se envía cuando se crea una nueva cuenta. No incluye detalles sobre cómo se envía realmente el mensaje de correo electrónico (es decir, si se usa un servidor SMTP o un directorio de almacenamiento de correo, cualquier información de autenticación, etc.). Estos detalles de bajo nivel deben definirse en la sección `<system.net>` en `Web.config`. Para obtener más información sobre estas opciones de configuración y sobre el envío de correo electrónico desde ASP.NET 2,0 en general, consulte las [preguntas más frecuentes en SystemNetMail.com](http://www.systemnetmail.com/) y mi artículo, [envío de correo electrónico en ASP.net 2,0](http://aspnet.4guysfromrolla.com/articles/072606-1.aspx).

### <a name="extending-the-createuserwizards-behavior-using-event-handlers"></a>Extender el comportamiento de CreateUserWizard mediante controladores de eventos

El control CreateUserWizard genera una serie de eventos durante su flujo de trabajo. Por ejemplo, después de que un visitante escriba su nombre de usuario, contraseña y otra información pertinente y haga clic en el botón "crear usuario", el control CreateUserWizard genera su [evento`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Si se produce un problema durante el proceso de creación, se desencadena el [evento`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) . sin embargo, si el usuario se crea correctamente, se genera el [evento`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) . Hay eventos de control CreateUserWizard adicionales que se generan, pero estos son los tres más alemanes.

En algunos escenarios, es posible que quieramos pulsar en el flujo de trabajo de CreateUserWizard, que podemos hacer mediante la creación de un controlador de eventos para el evento adecuado. Para ilustrar esto, vamos a mejorar el `RegisterUser` control CreateUserWizard para incluir una validación personalizada en el nombre de usuario y la contraseña. En concreto, vamos a mejorar nuestro CreateUserWizard para que los nombres de usuario no puedan contener espacios iniciales o finales y el nombre de usuario no pueda aparecer en ninguna parte de la contraseña. En Resumen, queremos impedir que alguien cree un nombre de usuario como "Scott", o que tenga una combinación de nombre de usuario/contraseña como "Scott" y "Scott. 1234".

Para ello, crearemos un controlador de eventos para el evento `CreatingUser` con el fin de realizar las comprobaciones de validación adicionales. Si los datos proporcionados no son válidos, es necesario cancelar el proceso de creación. También es necesario agregar un control Web de etiqueta a la página para mostrar un mensaje que explica que el nombre de usuario o la contraseña no son válidos. Comience agregando un control etiqueta debajo del control CreateUserWizard, estableciendo su propiedad `ID` en `InvalidUserNameOrPasswordMessage` y su propiedad `ForeColor` en `Red`. Desactive su propiedad `Text` y establezca sus propiedades `EnableViewState` y `Visible` en false.

[!code-aspx[Main](creating-user-accounts-cs/samples/sample7.aspx)]

A continuación, cree un controlador de eventos para el evento `CreatingUser` del control CreateUserWizard. Para crear un controlador de eventos, seleccione el control en el diseñador y, a continuación, vaya a la ventana Propiedades. Desde allí, haga clic en el icono de rayo y, a continuación, haga doble clic en el evento adecuado para crear el controlador de eventos.

Agregue el código siguiente al controlador de eventos `CreatingUser` :

[!code-csharp[Main](creating-user-accounts-cs/samples/sample8.cs)]

Tenga en cuenta que el nombre de usuario y la contraseña especificados en el control CreateUserWizard están disponibles a través de sus propiedades [`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx) y [`Password`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.password.aspx), respectivamente. Usamos estas propiedades en el controlador de eventos anterior para determinar si el nombre de usuario proporcionado contiene espacios iniciales o finales y si el nombre de usuario se encuentra dentro de la contraseña. Si se cumple alguna de estas condiciones, se muestra un mensaje de error en la etiqueta de `InvalidUserNameOrPasswordMessage` y la propiedad `e.Cancel` del controlador de eventos se establece en `true`. Si `e.Cancel` se establece en `true`, el valor de CreateUserWizard reduce el flujo de trabajo, con lo que se cancela el proceso de creación de la cuenta de usuario.

En la figura 15 se muestra una captura de pantalla de `CreatingUserAccounts.aspx` cuando el usuario escribe un nombre de usuario con espacios iniciales.

[no se permiten los nombres de usuario ![con espacios iniciales o finales.](creating-user-accounts-cs/_static/image44.png)](creating-user-accounts-cs/_static/image43.png)

**Figura 15**: No se permiten los nombres de usuario con espacios iniciales o finales ([haga clic para ver la imagen de tamaño completo](creating-user-accounts-cs/_static/image45.png))

> [!NOTE]
> Veremos un ejemplo del uso del evento `CreatedUser` del control CreateUserWizard en el *<a id="_msoanchor_11"></a>[tutorial sobre el almacenamiento de información de usuario adicional](storing-additional-user-information-cs.md)* .

## <a name="summary"></a>Resumen

El método `CreateUser` de la clase `Membership` crea una nueva cuenta de usuario en el marco de pertenencia. Para ello, delega la llamada al proveedor de pertenencia configurado. En el caso del `SqlMembershipProvider`, el método `CreateUser` agrega un registro a las tablas de base de datos `aspnet_Users` y `aspnet_Membership`.

Aunque se pueden crear nuevas cuentas de usuario mediante programación (como vimos en el paso 5), un enfoque más rápido y sencillo es usar el control CreateUserWizard. Este control representa una interfaz de usuario de varios pasos para recopilar información de usuario y crear un nuevo usuario en el marco de pertenencia. En segundo plano, este control usa el mismo método `Membership.CreateUser` que se ha examinado en el paso 5, pero el control crea la interfaz de usuario, los controles de validación y responde a los errores de creación de cuentas de usuario sin tener que escribir una y después de código.

En este momento, tenemos la funcionalidad para crear nuevas cuentas de usuario. Sin embargo, la página de inicio de sesión todavía se está validando con las credenciales codificadas de forma rígida que hemos especificado de nuevo en el segundo tutorial. En el <a id="_msoanchor_12"> </a> [siguiente tutorial](validating-user-credentials-against-the-membership-user-store-cs.md) , actualizaremos `Login.aspx` para validar las credenciales proporcionadas por el usuario en el marco de pertenencia.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [`CreateUser` documentación técnica](https://msdn.microsoft.com/library/system.web.security.membershipprovider.createuser.aspx)
- [Información general sobre el control CreateUserWizard](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/login/createuserwizard.aspx)
- [Crear un proveedor de mapa del sitio basado en el sistema de archivos](http://aspnet.4guysfromrolla.com/articles/020106-1.aspx)
- [Creación de una interfaz de usuario paso a paso con el control del asistente de ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Examinar la navegación del sitio de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Páginas maestras y navegación del sitio](https://asp.net/learn/data-access/tutorial-03-vb.aspx)
- [El proveedor del mapa del sitio de SQL que ha estado esperando](https://msdn.microsoft.com/msdnmag/issues/06/02/WickedCode/default.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro es *[SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial...

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial era Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](creating-the-membership-schema-in-sql-server-cs.md)
> [Siguiente](validating-user-credentials-against-the-membership-user-store-cs.md)
