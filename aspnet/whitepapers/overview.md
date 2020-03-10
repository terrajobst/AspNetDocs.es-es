---
uid: whitepapers/overview
title: Notas del producto | Microsoft Docs
author: rick-anderson
description: En esta página encontrará notas del producto para ayudarle a instalar y configurar ASP.NET y para ayudarle a escribir aplicaciones ASP.NET seguras, rápidas y flexibles.
ms.author: riande
ms.date: 11/15/2011
ms.assetid: d5e79470-01f2-4d65-8077-11c3e10a6784
msc.legacyurl: /whitepapers
msc.type: content
ms.openlocfilehash: 1a3a9fe5d685d4b38efe666fc88ff57016482ada
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78520591"
---
# <a name="whitepapers"></a>Notas del producto

> En esta página encontrará notas del producto para ayudarle a instalar y configurar ASP.NET y para ayudarle a escribir aplicaciones ASP.NET seguras, rápidas y flexibles.
> 
> - [ASP.NET 4](#aspnet4)
> - [Notas del producto de seguridad de ASP.NET](#security)
> - [Notas del producto de instalación y configuración](#setup)
> - [Notas del producto SQL Server](#sql)
> - [Notas del producto generales](#general)

<a id="aspnet4"></a>
## <a name="aspnet-4"></a>ASP.NET 4

Información relacionada con ASP.NET 4 y Visual Studio 2010.

[Notas de la versión de ASP.NET MVC 4](mvc4-beta-release-notes.md "Notas de la versión de MVC4")

En este documento se describen las nuevas características y mejoras introducidas en ASP.NET MVC 4 Developer Preview para Visual Studio 2010, así como notas de instalación y problemas conocidos.

[Notas de la versión de ASP.NET MVC 3](mvc3-release-notes.md "Notas de la versión de MVC3")

En este documento se describen las nuevas características y mejoras introducidas en ASP.NET MVC 3, así como las notas de instalación y los problemas conocidos.

[Información general sobre la implementación web de ASP.NET 4 y Visual Studio 2010](aspnet4/index.md "aspnet4")

Muchos cambios emocionantes para ASP.NET están disponibles en la versión 4 de .NET Framework. En este documento se proporciona información general sobre muchas de las nuevas características que se incluyen en la próxima versión.

[Cambios principales de ASP.NET 4 beta 2](aspnet4/breaking-changes.md "cambios problemáticos")

En este documento se describen los cambios realizados en la versión beta 2 de .NET Framework versión 4 (es decir, la versión beta 2 de ASP.NET 4) que pueden afectar a las aplicaciones que se crearon con versiones anteriores, incluida la versión de ASP.NET 4 beta 1.

[Novedades de ASP.NET MVC 2](what-is-new-in-aspnet-mvc.md "Novedades de ASPNET MVC")

En este documento se describen las nuevas características y mejoras introducidas en ASP.NET MVC 2.

[Actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2](aspnet-mvc2-upgrade-notes.md "ASPNET-mvc2-upgrade-Notes")

ASP.NET MVC 2 se puede instalar en paralelo con ASP.NET MVC 1,0 en el mismo servidor. Esto proporciona a los desarrolladores de aplicaciones flexibilidad a la hora de elegir cuándo actualizar una aplicación ASP.NET MVC 1,0 a ASP.NET MVC 2. En este documento se describe cómo actualizar manualmente y con un asistente en visual...

<a id="security"></a>
## <a name="aspnet-security-whitepapers"></a>Notas del producto de seguridad de ASP.NET

La seguridad es un aspecto importante de las aplicaciones de Internet y en estas notas del producto se explica cómo diseñar e implementar aplicaciones de ASP.NET seguras.

[Instrumentar aplicaciones ASP.NET 2,0 para la seguridad](https://msdn.microsoft.com/library/ms998325.aspx)

En este procedimiento se muestra cómo usar eventos personalizados de supervisión de estado para instrumentar la aplicación ASP.NET para realizar un seguimiento de las operaciones y los eventos relacionados con la seguridad. La versión 2,0 de ASP.NET proporciona supervisión de estado que incluye la instrumentación de muchos estándares...

[Realización de una revisión de implementación de seguridad para ASP.NET 2,0](https://msdn.microsoft.com/library/ms998367.aspx)

En este procedimiento se muestra cómo realizar una revisión de la implementación de la seguridad de una aplicación ASP.NET 2,0 para identificar posibles vulnerabilidades de seguridad introducidas por valores de configuración inadecuados. La mayoría del proceso de revisión implica la realización de...

[Usar ADAM para roles en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998331.aspx)

En este procedimiento se muestra cómo se puede desarrollar un sitio web de ASP.NET que usa Active Directory Application Mode (ADAM) para almacenar roles de ASP.NET. Muestra cómo configurar ADAM y el almacén de directivas del administrador de autorización (AzMan), cómo crear nuevos roles y...

[Usar administrador de autorización (AzMan) con ASP.NET 2,0](https://msdn.microsoft.com/library/ms998336.aspx)

En este procedimiento se muestra cómo usar el administrador de autorización (AzMan) junto con la API del administrador de roles de ASP.NET para administrar roles, comprobar la pertenencia a roles de usuario y autorizar roles para realizar operaciones específicas en un almacén de directivas de AzMan. ...

[Usar la pertenencia a ASP.NET 2,0](https://msdn.microsoft.com/library/ms998347.aspx)

En este procedimiento se muestra cómo usar la característica de pertenencia en aplicaciones de la versión 2,0 de ASP.NET. Muestra cómo usar dos proveedores de pertenencia diferentes: ActiveDirectoryMembershipProvider y SqlMembershipProvider. La característica de pertenencia...

[Usar el administrador de roles en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998314.aspx)

En este procedimiento se muestra cómo usar el administrador de roles de ASP.NET 2,0. El administrador de roles facilita la tarea de administrar roles y realizar la autorización basada en roles en la aplicación. Muestra cómo configurar los distintos proveedores de roles para su uso con...

[Usar la autenticación de Windows en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998358.aspx)

En este procedimiento se muestra cómo configurar y usar la autenticación de Windows en una aplicación Web ASP.NET. La autenticación de Windows es el método preferido siempre que los usuarios formen parte de su dominio de Windows. Este enfoque le permite usar un almacén de identidades existente...

[Realización de una revisión de código de seguridad para código administrado (actividad de línea de base)](https://msdn.microsoft.com/library/ms998364.aspx)

En este procedimiento se muestra cómo realizar revisiones de código de seguridad. Este módulo presenta los pasos implicados en la actividad y técnicas para analizar los resultados. Use este procedimiento con "lista de preguntas de seguridad: código administrado (.NET Framework 2,0)"...

[Realización de una revisión de implementación de seguridad para ASP.NET 2,0](https://msdn.microsoft.com/library/ms998367.aspx)

En este procedimiento se muestra cómo realizar una revisión de la implementación de la seguridad de una aplicación ASP.NET 2,0 para identificar posibles vulnerabilidades de seguridad introducidas por valores de configuración inadecuados. La mayoría del proceso de revisión implica la realización de...

[Implementar la delegación Kerberos para Windows 2000](https://msdn.microsoft.com/library/aa302400.aspx)

La delegación de Kerberos le permite fluir una identidad autenticada entre varios niveles físicos de una aplicación para admitir la autenticación y autorización de bajada. En este procedimiento se explican los pasos de configuración necesarios para realizar este trabajo.

[Usar la suplantación y la delegación en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998351.aspx)

En este procedimiento se muestra cómo y cuándo se debe usar la suplantación en aplicaciones ASP.NET 2,0. De forma predeterminada, la suplantación está desactivada y puede tener acceso a los recursos mediante la identidad de proceso de la aplicación Web ASP.NET. Sin embargo, puede usar...

[Crear un modelo de amenazas para una aplicación web en tiempo de diseño](https://msdn.microsoft.com/library/ms978527.aspx)

En este artículo se describe un enfoque para crear un modelo de amenazas para una aplicación Web. La actividad de modelado de amenazas le ayuda a modelar el diseño de seguridad para que pueda exponer posibles errores y vulnerabilidades de diseño de seguridad antes de invertir...

### <a name="forms-authentication"></a>Autenticación mediante formularios

[Protección de la autenticación de formularios en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998310.aspx)

En este procedimiento se muestra cómo configurar y usar la autenticación de formularios de forma segura con las aplicaciones de ASP.NET 2,0. Los factores clave a tener en cuenta incluyen la protección correcta del vale de autenticación y la protección del almacén de identidades de usuario y el acceso a dicho almacén. ...

[Usar la autenticación de formularios con Active Directory en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998360.aspx)

En este procedimiento se muestra cómo usar la autenticación de formularios con el servicio de directorio de Microsoft® Active Directory® mediante ActiveDirectoryMembershipProvider. En el procedimiento se muestra cómo configurar el proveedor y crear y autenticar usuarios....

[Usar la autenticación de formularios con Active Directory en varios dominios en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998345.aspx)

En este procedimiento se muestra cómo usar la autenticación de formularios con el servicio de directorio de Microsoft® Active Directory® mediante ActiveDirectoryMembershipProvider. En el procedimiento se muestra cómo configurar el proveedor y crear y autenticar usuarios....

[Usar la autenticación de formularios con SQL Server en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998317.aspx)

En este procedimiento se muestra cómo se puede utilizar la autenticación de formularios con el proveedor de pertenencia a SQL Server. La autenticación de formularios con SQL Server es más aplicable en situaciones en las que los usuarios de la aplicación no forman parte de su dominio de Windows y, como resultado,...

[Crear objetos GenericPrincipal con autenticación de formularios en ASP.NET 1,1](https://msdn.microsoft.com/library/aa302399.aspx)

En este procedimiento se muestra cómo crear y controlar objetos GenericPrincipal y FormsIdentity al utilizar la autenticación de formularios.

[Usar la autenticación de formularios con Active Directory en ASP.NET 1,1](https://msdn.microsoft.com/library/aa302397.aspx)

En este artículo se muestra cómo implementar la autenticación mediante formularios en un almacén de credenciales de Active Directory.

[Usar la autenticación de formularios con SQL Server en ASP.NET 1,1](https://msdn.microsoft.com/library/aa302398.aspx)

En este procedimiento se muestra cómo implementar la autenticación de formularios en un almacén de credenciales de SQL Server. También se muestra cómo almacenar resúmenes de contraseña en la base de datos.

### <a name="user-input-data-validation"></a>Validación de datos de entrada del usuario

[Validación de solicitudes - Prevenir ataques de scripts](request-validation.md "solicitud-validación")

En este documento se describe la característica de validación de solicitudes de ASP.NET en la que, de forma predeterminada, se evita que la aplicación procese el contenido HTML sin codificar que se envía al servidor. Esta característica de validación de solicitudes se puede deshabilitar cuando la aplicación está...

[Evitar el scripting entre sitios en ASP.NET](https://msdn.microsoft.com/library/ms998274.aspx)

En este procedimiento se muestra cómo puede ayudar a proteger sus aplicaciones ASP.NET de ataques de scripts entre sitios mediante técnicas de validación de entrada adecuadas y codificando la salida. También se describen otros mecanismos de protección que puede usar en...

[Protección contra la inyección de SQL en ASP.NET](https://msdn.microsoft.com/library/ms998271.aspx)

En este procedimiento se muestra una serie de maneras de ayudar a proteger la aplicación de ASP.NET frente a ataques por inyección de código SQL. La inyección de SQL puede producirse cuando una aplicación utiliza la entrada para construir instrucciones SQL dinámicas o cuando utiliza procedimientos almacenados para conectarse a...

[Usar expresiones regulares para restringir la entrada en ASP.NET](https://msdn.microsoft.com/library/ms998267.aspx)

En este procedimiento se muestra cómo puede usar expresiones regulares dentro de las aplicaciones de ASP.NET para restringir la entrada que no es de confianza. Las expresiones regulares son una buena manera de validar campos de texto como nombres, direcciones, números de teléfono y otra información de usuario. Puede usar...

### <a name="code-access-security"></a>Seguridad de acceso del código

[Usar la seguridad de acceso del código en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998326.aspx)

En este procedimiento se muestra cómo seleccionar un nivel de confianza adecuado para su aplicación y, si es necesario, cómo crear un archivo de directiva de seguridad de acceso del código de ASP.NET personalizado para definir un nivel de confianza personalizado. Puede usar una confianza de seguridad de acceso del código diferente...

[Crear un permiso de cifrado personalizado](https://msdn.microsoft.com/library/aa302362.aspx)

En este artículo se describe cómo crear un permiso de seguridad de acceso del código personalizado para controlar el acceso mediante programación a la funcionalidad de cifrado no administrada que proporciona Win32® la API de protección de datos (DPAPI). Usar este permiso personalizado con el contenedor DPAPI administrado...

[Usar la Directiva de seguridad de acceso del código para restringir un ensamblado](https://msdn.microsoft.com/library/aa302361.aspx)

Un administrador puede configurar la Directiva de seguridad de acceso del código para restringir las operaciones de .NET Framework código (ensamblados). En este procedimiento, configurará la Directiva de seguridad de acceso del código para restringir la capacidad de un ensamblado para realizar operaciones de e/s de archivos y restringir...

### <a name="communications-security"></a>Seguridad de las comunicaciones

[Configuración de SSL en un servidor Web](https://msdn.microsoft.com/library/aa302411.aspx)

Se debe configurar un servidor web para SSL con el fin de admitir conexiones https desde aplicaciones cliente. En este procedimiento se muestra cómo configurar SSL en un servidor Web.

[Configuración de certificados de cliente](https://msdn.microsoft.com/library/aa302412.aspx)

IIS admite la autenticación de certificados de cliente. En este procedimiento se muestra cómo configurar una aplicación web para requerir certificados de cliente. También se muestra cómo instalar un certificado en un equipo cliente y usarlo cuando se llama a la aplicación Web.

[Usar IPSec para filtrar los puertos y la autenticación](https://msdn.microsoft.com/library/aa302366.aspx)

El protocolo de seguridad de Internet (IPSec) es un protocolo, no un servicio, que proporciona servicios de cifrado, integridad y autenticación para el tráfico de red basado en IP. Dado que IPSec proporciona protección de servidor a servidor, puede usar IPSec para contrarrestar las amenazas internas...

[Usar IPSec para proporcionar comunicación segura entre dos servidores](https://msdn.microsoft.com/library/aa302413.aspx)

IPSec es una tecnología proporcionada por Windows 2000 que permite crear canales cifrados entre dos servidores. IPSec se puede usar para filtrar el tráfico IP y autenticar los servidores. En este procedimiento se muestra cómo configurar IPSec para proporcionar una protección segura (cifrada)...

[Usar SSL para proteger la comunicación con SQL Server](https://msdn.microsoft.com/library/aa302414.aspx)

A menudo, es fundamental que las aplicaciones puedan proteger los datos que se pasan a y desde un servidor de base de datos de SQL Server. Con SQL Server, puede usar SSL para crear un canal cifrado. En este procedimiento se muestra cómo instalar un certificado en el servidor de base de datos,...

[Llamar a un servicio Web mediante certificados de cliente de ASP.NET 1,1](https://msdn.microsoft.com/library/aa302408.aspx)

En este artículo se describe cómo puede pasar un certificado de cliente a un servicio web para la autenticación desde una aplicación Web de ASP.NET o desde una aplicación Windows Forms. Puede instalar el certificado de cliente en el almacén del equipo local o en el almacén del usuario. Si...

[Llamar a un servicio Web mediante SSL desde ASP.NET 1,1](https://msdn.microsoft.com/library/aa302409.aspx)

Se puede usar el cifrado de Capa de sockets seguros (SSL) para garantizar la integridad y confidencialidad de los mensajes pasados a un servicio Web y desde él. En este procedimiento se muestra cómo usar SSL con servicios Web.

### <a name="cryptography"></a>Criptografía

[Crear una biblioteca DPAPI en .NET 1,1](https://msdn.microsoft.com/library/aa302402.aspx)

En este procedimiento se muestra cómo crear una biblioteca de clases administrada que exponga la funcionalidad DPAPI a las aplicaciones que deseen cifrar datos, por ejemplo, las cadenas de conexión a bases de datos y las credenciales de la cuenta.

[Crear una biblioteca de cifrado en .NET 1,1](https://msdn.microsoft.com/library/aa302405.aspx)

En este procedimiento se muestra cómo crear una biblioteca de clases administrada para proporcionar la funcionalidad de cifrado para las aplicaciones. Permite que una aplicación Elija el algoritmo de cifrado. Entre los algoritmos admitidos se incluyen DES, triple DES, RC2 y Rijndael.

[Almacenar una cadena de conexión cifrada en el registro en ASP.NET 1,1](https://msdn.microsoft.com/library/aa302406.aspx)

Las aplicaciones pueden optar por almacenar datos cifrados, como cadenas de conexión y credenciales de cuenta en el registro de Windows. En este procedimiento se muestra cómo almacenar y recuperar cadenas cifradas en el registro.

[Usar DPAPI (almacén de la máquina) desde ASP.NET 1,1](https://msdn.microsoft.com/library/aa302403.aspx)

En este procedimiento se muestra cómo usar DPAPI desde un servicio web o una aplicación Web ASP.NET para cifrar la información confidencial.

[Usar DPAPI (almacén de usuarios) de ASP.NET 1,1 con Enterprise Services](https://msdn.microsoft.com/library/aa302404.aspx)

En este procedimiento se muestra cómo usar DPAPI desde un servicio o aplicación Web ASP.NET para cifrar la información confidencial. En este procedimiento se explica cómo usar DPAPI con el almacén de usuario, que requiere el uso de un componente de servicios empresariales fuera de proceso.

<a id="setup"></a>
## <a name="installation-and-setup-whitepapers"></a>Notas del producto de instalación y configuración

En estas notas del producto se proporcionan instrucciones paso a paso para instalar y configurar ASP.NET en el servidor.

[Creación de una cuenta de servicio para una aplicación ASP.NET 2,0](https://msdn.microsoft.com/library/ms998297.aspx)

En este procedimiento se muestra cómo crear y configurar una cuenta de servicio personalizada con privilegios mínimos para ejecutar una aplicación Web ASP.NET. De forma predeterminada, una aplicación de ASP.NET en Microsoft Windows Server 2003 e IIS 6,0 se ejecuta con el servicio de red integrado...

[Mejorar la seguridad al hospedar varias aplicaciones en ASP.NET 2,0](https://msdn.microsoft.com/library/aa480478.aspx)

En este procedimiento se muestra cómo puede aislar varias aplicaciones entre sí y desde recursos del sistema compartidos en un entorno de hospedaje Web. El entorno de hospedaje puede ser un servidor Web proporcionado por un proveedor de servicios Internet (ISP) que hospede varios...

[Usar confianza media en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998341.aspx)

En este procedimiento se muestra cómo configurar las aplicaciones Web de ASP.NET para que se ejecuten en un nivel de confianza medio. Si hospeda varias aplicaciones en el mismo servidor, puede usar la seguridad de acceso del código y el nivel de confianza medio para proporcionar aislamiento de la aplicación. Estableciendo...

[Usar la cuenta de servicio de red para tener acceso a los recursos de ASP.NET 2,0](https://msdn.microsoft.com/library/ms998320.aspx)

En este procedimiento se muestra cómo se puede usar la cuenta de equipo NT AUTHORITY\Network Service para tener acceso a recursos locales y de red. De forma predeterminada en Windows Server 2003, las aplicaciones de ASP.NET se ejecutan con la identidad de esta cuenta. Es un mínimo de privilegios...

[Usar transición de protocolo y delegación restringida en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998355.aspx)

En este procedimiento se muestra cómo configurar y usar la transición de protocolo y la delegación restringida para permitir que la aplicación ASP.NET acceda a los recursos de red durante la suplantación del autor de la llamada original. El sistema operativo Microsoft® Windows® 2000...

[Ejecución en paralelo de ASP.NET de .NET Framework 1.0 y 1.1](side-by-side-with-10.md "en paralelo con 1,0")

En estas notas del producto se describe cómo instalar .NET 1,0 y .NET 1,1 en el equipo, lo que permite que una aplicación Web ASP.NET se ejecute en cualquier versión del marco.

[Acceso denegado de ASP.NET a los directorios de IIS](denied-access-to-iis-directories.md "acceso denegado a los directorios de IIS")

En estas notas del producto se describe lo que debe hacer si una solicitud a la aplicación ASP.NET devuelve el error "denegado el acceso al directorio *directoryname* . No se pudo iniciar la supervisión de los cambios de directorio. "

[Ejecutar ASP.NET 1.1 con IIS 6.0](aspnet-and-iis6.md "ASPNET y IIS6")

Aunque Windows Server 2003 incluye tanto IIS 6,0 como ASP.NET 1,1, estos componentes están deshabilitados de forma predeterminada. En estas notas del producto se describe cómo habilitar IIS 6,0 y ASP.NET 1,1, y se recomiendan varias opciones de configuración para obtener la mejor opción...

[Corrección del error "la aplicación de servidor no está disponible" después de aplicar la actualización de seguridad para IE](ms03-32-issue.md "MS03-32-problema")

En este documento se describe la revisión que corrige un problema con la actualización de seguridad de MS03-32 para Internet Explorer que afecta a las aplicaciones de ASP.NET 1,0 que se ejecutan en Windows XP Professional.

[Crear una cuenta personalizada para ejecutar ASP.NET 1,1](https://msdn.microsoft.com/library/aa302396.aspx)

Las aplicaciones Web de ASP.NET normalmente se ejecutan con la cuenta de ASPNET integrada. En ocasiones, puede que desee usar una cuenta personalizada en su lugar. En este artículo se muestra cómo crear una cuenta local con privilegios mínimos para ejecutar aplicaciones Web de ASP.NET. ...

### <a name="configuration"></a>Configuración

[Configuración de la clave de equipo en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998288.aspx)

En este artículo se explica el elemento &lt;machineKey&gt; en el archivo Web. config y se muestra cómo configurar el elemento &lt;machineKey&gt; para controlar las pruebas de alteración y el cifrado de ViewState, los vales de autenticación de formularios y las cookies de rol. ViewState está firmado...

[Cifrar secciones de configuración en ASP.NET 2,0 con DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)

En este procedimiento se muestra cómo usar el proveedor de configuración protegida de la interfaz de programación de aplicaciones de protección de datos (DPAPI) de Windows y la herramienta ASPNET\_regiis. exe para cifrar secciones de los archivos de configuración. Puede usar la herramienta ASPNET\_regiis. exe para...

[Cifrar secciones de configuración en ASP.NET 2,0 con RSA](https://msdn.microsoft.com/library/ms998283.aspx)

En este procedimiento se muestra cómo usar el proveedor de configuración protegida de RSA y la herramienta ASPNET\_regiis. exe para cifrar secciones de los archivos de configuración. Puede usar ASPNET\_herramienta regiis. exe para cifrar datos confidenciales, como cadenas de conexión, contenidos en...

[Usar la suplantación y la delegación en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998351.aspx)

En este procedimiento se muestra cómo y cuándo se debe usar la suplantación en aplicaciones ASP.NET 2,0. De forma predeterminada, la suplantación está desactivada y puede tener acceso a los recursos mediante la identidad de proceso de la aplicación Web ASP.NET. Sin embargo, puede usar...

<a id="sql"></a>
## <a name="sql-server-whitepapers"></a>Notas del producto SQL Server

Aunque ASP.NET funciona con una variedad de bases de datos, estas notas del producto son específicas para la conexión de aplicaciones ASP.NET a SQL Server.

[Conexión a SQL Server mediante la autenticación de SQL en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998300.aspx)

En este procedimiento se muestra cómo conectar una aplicación ASP.NET de forma segura a Microsoft® SQL Server™ cuando la autenticación de acceso a bases de datos usa la autenticación de SQL nativa. La autenticación de Windows es la forma recomendada de conectarse a SQL Server porque...

[Conexión a SQL Server mediante la autenticación de Windows en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998292.aspx)

En este procedimiento se muestra cómo conectarse a SQL Server 2000 mediante una cuenta de servicio de Windows desde una aplicación de la versión 2,0 de ASP.NET. Siempre que sea posible, debe usar la autenticación de Windows en lugar de la autenticación de SQL, ya que evita almacenar credenciales en...

[Usar SSL para proteger la comunicación con SQL Server 2000](https://msdn.microsoft.com/library/aa302414.aspx)

A menudo, es fundamental que las aplicaciones puedan proteger los datos que se pasan a y desde un servidor de base de datos de SQL Server. Con SQL Server, puede usar SSL para crear un canal cifrado. En este procedimiento se muestra cómo instalar un certificado en el servidor de base de datos,...

<a id="general"></a>
## <a name="general-whitepapers"></a>Notas del producto generales

En el caso práctico siguiente se describe el proceso que se usó para migrar los sitios web de la comunidad de .NET de Microsoft de un entorno de hospedaje tradicional a Microsoft Azure.

[Migrar sitios web de la comunidad de ASP.NET y IIS.NET de Microsoft a Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=400656)

Estas notas del producto cubren una gran variedad de temas relacionados con ASP.NET.

[Uso de la supervisión de estado en ASP.NET 2,0](https://msdn.microsoft.com/library/ms998306.aspx)

En este procedimiento se muestra cómo usar la supervisión de estado para instrumentar la aplicación para un evento personalizado. Para crear un evento de supervisión de estado personalizado, cree una clase que derive de System. Web. Management. WebBaseEvent, configure el &lt;healthMonitoring&gt;...

[Implementar la administración de revisiones](https://msdn.microsoft.com/library/aa302364.aspx)

En este artículo se explica la administración de revisiones, incluido cómo mantener actualizados uno o varios servidores. No se requiere software adicional, salvo las herramientas disponibles para su descarga de Microsoft. Las operaciones y la Directiva de seguridad deben adoptar una administración de revisiones...

[Controles de servidor ASP.NET para Silverlight en el SDK de Silverlight 3](https://go.microsoft.com/fwlink/?LinkId=153377)

Los controles de servidor ASP.NET para Silverlight ("ASP.NET Silverlight Controls"), que son los controles MediaPlayer y Silverlight de ASP.NET, se han quitado del SDK de Silverlight para Silverlight versión 3. En este documento se proporcionan instrucciones para los desarrolladores que trabajaron con estos ASP.NET...

[Creación de aplicaciones Web de alto rendimiento](https://devexpress.com/act)

Aprenda a usar las nuevas características de ASP.NET AJAX Library para compilar aplicaciones Web de alto rendimiento.
