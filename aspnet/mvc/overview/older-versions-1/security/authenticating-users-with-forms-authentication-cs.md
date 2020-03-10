---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Autenticación de usuarios con autenticación de formulariosC#() | Microsoft Docs
author: microsoft
description: Aprenda a usar el atributo [Authorize] para proteger las páginas específicas de la aplicación MVC. También aprenderá a usar la administración de sitios Web...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: bed2eafa47fec25ac04cb07e0037f596494bb7d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78435451"
---
# <a name="authenticating-users-with-forms-authentication-c"></a>Autenticar a los usuarios con la autenticación de formularios (C#)

por [Microsoft](https://github.com/microsoft)

> Aprenda a usar el atributo [Authorize] para proteger las páginas específicas de la aplicación MVC. Aprenderá a usar la herramienta de administración de sitios web para crear y administrar usuarios y roles. También aprenderá a configurar dónde se almacena la información de la cuenta de usuario y el rol.

El objetivo de este tutorial es explicar cómo puede utilizar la autenticación de formularios para proteger las vistas en las aplicaciones ASP.NET MVC. Aprenderá a usar la herramienta de administración de sitios web para crear usuarios y roles. También aprenderá a evitar que usuarios no autorizados invoquen acciones de controlador. Por último, aprenderá a configurar dónde se almacenan los nombres de usuario y las contraseñas.

#### <a name="using-the-web-site-administration-tool"></a>Usar la herramienta de administración de sitios web

Antes de hacer nada más, debemos empezar por crear algunos usuarios y roles. La forma más fácil de crear nuevos usuarios y roles es aprovechar la herramienta de administración de sitios web de Visual Studio 2008. Puede iniciar esta herramienta seleccionando la opción de menú **proyecto, configuración de ASP.net**. Como alternativa, puede iniciar la herramienta de administración de sitios web haciendo clic en el icono de (en cierto modo) del martillo que llega al mundo que aparece en la parte superior de la ventana de Explorador de soluciones (consulte la figura 1).

**Figura 1: Inicio de la herramienta de administración de sitios web**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

Dentro de la herramienta de administración de sitios web, puede crear nuevos usuarios y roles; para ello, seleccione la pestaña seguridad. Haga clic en el vínculo **crear usuario** para crear un nuevo usuario llamado Stephen (consulte la figura 2). Proporcione al usuario Stephen la contraseña que desee (por ejemplo, *Secret*).

**Figura 2: creación de un nuevo usuario**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Para crear nuevos roles, primero se habilitan los roles y se definen uno o varios roles. Habilite los roles haciendo clic en el vínculo **habilitar funciones** . A continuación, cree un rol denominado *administradores* ; para ello, haga clic en el vínculo **crear o administrar roles** (consulte la figura 3).

**Figura 3: creación de un nuevo rol**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Por último, cree un nuevo usuario llamado Sally y asocie Sally con el rol administradores; para ello, haga clic en el vínculo crear usuario y seleccione administradores al crear Sally (consulte la figura 4).

**Ilustración 4: agregar un usuario a un rol**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Cuando se dice y se hace todo, debe tener dos nuevos usuarios denominados Stephen y Sally. También debe tener un nuevo rol denominado administradores. Sally es un miembro del rol Administrators y Stephen no lo es.

#### <a name="requiring-authorization"></a>Requerir autorización

Puede requerir que un usuario se autentique antes de que el usuario invoque una acción del controlador agregando el atributo [Authorize] a la acción. Puede aplicar el atributo [Authorize] a una acción de controlador individual o puede aplicar este atributo a una clase de controlador completa.

Por ejemplo, el controlador de la lista 1 expone una acción denominada CompanySecrets (). Dado que esta acción está decorada con el atributo [Authorize], esta acción no se puede invocar a menos que se autentique un usuario.

**Lista 1: Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Si invoca la acción CompanySecrets () escribiendo la dirección URL/Home/CompanySecrets en la barra de direcciones del explorador y no es un usuario autenticado, se le redirigirá a la vista de inicio de sesión automáticamente (consulte la figura 5).

**Figura 5: la vista de inicio de sesión**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

Puede usar la vista inicio de sesión para escribir su nombre de usuario y contraseña. Si no es un usuario registrado, puede hacer clic en el vínculo **registrar** para ir a la vista de registro (vea la figura 6). Puede usar la vista registro para crear una nueva cuenta de usuario.

**Ilustración 6: vista de registro**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Después de iniciar sesión correctamente, puede ver la vista CompanySecrets (consulte la figura 7). De forma predeterminada, seguirá iniciando sesión hasta que cierre la ventana del explorador.

**Ilustración 7: vista de CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorización por nombre de usuario o rol de usuario

Puede usar el atributo [Authorize] para restringir el acceso a una acción de controlador a un conjunto determinado de usuarios o a un conjunto determinado de roles de usuario. Por ejemplo, el controlador de inicio modificado en la lista 2 contiene dos nuevas acciones denominadas StephenSecrets () y AdministratorSecrets ().

**Lista 2: Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Solo un usuario con el nombre de usuario Stephen puede invocar la acción StephenSecrets (). Todos los demás usuarios se redirigen a la vista de inicio de sesión. La propiedad users acepta una lista separada por comas de nombres de cuentas de usuario.

Solo los usuarios del rol administradores pueden invocar la acción AdministratorSecrets (). Por ejemplo, dado que Sally es un miembro del grupo administradores, puede invocar la acción AdministratorSecrets (). Todos los demás usuarios se redirigen a la vista de inicio de sesión. La propiedad roles acepta una lista separada por comas de nombres de rol.

#### <a name="configuring-authentication"></a>Configuración de la autenticación

Llegados a este punto, es posible que se pregunte dónde se almacena la información de la cuenta de usuario y el rol. De forma predeterminada, la información se almacena en una base de datos SQL Express de (RANU) denominada ASPNETDB. MDF que se encuentra en la aplicación de la aplicación MVC\_la carpeta Data. Esta base de datos la genera el marco de trabajo de ASP.NET automáticamente al empezar a usar la pertenencia.

Para ver la base de datos ASPNETDB. MDF en la ventana Explorador de soluciones, primero debe seleccionar la opción de menú proyecto, Mostrar todos los archivos.

El uso de la base de datos SQL Express predeterminada es adecuado al desarrollar una aplicación. Sin embargo, lo más probable es que no desee usar la base de datos predeterminada ASPNETDB. MDF para una aplicación de producción. En ese caso, puede cambiar la ubicación en la que se almacena la información de la cuenta de usuario; para ello, complete los dos pasos siguientes:

1. Agregue el Servicios de aplicación objetos de base de datos a la base de datos de producción: cambie la cadena de conexión de la aplicación para que apunte a la base de datos de producción.

El primer paso consiste en Agregar todos los objetos de base de datos necesarios (tablas y procedimientos almacenados) a la base de datos de producción. La forma más fácil de agregar estos objetos a una nueva base de datos es aprovechar las ventajas del Asistente para la instalación de ASP.NET SQL Server (vea la figura 8). Puede iniciar esta herramienta abriendo el símbolo del sistema de Visual Studio 2008 desde el grupo de programas Microsoft Visual Studio 2008 y ejecutando el siguiente comando desde el símbolo del sistema:

ASPNET\_regsql

**Figura 8: Asistente para la instalación de ASP.NET SQL Server**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

El Asistente para la instalación de ASP.NET SQL Server le permite seleccionar una base de datos SQL Server en la red e instalar todos los objetos de base de datos necesarios para los servicios de aplicación de ASP.NET. No es necesario que el servidor de base de datos se encuentre en el equipo local.

> [!NOTE] 
> 
> Si no desea usar el Asistente para la instalación de ASP.NET SQL Server, puede encontrar scripts SQL para agregar los objetos de base de datos de servicios de aplicación en la siguiente carpeta:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727

Después de crear los objetos de base de datos necesarios, debe modificar la conexión de base de datos utilizada por la aplicación MVC. Modifique la cadena de conexión de ApplicationServices en el archivo de configuración Web (Web. config) para que apunte a la base de datos de producción. Por ejemplo, la conexión modificada de la lista 3 apunta a una base de datos denominada MyProductionDB (la cadena de conexión de ApplicationServices original se ha comentado).

**Lista 3: Web. config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configuración de permisos de base de datos

Si usa la seguridad integrada para conectarse a la base de datos, tendrá que agregar la cuenta de usuario de Windows correcta como inicio de sesión en la base de datos. La cuenta correcta depende de si utiliza el Servidor de desarrollo de ASP.NET o Internet Information Services como servidor Web. La cuenta de usuario correcta también depende del sistema operativo.

Si usa el Servidor de desarrollo de ASP.NET (el servidor Web predeterminado que usa Visual Studio), la aplicación se ejecuta en el contexto de su cuenta de usuario de Windows. En ese caso, debe agregar la cuenta de usuario de Windows como un inicio de sesión de servidor de bases de datos.

Como alternativa, si usa Internet Information Services, debe agregar la cuenta ASPNET o la cuenta NT AUTHORITY/NETWORK SERVICE como un inicio de sesión de servidor de bases de datos. Si usa Windows XP, agregue la cuenta ASPNET como un inicio de sesión a la base de datos. Si usa un sistema operativo más reciente, como Windows Vista o Windows Server 2008, agregue la cuenta NT AUTHORITY/NETWORK SERVICE como el inicio de sesión de la base de datos.

Puede Agregar una nueva cuenta de usuario a la base de datos mediante Microsoft SQL Server Management Studio (consulte la figura 9).

**Figura 9: creación de un nuevo inicio de sesión de Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Después de crear el inicio de sesión necesario, debe asignar el inicio de sesión a un usuario de base de datos con los roles de base de datos correctos. Haga doble clic en el inicio de sesión y seleccione la pestaña asignación de usuarios. Seleccione uno o varios roles de base de datos de servicios de aplicación. Por ejemplo, para autenticar a los usuarios, debe habilitar el rol de base de datos ASPNET\_Membership\_BasicAccess. Para crear nuevos usuarios, debe habilitar el rol de base de datos ASPNET\_Membership\_FullAccess (vea la figura 10).

**Figura 10: adición de roles de base de datos Servicios de aplicación**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Resumen

En este tutorial, aprendió a usar la autenticación de formularios al compilar una aplicación ASP.NET MVC. En primer lugar, aprendió a crear nuevos usuarios y roles aprovechando la herramienta de administración de sitios Web. A continuación, aprendió a usar el atributo [Authorize] para evitar que usuarios no autorizados invoquen acciones de controlador. Por último, ha aprendido a configurar la aplicación MVC para almacenar información de usuarios y roles en una base de datos de producción.

> [!div class="step-by-step"]
> [Siguiente](authenticating-users-with-windows-authentication-cs.md)
