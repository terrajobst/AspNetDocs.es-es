---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: Asignar roles a los usuarios (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial crearemos dos páginas de ASP.NET para ayudar a administrar qué usuarios pertenecen a qué roles. La primera página incluirá los recursos para ver qué...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: b53df4494eb0faef7c5e4547c2bf95e5fb071298
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74577853"
---
# <a name="assigning-roles-to-users-vb"></a>Asignar roles a los usuarios (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) o [Descargar PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> En este tutorial crearemos dos páginas de ASP.NET para ayudar a administrar qué usuarios pertenecen a qué roles. La primera página incluirá los recursos para ver qué usuarios pertenecen a un rol determinado, a qué roles pertenece un usuario determinado y la capacidad de asignar o quitar un usuario determinado de un rol determinado. En la segunda página, aumentaremos el control CreateUserWizard para que incluya un paso para especificar a qué roles pertenece el usuario recién creado. Esto resulta útil en escenarios en los que un administrador puede crear nuevas cuentas de usuario.

## <a name="introduction"></a>Introducción

En <a id="_msoanchor_1"> </a>el [tutorial anterior](creating-and-managing-roles-vb.md) se examinó el marco de trabajo de roles y el `SqlRoleProvider`; vimos cómo usar la clase `Roles` para crear, recuperar y eliminar roles. Además de crear y eliminar roles, es necesario poder asignar o quitar usuarios de un rol. Desafortunadamente, ASP.NET no se suministra con ningún control Web para administrar qué usuarios pertenecen a qué roles. En su lugar, debemos crear nuestras propias páginas de ASP.NET para administrar estas asociaciones. La buena noticia es que agregar y quitar usuarios de roles es bastante fácil. La clase `Roles` contiene una serie de métodos para agregar uno o más usuarios a uno o más roles.

En este tutorial crearemos dos páginas de ASP.NET para ayudar a administrar qué usuarios pertenecen a qué roles. La primera página incluirá los recursos para ver qué usuarios pertenecen a un rol determinado, a qué roles pertenece un usuario determinado y la capacidad de asignar o quitar un usuario determinado de un rol determinado. En la segunda página, aumentaremos el control CreateUserWizard para que incluya un paso para especificar a qué roles pertenece el usuario recién creado. Esto resulta útil en escenarios en los que un administrador puede crear nuevas cuentas de usuario.

Comencemos.

## <a name="listing-what-users-belong-to-what-roles"></a>Enumerar qué usuarios pertenecen a qué roles

El primer orden de negocio para este tutorial es crear una página web desde la que se pueden asignar usuarios a roles. Antes de preocuparnos por el modo de asignar usuarios a roles, vamos a concentrarnos en cómo determinar qué usuarios pertenecen a qué roles. Hay dos maneras de mostrar esta información: "por rol" o "por usuario". Podríamos permitir al visitante seleccionar un rol y, a continuación, mostrarles todos los usuarios que pertenecen al rol (la pantalla "por rol"), o bien podríamos solicitar al visitante que seleccione un usuario y, a continuación, mostrarles los roles asignados a ese usuario (la pantalla "por usuario").

La vista "por rol" es útil en circunstancias en las que el visitante desea conocer el conjunto de usuarios que pertenecen a un rol determinado. la vista "por usuario" es ideal cuando el visitante necesita conocer los roles de un usuario determinado. Vamos a incluir nuestra página en las interfaces "por rol" y "por usuario".

Comenzaremos por crear la interfaz "por usuario". Esta interfaz constará de una lista desplegable y una lista de casillas. La lista desplegable se rellenará con el conjunto de usuarios del sistema; las casillas enumerarán los roles. Al seleccionar un usuario de la lista desplegable, se comprobarán los roles a los que pertenece el usuario. La persona que visita la página puede activar o desactivar las casillas para agregar o quitar el usuario seleccionado de los roles correspondientes.

> [!NOTE]
> El uso de una lista desplegable para mostrar las cuentas de usuario no es una opción ideal para sitios web en los que puede haber cientos de cuentas de usuario. Una lista desplegable está diseñada para permitir que un usuario elija un elemento de una lista relativamente breve de opciones. El número de elementos de lista crece rápidamente. Si va a crear un sitio web que tendrá un gran número de cuentas de usuario, puede que desee considerar la posibilidad de usar una interfaz de usuario alternativa, como un control GridView paginable o una interfaz filtrable que muestre un mensaje en el que se indique al visitante que elija una letra y, a continuación, solo muestra aquellos usuarios cuyo nombre de usuario comienza por la letra seleccionada.

## <a name="step-1-building-the-by-user-user-interface"></a>Paso 1: creación de la interfaz de usuario "por usuario"

Abra la página `UsersAndRoles.aspx`. En la parte superior de la página, agregue un control Web Label denominado `ActionStatus` y borre su propiedad `Text`. Usaremos esta etiqueta para proporcionar comentarios sobre las acciones realizadas, Mostrar mensajes como, "el usuario Tito se ha agregado al rol de administradores" o "el usuario Jisun se ha quitado del rol supervisores". Para que estos mensajes destaquen, establezca la propiedad `CssClass` de la etiqueta en "Important".

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

A continuación, agregue la siguiente definición de clase CSS a la hoja de estilos `Styles.css`:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Esta definición de CSS indica al explorador que muestre la etiqueta con una fuente de color rojo grande. En la ilustración 1 se muestra este efecto a través del diseñador de Visual Studio.

[![la propiedad CssClass de la etiqueta da como resultado una fuente de color rojo grande](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Figura 1**: la propiedad `CssClass` de la etiqueta da como resultado una fuente de color rojo grande ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image3.png))

Después, agregue un DropDownList a la página, establezca su propiedad `ID` en `UserList`y establezca su propiedad `AutoPostBack` en true. Usaremos este DropDownList para enumerar todos los usuarios del sistema. Este DropDownList se enlazará a una colección de objetos MembershipUser. Dado que deseamos que DropDownList muestre la propiedad UserName del objeto MembershipUser (y lo use como el valor de los elementos de lista), establezca las propiedades `DataTextField` y `DataValueField` de DropDownList en "UserName".

Debajo de DropDownList, agregue un repetidor denominado `UsersRoleList`. Este repetidor enumerará todos los roles del sistema como una serie de casillas. Defina el `ItemTemplate` del repetidor mediante el siguiente marcado declarativo:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

El marcado `ItemTemplate` incluye un único control Web CheckBox denominado `RoleCheckBox`. La propiedad `AutoPostBack` de la casilla está establecida en true y la propiedad `Text` está enlazada a `Container.DataItem`. La razón por la que la sintaxis de DataBinding es simplemente `Container.DataItem` es porque el marco de trabajo de roles devuelve la lista de nombres de rol como una matriz de cadenas y es esta matriz de cadenas que se va a enlazar al repetidor. Una descripción detallada de por qué esta sintaxis se usa para mostrar el contenido de una matriz enlazada a un control Web de datos está fuera del ámbito de este tutorial. Para obtener más información sobre este tema, vea [enlazar una matriz escalar a un control Web de datos](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

En este momento, el marcado declarativo de la interfaz "por usuario" debe ser similar al siguiente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

Ahora estamos preparados para escribir el código para enlazar el conjunto de cuentas de usuario a DropDownList y el conjunto de roles al repetidor. En la clase de código subyacente de la página, agregue un método denominado `BindUsersToUserList` y otro `BindRolesList`con nombre, mediante el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

El método `BindUsersToUserList` recupera todas las cuentas de usuario del sistema a través del [método`Membership.GetAllUsers`](https://msdn.microsoft.com/library/dy8swhya.aspx). Esto devuelve un [objeto`MembershipUserCollection`](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), que es una colección de [instancias de`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). A continuación, esta colección se enlaza al `UserList` DropDownList. Las instancias de `MembershipUser` que estructuran la colección contienen diversas propiedades, como `UserName`, `Email`, `CreationDate`y `IsOnline`. Para indicar a DropDownList que muestre el valor de la propiedad `UserName`, asegúrese de que las propiedades `DataTextField` y `DataValueField` de DropDownList del `UserList` se han establecido en "UserName".

> [!NOTE]
> El método `Membership.GetAllUsers` tiene dos sobrecargas: una que no acepta parámetros de entrada y devuelve todos los usuarios, y otra que toma valores enteros para el índice de página y el tamaño de página, y devuelve solo el subconjunto especificado de los usuarios. Cuando se muestran grandes cantidades de cuentas de usuario en un elemento de la interfaz de usuario paginable, la segunda sobrecarga se puede usar para paginar más eficazmente a través de los usuarios, ya que devuelve solo el subconjunto preciso de cuentas de usuario en lugar de todas ellas.

El método `BindRolesToList` comienza llamando al [método`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)de la clase `Roles`, que devuelve una matriz de cadenas que contiene los roles del sistema. A continuación, esta matriz de cadenas se enlaza al repetidor.

Por último, es necesario llamar a estos dos métodos cuando la página se carga por primera vez. Agregue el código siguiente al controlador de eventos `Page_Load` :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Con este código en su lugar, dedique un momento a visitar la página a través de un explorador. la pantalla debe tener un aspecto similar al de la figura 2. Todas las cuentas de usuario se rellenan en la lista desplegable y, debajo, cada rol aparece como una casilla. Dado que se establecen las propiedades de `AutoPostBack` de DropDownList y las casillas en true, el cambio del usuario seleccionado o la comprobación o desactivación de un rol produce un postback. Sin embargo, no se realiza ninguna acción porque todavía hemos escrito código para controlar estas acciones. Abordaremos estas tareas en las dos secciones siguientes.

[![la página muestra los usuarios y los roles](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Figura 2**: la página muestra los usuarios y los roles ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image6.png))

### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Comprobando los roles a los que pertenece el usuario seleccionado

Cuando se carga la página por primera vez, o cuando el visitante selecciona un nuevo usuario en la lista desplegable, es necesario actualizar las casillas de `UsersRoleList`de modo que solo se seleccione una casilla de rol determinado si el usuario seleccionado pertenece a ese rol. Para ello, cree un método denominado `CheckRolesForSelectedUser` con el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

El código anterior comienza determinando quién es el usuario seleccionado. A continuación, usa el [método`GetRolesForUser(userName)`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) de la clase de roles para devolver el conjunto de roles del usuario especificado como una matriz de cadenas. A continuación, se enumeran los elementos del repetidor y se hace referencia mediante programación a la casilla `RoleCheckBox` de cada elemento. La casilla solo se activa si el rol al que corresponde está incluido en la matriz de cadenas de `selectedUsersRoles`.

> [!NOTE]
> La sintaxis de `Linq.Enumerable.Contains(Of String)(...)` no se compilará si usa la versión 2,0 de ASP.NET. El método `Contains(Of String)` forma parte de la [biblioteca LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), que es nuevo en ASP.net 3,5. Si todavía usa la versión 2,0 de ASP.NET, use en su lugar el [método`Array.IndexOf(Of String)`](https://msdn.microsoft.com/library/eha9t187.aspx) .

Se debe llamar al método `CheckRolesForSelectedUser` en dos casos: cuando se carga por primera vez la página y cada vez que se cambia el índice seleccionado de la `UserList` DropDownList. Por consiguiente, llame a este método desde el controlador de eventos `Page_Load` (después de las llamadas a `BindUsersToUserList` y `BindRolesToList`). Además, cree un controlador de eventos para el evento de `SelectedIndexChanged` de DropDownList y llame a este método desde allí.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Con este código en su lugar, puede probar la página a través del explorador. Sin embargo, puesto que la página `UsersAndRoles.aspx` no tiene la capacidad de asignar usuarios a roles, ningún usuario tiene roles. Vamos a crear la interfaz para asignar usuarios a roles en un momento, por lo que puede tomar la palabra que funciona en este código y comprobar que lo hace más tarde, o bien puede Agregar usuarios manualmente a roles insertando registros en la tabla de `aspnet_UsersInRoles` para probar esta funcionalidad ahora.

### <a name="assigning-and-removing-users-from-roles"></a>Asignar y quitar usuarios de roles

Cuando el visitante comprueba o desactiva una casilla en el `UsersRoleList` Repeater, es necesario agregar o quitar el usuario seleccionado del rol correspondiente. Actualmente, la propiedad `AutoPostBack` de la casilla está establecida en true, lo que produce un postback en el momento en que se activa o desactiva una casilla en el repetidor. En Resumen, es necesario crear un controlador de eventos para el evento `CheckChanged` de la casilla. Como la casilla está en un control Repeater, es necesario agregar manualmente la fontanería del controlador de eventos. Comience agregando el controlador de eventos a la clase de código subyacente como un método `Protected`, de la siguiente manera:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

En un momento, volveremos a escribir el código de este controlador de eventos. Pero primero vamos a completar la fontanería de control de eventos. En la casilla que se encuentra dentro del `ItemTemplate`del repetidor, agregue `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Esta sintaxis conecta el `RoleCheckBox_CheckChanged` controlador de eventos al evento `CheckedChanged` del `RoleCheckBox`.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

Nuestra tarea final es completar el controlador de eventos de `RoleCheckBox_CheckChanged`. Es necesario comenzar haciendo referencia al control CheckBox que provocó el evento porque esta instancia de casilla indica qué rol se ha activado o desactivado a través de sus propiedades `Text` y `Checked`. Con esta información junto con el nombre de usuario del usuario seleccionado, se agrega o se quita el usuario del rol a través del método [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) o [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)de la clase `Roles`.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

El código anterior comienza mediante programación para hacer referencia a la casilla que provocó el evento, que está disponible a través del parámetro de entrada `sender`. Si la casilla está activada, el usuario seleccionado se agrega al rol especificado; de lo contrario, se quita del rol. En cualquier caso, la etiqueta `ActionStatus` muestra un mensaje que resume la acción que se acaba de realizar.

Dedique un momento a probar esta página a través de un explorador. Seleccione usuario Tito y, a continuación, agregue Tito a los roles administradores y supervisores.

[![Tito se ha agregado a los roles administradores y supervisores](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Figura 3**: Tito se ha agregado a los roles administradores y supervisores ([haga clic para ver la imagen a tamaño completo](assigning-roles-to-users-vb/_static/image9.png))

A continuación, seleccione User Bruce en la lista desplegable. Hay un postback y las casillas del repetidor se actualizan mediante el `CheckRolesForSelectedUser`. Dado que Bruce todavía no pertenece a ningún rol, las dos casillas están desactivadas. A continuación, agregue Bruce al rol supervisores.

[![Bruce se ha agregado al rol supervisores](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Figura 4**: Bruce se ha agregado al rol supervisores ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image12.png))

Para comprobar aún más la funcionalidad del método `CheckRolesForSelectedUser`, seleccione un usuario que no sea Tito o Bruce. Tenga en cuenta que las casillas se desactivan automáticamente, lo que indica que no pertenecen a ningún rol. Vuelva a Tito. Deben comprobarse las casillas administradores y supervisores.

## <a name="step-2-building-the-by-roles-user-interface"></a>Paso 2: creación de la interfaz de usuario "por roles"

Llegados a este punto, hemos completado la interfaz "por usuarios" y estamos preparados para empezar a abordar la interfaz "por roles". La interfaz "por roles" solicita al usuario que seleccione un rol en una lista desplegable y, a continuación, muestra el conjunto de usuarios que pertenecen a ese rol en un control GridView.

Agregue otro control DropDownList al `UsersAndRoles.aspx page`. Colóquelo debajo del control Repeater, asígnele el nombre `RoleList`y establezca su propiedad `AutoPostBack` en true. Debajo, agregue un control GridView y asígnele el nombre `RolesUserList`. Este GridView mostrará una lista de los usuarios que pertenecen al rol seleccionado. Establezca la propiedad `AutoGenerateColumns` de GridView en false, agregue un TemplateField a la colección de `Columns` de la cuadrícula y establezca su propiedad `HeaderText` en "Users". Defina el `ItemTemplate` de TemplateField para que muestre el valor de la expresión de enlace de enlaces `Container.DataItem` en la propiedad `Text` de una etiqueta denominada `UserNameLabel`.

Después de agregar y configurar GridView, el marcado declarativo de la interfaz "por rol" debe ser similar al siguiente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

Es necesario rellenar el `RoleList` DropDownList con el conjunto de roles del sistema. Para ello, actualice el método `BindRolesToList` de modo que enlace la matriz de cadenas devuelta por el método `Roles.GetAllRoles` al `RolesList` DropDownList (así como al `UsersRoleList` Repeater).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Las dos últimas líneas del método `BindRolesToList` se han agregado para enlazar el conjunto de roles con el control DropDownList `RoleList`. En la figura 5 se muestra el resultado final cuando se ve a través de un explorador: una lista desplegable que se rellena con los roles del sistema.

[![los roles se muestran en el DropDownList RoleList](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Figura 5**: los roles se muestran en el `RoleList` DropDownList ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image15.png))

### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Mostrar los usuarios que pertenecen al rol seleccionado

Cuando se carga la página por primera vez, o cuando se selecciona un nuevo rol en el `RoleList` DropDownList, es necesario mostrar la lista de usuarios que pertenecen a ese rol en GridView. Cree un método denominado `DisplayUsersBelongingToRole` mediante el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Este método comienza obteniendo el rol seleccionado del `RoleList` DropDownList. A continuación, usa el [método`Roles.GetUsersInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) para recuperar una matriz de cadenas de los nombres de usuario de los usuarios que pertenecen a ese rol. Esta matriz se enlaza a la `RolesUserList` GridView.

Se debe llamar a este método en dos circunstancias: cuando la página se carga inicialmente y cuando cambia el rol seleccionado en el `RoleList` DropDownList. Por lo tanto, actualice el controlador de eventos `Page_Load` para que este método se invoque después de la llamada a `CheckRolesForSelectedUser`. A continuación, cree un controlador de eventos para el evento de `SelectedIndexChanged` del `RoleList`y llame también a este método desde allí.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Con este código en su lugar, el `RolesUserList` GridView debe mostrar los usuarios que pertenecen al rol seleccionado. Como se muestra en la figura 6, el rol supervisores consta de dos miembros: Bruce y Tito.

[![GridView muestra los usuarios que pertenecen al rol seleccionado](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Figura 6**: el control GridView muestra los usuarios que pertenecen al rol seleccionado ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image18.png))

### <a name="removing-users-from-the-selected-role"></a>Quitar usuarios del rol seleccionado

Vamos a aumentar el `RolesUserList` GridView para que incluya una columna de botones "quitar". Al hacer clic en el botón "quitar" de un usuario determinado, se quitarán de ese rol.

Comience agregando un campo de botón eliminar a GridView. Haga que este campo aparezca como el más a la izquierda y cambie su propiedad `DeleteText` de "eliminar" (valor predeterminado) a "quitar".

[![agregar el](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Figura 7**: agregar el botón "quitar" a GridView ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image21.png))

Cuando se hace clic en el botón "quitar", se produce un postback y se genera el evento de `RowDeleting` de GridView. Necesitamos crear un controlador de eventos para este evento y escribir código que quite al usuario del rol seleccionado. Cree el controlador de eventos y, a continuación, agregue el siguiente código:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

El código comienza determinando el nombre del rol seleccionado. A continuación, hace referencia mediante programación al control `UserNameLabel` desde la fila en la que se hizo clic en el botón "quitar" para determinar el nombre de usuario que se va a quitar. A continuación, el usuario se quita del rol a través de una llamada al método `Roles.RemoveUserFromRole`. A continuación, se actualiza el `RolesUserList` GridView y se muestra un mensaje a través del control etiqueta `ActionStatus`.

> [!NOTE]
> El botón "quitar" no requiere ningún tipo de confirmación por parte del usuario antes de quitar el usuario del rol. Le invitamos a agregar algún nivel de confirmación del usuario. Una de las formas más sencillas de confirmar una acción es a través de un cuadro de diálogo de confirmación en el lado cliente. Para obtener más información sobre esta técnica, consulte [Agregar confirmación del lado cliente al eliminar](https://asp.net/learn/data-access/tutorial-42-vb.aspx).

En la figura 8 se muestra la página después de haber quitado el usuario Tito del grupo supervisores.

[![lamentablemente, Tito ya no es un supervisor](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Figura 8**: Lamentablemente, Tito ya no es un supervisor ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image24.png))

### <a name="adding-new-users-to-the-selected-role"></a>Agregar nuevos usuarios al rol seleccionado

Además de quitar usuarios del rol seleccionado, el visitante de esta página también debe ser capaz de agregar un usuario al rol seleccionado. La mejor interfaz para agregar un usuario al rol seleccionado depende del número de cuentas de usuario que espera tener. Si el sitio web va a hospedar solo unas docenas cuentas de usuario o menos, podría usar un DropDownList aquí. Si puede haber miles de cuentas de usuario, querrá incluir una interfaz de usuario que permita al visitante desplazarse por las cuentas, buscar una cuenta determinada o filtrar las cuentas de usuario de otra manera.

En esta página, vamos a usar una interfaz muy sencilla que funciona independientemente del número de cuentas de usuario del sistema. Es decir, usaremos un cuadro de texto, solicitando al visitante que escriba el nombre del usuario que desea agregar al rol seleccionado. Si no existe ningún usuario con ese nombre o si el usuario ya es miembro del rol, se mostrará un mensaje en `ActionStatus` etiqueta. Pero si el usuario existe y no es miembro del rol, los agregaremos al rol y actualizaremos la cuadrícula.

Agregue un cuadro de texto y un botón debajo de GridView. Establezca la `ID` del cuadro de texto en `UserNameToAddToRole` y establezca las propiedades `ID` y `Text` del botón en `AddUserToRoleButton` y "Agregar usuario a rol", respectivamente.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

A continuación, cree un controlador de eventos `Click` para la `AddUserToRoleButton` y agregue el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

La mayoría del código del controlador de eventos `Click` realiza varias comprobaciones de validación. Garantiza que el visitante proporcionó un nombre de usuario en el cuadro de texto `UserNameToAddToRole`, que el usuario existe en el sistema y que aún no pertenece al rol seleccionado. Si se produce un error en cualquiera de estas comprobaciones, se muestra un mensaje adecuado en `ActionStatus` y se sale del controlador de eventos. Si se pasan todas las comprobaciones, el usuario se agrega al rol a través del método `Roles.AddUserToRole`. A continuación, se borra la propiedad `Text` del cuadro de texto, se actualiza GridView y la etiqueta `ActionStatus` muestra un mensaje que indica que el usuario especificado se agregó correctamente al rol seleccionado.

> [!NOTE]
> Para asegurarse de que el usuario especificado no pertenece al rol seleccionado, usamos el [método`Roles.IsUserInRole(userName, roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), que devuelve un valor booleano que indica si el *nombre de usuario* es un miembro de *roleName*. Usaremos este método de nuevo en el <a id="_msoanchor_2"> </a> [siguiente tutorial](role-based-authorization-vb.md) cuando examinemos la autorización basada en roles.

Visite la página a través de un explorador y seleccione el rol supervisores en el `RoleList` DropDownList. Intente escribir un nombre de usuario no válido; debería ver un mensaje que explica que el usuario no existe en el sistema.

[![no puede Agregar un usuario que no existe a un rol](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Figura 9**: no puede Agregar un usuario que no existe a un rol ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image27.png))

Ahora intente agregar un usuario válido. Continúe y vuelva a agregar Tito al rol supervisores.

[![Tito vuelve a ser un supervisor.](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Figura 10**: Tito vuelve a ser un supervisor.  ([Haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image30.png))

## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Paso 3: actualización cruzada de las interfaces "por usuario" y "por rol"

La página `UsersAndRoles.aspx` ofrece dos interfaces distintas para administrar usuarios y roles. Actualmente, estas dos interfaces actúan de forma independiente entre sí, por lo que es posible que un cambio realizado en una interfaz no se refleje inmediatamente en el otro. Por ejemplo, Imagine que el visitante de la página selecciona el rol supervisores en el `RoleList` DropDownList, que enumera Bruce y Tito como sus miembros. Después, el visitante selecciona Tito en el `UserList` DropDownList, que comprueba las casillas administradores y supervisores en el repetidor `UsersRoleList`. Si, a continuación, el visitante desmarca el rol supervisor del repetidor, Tito se quita del rol supervisores, pero esta modificación no se refleja en la interfaz "por rol". GridView seguirá mostrando Tito como miembro del rol supervisores.

Para solucionar este paso, es necesario actualizar GridView cada vez que se activa o desactiva un rol en el repetidor de `UsersRoleList`. Del mismo modo, es necesario actualizar el repetidor cada vez que se quita o agrega un usuario a un rol de la interfaz "por rol".

El repetidor de la interfaz "por usuario" se actualiza llamando al método `CheckRolesForSelectedUser`. La interfaz "por rol" se puede modificar en la `RolesUserList` controlador de eventos `RowDeleting` de GridView y en el controlador de eventos `Click` del botón `AddUserToRoleButton`. Por lo tanto, es necesario llamar al método `CheckRolesForSelectedUser` desde cada uno de estos métodos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Del mismo modo, el control GridView de la interfaz "por rol" se actualiza llamando al método `DisplayUsersBelongingToRole` y la interfaz "por usuario" se modifica a través del controlador de eventos `RoleCheckBox_CheckChanged`. Por lo tanto, es necesario llamar al método `DisplayUsersBelongingToRole` desde este controlador de eventos.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Con estos cambios de código secundarios, las interfaces "por usuario" y "por rol" ahora se actualizan correctamente. Para comprobarlo, visite la página a través de un explorador y seleccione Tito y supervisores en el `UserList` y `RoleList` DropDownLists, respectivamente. Tenga en cuenta que, al desproteger el rol supervisores de Tito del repetidor en la interfaz "por usuario", Tito se quita automáticamente de GridView en la interfaz "por rol". Al volver a agregar Tito al rol supervisores desde la interfaz "por rol", se vuelve a comprobar automáticamente la casilla supervisores en la interfaz "por usuario".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Paso 4: personalización de CreateUserWizard para incluir un paso "especificar roles"

En el <a id="_msoanchor_3"> </a>tutorial [*creación de cuentas de usuario*](../membership/creating-user-accounts-vb.md) , vimos cómo usar el control Web CreateUserWizard para proporcionar una interfaz para crear una nueva cuenta de usuario. El control CreateUserWizard se puede usar de dos maneras:

- Como un medio para que los visitantes creen su propia cuenta de usuario en el sitio, y
- Como un medio para que los administradores creen cuentas nuevas

En el primer caso de uso, un visitante llega al sitio y rellena el objeto CreateUserWizard, escribiendo su información para registrarse en el sitio. En el segundo caso, un administrador crea una cuenta nueva para otra persona.

Cuando un administrador de otra persona crea una cuenta, podría ser útil permitir que el administrador especifique a qué roles pertenece la nueva cuenta de usuario. En el <a id="_msoanchor_4"> </a>tutorial [ *almacenamiento* de *información de usuario adicional* ](../membership/storing-additional-user-information-vb.md) , vimos cómo personalizar CreateUserWizard agregando `WizardSteps`adicionales. Echemos un vistazo a cómo agregar un paso adicional a CreateUserWizard para especificar los roles del nuevo usuario.

Abra la página `CreateUserWizardWithRoles.aspx` y agregue un control CreateUserWizard denominado `RegisterUserWithRoles`. Establezca la propiedad `ContinueDestinationPageUrl` del control en "~/default.aspx". Dado que la idea es que un administrador va a utilizar este control CreateUserWizard para crear nuevas cuentas de usuario, establezca la propiedad `LoginCreatedUser` del control en false. Esta propiedad `LoginCreatedUser` especifica si el visitante inicia sesión automáticamente como el usuario recién creado y su valor predeterminado es true. Se establece en false porque, cuando un administrador crea una cuenta nueva, queremos mantenerla iniciada.

A continuación, seleccione la "Agregar o quitar `WizardSteps`..." en la etiqueta inteligente de CreateUserWizard y agregue una nueva `WizardStep`, estableciendo su `ID` en `SpecifyRolesStep`. Mueva el `SpecifyRolesStep WizardStep` para que venga después del paso "Suscribirse a la nueva cuenta", pero antes del paso "completar". Establezca la propiedad `Title` del `WizardStep`en "especificar roles", su propiedad `StepType` en `Step`y su propiedad `AllowReturn` en false.

[![agregar el](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Figura 11**: agregar el `WizardStep` "especificar roles" a CreateUserWizard ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image33.png))

Después de este cambio, el marcado declarativo de CreateUserWizard debería tener un aspecto similar al siguiente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

En el `WizardStep`"especificar roles", agregue un CheckBoxList denominado `RoleList.` este CheckBoxList enumerará los roles disponibles, lo que permite a la persona que visita la página comprobar a qué roles pertenece el usuario recién creado.

Nos queda con dos tareas de codificación: primero debemos rellenar el `RoleList` CheckBoxList con los roles del sistema. en segundo lugar, es necesario agregar el usuario creado a los roles seleccionados cuando el usuario se mueve del paso "especificar roles" al paso "completar". Podemos realizar la primera tarea en el controlador de eventos `Page_Load`. El código siguiente hace referencia mediante programación a la casilla `RoleList` en la primera visita a la página y enlaza los roles del sistema a ella.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

El código anterior debería resultar familiar. En el <a id="_msoanchor_5"> </a>tutorial [ *almacenamiento* de *información de usuario adicional* ](../membership/storing-additional-user-information-vb.md) , usamos dos instrucciones `FindControl` para hacer referencia a un control Web desde dentro de un `WizardStep`personalizado. Y el código que enlaza los roles al CheckBoxList se tomó anteriormente en este tutorial.

Para realizar la segunda tarea de programación, es necesario saber cuándo se ha completado el paso "especificar roles". Recuerde que CreateUserWizard tiene un evento `ActiveStepChanged`, que se activa cada vez que el visitante navega de un paso a otro. Aquí podemos determinar si el usuario ha alcanzado el paso "completar"; Si es así, es necesario agregar el usuario a los roles seleccionados.

Cree un controlador de eventos para el evento `ActiveStepChanged` y agregue el código siguiente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Si el usuario acaba de alcanzar el paso "completado", el controlador de eventos enumera los elementos de los `RoleList` CheckBoxList y el usuario recién creado se asigna a los roles seleccionados.

Visite esta página a través de un explorador. El primer paso de CreateUserWizard es el paso estándar "registrarse para obtener una nueva cuenta", que solicita el nombre de usuario, la contraseña, el correo electrónico y otra información clave del nuevo usuario. Escriba la información para crear un nuevo usuario llamado Wanda.

[![crear un nuevo usuario llamado Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Figura 12**: creación de un nuevo usuario llamado Wanda ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image36.png))

Haga clic en el botón "crear usuario". CreateUserWizard llama internamente al método `Membership.CreateUser`, crea la nueva cuenta de usuario y, a continuación, progresa al paso siguiente, "especificar roles". Aquí se enumeran los roles del sistema. Active la casilla supervisores y haga clic en siguiente.

[![convertir Wanda en miembro del rol supervisores](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Figura 13**: convertir Wanda en miembro del rol supervisores ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image39.png))

Al hacer clic en siguiente se produce un postback y se actualiza el `ActiveStep` al paso "Complete". En el controlador de eventos `ActiveStepChanged`, la cuenta de usuario creada recientemente se asigna al rol supervisores. Para comprobarlo, vuelva a la página `UsersAndRoles.aspx` y seleccione supervisores en el `RoleList` DropDownList. Como se muestra en la figura 14, los supervisores ahora se componen de tres usuarios: Bruce, Tito y Wanda.

[![Bruce, Tito y Wanda son supervisores](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Figura 14**: Bruce, Tito y Wanda son supervisores ([haga clic para ver la imagen de tamaño completo](assigning-roles-to-users-vb/_static/image42.png))

## <a name="summary"></a>Resumen

El marco de trabajo de roles ofrece métodos para recuperar información acerca de los roles y métodos de un usuario determinado para determinar qué usuarios pertenecen a un rol especificado. Además, hay una serie de métodos para agregar y quitar uno o más usuarios a uno o más roles. En este tutorial nos centramos solo en dos de estos métodos: `AddUserToRole` y `RemoveUserFromRole`. Hay otras variantes diseñadas para agregar varios usuarios a un único rol y asignar varios roles a un solo usuario.

En este tutorial también se incluye un vistazo a la extensión del control CreateUserWizard para incluir un `WizardStep` para especificar los roles del usuario recién creado. Este paso podría ayudar a un administrador a simplificar el proceso de creación de cuentas de usuario para nuevos usuarios.

En este punto, hemos descubierto cómo crear y eliminar roles y cómo agregar y quitar usuarios de roles. Sin embargo, todavía veremos cómo aplicar la autorización basada en roles. En el <a id="_msoanchor_6"> </a> [tutorial siguiente](role-based-authorization-vb.md) , veremos cómo definir las reglas de autorización de URL en función de cada rol, además de cómo limitar la funcionalidad de nivel de página en función de los roles del usuario que ha iniciado sesión actualmente.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Información general de la herramienta de administración de sitios web de ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Examinando ASP. Pertenencia, roles y Perfil de la red](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Implementación de su propia herramienta de administración de sitios web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial...

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial era Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-and-managing-roles-vb.md)
> [Siguiente](role-based-authorization-vb.md)
