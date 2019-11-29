---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
title: Almacenar información de usuario adicionalC#() | Microsoft Docs
author: rick-anderson
description: En este tutorial, se responderá a esta pregunta mediante la creación de una aplicación de libro de visitas muy rudimentaria. Al hacerlo, veremos distintas opciones para modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: 1642132a-1ca5-4872-983f-ab59fc8865d3
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-cs
msc.type: authoredcontent
ms.openlocfilehash: 24b96e86bc93e03d2639b73e35ed1fd1271bac5a
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74641454"
---
# <a name="storing-additional-user-information-c"></a>Almacenar información de usuario adicional (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_cs.pdf)

> En este tutorial, se responderá a esta pregunta mediante la creación de una aplicación de libro de visitas muy rudimentaria. Al hacerlo, veremos distintas opciones para modelar la información de usuario en una base de datos y, a continuación, veremos cómo asociar estos datos con las cuentas de usuario creadas por el marco de pertenencia.

## <a name="introduction"></a>Introducción

ASP. El marco de pertenencia de la red ofrece una interfaz flexible para administrar usuarios. La API de pertenencia incluye métodos para validar credenciales, recuperar información sobre el usuario que ha iniciado la sesión actual, crear una nueva cuenta de usuario y eliminar una cuenta de usuario, entre otros. Cada cuenta de usuario del marco de pertenencia solo contiene las propiedades necesarias para validar las credenciales y realizar tareas esenciales relacionadas con la cuenta de usuario. Esto se prueba mediante los métodos y las propiedades de la [clase`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), que modela una cuenta de usuario en el marco de pertenencia. Esta clase tiene propiedades como [`UserName`](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [`Email`](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)y [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx), y métodos como [`GetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) y [`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

A menudo, las aplicaciones necesitan almacenar información de usuario adicional no incluida en el marco de pertenencia. Por ejemplo, un distribuidor en línea podría requerir que cada usuario almacene sus direcciones de envío y facturación, la información de pago, las preferencias de entrega y el número de teléfono de contacto. Además, cada pedido del sistema está asociado a una cuenta de usuario determinada.

La clase `MembershipUser` no incluye propiedades como `PhoneNumber` o `DeliveryPreferences` o `PastOrders`. ¿Cómo se realiza el seguimiento de la información de usuario que necesita la aplicación y se integra con el marco de pertenencia? En este tutorial, se responderá a esta pregunta mediante la creación de una aplicación de libro de visitas muy rudimentaria. Al hacerlo, veremos distintas opciones para modelar la información de usuario en una base de datos y, a continuación, veremos cómo asociar estos datos con las cuentas de usuario creadas por el marco de pertenencia. Comencemos.

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Paso 1: crear el modelo de datos de la aplicación del libro de visitas

Hay varias técnicas que se pueden emplear para capturar información de usuario en una base de datos y asociarla a las cuentas de usuario creadas por el marco de pertenencia. Con el fin de ilustrar estas técnicas, tendremos que aumentar la aplicación web del tutorial para que capture algún tipo de datos relacionados con el usuario. (Actualmente, el modelo de datos de la aplicación solo contiene las tablas de servicios de aplicación que necesita el `SqlMembershipProvider`).

Vamos a crear una aplicación de libro de visitas muy sencilla en la que un usuario autenticado puede dejar un comentario. Además de almacenar los comentarios del libro de visitas, vamos a permitir que cada usuario almacene su ciudad, Página principal y firma. Si se proporciona, la ciudad, la Página principal y la firma del usuario aparecerán en cada mensaje que haya dejado en el libro de visitas.

### <a name="adding-theguestbookcommentstable"></a>Agregar la tabla`GuestbookComments`

Con el fin de capturar los comentarios del libro de visitas, es necesario crear una tabla de base de datos denominada `GuestbookComments` que tenga columnas como `CommentId`, `Subject`, `Body`y `CommentDate`. También es necesario que cada registro de la tabla `GuestbookComments` haga referencia al usuario que dejó el comentario.

Para agregar esta tabla a nuestra base de datos, vaya al Explorador de bases de datos en Visual Studio y explore en profundidad la base de datos de `SecurityTutorials`. Haga clic con el botón derecho en la carpeta tablas y elija Agregar nueva tabla. Esto abre una interfaz que nos permite definir las columnas de la nueva tabla.

[![agregar una nueva tabla a la base de datos SecurityTutorials](storing-additional-user-information-cs/_static/image2.png)](storing-additional-user-information-cs/_static/image1.png)

**Figura 1**: agregar una nueva tabla a la base de datos de `SecurityTutorials` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image3.png))

A continuación, defina las columnas del `GuestbookComments`. Comience agregando una columna denominada `CommentId` del tipo `uniqueidentifier`. Esta columna identificará de forma única cada comentario en el libro de visitas, por lo que no permita `NULL` s y márquelo como clave principal de la tabla. En lugar de proporcionar un valor para el campo `CommentId` en cada `INSERT`, podemos indicar que se debe generar automáticamente un nuevo valor de `uniqueidentifier` para este campo en `INSERT` estableciendo el valor predeterminado de la columna en `NEWID()`. Después de agregar este primer campo, marcarlo como la clave principal y configuración de su valor predeterminado, la pantalla debe ser similar a la captura de pantalla que se muestra en la figura 2.

[![agregar una columna principal denominada CommentId](storing-additional-user-information-cs/_static/image5.png)](storing-additional-user-information-cs/_static/image4.png)

**Figura 2**: agregar una columna principal denominada `CommentId` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image6.png))

A continuación, agregue una columna denominada `Subject` del tipo `nvarchar(50)` y una columna denominada `Body` de tipo `nvarchar(MAX)`, lo que no permite `NULL` s en ambas columnas. A continuación, agregue una columna denominada `CommentDate` del tipo `datetime`. No permitir `NULL` s y establecer el valor predeterminado de la columna `CommentDate` en `getdate()`.

Lo único que queda es agregar una columna que asocie una cuenta de usuario con cada comentario del libro de visitas. Una opción sería agregar una columna denominada `UserName` del tipo `nvarchar(256)`. Esta es una opción adecuada cuando se usa un proveedor de pertenencia que no sea el `SqlMembershipProvider`. Pero al usar el `SqlMembershipProvider`, como estamos en esta serie de tutoriales, no se garantiza que la columna `UserName` de la tabla `aspnet_Users` sea única. La clave principal de la tabla `aspnet_Users` es `UserId` y es de tipo `uniqueidentifier`. Por lo tanto, la tabla `GuestbookComments` necesita una columna denominada `UserId` de tipo `uniqueidentifier` (no se permiten `NULL` valores). Continúe y agregue esta columna.

> [!NOTE]
> Como se explicó en el tutorial [*crear el esquema de pertenencia en SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) , el marco de pertenencia está diseñado para permitir que varias aplicaciones web con cuentas de usuario diferentes compartan el mismo almacén de usuario. Para ello, crea particiones de las cuentas de usuario en distintas aplicaciones. Además, aunque se garantiza que cada nombre de usuario es único dentro de una aplicación, se puede usar el mismo nombre de usuario en aplicaciones diferentes con el mismo almacén de usuario. Hay una restricción `UNIQUE` compuesta en la tabla `aspnet_Users` de los campos `UserName` y `ApplicationId`, pero no en el campo `UserName`. Por lo tanto, es posible que la tabla de usuarios de ASPNET\_tenga dos (o más) registros con el mismo valor de `UserName`. No obstante, hay una restricción `UNIQUE` en el campo `UserId` de la tabla `aspnet_Users` (ya que es la clave principal). Una restricción `UNIQUE` es importante porque sin ella no se puede establecer una restricción FOREIGN KEY entre las tablas `GuestbookComments` y `aspnet_Users`.

Después de agregar el `UserId` columna, haga clic en el icono guardar de la barra de herramientas para guardar la tabla. Asigne un nombre a la nueva tabla `GuestbookComments`.

Tenemos un último problema para asistir con la tabla `GuestbookComments`: es necesario crear una [restricción FOREIGN KEY](https://msdn.microsoft.com/library/ms175464.aspx) entre la columna `GuestbookComments.UserId` y la columna `aspnet_Users.UserId`. Para ello, haga clic en el icono de relación en la barra de herramientas para iniciar el cuadro de diálogo relaciones de clave externa. (También puede iniciar este cuadro de diálogo; para ello, vaya al menú Diseñador de tablas y elija relaciones).

Haga clic en el botón Agregar situado en la esquina inferior izquierda del cuadro de diálogo relaciones de clave externa. Esto agregará una nueva restricción de clave externa, aunque todavía es necesario definir las tablas que participan en la relación.

[![usar el cuadro de diálogo relaciones de clave externa para administrar las restricciones de clave externa de una tabla](storing-additional-user-information-cs/_static/image8.png)](storing-additional-user-information-cs/_static/image7.png)

**Figura 3**: usar el cuadro de diálogo relaciones de clave externa para administrar las restricciones de clave externa de una tabla ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image9.png))

A continuación, haga clic en el icono de puntos suspensivos de la fila "Especificaciones de tabla y columnas" de la derecha. Se abrirá el cuadro de diálogo tablas y columnas, desde el que se puede especificar la tabla y la columna de la clave principal y la columna de clave externa de la tabla `GuestbookComments`. En concreto, seleccione `aspnet_Users` y `UserId` como la tabla y la columna de la clave principal y `UserId` de la tabla de `GuestbookComments` como columna de clave externa (consulte la figura 4). Después de definir las tablas y columnas de clave principal y externa, haga clic en Aceptar para volver al cuadro de diálogo relaciones de clave externa.

[![establecer una restricción de clave externa entre las tablas aspnet_Users y GuesbookComments](storing-additional-user-information-cs/_static/image11.png)](storing-additional-user-information-cs/_static/image10.png)

**Figura 4**: establecimiento de una restricción de clave externa entre las tablas `aspnet_Users` y `GuesbookComments` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image12.png))

En este momento se ha establecido la restricción FOREIGN KEY. La presencia de esta restricción garantiza la [integridad relacional](http://en.wikipedia.org/wiki/Referential_integrity) entre las dos tablas garantizando que nunca habrá una entrada de libro de visitas que hace referencia a una cuenta de usuario que no existe. De forma predeterminada, una restricción de clave externa impedirá que se elimine un registro primario si hay registros secundarios correspondientes. Es decir, si un usuario realiza uno o varios comentarios del libro de visitas y, a continuación, intentamos eliminar esa cuenta de usuario, se producirá un error en la eliminación a menos que primero se eliminen los comentarios del libro de visitas.

Las restricciones Foreign Key se pueden configurar para eliminar automáticamente los registros secundarios asociados cuando se elimina un registro primario. En otras palabras, podemos configurar esta restricción de clave externa para que las entradas del libro de visitas de un usuario se eliminen automáticamente cuando se elimine su cuenta de usuario. Para ello, expanda la sección "Especificación de inserción y actualización" y establezca la propiedad "eliminar regla" en cascada.

[![configurar la restricción FOREIGN KEY para las eliminaciones en cascada](storing-additional-user-information-cs/_static/image14.png)](storing-additional-user-information-cs/_static/image13.png)

**Figura 5**: configuración de la restricción FOREIGN KEY para las eliminaciones en cascada ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image15.png))

Para guardar la restricción de clave externa, haga clic en el botón Cerrar para salir de las relaciones de clave externa. A continuación, haga clic en el icono guardar en la barra de herramientas para guardar la tabla y la relación this.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Almacenar la ciudad, la Página principal y la firma del usuario

En la tabla `GuestbookComments` se muestra cómo almacenar información que comparte una relación de uno a varios con cuentas de usuario. Puesto que cada cuenta de usuario puede tener un número arbitrario de comentarios asociados, esta relación se modela mediante la creación de una tabla que contenga el conjunto de comentarios que incluye una columna que vincula cada comentario con un usuario determinado. Al utilizar el `SqlMembershipProvider`, este vínculo se establece mejor creando una columna denominada `UserId` de tipo `uniqueidentifier` y una restricción FOREIGN KEY entre esta columna y `aspnet_Users.UserId`.

Ahora es necesario asociar tres columnas a cada cuenta de usuario para almacenar la ciudad, la Página principal y la firma del usuario, que aparecerán en sus comentarios del libro de visitas. Hay varias maneras de lograr esto:

- <strong>Agregue nuevas columnas a las</strong> tablas<strong>`aspnet_Users`</strong> <strong>o</strong> <strong>`aspnet_Membership`</strong> <strong>.</strong> No recomiendo este enfoque porque modifica el esquema utilizado por el `SqlMembershipProvider`. Es posible que esta decisión vuelva a Haunt. Por ejemplo, ¿qué ocurre si una versión futura de ASP.NET usa un esquema de `SqlMembershipProvider` diferente. Microsoft puede incluir una herramienta para migrar los datos de ASP.NET 2,0 `SqlMembershipProvider` al nuevo esquema, pero si ha modificado el esquema ASP.NET 2,0 `SqlMembershipProvider`, tal conversión podría no ser posible.

- **Utilice ASP. El marco de trabajo de Perfil de la red, que define una propiedad de perfil para la ciudad de inicio, la Página principal y la firma.** ASP.NET incluye un marco de trabajo de perfil diseñado para almacenar datos adicionales específicos del usuario. Al igual que el marco de pertenencia, el marco de trabajo de perfil se genera sobre el modelo de proveedor. El .NET Framework se suministra con un `SqlProfileProvider` las completan almacena datos de perfil en una base de datos de SQL Server. De hecho, nuestra base de datos ya tiene la tabla usada por el `SqlProfileProvider` (`aspnet_Profile`), ya que se agregó al agregar los servicios de aplicación en <a id="_msoanchor_2"> </a>el tutorial [*crear el esquema de pertenencia en SQL Server*](creating-the-membership-schema-in-sql-server-cs.md) .   
  La principal ventaja de Profile Framework es que permite a los desarrolladores definir las propiedades de perfil en `Web.config`: no es necesario escribir código para serializar los datos de perfil en el almacén de datos subyacente y desde éste. En Resumen, es increíblemente fácil definir un conjunto de propiedades de perfil y trabajar con ellas en el código. Sin embargo, el sistema de perfiles deja de ser deseable cuando se trata de control de versiones, por lo que si tiene una aplicación en la que espera que se agreguen nuevas propiedades específicas del usuario en un momento posterior, o las existentes que se van a quitar o modificar, es posible que el marco de trabajo de perfil no sea el  mejor opción. Además, el `SqlProfileProvider` almacena las propiedades de Perfil de una manera muy desnormalizada, lo que le resultará imposible ejecutar consultas directamente en los datos de perfil (como, por ejemplo, cuántos usuarios tienen una ciudad de Nueva York).   
  Para obtener más información sobre el marco de trabajo de perfil, consulte la sección "Lecturas adicionales" al final de este tutorial.

- <strong>Agregue estas tres columnas a una nueva tabla de la base de datos y establezca una relación de uno a uno entre esta tabla y</strong> <strong>`aspnet_Users`</strong> <strong>.</strong> Este enfoque implica un poco más de trabajo que con el marco de trabajo de perfil, pero ofrece la máxima flexibilidad en la forma en que se modelan las propiedades de usuario adicionales en la base de datos. Esta es la opción que se usará en este tutorial.

Se creará una nueva tabla denominada `UserProfiles` para guardar la ciudad, la Página principal y la firma de cada usuario. Haga clic con el botón derecho en la carpeta tablas de la ventana de Explorador de bases de datos y elija crear una nueva tabla. Asigne a la primera columna el nombre `UserId` y establezca su tipo en `uniqueidentifier`. No permitir valores `NULL` y marcar la columna como clave principal. A continuación, agregue columnas denominadas: `HomeTown` del tipo `nvarchar(50)`; `HomepageUrl` de tipo `nvarchar(100)`; y la firma del tipo `nvarchar(500)`. Cada una de estas tres columnas puede aceptar un valor `NULL`.

[![crear la tabla UserProfiles](storing-additional-user-information-cs/_static/image17.png)](storing-additional-user-information-cs/_static/image16.png)

**Figura 6**: creación de la tabla de `UserProfiles` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image18.png))

Guarde la tabla y asígnele el nombre `UserProfiles`. Por último, establezca una restricción FOREIGN KEY entre el campo `UserId` de la tabla `UserProfiles` y el campo `aspnet_Users.UserId`. Como hicimos con la restricción FOREIGN KEY entre las tablas `GuestbookComments` y `aspnet_Users`, la restricción se elimina en cascada. Puesto que el campo de `UserId` de `UserProfiles` es la clave principal, garantiza que no habrá más de un registro en la tabla de `UserProfiles` para cada cuenta de usuario. Este tipo de relación se conoce como uno a uno.

Ahora que hemos creado el modelo de datos, estamos listos para usarlo. En los pasos 2 y 3, veremos cómo el usuario que ha iniciado sesión actualmente puede ver y editar la información de la ciudad, la Página principal y la firma. En el paso 4, crearemos la interfaz para que los usuarios autenticados envíen comentarios nuevos al libro de visitas y vean los existentes.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Paso 2: Mostrar la ciudad, la Página principal y la firma del usuario

Hay varias maneras de permitir que el usuario que ha iniciado sesión tenga que ver y editar la información de la ciudad, la Página principal y la firma. Podríamos crear manualmente la interfaz de usuario con controles de cuadro de texto y etiqueta, o podríamos usar uno de los controles Web de datos, como el control DetailsView. Para realizar la `SELECT` de base de datos y las instrucciones de `UPDATE` podríamos escribir código ADO.NET en la clase de código subyacente de la página o, como alternativa, emplear un enfoque declarativo con SqlDataSource. Idealmente, nuestra aplicación contiene una arquitectura en capas, que podríamos invocar mediante programación desde la clase de código subyacente de la página o mediante declaración a través del control ObjectDataSource.

Dado que esta serie de tutoriales se centra en la autenticación de formularios, la autorización, las cuentas de usuario y los roles, no habrá un análisis exhaustivo de estas distintas opciones de acceso a datos o por qué es preferible usar una arquitectura en capas para ejecutar instrucciones SQL directamente. en la página ASP.NET. Voy a recorrer el uso de DetailsView y SqlDataSource, la opción más rápida y más sencilla, pero los conceptos tratados se pueden aplicar ciertamente a controles Web alternativos y lógica de acceso a datos. Para obtener más información sobre cómo trabajar con datos en ASP.NET, consulte la serie de tutoriales sobre cómo *[trabajar con datos en ASP.NET 2,0](../../data-access/index.md)* .

Abra la página `AdditionalUserInfo.aspx` en la carpeta `Membership` y agregue un control DetailsView a la página, estableciendo su propiedad `ID` en `UserProfile` y borrando sus propiedades `Width` y `Height`. Expanda la etiqueta inteligente de DetailsView y elija enlazarla a un nuevo control de origen de datos. Se iniciará el Asistente para configuración de DataSource (vea la figura 7). El primer paso le pide que especifique el tipo de origen de datos. Como vamos a conectar directamente a la base de datos de `SecurityTutorials`, elija el icono de base de datos, especificando el `ID` como `UserProfileDataSource`.

[![agregar un nuevo control SqlDataSource denominado UserProfileDataSource](storing-additional-user-information-cs/_static/image20.png)](storing-additional-user-information-cs/_static/image19.png)

**Figura 7**: agregar un nuevo control SqlDataSource denominado `UserProfileDataSource` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image21.png))

En la pantalla siguiente se solicita que se use la base de datos. Ya hemos definido una cadena de conexión en `Web.config` para la base de datos de `SecurityTutorials`. Este nombre de cadena de conexión (`SecurityTutorialsConnectionString`) debe estar en la lista desplegable. Seleccione esta opción y haga clic en siguiente.

[![elija SecurityTutorialsConnectionString en la lista desplegable](storing-additional-user-information-cs/_static/image23.png)](storing-additional-user-information-cs/_static/image22.png)

**Figura 8**: elija `SecurityTutorialsConnectionString` de la lista desplegable ([haga clic para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image24.png))

La pantalla siguiente nos pide que especifique la tabla y las columnas que se van a consultar. Elija la `UserProfiles` tabla en la lista desplegable y seleccione todas las columnas.

[![devolver todas las columnas de la tabla UserProfiles](storing-additional-user-information-cs/_static/image26.png)](storing-additional-user-information-cs/_static/image25.png)

**Ilustración 9**: devolver todas las columnas de la tabla de `UserProfiles` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image27.png))

La consulta actual de la figura 9 devuelve *todos* los registros de `UserProfiles`, pero solo nos interesa el registro del usuario que ha iniciado sesión actualmente. Para agregar una cláusula de `WHERE`, haga clic en el botón `WHERE` para abrir el cuadro de diálogo Agregar cláusula de `WHERE` (vea la figura 10). Aquí puede seleccionar la columna por la que desea filtrar, el operador y el origen del parámetro de filtro. Seleccione `UserId` como columna y "=" como operador.

Desafortunadamente, no hay ningún origen de parámetro integrado para devolver el valor `UserId` del usuario que ha iniciado sesión actualmente. Deberá tomar este valor mediante programación. Por lo tanto, establezca la lista desplegable origen en "ninguno", haga clic en el botón Agregar para agregar el parámetro y, a continuación, haga clic en Aceptar.

[![agregar un parámetro de filtro en la columna UserId](storing-additional-user-information-cs/_static/image29.png)](storing-additional-user-information-cs/_static/image28.png)

**Figura 10**: agregar un parámetro de filtro en la columna `UserId` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image30.png))

Después de hacer clic en aceptar, se le devolverá a la pantalla que se muestra en la figura 9. Sin embargo, esta vez, la consulta SQL en la parte inferior de la pantalla debe incluir una cláusula `WHERE`. Haga clic en siguiente para pasar a la pantalla "consulta de prueba". Aquí puede ejecutar la consulta y ver los resultados. Haga clic en Finalizar para completar el asistente.

Al completar el Asistente para configuración de DataSource, Visual Studio crea el control SqlDataSource según la configuración especificada en el asistente. Además, agrega BoundFields manualmente a DetailsView para cada columna devuelta por la `SelectCommand`de SqlDataSource. No es necesario mostrar el `UserId` campo en DetailsView, ya que el usuario no necesita conocer este valor. Puede quitar este campo directamente del marcado declarativo del control DetailsView o haciendo clic en el vínculo "editar campos" de su etiqueta inteligente.

En este momento, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample1.aspx)]

Es necesario establecer mediante programación el parámetro `UserId` del control SqlDataSource en el `UserId` del usuario que ha iniciado sesión actualmente antes de que se seleccionen los datos. Para ello, puede crear un controlador de eventos para el evento de `Selecting` de SqlDataSource y agregar el código siguiente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample2.cs)]

El código anterior comienza mediante la obtención de una referencia al usuario que ha iniciado la sesión actual llamando al método `GetUser` de la clase `Membership`. Esto devuelve un objeto `MembershipUser`, cuya propiedad `ProviderUserKey` contiene la `UserId`. A continuación, se asigna el valor `UserId` al parámetro `@UserId` de SqlDataSource.

> [!NOTE]
> El método `Membership.GetUser()` devuelve información sobre el usuario que ha iniciado sesión actualmente. Si un usuario anónimo visita la página, devolverá un valor de `null`. En tal caso, esto dará lugar a un `NullReferenceException` en la siguiente línea de código al intentar leer la propiedad `ProviderUserKey`. Por supuesto, no tenemos que preocuparse por `Membership.GetUser()` devolver un valor `null` en la página `AdditionalUserInfo.aspx` porque se configuró la autorización de la dirección URL en un tutorial anterior para que solo los usuarios autenticados pudieran acceder a los recursos de ASP.NET en esta carpeta. Si necesita obtener acceso a información sobre el usuario que ha iniciado sesión actualmente en una página donde se permite el acceso anónimo, asegúrese de comprobar que el método `GetUser()` devuelva un objeto que no sea de`null MembershipUser` antes de hacer referencia a sus propiedades.

Si visita la página `AdditionalUserInfo.aspx` a través de un explorador, verá una página en blanco porque todavía hemos agregado filas a la tabla `UserProfiles`. En el paso 6, veremos cómo personalizar el control CreateUserWizard para agregar automáticamente una nueva fila a la tabla `UserProfiles` cuando se crea una nueva cuenta de usuario. Sin embargo, por ahora, deberá crear manualmente un registro en la tabla.

Navegue hasta el Explorador de bases de datos en Visual Studio y expanda la carpeta tablas. Haga clic con el botón secundario en la tabla `aspnet_Users` y elija "Mostrar datos de tabla" para ver los registros de la tabla. Haga lo mismo para la tabla `UserProfiles`. En la figura 11 se muestran estos resultados cuando se organizan en mosaico verticalmente. En la base de datos, actualmente hay `aspnet_Users` registros para Bruce, Fred y Tito, pero no registros en la tabla `UserProfiles`.

[![se muestran los contenidos de las tablas aspnet_Users y UserProfiles](storing-additional-user-information-cs/_static/image32.png)](storing-additional-user-information-cs/_static/image31.png)

**Figura 11**: se muestra el contenido de las tablas `aspnet_Users` y `UserProfiles` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image33.png))

Agregue un nuevo registro a la tabla `UserProfiles` escribiendo manualmente los valores de los campos `HomeTown`, `HomepageUrl`y `Signature`. La forma más fácil de obtener un valor de `UserId` válido en el nuevo registro de `UserProfiles` es seleccionar el campo de `UserId` de una cuenta de usuario determinada en la tabla de `aspnet_Users` y copiarlo y pegarlo en el campo `UserId` de `UserProfiles`. En la figura 12 se muestra la tabla `UserProfiles` una vez que se ha agregado un nuevo registro para Bruce.

[![se agregó un registro a UserProfiles para Bruce](storing-additional-user-information-cs/_static/image35.png)](storing-additional-user-information-cs/_static/image34.png)

**Figura 12**: se ha agregado un registro a `UserProfiles` para Bruce ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image36.png))

Vuelva a la página `AdditionalUserInfo.aspx`, que ha iniciado sesión como Bruce. Como se muestra en la figura 13, se muestra la configuración de Bruce.

[![el usuario que está visitando actualmente se muestra su configuración](storing-additional-user-information-cs/_static/image38.png)](storing-additional-user-information-cs/_static/image37.png)

**Figura 13**: el usuario que está visitando actualmente se muestra en su configuración ([haga clic para ver la imagen a tamaño completo](storing-additional-user-information-cs/_static/image39.png))

> [!NOTE]
> Continúe y agregue manualmente los registros en la tabla de `UserProfiles` para cada usuario de pertenencia. En el paso 6, veremos cómo personalizar el control CreateUserWizard para agregar automáticamente una nueva fila a la tabla `UserProfiles` cuando se crea una nueva cuenta de usuario.

## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Paso 3: permitir que el usuario edite su ciudad, Página principal y firma

En este momento, el usuario que ha iniciado sesión puede ver su ciudad de inicio, Página principal y configuración de firma, pero aún no pueden modificarlos. Vamos a actualizar el control DetailsView para que se puedan editar los datos.

Lo primero que debemos hacer es agregar un `UpdateCommand` para SqlDataSource, especificando la instrucción `UPDATE` que se va a ejecutar y sus parámetros correspondientes. Seleccione SqlDataSource y, en el ventana Propiedades, haga clic en los puntos suspensivos junto a la propiedad UpdateQuery para abrir el cuadro de diálogo Editor de parámetros y comandos. Escriba la siguiente instrucción de `UPDATE` en el cuadro de texto:

[!code-sql[Main](storing-additional-user-information-cs/samples/sample3.sql)]

A continuación, haga clic en el botón "actualizar parámetros", que creará un parámetro en la colección de `UpdateParameters` del control SqlDataSource para cada uno de los parámetros de la instrucción `UPDATE`. Deje el origen de todos los parámetros establecidos en ninguno y haga clic en el botón Aceptar para completar el cuadro de diálogo.

[![especificar UpdateCommand y UpdateParameters de SqlDataSource](storing-additional-user-information-cs/_static/image41.png)](storing-additional-user-information-cs/_static/image40.png)

**Figura 14**: especificación de los `UpdateCommand` y `UpdateParameters` de SqlDataSource ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image42.png))

Debido a las adiciones realizadas al control SqlDataSource, el control DetailsView ahora puede admitir la edición. En la etiqueta inteligente de DetailsView, active la casilla "habilitar edición". Esto agrega un CommandField a la colección de `Fields` del control con su propiedad `ShowEditButton` establecida en true. Esto representa un botón Editar cuando se muestra DetailsView en modo de solo lectura y botones actualizar y cancelar cuando se muestra en modo de edición. Sin embargo, en lugar de solicitar al usuario que haga clic en editar, podemos hacer que DetailsView se represente en un estado "siempre editable" estableciendo la [propiedad`DefaultMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) del control detailsview en `Edit`.

Con estos cambios, el marcado declarativo del control DetailsView debe ser similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample4.aspx)]

Observe la adición de CommandField y la propiedad `DefaultMode`.

Continúe y pruebe esta página a través de un explorador. Al visitar con un usuario que tiene un registro correspondiente en `UserProfiles`, la configuración del usuario se muestra en una interfaz editable.

[![DetailsView representa una interfaz modificable](storing-additional-user-information-cs/_static/image44.png)](storing-additional-user-information-cs/_static/image43.png)

**Figura 15**: DetailsView representa una interfaz modificable ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image45.png))

Intente cambiar los valores y haga clic en el botón actualizar. Aparece como si no sucede nada. Hay un postback y los valores se guardan en la base de datos, pero no hay ningún comentario visual de que se haya guardado.

Para solucionarlo, vuelva a Visual Studio y agregue un control etiqueta sobre DetailsView. Establezca su `ID` en `SettingsUpdatedMessage`, su propiedad `Text` en "la configuración se ha actualizado" y sus propiedades `Visible` y `EnableViewState` en `false`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample5.aspx)]

Es necesario mostrar la etiqueta de `SettingsUpdatedMessage` cada vez que se actualiza DetailsView. Para ello, cree un controlador de eventos para el evento `ItemUpdated` de DetailsView y agregue el código siguiente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample6.cs)]

Vuelva a la página `AdditionalUserInfo.aspx` a través de un explorador y actualice los datos. Esta vez, se muestra un mensaje de estado de utilidad.

[![se muestra un mensaje breve cuando se actualiza la configuración](storing-additional-user-information-cs/_static/image47.png)](storing-additional-user-information-cs/_static/image46.png)

**Figura 16**: se muestra un mensaje breve cuando se actualiza la configuración ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image48.png))

> [!NOTE]
> La interfaz de edición del control DetailsView deja mucho que se desee. Usa cuadros de texto de tamaño estándar, pero el campo de firma debería ser probablemente un cuadro de texto de varias líneas. Se debe utilizar un RegularExpressionValidator para asegurarse de que la dirección URL de la Página principal, si se escribe, comienza por "http://" o "https://". Además, puesto que el control DetailsView tiene su propiedad `DefaultMode` establecida en `Edit`, el botón Cancelar no hace nada. Se debe quitar o, cuando se hace clic en él, redirigir al usuario a otra página (por ejemplo, `~/Default.aspx`). Dejo estas mejoras como un ejercicio para el lector.

### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Agregar un vínculo a la página de`AdditionalUserInfo.aspx`en la página maestra

Actualmente, el sitio web no proporciona ningún vínculo a la página `AdditionalUserInfo.aspx`. La única manera de llegar a él es escribir la dirección URL de la página directamente en la barra de direcciones del explorador. Vamos a agregar un vínculo a esta página en la página maestra de `Site.master`.

Recuerde que la página maestra contiene un control Web LoginView en su `LoginContent` ContentPlaceHolder que muestra un marcado diferente para los visitantes autenticados y anónimos. Actualice el `LoggedInTemplate` del control LoginView para incluir un vínculo a la página de `AdditionalUserInfo.aspx`. Después de realizar estos cambios, el marcado declarativo del control LoginView debe ser similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample7.aspx)]

Observe la adición de `lnkUpdateSettings` control de hipervínculo al `LoggedInTemplate`. Con este vínculo en su lugar, los usuarios autenticados pueden saltar rápidamente a la página para ver y modificar la configuración de la ciudad, la Página principal y la firma.

## <a name="step-4-adding-new-guestbook-comments"></a>Paso 4: agregar nuevos comentarios del libro de visitas

La página `Guestbook.aspx` es donde los usuarios autenticados pueden ver el libro de visitas y dejar un comentario. Comencemos con la creación de la interfaz para agregar nuevos comentarios del libro de visitas.

Abra la página `Guestbook.aspx` de Visual Studio y construya una interfaz de usuario formada por dos controles de cuadro de texto, uno para el asunto del nuevo comentario y otro para su cuerpo. Establezca la propiedad `ID` del primer control de cuadro de texto en `Subject` y su propiedad `Columns` en 40; Establezca el `ID` de segundo en `Body`, su `TextMode` en `MultiLine`y sus propiedades `Width` y `Rows` en "95%" y 8, respectivamente. Para completar la interfaz de usuario, agregue un control de botón Web denominado `PostCommentButton` y establezca su propiedad `Text` en "publicar comentario".

Como cada comentario del libro de visitas requiere un asunto y un cuerpo, agregue un RequiredFieldValidator para cada uno de los cuadros de texto. Establezca la propiedad `ValidationGroup` de estos controles en "EnterComment". del mismo modo, establezca la propiedad `ValidationGroup` del control `PostCommentButton` en "EnterComment". Para obtener más información acerca de ASP. Controles de validación de la red, consulte [validación de formularios en ASP.net](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [desseccionando los controles de validación en ASP.net 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)y el [tutorial de controles de servidor de validación](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) en [w3schools](http://www.w3schools.com/).

Después de crear la interfaz de usuario, el marcado declarativo de la página debe tener un aspecto similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample8.aspx)]

Una vez completada la interfaz de usuario, la siguiente tarea consiste en insertar un nuevo registro en la tabla `GuestbookComments` cuando se hace clic en el `PostCommentButton`. Esto puede realizarse de varias maneras: podemos escribir código ADO.NET en el controlador de eventos `Click` del botón; podemos agregar un control SqlDataSource a la página, configurar su `InsertCommand`y, a continuación, llamar a su método `Insert` desde el controlador de eventos `Click`; o bien, podríamos crear un nivel intermedio que era responsable de insertar nuevos comentarios del libro de visitas e invocar esta funcionalidad desde el controlador de eventos `Click`. Como analizamos el uso de SqlDataSource en el paso 3, vamos a usar aquí el código ADO.NET.

> [!NOTE]
> Las clases ADO.NET que se usan para obtener acceso mediante programación a los datos de una base de datos Microsoft SQL Server se encuentran en el espacio de nombres `System.Data.SqlClient`. Es posible que deba importar este espacio de nombres en la clase de código subyacente de la página (es decir, `using System.Data.SqlClient;`).

Cree un controlador de eventos para el evento de `Click` del `PostCommentButton`y agregue el código siguiente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample9.cs)]

El controlador de eventos `Click` se inicia comprobando que los datos proporcionados por el usuario son válidos. Si no es así, el controlador de eventos se cierra antes de insertar un registro. Suponiendo que los datos proporcionados son válidos, el valor `UserId` del usuario que ha iniciado sesión actualmente se recupera y se almacena en la variable local `currentUserId`. Este valor es necesario porque debemos proporcionar un valor `UserId` al insertar un registro en `GuestbookComments`.

A continuación, se recupera la cadena de conexión para la base de datos `SecurityTutorials` de `Web.config` y se especifica la instrucción SQL `INSERT`. A continuación, se crea y se abre un objeto de `SqlConnection`. A continuación, se construye un objeto de `SqlCommand` y se asignan los valores de los parámetros usados en la consulta de `INSERT`. A continuación, se ejecuta la instrucción `INSERT` y se cierra la conexión. Al final del controlador de eventos, se borran las propiedades de `Text` de los cuadros de texto de `Body` y `Subject` para que los valores del usuario no se conserven en el PostBack.

Continúe y pruebe esta página en un explorador. Como esta página está en la carpeta `Membership`, no es accesible para los visitantes anónimos. Por lo tanto, tendrá que iniciar sesión primero (si aún no lo ha hecho). Escriba un valor en los cuadros de texto `Subject` y `Body` y haga clic en el botón `PostCommentButton`. Esto hará que se agregue un nuevo registro a `GuestbookComments`. En el postback, el asunto y el cuerpo que proporcionó se borran de los cuadros de texto.

Después de hacer clic en el botón `PostCommentButton`, no hay ningún comentario visual de que el comentario se haya agregado al libro de visitas. Todavía es necesario actualizar esta página para mostrar los comentarios del libro de visitas existentes, que haremos en el paso 5. Una vez hecho esto, el comentario recién agregado aparecerá en la lista de comentarios y proporcionará los comentarios visuales adecuados. Por ahora, confirme que el comentario del libro de visitas se ha guardado examinando el contenido de la tabla de `GuestbookComments`.

En la figura 17 se muestra el contenido de la tabla `GuestbookComments` una vez que se han dejado dos comentarios.

[![puede ver los comentarios del libro de visitas en la tabla GuestbookComments](storing-additional-user-information-cs/_static/image50.png)](storing-additional-user-information-cs/_static/image49.png)

**Figura 17**: puede ver los comentarios del libro de visitas en la tabla `GuestbookComments` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image51.png))

> [!NOTE]
> Si un usuario intenta insertar un comentario del libro de visitas que contenga un marcado potencialmente peligroso, como HTML – ASP.NET, producirá una `HttpRequestValidationException`. Para obtener más información sobre esta excepción, por qué se produce y cómo permitir a los usuarios enviar valores potencialmente peligrosos, consulte las [notas del producto sobre validación de solicitudes](../../../../whitepapers/request-validation.md).

## <a name="step-5-listing-the-existing-guestbook-comments"></a>Paso 5: enumeración de los comentarios del libro de visitas existentes

Además de mantener los comentarios, un usuario que visite la página `Guestbook.aspx` también debe poder ver los comentarios existentes del libro de visitas. Para ello, agregue un control ListView denominado `CommentList` a la parte inferior de la página.

> [!NOTE]
> El control ListView es nuevo en la versión 3,5 de ASP.NET. Está diseñada para mostrar una lista de elementos en un diseño muy personalizable y flexible, pero sigue ofreciendo funcionalidad integrada de edición, inserción, eliminación, paginación y ordenación como GridView. Si usa ASP.NET 2,0, tendrá que usar en su lugar el control DataList o Repeater. Para obtener más información sobre el uso de ListView, vea la entrada de blog de [Scott Guthrie](https://weblogs.asp.net/scottgu/), [el control ASP: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)y mi artículo, que [muestra los datos con el control ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Abra la etiqueta inteligente de ListView y, en la lista desplegable elegir origen de datos, enlace el control a un nuevo origen de datos. Como vimos en el paso 2, se iniciará el Asistente para la configuración de orígenes de datos. Seleccione el icono de base de datos, asigne un nombre al `CommentsDataSource`SqlDataSource resultante y haga clic en Aceptar. A continuación, seleccione la cadena de conexión de `SecurityTutorialsConnectionString` en la lista desplegable y haga clic en siguiente.

En este punto del paso 2 se especificaron los datos que se van a consultar seleccionando la tabla `UserProfiles` de la lista desplegable y seleccionando las columnas que se van a devolver (consulte la figura 9). Sin embargo, esta vez queremos crear una instrucción SQL que extraiga no solo los registros de `GuestbookComments`, sino también la ciudad, la Página principal, la firma y el nombre de usuario del comentario. Por lo tanto, seleccione el botón de radio "especificar una instrucción SQL personalizada o un procedimiento almacenado" y haga clic en siguiente.

Se abrirá la pantalla "definir instrucciones o procedimientos almacenados personalizados". Haga clic en el botón Generador de consultas para compilar la consulta de forma gráfica. El Generador de consultas comienza solicitando que se especifiquen las tablas de las que se desea realizar la consulta. Seleccione las tablas `GuestbookComments`, `UserProfiles`y `aspnet_Users` y haga clic en Aceptar. Se agregarán las tres tablas a la superficie de diseño. Dado que existen restricciones de clave externa entre las tablas `GuestbookComments`, `UserProfiles`y `aspnet_Users`, la Generador de consultas `JOIN` las tablas automáticamente.

Todo lo que queda es especificar las columnas que se van a devolver. En la tabla `GuestbookComments`, seleccione las columnas `Subject`, `Body`y `CommentDate`; Devuelve las columnas `HomeTown`, `HomepageUrl`y `Signature` de la tabla `UserProfiles`; y devuelven `UserName` de `aspnet_Users`. Además, agregue "`ORDER BY CommentDate DESC`" al final de la consulta `SELECT` para que se devuelvan primero las entradas más recientes. Después de efectuar estas selecciones, la interfaz de Generador de consultas debe ser similar a la captura de pantalla de la figura 18.

[![la consulta construida combina las tablas GuestbookComments, UserProfiles y aspnet_Users](storing-additional-user-information-cs/_static/image53.png)](storing-additional-user-information-cs/_static/image52.png)

**Figura 18**: la consulta construida `JOIN` s las tablas `GuestbookComments`, `UserProfiles`y `aspnet_Users` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image54.png))

Haga clic en Aceptar para cerrar la ventana de Generador de consultas y volver a la pantalla "definir instrucciones o procedimientos almacenados personalizados". Haga clic en siguiente para avanzar a la pantalla "consulta de prueba", donde puede ver los resultados de la consulta haciendo clic en el botón probar consulta. Cuando esté listo, haga clic en finalizar para completar el Asistente para configurar orígenes de datos.

Cuando se complete el Asistente para la configuración de orígenes de datos en el paso 2, la colección de `Fields` del control DetailsView asociado se actualizó para incluir una BoundField para cada columna devuelta por el `SelectCommand`. No obstante, el control ListView permanece sin cambios; todavía necesitamos definir el diseño. El diseño de ListView se puede construir manualmente a través de su marcado declarativo o de la opción "configurar ListView" en su etiqueta inteligente. Normalmente preferimos definir el marcado manualmente, pero usar el método que sea más natural.

He terminado usando los siguientes `LayoutTemplate`, `ItemTemplate`y `ItemSeparatorTemplate` para el control ListView:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample10.aspx)]

El `LayoutTemplate` define el marcado emitido por el control, mientras que el `ItemTemplate` representa cada elemento devuelto por SqlDataSource. El marcado resultante del `ItemTemplate`se coloca en el control de `itemPlaceholder` del `LayoutTemplate`. Además de la `itemPlaceholder`, el `LayoutTemplate` incluye un control de paginación de elementos, que limita el objeto ListView para mostrar solo 10 comentarios del libro de visitas por página (valor predeterminado) y representa una interfaz de paginación.

Mi `ItemTemplate` muestra el asunto de cada comentario del libro de visitas en un elemento `<h4>` con el cuerpo situado debajo del asunto. Tenga en cuenta que la sintaxis que se usa para mostrar el cuerpo toma los datos devueltos por el `Eval("Body")` instrucción DataBinding, los convierte en una cadena y reemplaza los saltos de línea por el elemento `<br />`. Esta conversión es necesaria para mostrar los saltos de línea especificados al enviar el comentario, ya que HTML omite el espacio en blanco. La firma del usuario se muestra debajo del cuerpo en cursiva, seguido de la ciudad del usuario, un vínculo a su página principal, la fecha y la hora en que se realizó el comentario y el nombre de usuario de la persona que abandonó el comentario.

Tómese un momento para ver la página a través de un explorador. Debería ver los comentarios que agregó al libro de visitas en el paso 5 que se muestra aquí.

[![el libro de visitas. aspx muestra ahora los comentarios del libro de visitas](storing-additional-user-information-cs/_static/image56.png)](storing-additional-user-information-cs/_static/image55.png)

**Figura 19**: `Guestbook.aspx` ahora muestra los comentarios del libro de visitas ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image57.png))

Intente agregar un nuevo comentario al libro de visitas. Al hacer clic en el botón `PostCommentButton`, la página devuelve datos y el comentario se agrega a la base de datos, pero el control ListView no se actualiza para mostrar el nuevo comentario. Esto puede solucionarse de una de estas dos acciones:

- Actualizar el controlador de eventos del `Click` del botón `PostCommentButton` para que invoque el método `DataBind()` del control ListView después de insertar el nuevo comentario en la base de datos, o bien
- Establecer la propiedad `EnableViewState` del control ListView en `false`. Este enfoque funciona porque al deshabilitar el estado de vista del control, se debe volver a enlazar a los datos subyacentes en cada postback.

En el sitio web del tutorial descargable de este tutorial se muestran ambas técnicas. La propiedad `EnableViewState` del control ListView para `false` y el código necesario para volver a enlazar los datos mediante programación al control ListView están presentes en el controlador de eventos `Click`, pero se marca como comentario.

> [!NOTE]
> Actualmente, la página `AdditionalUserInfo.aspx` permite al usuario ver y editar la configuración de la ciudad, la Página principal y la firma. Puede ser conveniente actualizar `AdditionalUserInfo.aspx` para mostrar los comentarios del libro de visitas del usuario que ha iniciado sesión. Es decir, además de examinar y modificar su información, un usuario puede visitar la página de `AdditionalUserInfo.aspx` para ver qué comentarios del libro de visitas ha realizado en el pasado. Dejo esto como un ejercicio para el lector interesado.

## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Paso 6: personalización del control CreateUserWizard para incluir una interfaz para la ciudad, la Página principal y la firma del hogar

La consulta `SELECT` utilizada por la página `Guestbook.aspx` utiliza una `INNER JOIN` para combinar los registros relacionados entre las tablas `GuestbookComments`, `UserProfiles`y `aspnet_Users`. Si un usuario que no tiene ningún registro en `UserProfiles` crea un comentario del libro de visitas, el comentario no se mostrará en el control ListView porque el `INNER JOIN` solo devuelve `GuestbookComments` registros cuando hay registros coincidentes en `UserProfiles` y `aspnet_Users`. Como vimos en el paso 3, si un usuario no tiene un registro en `UserProfiles` no puede ver ni editar su configuración en la página `AdditionalUserInfo.aspx`.

No es necesario decir que, debido a nuestras decisiones de diseño, es importante que cada cuenta de usuario del sistema de pertenencia tenga un registro coincidente en la tabla `UserProfiles`. Lo que queremos es que se agregue un registro correspondiente a `UserProfiles` siempre que se cree una nueva cuenta de usuario de pertenencia a través de CreateUserWizard.

Como se describe en el tutorial [*creación de cuentas de usuario*](creating-user-accounts-cs.md) , una vez creada la nueva cuenta de usuario de pertenencia, el control CreateUserWizard genera su [evento`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). Podemos crear un controlador de eventos para este evento, obtener el identificador de usuario del usuario que se acaba de crear y, a continuación, insertar un registro en la tabla `UserProfiles` con valores predeterminados para las columnas `HomeTown`, `HomepageUrl`y `Signature`. Lo que es más, es posible pedir al usuario estos valores mediante la personalización de la interfaz del control CreateUserWizard para incluir cuadros de texto adicionales.

Veamos primero cómo agregar una nueva fila a la tabla `UserProfiles` del controlador de eventos `CreatedUser` con valores predeterminados. A continuación, veremos cómo personalizar la interfaz de usuario del control CreateUserWizard para incluir campos de formulario adicionales para recopilar la ciudad, la Página principal y la firma del nuevo usuario.

### <a name="adding-a-default-row-touserprofiles"></a>Agregar una fila predeterminada a`UserProfiles`

En el tutorial [*creación de cuentas de usuario*](creating-user-accounts-cs.md) se ha agregado un control CreateUserWizard a la página `CreatingUserAccounts.aspx` de la carpeta `Membership`. Para que el control CreateUserWizard agregue un registro a `UserProfiles` tabla tras la creación de la cuenta de usuario, es necesario actualizar la funcionalidad del control CreateUserWizard. En lugar de realizar estos cambios en la página `CreatingUserAccounts.aspx`, vamos a agregar un nuevo control CreateUserWizard a `EnhancedCreateUserWizard.aspx` página y realizar las modificaciones en este tutorial.

Abra la página `EnhancedCreateUserWizard.aspx` de Visual Studio y arrastre un control CreateUserWizard desde el cuadro de herramientas hasta la página. Establezca la propiedad `ID` del control CreateUserWizard en `NewUserWizard`. Como se explicó en el <a id="_msoanchor_5"> </a>tutorial [*creación de cuentas de usuario*](creating-user-accounts-cs.md) , la interfaz de usuario predeterminada de CreateUserWizard solicita al visitante la información necesaria. Una vez que se ha proporcionado esta información, el control crea internamente una nueva cuenta de usuario en el marco de pertenencia, todo sin nosotros tenemos que escribir una sola línea de código.

El control CreateUserWizard genera una serie de eventos durante su flujo de trabajo. Después de que un visitante proporcione la información de la solicitud y envíe el formulario, el control CreateUserWizard activa inicialmente su [evento`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Si se produce un problema durante el proceso de creación, se desencadena el [evento`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) . sin embargo, si el usuario se crea correctamente, se genera el [evento`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) . En el <a id="_msoanchor_6"> </a>tutorial [*creación de cuentas de usuario*](creating-user-accounts-cs.md) , creamos un controlador de eventos para el evento `CreatingUser` para asegurarnos de que el nombre de usuario proporcionado no contenía ningún espacio inicial o final, y que el nombre de usuario no aparecía en ninguna parte de la contraseña.

Para agregar una fila en la tabla de `UserProfiles` para el usuario que acaba de crear, es necesario crear un controlador de eventos para el evento `CreatedUser`. En el momento en que se ha desencadenado el evento de `CreatedUser`, la cuenta de usuario ya se ha creado en el marco de pertenencia, lo que nos permite recuperar el valor de UserId de la cuenta.

Cree un controlador de eventos para el evento de `CreatedUser` del `NewUserWizard`y agregue el código siguiente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample11.cs)]

En el código anterior, se recupera el UserId de la cuenta de usuario recién agregada. Esto se consigue mediante el método `Membership.GetUser(username)` para devolver información sobre un usuario determinado y, a continuación, utilizar la propiedad `ProviderUserKey` para recuperar su UserId. El nombre de usuario especificado por el usuario en el control CreateUserWizard está disponible a través de su [propiedad`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

A continuación, se recupera la cadena de conexión de `Web.config` y se especifica la instrucción `INSERT`. Se crean instancias de los objetos ADO.NET necesarios y se ejecuta el comando. El código asigna una instancia de [`DBNull`](https://msdn.microsoft.com/library/system.dbnull.aspx) a los parámetros `@HomeTown`, `@HomepageUrl`y `@Signature`, lo que tiene el efecto de insertar valores de `NULL` de base de datos para los campos `HomeTown`, `HomepageUrl`y `Signature`.

Visite la página `EnhancedCreateUserWizard.aspx` a través de un explorador y cree una nueva cuenta de usuario. Después de hacerlo, vuelva a Visual Studio y examine el contenido de las tablas `aspnet_Users` y `UserProfiles` (como hemos vuelto en la figura 12). Debería ver la nueva cuenta de usuario en `aspnet_Users` y una fila de `UserProfiles` correspondiente (con `NULL` valores para `HomeTown`, `HomepageUrl`y `Signature`).

[![se han agregado una nueva cuenta de usuario y un registro UserProfiles](storing-additional-user-information-cs/_static/image59.png)](storing-additional-user-information-cs/_static/image58.png)

**Figura 20**: se ha agregado una nueva cuenta de usuario y un registro de `UserProfiles` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image60.png))

Una vez que el visitante ha proporcionado la información de la nueva cuenta y ha haga clic en el botón "crear usuario", se crea la cuenta de usuario y se agrega una fila a la tabla de `UserProfiles`. Después, CreateUserWizard muestra su `CompleteWizardStep`, que muestra un mensaje de operación correcta y un botón continuar. Al hacer clic en el botón continuar se produce un postback, pero no se realiza ninguna acción, lo que deja el usuario atascado en la página `EnhancedCreateUserWizard.aspx`.

Podemos especificar una dirección URL a la que enviar al usuario cuando se haga clic en el botón Continuar mediante la [propiedad`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)del control CreateUserWizard. Establezca la propiedad `ContinueDestinationPageUrl` en "~/Membership/AdditionalUserInfo.aspx". Esto hace que el nuevo usuario `AdditionalUserInfo.aspx`, donde puede ver y actualizar su configuración.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personalización de la interfaz de CreateUserWizard para solicitar la ciudad, la Página principal y la firma del nuevo usuario

La interfaz predeterminada del control CreateUserWizard es suficiente para escenarios de creación de cuentas simples en los que solo es necesario recopilar información básica de la cuenta de usuario, como el nombre de usuario, la contraseña y el correo electrónico. Pero ¿qué ocurre si quisiéramos pedir al visitante que escriba su ciudad, Página principal y firma al crear su cuenta? Es posible personalizar la interfaz del control CreateUserWizard para recopilar información adicional en el registro, y esta información se puede usar en el controlador de eventos `CreatedUser` para insertar registros adicionales en la base de datos subyacente.

El control CreateUserWizard extiende el control del Asistente para ASP.NET, que es un control que permite a un desarrollador de páginas definir una serie de `WizardSteps`ordenados. El control del asistente representa el paso activo y proporciona una interfaz de navegación que permite al visitante desplazarse por estos pasos. El control Wizard es idóneo para desglosar una tarea larga en varios pasos breves. Para obtener más información sobre el control del asistente, vea [crear una interfaz de usuario paso a paso con el control del asistente de ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

El marcado predeterminado del control CreateUserWizard define dos `WizardSteps`: `CreateUserWizardStep` y `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample12.aspx)]

La primera `WizardStep`, `CreateUserWizardStep`, representa la interfaz que solicita el nombre de usuario, la contraseña, el correo electrónico, etc. Después de que el visitante proporcione esta información y haga clic en "crear usuario", se muestra el `CompleteWizardStep`, que muestra el mensaje de operación correcta y un botón continuar.

Para personalizar la interfaz del control CreateUserWizard para incluir campos de formulario adicionales, podemos:

- <strong>Cree una o varias nuevas</strong> <strong>`WizardStep`</strong> <strong>s para contener los elementos de la interfaz de usuario adicionales</strong>. Para agregar un nuevo `WizardStep` a CreateUserWizard, haga clic en el vínculo "Agregar o quitar `WizardSteps`" de su etiqueta inteligente para iniciar el editor de la colección de `WizardStep`. Desde allí puede Agregar, quitar o cambiar el orden de los pasos del asistente. Este es el enfoque que se utilizará para este tutorial.

- <strong>Convierta el</strong> <strong>`CreateUserWizardStep`</strong> <strong>en un`WizardStep`modificable</strong> <strong>.</strong> Esto reemplaza el `CreateUserWizardStep` por una `WizardStep` equivalente cuyo marcado define una interfaz de usuario que coincide con la del `CreateUserWizardStep`. Al convertir el `CreateUserWizardStep` en un `WizardStep` podemos cambiar la posición de los controles o agregar elementos de la interfaz de usuario adicionales a este paso. Para convertir el `CreateUserWizardStep` o `CompleteWizardStep` en un `WizardStep`editable, haga clic en el vínculo "personalizar el paso crear usuario" o "personalizar el paso completo" de la etiqueta inteligente del control.

- **Use alguna combinación de las dos opciones anteriores.**

Es importante tener en cuenta que el control CreateUserWizard ejecuta el proceso de creación de cuentas de usuario cuando se hace clic en el botón "crear usuario" desde el `CreateUserWizardStep`. No importa si hay `WizardStep` adicionales después del `CreateUserWizardStep` o no.

Al agregar un `WizardStep` personalizado al control CreateUserWizard para recopilar datos proporcionados por el usuario, el `WizardStep` personalizado puede colocarse antes o después de la `CreateUserWizardStep`. Si se encuentra antes de la `CreateUserWizardStep`, la entrada de usuario adicional recopilada de la `WizardStep` personalizada está disponible para el controlador de eventos `CreatedUser`. Sin embargo, si el `WizardStep` personalizado se produce después `CreateUserWizardStep`, en el momento en que se muestra el `WizardStep` personalizado, ya se ha creado la nueva cuenta de usuario y ya se ha activado el evento de `CreatedUser`.

La figura 21 muestra el flujo de trabajo cuando el `WizardStep` agregado precede a la `CreateUserWizardStep`. Dado que la información de usuario adicional se ha recopilado en el momento en que se activa el evento de `CreatedUser`, lo único que debemos hacer es actualizar el controlador de eventos de `CreatedUser` para recuperar estas entradas y utilizarlas para los valores de parámetro de la instrucción `INSERT` (en lugar de `DBNull.Value`).

[![el flujo de trabajo de CreateUserWizard cuando un WizardStep adicional precede a CreateUserWizardStep](storing-additional-user-information-cs/_static/image62.png)](storing-additional-user-information-cs/_static/image61.png)

**Figura 21**: el flujo de trabajo de CreateUserWizard cuando un `WizardStep` adicional precede al `CreateUserWizardStep` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image63.png))

Sin embargo, si el `WizardStep` personalizado se coloca *después* del `CreateUserWizardStep`, se produce el proceso de creación de cuenta de usuario antes de que el usuario haya tenido la oportunidad de escribir su ciudad, Página principal o firma. En tal caso, se debe insertar esta información adicional en la base de datos después de que se haya creado la cuenta de usuario, como se muestra en la figura 22.

[![el flujo de trabajo de CreateUserWizard cuando un WizardStep adicional aparece después de CreateUserWizardStep](storing-additional-user-information-cs/_static/image65.png)](storing-additional-user-information-cs/_static/image64.png)

**Figura 22**: el flujo de trabajo de CreateUserWizard cuando un `WizardStep` adicional aparece después del `CreateUserWizardStep` ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image66.png))

El flujo de trabajo que se muestra en la figura 22 espera a que se inserte un registro en la tabla de `UserProfiles` hasta que finalice el paso 2. Sin embargo, si el visitante cierra su explorador después del paso 1, se habrá alcanzado un estado en el que se ha creado una cuenta de usuario, pero no se ha agregado ningún registro a `UserProfiles`. Una solución consiste en tener un registro con `NULL` o valores predeterminados insertados en `UserProfiles` en el controlador de eventos de `CreatedUser` (que se desencadena después del paso 1) y, a continuación, actualizar este registro una vez completado el paso 2. Esto garantiza que se agregará un registro de `UserProfiles` para la cuenta de usuario, incluso si el usuario sale del proceso de registro a través de.

En este tutorial vamos a crear un nuevo `WizardStep` que se produce después del `CreateUserWizardStep` pero antes de la `CompleteWizardStep`. En primer lugar, vamos a obtener el WizardStep y configurarlo y, a continuación, veremos el código.

En la etiqueta inteligente del control CreateUserWizard, seleccione "Agregar o quitar `WizardStep` s", que abre el cuadro de diálogo Editor de la colección `WizardStep`. Agregue un nuevo `WizardStep`, establezca su `ID` en `UserSettings`, su `Title` en "su configuración" y su `StepType` en `Step`. A continuación, colóquelo para que venga después del `CreateUserWizardStep` ("Regístrese para obtener una nueva cuenta") y antes de la `CompleteWizardStep` ("completa"), como se muestra en la Figura 23.

[![agregar un nuevo WizardStep al control CreateUserWizard](storing-additional-user-information-cs/_static/image68.png)](storing-additional-user-information-cs/_static/image67.png)

**Figura 23**: agregar un nuevo `WizardStep` al control CreateUserWizard ([haga clic para ver la imagen de tamaño completo](storing-additional-user-information-cs/_static/image69.png))

Haga clic en Aceptar para cerrar el cuadro de diálogo Editor de la colección de `WizardStep`. El nuevo `WizardStep` se evidencia mediante el marcado declarativo actualizado del control CreateUserWizard:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample13.aspx)]

Tenga en cuenta el nuevo elemento `<asp:WizardStep>`. Necesitamos agregar la interfaz de usuario para recopilar aquí la ciudad, la Página principal y la firma del nuevo usuario. Puede escribir este contenido en la sintaxis declarativa o a través del diseñador. Para usar el diseñador, seleccione el paso "su configuración" en la lista desplegable de la etiqueta inteligente para ver el paso en el diseñador.

> [!NOTE]
> Al seleccionar un paso a través de la lista desplegable de la etiqueta inteligente, se actualiza la [propiedad`ActiveStepIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)del control CreateUserWizard, que especifica el índice del paso inicial. Por lo tanto, si usa esta lista desplegable para editar el paso "su configuración" en el diseñador, asegúrese de volver a establecerla en "Suscribirse a la nueva cuenta" para que se muestre este paso cuando los usuarios visiten por primera vez la página `EnhancedCreateUserWizard.aspx`.

Cree una interfaz de usuario en el paso "su configuración" que contenga tres controles de cuadro de texto denominados `HomeTown`, `HomepageUrl`y `Signature`. Después de construir esta interfaz, el marcado declarativo de CreateUserWizard debe ser similar al siguiente:

[!code-aspx[Main](storing-additional-user-information-cs/samples/sample14.aspx)]

Continúe y visite esta página a través de un explorador y cree una nueva cuenta de usuario, especificando los valores de la ciudad, la Página principal y la firma. Después de completar el `CreateUserWizardStep` la cuenta de usuario se crea en el marco de pertenencia y se ejecuta el controlador de eventos `CreatedUser`, que agrega una nueva fila a `UserProfiles`, pero con un valor de `NULL` de base de datos para `HomeTown`, `HomepageUrl`y `Signature`. Los valores especificados para la ciudad, la Página principal y la firma no se usan nunca. El resultado neto es una cuenta de usuario nueva con un registro de `UserProfiles` cuyos campos `HomeTown`, `HomepageUrl`y `Signature` todavía se han especificado.

Necesitamos ejecutar código después del paso "su configuración" que toma los valores de ciudad, honepage y firma especificados por el usuario y actualiza el registro de `UserProfiles` adecuado. Cada vez que el usuario se desplaza entre los pasos de un control de asistente, se desencadena el [evento`ActiveStepChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) del asistente. Podemos crear un controlador de eventos para este evento y actualizar la tabla `UserProfiles` cuando se haya completado el paso "su configuración".

Agregue un controlador de eventos para el evento de `ActiveStepChanged` de CreateUserWizard y agregue el código siguiente:

[!code-csharp[Main](storing-additional-user-information-cs/samples/sample15.cs)]

El código anterior se inicia determinando si acabamos de alcanzar el paso "completar". Puesto que el paso "completo" se produce inmediatamente después del paso "su configuración", cuando el visitante alcanza el paso "completar", significa que acaba de finalizar el paso "su configuración".

En tal caso, es necesario hacer referencia mediante programación a los controles de cuadro de texto en el `UserSettings WizardStep`. Esto se logra utilizando primero el método `FindControl` para hacer referencia mediante programación a la `UserSettings WizardStep`y, a continuación, de nuevo para hacer referencia a los cuadros de texto desde dentro de la `WizardStep`. Una vez que se ha hecho referencia a los cuadros de texto, estamos listos para ejecutar la instrucción `UPDATE`. La instrucción `UPDATE` tiene el mismo número de parámetros que la instrucción `INSERT` en el controlador de eventos `CreatedUser`, pero aquí usamos los valores de ciudad, Página principal y firma proporcionados por el usuario.

Con este controlador de eventos en su lugar, visite la página `EnhancedCreateUserWizard.aspx` a través de un explorador y cree una cuenta de usuario nueva que especifique los valores de la ciudad, la Página principal y la firma. Después de crear la nueva cuenta, debe redirigirse a la página `AdditionalUserInfo.aspx`, donde se muestra la información de la ciudad, la Página principal y la firma que se acaban de escribir.

> [!NOTE]
> Nuestro sitio web tiene actualmente dos páginas desde las que un visitante puede crear una nueva cuenta: `CreatingUserAccounts.aspx` y `EnhancedCreateUserWizard.aspx`. El mapa del sitio web y la página de inicio de sesión apuntan a la página `CreatingUserAccounts.aspx`, pero en la página `CreatingUserAccounts.aspx` no se solicita al usuario la información de la ciudad, la Página principal y la firma, y no se agrega una fila correspondiente a `UserProfiles`. Por lo tanto, actualice la página `CreatingUserAccounts.aspx` de modo que ofrezca esta funcionalidad o actualice el mapa del sitio y la página de inicio de sesión para hacer referencia a `EnhancedCreateUserWizard.aspx` en lugar de `CreatingUserAccounts.aspx`. Si elige esta última opción, asegúrese de actualizar el archivo `Web.config` de la carpeta de `Membership` para permitir que los usuarios anónimos tengan acceso a la página `EnhancedCreateUserWizard.aspx`.

## <a name="summary"></a>Resumen

En este tutorial se han examinado las técnicas de modelado de datos relacionados con las cuentas de usuario en el marco de pertenencia. En concreto, analizamos las entidades de modelado que comparten una relación de uno a varios con cuentas de usuario, así como datos que comparten una relación de uno a uno. Además, vimos cómo se podría mostrar, insertar y actualizar esta información relacionada, con algunos ejemplos de uso del control SqlDataSource y otros usuarios con código ADO.NET.

En este tutorial se completa nuestra mirada a las cuentas de usuario. A partir del siguiente tutorial, se activará nuestra atención a los roles. En los siguientes tutoriales, veremos el marco de trabajo de roles, cómo crear nuevos roles, cómo asignar roles a los usuarios, cómo determinar a qué roles pertenece un usuario y cómo aplicar la autorización basada en roles.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Obtener acceso a los datos y actualizarlos en ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Control del Asistente para ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Creación de una interfaz de usuario paso a paso con el control del asistente de ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Crear parámetros de control DataSource personalizados](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personalización del control CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Inicios rápidos del control DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Mostrar datos con el control ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Dessección de los controles de validación en ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Editar los datos de inserción y eliminación](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Validación de formulario en ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Recopilar información de registro de usuario personalizada](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Perfiles en ASP.NET 2,0](http://www.odetocode.com/Articles/440.aspx)
- [El control ASP: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Inicio rápido de perfiles de usuario](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Acerca del autor

Scott Mitchell, autor de varios libros de ASP/ASP. NET y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se *[enseña a ASP.NET 2,0 en 24 horas](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Se puede acceder a Scott en [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial...

Muchos revisores útiles revisaron esta serie de tutoriales. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](user-based-authorization-cs.md)
> [Siguiente](creating-the-membership-schema-in-sql-server-vb.md)
