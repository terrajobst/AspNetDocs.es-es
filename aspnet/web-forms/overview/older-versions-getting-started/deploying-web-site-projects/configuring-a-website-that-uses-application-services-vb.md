---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
title: Configurar un sitio web que usa Servicios de aplicación (VB) | Microsoft Docs
author: rick-anderson
description: La versión 2,0 de ASP.NET presentó una serie de servicios de aplicación, que forman parte de la .NET Framework y actúan como un conjunto de servicios de bloques de creación que yo...
ms.author: riande
ms.date: 04/23/2009
ms.assetid: 9c31a42f-d8bb-4c0f-9ccc-597d4f70ac42
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-vb
msc.type: authoredcontent
ms.openlocfilehash: 19e7258b558372259c7554a36c6ad73ce572dfa8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588627"
---
# <a name="configuring-a-website-that-uses-application-services-vb"></a>Configurar un sitio web que usa servicios de aplicaciones (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_09_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial09_AppServicesConfig_vb.pdf)

> La versión 2,0 de ASP.NET presentó una serie de servicios de aplicación, que forman parte de la .NET Framework y sirven como conjunto de servicios de bloques de creación que puede usar para agregar una funcionalidad enriquecida a su aplicación Web. En este tutorial se explica cómo configurar un sitio web en el entorno de producción para usar servicios de aplicación y se abordan los problemas comunes relacionados con la administración de las cuentas de usuario y los roles en el entorno de producción.

## <a name="introduction"></a>Introducción

La versión 2,0 de ASP.NET presentó una serie de *servicios de aplicación*, que forman parte de la .NET Framework y sirven como conjunto de servicios de bloques de creación que puede usar para agregar una funcionalidad enriquecida a su aplicación Web. Los servicios de aplicación incluyen:

- **Pertenencia** : una API para crear y administrar cuentas de usuario.
- **Roles** : una API para clasificar los usuarios en grupos.
- **Profile** : una API para almacenar contenido personalizado específico del usuario.
- **Mapa del sitio** : una API para definir una estructura lógica del sitio en forma de una jerarquía, que se puede mostrar a continuación a través de controles de navegación, como menús y rutas de navegación.
- **Personalización** : una API para mantener las preferencias de personalización, que suele usarse con [*elementos Web*](https://msdn.microsoft.com/library/e0s9t4ck.aspx).
- **Seguimiento de estado** : una API para supervisar el rendimiento, la seguridad, los errores y otras métricas de mantenimiento del sistema para una aplicación web en ejecución.

Las API de servicios de aplicación no están asociadas a una implementación específica. En su lugar, se indica a los servicios de aplicación que usen un *proveedor*determinado y ese proveedor implementa el servicio mediante una tecnología determinada. Los proveedores que se usan con más frecuencia para las aplicaciones web basadas en Internet hospedadas en una empresa de hospedaje web son aquellos proveedores que usan una implementación de base de datos SQL Server. Por ejemplo, el `SqlMembershipProvider` es un proveedor de la API de pertenencia que almacena la información de la cuenta de usuario en una base de datos de Microsoft SQL Server.

El uso de los proveedores de servicios de aplicaciones y SQL Server agrega algunos desafíos al implementar la aplicación. En el caso de los iniciadores, los objetos de base de datos de servicios de aplicación deben crearse correctamente en las bases de datos de desarrollo y de producción y inicializarse de manera adecuada. También hay opciones de configuración importantes que se deben realizar.

> [!NOTE]
> Las API de servicios de aplicación se diseñaron con el [*modelo de proveedor*](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), un modelo de diseño que permite proporcionar los detalles de implementación de la API s en tiempo de ejecución. El .NET Framework se suministra con varios proveedores de servicios de aplicaciones que se pueden usar, como el `SqlMembershipProvider` y el `SqlRoleProvider`, que son proveedores de las API de pertenencia y roles que usan una implementación de base de datos SQL Server. También puede crear y complementar un proveedor personalizado. De hecho, la aplicación Web de reseñas de libros ya contiene un proveedor personalizado para la API del mapa del sitio (`ReviewSiteMapProvider`), que construye el mapa del sitio a partir de los datos de las tablas `Genres` y `Books` de la base de datos.

En este tutorial se empieza con una visión de cómo amplié la aplicación Web de reseñas de libros para usar las API de pertenencia y roles. A continuación, se describe la implementación de una aplicación web que usa servicios de aplicación con una implementación de base de datos de SQL Server, y concluye por solucionar problemas comunes relacionados con la administración de roles y cuentas de usuario en el entorno de producción.

## <a name="updates-to-the-book-reviews-application"></a>Actualizaciones de la aplicación de revisiones del libro

En los últimos dos tutoriales, la aplicación Web de reseñas de libros se actualizó desde un sitio Web estático a una aplicación web dinámica controlada por datos completada con un conjunto de páginas de administración para administrar géneros y revisiones. Sin embargo, esta sección de administración no está protegida actualmente: cualquier usuario que sepa (o adivine) la dirección URL de la página de administración puede Waltz en el sitio y crear, editar o eliminar revisiones en nuestro sitio. Una forma común de proteger determinadas partes de un sitio web es implementar cuentas de usuario y, a continuación, usar reglas de autorización de URL para restringir el acceso a determinados usuarios o roles. La aplicación Web de reseñas de libros disponible para su descarga con este tutorial es compatible con las cuentas de usuario y los roles. Tiene un rol único definido como admin y solo los usuarios de este rol pueden tener acceso a las páginas de administración.

> [!NOTE]
> He creado tres cuentas de usuario en el libro Reviews Web Application: Scott, Jisun y Alice. Los tres usuarios tienen la misma contraseña: **contraseña.** Scott y Jisun están en el rol de administrador, Alicia no lo está. Las páginas de sitios que no son de administración siguen siendo accesibles para los usuarios anónimos. Es decir, no es necesario iniciar sesión para visitar el sitio, a menos que desee administrarlo, en cuyo caso debe iniciar sesión como usuario en el rol de administrador.

El libro revisa la página maestra de la aplicación se ha actualizado para incluir una interfaz de usuario diferente para los usuarios autenticados y anónimos. Si un usuario anónimo visita el sitio, ve un vínculo de inicio de sesión en la esquina superior derecha. Un usuario autenticado Ve el mensaje "Bienvenido atrás, *nombre de usuario*". y un vínculo para cerrar la sesión. También hay una página de inicio de sesión (`~/Login.aspx`), que contiene un control Web de inicio de sesión que proporciona la interfaz de usuario y la lógica para autenticar a un visitante. Solo los administradores pueden crear nuevas cuentas. (Hay páginas para crear y administrar cuentas de usuario en la carpeta `~/Admin`).

### <a name="configuring-the-membership-and-roles-apis"></a>Configuración de las API de pertenencia y roles

La aplicación Web de reseñas de libros utiliza las API de pertenencia y roles para admitir las cuentas de usuario y agrupar esos usuarios en roles (es decir, el rol de administrador). Se usan las clases de proveedor `SqlMembershipProvider` y `SqlRoleProvider` porque queremos almacenar la información de la cuenta y el rol en una base de datos de SQL Server.

> [!NOTE]
> Este tutorial no pretende ser un examen detallado de la configuración de una aplicación web para admitir las API de pertenencia y roles. Para obtener una visión detallada de estas API y los pasos que debe seguir para configurar un sitio web para usarlas, consulte los [*tutoriales de seguridad de mi sitio web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

Para usar los servicios de aplicación con una base de datos de SQL Server, primero debe agregar los objetos de base de datos usados por estos proveedores a la base de datos en la que desea almacenar la información de la cuenta de usuario y el rol. Estos objetos de base de datos necesarios incluyen una variedad de tablas, vistas y procedimientos almacenados. A menos que se especifique lo contrario, las clases de proveedor `SqlMembershipProvider` y `SqlRoleProvider` usan una base de datos de la edición SQL Server Express denominada `ASPNETDB` ubicada en la carpeta Application s `App_Data`; Si no existe una base de datos de este tipo, estos proveedores se crean automáticamente con los objetos de base de datos necesarios en tiempo de ejecución.

Es posible, y normalmente ideal, crear los objetos de base de datos de servicios de aplicación en la misma base de datos donde se almacenan los datos específicos de la aplicación del sitio Web. El .NET Framework se suministra con una herramienta denominada `aspnet_regsql.exe` que instala los objetos de base de datos en una base de datos especificada. He dejado de usar esta herramienta para agregar estos objetos a la `Reviews.mdf` base de datos en la carpeta `App_Data` (la base de datos de desarrollo). Veremos cómo usar esta herramienta más adelante en este tutorial cuando agreguemos estos objetos a la base de datos de producción.

Si agrega los objetos de base de datos de servicios de aplicación a una base de datos que no sea `ASPNETDB` deberá personalizar las configuraciones de las clases de proveedor `SqlMembershipProvider` y `SqlRoleProvider` para que usen la base de datos adecuada. Para personalizar el proveedor de pertenencia, agregue un [*elemento&lt;Membership&gt;* ](https://msdn.microsoft.com/library/1b9hw62f.aspx) dentro de la sección `<system.web>` en `Web.config`; Use el [*elemento&lt;roleManager&gt;* ](https://msdn.microsoft.com/library/ms164660.aspx) para configurar el proveedor de funciones. El siguiente fragmento de código se toma del libro Reviews Application s `Web.config` y muestra los valores de configuración de las API de pertenencia y roles. Tenga en cuenta que ambos registran un nuevo proveedor-`ReviewMembership` y `ReviewRole`, que usan los proveedores `SqlMembershipProvider` y `SqlRoleProvider`, respectivamente.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample1.xml)]

El elemento `Web.config` File s `<authentication>` también se ha configurado para admitir la autenticación basada en formularios.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample2.xml)]

### <a name="limiting-access-to-the-administration-pages"></a>Limitar el acceso a las páginas de administración

ASP.NET facilita la concesión o denegación de acceso a un archivo o carpeta determinados por usuario o por rol a través de su característica de *autorización de URL* . (Se describe brevemente la autorización de direcciones URL en las *diferencias principales entre IIS y el servidor de desarrollo de ASP.net* tutorial y se ha mostrado cómo IIS y el servidor de desarrollo de ASP.net aplicar las reglas de autorización de URL de forma diferente para el contenido estático y dinámico). Dado que queremos prohibir el acceso a la carpeta `~/Admin` excepto a los usuarios del rol de administrador, es necesario agregar reglas de autorización de URL a esta carpeta. En concreto, las reglas de autorización de URL deben permitir a los usuarios con el rol de administrador y denegar a todos los demás usuarios. Esto se logra agregando un archivo `Web.config` a la carpeta `~/Admin` con el siguiente contenido:

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample3.xml)]

Para obtener más información acerca de la característica de autorización de URL de ASP.NET s y cómo usarla para deletrear las reglas de autorización para los usuarios y los roles, asegúrese de leer los tutoriales de autorización basada en [*roles*](../../older-versions-security/roles/role-based-authorization-vb.md) y [*autorización basada*](../../older-versions-security/membership/user-based-authorization-vb.md) en el usuario de los [*tutoriales de seguridad de los sitios web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).

## <a name="deploying-a-web-application-that-uses-application-services"></a>Implementar una aplicación web que usa Servicios de aplicación

Al implementar un sitio web que usa servicios de aplicación y un proveedor que almacena la información de los servicios de aplicación en una base de datos, es imperativo que los objetos de base de datos necesarios para los servicios de la aplicación se creen en la base de datos de producción. Inicialmente, la base de datos de producción no contiene estos objetos, por lo que cuando la aplicación se implementa por primera vez (o cuando se implementa por primera vez después de que se han agregado servicios de aplicación), debe realizar pasos adicionales para obtener estos objetos de base de datos necesarios en el base de datos de producción.

Otro reto puede surgir al implementar un sitio web que usa servicios de aplicaciones si desea replicar las cuentas de usuario creadas en el entorno de desarrollo en el entorno de producción. En función de la configuración de pertenencia y roles, es posible que, aunque se copien correctamente las cuentas de usuario que se crearon en el entorno de desarrollo en la base de datos de producción, estos usuarios no puedan iniciar sesión en la aplicación web en producción. Veremos la causa de este problema y veremos cómo evitar que se produzca.

ASP.NET se suministra con una atractiva [*herramienta de administración de sitios web (WSAT)* ](https://msdn.microsoft.com/library/yy40ytx0.aspx) que se puede iniciar desde Visual Studio y permite que la cuenta de usuario, los roles y las reglas de autorización se administren mediante una interfaz basada en Web. Desafortunadamente, el WSAT solo funciona para sitios web locales, lo que significa que no se puede usar para administrar de forma remota las cuentas de usuario, los roles y las reglas de autorización para la aplicación web en el entorno de producción. Veremos diferentes maneras de implementar el comportamiento similar a WSAT desde el sitio web de producción.

### <a name="adding-the-database-objects-using-aspnet_regsqlexe"></a>Agregar los objetos de base de datos mediante ASPNET\_regsql. exe

En el tutorial *implementación de una base* de datos se ha mostrado cómo copiar las tablas y los datos de la base de datos de desarrollo en la base de datos de producción, y estas técnicas se pueden usar con seguridad para copiar los objetos de base de datos de servicios de aplicación en la base de datos de producción. Otra opción es la herramienta de `aspnet_regsql.exe`, que agrega o quita los objetos de base de datos de servicios de aplicación de una base de datos.

> [!NOTE]
> La herramienta `aspnet_regsql.exe` crea los objetos de base de datos en una base de datos especificada. No migra datos de estos objetos de base de datos de desarrollo a la base de datos de producción. Si quiere copiar la información de la cuenta de usuario y de los roles de la base de datos de desarrollo en la base de datos de producción, use las técnicas que se describen en el tutorial *implementación de una base de datos* .

Echemos un vistazo a cómo agregar los objetos de base de datos a la base de datos de producción mediante la herramienta `aspnet_regsql.exe`. Para empezar, abra el explorador de Windows y vaya al directorio .NET Framework versión 2,0 del equipo,% WINDIR% \ Microsoft. NET\Framework\v2.0.50727. Allí debe encontrar la herramienta `aspnet_regsql.exe`. Esta herramienta se puede usar desde la línea de comandos, pero también incluye una interfaz gráfica de usuario. Haga doble clic en el archivo de `aspnet_regsql.exe` para iniciar su componente gráfico.

La herramienta se inicia mostrando una pantalla de presentación que explica su propósito. Haga clic en siguiente para avanzar a la pantalla "seleccionar una opción de instalación", que se muestra en la figura 1. Desde aquí puede elegir agregar los objetos de base de datos de servicios de aplicación o quitarlos de una base de datos. Dado que deseamos agregar estos objetos a la base de datos de producción, seleccione la opción "configurar SQL Server para servicios de aplicación" y haga clic en siguiente.

[![optar por configurar SQL Server para Servicios de aplicación](configuring-a-website-that-uses-application-services-vb/_static/image2.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image1.jpg)

**Figura 1**: configuración de SQL Server para servicios de aplicación ([haga clic para ver la imagen de tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image3.jpg))

En la pantalla "seleccionar el servidor y la base de datos", se solicita información para conectarse a la base de datos. Escriba el servidor de base de datos, las credenciales de seguridad y el nombre de la base de datos que le proporcionó su empresa de hospedaje web y haga clic en siguiente.

> [!NOTE]
> Después de especificar el servidor de base de datos y las credenciales, puede obtener un error al expandir la lista desplegable base de datos. La herramienta `aspnet_regsql.exe` consulta la tabla del sistema `sysdatabases` para recuperar una lista de bases de datos en el servidor, pero algunas empresas de hospedaje web bloquean sus servidores de bases de datos para que esta información no esté disponible públicamente. Si obtiene este error, puede escribir el nombre de la base de datos directamente en la lista desplegable.

[![proporcionar la herramienta con la información de conexión de la base de datos](configuring-a-website-that-uses-application-services-vb/_static/image5.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image4.jpg)

**Figura 2**: proporcione la herramienta con la información de conexión de la base de datos ([haga clic para ver la imagen de tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image6.jpg))

En la pantalla siguiente se resumen las acciones que se van a realizar, es decir, que los objetos de base de datos de servicios de aplicación se van a agregar a la base de datos especificada. Haga clic en siguiente para completar esta acción. Transcurridos unos instantes, se muestra la pantalla final, teniendo en cuenta que se han agregado los objetos de base de datos (consulte la figura 3).

[![correcto. Los objetos de base de datos de Servicios de aplicación se agregaron a la base de datos de producción](configuring-a-website-that-uses-application-services-vb/_static/image8.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image7.jpg)

**Figura 3**: correcto. Los objetos de base de datos de Servicios de aplicación se agregaron a la base de datos de producción ([haga clic para ver la imagen de tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image9.jpg))

Para comprobar que los objetos de base de datos de servicios de aplicación se agregaron correctamente a la base de datos de producción, abra SQL Server Management Studio y conéctese a la base de datos de producción. Como se muestra en la figura 4, ahora debería ver las tablas de base de datos de servicios de aplicación en la base de datos, `aspnet_Applications`, `aspnet_Membership`, `aspnet_Users`, etc.

[![confirmar que los objetos de base de datos se agregaron a la base de datos de producción](configuring-a-website-that-uses-application-services-vb/_static/image11.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image10.jpg)

**Ilustración 4**: confirmar que los objetos de base de datos se agregaron a la base de datos de producción ([haga clic para ver la imagen de tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image12.jpg))

Solo tendrá que usar la herramienta `aspnet_regsql.exe` al implementar la aplicación web por primera vez o por primera vez después de haber empezado a usar los servicios de aplicación. Una vez que estos objetos de base de datos se encuentran en la base de datos de producción, no es necesario volver a agregar o modificar.

### <a name="copying-user-accounts-from-development-to-production"></a>Copia de cuentas de usuario desde el desarrollo hasta el entorno de producción

Al utilizar las clases de proveedor `SqlMembershipProvider` y `SqlRoleProvider` para almacenar la información de servicios de aplicación en una base de datos de SQL Server, la información de la cuenta de usuario y el rol se almacena en diversas tablas de base de datos, como `aspnet_Users`, `aspnet_Membership`, `aspnet_Roles`y `aspnet_UsersInRoles`, entre otros. Si durante el desarrollo crea cuentas de usuario en el entorno de desarrollo, puede replicar esas cuentas de usuario en producción copiando los registros correspondientes de las tablas de base de datos aplicables. Si utilizó el Asistente para la publicación de bases de datos para implementar los objetos de base de datos de servicios de aplicación, puede que también haya elegido copiar los registros, lo que daría lugar a que las cuentas de usuario creadas en desarrollo también estén en producción. Sin embargo, en función de los valores de configuración, es posible que los usuarios cuyas cuentas se crearon en desarrollo y se copien en producción no puedan iniciar sesión en el sitio web de producción. ¿Qué proporciona?

Las clases de proveedor `SqlMembershipProvider` y `SqlRoleProvider` se diseñaron de forma que una sola base de datos podría servir como almacén de usuario para varias aplicaciones, donde cada aplicación podría, en teoría, tener usuarios con nombres de usuario y roles superpuestos con el mismo nombre. Para permitir esta flexibilidad, la base de datos mantiene una lista de aplicaciones en la tabla de `aspnet_Applications` y cada usuario está asociado a una de estas aplicaciones. En concreto, la tabla `aspnet_Users` tiene una columna `ApplicationId` que une cada usuario a un registro de la tabla `aspnet_Applications`.

Además de la columna `ApplicationId`, la tabla `aspnet_Applications` incluye también una columna `ApplicationName`, que proporciona un nombre más descriptivo para la aplicación. Cuando un sitio web intenta trabajar con una cuenta de usuario, como la validación de las credenciales de un usuario desde la página de inicio de sesión, debe indicar a la clase de `SqlMembershipProvider` la aplicación con la que se va a trabajar. Normalmente esto lo hace proporcionando el nombre de la aplicación y este valor procede de la configuración del proveedor en `Web.config`, específicamente a través del atributo `applicationName`.

Pero ¿qué ocurre si el atributo `applicationName` no se especifica en `Web.config`? En tal caso, el sistema de pertenencia utiliza la ruta de acceso raíz de la aplicación como el valor `applicationName`. Si el atributo `applicationName` no se establece explícitamente en `Web.config`, existe la posibilidad de que el entorno de desarrollo y el entorno de producción utilicen una raíz de aplicación diferente y, por tanto, se asociarán a distintos nombres de aplicación en los servicios de aplicación. Si se produce un error de coincidencia, los usuarios creados en el entorno de desarrollo tendrán un valor `ApplicationId` que no coincide con el valor `ApplicationId` para el entorno de producción. El resultado neto es que esos usuarios no podrán iniciar sesión.

> [!NOTE]
> Si se encuentra en esta situación, con las cuentas de usuario copiadas en producción con un valor de `ApplicationId` no coincidente: puede escribir una consulta para actualizar estos valores `ApplicationId` incorrectos a los `ApplicationId` utilizados en producción. Una vez actualizados, los usuarios cuyas cuentas se crearon en el entorno de desarrollo ahora podrán iniciar sesión en la aplicación web en producción.

La buena noticia es que hay un paso sencillo que puede realizar para asegurarse de que los dos entornos usan el mismo `ApplicationId`: establezca explícitamente el atributo de `applicationName` en `Web.config` para todos los proveedores de servicios de aplicaciones. Establezca explícitamente el atributo `applicationName` en "BookReviews" en los elementos `<membership>` y `<roleManager>` como este fragmento de código de `Web.config` muestra.

[!code-xml[Main](configuring-a-website-that-uses-application-services-vb/samples/sample4.xml)]

Para obtener más información sobre cómo establecer el atributo de `applicationName` y la lógica subyacente, consulte la entrada de blog de [*Scott Guthrie*](https://weblogs.asp.net/scottgu/) s y [*establezca siempre la propiedad ApplicationName al configurar la pertenencia a ASP.net y otros proveedores*](https://weblogs.asp.net/scottgu/443634).

### <a name="managing-user-accounts-in-the-production-environment"></a>Administrar cuentas de usuario en el entorno de producción

La herramienta de administración de sitios web (WSAT) de ASP.NET facilita la creación y administración de cuentas de usuario, la definición y la aplicación de roles, y la escritura de reglas de autorización basadas en roles y usuarios. Puede iniciar el WSAT desde Visual Studio; para ello, vaya a la Explorador de soluciones y haga clic en el icono de configuración de ASP.NET o vaya a los menús del sitio web o proyecto y seleccione el elemento de menú configuración de ASP.NET. Desafortunadamente, el WSAT solo puede funcionar con sitios web locales. Por lo tanto, no puede usar el WSAT de su estación de trabajo para administrar el sitio web en el entorno de producción.

La buena noticia es que todas las funcionalidades expuestas que proporciona WSAT están disponibles mediante programación a través de las API de pertenencia y roles. Además, muchas de las pantallas de WSAT usan los controles estándar relacionados con el inicio de sesión de ASP.NET. En Resumen, puede agregar páginas ASP.NET a su sitio web que ofrezcan las capacidades de administración necesarias.

Recuerde que en un tutorial anterior se ha actualizado la aplicación web revisar el libro para incluir una `~/Admin` carpeta y que esta carpeta se ha configurado para permitir solo los usuarios con el rol administrador. He agregado una página a esa carpeta llamada `CreateAccount.aspx` desde la que un administrador puede crear una nueva cuenta de usuario. En esta página se usa el control CreateUserWizard para mostrar la interfaz de usuario y la lógica de back-end para crear una nueva cuenta de usuario. Lo que es más, personalizó el control para que incluya una casilla que le pregunte si el nuevo usuario también debe agregarse al rol de administrador (vea la figura 5). Con un poco de trabajo puede crear un conjunto personalizado de páginas que implementa las tareas relacionadas con la administración de usuarios y roles que, de lo contrario, proporcionaría el WSAT.

> [!NOTE]
> Para obtener más información sobre el uso de las API de pertenencia y roles junto con los controles Web de ASP.NET relacionados con el inicio de sesión, asegúrese de leer los [*tutoriales de seguridad de los sitios web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Para obtener más información sobre cómo personalizar el control CreateUserWizard, consulte los tutoriales [*creación de cuentas de usuario*](../../older-versions-security/membership/creating-user-accounts-vb.md) y almacenamiento de información de [*usuario adicional*](../../older-versions-security/membership/storing-additional-user-information-vb.md) , o consulte [*Erich Peterson*](http://www.erichpeterson.com/) s article, [*Personalización del control CreateUserWizard*](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx).

[![los administradores pueden crear nuevas cuentas de usuario](configuring-a-website-that-uses-application-services-vb/_static/image14.jpg)](configuring-a-website-that-uses-application-services-vb/_static/image13.jpg)

**Figura 5**: los administradores pueden crear nuevas cuentas de usuario ([haga clic para ver la imagen a tamaño completo](configuring-a-website-that-uses-application-services-vb/_static/image15.jpg))

Si necesita la funcionalidad completa de WSAT, consulte el tema sobre la creación de una [*herramienta de administración de sitios web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)en la que Author dan Clem le guía por el proceso de creación de una herramienta de tipo WSAT personalizada. Juan comparte el código fuente de la aplicación ( C#en) y proporciona instrucciones paso a paso para agregarlo al sitio web hospedado.

## <a name="summary"></a>Resumen

Al implementar una aplicación web que usa la implementación de base de datos de servicios de aplicación, primero debe asegurarse de que la base de datos de producción tenga los objetos de base de datos necesarios. Estos objetos se pueden agregar mediante las técnicas que se describen en el tutorial *implementación de una base de datos* ; como alternativa, puede usar la herramienta de `aspnet_regsql.exe`, como vimos en este tutorial. Otros desafíos que hemos tratado en el centro sobre la sincronización del nombre de la aplicación que se usa en los entornos de desarrollo y producción (que es importante si desea que los usuarios y los roles creados en el entorno de desarrollo sean válidos en producción) y técnicas para administrar los usuarios y los roles en el entorno de producción.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [*Herramienta de registro de SQL Server de ASP.NET (aspnet_regsql. exe)* ](https://msdn.microsoft.com/library/ms229862.aspx)
- [*Crear la base de datos de Servicios de aplicación para SQL Server*](https://msdn.microsoft.com/library/x28wfk74.aspx)
- [*Crear el esquema de pertenencia en SQL Server*](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-vb.md)
- [*Examen de la pertenencia, los roles y el perfil de ASP.NET s*](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [*Implementar su propia herramienta de administración de sitios web*](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [*Tutoriales de seguridad de sitios web*](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)
- [*Información general de la herramienta de administración de sitios web*](https://msdn.microsoft.com/library/yy40ytx0.aspx)

> [!div class="step-by-step"]
> [Anterior](configuring-the-production-web-application-to-use-the-production-database-vb.md)
> [Siguiente](strategies-for-database-development-and-deployment-vb.md)
