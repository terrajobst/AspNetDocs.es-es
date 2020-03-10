---
uid: web-pages/readme/beta3
title: Archivo Léame de la versión beta 3 de web Matrix y ASP.NET Web Pages (Razor) | Microsoft Docs
author: rick-anderson
description: Archivo Léame de la versión Beta 3 de WebMatrix y ASP.NET Web Pages (Razor)
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510343"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>Archivo Léame de la versión Beta 3 de WebMatrix y ASP.NET Web Pages (Razor)

> Archivo Léame de la versión Beta 3 de WebMatrix y ASP.NET Web Pages (Razor)

9 de noviembre de 2010

## <a name="contents"></a>Contenido

- [Información general](#Overview)
- [Instalación](#Installation_Notes)
- [Nuevas características, cambios y problemas conocidos de la versión beta 3](#Known_Issues)

    - [Problemas de instalación de WebMatrix](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET) (Más información sobre páginas web de ASP.NET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Instalación de aplicaciones](#Known_Issues_Installing_Applications)
    - [Publicar aplicaciones](#Known_Issues_Publishing_Applications)
    - [Otros problemas](#Known_Issues_Other_Issues)
- [Para obtener más información](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Información general

> Microsoft WebMatrix beta es una pila de desarrollo web gratuita que se instala en minutos. Integra un servidor Web con marcos de trabajo de programación y base de datos para crear una experiencia única e integrada. Puede usar WebMatrix beta para simplificar la forma de codificar, probar y publicar su propio sitio Web ASP.NET o PHP, o bien puede usar WebMatrix beta para iniciar un nuevo sitio web con aplicaciones populares de código abierto como DotNetNuke, Umbraco, WordPress o Joomla. WebMatrix beta usa el mismo servidor Web, motor de base de datos y entorno de marcos que ejecutará el sitio web en Internet, lo que hace que la transición de desarrollo a producción sea fluida y fluida.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Instalación

> Para instalar WebMatrix beta 3, use [Instalador de plataforma web de Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Después de instalar el instalador de plataforma web, puede usarlo para instalar WebMatrix beta 3.
> 
> Si tiene problemas durante la instalación, consulte la [solución de problemas con instalador de plataforma web de Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Instrucciones para publicar aplicaciones

> Consulte [las instrucciones paso a paso para publicar aplicaciones](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nuevas características, cambios, problemas de andKnown

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Instalación de WebMatrix beta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix beta 3 solo está disponible en plataformas que admiten Microsoft .NET Framework 4

> La versión 4 de .NET Framework es necesaria para WebMatrix beta. En algunos casos, el instalador de WebMatrix beta le permitirá instalar en una plataforma que no forma parte del conjunto de configuración compatible. En concreto, Windows Vista sin la actualización de SP1 le permitirá comenzar la instalación de WebMatrix beta, pero se producirá un error en el componente .NET Framework 4 y se bloqueará la instalación.
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

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: no se puede instalar WebMatrix beta 3 si Microsoft Visual Studio 2008 está instalado sin Microsoft Visual Studio 2008 SP1

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

En esta sección del documento se describen las nuevas características, los cambios y los problemas conocidos de la versión beta 3 de ASP.NET Web Pages con sintaxis Razor.

- [Características nuevas](#NewFeatures)
- [Cambios](#Changes)
- [Issues](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nuevas características de la versión beta 3 para ASP.NET Web Pages con sintaxis Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Nuevo: el método "HTML. Raw" representa el marcado sin codificar

> El nuevo método de `Html.Raw` permite representar el marcado HTML como marcado en lugar de representar la salida codificada. (De forma predeterminada, ASP.NET Razor codifica las cadenas antes de procesarlas). La sintaxis es:
> 
> `Html.Raw(value)`
> 
> En el siguiente ejemplo se muestra cómo usar `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Cambios en beta 3 para ASP.NET Web Pages con sintaxis Razor

#### <a name="change-hrefattribute-method-removed"></a>Cambio: método "HrefAttribute" quitado

> Se ha quitado el método `HrefAttribute` de la clase `WebPage`. Esta aplicación auxiliar se usó para codificar caracteres no seguros en direcciones URL. Ya no es necesario porque ASP.NET Razor codifica automáticamente las cadenas. (Utilice el método New `Html.Raw` para representar cadenas sin codificar).

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Cambiar: la sintaxis de las aplicaciones auxiliares declarativas "@helper" ha cambiado

> En la versión beta 3, ASP.NET cambia el modo en que analiza las aplicaciones auxiliares que se crean con la sintaxis de `@helper`. En esencia, la sintaxis de `@helper` se analiza ahora como un bloque de código en lugar de como un bloque de marcado que puede incluir código. Por lo tanto, no es necesario que el código dentro del ayudante esté incluido en `@{ }` bloques. Por el contrario, el marcado dentro del ayudante debe incluirse explícitamente en los elementos HTML o en ASP.NET Razor `<text></text>` etiquetas.
> 
> Por ejemplo, la siguiente sintaxis de `@helper` funciona en la versión beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> En la versión beta 3, este ayudante debe cambiarse para que tenga un aspecto similar al ejemplo siguiente:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Observe que ya no se usan los caracteres `@{ }` alrededor del Código inicial del ayudante. Esto se debe a que el contenido de las aplicaciones auxiliares se trata como un bloque de código de forma predeterminada. El ayudante representa el marcado, que comienza con la etiqueta de apertura `<a>`. Si el ayudante debe representar texto sin formato o etiquetas que no incluyen una etiqueta de cierre (por ejemplo, etiquetas de `<meta>`), el contenido que se va a representar debe estar en `<text></text>` etiquetas.

#### <a name="change-webpagecontexthttpcontext-removed"></a>Cambio: se quitó "WebPageContext. HttpContext"

> Se ha quitado la propiedad `WebPageContext.HttpContext`. Utilice `HttpContext.Current` en su lugar. (La propiedad `WebPageContext.HttpContext` simplemente ajustada).

#### <a name="change-facebook-helper-moved-to-new-package"></a>Cambiar: la aplicación auxiliar "Facebook" se ha migrado al nuevo paquete

> La aplicación auxiliar de `Facebook` se ha migrado a la biblioteca de *Facebook. auxiliar* , que incluye la aplicación auxiliar de `Facebook` y la funcionalidad adicional. Debe instalar esta biblioteca como un paquete independiente, tal como se describe en "instalación de aplicaciones auxiliares con el administrador de paquetes" en el tutorial [Introducción con páginas de ASP.net](https://go.microsoft.com/fwlink/?LinkId=202889).

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Cambio: la pertenencia, el rol y los tipos de seguridad se mueven al nuevo ensamblado

> Los siguientes tipos se movieron al ensamblado `WebMatrix.WebData`:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Cambiar: la clase "TagBuilder" se ha migrado al ensamblado System. Web. Webpages. dll

> La clase de `TagBuilde` r se ha pasado al ensamblado System. Web. Webpages. dll. Anteriormente, se encontraba en un ensamblado que formaba parte de ASP.NET MVC. Este cambio significa que no tiene que instalar ASP.NET MVC para poder usar la clase `TagBuilder`.
> 
> Sin embargo, la clase todavía está en el espacio de nombres `System.Web.Mvc`. Para usar la clase `TagBuilder` (por ejemplo, en una aplicación auxiliar de ASP.NET Razor personalizada), debe hacer referencia al espacio de nombres (por ejemplo, agregando `@using System.Web.Mvc` al código).

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Cambiar: la sintaxis de la validación de la solicitud ha cambiado; Clase "Validation" quitada

> En la versión beta 3, para deshabilitar la validación de un campo o conjunto de campos individuales, puede llamar al método `Validation.Exclude`, pasando el nombre o los nombres de los campos que se van a excluir de la validación. Hay una nueva sintaxis disponible en la versión beta 3 para omitir la validación. Se ha quitado el método de `Validation` usado en beta 3.
> 
> > [!NOTE]
> > Si no deshabilita la validación de solicitudes, si los usuarios intentan cargar el marcado HTML (por ejemplo, mediante un editor de texto enriquecido en una página), el sitio web informará de un error como *una solicitud potencialmente peligrosa. el valor de formulario se detectó desde el cliente* y no se aceptó la entrada del usuario. Si deshabilita la validación de solicitudes, debe comprobar manualmente los datos proporcionados por el usuario para asegurarse de que no contenga marcado o script potencialmente peligroso con algo parecido a la [biblioteca de scripts de sitios contra la versión v 4.0 de Microsoft](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Para deshabilitar la validación automática de solicitudes, llame al método `Request.Unvalidated`, pasándole el nombre del campo u otro objeto post para el que desea omitir la validación de solicitudes. Puede usar este método para omitir la validación de los elementos de las colecciones `Form`, `QueryString`, `Cookies`y `ServerVariables`. En los siguientes ejemplos se muestra cómo usar el método `Unvalidated`:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Problemas conocidos de ASP.NET Web Pages con sintaxis Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: comportamiento inesperado al usar una tabla de usuario personalizada para la pertenencia

> Para inicializar el proveedor de pertenencia para un sitio web de Razor para ASP.NET, llame al método `WebSecurity.InitializeDatabaseConnection`. (En WebMatrix, la plantilla del sitio de inicio incluye una llamada a este método en el archivo *\_AppStart. cshtml* ). Si el parámetro `autoCreateTables` de este método está establecido en true (de forma predeterminada, se establece en true en la plantilla del sitio de inicio) y si se pasa un nombre de tabla no reconocido al método (el segundo parámetro), el método no produce un error. En su lugar, crea la tabla automáticamente.
> 
> Esto puede ser un problema si tiene previsto usar una tabla de usuario personalizada para la pertenencia pero pasar el nombre de tabla incorrecto al método `WebSecurity.InitializeDatabaseConnection`. Dado que el método no genera de forma predeterminada un error si la tabla que especifique no existe, y porque en su lugar crea una nueva tabla, la aplicación puede parecer que funciona. Sin embargo, el código de aplicación que se basa en la tabla de usuario personalizada (y en los campos que contiene) puede producir errores inesperados.
> 
> **Solución alternativa**  
> Asegúrese de que el nombre pasado en el método `InitializeDatabaseConnection` coincide con la tabla de perfiles de usuario de la base de datos de pertenencia, o asegúrese de que el parámetro `autoCreateTables` está establecido en false.

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: "no se pudo generar una instancia de usuario de SQL Server"

> Si una aplicación Web de WebMatrix usa SQL Server Express y ejecuta IIS 7,5 en Windows 7 o Windows Server 2008 R2, podría ver un error que indica que SQL Server no puede recuperar la ruta de acceso de la aplicación local del usuario en tiempo de ejecución.
> 
> **Solución alternativa** Asegúrese de que la cuenta de Windows en la que se ejecuta la aplicación (normalmente servicio de red) tenga permisos de lectura y escritura para las carpetas raíz de la aplicación y para las subcarpetas, como los *datos de\_* de la aplicación. Puede encontrar información más detallada en el artículo de Knowledge base sobre [problemas con SQL Server Express los proyectos de aplicación Web ASP.net y creación de instancias de usuario](https://support.microsoft.com/kb/2002980).

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problema: en Visual Studio, los espacios de nombres de los ensamblados personalizados (dll) no se importan automáticamente.

> Si usa ensamblados personalizados en un proyecto de Visual Studio, los espacios de nombres declarados en esos ensamblados no se importan automáticamente en tiempo de diseño. Como resultado, es posible que las referencias a tipos personalizados no se reconozcan en tiempo de diseño y estén marcadas como no reconocidas en Visual Studio (con "zigzag"). Este problema solo se produce en tiempo de diseño en Visual Studio; la aplicación se ejecuta correctamente.
> 
> **Solución alternativa**  
> Incluya una instrucción `using` (`imports` en Visual Basic) que haga referencia a las entidades que no se reconocen en tiempo de diseño.

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: IntelliSense de Visual Studio y las plantillas de proyecto solo están disponibles en ASP.NET MVC versión 3

> La instalación de ASP.NET Web Pages no instala también herramientas para Visual Studio, como IntelliSense y plantillas de proyecto para las aplicaciones de ASP.NET Web Pages.
> 
> **Solución alternativa** Para usar IntelliSense y plantillas de proyecto para ASP.NET Web Pages aplicaciones en Visual Studio, instale ASP.NET MVC 3 RC mediante el instalador de plataforma web o el [instalador independiente](https://go.microsoft.com/fwlink/?LinkID=191797).

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problema: "no se encuentra la&lt;auxiliar&gt; clase"

> Después de actualizar a la versión beta 3, es posible que vea un error que no se puede encontrar en una clase auxiliar (por ejemplo, la clase `Facebook`). A partir de la versión beta 2 y continuar en la versión beta 3, las aplicaciones auxiliares se han migrado a paquetes que debe instalar explícitamente. Los sitios existentes no se actualizan para incluir estos paquetes. Esto incluye los sitios de las carpetas *\Mis Documents\IISExpress* o *\Mis documentos\Mis sitios web* . En concreto, verá este error si usa el sitio predeterminado en *mis sitios* (WebSite1), que incluye una referencia a la aplicación auxiliar de `Twitter`.
> 
> **Solución alternativa**  
> Comente las llamadas a cualquier aplicación auxiliar en el sitio, ejecute la página de *Administración de\_* e instale el paquete o paquetes que incluyan las aplicaciones auxiliares que desee usar. Después de instalar el paquete, puede quitar la marca de comentario de las líneas que hacen referencia a las aplicaciones auxiliares.

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problema: la implementación de ensamblados de Razor beta 3 ASP.NET en la carpeta bin podría no funcionar en sitios de hospedaje

> Si implementa un sitio web de ASP.NET Web Pages en un sitio de hospedaje, y si implementa los ensamblados de ASP.NET Razor beta 3 en la carpeta *bin* del sitio, es posible que experimente errores, como los siguientes:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Esto puede ocurrir si el proveedor de hospedaje ha instalado el ASP.NET Web Pages ensamblados de la versión beta 1 en la caché de aplicación global (GAC) del servidor. Los ensamblados de la GAC obtienen prioridad sobre los ensamblados instalados localmente en la carpeta *bin* .
> 
> **Solución alternativa** Póngase en contacto con su proveedor de hospedaje para confirmar que los errores que está viendo se deben a un conflicto entre las versiones del proveedor de los ensamblados y el suyo. Si es así, solicite al proveedor de hospedaje que actualice los ensamblados en la GAC del servidor.

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: leer fuentes u otros datos externos a través de un servidor proxy

> Si el servidor que ejecuta el sitio está detrás de un servidor proxy, es posible que tenga que configurar la información del proxy en el archivo *Web. config* para poder leer información que procede de fuera de su sitio. Por ejemplo, si usa la aplicación auxiliar `ReCaptcha`, el ayudante se comunica con el servicio reCAPTCHA, pero podría estar bloqueado por el servidor proxy. Del mismo modo, las fuentes que se usan en ASP.NET Web Pages, como la fuente utilizada por el administrador de paquetes, pueden requerir la configuración del proxy.
> 
> Si tiene problemas al trabajar con un servicio externo o al trabajar con la fuente del paquete, coloque los elementos siguientes en el archivo *Web. config* raíz de la aplicación:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Para obtener más información acerca de la configuración de un servidor proxy, vea [&lt;elemento&gt; del proxy (configuración de red)](https://msdn.microsoft.com/library/sa91de1e.aspx) en el sitio web de MSDN.

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problema: error "Microsoft. Web. Infrastructure. dll no se puede cargar"

> Si anteriormente instaló la versión beta 1 de ASP.NET Web Pages con sintaxis Razor y, a continuación, instala la versión beta 3, se instalan todos los ensamblados adecuados en la GAC, excepto *Microsoft. Web. Infrastructure. dll*. Como consecuencia, al ejecutar las páginas de Razor de ASP.NET, verá un error que indica que no se pudo cargar *Microsoft. Web. Infrastructure. dll* .
> 
> Este problema no se produce si cargó la versión beta 3 en un equipo limpio.
> 
> **Solución alternativa**  
> En el panel de control, desinstale ASP.NET Web Pages. A continuación, vuelva a instalar la versión beta 3.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: la desinstalación de la versión 4 de .NET Framework deshabilita ASP.NET Web Pages con sintaxis Razor

> Si desinstala la versión 4 de .NET Framework y, a continuación, vuelve a instalarla, ASP.NET Web Pages con sintaxis Razor está deshabilitada. Las páginas con la extensión *. cshtml* no se ejecutan correctamente. ASP.NET Web Pages registra un ensamblado en el archivo *Web. config* raíz del equipo y quita el .NET Framework quita ese archivo. Al volver a instalar el .NET Framework se instala una nueva versión del archivo de configuración, pero no se agrega la referencia del ensamblado ASP.NET Web Pages.
> 
> **Solución alternativa** Después de volver a instalar el .NET Framework, vuelva a instalar ASP.NET Web Pages con sintaxis Razor. Esto agrega el siguiente elemento al archivo *Web. config* en la raíz de la máquina, que normalmente se encuentra en la siguiente ubicación:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problema: las aplicaciones implementadas previamente con ensamblados de ASP.NET en la carpeta bin experimentan errores

> Durante la implementación, copia de los ensamblados de ASP.NET Web Pages (por ejemplo, *Microsoft. Webpages. dll*) en la carpeta *bin* del sitio web en el servidor. (Esto puede haber ocurrido automáticamente durante la implementación o porque el desarrollador ha copiado explícitamente los ensamblados). Sin embargo, cuando se instala la versión beta 3, se producen errores, como errores que no se pueden encontrar determinados tipos. Esto se debe a que se han pasado varios tipos de ASP.NET Web Pages a diferentes espacios de nombres para la versión beta 3.
> 
> **Solución alternativa**   
> Borre la carpeta *bin* de la aplicación implementada, copie los nuevos ensamblados en la carpeta (o vuelva a implementar la aplicación) y, a continuación, reinicie la aplicación.

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
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problema: usar el proyecto de aplicación web o ASP.NET MVC y páginas web de ASP.NET en la misma aplicación

> Si usaba ASP.NET Web Pages en un proyecto de aplicación web o en una aplicación ASP.NET MVC, es posible que vea un error que no se puede encontrar *WebPageHttpApplication* .
> 
> **Solución alternativa**  
> Si recibe este error, cambie la clase base de la que se deriva la aplicación. En el archivo *global. asax* , cambie la siguiente línea:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> Por esta otra:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> En efecto, se invierte un cambio introducido para la versión beta 1 de ASP.NET Web Pages con sintaxis Razor.

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: implementación de una aplicación en un equipo que no tiene SQL Server Compact instalado

> Las aplicaciones que incluyen SQL Server Compact bases de datos se pueden ejecutar en un equipo en el que no esté instalado SQL Server Compact. Microsoft WebMatrix beta 3 copia automáticamente estos archivos binarios y realiza las transformaciones del archivo *Web. config* adecuado.
> 
> **Solución alternativa** Si necesita copiar estos archivos y hacer que los cambios en el archivo *Web. config* se realicen manualmente, haga lo siguiente:
> 
> 1. Copie los ensamblados del motor de base de datos en la carpeta *bin* (y en las subcarpetas) de la aplicación en el equipo de destino: 
> 
>     - Copie *c:\Archivos de programa\microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **en** *\Bin*
>     - Copie *c:\Archivos de programa\microsoft SQL Server Compact Edition\v4.0\Private\x86\\* * **en** *\Bin\x86*
>     - Copie *c:\Archivos de programa\microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **en** *\Bin\amd64*
> 2. En la carpeta raíz del sitio web, cree o abra un archivo *Web. config* . (En WebMatrix beta 3, este tipo de archivo está disponible si hace clic en **todo** en el cuadro de diálogo **elegir un tipo de archivo** ).
> 3. Agregue el siguiente elemento como elemento secundario del elemento **&lt;configuration&gt;** (no dentro del elemento **&lt;system. Web&gt;** ):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: las aplicaciones auxiliares de la base de datos y WebGrid no funcionan en un nivel de confianza medio en Visual Basic

> Si usa Visual Basic (crear archivos *. vbhtml* ), las aplicaciones auxiliares de `Database` y `WebGrid` no funcionarán si la aplicación está configurada para usar el nivel de confianza medio.
> 
> **Solución alternativa**  
> Configure temporalmente la aplicación para que utilice plena confianza.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problema: no se reconoce la propiedad "Encrypt"

> SQL Server Compact 4,0 no reconoce la propiedad `Encrypt` de la clase `SqlCeConnection`. No debe utilizar esta propiedad para cifrar archivos de base de datos. La propiedad `Encrypt` quedó en desuso en SQL Server Compact versión 3,5 y se reservó solo por compatibilidad con versiones anteriores. 
> 
> **Solución alternativa**  
> Utilice la propiedad `Encryption Mode` de la clase `SqlCeConnection` para cifrar SQL Server Compact archivos de base de datos 4,0. En el ejemplo siguiente se muestra cómo crear una base de datos cifrada SQL Server Compact 4,0 mediante la propiedad `Encryption Mode`:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Para cambiar el modo de cifrado de una base de datos SQL Server Compact 4,0 existente, haga lo siguiente:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Para cifrar una base de datos SQL Server Compact 4,0 no cifrada, haga lo siguiente:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problema: se necesitan C++ las bibliotecas en tiempo de ejecución de Microsoft Visual 2008

> Los archivos DLL nativos de SQL Server Compact 4,0 necesitan las bibliotecas C++ en tiempo de ejecución de Microsoft Visual 2008 (x86, IA64 y x64), Service Pack 1.
> 
> **Solución alternativa**  
> Instale el .NET Framework 3,5 SP1. Esto también instala las bibliotecas en C++ tiempo de ejecución de Visual 2008 SP1. Puede descargar las bibliotecas desde la siguiente ubicación:   
>   
> [Actualización de C++ seguridad ATL del paquete redistribuible de Microsoft Visual 2008 Service Pack 1](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Tenga en cuenta que la instalación del .NET Framework 2,0, 3,0 o 4 *no* instala las C++ bibliotecas en tiempo de ejecución de Visual 2008 SP1.

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problema: si SQL Server Compact se instala antes de instalar .NET Framework en el equipo, el nombre invariable del proveedor no se registra en el archivo Machine. config de .NET Framework

> SQL Server Compact se puede instalar en un equipo que no tiene .NET Framework instalado porque SQL Server Compact requiere .NET Framework. Si no se instalan .NET Framework versión 3,5 ni 4 antes de instalar SQL Server Compact, el programa de instalación de SQL Server Compact no registra el nombre invariable del proveedor en el archivo *Machine. config* . Cualquier aplicación que se base en la entrada SQL Server Compact en el archivo *Machine. config* producirá un error. La entrada de registro de nombre invariable en *Machine. config* es similar al ejemplo siguiente:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Solución alternativa**  
> Desinstale SQL Server Compact 4,0 CTP1. Descargue e instale las versiones completas del .NET Framework desde la siguiente ubicación:
> 
> [Microsoft .NET Framework 3,5 Service Pack 1 (paquete completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Versión 4,0 de Microsoft .NET Framework (paquete completo)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> A continuación, reinstale [SQL Server Compact 4,0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Instalación de aplicaciones

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: la instalación de una aplicación puede tardar mucho tiempo si la carpeta Mis documentos del usuario se redirige a un recurso compartido de red.

> **Solución alternativa**  
> Ninguno. La aplicación puede tardar un tiempo en instalarse, pero se instalará correctamente.

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Publicar aplicaciones

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

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Otros problemas

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problema: la búsqueda o el filtro no funcionan en los informes de agrupar por: tipo de problema

> Al ejecutar un informe para un sitio, si escribe texto en el cuadro *filtrar por dirección URL* y hace clic en *Buscar*, no sucede nada. Esto se debe a que este control no es funcional mientras que el estado de *grupo por* el informe se establece en *tipo de problema*, que es el valor predeterminado.
> 
> **Solución alternativa** En la pestaña *Agrupar por* de la cinta de opciones, haga clic en *dirección URL* para agrupar las entradas por su dirección URL de origen. El cuadro de texto y el botón para filtrar las entradas son funcionales mientras se encuentre en este estado.

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problema: las aplicaciones WCF no se pueden ejecutar con IIS Express

> La exploración de una aplicación WCF genera un error similar al siguiente:
> 
> *No se pudo cargar el archivo o ensamblado ' Microsoft. Web. Administration, version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35 ' o una de sus dependencias. El sistema no encuentra el archivo especificado.*
> 
> Esto se debe a que IIS Express versión beta no es compatible con WCF de forma predeterminada.
> 
> **Solución alternativa** Use una de las siguientes soluciones alternativas (solución #2 requiere Microsoft Windows Vista o posterior):
> 
> 
> 1. Copie los ensamblados *Microsoft. Web. dll* y *Microsoft. Web. Administration. dll* de la ubicación de instalación de WebMatrix en el directorio *bin* de la aplicación WCF. De forma predeterminada, WebMatrix se instala en la subcarpeta de *Microsoft WebMatrix* , en la carpeta *archivos de programa* del sistema.
> 2. En Microsoft Windows Vista o posterior, cree un vínculo simbólico a los ensamblados en el directorio *bin* mediante los siguientes comandos. (Este enfoque tiene la ventaja de que no crea una copia de los ensamblados).
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Instale los dos ensamblados en la GAC. En un símbolo del sistema con privilegios elevados, ejecute los siguientes comandos:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: WebMatrix beta 3 no puede realizar determinadas tareas que requieren elevación

> WebMatrix beta 3 no puede realizar determinadas tareas que requieren elevación, como la instalación de componentes adicionales en las situaciones siguientes:
> 
> - En Windows Vista o Windows 7, inició sesión con una cuenta que no tiene privilegios administrativos y el control de cuentas de usuario (UAC) está deshabilitado.
> - Utiliza Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Solución alternativa**  
> La mayoría de las tareas de WebMatrix beta 3 no requieren permisos administrativos. Para los que sí lo hacen, puede realizar la operación como administrador o seguir estos pasos:
> 
> - En Windows Vista o Windows 7, habilite UAC.
> - En Windows XP, agregue el usuario al grupo de seguridad administradores.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "sitio de la galería Web" está deshabilitado

> La opción **sitio de la galería web** está deshabilitada si el instalador de plataforma web 3,0 no está instalado.
> 
> **Solución alternativa**  
> Instale el [Instalador de plataforma web de Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problema: en Windows Server 2003, IIS Express no se inicia para un usuario no administrativo

> En Windows Server 2003, cuando se inicia una página o se inicia IIS Express, IIS Express no se inicia. En el caso de las páginas web, se muestra un error que indica que un usuario no administrativo ha iniciado la aplicación.
> 
> **Solución alternativa**  
> Inicie WebMatrix beta 3 como usuario administrativo. Para obtener más información, vea el siguiente artículo de KnowledgeBase:  
>   
> [Una aplicación iniciada por un usuario no administrativo no puede escuchar el tráfico HTTP del equipo en el que se ejecuta la aplicación en Windows Vista, Windows Server 2003 o Windows XP.](https://support.microsoft.com/kb/939786)

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

#### <a name="issue-the-relationships-button-is-disabled"></a>Problema: el botón "relaciones" está deshabilitado

> El botón **relaciones** en la pestaña **tabla** del área de trabajo **bases de datos** está deshabilitado para las bases de datos de SQL Server Compact.
> 
> **Solución alternativa**  
> Ninguno. SQL Server Compact no es compatible con las relaciones entre las tablas.

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problema: las consultas SQL parametrizadas producen excepciones

> En SQL Server Compact 4,0, si no se especifica un tipo de datos como `SqlDbType` o `DbType` para los parámetros de las consultas con parámetros, se produce una excepción cuando se ejecuta la consulta.
> 
> **Solución alternativa**  
> Establezca explícitamente el tipo de datos para los parámetros como `SqlDbType` o `DbType`. Esto es fundamental en el caso de los tipos de datos BLOB (`image` y `ntext`). Use código similar al siguiente:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a>Para obtener más información

Para obtener más información sobre WebMatrix beta 3, consulte los siguientes sitios web:

- [IIS.net](http://iis.net/)
- [ASP.NET](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

---

© 2010 Microsoft Corporation. Reservados todos los derechos. [Términos de uso](https://msdn.microsoft.cos/cc300389.aspx).
