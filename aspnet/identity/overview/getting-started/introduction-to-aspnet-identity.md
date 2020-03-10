---
uid: identity/overview/getting-started/introduction-to-aspnet-identity
title: Introducción a ASP.NET Identity-ASP.NET 4. x
author: jongalloway
description: El sistema de pertenencia de ASP.NET se presentó con ASP.NET 2,0 de vuelta en 2005 y, desde entonces, se han producido muchos cambios en las distintas formas de aplicaciones Web...
ms.author: riande
ms.date: 01/22/2019
ms.assetid: 38717fc1-5989-43cf-952d-4007cc1dd923
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/introduction-to-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 0268dfc16cd2cfb1e79ee14997a4c5eb247af950
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78471739"
---
# <a name="introduction-to-aspnet-identity"></a>Introducción a ASP.NET Identity

> El sistema de pertenencia de ASP.NET se presentó con ASP.NET 2,0 de vuelta en 2005 y, desde entonces, se han producido muchos cambios en las maneras en las que las aplicaciones web normalmente administran la autenticación y la autorización. ASP.NET Identity es una nueva visión de lo que debe ser el sistema de pertenencia al compilar aplicaciones modernas para la web, el teléfono o la tableta.

## <a name="background-membership-in-aspnet"></a>Background: pertenencia a ASP.NET

### <a name="aspnet-membership"></a>Pertenencia a ASP.NET

La [pertenencia a ASP.net](https://msdn.microsoft.com/library/yh26yfzy(v=VS.100).aspx) se diseñó para resolver los requisitos de pertenencia a sitios que eran comunes en 2005, que implicaban la autenticación de formularios y una SQL Server base de datos para nombres de usuario, contraseñas y datos de perfil. En la actualidad, hay una matriz mucho más amplia de opciones de almacenamiento de datos para las aplicaciones web y la mayoría de los desarrolladores desean habilitar sus sitios para usar proveedores de identidades sociales para la funcionalidad de autenticación y autorización. Las limitaciones del diseño de la pertenencia a ASP.NET dificultan esta transición:

- El esquema de la base de datos se diseñó para SQL Server y no se puede cambiar. Puede agregar información de perfil, pero los datos adicionales se empaquetan en una tabla diferente, lo que dificulta el acceso a través de cualquier medio, excepto a través de la API del proveedor de perfiles.
- El sistema del proveedor permite cambiar el almacén de datos de respaldo, pero el sistema está diseñado en torno a las suposiciones adecuadas para una base de datos relacional. Puede escribir un proveedor para almacenar información de pertenencia en un mecanismo de almacenamiento no relacional, como Azure Storage tablas, pero después tiene que solucionar el diseño relacional escribiendo gran cantidad de código y muchas excepciones `System.NotImplementedException` para los métodos que no se aplican a las bases de datos NoSQL.
- Dado que la funcionalidad de inicio de sesión y cierre de sesión se basa en la autenticación de formularios, el sistema de pertenencia no puede usar [OWIN](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md). OWIN incluye componentes de middleware para la autenticación, incluida la compatibilidad con inicios de sesión mediante proveedores de identidades externos (por ejemplo, cuentas de Microsoft, Facebook, Google, Twitter) e inicios de sesión con cuentas de organización desde Active Directory locales o Azure Active Directory. OWIN también incluye compatibilidad con OAuth 2,0, JWT y CORS.

### <a name="aspnet-simple-membership"></a>Pertenencia simple de ASP.NET

La [pertenencia simple a ASP.net](../../../web-pages/overview/security/16-adding-security-and-membership.md) se desarrolló como un sistema de pertenencia para ASP.NET Web pages. Se lanzó con WebMatrix y Visual Studio 2010 SP1. El objetivo de la pertenencia simple era facilitar la incorporación de la funcionalidad de pertenencia a una aplicación de páginas Web.

La pertenencia simple facilitaba la personalización de la información de Perfil de usuario, pero también comparte los demás problemas con la pertenencia a ASP.NET y tiene algunas limitaciones:

- Era difícil conservar los datos del sistema de pertenencia en un almacén no relacional.
- No se puede usar con OWIN.
- No funciona bien con los proveedores de pertenencia a ASP.NET existentes y no es extensible.

### <a name="aspnet-universal-providers"></a>Proveedores universales de ASP.NET

[Proveedores universales de ASP.net](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) se desarrollaron para hacer posible la persistencia de la información de pertenencia en Microsoft Azure SQL Database y también funcionan con SQL Server Compact. El Proveedores universales se compiló en Entity Framework Code First, lo que significa que el Proveedores universales se puede usar para conservar los datos en cualquier almacén compatible con EF. Con el Proveedores universales, el esquema de la base de datos también se limpió bastante.

El Proveedores universales se basa en la infraestructura de pertenencia a ASP.NET, por lo que sigue teniendo las mismas limitaciones que el proveedor SqlMembership. Es decir, se diseñaron para bases de datos relacionales y es difícil personalizar la información del perfil y del usuario. Estos proveedores también usan la autenticación de formularios para la funcionalidad de inicio y cierre de sesión.

## <a name="aspnet-identity"></a>ASP.NET Identity

Como el caso de pertenencia de ASP.NET ha evolucionado a lo largo de los años, el equipo de ASP.NET ha aprendido mucho de los comentarios de los clientes.

La suposición de que los usuarios inician sesión especificando un nombre de usuario y una contraseña que se han registrado en su propia aplicación ya no son válidos. La web ha vuelto a ser más social. Los usuarios interactúan entre sí en tiempo real a través de canales sociales, como Facebook, Twitter y otros sitios web sociales. Los desarrolladores desean que los usuarios puedan iniciar sesión con sus identidades sociales para que puedan tener una experiencia enriquecida en sus sitios Web. Un sistema de pertenencia moderno debe habilitar los inicios de sesión basados en la redirección a proveedores de autenticación como Facebook, Twitter y otros.

A medida que evolucionó el desarrollo web, se hicieron los patrones de desarrollo web. La prueba unitaria del código de la aplicación se convirtió en una preocupación principal para los desarrolladores de aplicaciones. En 2008 ASP.NET agregó un nuevo marco basado en el patrón Model-View-Controller (MVC), en parte para ayudar a los desarrolladores a compilar aplicaciones ASP.NET de prueba unitarias. Los desarrolladores que quieren realizar pruebas unitarias de la lógica de la aplicación también querían poder hacerlo con el sistema de pertenencia.

Teniendo en cuenta estos cambios en el desarrollo de aplicaciones Web, ASP.NET Identity se desarrolló con los siguientes objetivos:

- **Un sistema ASP.NET Identity**

    - ASP.NET Identity puede usarse con todos los marcos de trabajo de ASP.NET, como ASP.NET MVC, formularios Web Forms, Web pages, Web API y Signalr.
    - ASP.NET Identity puede usarse al compilar aplicaciones Web, de teléfono, de tienda o híbridas.
- **Facilidad de conexión de datos de perfil sobre el usuario**

    - Tiene control sobre el esquema de información de usuario y perfil. Por ejemplo, puede habilitar fácilmente el sistema para que almacene fechas de nacimiento escritas por los usuarios al registrar una cuenta en la aplicación.

- **Control Persistence**

    - De forma predeterminada, el sistema ASP.NET Identity almacena toda la información de usuario en una base de datos. ASP.NET Identity usa Entity Framework Code First para implementar todo su mecanismo de persistencia.
    - Puesto que controla el esquema de la base de datos, las tareas comunes, como cambiar los nombres de tabla o cambiar el tipo de datos de las claves principales, son sencillas.
    - Es fácil conectar mecanismos de almacenamiento diferentes, como SharePoint, Azure Storage Table Service, bases de datos NoSQL, etc., sin tener que iniciar excepciones `System.NotImplementedExceptions`.
- **Capacidad de prueba unitaria**

    - ASP.NET Identity hace que la aplicación web sea más comprobable. Puede escribir pruebas unitarias para las partes de la aplicación que utilizan ASP.NET Identity.
- **Proveedor de funciones**

    - Hay un proveedor de roles que le permite restringir el acceso a las partes de la aplicación por parte de los roles. Puede crear fácilmente roles como "admin" y agregar usuarios a los roles.
- **Basado en notificaciones**

    - ASP.NET Identity admite la autenticación basada en notificaciones, donde la identidad del usuario se representa como un conjunto de notificaciones. Las notificaciones permiten a los desarrolladores ser mucho más expresivos en la descripción de la identidad de un usuario que los roles permitidos. Mientras que la pertenencia al rol es simplemente un booleano (miembro o no miembro), una demanda puede incluir información enriquecida sobre la identidad y la pertenencia del usuario.
- **Proveedores de inicio de sesión social**

    - Puede agregar fácilmente inicios de sesión sociales, como cuenta de Microsoft, Facebook, Twitter, Google y otros a su aplicación, y almacenar los datos específicos del usuario en la aplicación.

- **Integración OWIN**

    - La autenticación ASP.NET se basa ahora en middleware OWIN que se puede usar en cualquier host basado en OWIN. ASP.NET Identity no tiene ninguna dependencia en System. Web. Es un marco OWIN totalmente compatible y se puede usar en cualquier aplicación hospedada por OWIN.
    - ASP.NET Identity usa la autenticación OWIN para el inicio de sesión y el cierre de sesión de los usuarios en el sitio Web. Esto significa que, en lugar de usar FormsAuthentication para generar la cookie, la aplicación usa OWIN CookieAuthentication para hacerlo.
- **Paquete NuGet**

    - ASP.NET Identity se redistribuye como un paquete NuGet que se instala en las plantillas de ASP.NET MVC, Web Forms y Web API que se incluyen con Visual Studio 2017. Puede descargar este paquete de NuGet desde la galería de NuGet.
    - La liberación de ASP.NET Identity como un paquete NuGet facilita que el equipo de ASP.NET itere en nuevas características y correcciones de errores y los entregue a los desarrolladores de forma ágil.

## <a name="get-started-with-aspnet-identity"></a>Introducción a ASP.NET Identity

ASP.NET Identity se usa en las plantillas de proyecto de Visual Studio 2017 para ASP.NET MVC, Web Forms, Web API y SPA. En este tutorial, veremos cómo se usan las plantillas de proyecto ASP.NET Identity para agregar funcionalidad para registrarse, iniciar sesión y cerrar la sesión de un usuario.

ASP.NET Identity se implementa mediante el procedimiento siguiente. El propósito de este artículo es proporcionarle una introducción de alto nivel de ASP.NET Identity. puede seguirlo paso a paso o simplemente leer los detalles. Para obtener instrucciones más detalladas sobre cómo crear aplicaciones con ASP.NET Identity, incluido el uso de la nueva API para agregar usuarios, roles e información de perfil, consulte la sección pasos siguientes al final de este artículo.

1. Cree una aplicación de ASP.NET MVC con cuentas individuales. Puede usar ASP.NET Identity en ASP.NET MVC, Web Forms, Web API, Signalr, etc. En este artículo, comenzaremos con una aplicación ASP.NET MVC.  
  
    ![](introduction-to-aspnet-identity/_static/image1.png)
2. El proyecto creado contiene los tres paquetes siguientes para ASP.NET Identity.

    - [`Microsoft.AspNet.Identity.EntityFramework`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.EntityFramework/)  
   Este paquete tiene la implementación de Entity Framework de ASP.NET Identity que conservará los datos de ASP.NET Identity y el esquema que se SQL Server.
    - [`Microsoft.AspNet.Identity.Core`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Core/)  
   Este paquete tiene las interfaces principales para ASP.NET Identity. Este paquete se puede usar para escribir una implementación para ASP.NET Identity que tenga como destino distintos almacenes de persistencia, como Azure Table Storage, bases de datos NoSQL, etc.
    - [`Microsoft.AspNet.Identity.OWIN`](http://www.nuget.org/packages/Microsoft.AspNet.Identity.Owin/)  
   Este paquete contiene funcionalidad que se usa para conectar la autenticación OWIN con ASP.NET Identity en aplicaciones ASP.NET. Se usa cuando se agrega la funcionalidad de inicio de sesión a la aplicación y se llama a middleware de autenticación de cookies OWIN para generar una cookie.
3. Creación de un usuario.  
   Inicie la aplicación y, a continuación, haga clic en el vínculo **registrar** para crear un usuario. La siguiente imagen muestra la página de registro que recopila el nombre de usuario y la contraseña.  
  
    ![](introduction-to-aspnet-identity/_static/image2.png)  
  
   Cuando el usuario selecciona el botón **registrar** , la acción `Register` del controlador de cuenta crea el usuario mediante una llamada a la API de ASP.net Identity, como se resalta a continuación:

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample1.cs?highlight=8-9)]
4. Inicie sesión.  
   Si el usuario se creó correctamente, ha iniciado sesión en el método `SignInAsync`.  

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample6.cs?highlight=12)]

   El método `SignInManager.SignInAsync` genera un [ClaimsIdentity](https://msdn.microsoft.com/library/system.security.claims.claimsidentity.aspx). Como ASP.NET Identity y la autenticación de cookies OWIN son un sistema basado en notificaciones, el marco de trabajo requiere que la aplicación genere un ClaimsIdentity para el usuario. ClaimsIdentity contiene información sobre todas las notificaciones para el usuario, como los roles a los que pertenece el usuario.   
 
5. Cierre la sesión.  
   Seleccione el vínculo **Cerrar sesión** para llamar a la acción de cierre de sesión en el controlador de cuentas. 

    [!code-csharp[Main](introduction-to-aspnet-identity/samples/sample5.cs?highlight=6)]

   El código resaltado anterior muestra el método OWIN `AuthenticationManager.SignOut`. Esto es análogo al método [FormsAuthentication. SignOut](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx) usado por el módulo [FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx) en formularios Web Forms.

## <a name="components-of-aspnet-identity"></a>Componentes de ASP.NET Identity

En el diagrama siguiente se muestran los componentes del sistema ASP.NET Identity (seleccione en [este](introduction-to-aspnet-identity/_static/image3.png) o en el diagrama para ampliarlo). Los paquetes en verde componen el sistema ASP.NET Identity. Todos los demás paquetes son dependencias que se necesitan para usar el sistema ASP.NET Identity en aplicaciones ASP.NET.

[![](introduction-to-aspnet-identity/_static/image5.png)](introduction-to-aspnet-identity/_static/image4.png)

La siguiente es una breve descripción de los paquetes de NuGet no mencionados anteriormente:

- [Microsoft.Owin.Security.Cookies](http://www.nuget.org/packages/Microsoft.Owin.Security.Cookies/)  
 Middleware que permite que una aplicación use la autenticación basada en cookies, similar a la de ASP. Autenticación de formularios de la red.
- [EntityFramework](http://www.nuget.org/packages/EntityFramework/)  
 Entity Framework es la tecnología de acceso a datos recomendada por Microsoft para las bases de datos relacionales.

## <a name="migrating-from-membership-to-aspnet-identity"></a>Migración de la pertenencia a ASP.NET Identity

Esperamos proporcionar pronto instrucciones sobre cómo migrar las aplicaciones existentes que usan la pertenencia a ASP.NET o la pertenencia sencilla al nuevo sistema ASP.NET Identity.

## <a name="next-steps"></a>Pasos siguientes

- [Creación de una aplicación ASP.NET MVC 5 con el inicio de sesión de Facebook y Google OAuth2 y OpenID](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)  
 En el tutorial se usa la API de ASP.NET Identity para agregar información de perfil a la base de datos de usuario y cómo autenticarse con Google y Facebook.
- [Crear una aplicación ASP.NET MVC con la autenticación y Base de datos SQL e implementar a Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)  
 En este tutorial se muestra cómo usar la API de identidad para agregar usuarios y roles.
- [https://github.com/rustd/AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample)  
 Aplicación de ejemplo que muestra cómo agregar roles básicos y la compatibilidad con los usuarios, y cómo realizar roles y administración de usuarios.
