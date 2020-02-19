---
uid: identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
title: Migración de un sitio web existente desde la pertenencia de SQL a ASP.NET Identity ASP.NET 4. x
author: Rick-Anderson
description: En este tutorial se muestran los pasos para migrar una aplicación web existente con datos de usuarios y roles creados mediante la pertenencia a SQL al nuevo ASP.NET Identity...
ms.author: riande
ms.date: 12/19/2014
ms.custom: seoapril2019
ms.assetid: 220d3d75-16b2-4240-beae-a5b534f06419
msc.legacyurl: /identity/overview/migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 633229cc4311d151121bf6a91b9fa8aeecca1197
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456158"
---
# <a name="migrating-an-existing-website-from-sql-membership-to-aspnet-identity"></a>Migrar un sitio web existente desde la pertenencia de SQL a ASP.NET Identity

por [Rick Anderson](https://twitter.com/RickAndMSFT), [Suhas Joshi](https://github.com/suhasj)

> En este tutorial se muestran los pasos necesarios para migrar una aplicación web existente con datos de roles y usuarios creados mediante la pertenencia a SQL al nuevo sistema de ASP.NET Identity. Este enfoque implica cambiar el esquema de la base de datos existente por el que necesita el ASP.NET Identity y enlazarlo en las clases antiguas y nuevas. Después de adoptar este enfoque, una vez que se haya migrado la base de datos, las actualizaciones futuras de la identidad se tratarán sin esfuerzo.

En este tutorial, se tomará una plantilla de aplicación web (formularios Web Forms) creada con Visual Studio 2010 para crear datos de usuarios y roles. A continuación, usaremos scripts SQL para migrar la base de datos existente a las tablas que necesita el sistema de identidad. A continuación, instalaremos los paquetes de NuGet necesarios y agregaremos nuevas páginas de administración de cuentas que usan el sistema de identidad para la administración de la pertenencia. Como prueba de la migración, los usuarios creados mediante la pertenencia a SQL deberían poder iniciar sesión y los usuarios nuevos deben poder registrarse. Puede encontrar el ejemplo completo [aquí](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/). Vea también [migrar de la pertenencia a ASP.net a ASP.net Identity](http://travis.io/blog/2015/03/24/migrate-from-aspnet-membership-to-aspnet-identity.html).

## <a name="getting-started"></a>Introducción

### <a name="creating-an-application-with-sql-membership"></a>Creación de una aplicación con pertenencia a SQL

1. Necesitamos comenzar con una aplicación existente que use la pertenencia a SQL y tenga datos de usuario y de rol. Para el propósito de este artículo, vamos a crear una aplicación web en Visual Studio 2010.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.jpg)
2. Con la herramienta de configuración de ASP.NET, cree 2 usuarios: **oldAdminUser** y **oldUser.**

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image1.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.jpg)
3. Cree un rol denominado admin y agregue ' oldAdminUser ' como usuario en ese rol.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image2.png)
4. Cree una sección de administración del sitio con default. aspx. Establezca la etiqueta de autorización en el archivo Web. config para permitir el acceso solo a los usuarios de roles de administrador. Puede encontrar más información aquí [https://www.asp.net/web-forms/tutorials/security/roles/role-based-authorization-cs](../../../web-forms/overview/older-versions-security/roles/role-based-authorization-cs.md)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.png)
5. Vea la base de datos en Explorador de servidores para comprender las tablas creadas por el sistema de pertenencia de SQL. Los datos de inicio de sesión del usuario se almacenan en las tablas de pertenencia ASPNET\_Users y ASPNET\_, mientras que los datos de roles se almacenan en la tabla ASPNET\_roles. Información sobre los usuarios en los que se almacenan roles en la tabla UsersInRoles de ASPNET\_. Para la administración básica de la pertenencia, es suficiente para portar la información de las tablas anteriores al sistema ASP.NET Identity.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.png)

### <a name="migrating-to-visual-studio-2013"></a>Migrar a Visual Studio 2013

1. Instale Visual Studio Express 2013 para web o Visual Studio 2013 junto con las [actualizaciones más recientes](https://www.microsoft.com/download/details.aspx?id=44921).
2. Abra el proyecto anterior en la versión instalada de Visual Studio. Si SQL Server Express no está instalado en el equipo, se muestra un mensaje al abrir el proyecto, ya que la cadena de conexión utiliza SQL Express. Puede optar por instalar SQL Express o como solución alternativa cambiar la cadena de conexión a LocalDb. En este artículo, cambiaremos a LocalDb.
3. Abra Web. config y cambie la cadena de conexión de. SQLExpress en (LocalDb) v 11.0. Quite ' User Instance = true ' de la cadena de conexión.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image3.jpg)
4. Abra Explorador de servidores y compruebe que se pueden observar el esquema de la tabla y los datos.
5. El sistema ASP.NET Identity funciona con la versión 4,5 o posterior del marco. Redestinar la aplicación a 4,5 o superior.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image5.png)

    Compile el proyecto para comprobar que no hay errores.

### <a name="installing-the-nuget-packages"></a>Instalación de paquetes Nuget

1. En Explorador de soluciones, haga clic con el botón derecho en el proyecto &gt; **administrar paquetes NuGet**. En el cuadro de búsqueda, escriba "Asp.net Identity". Seleccione el paquete en la lista de resultados y haga clic en instalar. Para aceptar el contrato de licencia, haga clic en el botón "Acepto". Tenga en cuenta que este paquete instalará los paquetes de dependencia: EntityFramework y Microsoft ASP.NET principal de identidad. De igual forma, instale los siguientes paquetes (omita los últimos 4 paquetes OWIN si no desea habilitar el inicio de sesión de OAuth):

   - Microsoft.AspNet.Identity.Owin
   - Microsoft.Owin.Host.SystemWeb
   - Microsoft.Owin.Security.Facebook
   - Microsoft.Owin.Security.Google
   - Microsoft.Owin.Security.MicrosoftAccount
   - Microsoft.Owin.Security.Twitter

     ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image6.png)

### <a name="migrate-database-to-the-new-identity-system"></a>Migrar la base de datos al nuevo sistema de identidad

El siguiente paso consiste en migrar la base de datos existente a un esquema requerido por el sistema ASP.NET Identity. Para lograr esto, ejecutamos un script SQL que tiene un conjunto de comandos para crear nuevas tablas y migrar la información de usuario existente a las nuevas tablas. El archivo de script se puede encontrar [aquí](https://github.com/aspnet/samples/blob/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/Migrations.sql).

Este archivo de script es específico de este ejemplo. Si el esquema de las tablas creadas mediante la pertenencia a SQL es personalizado o modificado, los scripts deben cambiarse según corresponda.

### <a name="how-to-generate-the-sql-script-for-schema-migration"></a>Cómo generar el script SQL para la migración de esquemas

Para que las clases de ASP.NET Identity funcionen de forma prebox con los datos de los usuarios existentes, es necesario migrar el esquema de la base de datos al que necesita ASP.NET Identity. Podemos hacer esto agregando nuevas tablas y copiando la información existente en esas tablas. De forma predeterminada ASP.NET Identity usa EntityFramework para volver a asignar las clases del modelo de identidad a la base de datos para almacenar o recuperar información. Estas clases de modelo implementan las interfaces de identidad básicas que definen los objetos de usuario y de función. Las tablas y las columnas de la base de datos se basan en estas clases de modelo. Las clases del modelo EntityFramework en la identidad v 2.1.0 y sus propiedades se definen a continuación.

| **IdentityUser** | **Tipo** | **IdentityRole** | **IdentityUserRole** | **IdentityUserLogin** | **IdentityUserClaim** |
| --- | --- | --- | --- | --- | --- |
| Identificador | string | Identificador | RoleId | ProviderKey | Identificador |
| Nombre de usuario | string | Nombre | UserId | UserId | ClaimType |
| PasswordHash | string |  |  | LoginProvider | ClaimValue |
| SecurityStamp | string |  |  |  | Identificador de\_de usuario |
| Email | string |  |  |  |  |
| EmailConfirmed | bool |  |  |  |  |
| PhoneNumber | string |  |  |  |  |
| PhoneNumberConfirmed | bool |  |  |  |  |
| LockoutEnabled | bool |  |  |  |  |
| LockoutEndDate | DateTime |  |  |  |  |
| AccessFailedCount | int |  |  |  |  |

Necesitamos tener tablas para cada uno de estos modelos con columnas que se correspondan con las propiedades. La asignación entre las clases y las tablas se define en el método `OnModelCreating` de la `IdentityDBContext`. Esto se conoce como el método de configuración de la API fluida y se puede encontrar más información [aquí](https://msdn.microsoft.com/data/jj591617.aspx). La configuración de las clases es como se menciona a continuación

| **Clase** | **Table** | **Clave principal** | **Clave externa** |
| --- | --- | --- | --- |
| IdentityUser | AspnetUsers | Identificador |  |
| IdentityRole | AspnetRoles | Identificador |  |
| IdentityUserRole | AspnetUserRole | UserId + RoleId | Identificador de\_de usuario:&gt;AspnetUsers RoleId-&gt;AspnetRoles |
| IdentityUserLogin | AspnetUserLogins | ProviderKey + UserId + LoginProvider | UserId-&gt;AspnetUsers |
| IdentityUserClaim | AspnetUserClaims | Identificador | Identificador de\_de usuario:&gt;AspnetUsers |

Con esta información, podemos crear instrucciones SQL para crear nuevas tablas. Podemos escribir cada instrucción individualmente o generar el script completo mediante los comandos de PowerShell de EntityFramework que podemos editar según sea necesario. Para ello, en VS, abra la **consola del administrador de paquetes** desde el menú **Ver** o **herramientas** .

- Ejecute el comando "Enable-Migrations" para habilitar las migraciones de EntityFramework.
- Ejecute el comando "Add-Migration Initial", que crea el código de instalación inicial para crear C#la base de datos en/VB.
- El paso final consiste en ejecutar el comando "Update-Database – script" que genera el script SQL basado en las clases de modelo.

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

Este script de generación de bases de datos se puede usar como un inicio en el que se van a realizar más cambios para agregar nuevas columnas y copiar los datos. La ventaja de esto es que se genera la tabla `_MigrationHistory` que usa EntityFramework para modificar el esquema de la base de datos cuando las clases de modelo cambian en versiones futuras de las versiones de identidad.

La información del usuario de pertenencia a SQL tenía otras propiedades además de las de la clase de modelo de usuario de identidad, es decir, el correo electrónico, los intentos de contraseña, la fecha de último inicio de sesión, la última fecha de bloqueo, etc. Esta información es útil y nos gustaría que se lleve a cabo en el sistema de identidades. Esto puede hacerse agregando propiedades adicionales al modelo de usuario y asignándolos de nuevo a las columnas de la tabla en la base de datos. Para ello, se agrega una clase que subclase el modelo de `IdentityUser`. Podemos agregar las propiedades a esta clase personalizada y editar el script SQL para agregar las columnas correspondientes al crear la tabla. El código de esta clase se describe con más detalle en el artículo. El script SQL para crear la tabla `AspnetUsers` después de agregar las nuevas propiedades sería

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample1.sql)]

A continuación, es necesario copiar la información existente de la base de datos de pertenencia a SQL en las tablas recién agregadas para la identidad. Esto puede realizarse a través de SQL copiando los datos directamente de una tabla a otra. Para agregar datos a las filas de la tabla, usamos la construcción `INSERT INTO [Table]`. Para copiar desde otra tabla, se puede usar la instrucción `INSERT INTO` junto con la instrucción `SELECT`. Para obtener toda la información de usuario necesaria para consultar los *usuarios de aspnet\_* y las tablas de *pertenencia de ASPNET\_* y copiar los datos en la tabla *AspNetUsers* . Usamos el `INSERT INTO` y `SELECT` junto con las instrucciones `JOIN` y `LEFT OUTER JOIN`. Para obtener más información sobre cómo consultar y copiar datos entre tablas, consulte [este](https://technet.microsoft.com/library/ms190750%28v=sql.105%29.aspx) vínculo. Además, las tablas AspnetUserLogins y AspnetUserClaims están vacías para comenzar, ya que no hay información en la pertenencia a SQL que se asigne a esta de forma predeterminada. La única información que se copia es para usuarios y roles. En el caso del proyecto creado en los pasos anteriores, la consulta SQL para copiar información en la tabla de usuarios sería

[!code-sql[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample2.sql)]

En la instrucción SQL anterior, la información sobre cada usuario de las tablas de pertenencia de *aspnet\_users* y *ASPNET\_* se copia en las columnas de la tabla *AspnetUsers* . La única modificación realizada aquí es cuando se copia la contraseña. Dado que el algoritmo de cifrado para contraseñas en la pertenencia a SQL usó ' PasswordSalt ' y ' PasswordFormat ', copiamos esto junto con la contraseña con hash para que se pueda usar para descifrar la contraseña por identidad. Esto se explica con más detalle en el artículo al enlazar un hash de contraseña personalizado.

Este archivo de script es específico de este ejemplo. En el caso de las aplicaciones que tienen tablas adicionales, los desarrolladores pueden seguir un enfoque similar para agregar propiedades adicionales a la clase de modelo de usuario y asignarlas a las columnas de la tabla AspnetUsers. Para ejecutar el script,

1. Abra el Explorador de servidores. Expanda la conexión ' ApplicationServices ' para mostrar las tablas. Haga clic con el botón derecho en el nodo tablas y seleccione la opción "nueva consulta".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image7.png)
2. En la ventana de consulta, copie y pegue todo el script SQL del archivo Migrations. SQL. Ejecute el archivo de script presionando el botón de flecha "ejecutar".

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image4.jpg)

    Actualice la ventana Explorador de servidores. Se crean cinco tablas nuevas en la base de datos.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image8.png)

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image9.png)

    A continuación se muestra cómo se asigna la información de las tablas de pertenencia a SQL al nuevo sistema de identidad.

    Roles de\_de ASPNET:&gt; AspNetRoles

    ASP\_netUsers y ASP\_netMembership--&gt; AspNetUsers

    ASPNET\_UserInRoles--&gt; AspNetUserRoles

    Como se explicó en la sección anterior, las tablas AspNetUserClaims y AspNetUserLogins están vacías. El campo ' discriminador ' de la tabla AspNetUser debe coincidir con el nombre de la clase de modelo que se define como paso siguiente. Además, la columna PasswordHash tiene el formato ' contraseña cifrada | contraseña sal | formato de contraseña '. Esto le permite usar lógica de cifrado de pertenencia a SQL especial para poder reutilizar las contraseñas antiguas. Esto se explica más adelante en el artículo.

### <a name="creating-models-and-membership-pages"></a>Crear modelos y páginas de pertenencia

Como se mencionó anteriormente, la característica de identidad usa Entity Framework para comunicarse con la base de datos para almacenar información de la cuenta de forma predeterminada. Para trabajar con los datos existentes en la tabla, es necesario crear clases de modelo que se asignan a las tablas y se enlazan en el sistema de identidades. Como parte del contrato de identidad, las clases de modelo deben implementar las interfaces definidas en el archivo dll de identidad. Core o pueden extender la implementación existente de estas interfaces disponibles en Microsoft. AspNet. Identity. EntityFramework.

En nuestro ejemplo, las tablas AspNetRoles, AspNetUserClaims, AspNetLogins y AspNetUserRole tienen columnas que son similares a la implementación existente del sistema de identidad. Por lo tanto, se pueden volver a usar las clases existentes para asignar a estas tablas. La tabla AspNetUser tiene algunas columnas adicionales que se usan para almacenar información adicional de las tablas de pertenencia de SQL. Esto puede asignarse mediante la creación de una clase de modelo que extienda la implementación existente de ' IdentityUser ' y agregue las propiedades adicionales.

1. Cree una carpeta models en el proyecto y agregue un usuario de clase. El nombre de la clase debe coincidir con los datos agregados en la columna ' Discriminator ' de la tabla ' AspnetUsers '.

    ![](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/_static/image10.png)

    La clase de usuario debe extender la clase IdentityUser que se encuentra en el archivo dll *Microsoft. Aspnet. Identity. EntityFramework* . Declare las propiedades de la clase que se asignan a las columnas AspNetUser. Las propiedades ID, username, PasswordHash y SecurityStamp se definen en IdentityUser, por lo que se omiten. A continuación se muestra el código de la clase de usuario que tiene todas las propiedades

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample3.cs)]
2. Se requiere una Entity Framework clase DbContext para conservar los datos de los modelos en las tablas y recuperar los datos de las tablas para rellenar los modelos. El archivo dll *Microsoft. Aspnet. Identity. EntityFramework* define la clase IdentityDbContext, que interactúa con las tablas de identidad para recuperar y almacenar información. El&gt; IdentityDbContext&lt;TUser toma una clase ' TUser ' que puede ser cualquier clase que extienda la clase IdentityUser.

    Cree una nueva clase ApplicationDBContext que extienda IdentityDbContext en la carpeta ' models ', pasando la clase ' User ' creada en el paso 1

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample4.cs)]
3. La administración de usuarios en el nuevo sistema de identidad se realiza mediante la clase UserManager&lt;TUser&gt; definida en el archivo dll *Microsoft. Aspnet. Identity. EntityFramework* . Necesitamos crear una clase personalizada que extienda UserManager y pasar la clase ' User ' creada en el paso 1.

    En la carpeta models, cree una nueva clase UserManager que extienda UserManager&lt;usuario&gt;

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample5.cs)]
4. Las contraseñas de los usuarios de la aplicación se cifran y almacenan en la base de datos. El algoritmo criptográfico que se usa en la pertenencia a SQL es diferente del del nuevo sistema de identidad. Para volver a usar las contraseñas antiguas, es necesario descifrar selectivamente las contraseñas cuando los usuarios antiguos inician sesión con el algoritmo de pertenencia a SQL y al usar el algoritmo de cifrado en la identidad para los nuevos usuarios.

    La clase UserManager tiene una propiedad ' PasswordHasher ' que almacena una instancia de una clase que implementa la interfaz ' IPasswordHasher '. Se usa para cifrar o descifrar las contraseñas durante las transacciones de autenticación del usuario. En la clase UserManager definida en el paso 3, cree una nueva clase SQLPasswordHasher y copie el código siguiente.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample6.cs)]

    Resuelva los errores de compilación importando los espacios de nombres System. Text y System. Security. Cryptography.

    El método EncodePassword cifra la contraseña según la implementación de cifrado de pertenencia a SQL predeterminada. Esto se toma del archivo dll de System. Web. Si la aplicación anterior usaba una implementación personalizada, debe reflejarse aquí. Necesitamos definir otros dos métodos *HashPassword* y *VerifyHashedPassword* que usan el método *EncodePassword* para aplicar un algoritmo hash a una contraseña determinada o comprobar una contraseña de texto sin formato con la que existe en la base de datos.

    El sistema de pertenencia de SQL usó PasswordHash, PasswordSalt y PasswordFormat para aplicar un algoritmo hash a la contraseña especificada por los usuarios al registrar o cambiar su contraseña. Durante la migración, todos los tres campos se almacenan en la columna PasswordHash de la tabla AspNetUser separadas por el carácter "|". Cuando un usuario inicia sesión y la contraseña tiene estos campos, usamos la criptografía de pertenencia de SQL para comprobar la contraseña; en caso contrario, usamos la criptografía predeterminada del sistema de identidad para comprobar la contraseña. De esta forma, los usuarios antiguos no tendrían que cambiar sus contraseñas una vez migrada la aplicación.
5. Declare el constructor para la clase UserManager y páselo como SQLPasswordHasher a la propiedad en el constructor.

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample7.cs)]

### <a name="create-new-account-management-pages"></a>Crear nuevas páginas de administración de cuentas

El siguiente paso de la migración es agregar páginas de administración de cuentas que permitan que un usuario se registre e inicie sesión. Las páginas de cuenta antiguas de la pertenencia a SQL usan controles que no funcionan con el nuevo sistema de identidad. Para agregar las nuevas páginas de administración de usuarios, siga el tutorial de este vínculo [https://www.asp.net/identity/overview/getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project](../getting-started/adding-aspnet-identity-to-an-empty-or-existing-web-forms-project.md) a partir del paso ' agregar Web Forms para registrar usuarios a la aplicación ' porque ya hemos creado el proyecto y agregado los paquetes de NuGet.

Necesitamos realizar algunos cambios en el ejemplo para trabajar con el proyecto que tenemos aquí.

- Las clases de código subyacente Register.aspx.cs y Login.aspx.cs usan el `UserManager` de los paquetes de identidad para crear un usuario. En este ejemplo, use el UserManager agregado en la carpeta models siguiendo los pasos mencionados anteriormente.
- Use la clase de usuario creada en lugar de IdentityUser en las clases de código subyacente Register.aspx.cs y Login.aspx.cs. Esto enlaza en la clase de usuario personalizada en el sistema de identidades.
- Se puede omitir el elemento para crear la base de datos.
- El desarrollador debe establecer el valor de ApplicationId del nuevo usuario para que coincida con el identificador de aplicación actual. Esto puede hacerse consultando el ApplicationId de esta aplicación antes de que se cree un objeto de usuario en la clase Register.aspx.cs y estableciéndolo antes de crear el usuario.

    Ejemplo:

    Defina un método en la página Register.aspx.cs para consultar la tabla de aplicaciones ASPNET\_y obtener el identificador de la aplicación según el nombre de la aplicación

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample8.cs)]

    Ahora, Get Set this en el objeto User

    [!code-csharp[Main](migrating-an-existing-website-from-sql-membership-to-aspnet-identity/samples/sample9.cs)]

Use el nombre de usuario y la contraseña antiguos para iniciar sesión con un usuario existente. Use la página registrar para crear un nuevo usuario. Compruebe también que los usuarios están en roles según lo esperado.

La migración al sistema de identidad ayuda al usuario a agregar autenticación abierta (OAuth) a la aplicación. Consulte [el ejemplo que](https://github.com/aspnet/samples/tree/master/samples/aspnet/Identity/SQLMembership-Identity-OWIN/) tiene OAuth habilitado.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se ha mostrado cómo trasladar a los usuarios de la pertenencia a SQL a ASP.NET Identity, pero no se han migrado los datos de perfil. En el siguiente tutorial, veremos cómo trasladar los datos de Perfil de la pertenencia de SQL al nuevo sistema de identidad.

Puede dejar comentarios en la parte inferior de este artículo.

*Gracias a Tom Dykstra y Rick Anderson para revisar el artículo.*
