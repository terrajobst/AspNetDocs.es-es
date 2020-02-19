---
uid: identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
title: Migrar datos del proveedor universal para la pertenencia y los perfilesC#de usuario a ASP.net Identity ()-ASP.net 4. x
author: rustd
description: En este tutorial se describen los pasos necesarios para migrar datos de usuario y de rol, y datos de Perfil de usuario creados con Proveedores universales de una aplicación existente...
ms.author: riande
ms.date: 12/13/2013
ms.custom: seoapril2019
ms.assetid: 2e260430-d13c-4658-bd05-e256fc0d63b8
msc.legacyurl: /identity/overview/migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 31f02a0cec3c531c45c37b7aad8456e01e80b5ea
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456119"
---
# <a name="migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity-c"></a>Migrar datos de proveedor universal para la pertenencia y los perfiles de usuario a ASP.NET Identity (C#)

por [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://twitter.com/RickAndMSFT), [Robert McMurray](https://github.com/rmcmurray), [Suhas Joshi](https://github.com/suhasj)

> En este tutorial se describen los pasos necesarios para migrar datos de usuario y de rol, y datos de Perfil de usuario creados con Proveedores universales de una aplicación existente al modelo de ASP.NET Identity. El enfoque que se menciona aquí para migrar los datos de Perfil de usuario puede usarse también en una aplicación con pertenencia a SQL.

Con el lanzamiento de Visual Studio 2013, el equipo de ASP.NET presentó un nuevo sistema ASP.NET Identity y puede leer más sobre esa versión [aquí](../../index.md). En este artículo se explican los pasos para migrar aplicaciones Web de [la pertenencia a SQL al nuevo sistema de identidad](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md), en este artículo se describen los pasos para migrar las aplicaciones existentes que siguen el modelo de proveedores para la administración de usuarios y roles al nuevo modelo de identidad. Este tutorial se centrará principalmente en la migración de los datos de Perfil de usuario para enlazarlos sin problemas al nuevo sistema. La migración de información de usuarios y roles es similar para la pertenencia a SQL. El enfoque seguido para migrar los datos de perfil también se puede usar en una aplicación con la pertenencia a SQL.

Por ejemplo, comenzaremos con una aplicación Web creada con Visual Studio 2012 que usa el modelo de proveedores. Después, agregaremos código para la administración de perfiles, registrará un usuario, agregará datos de perfil para los usuarios, migrará el esquema de la base de datos y, a continuación, cambiará la aplicación para usar el sistema de identidades para la administración de usuarios y roles. Como prueba de la migración, los usuarios creados con Proveedores universales deberían poder iniciar sesión y los nuevos usuarios podrán registrarse.

> [!NOTE]
> Puede encontrar el ejemplo completo en [https://github.com/suhasj/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations).

## <a name="profile-data-migration-summary"></a>Resumen de migración de datos de perfil

Antes de comenzar con las migraciones, echemos un vistazo a la experiencia de almacenamiento de datos de perfil en el modelo de proveedores. Los datos de Perfil de los usuarios de la aplicación se pueden almacenar de varias maneras, lo más habitual es que se usen los proveedores de perfiles integrados que se suministran junto con el Proveedores universales. Entre los pasos se incluyen

1. Agregue una clase que tenga las propiedades que se usan para almacenar los datos del perfil.
2. Agregue una clase que extienda "ProfileBase" e implemente los métodos para obtener los datos de perfil anteriores para el usuario.
3. Habilite el uso de proveedores de perfiles predeterminados en el archivo *Web. config* y defina la clase declarada en el paso #2 que se va a usar para obtener acceso a la información de perfil.

La información del perfil se almacena como datos XML y binarios serializados en la tabla ' profiles ' de la base de datos.

Después de migrar la aplicación para usar el nuevo sistema de ASP.NET Identity, la información del perfil se deserializa y se almacena como propiedades en la clase User. Cada propiedad se puede asignar a las columnas de la tabla de usuario. La ventaja es que las propiedades pueden trabajar directamente con la clase de usuario, además de no tener que serializar o deserializar la información de los datos cada vez que se accede a ella.

## <a name="getting-started"></a>Introducción

1. Cree una nueva aplicación de formularios Web Forms de ASP.NET 4,5 en Visual Studio 2012. En el ejemplo actual se usa la plantilla de formularios Web Forms, pero también se puede usar la aplicación MVC.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.jpg)
2. Crear una nueva carpeta ' models ' para almacenar información de perfil  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image1.png)
3. Por ejemplo, permítanos almacenar la fecha de nacimiento, la ciudad, el alto y el peso del usuario en el perfil. El alto y el peso se almacenan como una clase personalizada denominada ' PersonalStats '. Para almacenar y recuperar el perfil, necesitamos una clase que extienda "ProfileBase". Vamos a crear una nueva clase ' AppProfile ' para obtener y almacenar información de perfil.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample1.cs)]
4. Habilite el perfil en el archivo *Web. config* . Escriba el nombre de clase que se va a usar para almacenar o recuperar la información de usuario creada en el paso #3.

    [!code-xml[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample2.xml)]
5. Agregue una página de formularios Web Forms en la carpeta ' account ' para obtener los datos de perfil del usuario y almacenarlos. Haga clic con el botón derecho en el proyecto y seleccione "Agregar nuevo elemento". Agregue una nueva página Web Forms con la página maestra ' AddProfileData. aspx '. Copie lo siguiente en la sección ' MainContent ':

    [!code-html[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample3.html)]

   Agregue el código siguiente en el código subyacente:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample4.cs)]

   Agregue el espacio de nombres en el que se define la clase AppProfile para eliminar los errores de compilación.
6. Ejecute la aplicación y cree un nuevo usuario con el nombre de usuario "**olduser".** Vaya a la página "AddProfileData" y agregue información de perfil para el usuario.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.png)

Puede comprobar que los datos se almacenan como XML serializado en la tabla ' profiles ' mediante el Explorador de servidores ventana. En Visual Studio, en el menú "ver", elija "Explorador de servidores". Debe haber una conexión de datos para la base de datos definida en el archivo *Web. config* . Al hacer clic en la conexión de datos se muestran distintas subcategorías. Expanda ' tablas ' para mostrar las diferentes tablas de la base de datos, haga clic con el botón derecho en ' perfiles ' y elija ' Mostrar datos de tabla ' para ver los datos de perfil almacenados en la tabla de perfiles.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.png)

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image4.png)

## <a name="migrating-database-schema"></a>Migración del esquema de la base de datos

Para que la base de datos existente funcione con el sistema de identidad, es necesario actualizar el esquema de la base de datos de identidad para que admita los campos que se agregaron a la base de datos original. Esto puede hacerse mediante scripts SQL para crear nuevas tablas y copiar la información existente. En la ventana "Explorador de servidores", expanda "DefaultConnection" para mostrar las tablas. Haga clic con el botón derecho en tablas y seleccione ' nueva consulta '

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image5.png)

Pegue el script SQL de [https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt](https://raw.github.com/suhasj/UniversalProviders-Identity-Migrations/master/Migration.txt) y ejecútelo. Si se actualiza "DefaultConnection", podemos ver que se han agregado las nuevas tablas. Puede comprobar los datos de las tablas para ver que se ha migrado la información.

![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image6.png)

## <a name="migrating-the-application-to-use-aspnet-identity"></a>Migración de la aplicación para utilizar ASP.NET Identity

1. Instale los paquetes de Nuget necesarios para ASP.NET Identity:

    - Microsoft.AspNet.Identity.EntityFramework
    - Microsoft.AspNet.Identity.Owin
    - Microsoft.Owin.Host.SystemWeb
    - Microsoft.Owin.Security.Facebook
    - Microsoft.Owin.Security.Google
    - Microsoft.Owin.Security.MicrosoftAccount
    - Microsoft.Owin.Security.Twitter

   Puede encontrar más información sobre la administración de paquetes Nuget [aquí](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog)
2. Para trabajar con los datos existentes en la tabla, es necesario crear clases de modelo que se asignan a las tablas y se enlazan en el sistema de identidades. Como parte del contrato de identidad, las clases de modelo deben implementar las interfaces definidas en el archivo dll de identidad. Core o pueden extender la implementación existente de estas interfaces disponibles en Microsoft. AspNet. Identity. EntityFramework. Usaremos las clases existentes para el rol, los inicios de sesión de usuario y las notificaciones de usuario. Necesitamos usar un usuario personalizado para nuestro ejemplo. Haga clic con el botón derecho en el proyecto y cree una nueva carpeta ' IdentityModels '. Agregue una nueva clase ' User ' como se muestra a continuación:

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample5.cs)]

   Observe que ' ProfileInfo ' es ahora una propiedad en la clase de usuario. Por lo tanto, podemos usar la clase User para trabajar directamente con datos de perfil.

Copie los archivos de las carpetas **IdentityModels** y **IdentityAccount** del origen de descarga ( [https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations](https://github.com/suhasj/UniversalProviders-Identity-Migrations/tree/master/UniversalProviders-Identity-Migrations) ). Estos tienen las clases de modelo restantes y las nuevas páginas necesarias para la administración de usuarios y roles mediante el ASP.NET Identity API. El enfoque que se usa es similar a la pertenencia a SQL y se puede encontrar una explicación detallada [aquí](migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md).

[!INCLUDE[](../../../includes/identity/alter-command-exception.md)]

## <a name="copying-profile-data-to-the-new-tables"></a>Copiar datos de perfil en las nuevas tablas

Como se mencionó anteriormente, es necesario deserializar los datos XML en las tablas de perfiles y almacenarlos en las columnas de la tabla AspNetUsers. Las nuevas columnas se crearon en la tabla users del paso anterior, de modo que todo lo que queda es rellenar esas columnas con los datos necesarios. Para ello, usaremos una aplicación de consola que se ejecuta una vez para rellenar las columnas recién creadas en la tabla de usuarios.

1. Cree una nueva aplicación de consola en la solución de salida.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image2.jpg)
2. Instale la versión más reciente del paquete de Entity Framework.
3. Agregue la aplicación Web creada anteriormente como referencia a la aplicación de consola. Para ello, haga clic con el botón derecho en el proyecto, luego en "Agregar referencias", en solución, en el proyecto y en Aceptar.
4. Copie el código siguiente en la clase Program.cs. Esta lógica Lee los datos de Perfil de cada usuario, los serializa como objeto "ProfileInfo" y los almacena de nuevo en la base de datos.

    [!code-csharp[Main](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/samples/sample6.cs)]

   Algunos de los modelos utilizados se definen en la carpeta ' IdentityModels ' del proyecto de aplicación Web, por lo que debe incluir los espacios de nombres correspondientes.
5. El código anterior funciona en el archivo de base de datos de la carpeta app\_data del proyecto de aplicación web creado en los pasos anteriores. Para hacer referencia a ella, actualice la cadena de conexión en el archivo app. config de la aplicación de consola con la cadena de conexión en el archivo Web. config de la aplicación Web. Proporcione también la ruta de acceso física completa en la propiedad ' AttachDbFilename '.
6. Abra un símbolo del sistema y vaya a la carpeta bin de la aplicación de consola anterior. Ejecute el archivo ejecutable y revise la salida del registro tal como se muestra en la siguiente imagen.  
    ![](migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity/_static/image3.jpg)
7. Abra la tabla ' AspNetUsers ' en el Explorador de servidores y compruebe los datos de las columnas nuevas que contienen las propiedades. Deben actualizarse con los valores de propiedad correspondientes.

## <a name="verify-functionality"></a>Comprobar la funcionalidad

Use las páginas de pertenencia recién agregadas que se implementan mediante ASP.NET Identity para iniciar sesión de un usuario de la base de datos anterior. El usuario debe poder iniciar sesión con las mismas credenciales. Pruebe el resto de funcionalidades, como la adición de OAuth, la creación de un nuevo usuario, el cambio de una contraseña, la adición de roles, la incorporación de usuarios a roles, etc.

Los datos de perfil para el usuario anterior y los nuevos usuarios se deben recuperar y almacenar en la tabla de usuarios. Ya no se debe hacer referencia a la tabla anterior.

## <a name="conclusion"></a>Conclusión

En el artículo se describe el proceso de migración de aplicaciones web que usaban el modelo de proveedor para la pertenencia a ASP.NET Identity. En el artículo también se describe la migración de datos de perfil para que los usuarios se enlacen al sistema de identidad. Deje comentarios a continuación para preguntas y problemas que surjan al migrar la aplicación.

*Gracias a Rick Anderson y Robert McMurray para revisar el artículo.*
