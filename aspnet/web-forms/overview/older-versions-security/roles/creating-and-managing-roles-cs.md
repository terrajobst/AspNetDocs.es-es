---
uid: web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
title: Crear y administrar roles (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial se examinan los pasos necesarios para configurar el marco de trabajo de roles. A continuación, crearemos páginas web para crear y eliminar roles.
ms.author: riande
ms.date: 03/24/2008
ms.assetid: 113f10b3-a19a-471b-8ff6-db3c79ce8a91
msc.legacyurl: /web-forms/overview/older-versions-security/roles/creating-and-managing-roles-cs
msc.type: authoredcontent
ms.openlocfilehash: a7883d0b05f2fa5a3fdac887f8c8b39d70418fb3
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74595791"
---
# <a name="creating-and-managing-roles-c"></a>Crear y administrar roles (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/CS.09.zip) o [Descargar PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial09_CreatingRoles_cs.pdf)

> En este tutorial se examinan los pasos necesarios para configurar el marco de trabajo de roles. A continuación, crearemos páginas web para crear y eliminar roles.

## <a name="introduction"></a>Introducción

En el <a id="_msoanchor_1"> </a>tutorial de [*autorización basada*](../membership/user-based-authorization-cs.md) en el usuario, examinamos el uso de la autorización de URL para restringir determinados usuarios de un conjunto de páginas y las técnicas declarativas y de programación exploradas para ajustar la funcionalidad de una página de ASP.net en función del usuario visitante. Sin embargo, conceder permiso para el acceso a las páginas o la funcionalidad para cada usuario puede convertirse en una pesadilla de mantenimiento en escenarios en los que hay muchas cuentas de usuario o cuando los privilegios de los usuarios cambian a menudo. Cada vez que un usuario obtiene o pierde autorización para realizar una tarea determinada, el administrador debe actualizar las reglas de autorización de dirección URL adecuadas, el marcado declarativo y el código.

Normalmente, ayuda a clasificar a los usuarios en grupos o *roles* y, a continuación, aplicar permisos en función de cada rol. Por ejemplo, la mayoría de las aplicaciones web tienen un determinado conjunto de páginas o tareas que solo se reservan para los usuarios administrativos. Con las técnicas aprendidas en el tutorial de *autorización basada* en el usuario, agregaremos las reglas de autorización de dirección URL adecuadas, el marcado declarativo y el código para permitir que las cuentas de usuario especificadas realicen tareas administrativas. Pero si se agregó un nuevo administrador o si un administrador existente necesitaba tener sus derechos de administración revocados, tendríamos que devolver y actualizara los archivos de configuración y las páginas Web. Sin embargo, con roles, podríamos crear un rol denominado administradores y asignar esos usuarios de confianza a la función administradores. A continuación, agregaremos las reglas de autorización de dirección URL adecuadas, el marcado declarativo y el código para permitir que el rol administradores realice las diversas tareas administrativas. Con esta infraestructura implementada, la adición de nuevos administradores al sitio o la eliminación de los existentes es tan sencilla como incluir o quitar al usuario del rol administradores. No es necesario realizar ningún cambio de configuración, marcado declarativo o código.

ASP.NET ofrece un marco de trabajo de roles para definir roles y asociarlos con cuentas de usuario. Con el marco de trabajo de roles, se pueden crear y eliminar roles, agregar o quitar usuarios de un rol, determinar el conjunto de usuarios que pertenecen a un rol determinado e indicar si un usuario pertenece a un rol determinado. Una vez que se ha configurado el marco de trabajo de roles, podemos limitar el acceso a las páginas en función de cada rol a través de reglas de autorización de URL y mostrar u ocultar información adicional o funcionalidad en una página basada en los roles del usuario que ha iniciado sesión actualmente.

En este tutorial se examinan los pasos necesarios para configurar el marco de trabajo de roles. A continuación, crearemos páginas web para crear y eliminar roles. En el <a id="_msoanchor_2"> </a>tutorial [*asignar roles a usuarios*](assigning-roles-to-users-cs.md) , veremos cómo agregar y quitar usuarios de roles. Además, en <a id="_msoanchor_3"> </a>el tutorial de [*autorización basada en roles*](role-based-authorization-cs.md) , veremos cómo limitar el acceso a las páginas en función de cada rol, además de cómo ajustar la funcionalidad de la página en función del rol del usuario que ha visitado. Comencemos.

## <a name="step-1-adding-new-aspnet-pages"></a>Paso 1: agregar nuevas páginas de ASP.NET

En este tutorial y en los dos siguientes, examinaremos varias funciones y funcionalidades relacionadas con los roles. Se necesitará una serie de páginas de ASP.NET para implementar los temas examinados en estos tutoriales. Vamos a crear estas páginas y a actualizar el mapa del sitio.

Empiece por crear una nueva carpeta en el proyecto denominado `Roles`. A continuación, agregue cuatro nuevas páginas de ASP.NET a la carpeta `Roles`, vinculando cada página con la página maestra de `Site.master`. Asigne un nombre a las páginas:

- `ManageRoles.aspx`
- `UsersAndRoles.aspx`
- `CreateUserWizardWithRoles.aspx`
- `RoleBasedAuthorization.aspx`

En este momento el Explorador de soluciones del proyecto debe ser similar a la captura de pantalla que se muestra en la figura 1.

[![se han agregado cuatro nuevas páginas a la carpeta roles](creating-and-managing-roles-cs/_static/image2.png)](creating-and-managing-roles-cs/_static/image1.png)

**Figura 1**: se han agregado cuatro nuevas páginas a la carpeta `Roles` ([haga clic para ver la imagen de tamaño completo](creating-and-managing-roles-cs/_static/image3.png))

En este punto, cada página debe tener los dos controles de contenido, uno para cada una de las ContentPlaceHolders: `MainContent` y `LoginContent`de la página maestra.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample1.aspx)]

Recuerde que el marcado predeterminado de `LoginContent` ContentPlaceHolder muestra un vínculo para iniciar sesión o cerrar sesión en el sitio, en función de si el usuario está autenticado. Sin embargo, la presencia de `Content2` control de contenido en la página ASP.NET invalida el marcado predeterminado de la página maestra. Como se explicó en <a id="_msoanchor_4"> </a> [*información general sobre el tutorial de autenticación de formularios*](../introduction/an-overview-of-forms-authentication-cs.md) , reemplazar el marcado predeterminado es útil en las páginas donde no se desea mostrar las opciones relacionadas con el inicio de sesión en la columna izquierda.

Sin embargo, en estas cuatro páginas queremos mostrar el marcado predeterminado de la página maestra para el `LoginContent` ContentPlaceHolder. Por lo tanto, quite el marcado declarativo para el control de contenido `Content2`. Después de hacerlo, cada una de las cuatro marcas de la página debe contener solo un control de contenido.

Por último, vamos a actualizar el mapa del sitio (`Web.sitemap`) para incluir estas nuevas páginas Web. Agregue el siguiente código XML después de la `<siteMapNode>` agregamos para los tutoriales de pertenencia.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample2.xml)]

Con el mapa del sitio actualizado, visite el sitio a través de un explorador. Como se muestra en la figura 2, la navegación de la izquierda incluye ahora los elementos para los tutoriales de roles.

[![se han agregado cuatro nuevas páginas a la carpeta roles](creating-and-managing-roles-cs/_static/image5.png)](creating-and-managing-roles-cs/_static/image4.png)

**Figura 2**: se han agregado cuatro nuevas páginas a la carpeta `Roles` ([haga clic para ver la imagen de tamaño completo](creating-and-managing-roles-cs/_static/image6.png))

## <a name="step-2-specifying-and-configuring-the-roles-framework-provider"></a>Paso 2: especificar y configurar el proveedor de Framework de roles

Al igual que el marco de pertenencia, el marco de trabajo de roles se crea sobre el modelo de proveedor. Como se describe en <a id="_msoanchor_5"> </a>el tutorial de compatibilidad con los [*conceptos básicos de seguridad y ASP.net*](../introduction/security-basics-and-asp-net-support-cs.md) , el .NET Framework se suministra con tres proveedores de roles integrados: [`AuthorizationStoreRoleProvider`](https://msdn.microsoft.com/library/system.web.security.authorizationstoreroleprovider.aspx), [`WindowsTokenRoleProvider`](https://msdn.microsoft.com/library/system.web.security.windowstokenroleprovider.aspx)y [`SqlRoleProvider`](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx). Esta serie de tutoriales se centra en la `SqlRoleProvider`, que utiliza una base de datos Microsoft SQL Server como almacén de roles.

Debajo de se trata el marco de trabajo de roles y `SqlRoleProvider` funcionan como el marco de pertenencia y el `SqlMembershipProvider`. El .NET Framework contiene una clase `Roles` que actúa como la API del marco de trabajo de roles. La clase `Roles` tiene métodos estáticos como `CreateRole`, `DeleteRole`, `GetAllRoles`, `AddUserToRole`, `IsUserInRole`, etc. Cuando se invoca uno de estos métodos, la clase `Roles` delega la llamada al proveedor configurado. El `SqlRoleProvider` funciona con las tablas específicas del rol (`aspnet_Roles` y `aspnet_UsersInRoles`) en respuesta.

Para usar el proveedor de `SqlRoleProvider` en nuestra aplicación, es necesario especificar qué base de datos se va a usar como almacén. El `SqlRoleProvider` espera que el almacén de roles especificado tenga determinadas tablas, vistas y procedimientos almacenados de base de datos. Estos objetos de base de datos necesarios se pueden agregar mediante la [herramienta`aspnet_regsql.exe`](https://msdn.microsoft.com/library/ms229862.aspx). En este momento ya tenemos una base de datos con el esquema necesario para el `SqlRoleProvider`. De nuevo en <a id="_msoanchor_6"> </a>el tutorial [*crear el esquema de pertenencia en SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) se creó una base de datos denominada `SecurityTutorials.mdf` y se usa `aspnet_regsql.exe` para agregar los servicios de aplicación, que incluían los objetos de base de datos necesarios para la `SqlRoleProvider`. Por lo tanto, solo es necesario indicar al marco de trabajo de roles que habilite la compatibilidad con roles y use el `SqlRoleProvider` con la `SecurityTutorials.mdf` base de datos como almacén de roles.

El marco de trabajo de roles se configura mediante el &lt;`roleManager`elemento &gt; en el archivo de `Web.config` de la aplicación. De forma predeterminada, la compatibilidad con roles está deshabilitada. Para habilitarlo, debe establecer el atributo `enabled` del elemento de [&gt;de&lt;`roleManager`](https://msdn.microsoft.com/library/ms164660.aspx) en `true` similar a la siguiente:

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample3.xml)]

De forma predeterminada, todas las aplicaciones web tienen un proveedor de roles denominado `AspNetSqlRoleProvider` del tipo `SqlRoleProvider`. Este proveedor predeterminado se registra en `machine.config` (ubicado en `%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG`):

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample4.xml)]

El atributo `connectionStringName` del proveedor especifica el almacén de roles que se utiliza. El proveedor de `AspNetSqlRoleProvider` establece este atributo en `LocalSqlServer`, que también se define en `machine.config` y, de forma predeterminada, en una base de datos de SQL Server 2005 Express Edition en la carpeta `App_Data` denominada `aspnet.mdf`.

Por consiguiente, si simplemente se habilita el marco de trabajo de roles sin especificar ninguna información de proveedor en el archivo de `Web.config` de la aplicación, la aplicación usa el proveedor de roles registrados predeterminado, `AspNetSqlRoleProvider`. Si no existe el `~/App_Data/aspnet.mdf` base de datos, el tiempo de ejecución de ASP.NET lo creará automáticamente y agregará el esquema de servicios de aplicación. Sin embargo, no queremos usar la base de datos de `aspnet.mdf`; en su lugar, queremos usar la base de datos de `SecurityTutorials.mdf` que ya hemos creado y agregado el esquema de servicios de aplicación a. Esta modificación puede realizarse de una de estas dos maneras:

- <strong>Especifique un valor para el</strong>nombre de la <strong>cadena de conexión`LocalSqlServer`en</strong> <strong>`Web.config`</strong> <strong>.</strong> Al sobrescribir el valor de nombre de cadena de conexión de `LocalSqlServer` en `Web.config`, se puede usar el proveedor de roles registrados predeterminado (`AspNetSqlRoleProvider`) y hacer que funcione correctamente con la base de datos de `SecurityTutorials.mdf`. Para obtener más información sobre esta técnica, vea la entrada de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [configurar ASP.net 2,0 Servicios de aplicación para usar SQL Server 2000 o SQL Server 2005](https://weblogs.asp.net/scottgu/archive/2005/08/25/423703.aspx).
- <strong>Agregue un nuevo proveedor registrado de tipo</strong> <strong>`SqlRoleProvider`</strong> <strong>y configure su</strong> <strong>`connectionStringName`</strong> <strong>configuración para que apunte a la</strong> <strong>base de datos`SecurityTutorials.mdf`.</strong> Este es el enfoque que se recomienda y se usa <a id="_msoanchor_7"> </a>en el tutorial [*crear el esquema de pertenencia en SQL Server*](../membership/creating-the-membership-schema-in-sql-server-cs.md) , y es el enfoque que usaré también en este tutorial.

Agregue el siguiente marcado de configuración de roles al archivo `Web.config`. Este marcado registra un nuevo proveedor denominado `SecurityTutorialsSqlRoleProvider`.

[!code-xml[Main](creating-and-managing-roles-cs/samples/sample5.xml)]

La marcación anterior define el `SecurityTutorialsSqlRoleProvider` como el proveedor predeterminado (a través del atributo `defaultProvider` en el elemento `<roleManager>`). También establece el valor de `applicationName` del `SecurityTutorialsSqlRoleProvider`en `SecurityTutorials`, que es la misma configuración de `applicationName` que usa el proveedor de pertenencia (`SecurityTutorialsSqlMembershipProvider`). Aunque no se muestra aquí, el [elemento`<add>`](https://msdn.microsoft.com/library/ms164662.aspx) del `SqlRoleProvider` también puede contener un atributo `commandTimeout` para especificar la duración del tiempo de espera de la base de datos, en segundos. El valor predeterminado es 30.

Con este marcado de configuración en su lugar, estamos preparados para empezar a usar la funcionalidad de rol dentro de nuestra aplicación.

> [!NOTE]
> El marcado de configuración anterior muestra &lt;el uso de los atributos `enabled` y `defaultProvider` del elemento &gt; `roleManager`. Hay otra serie de atributos que afectan a la forma en que el marco de trabajo de roles asocia la información de los roles de usuario por usuario. Examinaremos estas opciones en el <a id="_msoanchor_8"> </a>tutorial de [*autorización basada en roles*](role-based-authorization-cs.md) .

## <a name="step-3-examining-the-roles-api"></a>Paso 3: examen de la API de roles

La funcionalidad de la plataforma de roles se expone a través de la [clase`Roles`](https://msdn.microsoft.com/library/system.web.security.roles.aspx), que contiene trece métodos estáticos para realizar operaciones basadas en roles. Cuando examinemos la creación y eliminación de roles en los pasos 4 y 6, usaremos los métodos [`CreateRole`](https://msdn.microsoft.com/library/system.web.security.roles.createrole.aspx) y [`DeleteRole`](https://msdn.microsoft.com/library/system.web.security.roles.deleterole.aspx) , que agregan o quitan un rol del sistema.

Para obtener una lista de todos los roles del sistema, utilice el [método`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx) (consulte el paso 5). El [método`RoleExists`](https://msdn.microsoft.com/library/system.web.security.roles.roleexists.aspx) devuelve un valor booleano que indica si existe un rol especificado.

En el siguiente tutorial, examinaremos cómo asociar usuarios a roles. Los métodos [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx), [`AddUserToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.addusertoroles.aspx), [`AddUsersToRole`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstorole.aspx)y [`AddUsersToRoles`](https://msdn.microsoft.com/library/system.web.security.roles.adduserstoroles.aspx) de la clase `Roles` agregan uno o más usuarios a uno o más roles. Para quitar usuarios de roles, use los métodos [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx), [`RemoveUserFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromroles.aspx), [`RemoveUsersFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromrole.aspx)o [`RemoveUsersFromRoles`](https://msdn.microsoft.com/library/system.web.security.roles.removeusersfromroles.aspx) .

En el <a id="_msoanchor_9"> </a>tutorial de [*autorización basada en roles*](role-based-authorization-cs.md) , veremos cómo mostrar u ocultar la funcionalidad mediante programación según el rol del usuario que ha iniciado sesión actualmente. Para ello, podemos usar los métodos [`FindUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.findusersinrole.aspx), [`GetRolesForUser`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx), [`GetUsersInRole`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx)o [`IsUserInRole`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx) de la clase `Role`.

> [!NOTE]
> Tenga en cuenta que cada vez que se invoca uno de estos métodos, la clase `Roles` delega la llamada al proveedor configurado. En nuestro caso, esto significa que la llamada se envía al `SqlRoleProvider`. A continuación, el `SqlRoleProvider` realiza la operación de base de datos adecuada según el método invocado. Por ejemplo, el código `Roles.CreateRole("Administrators")` da como resultado el `SqlRoleProvider` ejecutar el `aspnet_Roles_CreateRole` procedimiento almacenado, que inserta un nuevo registro en la tabla de `aspnet_Roles` denominada Administrators.

En el resto de este tutorial se examina el uso de los métodos `CreateRole`, `GetAllRoles`y `DeleteRole` de la clase `Roles` para administrar los roles del sistema.

## <a name="step-4-creating-new-roles"></a>Paso 4: crear nuevos roles

Los roles ofrecen una manera de agrupar a los usuarios de forma arbitraria y, normalmente, se usa esta agrupación para una manera más cómoda de aplicar reglas de autorización. Sin embargo, para poder usar los roles como mecanismo de autorización, primero debemos definir qué roles existen en la aplicación. Desafortunadamente, ASP.NET no incluye un control CreateRoleWizard. Para agregar nuevos roles, necesitamos crear una interfaz de usuario adecuada e invocar la API de roles por nosotros mismos. La buena noticia es que esto es muy fácil de lograr.

> [!NOTE]
> Aunque no hay ningún control Web CreateRoleWizard, existe la [herramienta de administración de sitios web de ASP.net](https://msdn.microsoft.com/library/ms228053.aspx), que es una aplicación ASP.net local diseñada para ayudar a ver y administrar la configuración de la aplicación Web. Sin embargo, no soy un gran abanico de la herramienta de administración de sitios Web ASP.NET por dos motivos. En primer lugar, es un bit con errores y la experiencia del usuario deja mucho que desee. En segundo lugar, la herramienta de administración de sitios Web ASP.NET está diseñada para funcionar solo de forma local, lo que significa que tendrá que crear sus propias páginas web de administración de roles si necesita administrar roles en un sitio activo de forma remota. Por estos dos motivos, este tutorial y el siguiente se centrarán en la creación de las herramientas de administración de roles necesarias en una página web en lugar de depender de la herramienta de administración de sitios web de ASP.NET.

Abra la página `ManageRoles.aspx` en la carpeta `Roles` y agregue un cuadro de texto y un control Web de botón a la página. Establezca la propiedad `ID` del control TextBox en `RoleName` y las propiedades `ID` y `Text` del botón para `CreateRoleButton` y crear el rol, respectivamente. En este momento, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample6.aspx)]

A continuación, haga doble clic en el control de botón `CreateRoleButton` en el diseñador para crear un controlador de eventos `Click` y, a continuación, agregue el código siguiente:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample7.cs)]

El código anterior comienza asignando el nombre de rol recortado especificado en el cuadro de texto `RoleName` a la variable `newRoleName`. A continuación, se llama al método `RoleExists` de la clase `Roles` para determinar si el rol `newRoleName` ya existe en el sistema. Si el rol no existe, se crea a través de una llamada al método `CreateRole`. Si se pasa al método `CreateRole` un nombre de rol que ya existe en el sistema, se produce una excepción de `ProviderException`. Este es el motivo por el que el código comprueba primero para asegurarse de que el rol aún no existe en el sistema antes de llamar a `CreateRole`. El controlador de eventos `Click` concluye desactivando la propiedad `Text` del cuadro de `RoleName`.

> [!NOTE]
> Es posible que se pregunte qué ocurrirá si el usuario no escribe ningún valor en el cuadro de texto `RoleName`. Si el valor que se pasa al método `CreateRole` es `null` o una cadena vacía, se produce una excepción. Del mismo modo, si el nombre de rol contiene una coma, se produce una excepción. Por consiguiente, la página debe contener controles de validación para asegurarse de que el usuario especifica un rol y que no contiene comas. Dejo como un ejercicio para el lector.

Vamos a crear un rol denominado administradores. Visite la página `ManageRoles.aspx` a través de un explorador, escriba administradores en el cuadro de texto (vea la figura 3) y, a continuación, haga clic en el botón crear rol.

[![crear un rol de administrador](creating-and-managing-roles-cs/_static/image8.png)](creating-and-managing-roles-cs/_static/image7.png)

**Figura 3**: creación de un rol de administrador ([haga clic para ver la imagen de tamaño completo](creating-and-managing-roles-cs/_static/image9.png))

¿Qué ocurre? Se produce un postback, pero no hay ninguna indicación visual de que el rol se ha agregado realmente al sistema. Esta página se actualizará en el paso 5 para incluir comentarios visuales. Sin embargo, por ahora, para comprobar que el rol se ha creado, vaya a la base de datos de `SecurityTutorials.mdf` y muestre los datos de la tabla `aspnet_Roles`. Como se muestra en la figura 4, la tabla `aspnet_Roles` contiene un registro para los roles de administradores que se acaban de agregar.

[![la tabla aspnet_Roles tiene una fila para los administradores](creating-and-managing-roles-cs/_static/image11.png)](creating-and-managing-roles-cs/_static/image10.png)

**Figura 4**: la tabla `aspnet_Roles` tiene una fila para los administradores ([haga clic para ver la imagen de tamaño completo](creating-and-managing-roles-cs/_static/image12.png))

## <a name="step-5-displaying-the-roles-in-the-system"></a>Paso 5: mostrar los roles en el sistema

Vamos a aumentar el `ManageRoles.aspx` página para incluir una lista de los roles actuales en el sistema. Para ello, agregue un control GridView a la página y establezca su propiedad `ID` en `RoleList`. A continuación, agregue un método a la clase de código subyacente de la página denominada `DisplayRolesInGrid` mediante el código siguiente:

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample8.cs)]

El método `GetAllRoles` de la clase `Roles` devuelve todos los roles del sistema como una matriz de cadenas. A continuación, esta matriz de cadenas se enlaza a GridView. Para enlazar la lista de roles a GridView cuando se carga la página por primera vez, es necesario llamar al método `DisplayRolesInGrid` desde el controlador de eventos `Page_Load` de la página. El código siguiente llama a este método cuando se visita por primera vez la página, pero no en los postbacks subsiguientes.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample9.cs)]

Con este código en su lugar, visite la página a través de un explorador. Como se muestra en la figura 5, debería ver una cuadrícula con una sola columna con la etiqueta elemento. La cuadrícula incluye una fila para el rol de administradores que se agregó en el paso 4.

[![GridView muestra los roles en una sola columna](creating-and-managing-roles-cs/_static/image14.png)](creating-and-managing-roles-cs/_static/image13.png)

**Figura 5**: GridView muestra los roles en una sola columna ([haga clic para ver la imagen de tamaño completo](creating-and-managing-roles-cs/_static/image15.png))

GridView muestra una columna con la etiqueta elemento, porque la propiedad `AutoGenerateColumns` de GridView está establecida en true (el valor predeterminado), lo que hace que GridView cree automáticamente una columna para cada propiedad de su `DataSource`. Una matriz tiene una propiedad única que representa los elementos de la matriz, por lo tanto, la única columna de GridView.

Al mostrar los datos con un control GridView, prefiero definir explícitamente mis columnas en lugar de hacer que GridView las genere implícitamente. Al definir explícitamente las columnas, es mucho más fácil dar formato a los datos, reorganizar las columnas y realizar otras tareas comunes. Por lo tanto, vamos a actualizar el marcado declarativo de GridView para que sus columnas se definan explícitamente.

Para empezar, establezca la propiedad `AutoGenerateColumns` de GridView en false. A continuación, agregue TemplateField a la cuadrícula, establezca su propiedad `HeaderText` en roles y configure su `ItemTemplate` para que muestre el contenido de la matriz. Para ello, agregue un control Web Label denominado `RoleNameLabel` a la `ItemTemplate` y enlace su propiedad `Text` a `Container.DataItem`.

Estas propiedades y el contenido del `ItemTemplate`se pueden establecer mediante declaración o a través del cuadro de diálogo campos de GridView y la interfaz de edición de plantillas. Para llegar al cuadro de diálogo campos, haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView. A continuación, desactive la casilla generar campos automáticamente para establecer la propiedad `AutoGenerateColumns` en false y agregue TemplateField a GridView, estableciendo su propiedad `HeaderText` en role. Para definir el contenido del `ItemTemplate`, elija la opción editar plantillas de la etiqueta inteligente de GridView. Arrastre un control Web Label al `ItemTemplate`, establezca su propiedad `ID` en `RoleNameLabel`y configure sus valores de DataBinding de modo que su propiedad `Text` esté enlazada a `Container.DataItem`.

Independientemente del enfoque que use, el marcado declarativo resultante de GridView debería tener un aspecto similar al siguiente cuando haya terminado.

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample10.aspx)]

> [!NOTE]
> El contenido de la matriz se muestra mediante la sintaxis de DataBinding `<%# Container.DataItem %>`. Una descripción detallada del motivo por el que se usa esta sintaxis al mostrar el contenido de una matriz enlazada a GridView está fuera del ámbito de este tutorial. Para obtener más información sobre este tema, vea [enlazar una matriz escalar a un control Web de datos](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

Actualmente, el `RoleList` GridView solo está enlazado a la lista de roles cuando la página se visita por primera vez. Es necesario actualizar la cuadrícula cada vez que se agrega un nuevo rol. Para ello, actualice el controlador de eventos de `Click` del botón `CreateRoleButton` de modo que llame al método `DisplayRolesInGrid` si se crea un nuevo rol.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample11.cs)]

Ahora, cuando el usuario agrega un nuevo rol, el `RoleList` GridView muestra el rol que se acaba de agregar en el postback, lo que proporciona información visual de que el rol se ha creado correctamente. Para ilustrar esto, visite la página `ManageRoles.aspx` a través de un explorador y agregue un rol denominado supervisores. Al hacer clic en el botón crear rol, se producirá un postback y la cuadrícula se actualizará para incluir los administradores, así como el nuevo rol, supervisores.

[![se ha agregado el rol supervisores](creating-and-managing-roles-cs/_static/image17.png)](creating-and-managing-roles-cs/_static/image16.png)

**Figura 6**: se ha agregado el rol supervisores ([haga clic para ver la imagen de tamaño completo](creating-and-managing-roles-cs/_static/image18.png))

## <a name="step-6-deleting-roles"></a>Paso 6: eliminación de roles

En este momento, un usuario puede crear un nuevo rol y ver todos los roles existentes en la página `ManageRoles.aspx`. Vamos a permitir a los usuarios eliminar también roles. El método `Roles.DeleteRole` tiene dos sobrecargas:

- [`DeleteRole(roleName)`](https://msdn.microsoft.com/library/ek4sywc0.aspx) : elimina el rol *roleName*. Se produce una excepción si el rol contiene uno o varios miembros.
- [`DeleteRole(roleName, throwOnPopulatedRole)`](https://msdn.microsoft.com/library/38h6wf59.aspx) : elimina el rol *roleName*. Si *throwOnPopulateRole* es `true`, se produce una excepción si el rol contiene uno o varios miembros. Si *throwOnPopulateRole* es `false`, el rol se eliminará si contiene miembros o no. Internamente, el método `DeleteRole(roleName)` llama a `DeleteRole(roleName, true)`.

El método `DeleteRole` también producirá una excepción si *roleName* es `null` o una cadena vacía, o si *roleName* contiene una coma. Si *roleName* no existe en el sistema, `DeleteRole` producirá un error de forma silenciosa, sin generar una excepción.

Vamos a aumentar el GridView en `ManageRoles.aspx` para incluir un botón eliminar que, al hacer clic en él, elimina el rol seleccionado. Comience agregando un botón eliminar a GridView; para ello, vaya al cuadro de diálogo campos y agregue un botón eliminar, que se encuentra en la opción CommandField. Haga que el botón Eliminar sea la columna de la izquierda y establezca su propiedad `DeleteText` en eliminar rol.

[![agregar un botón eliminar a GridView RoleList](creating-and-managing-roles-cs/_static/image20.png)](creating-and-managing-roles-cs/_static/image19.png)

**Figura 7**: agregar un botón eliminar al `RoleList` GridView ([haga clic para ver la imagen de tamaño completo](creating-and-managing-roles-cs/_static/image21.png))

Después de agregar el botón eliminar, el marcado declarativo de GridView debería tener un aspecto similar al siguiente:

[!code-aspx[Main](creating-and-managing-roles-cs/samples/sample12.aspx)]

A continuación, cree un controlador de eventos para el evento `RowDeleting` de GridView. Este es el evento que se genera en el PostBack cuando se hace clic en el botón Eliminar rol. Agregue el siguiente código al controlador de eventos.

[!code-csharp[Main](creating-and-managing-roles-cs/samples/sample13.cs)]

El código comienza haciendo referencia mediante programación al control Web `RoleNameLabel` en la fila cuyo botón Eliminar rol se hizo clic. A continuación, se invoca al método `Roles.DeleteRole`, pasando el `Text` de la `RoleNameLabel` y `false`, con lo que se elimina el rol independientemente de si hay algún usuario asociado al rol. Por último, el `RoleList` GridView se actualiza para que el rol que se acaba de eliminar ya no aparezca en la cuadrícula.

> [!NOTE]
> El botón Eliminar rol no requiere ningún tipo de confirmación por parte del usuario antes de eliminar el rol. Una de las formas más sencillas de confirmar una acción es a través de un cuadro de diálogo de confirmación en el lado cliente. Para obtener más información sobre esta técnica, consulte [Agregar confirmación del lado cliente al eliminar](https://asp.net/learn/data-access/tutorial-42-cs.aspx).

## <a name="summary"></a>Resumen

Muchas aplicaciones web tienen ciertas reglas de autorización o funciones de nivel de página que solo están disponibles para determinadas clases de usuarios. Por ejemplo, puede haber un conjunto de páginas web a las que solo puedan acceder los administradores. En lugar de definir estas reglas de autorización para cada usuario, a menudo resulta más útil definir las reglas basadas en un rol. Es decir, en lugar de permitir explícitamente a los usuarios Scott y Jisun tener acceso a las páginas web administrativas, un enfoque más fácil de mantener es permitir a los miembros del rol administradores tener acceso a estas páginas y, a continuación, indicar a Scott y a Jisun como usuarios que pertenecen a la Rol administradores.

El marco de trabajo de roles facilita la creación y administración de roles. En este tutorial, hemos examinado cómo configurar el marco de trabajo de roles para usar el `SqlRoleProvider`, que usa una base de datos Microsoft SQL Server como almacén de roles. También hemos creado una página web que muestra los roles existentes en el sistema y permite que se creen nuevos roles y que se eliminen los existentes. En los tutoriales siguientes, veremos cómo asignar usuarios a roles y cómo aplicar la autorización basada en roles.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Examen de la pertenencia, los roles y el perfil de ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Cómo: usar el administrador de roles en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998314.aspx)
- [Proveedores de roles](https://msdn.microsoft.com/library/aa478950.aspx)
- [Implementación de su propia herramienta de administración de sitios web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Documentación técnica del elemento `<roleManager>`](https://msdn.microsoft.com/library/ms164660.aspx)
- [Uso de las API de pertenencia y del administrador de roles](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/security/membership.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial incluyen Alicja Maziarz, Banerjee y Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](assigning-roles-to-users-cs.md)
