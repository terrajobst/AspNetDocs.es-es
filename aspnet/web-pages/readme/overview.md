---
uid: web-pages/readme/overview
title: Archivo Léame de WebMatrix | Microsoft Docs
author: rick-anderson
description: Archivo Léame de la versión 1,0 de WebMatrix y ASP.NET Web Pages (Razor)
ms.author: riande
ms.date: 01/06/2011
ms.assetid: 36c5beeb-45a7-48a0-9c30-f82cdf5c5f5f
msc.legacyurl: /web-pages/readme
msc.type: content
ms.openlocfilehash: fac53e935860a90d8f2aa96699d56d66ade3a40f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78454273"
---
# <a name="webmatrix-readme"></a>Archivo Léame de WebMatrix

13 de enero de 2011

## <a name="contents"></a>Contenido

> [!NOTE]
> Este archivo Léame se aplica a la versión 1,0 de WebMatrix.

- [Información general](#Overview)
- [Instalación](#Installation_Notes)
- [Publicación de aplicaciones](#InstructionsForPublishingApplications)
- [Cambios y problemas](#ChangesAndIssues)

    - [Instalación de WebMatrix 1,0](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET) (Más información sobre páginas web de ASP.NET)
    - [WebMatrix](#Known_Issues_WebMatrix)
    - [IIS Express](#Known_Issues_IISExpress)
    - [SQL Server Compact](#Known_Issues_SQLServerCompact)
    - [Instalación de aplicaciones](#Known_Issues_Installing_Applications)
    - [Publicar aplicaciones](#Known_Issues_Publishing_Applications)
- [Para obtener más información](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Información general

> Microsoft WebMatrix 1,0 es una pila de desarrollo web gratuita que se instala en minutos. Integra un servidor Web con marcos de trabajo de programación y base de datos para crear una experiencia única e integrada. Puede usar WebMatrix para simplificar la forma de codificar, probar y publicar su propio sitio Web ASP.NET o PHP, o puede usar WebMatrix para iniciar un nuevo sitio web con aplicaciones populares de código abierto como DotNetNuke, Umbraco, WordPress o Joomla. WebMatrix usa el mismo servidor Web, motor de base de datos y entorno de marcos que ejecutará el sitio web en Internet, lo que hace que la transición de desarrollo a producción sea fluida y fluida.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalación

> Para instalar WebMatrix 1,0, primero debe instalar el [Instalador de plataforma web de Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Después de instalar el instalador de plataforma web, puede usarlo para instalar WebMatrix.
> 
> Si tiene problemas durante la instalación, consulte la [solución de problemas con instalador de plataforma web de Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="InstructionsForPublishingApplications"></a>
## <a name="how-to-publish-applications"></a>Publicación de aplicaciones

> Consulte [las instrucciones paso a paso para publicar aplicaciones](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="ChangesAndIssues"></a>

## <a name="changes-and-issues"></a>Cambios y problemas

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-10-installation-issues"></a>Problemas de instalación de WebMatrix 1,0

#### <a name="issue-webmatrix-10-is-available-only-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix 1,0 solo está disponible en plataformas compatibles con Microsoft .NET Framework 4

> La versión 4 de .NET Framework es necesaria para WebMatrix. En algunos casos, el instalador de WebMatrix 1,0 le permitirá instalar en una plataforma que no forma parte del conjunto de configuración compatible. En concreto, Windows Vista sin la actualización de SP1 le permitirá comenzar la instalación de WebMatrix, pero se producirá un error en el componente .NET Framework 4 y se bloqueará la instalación.
> 
> **Solución alternativa**  
> Instale en una plataforma compatible, que incluye:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 o posterior
> - Windows XP SP3
> - Windows Server 2003 SP2

#### <a name="issue-cannot-install-webmatrix-10-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: no se puede instalar WebMatrix 1,0 si Microsoft Visual Studio 2008 está instalado sin Microsoft Visual Studio 2008 SP1

> **Solución alternativa**  
> Instale [Microsoft Visual Studio SP1 de 2008](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) desde el centro de descarga de Microsoft.

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problema: algunos ensamblados para SQL Server Compact 4,0 no están instalados en la GAC

> Los ensamblados administrados para SQL Server Compact 4,0 no se colocan en la caché de ensamblados global (GAC) cuando se instala SQL Server Compact 4,0 en un equipo de 64 bits y el equipo solo tiene instalado el perfil de cliente de .NET Framework 3,5 SP1. Los ensamblados administrados que no están instalados en la GAC son:
> 
> - *System. Data. SqlServerCe. dll* (proveedor ADO.net)
> - *System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)
> 
> **Solución alternativa**  
> Desinstale SQL Server Compact 4,0. Descargue e instale la versión completa de .NET Framework 3,5 SP1 desde la siguiente ubicación:  
>   
> [Microsoft .NET Framework 3,5 Service Pack 1 (paquete completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> A continuación, reinstale SQL Server Compact 4,0.

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problema: no se puede desinstalar SQL Server Compact mediante la línea de comandos

> La desinstalación de SQL Server Compact con las opciones de línea de comandos no funciona en esta versión.
> 
> **Solución alternativa**  
> Use *programas y características* en el panel de control de Windows para desinstalar Microsoft SQL Server Compact 4,0.

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>ASP.NET Web Pages

En esta sección del documento se describen las nuevas características, los cambios y los problemas conocidos de la versión 1,0 de ASP.NET Web Pages con sintaxis Razor.

- [Características nuevas](#NewFeatures)
- [Cambios](#Changes)
- [Issues](#Issues)

#### <a id="NewFeatures"></a>Nuevas características

#### <a name="new-configuration-setting-added-to-disable-the-package-manager"></a>Nuevo: se ha agregado una opción de configuración para deshabilitar el administrador de paquetes

> Hay una nueva clave de `asp:AdminManagerEnabled` disponible para el elemento `<appSettings>` en el archivo *Web. config* , que le permite deshabilitar por completo el administrador de paquetes. El valor predeterminado de este elemento es true, lo que significa que si no se incluye en el archivo *Web. config* , el administrador de paquetes está habilitado. Para deshabilitar el administrador de paquetes, agregue el siguiente elemento al archivo *Web. config* en la raíz del sitio web:
> 
> [!code-xml[Main](overview/samples/sample1.xml)]

#### <a id="Changes"></a>Realizadas

#### <a name="change-webpagesadminfoldervirtualpath-key-renamed-to-aspadminfoldervirtualpath"></a>Cambio: la clave "Webpages: AdminFolderVirtualPath" cambió de nombre a "ASP: AdminFolderVirtualPath"

> Se ha cambiado el nombre de la clave `webPages:AdminFolderVirtualPath` que se puede Agregar al archivo *Web. config* para especificar la ubicación del administrador de paquetes para usar el espacio de nombres `asp:` en lugar del espacio de nombres `webPages`. Si ha usado este elemento, debe cambiarlo en el archivo de configuración.

#### <a id="Issues"></a>  Problemas conocidos

#### <a name="issue-passwords-for-membership-users-no-longer-recognized"></a>Problema: las contraseñas de los usuarios de pertenencia ya no se reconocen

> El algoritmo para crear y almacenar contraseñas de pertenencia (inicio de sesión) se ha cambiado para que sea más seguro. Como resultado, no se reconocerán las contraseñas almacenadas para los miembros (usuarios) creados en las versiones beta de Razor ASP.NET. 
> 
> **Solución alternativa** Si el sitio todavía no se ha puesto en producción, quite los registros de usuario de la base de datos de pertenencia. Si la base de datos está activa, regenere las contraseñas existentes en la base de datos de pertenencia mediante programación.

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: comportamiento inesperado al usar una tabla de usuario personalizada para la pertenencia

> Para inicializar el proveedor de pertenencia para un sitio web de Razor para ASP.NET, llame al método `WebSecurity.InitializeDatabaseConnection`. (En WebMatrix, la plantilla del sitio de inicio incluye una llamada a este método en el archivo *\_AppStart. cshtml* ). Si el parámetro `autoCreateTables` de este método está establecido en true (de forma predeterminada, se establece en true en la plantilla del sitio de inicio) y si se pasa un nombre de tabla no reconocido al método (el segundo parámetro), el método no produce un error. En su lugar, crea la tabla automáticamente.
> 
> Esto puede ser un problema si tiene previsto usar una tabla de usuario personalizada para la pertenencia pero pasar el nombre de tabla incorrecto al método `WebSecurity.InitializeDatabaseConnection`. Dado que el método no genera de forma predeterminada un error si la tabla que especifique no existe, y porque en su lugar crea una nueva tabla, la aplicación puede parecer que funciona. Sin embargo, el código de aplicación que se basa en la tabla de usuario personalizada (y en los campos que contiene) puede producir errores inesperados.
> 
> **Solución alternativa**  
> Asegúrese de que el nombre pasado en el método `InitializeDatabaseConnection` coincide con la tabla de perfiles de usuario de la base de datos de pertenencia, o asegúrese de que el parámetro `autoCreateTables` está establecido en false.

#### <a name="issue-error-message-the-admin-module-requires-access-to-app_data"></a>Problema: mensaje de error "el módulo de administración requiere acceso a ~/App\_datos"

> En algunas circunstancias, intentar crear usuarios o trabajar de otro modo con el sistema de pertenencia de ASP.NET puede hacer que la página muestre el error. *el módulo de administración requiere acceso a ~/App\_datos*. Esto sucede si la cuenta con la que se ejecuta IIS o IIS Express no tiene permisos para crear y escribir en la carpeta de *datos de\_* de la aplicación en la raíz del sitio Web. 
> 
> **Solución alternativa** Cree manualmente una carpeta de *datos de\_de aplicaciones* para el sitio Web. A continuación, asegúrese de que la cuenta de Windows en la que se ejecuta la aplicación (normalmente servicio de red) tiene permisos de lectura y escritura para las carpetas raíz de la aplicación y para las subcarpetas, como los datos de\_de la aplicación. Puede encontrar información más detallada en el artículo de Knowledge base sobre [problemas con SQL Server Express los proyectos de aplicación Web ASP.net y creación de instancias de usuario](https://support.microsoft.com/kb/2002980).

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: "no se pudo generar una instancia de usuario de SQL Server"

> Si una aplicación Web de WebMatrix usa SQL Server Express y ejecuta IIS 7,5 en Windows 7 o Windows Server 2008 R2, podría ver un error que indica que SQL Server no puede recuperar la ruta de acceso de la aplicación local del usuario en tiempo de ejecución.
> 
> **Solución alternativa** Asegúrese de que la cuenta de Windows en la que se ejecuta la aplicación (normalmente servicio de red) tenga permisos de lectura y escritura para las carpetas raíz de la aplicación y para las subcarpetas, como los *datos de\_* de la aplicación. Puede encontrar información más detallada en el artículo de Knowledge base sobre [problemas con SQL Server Express los proyectos de aplicación Web ASP.net y creación de instancias de usuario](https://support.microsoft.com/kb/2002980).

#### <a name="issue-files-that-contains-package-manager-resources-or-package-manager-passwords-are-servable-under-iis-60-and-earlier"></a>Problema: los archivos que contienen recursos del administrador de paquetes o contraseñas del administrador de paquetes se pueden disponer en IIS 6,0 y versiones anteriores.

> Si implementa una aplicación de ASP.NET Web Pages (Razor) que se compiló con la versión RC2 y, si la aplicación contiene un archivo *password. txt* o *packagesources. txt* en */App\_Data/admin*, IIS 6,0 atenderá el archivo si se solicita, lo que podría exponer las contraseñas de la instancia del administrador de paquetes. 
> 
> **Solución alternativa** Cambie el nombre del archivo *password. txt* o *packagesources. txt* a *password. config* o *packagesources. config*. De forma predeterminada, IIS 6,0 no sirve los archivos que tienen la extensión *. config* . (En IIS 7, no se sirven archivos en la *aplicación\_* carpeta de datos, por lo que no es necesario cambiar el nombre de los archivos).

#### <a name="issue-uninstalling-packages-installed-using-the-beta-3-release-does-not-completely-remove-package-components"></a>Problema: la desinstalación de paquetes instalados con la versión beta 3 no quita por completo los componentes del paquete

> Si instaló un paquete con el administrador de paquetes en la versión beta 3 y, a continuación, intenta desinstalarlo con la versión actual, el paquete no se desinstalará por completo. Al usar el botón **desinstalar** del administrador de paquetes, se quitan algunos componentes, *pero el código* de la biblioteca del paquete no se actualiza.
> 
> **Solución alternativa**   
> Siga estos pasos:  
> 1. Elimine la carpeta Data\packages de la *aplicación\_* . Esto quita todos los paquetes.   
> 2. Elimine el archivo *packages. config* en la raíz del sitio Web.

#### <a name="issue-in-visual-studio-invoking-the-web-based-package-manager-takes-the-application-offline"></a>Problema: en Visual Studio, al invocar el administrador de paquetes basado en Web, la aplicación se desconecta.

> Si está trabajando en Visual Studio (no WebMatrix) y usa la funcionalidad de *Administración de\_* para iniciar el administrador de paquetes, Visual Studio pone la aplicación sin conexión y envía la aplicación *\_offline. htm* a la raíz del sitio web, lo que interrumpe su capacidad de usar el administrador de paquetes.
> 
> [!NOTE]
> Aunque lo más habitual es ver este comportamiento cuando se usa la interfaz del administrador de paquetes basada en Web, se produce el mismo comportamiento si se agregan, quitan o modifican archivos en la carpeta *\_de datos* de la aplicación.
> 
> **Solución alternativa**   
> Para trabajar con paquetes en Visual Studio, use la extensión NuGet en lugar del administrador de paquetes basado en Web. Para obtener información, consulte la [documentación de NuGet](https://docs.microsoft.com/nuget/). Si está trabajando con otros archivos en la carpeta *App\_Data* , considere la posibilidad de mantener los archivos en otro lugar para evitar este problema. Si no es práctico, elimine la *aplicación\_archivo. htm sin conexión* manualmente o espere hasta que el sitio vuelva a estar en línea automáticamente (de forma predeterminada, después de 30 segundos).

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: IntelliSense de Visual Studio y las plantillas de proyecto solo están disponibles en ASP.NET MVC versión 3

> La instalación de ASP.NET Web Pages no instala también herramientas para Visual Studio, como IntelliSense y plantillas de proyecto para las aplicaciones de ASP.NET Web Pages.
> 
> **Solución alternativa** Para usar IntelliSense y plantillas de proyecto para ASP.NET Web Pages aplicaciones en Visual Studio, instale ASP.NET MVC 3 RC mediante el instalador de plataforma web o el [instalador independiente](https://go.microsoft.com/fwlink/?LinkID=191797).

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: leer fuentes u otros datos externos a través de un servidor proxy

> Si el servidor que ejecuta el sitio está detrás de un servidor proxy, es posible que tenga que configurar la información del proxy en el archivo *Web. config* para poder leer información que procede de fuera de su sitio. Por ejemplo, si usa la aplicación auxiliar `ReCaptcha`, el ayudante se comunica con el servicio reCAPTCHA, pero podría estar bloqueado por el servidor proxy. Del mismo modo, las fuentes que se usan en ASP.NET Web Pages, como la fuente utilizada por el administrador de paquetes, pueden requerir la configuración del proxy.
> 
> Si tiene problemas al trabajar con un servicio externo o al trabajar con la fuente del paquete, coloque los elementos siguientes en el archivo *Web. config* raíz de la aplicación:
> 
> [!code-xml[Main](overview/samples/sample2.xml)]
> 
> Para obtener más información acerca de la configuración de un servidor proxy, vea [&lt;elemento&gt; del proxy (configuración de red)](https://msdn.microsoft.com/library/sa91de1e.aspx) en el sitio web de MSDN.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: la desinstalación de la versión 4 de .NET Framework deshabilita ASP.NET Web Pages con sintaxis Razor

> Si desinstala la versión 4 de .NET Framework y, a continuación, vuelve a instalarla, ASP.NET Web Pages con sintaxis Razor está deshabilitada. Las páginas con la extensión *. cshtml* no se ejecutan correctamente. ASP.NET Web Pages registra un ensamblado en el archivo *Web. config* raíz del equipo y quita el .NET Framework quita ese archivo. Al volver a instalar el .NET Framework se instala una nueva versión del archivo de configuración, pero no se agrega la referencia del ensamblado ASP.NET Web Pages.
> 
> **Solución alternativa** Después de volver a instalar el .NET Framework, vuelva a instalar ASP.NET Web Pages con sintaxis Razor. Esto agrega el siguiente elemento al archivo *Web. config* en la raíz de la máquina, que normalmente se encuentra en la siguiente ubicación:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](overview/samples/sample3.xml)]

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problema: las direcciones URL sin extensión no encuentran archivos. cshtml/. vbhtml en IIS 7 o IIS 7,5

> En IIS 7 o IIS 7,5, las solicitudes con una dirección URL como la siguiente no pueden encontrar páginas que tengan la extensión *. cshtml* o *. vbhtml* :  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> El problema surge porque la reescritura de direcciones URL no está habilitada de forma predeterminada para IIS 7 o IIS 7,5. El escenario likeliest es que no ve el problema al realizar pruebas de forma local con IIS Express, pero lo experimenta al implementar el sitio web en un sitio web de hospedaje.
> 
> **Solución alternativa**
> 
> - Si tiene control sobre el equipo servidor, en el equipo servidor Instale la actualización que se describe en [una actualización disponible que permite que determinados controladores de iis 7,0 o iis 7,5 controlen las solicitudes cuyas direcciones URL no finalizan con un punto](https://support.microsoft.com/kb/980368).
> - Si no tiene control sobre el equipo del servidor (por ejemplo, si está implementando en un sitio web de hospedaje), agregue lo siguiente al archivo *Web. config* del sitio web: 
> 
>     [!code-xml[Main](overview/samples/sample4.xml)]

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: implementación de una aplicación en un equipo que no tiene SQL Server Compact instalado

> Las aplicaciones que incluyen SQL Server Compact bases de datos se pueden ejecutar en un equipo en el que no esté instalado SQL Server Compact. Microsoft WebMatrix 1,0 copia automáticamente estos archivos binarios y realiza las transformaciones del archivo *Web. config* adecuado.
> 
> **Solución alternativa** Si necesita copiar estos archivos y hacer que los cambios en el archivo *Web. config* se realicen manualmente, haga lo siguiente:
> 
> 1. Copie los ensamblados del motor de base de datos en la carpeta *bin* (y en las subcarpetas) de la aplicación en el equipo de destino:  
> 
>    - Copy *c:\Archivos de programa\microsoft SQL Server Edition\v4.0\Desktop\System.Data.SqlServerCe.dll*   
>      **a** *\Bin*
>    - Copie *c:\Archivos de programa\microsoft SQL Server Compact Edition\v4.0\Private\x86\\* **en** *\Bin\x86*
>    - Copie *c:\Archivos de programa\microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **en** *\Bin\amd64*
> 
> 2. En la carpeta raíz del sitio web, cree o abra un archivo *Web. config* . (En WebMatrix 1,0, este tipo de archivo está disponible si hace clic en **todo** en el cuadro de diálogo **elegir un tipo de archivo** ).
> 3. Agregue el siguiente elemento como elemento secundario del elemento `<configuration>` (no dentro del elemento `<system.web>`):
> 
>     [!code-xml[Main](overview/samples/sample5.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: las aplicaciones auxiliares de "base de datos" y "WebGrid" no funcionan en un nivel de confianza medio en Visual Basic

> Si usa Visual Basic (crear archivos *. vbhtml* ), las aplicaciones auxiliares de `Database` y `WebGrid` no funcionarán si la aplicación está configurada para usar el nivel de confianza medio.
> 
> **Solución alternativa**  
> Si usa Visual Studio 2010, puede resolver este problema instalando la versión de Service Pack 1. Hasta que la versión final de la versión SP1 esté disponible, puede descargar la versión beta de SP1 desde la página [Microsoft Visual Studio 2010 Service Pack 1 beta](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=11ea69cb-cf12-4842-a3d7-b32a1e5642e2&amp;displaylang=en) del centro de descarga de Microsoft.   
>   
> Si esto no es práctico, o si no usa Visual Studio 2010, puede establecer temporalmente la aplicación para que utilice plena confianza.

#### <a name="issue-applicationpart-resources-are-externally-accessible"></a>Problema: los recursos "ApplicationPart" son accesibles externamente

> Si un ensamblado contiene objetos que se derivan de la clase `ApplicationPart`, la clase `ResourceRouteHandler` expone los recursos de ese ensamblado. Pongamos como ejemplo esta URL:  
>   
> `~/r.ashx/System.Web.WebPages.Administration/Resources/AdminResources.resources`  
>   
> Esta solicitud descarga todas las cadenas de recursos en el ensamblado *System. Web. Webpages. Administration. dll* . Se descargan todos los recursos incrustados (incluso los que no están diseñados para servir como contenido estático). Si los recursos incrustados contienen información confidencial, esto puede representar un riesgo para la seguridad. 
> 
> **Solución alternativa**   
> Si crea un objeto **ApplicationPart** , asegúrese de que los recursos incrustados asociados al ensamblado del objeto **ApplicationPart** no contengan información confidencial.

<a id="Known_Issues_WebMatrix"></a>

### <a name="webmatrix"></a>WebMatrix

> [!NOTE]
> Para obtener información sobre los problemas de instalación de WebMatrix, consulte los [problemas de instalación de WebMatrix](#Known_Issues_Installation) anteriormente en este documento.

En esta sección del documento se describen los problemas conocidos del entorno de desarrollo de WebMatrix.

#### <a name="issue-changes-in-the-username-or-password-of-a-database-connection-string-in-a-webconfig-file-are-not-reflected-in-the-databases-workspace"></a>Problema: los cambios en el nombre de usuario o la contraseña de una cadena de conexión de base de datos en un archivo Web. config no se reflejan en el área de trabajo bases de datos.

> **Solución alternativa**  
> 
> 1. En el archivo *Web. config* , cambie el nombre de la base de datos en la cadena de conexión (por ejemplo, agregue "1" a ella).
> 2. Guarde el archivo *Web. config* .
> 3. Haga clic en **bases de datos** y en actualizar.
> 4. Vuelva a cambiar el nombre de la base de datos en la cadena de conexión en el archivo *Web. config* al nombre de la base de datos original.
> 5. Guarde el archivo *Web. config* .
> 6. Haga clic en **bases de datos** y en actualizar.

#### <a name="issue-folders-created-by-webmatrix-cannot-be-deleted"></a>Problema: no se pueden eliminar las carpetas creadas por WebMatrix

> Si WebMatrix se ejecuta con permisos elevados (es decir, inició WebMatrix con la opción **Ejecutar como administrador** en Windows), las carpetas que crea WebMatrix no se pueden eliminar con el explorador de Windows.
> 
> **Solución alternativa**  
> Ejecute el explorador de Windows con permisos elevados. Siga estos pasos:  
> 
> 1. En Windows, haga clic en **iniciar**.
> 2. Escriba "explorador de Windows" y haga clic con el botón derecho en la entrada del **Explorador de Windows**.
> 3. Haga clic en **Ejecutar como administrador**. Después, puede eliminar las carpetas.

#### <a name="issue-webmatrix-10-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: WebMatrix 1,0 no puede realizar determinadas tareas que requieren elevación

> WebMatrix 1,0 no puede realizar determinadas tareas que requieren elevación, como la instalación de componentes adicionales en las siguientes situaciones:
> 
> - En Windows Vista o Windows 7, inició sesión con una cuenta que no tiene privilegios administrativos y el control de cuentas de usuario (UAC) está deshabilitado.
> - Utiliza Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Solución alternativa**  
> La mayoría de las tareas de WebMatrix 1,0 no requieren permisos administrativos. Para los que sí lo hacen, puede realizar la operación como administrador o seguir estos pasos:
> 
> - En Windows Vista o Windows 7, habilite UAC.
> - En Windows XP, agregue el usuario al grupo de seguridad administradores.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "sitio de la galería Web" está deshabilitado

> La opción **sitio de la galería web** está deshabilitada si el instalador de plataforma web 3,0 no está instalado.
> 
> **Solución alternativa**  
> Instale el [Instalador de plataforma web de Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problema: Google Chrome no está disponible como opción de ejecución

> Google Chrome no se muestra en la lista de exploradores en **Ejecutar** en la pestaña **Inicio** .
> 
> **Solución alternativa**  
> Algunas versiones de Google Chrome no se registran correctamente con la característica programas predeterminados de Windows. Como solución alternativa, inicie Google Chrome, haga clic en el menú *personalizar y controlar Google Chrome* , haga clic en *Opciones*y, a continuación, haga clic en *crear Google Chrome mi explorador predeterminado*.

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problema: el cuadro de diálogo "clave externa" no permite escribir una clave principal

> El cuadro de diálogo **clave externa** no permite especificar el nombre de la clave principal de la tabla de clave principal.
> 
> **Solución alternativa**  
> Esto tiene su porqué. No es necesario que escriba el nombre de la clave principal de la tabla de clave principal.

#### <a name="issue-intellisense-is-not-available-in-webmatrix-for-razor-syntax-c-or-visual-basic"></a>Problema: IntelliSense no está disponible en WebMatrix para sintaxis Razor, C#o Visual Basic

> IntelliSense es compatible con WebMatrix para HTML y CSS. Sin embargo, no está disponible para otros idiomas. 
> 
> **Solución alternativa**   
> Ninguno.

#### <a name="issue-intellisense-for-html-and-css-suggests-elements-that-are-not-contextually-appropriate"></a>Problema: IntelliSense para HTML y CSS sugiere elementos que no son apropiados contextualmente.

> IntelliSense para el marcado en WebMatrix admite HTML mediante el [esquema de transición XHTML 1,0](http://www.w3.org/TR/2002/NOTE-xhtml1-schema-20020902/#xhtml1-transitional) y CSS con el [esquema CSS 2,1](http://www.w3.org/TR/CSS2/). Dado que IntelliSense se basa en estos esquemas específicos, se pueden sugerir ciertas etiquetas, atributos o propiedades que no son adecuadas para la página o definición de estilo actual. En el caso de HTML, también puede provocar sugerencias inesperadas en el contenido que podrían interpretarse como XHTML con formato incorrecto (por ejemplo, cuando las etiquetas no están cerradas). Este problema puede ser más evidente si el punto de inserción está dentro de una etiqueta incompleta; en ese caso, IntelliSense podría sugerir nuevas etiquetas de apertura u ofrecer otras sugerencias incorrectas. 
> 
> **Solución alternativa**   
> Para HTML, asegúrese de que está trabajando en una página XHTML completa y bien formada. En el caso de CSS, no hay ninguna solución alternativa.

#### <a name="issue-intellisense-is-not-invoked-while-you-type"></a>Problema: IntelliSense no se invoca mientras escribe

> En ocasiones, es posible que IntelliSense no se invoque como HTML o CSS se esté escribiendo en el editor. En concreto, esto puede ocurrir cuando el punto de inserción se encuentra directamente junto a otro elemento o al final de un archivo. 
> 
> **Solución alternativa**   
> Asegúrese de que hay espacio en blanco alrededor del punto de inserción y que el punto de inserción no está al final de un archivo. También puede invocar IntelliSense manualmente presionando CTRL + barra espaciadora.

#### <a name="issue-no-ui-is-available-for-disabling-intellisense"></a>Problema: no hay ninguna interfaz de usuario disponible para deshabilitar IntelliSense

> WebMatrix 1,0 no proporciona ninguna interfaz de usuario ni gesto para deshabilitar IntelliSense. 
> 
> **Solución alternativa**   
> Inicie WebMatrix con el siguiente comando, que incluye un modificador que deshabilita IntelliSense:  
>   
> `WebMatrix.exe #ExecuteCommand# EditorIntelliSense off`

<a id="Known_Issues_IISExpress"></a>
### <a name="iis-express"></a>IIS Express

IIS Express tiene su propio archivo Léame, que está disponible en la siguiente dirección URL:

[https://go.microsoft.com/fwlink/?LinkID=207675&amp; clcid = 0xA](https://go.microsoft.com/fwlink/?LinkID=207675&amp;clcid=0x409)

<a id="Known_Issues_SQLServerCompact"></a>

### <a name="sql-server-compact"></a>SQL Server Compact

SQL Server Compact tiene su propio archivo Léame, que está disponible en la siguiente dirección URL:

[https://go.microsoft.com/fwlink/?LinkID=208545](https://go.microsoft.com/fwlink/?LinkID=208545&amp;clcid=0x409)

Para obtener información sobre los problemas que implican la instalación de SQL Server Compact como parte de WebMatrix, consulte los [problemas de instalación de WebMatrix](#Known_Issues_Installation) anteriormente en este documento.

### <a id="Known_Issues_Installing_Applications"></a>Instalación de aplicaciones

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: la instalación de una aplicación puede tardar mucho tiempo si la carpeta Mis documentos del usuario se redirige a un recurso compartido de red.

> **Solución alternativa**  
> Ninguno. La aplicación puede tardar un tiempo en instalarse, pero se instalará correctamente.

### <a id="Known_Issues_Publishing_Applications"></a>Publicar aplicaciones

#### <a name="issue-required-permissions-cannot-be-acquired-error-when-publishing-a-sql-compact-database"></a>Problema: "no se pueden adquirir los permisos necesarios" al publicar una base de datos de SQL Compact

> WebMatrix no es totalmente compatible con la implementación de archivos binarios auxiliares para SQL Server Compact en un servidor que ejecute .NET Framework versión 3,5 con una configuración de confianza media.
> 
> **Solución alternativa**  
> La solución preferida es instalar el .NET Framework 4 en el servidor. Como alternativa, haga lo siguiente:
> 
> 1. Agregue los siguientes elementos a la sección `SecurityClasses` en el archivo *Web\_MediumTrust. config* :
> 
>     [!code-html[Main](overview/samples/sample6.html)]
> 2. Cree un nuevo conjunto de permisos en el archivo *Web\_MediumTrust. config* con los siguientes permisos necesarios:
> 
>     [!code-html[Main](overview/samples/sample7.html)]
> 3. Aplique el conjunto de permisos a SQL Server Compact colocando los siguientes elementos en el archivo *Web\_MediumTrust. config* :
> 
>     [!code-html[Main](overview/samples/sample8.html)]

#### <a name="issue-gallery-and-phpbb-web-applications-display-a-service-is-unavailable-error-after-publishing"></a>Problema: las aplicaciones Web de galería y PhpBB muestran un error "el servicio no está disponible" después de la publicación

> En algunas circunstancias, la publicación de una aplicación provoca el error "el servicio no está disponible".
> 
> **Solución alternativa**  
> En WebMatrix, agregue una barra diagonal inversa (\) al final del nombre del servidor en la ventana **configuración de publicación** y, a continuación, vuelva a publicar la aplicación.

#### <a name="issue-moodle-website-layout-and-links-are-broken-after-publishing"></a>Problema: los vínculos y el diseño del sitio web de Moodle se rompen después de la publicación

> Después de publicar una aplicación de Moodle, la aplicación no funciona correctamente.
> 
> **Solución alternativa**  
> En WebMatrix, agregue una barra diagonal (/) al final del campo **nombre del sitio** en la ventana **configuración de publicación** y, a continuación, vuelva a publicar la aplicación.

#### <a name="issue-publishing-nopcommerce-fails-with-a-database-error"></a>Problema: la publicación de nopCommerce produce un error de base de datos

> No se puede publicar nopCommerce y se notifica un error de base de datos como "error al insertar en la tabla de registro NOP\_."
> 
> **Solución alternativa**  
> 
> 1. En WebMatrix, haga clic en **Ejecutar** para iniciar nopCommerce localmente.
> 2. Inicie sesión en la página de administración de.
> 3. Haga clic en el menú **sistema** .
> 4. Haga clic en la opción **registrar** .
> 5. Haga clic en el botón **Vaciar registro**.
> 6. Vuelva a publicar nopCommerce.

#### <a name="issue-silverstripe-cms-displays-a-http-500-php-fcgi-error-when-you-download-a-published-site"></a>Problema: SilverStripe CMS muestra un error de FCGI de PHP HTTP 500 al descargar un sitio publicado

> **Solución alternativa**  
> Después de hacer clic en **Descargar sitio publicado**, omita `silverstripe-cache/manifest_main` en la **vista previa de publicación**. Este archivo se usa para el almacenamiento en caché y es específico para cada equipo.

#### <a name="issue-subtext-displays-server-error-in--application-when-you-download-a-published-site"></a>Problema: el subtexto muestra el mensaje "error de servidor en '/' aplicación" al descargar un sitio publicado

> **Solución alternativa**  
> Abra el archivo *Web. config* del sitio y reemplace el ID. de usuario y la contraseña en la cadena de conexión de base de datos por el SQL Server credenciales de administrador (las credenciales "SA").
> 
> También puede seguir estos pasos para dar a la cuenta de usuario con la que ha iniciado sesión `db_owner` permisos:
> 
> 1. Instale SQL Server Management Studio mediante el instalador de plataforma Web.
> 2. Conéctese a la instancia local de SQL Server Express (de forma predeterminada, `.\SQLEXPRESS`).
> 3. Haga clic en **bases de datos** &gt; *[LocalSubtextDatabase]* &gt; **Security** &gt; **users** &gt; *[localSubtextUser*] (el valor predeterminado es `subtextuser`], haga clic con el botón secundario y haga clic en **propiedades**.
> 4. Seleccione **db\_Owner** en la sección pertenencia a roles.

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problema: es posible que el sitio no funcione después de la publicación si el campo "dirección URL de destino" no tiene el prefijo http://o https://

> En el cuadro de diálogo **configuración de publicación** , si la dirección URL de destino no comienza con `http://` o `https://`, es posible que el sitio no funcione después de la implementación.
> 
> **Solución alternativa**  
> Asegúrese de que antes de publicar un sitio, la dirección URL de destino en el cuadro de diálogo **configuración de publicación** comienza con `http://` o `https://`.

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problema: al publicar una base de datos MySQL se produce el error "no se pudo publicar la base de datos. Esto puede ocurrir si la base de datos remota no puede ejecutar el script ".

> El error puede producirse por una serie de motivos. Uno de los motivos por los que puede ver este error es si el script de la base de datos contiene un carácter de comilla simple (') y el juego de caracteres predeterminado de la base de datos MySQL de destino no es UTF-8.
> 
> **Solución alternativa**  
> Establezca el juego de caracteres predeterminado para la base de datos MySQL remota en UTF-8.

#### <a name="issue-some-links-are-not-visible-in-dotnetnuke-after-publishing-or-downloading-the-site"></a>Problema: algunos vínculos no están visibles en DotNetNuke después de publicar o descargar el sitio

> Si publica o descarga un sitio de DotNetNuke, es posible que tenga que borrar la memoria caché para que los nuevos vínculos aparezcan en el sitio.
> 
> **Solución alternativa**
> 
> 1. Inicie sesión como "host".
> 2. Vaya al menú host y seleccione **configuración de host**.
> 3. Desplácese hacia abajo y, en **Configuración avanzada**, expanda **configuración de rendimiento**.
> 4. Haga clic en el vínculo **Borrar caché** para las páginas.
> 5. Vaya a la parte inferior de la página y reinicie la aplicación.

#### <a name="issue-some-links-in-atomsite-are-broken-after-you-download-a-published-site"></a>Problema: algunos vínculos de AtomSite están rotos después de descargar un sitio publicado

> **Solución alternativa**  
> En el archivo *Service. config* , el archivo *users. config* y todos los archivos *. XML* , reemplace la cadena de dirección URL (por ejemplo, `http://myhost.com/atomsite`) por la local (por ejemplo, `http://localhost:1239`).

#### <a name="issue-mysql-based-applications-like-wordpress-fail-to-publish-and-report-a-database-error"></a>Problema: las aplicaciones basadas en MySQL como WordPress no pueden publicar y notificar un error de base de datos

> De forma predeterminada, WebMatrix instala MySQL con el juego de caracteres UTF-8. Si instala MySQL por su cuenta y el juego de caracteres no es UTF-8 (por ejemplo, es latin1), puede producirse un error en el proceso de publicación de las bases de datos.
> 
> **Solución alternativa**
> 
> 1. Cambie el juego de caracteres de MySQL a UTF-8. (Para obtener más información, consulte el [juego de caracteres del servidor y la intercalación](http://dev.mysql.com/doc/refman/5.0/en/charset-server.html) en el sitio web de MySQL).
> 2. Vuelva a instalar la aplicación.
> 3. Vuelva a publicar la aplicación.

#### <a name="issue-download-published-site-fails-for-applications-that-have-browser-based-setup"></a>Problema: "descarga del sitio publicado" produce un error en las aplicaciones que tienen una instalación basada en explorador

> Algunas aplicaciones (por ejemplo, Kentico CMS) requieren que se inicien en el explorador para realizar la configuración posterior a la instalación, como crear una base de datos. Si publica una aplicación como esta sin completar la instalación basada en explorador, se producirá un error al intentar descargar el mismo sitio desde un servidor remoto.
> 
> **Solución alternativa**  
> Finalice la instalación basada en explorador antes de publicar el sitio.

#### <a name="issue-download-published-site-fails-with-a-database-error-for-dotnetnuke-and-kooboo-cms"></a>Problema: "download Published site" produce un error de base de datos para DotNetNuke y Kooboo CMS

> Si intenta descargar una aplicación desde un servidor y tiene credenciales de administrador en la cadena de conexión de base de datos en el cuadro de diálogo **configuración de publicación** , es posible que vea el siguiente error en el registro de publicación:
> 
> [!code-console[Main](overview/samples/sample9.cmd)]
> 
> **Solución alternativa**  
> Si es práctico, vuelva a publicar el sitio (o haga que se publique) con credenciales que no sean de administrador para la base de datos.

<a id="More_Info"></a>

## <a name="for-more-information"></a>Para obtener más información

Para obtener más información acerca de WebMatrix 1,0, consulte los siguientes sitios web:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

© 2011 Microsoft Corporation. Reservados todos los derechos. [Términos de uso](https://msdn.microsoft.cos/cc300389.aspx).
