---
uid: identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
title: Información general de los proveedores de almacenamiento personalizados para ASP.NET Identity ASP.NET 4. x
author: Rick-Anderson
description: ASP.NET Identity es un sistema extensible que le permite crear su propio proveedor de almacenamiento y conectarlo a la aplicación sin tener que volver a trabajar con las aplicaciones...
ms.author: riande
ms.date: 10/13/2014
ms.assetid: 681a9204-462e-4260-9a0b-19f0644d6ad7
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/extensibility/overview-of-custom-storage-providers-for-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 21baedf6285b411f89627df9ca25d47a2a42e387
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519107"
---
# <a name="overview-of-custom-storage-providers-for-aspnet-identity"></a>Información general sobre los proveedores de almacenamiento personalizado para ASP.NET Identity

por [Tom FitzMacken](https://github.com/tfitzmac)

> ASP.NET Identity es un sistema extensible que le permite crear su propio proveedor de almacenamiento y conectarlo a la aplicación sin tener que volver a trabajar con la aplicación. En este tema se describe cómo crear un proveedor de almacenamiento personalizado para ASP.NET Identity. Trata los conceptos importantes para crear su propio proveedor de almacenamiento, pero no es un tutorial paso a paso de la implementación de un proveedor de almacenamiento personalizado.
> 
> Para obtener un ejemplo de implementación de un proveedor de almacenamiento personalizado, consulte [implementación de un proveedor de almacenamiento de ASP.net Identity MySQL personalizado](implementing-a-custom-mysql-aspnet-identity-storage-provider.md).
> 
> Este tema se actualizó en ASP.NET Identity 2,0.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software usadas en el tutorial
> 
> 
> - Visual Studio 2013 con Update 2
> - ASP.NET Identity 2

## <a name="introduction"></a>Introducción

De forma predeterminada, el sistema ASP.NET Identity almacena información de usuario en una base de datos SQL Server y utiliza Entity Framework Code First para crear la base de datos. En muchas aplicaciones, este enfoque funciona bien. Sin embargo, es posible que prefiera usar un tipo diferente de mecanismo de persistencia, como Azure Table Storage, o que ya tenga tablas de base de datos con una estructura muy diferente de la implementación predeterminada. En cualquier caso, puede escribir un proveedor personalizado para su mecanismo de almacenamiento y conectar ese proveedor a la aplicación.

ASP.NET Identity se incluye de forma predeterminada en muchas de las plantillas de Visual Studio 2013. Puede obtener actualizaciones para ASP.NET Identity mediante el [paquete NuGet de Microsoft ASPNET Identity EntityFramework](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/).

En este tema se incluyen las secciones siguientes:

- [Descripción de la arquitectura](#architecture)
- [Descripción de los datos que se almacenan](#data)
- [Crear la capa de acceso a datos](#dal)
- [Personalización de la clase de usuario](#user)
- [Personalización del almacén de usuarios](#userstore)
- [Personalización de la clase role](#role)
- [Personalización del almacén de roles](#rolestore)
- [Volver a configurar la aplicación para usar el nuevo proveedor de almacenamiento](#reconfigure)
- [Otras implementaciones de proveedores de almacenamiento personalizados](#other)

<a id="architecture"></a>
## <a name="understand-the-architecture"></a>Descripción de la arquitectura

ASP.NET Identity consta de clases denominadas administradores y almacenes. Los administradores son clases de alto nivel que utiliza un desarrollador de aplicaciones para realizar operaciones, como la creación de un usuario, en el sistema ASP.NET Identity. Los almacenes son clases de nivel inferior que especifican cómo se conservan las entidades, como usuarios y roles. Los almacenes están estrechamente acoplados con el mecanismo de persistencia, pero los administradores se desacoplan de los almacenes, lo que significa que puede reemplazar el mecanismo de persistencia sin interrumpir toda la aplicación.

En el diagrama siguiente se muestra cómo interactúa la aplicación web con los administradores y las tiendas interactúan con la capa de acceso a datos.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image1.png)

Para crear un proveedor de almacenamiento personalizado para ASP.NET Identity, debe crear el origen de datos, la capa de acceso a datos y las clases de almacén que interactúan con esta capa de acceso a datos. Puede seguir usando las mismas API de administrador para realizar operaciones de datos en el usuario, pero ahora los datos se guardan en un sistema de almacenamiento diferente.

No es necesario personalizar las clases de administrador porque al crear una nueva instancia de UserManager o RoleManager, se proporciona el tipo de la clase de usuario y se pasa una instancia de la clase Store como argumento. Este enfoque le permite conectar las clases personalizadas a la estructura existente. Verá cómo crear una instancia de UserManager y RoleManager con las clases de almacenamiento personalizadas en la sección [reconfigure Application para usar el nuevo proveedor de almacenamiento](#reconfigure).

<a id="data"></a>
## <a name="understand-the-data-that-is-stored"></a>Descripción de los datos que se almacenan

Para implementar un proveedor de almacenamiento personalizado, debe comprender los tipos de datos que se usan con ASP.NET Identity y decidir qué características son relevantes para su aplicación.

| Datos | Descripción |
| --- | --- |
| Usuarios de | Usuarios registrados de su sitio Web. Incluye el identificador de usuario y el nombre de usuario. Puede incluir una contraseña con hash si los usuarios inician sesión con credenciales específicas de su sitio (en lugar de usar credenciales de un sitio externo como Facebook) y marca de seguridad para indicar si ha cambiado algo en las credenciales de usuario. También puede incluir la dirección de correo electrónico, el número de teléfono, si está habilitada la autenticación en dos fases, el número actual de inicios de sesión erróneos y si una cuenta se ha bloqueado. |
| Notificaciones de usuario | Un conjunto de instrucciones (o notificaciones) sobre el usuario que representa la identidad del usuario. Puede habilitar una expresión mayor de la identidad del usuario que se puede lograr a través de los roles. |
| Inicios de sesión de usuario | Información sobre el proveedor de autenticación externo (como Facebook) que se va a usar al iniciar sesión en un usuario. |
| Roles de | Grupos de autorización para el sitio. Incluye el identificador de rol y el nombre de rol (por ejemplo, "admin" o "Employee"). |

<a id="dal"></a>
## <a name="create-the-data-access-layer"></a>Crear la capa de acceso a datos

En este tema se da por supuesto que está familiarizado con el mecanismo de persistencia que va a usar y cómo crear entidades para ese mecanismo. En este tema no se proporcionan detalles sobre cómo crear los repositorios o las clases de acceso a datos. en su lugar, proporciona algunas sugerencias sobre las decisiones de diseño que debe tomar al trabajar con ASP.NET Identity.

Tiene una gran libertad al diseñar los repositorios de un proveedor de almacenamiento personalizado. Solo tiene que crear repositorios para las características que desea utilizar en la aplicación. Por ejemplo, si no usa roles en la aplicación, no es necesario crear almacenamiento para roles o roles de usuario. La tecnología y la infraestructura existente pueden requerir una estructura muy diferente de la implementación predeterminada de ASP.NET Identity. En la capa de acceso a datos, se proporciona la lógica para trabajar con la estructura de los repositorios.

Para obtener una implementación de MySQL de repositorios de datos para ASP.NET Identity 2,0, consulte [MySQLIdentity. SQL](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLIdentity.sql).

En la capa de acceso a datos, se proporciona la lógica para guardar los datos de ASP.NET Identity en el origen de datos. La capa de acceso a datos para el proveedor de almacenamiento personalizado podría incluir las siguientes clases para almacenar información de usuarios y roles.

| Clase | Descripción | Ejemplo |
| --- | --- | --- |
| Contexto | Encapsula la información para conectarse al mecanismo de persistencia y ejecutar consultas. Esta clase es fundamental para el nivel de acceso a datos. Las demás clases de datos requerirán una instancia de esta clase para realizar sus operaciones. También inicializará las clases de almacén con una instancia de esta clase. | [MySQLDatabase](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/MySQLDatabase.cs) |
| Almacenamiento de usuario | Almacena y recupera información de usuario (como el nombre de usuario y el hash de contraseña). | [UserTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserTable.cs) |
| Almacenamiento de rol | Almacena y recupera información de roles (por ejemplo, el nombre del rol). | [RoleTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleTable.cs) |
| Almacenamiento de UserClaims | Almacena y recupera información de notificaciones de usuario (por ejemplo, el tipo y el valor de la demanda). | [UserClaimsTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserClaimsTable.cs) |
| Almacenamiento de UserLogins | Almacena y recupera información de inicio de sesión del usuario (por ejemplo, un proveedor de autenticación externo). | [UserLoginsTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserLoginsTable.cs) |
| Almacenamiento de UserRole | Almacena y recupera los roles a los que está asignado un usuario. | [UserRoleTable (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserRoleTable.cs) |

De nuevo, solo tiene que implementar las clases que desea utilizar en la aplicación.

En las clases de acceso a datos, se proporciona código para realizar las operaciones de datos para el mecanismo de persistencia en cuestión. Por ejemplo, dentro de la implementación de MySQL, la clase UserTable contiene un método para insertar un nuevo registro en la tabla de base de datos de usuarios. La variable denominada `_database` es una instancia de la clase MySQLDatabase.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample1.cs)]

Después de crear las clases de acceso a datos, debe crear clases de almacén que llamen a los métodos específicos de la capa de acceso a datos.

<a id="user"></a>
## <a name="customize-the-user-class"></a>Personalización de la clase de usuario

Al implementar su propio proveedor de almacenamiento, debe crear una clase de usuario que sea equivalente a la clase [IdentityUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityuser(v=vs.108).aspx) en el espacio de nombres [Microsoft. asp. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

En el diagrama siguiente se muestra la clase IdentityUser que debe crear y la interfaz que se va a implementar en esta clase.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image2.png)

La interfaz [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) define las propiedades que el UserManager intenta llamar al realizar las operaciones solicitadas. La interfaz contiene dos propiedades: ID y UserName. La interfaz [IUser&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613291(v=vs.108).aspx) le permite especificar el tipo de la clave para el usuario mediante el parámetro de **TKey** genérico. El tipo de la propiedad ID coincide con el valor del parámetro TKey.

El marco de trabajo de identidad también proporciona la interfaz [IUser](https://msdn.microsoft.com/library/microsoft.aspnet.identity.iuser(v=vs.108).aspx) (sin el parámetro genérico) cuando se desea usar un valor de cadena para la clave.

La clase IdentityUser implementa IUser y contiene cualquier propiedad o constructores adicionales para los usuarios en el sitio Web. En el ejemplo siguiente se muestra una clase IdentityUser que utiliza un entero para la clave. El campo ID se establece en **int** para que coincida con el valor del parámetro Generic. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample2.cs)]

 Para obtener una implementación completa, vea [IdentityUser (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityUser.cs). 

<a id="userstore"></a>
## <a name="customize-the-user-store"></a>Personalización del almacén de usuarios

También creará una clase UserStore que proporciona los métodos para todas las operaciones de datos del usuario. Esta clase es equivalente a la clase [UserStore&lt;TUser&gt;](https://msdn.microsoft.com/library/dn315446(v=vs.108).aspx) en el espacio de nombres [Microsoft. asp. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) . En la clase UserStore, implemente el [IUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) y cualquiera de las interfaces opcionales. Seleccione las interfaces opcionales que se implementarán en función de la funcionalidad que desea proporcionar en la aplicación.

En la imagen siguiente se muestra la clase UserStore que debe crear y las interfaces pertinentes.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image3.png)

La plantilla de proyecto predeterminada de Visual Studio contiene código que supone que muchas de las interfaces opcionales se han implementado en el almacén de usuario. Si usa la plantilla predeterminada con un almacén de usuario personalizado, debe implementar interfaces opcionales en el almacén de usuario o modificar el código de plantilla para que no se llame a los métodos de las interfaces que no ha implementado.

 En el ejemplo siguiente se muestra una clase simple de almacén de usuario. El parámetro genérico **TUser** toma el tipo de la clase de usuario, que normalmente es la clase IdentityUser que ha definido. El parámetro genérico **TKey** toma el tipo de la clave de usuario. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample3.cs)]

 En este ejemplo, el constructor que toma un parámetro denominado *Database* de tipo ExampleDatabase solo es una ilustración de cómo pasar la clase de acceso a datos. Por ejemplo, en la implementación de MySQL, este constructor toma un parámetro de tipo MySQLDatabase. 

Dentro de la clase UserStore, use las clases de acceso a datos que creó para realizar operaciones. Por ejemplo, en la implementación de MySQL, la clase UserStore tiene el método CreateAsync, que usa una instancia de UserTable para insertar un nuevo registro. El método **Insert** en el objeto **userTable** es el mismo método que se mostró en la sección anterior. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample4.cs)]

### <a name="interfaces-to-implement-when-customizing-user-store"></a>Interfaces que se implementan al personalizar el almacén del usuario

En la imagen siguiente se muestran más detalles sobre la funcionalidad definida en cada interfaz. Todas las interfaces opcionales heredan de IUserStore.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image4.png)

- **IUserStore**  
  La interfaz [IUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613278(v=vs.108).aspx) es la única interfaz que debe implementar en el almacén de usuario. Define métodos para crear, actualizar, eliminar y recuperar usuarios.
- **IUserClaimStore**  
  La interfaz [IUserClaimStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613265(v=vs.108).aspx) define los métodos que debe implementar en el almacén de usuario para habilitar las notificaciones de usuario. Contiene métodos o agrega, quita y recupera notificaciones de usuario.
- **IUserLoginStore**  
  [IUserLoginStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613272(v=vs.108).aspx) define los métodos que debe implementar en el almacén de usuario para habilitar proveedores de autenticación externos. Contiene métodos para agregar, quitar y recuperar inicios de sesión de usuario, y un método para recuperar un usuario en función de la información de inicio de sesión.
- **IUserRoleStore**  
  La interfaz [IUserRoleStore&lt;TKey, TUser&gt;](https://msdn.microsoft.com/library/dn613276(v=vs.108).aspx) define los métodos que debe implementar en el almacén de usuario para asignar un usuario a un rol. Contiene métodos para agregar, quitar y recuperar los roles de un usuario, y un método para comprobar si un usuario está asignado a un rol.
- **IUserPasswordStore**  
  La interfaz [IUserPasswordStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613273(v=vs.108).aspx) define los métodos que debe implementar en el almacén del usuario para conservar las contraseñas con algoritmo hash. Contiene métodos para obtener y establecer la contraseña hash y un método que indica si el usuario ha establecido una contraseña.
- **IUserSecurityStampStore**  
  La interfaz [IUserSecurityStampStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613277(v=vs.108).aspx) define los métodos que debe implementar en el almacén del usuario para usar una marca de seguridad que indique si la información de la cuenta del usuario ha cambiado. Esta marca se actualiza cuando un usuario cambia la contraseña, o agrega o quita inicios de sesión. Contiene métodos para obtener y establecer la marca de seguridad.
- **IUserTwoFactorStore**  
  La interfaz [IUserTwoFactorStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613279(v=vs.108).aspx) define los métodos que debe implementar para implementar la autenticación en dos fases. Contiene métodos para obtener y establecer si la autenticación en dos fases está habilitada para un usuario.
- **IUserPhoneNumberStore**  
  La interfaz [IUserPhoneNumberStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613275(v=vs.108).aspx) define los métodos que se deben implementar para almacenar los números de teléfono del usuario. Contiene métodos para obtener y establecer el número de teléfono y si se ha confirmado el número de teléfono.
- **IUserEmailStore**  
  La interfaz [IUserEmailStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613143(v=vs.108).aspx) define los métodos que se deben implementar para almacenar direcciones de correo electrónico de usuario. Contiene métodos para obtener y establecer la dirección de correo electrónico y si se confirma el correo electrónico.
- **IUserLockoutStore**  
  La interfaz [IUserLockoutStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613271(v=vs.108).aspx) define los métodos que debe implementar para almacenar información sobre el bloqueo de una cuenta. Contiene métodos para obtener el número actual de intentos de acceso incorrectos, obtener y establecer si la cuenta se puede bloquear, obtener y establecer la fecha de finalización del bloqueo, incrementar el número de intentos fallidos y restablecer el número de intentos fallidos.
- **IQueryableUserStore**  
  La interfaz [IQueryableUserStore&lt;TUser, TKey&gt;](https://msdn.microsoft.com/library/dn613267(v=vs.108).aspx) define los miembros que debe implementar para proporcionar un almacén de usuarios consultable. Contiene una propiedad que contiene los usuarios que se pueden consultar.

  Las interfaces que se necesitan en la aplicación se implementan. como, por ejemplo, las interfaces IUserClaimStore, IUserLoginStore, IUserRoleStore, IUserPasswordStore y IUserSecurityStampStore, como se muestra a continuación. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample5.cs)]

Para obtener una implementación completa (incluidas todas las interfaces), consulte [UserStore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/UserStore.cs).

### <a name="identityuserclaim-identityuserlogin-and-identityuserrole"></a>IdentityUserClaim, IdentityUserLogin y IdentityUserRole

El espacio de nombres Microsoft. AspNet. Identity. EntityFramework contiene implementaciones de las clases [IdentityUserClaim](https://msdn.microsoft.com/library/dn613250(v=vs.108).aspx), [IdentityUserLogin](https://msdn.microsoft.com/library/dn613251(v=vs.108).aspx)y [IdentityUserRole](https://msdn.microsoft.com/library/dn613252(v=vs.108).aspx) . Si usa estas características, puede que desee crear sus propias versiones de estas clases y definir las propiedades de la aplicación. Sin embargo, a veces es más eficaz no cargar estas entidades en la memoria al realizar operaciones básicas (como agregar o quitar la demanda de un usuario). En su lugar, las clases de almacén de back-end pueden ejecutar estas operaciones directamente en el origen de datos. Por ejemplo, el método UserStore. GetClaimsAsync () puede llamar a userClaimTable. FindByUserId (User. ID) para ejecutar una consulta en esa tabla directamente y devolver una lista de notificaciones.

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample6.cs)]

<a id="role"></a>
## <a name="customize-the-role-class"></a>Personalización de la clase role

Al implementar su propio proveedor de almacenamiento, debe crear una clase de rol que sea equivalente a la clase [IdentityRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework.identityrole(v=vs.108).aspx) en el espacio de nombres [Microsoft. asp. net. Identity. EntityFramework](https://msdn.microsoft.com/library/microsoft.aspnet.identity.entityframework(v=vs.108).aspx) :

En el diagrama siguiente se muestra la clase IdentityRole que debe crear y la interfaz que se va a implementar en esta clase.

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image5.png)

La interfaz [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) define las propiedades que el RoleManager intenta llamar al realizar las operaciones solicitadas. La interfaz contiene dos propiedades: ID y Name. La interfaz [IRole&lt;TKey&gt;](https://msdn.microsoft.com/library/dn613268(v=vs.108).aspx) permite especificar el tipo de clave para el rol mediante el parámetro de **TKey** genérico. El tipo de la propiedad ID coincide con el valor del parámetro TKey.

El marco de trabajo de identidad también proporciona la interfaz [IRole](https://msdn.microsoft.com/library/microsoft.aspnet.identity.irole(v=vs.108).aspx) (sin el parámetro genérico) cuando se desea usar un valor de cadena para la clave.

En el ejemplo siguiente se muestra una clase IdentityRole que utiliza un entero para la clave. El campo ID se establece en int para que coincida con el valor del parámetro Generic. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample7.cs)]

 Para obtener una implementación completa, vea [IdentityRole (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/IdentityRole.cs). 

<a id="rolestore"></a>
## <a name="customize-the-role-store"></a>Personalización del almacén de roles

También creará una clase RoleStore que proporciona los métodos para todas las operaciones de datos en roles. Esta clase es equivalente a la clase [RoleStore&lt;&gt;de trol](https://msdn.microsoft.com/library/dn468181(v=vs.108).aspx) en el espacio de nombres Microsoft. asp. net. Identity. EntityFramework. En la clase RoleStore, implemente el [IRoleStore&lt;ReIQueryableRoleStores, tkey&gt;](https://msdn.microsoft.com/library/dn613266(v=vs.108).aspx) y, opcionalmente, la interfaz [&lt;de RER, TKey&gt;](https://msdn.microsoft.com/library/dn613262(v=vs.108).aspx) .

![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image6.png)

En el ejemplo siguiente se muestra una clase de almacén de roles. El parámetro genérico de Comtrol toma el tipo de su clase de rol, que normalmente es la clase IdentityRole que ha definido. El parámetro genérico TKey toma el tipo de la clave de rol. 

[!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample8.cs)]

- **IRoleStore&lt;TRole&gt;**  
  La interfaz [IRoleStore](https://msdn.microsoft.com/library/dn468195.aspx) define los métodos que se van a implementar en la clase de almacén de roles. Contiene métodos para crear, actualizar, eliminar y recuperar roles.
- **RoleStore&lt;TRole&gt;**  
  Para personalizar RoleStore, cree una clase que implemente la interfaz IRoleStore. Solo tiene que implementar esta clase si desea usar roles en el sistema. El constructor que toma un parámetro denominado *Database* de tipo ExampleDatabase solo es una ilustración de cómo pasar la clase de acceso a datos. Por ejemplo, en la implementación de MySQL, este constructor toma un parámetro de tipo MySQLDatabase.  
  
  Para obtener una implementación completa, vea [RoleStore (MySQL)](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/AspNet.Identity.MySQL/RoleStore.cs) .

<a id="reconfigure"></a>
## <a name="reconfigure-application-to-use-new-storage-provider"></a>Volver a configurar la aplicación para usar el nuevo proveedor de almacenamiento

Ha implementado el nuevo proveedor de almacenamiento. Ahora, debe configurar la aplicación para que use este proveedor de almacenamiento. Si el proveedor de almacenamiento predeterminado se incluyó en el proyecto, debe quitar el proveedor predeterminado y reemplazarlo por el proveedor.

### <a name="replace-default-storage-provider-in-mvc-project"></a>Reemplazar el proveedor de almacenamiento predeterminado en el proyecto de MVC

1. En la ventana **administrar paquetes NuGet** , desinstale el paquete **Microsoft ASP.net Identity EntityFramework** . Puede encontrar este paquete buscando en los paquetes instalados Identity. EntityFramework.  
    ![](overview-of-custom-storage-providers-for-aspnet-identity/_static/image7.png) se le preguntará si también desea desinstalar Entity Framework. Si no lo necesita en otras partes de la aplicación, puede desinstalarla.
2. En el archivo IdentityModels.cs de la carpeta models, elimine o comente las clases **ApplicationUser** y **ApplicationDbContext** . En una aplicación MVC, puede eliminar el archivo IdentityModels.cs completo. En una aplicación de formularios Web Forms, elimine las dos clases, pero asegúrese de mantener la clase auxiliar que también se encuentra en el archivo IdentityModels.cs.
3. Si el proveedor de almacenamiento reside en un proyecto independiente, agregue una referencia a él en la aplicación Web.
4. Reemplace todas las referencias a `using Microsoft.AspNet.Identity.EntityFramework;` por una instrucción using para el espacio de nombres de su proveedor de almacenamiento.
5. En la clase **Startup.auth.CS** , cambie el método **ConfigureAuth** para usar una única instancia del contexto adecuado. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample9.cs?highlight=3)]
6. En la carpeta de inicio de la aplicación\_, Abra **IdentityConfig.CS**. En la clase ApplicationUserManager, cambie el método **Create** para que se devuelva un administrador de usuarios que use el almacén de usuario personalizado. 

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample10.cs?highlight=3)]
7. Reemplace todas las referencias a **ApplicationUser** por **IdentityUser**.
8. El proyecto predeterminado incluye algunos miembros de la clase de usuario que no están definidos en la interfaz IUser. por ejemplo, correo electrónico, PasswordHash y GenerateUserIdentityAsync. Si la clase de usuario no tiene estos miembros, debe implementarlos o cambiar el código que los usa.
9. Si ha creado alguna instancia de RoleManager, cambie ese código para usar la nueva clase RoleStore.  

    [!code-csharp[Main](overview-of-custom-storage-providers-for-aspnet-identity/samples/sample11.cs)]
10. El proyecto predeterminado está diseñado para una clase de usuario que tiene un valor de cadena para la clave. Si la clase de usuario tiene un tipo diferente para la clave (por ejemplo, un entero), debe cambiar el proyecto para que funcione con su tipo. Consulte [cambio de la clave principal de los usuarios en ASP.net Identity](change-primary-key-for-users-in-aspnet-identity.md).
11. Si es necesario, agregue la cadena de conexión al archivo Web. config.

<a id="other"></a>
## <a name="other-resources"></a>Otros recursos

- Blog: [implementación de ASP.net Identity](http://odetocode.com/blogs/scott/archive/2014/01/20/implementing-asp-net-identity.aspx)
- Tutorial y código GIT: [proveedor de identidades simple. Data ASP.net](http://designcoderelease.blogspot.co.uk/2015/03/simpledata-aspnet-identity-provider.html)
- Tutorial:[configuración de las cuentas de identidad básicas y su señalización en una base de datos externa](http://typecastexception.com/post/2013/10/27/Configuring-Db-Connection-and-Code-First-Migration-for-Identity-Accounts-in-ASPNET-MVC-5-and-Visual-Studio-2013.aspx). Por [@xivSolutions](https://twitter.com/xivSolutions).
- Tutorial[: implementar un proveedor de almacenamiento de ASP.net Identity MySQL personalizado](implementing-a-custom-mysql-aspnet-identity-storage-provider.md)
- [Entidades CodeFluent](http://blog.codefluententities.com/2014/04/30/asp-net-identity-v2-and-codefluent-entities/) por [SoftFluent](http://www.softfluent.com/)
- [Azure Table Storage](https://www.nuget.org/packages/accidentalfish.aspnet.identity.azure/) de James Díaz.
- Azure Table Storage: [Aspnet. Identity. TableStorage](https://github.com/stuartleeks/leeksnet.AspNet.Identity.TableStorage) por [@stuartleeks](https://twitter.com/stuartleeks).
- [CouchDB/Cloudn de Daniel Wertheim.](https://github.com/danielwertheim/mycouch.aspnet.identity)
- Elástico bús[h: identidad elástica](https://github.com/bmbsqd/elastic-identity) de Bombsquad AB.
- [MongoDB](http://www.nuget.org/packages/MongoDB.AspNet.Identity/) de Jonathan Sheely Jonathan Sheely.
- [NHibernate. Aspnet. Identity](https://github.com/milesibastos/NHibernate.AspNet.Identity) por Antônio kilómetrosi Bastos.
- [RavenDB](http://www.nuget.org/packages/AspNet.Identity.RavenDB/1.0.0) por [@tourismgeek](https://twitter.com/tourismgeek).
- [RavenDB. Aspnet. Identity](https://github.com/ILMServices/RavenDB.AspNet.Identity) por [ILMServices](http://www.ilmservice.com/).
- Redis: [Redis. Aspnet. Identity](https://github.com/aminjam/Redis.AspNet.Identity)
- Plantillas T4 para generar código EF para un almacén de usuario de "base de datos primero": [Aspnet. Identity. EntityFramework](https://github.com/cbfrank/AspNet.Identity.EntityFramework)
