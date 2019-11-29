---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
title: Usuarios y roles en el sitio web de producción (VB) | Microsoft Docs
author: rick-anderson
description: La herramienta de administración de sitios web de ASP.NET (WSAT) proporciona una interfaz de usuario basada en web para configurar las opciones de pertenencia y roles, y para crear, editar,...
ms.author: riande
ms.date: 06/09/2009
ms.assetid: 491ed5ae-9be1-4191-87be-65e4e1c57690
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-vb
msc.type: authoredcontent
ms.openlocfilehash: d4ce8b278322684be2d44faefd6e69fc524bbe18
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74617918"
---
# <a name="users-and-roles-on-the-production-website-vb"></a>Usuarios y roles en el sitio web de producción (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](https://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial16_CustomAWAT_vb.pdf)

> La herramienta de administración de sitios web de ASP.NET (WSAT) proporciona una interfaz de usuario basada en web para configurar las opciones de pertenencia y roles, y para crear, editar y eliminar usuarios y roles. Desafortunadamente, el WSAT solo funciona cuando se visita desde localhost, lo que significa que no se puede tener acceso a la herramienta de administración del sitio web de producción a través del explorador. La buena noticia es que hay soluciones alternativas que permiten administrar usuarios y roles en producción. En este tutorial se examinan estas soluciones alternativas y otras.

## <a name="introduction"></a>Introducción

ASP.NET 2,0 presentó una serie de *servicios de aplicación*, que son un conjunto de servicios de bloques de creación que se pueden agregar a la aplicación Web. Hemos agregado los servicios de pertenencia y roles al sitio web de revisiones del libro en el [tutorial *configuración de un sitio web que usa servicios de aplicación* ](configuring-a-website-that-uses-application-services-vb.md). El servicio de pertenencia facilita la creación y administración de cuentas de usuario. el servicio de roles ofrece una API para clasificar los usuarios en grupos. El sitio de reseñas de libros tiene tres cuentas de usuario: Scott, Jisun y Alice, y un único rol, admin, con Scott y Jisun en el rol administrador.

ASP. Los servicios de aplicación de la red no están asociados a una implementación específica. En su lugar, se indica a los servicios de aplicación que usen un *proveedor*determinado y ese proveedor implementa el servicio mediante una tecnología determinada. Hemos configurado la aplicación Web Book reviews para usar los proveedores de `SqlMembershipProvider` y `SqlRoleProvider` de los servicios de pertenencia y roles. Estos dos proveedores almacenan información de roles y cuentas de usuario en una base de datos de SQL Server y son los proveedores más usados para las aplicaciones web basadas en Internet hospedadas en una empresa de hospedaje Web.

Un desafío común para los desarrolladores que usan los servicios de pertenencia y roles es la administración de los usuarios y los roles en el entorno de producción. ¿Cómo se elimina una cuenta de usuario del sitio web de producción, se agrega un nuevo rol o se agrega un usuario existente a un rol existente? En este tutorial se exploran distintas técnicas para administrar usuarios y roles en el sitio web de producción.

## <a name="using-the-aspnet-web-site-administration-tool"></a>Uso de la herramienta de administración de sitios Web ASP.NET

ASP.NET incluye una [herramienta de administración de sitios web](https://msdn.microsoft.com/library/yy40ytx0.aspx) (WSAT) que facilita la creación y administración de roles y cuentas de usuario, y la especificación de reglas de autorización basadas en roles y usuarios. Para usar WSAT, haga clic en el icono de configuración de ASP.NET en el Explorador de soluciones, o bien, vaya al menú de proyecto o sitio web y elija la opción de configuración ASP.NET. Cualquier enfoque inicia un explorador Web y lo apunta a WSAT en una dirección como: `http://localhost:portNumber/asp.netwebadminfiles/default.aspx?applicationPhysicalPath=pathToApplication`

El WSAT se divide en tres secciones:

- **Seguridad** : administrar usuarios, roles y reglas de autorización.
- **ApplicationConfiguration** : administre la configuración de &lt;appSettings&gt; y SMTP desde aquí. También puede desconectar la aplicación y administrar la configuración de la depuración y el seguimiento desde aquí, así como especificar la página de error personalizada predeterminada.
- **ProviderConfiguration** : Configure los proveedores que usan los servicios de aplicación.

La sección de seguridad (que se muestra en la **figura 1**) incluye vínculos para crear nuevos usuarios, administrar usuarios, crear y administrar roles, y crear y administrar reglas de acceso. Desde aquí puede Agregar un nuevo rol al sistema, eliminar un usuario existente o agregar o quitar roles de una cuenta de usuario determinada.

[![](users-and-roles-on-the-production-website-vb/_static/image2.png)](users-and-roles-on-the-production-website-vb/_static/image1.png)

**Figura 1**: la sección de seguridad de WSAT incluye opciones para administrar usuarios y roles  
([Haga clic para ver la imagen de tamaño completo](users-and-roles-on-the-production-website-vb/_static/image3.png))

Desafortunadamente, el WSAT solo es accesible localmente. No puede visitar WSAT en su sitio web de producción remota; Si visita `www.yoursite.com/asp.netwebadminfiles/default.aspx` obtendrá una respuesta 404 no encontrada. El código que alimenta el WSAT usa las clases `Membership` y `Roles` de la .NET Framework para crear, editar y eliminar usuarios y roles. Estas clases consultan la información de configuración de la aplicación web para determinar qué proveedor usar; de vuelta en la página [ *configuración de un sitio web que usa servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-vb.md) , se configura el sitio web de reseñas de libros para usar los proveedores de `SqlMembershipProvider` y `SqlRoleProvider`. Esto supondría agregar `<membership>` y `<roleManager>` secciones a `Web.config`.

[!code-xml[Main](users-and-roles-on-the-production-website-vb/samples/sample1.xml)]

Tenga en cuenta que las secciones `<membership>` y `<roleManager>` hacen referencia a los proveedores `SqlMembershipProvider` y `SqlRoleProvider` en su `type` atributo, respectivamente. Estos proveedores almacenan la información de usuario y de roles en una base de datos de SQL Server especificada. La base de datos utilizada por estos proveedores se especifica mediante el atributo `connectionStringName`, `ReviewsConnectionString`, que se define en el archivo `~/ConfigSections/databaseConnectionStrings.config`. Recuerde que el archivo de `databaseConnectionStrings.config` en el entorno de desarrollo contiene la cadena de conexión a la base de datos de desarrollo, mientras que el archivo de `databaseConnectionStrings.config` en producción contiene la cadena de conexión a la base de datos de producción.

En pocas palabras, se debe tener acceso a WSAT localmente a través del entorno de desarrollo y funciona con la información del usuario y del rol en la base de datos especificada en el archivo de `databaseConnectionStrings.config`. Por lo tanto, si cambiamos la información de la cadena de conexión en el archivo `databaseConnectionStrings.config` en el entorno de desarrollo, podemos usar el WSAT localmente para administrar usuarios y roles en el entorno de producción.

Para ilustrar esta funcionalidad, abra el archivo de `databaseConnectionStrings.config` en Visual Studio en el entorno de desarrollo y reemplace la cadena de conexión de la base de datos de desarrollo por la cadena de conexión de la base de datos de producción. A continuación, inicie WSAT, vaya a la pestaña seguridad y agregue un nuevo usuario llamado Sam con la contraseña "contraseña". (menos las comillas). En la **ilustración 2** se muestra la pantalla WSAT al crear esta cuenta.

[![](users-and-roles-on-the-production-website-vb/_static/image5.png)](users-and-roles-on-the-production-website-vb/_static/image4.png)

**Figura 2**: creación de un nuevo usuario llamado Sam en el entorno de producción  
([Haga clic para ver la imagen de tamaño completo](users-and-roles-on-the-production-website-vb/_static/image6.png))

Dado que hemos cambiado la cadena de conexión de `databaseConnectionStrings.config` para que apunte al servidor de base de datos de producción, Sam se agregó como usuario en el entorno de producción. Para comprobarlo, vuelva a cambiar la cadena de conexión del `databaseConnectionStrings.config` archivo a la base de datos de desarrollo y, a continuación, visite la página `Login.aspx` en el entorno de desarrollo. Intente iniciar sesión como Sam (consulte la **figura 3**).

[![](users-and-roles-on-the-production-website-vb/_static/image8.png)](users-and-roles-on-the-production-website-vb/_static/image7.png)

**Figura 3**: no puede iniciar sesión como Sam en el entorno de desarrollo  
([Haga clic para ver la imagen de tamaño completo](users-and-roles-on-the-production-website-vb/_static/image9.png))

No se puede iniciar sesión como Sam en el entorno de desarrollo porque la información de la cuenta de usuario no existe en la base de datos local. En su lugar, se ha agregado a la base de datos de producción. Para comprobarlo, vea el contenido de la tabla `aspnet_Users` en las bases de datos de desarrollo y de producción. En el entorno de desarrollo debe haber solo tres registros para los usuarios Scott, Jisun y Alice. Sin embargo, la tabla de `aspnet_Users` de la base de datos de producción tiene cuatro registros: Scott, JISUN, Alice y Sam. Por lo tanto, Sam puede iniciar sesión a través del sitio web en producción, pero no a través del entorno de desarrollo.

[![](users-and-roles-on-the-production-website-vb/_static/image11.png)](users-and-roles-on-the-production-website-vb/_static/image10.png)

**Figura 4**: Sam puede iniciar sesión en el sitio web de producción  
([Haga clic para ver la imagen de tamaño completo](users-and-roles-on-the-production-website-vb/_static/image12.png))

> [!NOTE]
> No olvide cambiar la cadena de conexión del archivo de `databaseConnectionStrings.config` a la cadena de conexión de la base de datos de desarrollo cuando haya terminado de trabajar con el WSAT. de lo contrario, trabajará con datos de producción al probar el sitio a través del entorno de desarrollo. Tenga en cuenta también que aunque la técnica que se acaba de describir nos permite usar WSAT para administrar de forma remota los usuarios y los roles, los cambios en cualquiera de las otras opciones de configuración de WSAT (reglas de acceso, configuración de SMTP, configuración de depuración y seguimiento, etc.) modifican el archivo de `Web.config`. Por lo tanto, los cambios realizados en la configuración se aplican al entorno de desarrollo y no al entorno de producción.

## <a name="creating-custom-user-and-role-management-web-pages"></a>Creación de páginas web personalizadas de administración de roles y usuarios

El WSAT proporciona un sistema de forma integrada para administrar usuarios y roles, pero solo se puede iniciar localmente y requiere realizar cambios en la información de la cadena de conexión para administrar los usuarios y los roles en producción. La mayoría de los sitios web que admiten las cuentas de usuario también incluyen una serie de páginas web de administración de usuarios y roles que permiten a los administradores administrar usuarios y roles desde páginas del sitio. Estas páginas de administración basadas en Web facilitan la administración de usuarios y roles, y son esenciales para los sitios en los que puede haber muchos administradores o administradores que no tienen acceso a o el técnico para usar Visual Studio para iniciar el WSAT.

ASP.NET incluye una serie de controles Web relacionados con el inicio de sesión que hacen que la implementación de muchas de estas páginas web administrativas sea tan fácil como arrastrar y colocar. Por ejemplo, puede crear una página para que los administradores creen una nueva cuenta de usuario arrastrando el control CreateUserWizard hasta la página y estableciendo algunas propiedades. De hecho, la página para crear usuarios en el WSAT que se muestra en la **figura 2** usa el mismo control CreateUserWizard que se puede Agregar a las páginas. Además, las funcionalidades de los servicios de pertenencia y roles están disponibles mediante programación a través de las clases `Membership` y `Roles` de la .NET Framework. Con estas clases puede escribir código para crear, editar y eliminar usuarios y roles, así como para agregar o quitar usuarios a roles, para determinar qué usuarios son los roles y para realizar otras tareas relacionadas con el usuario y los roles.

En el [tutorial *configuración de un sitio web que usa servicios de aplicación* ](configuring-a-website-that-uses-application-services-vb.md) he agregado una página a la carpeta `Admin` denominada `CreateAccount.aspx`. Esta página permite a un administrador agregar una nueva cuenta de usuario al sitio y especificar si el usuario recién creado está en el rol de administrador (vea la **figura 5**).

[![](users-and-roles-on-the-production-website-vb/_static/image14.png)](users-and-roles-on-the-production-website-vb/_static/image13.png)

**Figura 5**: los administradores pueden crear nuevas cuentas de usuario  
([Haga clic para ver la imagen de tamaño completo](users-and-roles-on-the-production-website-vb/_static/image15.png))

Para obtener información más detallada sobre la creación de páginas de administración de usuarios y roles, junto con instrucciones paso a paso sobre el uso de las clases `Membership` y `Roles` y los controles Web ASP.NET relacionados con el inicio de sesión, asegúrese de leer los [tutoriales de seguridad de los sitios web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md). Allí encontrará instrucciones sobre cómo crear páginas web para crear nuevas cuentas, crear y administrar roles, asignar usuarios a roles y otras tareas administrativas comunes.

Para implementar la funcionalidad de tipo WSAT en el sitio web de producción, siempre puede crear su propia serie de páginas web que implementan las características de WSAT. Para ayudar a empezar, consulte el código fuente de WSAT, que se encuentra en la carpeta `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\ASP.NETWebAdminFiles`. Otra opción consiste en usar la alternativa WSAT de dan Clem, que comparte en este artículo, para [lanzar su propia herramienta de administración de sitios web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx). Dan guiará a los lectores a través del proceso de creación de una herramienta personalizada de WSAT, que incluye el código fuente de C#la aplicación para su descarga (en) y proporciona instrucciones paso a paso para agregar su WSAT personalizado a un sitio web hospedado.

## <a name="summary"></a>Resumen

La herramienta de administración de sitios web (WSAT) de ASP.NET se puede usar junto con los servicios de aplicaciones de roles y de pertenencia para administrar la información de usuarios y roles de su sitio Web. Desafortunadamente, el WSAT solo es accesible localmente y no se puede visitar desde su sitio web de producción. Sin embargo, al cambiar la cadena de conexión en el entorno de desarrollo para que apunte a la base de datos de producción, puede usar WSAT para administrar los usuarios y los roles en el sitio web de producción.

Aunque el enfoque de WSAT ofrece una manera rápida y sencilla de administrar usuarios y roles, necesita iniciar WSAT desde Visual Studio, así como cambios temporales en la información de la cadena de conexión. WSAT ofrece una manera rápida de administrar usuarios y roles en producción, pero es engorroso y no funciona bien para sitios web con varios administradores o con administradores que no tienen o no están familiarizados con Visual Studio y con WSAT. Por estos motivos, la mayoría de los sitios web que admiten cuentas de usuario incluyen un conjunto de páginas web administrativas. Este conjunto de páginas web elimina la necesidad de WSAT y la usan varios usuarios administrativos desde cualquier equipo.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Examinando ASP. Pertenencia, roles y Perfil de la red](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Implementar su propia herramienta de administración de sitios web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)
- [Información general de la herramienta de administración de sitios web](https://msdn.microsoft.com/library/yy40ytx0.aspx)
- [Tutoriales de seguridad de sitios web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Anterior](precompiling-your-website-vb.md)
