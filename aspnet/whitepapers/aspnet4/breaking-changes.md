---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 cambios importantes | Microsoft Docs
author: rick-anderson
description: En este documento se describen los cambios realizados en la versión 4 de .NET Framework que pueden afectar a las aplicaciones que se crearon con...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 8ccad3b40a723c92a3164de082e1f94577141008
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78439513"
---
# <a name="aspnet-4-breaking-changes"></a>Cambios importantes de ASP.NET 4

> En este documento se describen los cambios realizados en la versión 4 de .NET Framework que pueden afectar a las aplicaciones que se crearon con versiones anteriores, incluidas las versiones beta 1 y beta 2 de ASP.NET 4.
> 
> [Descargue estas notas del producto](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)

<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Contenido

[Configuración de ControlRenderingCompatibilityVersion en el archivo Web. config](#0.1__Toc256770141 "_Toc256770141")  
[ClientIDMode cambios](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode y UrlEncode ahora codifican comillas simples](#0.1__Toc256770143 "_Toc256770143")  
[El analizador de páginas ASP.NET (. aspx) es más estricto](#0.1__Toc256770144 "_Toc256770144")  
[Archivos de definición del explorador actualizados](#0.1__Toc256770145 "_Toc256770145")  
[System. Web. Mobile. dll quitado del archivo de configuración Web raíz](#0.1__Toc256770146 "_Toc256770146")  
[Validación de solicitud de ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[El algoritmo hash predeterminado es ahora HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Errores de configuración relacionados con la nueva configuración raíz de ASP.NET 4](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 las aplicaciones secundarias no se inician cuando se están en aplicaciones ASP.NET 2,0 o ASP.NET 3,5](#0.1__Toc256770150 "_Toc256770150")  
[ASP.NET 4 los sitios web no se inician en los equipos en los que SharePoint está instalado](#0.1__Toc256770151 "_Toc256770151")  
[La propiedad HttpRequest. FilePath ya no incluye los valores de PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[Las aplicaciones de ASP.NET 2,0 pueden generar errores de HttpException que hacen referencia a EURL. axd](#0.1__Toc256770153 "_Toc256770153")  
[Es posible que los controladores de eventos no se generen en un documento predeterminado en el modo integrado de IIS 7 o IIS 7,5](#0.1__Toc256770154 "_Toc256770154")  
[Cambios en la implementación de seguridad de acceso del código (CAS) de ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[Se han migrado MembershipUser y otros tipos en el espacio de nombres System. Web. Security](#0.1__Toc256770156 "_Toc256770156")  
[El almacenamiento en caché de resultados cambia para variar \* encabezado HTTP](#0.1__Toc256770157 "_Toc256770157")  
[System. Web. Security Types for Passport está obsoleto](#0.1__Toc256770158 "_Toc256770158")  
[La propiedad MenuItem. PopOutImageUrl no representa una imagen en ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[MENU. StaticPopOutImageUrl y menu. DynamicPopOutImageUrl no pueden representar imágenes cuando las rutas de acceso contienen barras diagonales inversas](#0.1__Toc256770160 "_Toc256770160")  
[Ausencia](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Configuración de ControlRenderingCompatibilityVersion en el archivo Web. config

Los controles ASP.NET se han modificado en la versión 4 de .NET Framework para que pueda especificar de forma más precisa cómo representan el marcado. En versiones anteriores del .NET Framework, algunos controles emitieron marcado que no se podía deshabilitar. De forma predeterminada, ASP.NET 4 este tipo de marcado ya no se genera.

Si usa Visual Studio 2010 para actualizar la aplicación desde ASP.NET 2,0 o ASP.NET 3,5, la herramienta agrega automáticamente una configuración al archivo `Web.config` que conserva la representación heredada. Sin embargo, si actualiza una aplicación cambiando el grupo de aplicaciones en IIS para elegir como destino .NET Framework 4, ASP.NET usa el nuevo modo de representación de forma predeterminada. Para deshabilitar el nuevo modo de representación, agregue la siguiente configuración en el archivo `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Los cambios de representación principales que aporta el nuevo comportamiento son los siguientes:

- Los controles **Image** e **ImageButton** ya no representan un atributo `border="0"`.
- La clase **BaseValidator** y los controles de validación que derivan de ella ya no representan texto rojo de forma predeterminada.
- El control **HtmlForm** no representa un atributo de **nombre** .
- El control de **tabla** ya no representa un atributo de `border="0"`.
- Los controles que no están diseñados para la entrada del usuario (por ejemplo, el control de **etiqueta** ) ya no representan el atributo `disabled="disabled"` si su propiedad **Enabled** está establecida en **false** (o si heredan esta configuración de un control contenedor).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>ClientIDMode cambios

El valor **ClientIDMode** de ASP.net 4 le permite especificar cómo ASP.net genera el atributo **ID** para los elementos HTML. En versiones anteriores de ASP.NET, el comportamiento predeterminado era equivalente al valor de **AutoID** de **ClientIDMode**. Sin embargo, el valor predeterminado es ahora **predecible**.

Si usa Visual Studio 2010 para actualizar la aplicación desde ASP.NET 2,0 o ASP.NET 3,5, la herramienta agrega automáticamente una configuración al archivo `Web.config` que conserva el comportamiento de las versiones anteriores del .NET Framework. Sin embargo, si actualiza una aplicación cambiando el grupo de aplicaciones en IIS para elegir como destino .NET Framework 4, ASP.NET usará el nuevo modo de forma predeterminada. Para deshabilitar el nuevo modo de identificador de cliente, agregue la siguiente configuración en el archivo `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode y UrlEncode ahora codifican comillas simples

En ASP.NET 4, los métodos **HtmlEncode** y **UrlEncode** de las clases **HttpUtility** y **HttpServerUtility** se han actualizado para codificar el carácter de comilla simple (') como se indica a continuación:

- El método **HtmlEncode** codifica las instancias de la comilla simple como '.
- El método **UrlEncode** codifica las instancias de la comilla simple como %27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>El analizador de páginas ASP.NET (. aspx) es más estricto

El analizador de páginas para páginas ASP.NET (archivos`.aspx`) y controles de usuario (archivos`.ascx`) es más estricto en ASP.NET 4 y rechazará más instancias de marcado no válido. Por ejemplo, los dos fragmentos de código siguientes se analizarán correctamente en las versiones anteriores de ASP.NET, pero ahora generarán errores de analizador en ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Observe el punto y coma no válido al final de la etiqueta **HiddenField** .

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Observe el atributo de **estilo** sin cerrar que se ejecuta en el atributo **CssClass** .

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>Archivos de definición del explorador actualizados

Los archivos de definición de explorador se han actualizado para que incluyan información sobre exploradores y dispositivos nuevos y actualizados. Se han quitado exploradores y dispositivos anteriores, como Netscape Navigator, y se han agregado exploradores y dispositivos más recientes, como Google Chrome y Apple iPhone.

Si la aplicación contiene definiciones de explorador personalizadas que heredan de una de las definiciones de explorador que se han quitado, aparecerá un error. Por ejemplo, si la carpeta `App_Browsers` contiene una definición de explorador que hereda de la definición del explorador IE2, recibirá el siguiente mensaje de error de configuración:

- No se encuentra el elemento de puerta de enlace o el explorador con el identificador ' IE2 '.

> [!NOTE]
> Los archivos de definiciones del explorador controlan el objeto **HttpBrowserCapabilities** (que se expone mediante la propiedad **request. Browser** de la página). Por lo tanto, la información devuelta por el acceso a una propiedad de este objeto en ASP.NET 4 podría ser diferente de la información devuelta en una versión anterior de ASP.NET.

Puede revertir a los archivos de definición del explorador antiguos copiando los archivos de definición del explorador de la siguiente carpeta:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Copie los archivos en la carpeta de `\CONFIG\Browsers` correspondiente para ASP.NET 4. Después de copiar los archivos, ejecute la herramienta de línea de comandos ASPNET\_regbrowsers. exe.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System. Web. Mobile. dll quitado del archivo de configuración Web raíz

En versiones anteriores de ASP.NET, se incluyó una referencia al ensamblado System. Web. Mobile. dll en el archivo `Web.config` raíz de la sección de **ensamblados** en. Para mejorar el rendimiento, se quitó la referencia a este ensamblado.

El ensamblado System. Web. Mobile. dll se incluye en ASP.NET 4, pero está en desuso. Si desea usar tipos del ensamblado System. Web. Mobile. dll, agregue una referencia a este ensamblado en el archivo `Web.config` raíz o en un archivo de `Web.config` de la aplicación. Por ejemplo, si desea utilizar cualquiera de los controles móviles de ASP.NET (en desuso), debe agregar una referencia al ensamblado System. Web. Mobile. dll en el archivo de `Web.config`.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Validación de solicitud de ASP.NET

La característica de validación de solicitudes de ASP.NET proporciona un cierto nivel de protección predeterminada contra ataques de scripting entre sitios (XSS). En versiones anteriores de ASP.NET, la validación de solicitudes estaba habilitada de forma predeterminada. Sin embargo, solo se aplica a las páginas ASP.NET (archivos de`.aspx` y sus archivos de clase) y solo cuando se ejecutan esas páginas.

En ASP.NET 4, de forma predeterminada, la validación de solicitudes está habilitada para todas las solicitudes, ya que está habilitada antes de la fase **BeginRequest** de una solicitud HTTP. Como resultado, la validación de solicitudes se aplica a las solicitudes de todos los recursos de ASP.NET, no solo de las solicitudes de página. aspx. Esto incluye solicitudes como llamadas a servicios web y controladores HTTP personalizados. La validación de solicitudes también está activa cuando los módulos HTTP personalizados leen el contenido de una solicitud HTTP.

Como resultado, ahora se pueden producir errores de validación de solicitudes para las solicitudes que anteriormente no desencadenaron errores. Para revertir al comportamiento de la característica de validación de solicitudes de ASP.NET 2,0, agregue la siguiente configuración en el archivo `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Sin embargo, se recomienda que analice los errores de validación de solicitudes para determinar si los controladores existentes, los módulos u otro código personalizado tiene acceso a entradas HTTP potencialmente no seguras que podrían ser vectores de ataque de XSS.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>El algoritmo hash predeterminado es ahora HMACSHA256

ASP.NET usa cifrado y algoritmos hash para ayudar a proteger los datos, como las cookies de autenticación de formularios y el estado de vista. De forma predeterminada, ASP.NET 4 ahora usa el algoritmo HMACSHA256 para las operaciones hash en las cookies y el estado de vista. Las versiones anteriores de ASP.NET usaban el algoritmo HMACSHA1 anterior.

Las aplicaciones pueden verse afectadas si se ejecutan entornos mixtos de ASP.NET 2.0/ASP. NET 4 en los que los datos, como las cookies de autenticación de formularios, deben funcionar con las versiones de .NET Framework. Para configurar una aplicación Web de ASP.NET 4 para que use el algoritmo HMACSHA1 anterior, agregue la siguiente configuración en el archivo `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Errores de configuración relacionados con la nueva configuración raíz de ASP.NET 4

Los archivos de configuración raíz (el archivo de `machine.config` y el archivo raíz `Web.config`) de la .NET Framework 4 (y, por tanto, ASP.NET 4) se han actualizado para incluir la mayor parte de la información de configuración reutilizable que en ASP.NET 3,5 se encontró en los archivos de `Web.config` de la aplicación. Debido a la complejidad de los sistemas de configuración de IIS 7 e IIS 7,5 administrados, la ejecución de aplicaciones de ASP.NET 3,5 en ASP.NET 4 y en IIS 7 e IIS 7,5 puede producir errores de configuración de ASP.NET o IIS.

Se recomienda actualizar las aplicaciones de ASP.NET 3,5 a ASP.NET 4 mediante el uso de las herramientas de actualización de proyectos en Visual Studio 2010, si es práctico. Visual Studio 2010 modifica automáticamente el archivo `Web.config` de la aplicación ASP.NET 3,5 para que contenga la configuración adecuada para ASP.NET 4.

Sin embargo, es un escenario admitido para ejecutar aplicaciones de ASP.NET 3,5 mediante el .NET Framework 4 sin volver a compilar. En ese caso, es posible que tenga que modificar manualmente el archivo de `Web.config` de la aplicación antes de ejecutar la aplicación en el .NET Framework 4 y en IIS 7 o IIS 7,5.

En las dos secciones siguientes se describen los cambios que es posible que deba realizar para diferentes combinaciones de software.

**Windows Vista SP1 o Windows Server 2008 SP1, donde no se instalan la revisión KB958854 ni el SP2.** En esta configuración, el sistema de configuración de IIS 7 combina incorrectamente la configuración administrada de una aplicación comparando el archivo de `Web.config` de nivel de aplicación con los archivos de ASP.NET 2,0 `machine.config`. Por este motivo, los archivos de `Web.config` de nivel de aplicación del .NET Framework 3,5 o posterior deben tener una definición de la sección de configuración **System. Web. Extensions** (el elemento) para que no se produzca un error de validación de IIS 7.

Sin embargo, las entradas de archivo `Web.config` de nivel de aplicación modificadas manualmente que no coincidan exactamente con las definiciones de la sección de configuración reutilizable original que se introdujeron con Visual Studio 2008 provocarán errores de configuración de ASP.NET. (Las entradas de configuración predeterminadas generadas por Visual Studio 2008 funcionan correctamente). Un problema común es que los archivos de `Web.config` modificados de forma manual omiten los atributos de configuración **allowDefinition** y **requirePermission** que se encuentran en diversas definiciones de sección de configuración. Esto provoca una falta de coincidencia entre la sección de configuración abreviada en los archivos de `Web.config` de nivel de aplicación y la definición completa en el archivo de `machine.config` ASP.NET 4. Como resultado, en tiempo de ejecución, el sistema de configuración ASP.NET 4 produce un error de configuración.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 y también Windows Vista SP1 y Windows Server 2008 SP1, donde está instalada la revisión KB958854.**

En este escenario, el sistema de configuración nativa de IIS 7 e IIS 7,5 devuelve un error de configuración porque realiza una comparación de texto en el atributo de **tipo** definido para un controlador de sección de configuración administrada. Dado que todos los `Web.config` archivos generados por Visual Studio 2008 y Visual Studio 2008 SP1 tienen "3,5" en la cadena de tipo del sistema. controladores de la sección de configuración **Web. Extensions** (y relacionados), y dado que el archivo ASP.NET 4 `machine.config` tiene "4,0" en el atributo **Type** para los mismos controladores de sección de configuración, las aplicaciones que se generan en Visual Studio 2008 o Visual Studio 2008 SP1 siempre producen un error en la validación de configuración en iis 7 y IIS 7,5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Resolver estos problemas

La solución alternativa para el primer escenario es actualizar el archivo de `Web.config` de nivel de aplicación incluyendo el texto de configuración reutilizable de un archivo `Web.config` generado automáticamente por Visual Studio 2008.

Una solución alternativa para el primer escenario es instalar el Service Pack 2 para vista o Windows Server 2008 en el equipo, o bien instalar la revisión KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) para corregir el comportamiento incorrecto de la combinación de configuración del sistema de configuración de IIS. Sin embargo, después de realizar cualquiera de estas acciones, es probable que la aplicación detecte un error de configuración debido al problema descrito en el segundo escenario.

La solución alternativa para el segundo escenario es eliminar o comentar todas las definiciones de la sección de configuración de **System. Web. Extensions** y las definiciones de grupos de secciones de configuración del archivo de `Web.config` en el nivel de la aplicación. Estas definiciones suelen estar en la parte superior del archivo de `Web.config` de nivel de aplicación y se pueden identificar mediante el elemento **configSections** y sus elementos secundarios.

En ambos escenarios, se recomienda que también elimine manualmente la sección **System. CodeDom** , aunque esto no es necesario.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 las aplicaciones secundarias no se inician cuando se están en aplicaciones ASP.NET 2,0 o ASP.NET 3,5

Las aplicaciones de ASP.NET 4 configuradas como elementos secundarios de aplicaciones que ejecutan versiones anteriores de ASP.NET podrían no iniciarse debido a errores de configuración o de compilación. En el ejemplo siguiente se muestra una estructura de directorios para una aplicación afectada.

`/parentwebapp` (configurado para usar ASP.NET 2,0 o ASP.NET 3,5)  
`/childwebapp` (configurado para usar ASP.NET 4)

La aplicación en la carpeta `childwebapp` no se iniciará en IIS 7 o IIS 7,5 y notificará un error de configuración. El texto de error incluirá un mensaje similar al siguiente:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

En IIS 6, la aplicación de la carpeta `childwebapp` tampoco podrá iniciarse, pero notificará un error diferente. Por ejemplo, el texto del error podría indicar lo siguiente:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Estos escenarios se producen porque la información de configuración de la aplicación primaria en la carpeta `parentwebapp` forma parte de la jerarquía de información de configuración que determina los valores de configuración finales combinados que usa la aplicación web secundaria en la carpeta `childwebapp`. En función de si la aplicación Web ASP.NET 4 se está ejecutando en IIS 7 (o IIS 7,5) o en IIS 6, el sistema de configuración de IIS o el sistema de compilación ASP.NET 4 devolverán un error.

Los pasos que debe seguir para resolver este problema y para que la aplicación secundaria de ASP.NET 4 funcione dependen de si la aplicación ASP.NET 4 se ejecuta en IIS 6 o en IIS 7 (o IIS 7,5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Paso 1 (solo IIS 7 o IIS 7,5)

Este paso solo es necesario en los sistemas operativos que ejecutan IIS 7 o IIS 7,5, que incluye Windows Vista, Windows Server 2008, Windows 7 y Windows Server 2008 R2.

Mueva la definición de **configSections** en el archivo de `Web.config` de la aplicación primaria (la aplicación que ejecuta ASP.net 2,0 o ASP.net 3,5) en el archivo raíz `Web.config` para the.NET Framework 2,0. El sistema de configuración nativa de IIS 7 e IIS 7,5 examina el elemento **configSections** cuando combina la jerarquía de archivos de configuración. Mover la definición de **configSections** desde el archivo de `Web.config` de la aplicación web primaria al archivo raíz `Web.config` oculta de forma eficaz el elemento del proceso de combinación de configuración que se produce para la aplicación secundaria ASP.net 4.

En un sistema operativo de 32 bits o en los grupos de aplicaciones de 32 bits, el archivo de `Web.config` raíz para ASP.NET 2,0 y ASP.NET 3,5 se encuentra normalmente en la carpeta siguiente:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

En un sistema operativo de 64 bits o en los grupos de aplicaciones de 64 bits, el archivo de `Web.config` raíz para ASP.NET 2,0 y ASP.NET 3,5 se encuentra normalmente en la carpeta siguiente:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Si ejecuta aplicaciones Web de 32 y 64 bits en un equipo de 64 bits, debe trasladar el elemento **configSections** hacia arriba en los archivos de `Web.config` raíz para los sistemas de 32 y de 64 bits.

Al colocar el elemento **configSections** en el archivo `Web.config` raíz, pegue la sección inmediatamente después del elemento de **configuración** . En el ejemplo siguiente se muestra el aspecto de la parte superior del archivo `Web.config` raíz cuando ha terminado de mover los elementos.

> [!NOTE]
> En el ejemplo siguiente, las líneas se han ajustado para mejorar la legibilidad.

[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Paso 2 (todas las versiones de IIS)

Este paso es necesario si la aplicación web secundaria ASP.NET 4 se está ejecutando en IIS 6 o en IIS 7 (o IIS 7,5).

En el archivo de `Web.config` de la aplicación web primaria que ejecuta ASP.NET 2 o ASP.NET 3,5, agregue una etiqueta de **Ubicación** que especifique explícitamente (para los sistemas de configuración de IIS y ASP.net) que las entradas de configuración solo se aplican a la aplicación web primaria. En el ejemplo siguiente se muestra la sintaxis del elemento **Location** que se va a agregar:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

En el ejemplo siguiente se muestra cómo se usa la etiqueta **Location** para incluir todas las secciones de configuración comenzando por la sección **appSettings** y finalizando con la sección **System. WebServer** .

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Cuando haya completado los pasos 1 y 2, las aplicaciones web secundarias de ASP.NET 4 se iniciarán sin errores.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>ASP.NET 4 los sitios web no se inician en los equipos en los que SharePoint está instalado

Los servidores web que ejecutan SharePoint tienen un archivo `Web.config` que se implementa en la raíz de un sitio web de SharePoint (por ejemplo, `c:\inetpub\wwwroot\web.config` para sitio web predeterminado). En este archivo de `Web.config`, SharePoint establece un nivel de confianza parcial personalizado denominado WSS\_mínima.

Si intenta ejecutar un sitio web de ASP.NET 4 que está implementado como un elemento secundario de este tipo de sitio web de SharePoint, verá el siguiente error:

`Could not find permission set named 'ASP.NET'.`

Este error se produce porque la infraestructura de seguridad de acceso del código (CAS) de ASP.NET 4 busca un conjunto de permisos denominado ASP.NET. Sin embargo, el archivo de configuración de confianza parcial al que hace referencia WSS\_mínimo no contiene ningún conjunto de permisos con ese nombre.

Actualmente no hay disponible una versión de SharePoint que sea compatible con ASP.NET. Como resultado, no debe intentar ejecutar un sitio web de ASP.NET 4 como sitio secundario bajo sitios web de SharePoint.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>La propiedad HttpRequest. FilePath ya no incluye los valores de PathInfo

En las versiones anteriores de ASP.NET se incluía un valor de **PathInfo** en el valor devuelto desde varias propiedades relacionadas con la ruta de acceso de archivo, incluidas **HttpRequest. FilePath**, **HttpRequest. AppRelativeCurrentExecutionFilePath**y **HttpRequest. CurrentExecutionFilePath**. ASP.NET 4 ya no incluye el valor de **PathInfo** en los valores devueltos de estas propiedades. En su lugar, la información de **PathInfo** está disponible en **HttpRequest. PathInfo**. Por ejemplo, veamos el siguiente fragmento de dirección URL:

`/testapp/Action.mvc/SomeAction`

En versiones anteriores de ASP.NET, las propiedades **HttpRequest** tienen los siguientes valores:

**HttpRequest. FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest. PathInfo**: (vacío)

En ASP.NET 4, en su lugar, las propiedades **HttpRequest** tienen los valores siguientes:

**HttpRequest. FilePath**: `/testapp/Action.mvc`

**HttpRequest. PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>Las aplicaciones de ASP.NET 2,0 pueden generar errores de HttpException que hacen referencia a EURL. axd

Una vez que se ha habilitado ASP.NET 4 en IIS 6, las aplicaciones de ASP.NET 2.0 que se ejecutan en IIS 6 (en Windows Server 2003 o Windows Server 2003 R2) podrían generar errores como el siguiente:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Este error se produce porque, cuando ASP.NET detecta que un sitio web está configurado para usar ASP.NET 4, un componente nativo de ASP.NET 4 pasa una dirección URL sin extensión a la parte administrada de ASP.NET para su posterior procesamiento. Sin embargo, si los directorios virtuales que se encuentran debajo de un sitio web de ASP.NET 4 están configurados para usar ASP.NET 2,0, el procesamiento de la dirección URL sin extensión de esta manera da como resultado una dirección URL modificada que contiene la cadena "EURL. axd". Esta dirección URL modificada se envía a la aplicación ASP.NET 2,0. ASP.NET 2,0 no reconoce el formato "EURL. axd". Por lo tanto, ASP.NET 2,0 intenta encontrar un archivo llamado `eurl.axd` y ejecutarlo. Dado que no existe este archivo, se produce un error en la solicitud con una excepción **HttpException** .

Puede solucionar este problema mediante una de las siguientes opciones.

### <a name="option-1"></a>Opción 1

Si ASP.NET 4 no es necesario para ejecutar el sitio web, vuelva a asignar el sitio para usar ASP.NET 2,0 en su lugar.

### <a name="option-2"></a>Opción 2

Si se requiere ASP.NET 4 para ejecutar el sitio web, mueva los directorios virtuales ASP.NET 2,0 secundarios a otro sitio web que esté asignado a ASP.NET 2,0.

### <a name="option-3"></a>Opción 3

Si no es práctico reasignar el sitio web a ASP.NET 2,0 o cambiar la ubicación de un directorio virtual, deshabilite explícitamente el procesamiento de direcciones URL sin extensión en ASP.NET 4. Utilice el siguiente procedimiento:

1. En el registro de Windows, abra el siguiente nodo:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Cree un nuevo valor **DWORD** denominado **EnableExtensionlessUrls**.
2. Establezca **EnableExtensionlessUrls** en 0. Esto deshabilita el comportamiento de la dirección URL sin extensión.
3. Guarde el valor del registro y cierre el editor del registro.
4. Ejecute la herramienta de línea de comandos **iisreset** , que hace que IIS Lea el nuevo valor del registro.

> [!NOTE]
> Al establecer **EnableExtensionlessUrls** en 1, se habilita el comportamiento de la dirección URL sin extensión. Esta es la configuración predeterminada si no se especifica ningún valor.

<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>Es posible que los controladores de eventos no se generen en un documento predeterminado en el modo integrado de IIS 7 o IIS 7,5

ASP.NET 4 incluye modificaciones que cambian cómo se representa el atributo de **acción** del elemento de **formulario** HTML cuando una dirección URL sin extensión se resuelve como un documento predeterminado. Un ejemplo de una dirección URL sin extensión que se resuelve en un documento predeterminado sería [http://contoso.com/](http://contoso.com/), lo que da lugar a una solicitud a [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx).

ASP.NET 4 ahora representa el valor del atributo de **acción** del elemento de **formulario** HTML como una cadena vacía cuando se realiza una solicitud a una dirección URL sin extensión que tiene un documento predeterminado asignado. Por ejemplo, en versiones anteriores de ASP.NET, una solicitud a [http://contoso.com](http://contoso.com) daría como resultado una solicitud a `Default.aspx`. En ese documento, la etiqueta de apertura del **formulario** se representaría como en el ejemplo siguiente:

`<form action="Default.aspx" />`

En ASP.NET 4, una solicitud a [http://contoso.com](http://contoso.com) también produce una solicitud a `Default.aspx`. Sin embargo, ASP.NET ahora representa la etiqueta HTML de apertura del **formulario** como en el ejemplo siguiente:

`<form action="" />`

Esta diferencia en la forma en que se representa el atributo de **acción** puede provocar cambios sutiles en el modo en que IIS y ASP.net procesan una publicación de formulario. Cuando el atributo **Action** es una cadena vacía, el objeto **DefaultDocumentModule** de IIS creará una solicitud secundaria para `Default.aspx`. En la mayoría de las condiciones, esta solicitud secundaria es transparente para el código de aplicación y la página `Default.aspx` se ejecuta con normalidad.

A pesar de ello, una interacción potencial entre el código administrado y el modo integrado de IIS 7 o IIS 7.5 puede hacer que las páginas .aspx administradas dejen de funcionar correctamente durante la solicitud secundaria. Si se producen las condiciones siguientes, la solicitud secundaria a un documento de `Default.aspx` producirá un error o un comportamiento inesperado:

1. Se envía una página. aspx al explorador con el atributo de **acción** del elemento de **formulario** establecido en "".
2. El formulario se devuelve a ASP.NET.
3. Un módulo HTTP administrado Lee parte del cuerpo de la entidad. Por ejemplo, un módulo Lee **request. Form** o **request. params**. Esto hace que el cuerpo de la entidad de la solicitud POST se lea en la memoria administrada. Como resultado, el cuerpo de la entidad ya no está disponible para los módulos de código nativo que se ejecutan en el modo integrado de IIS 7 o IIS 7.5.
4. El objeto **DefaultDocumentModule** de IIS se ejecuta finalmente y crea una solicitud secundaria al documento `Default.aspx`. Sin embargo, dado que un fragmento del código administrado ya ha leído el cuerpo de la entidad, no hay ningún cuerpo de entidad disponible para enviarlo a la solicitud secundaria.
5. Cuando se ejecuta la canalización HTTP para la solicitud secundaria, el controlador de `.aspx` archivos se ejecuta durante la fase de ejecución del controlador.
6. Dado que no hay ningún cuerpo de entidad, no hay ninguna variable de formulario y ningún estado de vista, por lo que no hay información disponible para que el controlador de páginas. aspx determine qué evento (si existe) se supone que se va a producir. Como resultado, no se ejecuta ninguno de los controladores de eventos postback para la página .aspx afectada.

Puede solucionar este comportamiento de las siguientes maneras:

- Identifique el módulo HTTP que está accediendo al cuerpo de la entidad de la solicitud durante las solicitudes de documento predeterminadas y determine si puede configurarse para que se ejecute solo para las solicitudes administradas. En el modo integrado de IIS 7 e IIS 7,5, los módulos HTTP se pueden marcar para que se ejecuten solo para las solicitudes administradas agregando el atributo siguiente a la entrada **System. WebServer/modules** del módulo:

- `precondition="managedHandler"`

- Esta configuración deshabilita el módulo para las solicitudes que IIS 7 e IIS 7,5 determinan como solicitudes no administradas. En el caso de las solicitudes de documento predeterminadas, la primera solicitud es a una dirección URL sin extensión. Por lo tanto, IIS no ejecuta módulos administrados marcados con una condición previa de controlador administrado durante el procesamiento de la solicitud inicial. Como resultado, los módulos administrados no leerán accidentalmente el cuerpo de la entidad y, por lo tanto, el cuerpo de la entidad seguirá estando disponible y se pasará a la solicitud secundaria y al documento predeterminado.

- Si los módulos HTTP problemáticos deben ejecutarse para todas las solicitudes (para los archivos estáticos, para las direcciones URL sin extensión que se resuelven en el objeto **DefaultDocumentModule** , para las solicitudes administradas, etc.), modifique las páginas. aspx afectadas estableciendo explícitamente la propiedad **acción** del control **System. Web. UI. HtmlControls. HtmlForm** de la página en una cadena no vacía. Por ejemplo, si el documento predeterminado es `Default.aspx`, modifique el código de la página para establecer explícitamente la propiedad de **acción** del control **HtmlForm** en "default. aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Cambios en la implementación de seguridad de acceso del código (CAS) de ASP.NET

ASP.NET 2,0 y, por extensión, las características de ASP.NET que se agregaron en 3,5, usan el modelo de seguridad de acceso del código (CAS) de .NET Framework 1,1 y 2,0. Aun así, la implementación de CAS en ASP.NET 4 se ha mejorado considerablemente. Como resultado, las aplicaciones ASP.NET de confianza parcial que se basan en el código de confianza que se ejecuta en la caché de ensamblados global (GAC) podrían producir errores con varias excepciones de seguridad. Las aplicaciones de confianza parcial que se basan en modificaciones extensas en la Directiva de CA de equipo también podrían producir un error con excepciones de seguridad.

Puede revertir las aplicaciones de ASP.NET 4 de confianza parcial al comportamiento de ASP.NET 1,1 y 2,0 con el nuevo atributo **legacyCasModel** en el elemento de configuración de **confianza** , como se muestra en el ejemplo siguiente:

`<trust level= "Medium" legacyCasModel="true" />`

Al revertir al modelo CAS heredado, se habilitan los siguientes comportamientos de CAS antiguos:

- Se respeta la Directiva de CAS del equipo.
- Se permiten varios conjuntos de permisos diferentes en un dominio de aplicación único.
- No se requieren aserciones de permisos explícitas para los ensamblados de la GAC que se invocan cuando solo ASP.NET u otro código .NET Framework está en la pila.

Un escenario no se puede revertir en el .NET Framework 4: las aplicaciones que no son de confianza parcial web ya no pueden llamar a determinadas API en System. Web. dll y System. Web. Extensions. dll. En versiones anteriores de la .NET Framework, era posible que las aplicaciones que no son de confianza parcial de Web se concedieran explícitamente permisos de **AspNetHostingPermission** . Después, estas aplicaciones podrían usar **System. Web. HttpUtility**, los tipos de los espacios de nombres **System. Web. ClientServices.\*** y los tipos relacionados con la pertenencia, los roles y los perfiles. La llamada a estos tipos de aplicaciones de confianza parcial que no son web ya no se admite en el .NET Framework 4.

> [!NOTE]
> La funcionalidad **HtmlEncode** y **HtmlDecode** de la clase **System. Web. HttpUtility** se ha migrado a la nueva clase .NET Framework 4 **System .net. webutility** . Si esa era la única funcionalidad de ASP.NET que se estaba usando, modifique el código de la aplicación para usar la nueva clase **Webutility** en su lugar.

A continuación se encuentra un resumen de alto nivel de los cambios en la implementación de CAS predeterminada en ASP.NET 4:

- Los dominios de aplicación de ASP.NET son ahora dominios de aplicación homogéneos. En un dominio de aplicación solo están disponibles conjuntos de permisos de confianza parcial y plena confianza.
- Los conjuntos de concesión de confianza parcial de ASP.NET son independientes de cualquier directiva CAS de nivel empresarial, de nivel de equipo o de nivel de usuario.
- Los ensamblados de ASP.NET que se distribuyeron en 3,5 y 3,5 SP1 se han convertido para usar el modelo de transparencia de .NET Framework 4.
- El uso del atributo **AspNetHostingPermission** ASP.net se ha reducido considerablemente. La mayoría de las instancias de este atributo se han quitado de las API de ASP.NET públicas.
- Los ensamblados compilados de forma dinámica creados por los proveedores de compilación de ASP.NET se han actualizado para marcar explícitamente los ensamblados como transparentes.
- Todos los ensamblados de ASP.NET se marcan ahora de forma que el atributo APTCA se respeta solo en entornos de hospedaje Web. Los entornos de hospedaje no Web de confianza parcial, como ClickOnce, no podrán llamar a los ensamblados de ASP.NET.

Para obtener más información sobre el nuevo modelo de seguridad de acceso del código de ASP.NET 4, vea [using Code Access Security in ASP.Net Applications](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) en el sitio web de MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>Se han migrado MembershipUser y otros tipos en el espacio de nombres System. Web. Security

Algunos tipos que se usan en la pertenencia a ASP.NET se han pasado de `System.Web.dll` al nuevo ensamblado System. Web. ApplicationServices. dll. Estos tipos se han movido para resolver las dependencias de capas de arquitectura entre los tipos del cliente y de las SKU de .NET Framework extendidas.

Los proyectos de sitios web no tienen problemas como resultado de mover estos tipos, ya que System. Web. ApplicationServices. dll se ha agregado a la lista de ensamblados a los que se hace referencia que el sistema de compilación ASP.NET usa de forma predeterminada. Si actualiza un proyecto de sitio web creado con una versión anterior de ASP.NET a ASP.NET 4 abriéndolo en Visual Studio 2010, el proyecto se compilará sin errores.

Del mismo modo, si actualiza un proyecto de aplicación web creado en una versión anterior de ASP.NET a ASP.NET 4 abriéndolo en Visual Studio 2010, el proceso de actualización agrega una referencia a System. Web. ApplicationServices. dll al proyecto. Por consiguiente, los proyectos de aplicación web actualizados también se compilarán sin errores.

Los archivos compilados (binarios) que se crearon con versiones anteriores de ASP.NET también se ejecutarán sin errores en ASP.NET 4, aunque los tipos de pertenencia se movieron a otro ensamblado. Se ha agregado información de reenvío de tipos a la versión ASP.NET 4 de `System.Web.dll` que enruta automáticamente las referencias en tiempo de ejecución de estos tipos a la nueva ubicación de los tipos.

Sin embargo, las bibliotecas de clases que usan tipos de pertenencia específicos y que se han actualizado desde versiones anteriores de ASP.NET no se compilarán cuando se usen en un proyecto de ASP.NET 4. Por ejemplo, un proyecto de biblioteca de clases podría no compilarse e informar de un error como el siguiente:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Puede solucionar este problema agregando una referencia en el proyecto de biblioteca de clases a System. Web. ApplicationServices. dll.

En la lista siguiente se muestran los tipos *System. Web. Security* que se movieron de `System.Web.dll` a System. Web. ApplicationServices. dll:

- *System. Web. Security. MembershipCreateStatus@*
- *System. Web. Security. Membership. CreateUserException*
- *System. Web. Security. MembershipPasswordException*
- *System. Web. Security. MembershipPasswordFormat*
- *System. Web. Security. MembershipProvider*
- *System. Web. Security. MembershipProviderCollection*
- *System. Web. Security. MembershipUser*
- *System. Web. Security. MembershipUserCollection*
- *System. Web. Security. MembershipValidatePasswordEventHandler*
- *System. Web. Security. ValidatePasswordEventArgs*
- *System. Web. Security. RoleProvider*
- <a id="0.1_a"></a>*System. Web. Configuration. MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>El almacenamiento en caché de resultados cambia para variar \* encabezado HTTP

En ASP.NET 1,0, un error provocó que las páginas almacenadas en caché especificaban `Location="ServerAndClient"` como una configuración de caché de resultados para emitir un encabezado HTTP `Vary:*` en la respuesta. Como consecuencia, se indicaba a los exploradores de cliente que nunca almacenasen en caché la página localmente.

En ASP.NET 1,1, se ha agregado el método **System. Web. HttpCachePolicy. SetOmitVaryStar** , al que se puede llamar para suprimir el encabezado `Vary:*`. Se eligió este método porque el cambio del encabezado HTTP emitido se consideró un cambio potencialmente importante en este momento. Sin embargo, los desarrolladores han sido confundidos por el comportamiento de ASP.NET y los informes de errores sugieren que los desarrolladores no saben cuál es el comportamiento de **SetOmitVaryStar** existente.

En ASP.NET 4, se tomó la decisión de corregir el problema raíz. El encabezado HTTP `Vary:*` ya no se emite desde las respuestas que especifican la siguiente directiva:

`<%@OutputCache Location="ServerAndClient" %>`

Como resultado, **SetOmitVaryStar** ya no es necesario para suprimir el encabezado `Vary:*`.

En las aplicaciones que especifican `Location="ServerAndClient"` en la directiva **@ OutputCache** en una página, ahora verá el comportamiento implícito por el nombre del valor del atributo de **Ubicación** , es decir, las páginas se podrán almacenar en caché en el explorador sin necesidad de llamar al método **SetOmitVaryStar** .

Si las páginas de la aplicación deben emitir `Vary:*`, llame al método **AppendHeader** , como en el ejemplo siguiente:

`HttpResponse.AppendHeader("Vary","*");`

Como alternativa, puede cambiar el valor del atributo **Ubicación** de almacenamiento en caché de resultados a "servidor".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>System. Web. Security Types for Passport está obsoleto

La compatibilidad con Passport integrada en ASP.NET 2,0 está obsoleta y no se admite durante unos años debido a cambios en Passport (ahora LiveID). Como resultado, los cinco tipos relacionados con Passport en **System. Web. Security** ahora se marcan con el atributo **ObsoleteAttribute** .

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>La propiedad MenuItem. PopOutImageUrl no representa una imagen en ASP.NET 4

En ASP.NET 3,5, la propiedad *MenuItem. PopOutImageUrl* permite especificar la dirección URL de una imagen que se muestra en un elemento de menú para indicar que el elemento de menú tiene un submenú dinámico. En el ejemplo siguiente se muestra cómo especificar esta propiedad en el marcado en ASP.NET 3,5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

Como resultado de un cambio de diseño en ASP.NET 4, no se representa ninguna salida para *PopOutImageUrl* si la propiedad está establecida para la clase *MenuItem* . En su lugar, debe especificar una dirección URL de imagen directamente en el control de *menú* mediante la propiedad *StaticPopOutImageUrl* o la propiedad *DynamicPopOutImageUrl* . Cuando se trabaja con un menú estático, la propiedad *Menu. StaticPopOutImageUrl* especifica la dirección URL de una imagen que se muestra para indicar que el elemento de menú estático tiene un submenú, tal como se muestra en el ejemplo siguiente:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Si está trabajando con un menú dinámico, use la propiedad *Menu. DynamicPopOutImageUrl* para especificar la dirección URL de una imagen que indica que un elemento de menú dinámico tiene un submenú. El siguiente ejemplo es similar al anterior, pero muestra cómo establecer la propiedad *DynamicPopOutImageUrl* para un menú dinámico.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Si no se establece la propiedad *Menu. DynamicPopOutImageUrl* y la propiedad *Menu. DynamicEnableDefaultPopOutImage* se establece en *false*, no se muestra ninguna imagen. Del mismo modo, si no se establece la propiedad *StaticPopOutImageUrl* y la propiedad *StaticEnableDefaultPopOutImage* se establece en *false*, no se muestra ninguna imagen.

Cuando establezca las rutas de acceso para estas propiedades, use una barra diagonal (/) en lugar de una barra diagonal inversa (\). Para obtener más información, vea [Menu. StaticPopOutImageUrl y menu. DynamicPopOutImageUrl no pueden representar imágenes cuando las rutas de acceso contienen barras diagonales inversas](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu. StaticPopOutImageUrl_and_Menu.") en otro lugar de este documento.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>MENU. StaticPopOutImageUrl y menu. DynamicPopOutImageUrl no pueden representar imágenes cuando las rutas de acceso contienen barras diagonales inversas

En ASP.NET 4, las imágenes que se especifican con las propiedades *Menu. StaticPopOutImageUrl* y *Menu. DynamicPopOutImageUrl* no se representarán si la ruta de acceso contiene backlashes (\). Se trata de un cambio respecto a las versiones anteriores de ASP.NET.

En el siguiente ejemplo de marcado de control de *menú* se muestra el conjunto de propiedades *StaticPopOutImageUrl* con una ruta de acceso que contiene una barra diagonal inversa. En ASP.NET 4, la imagen especificada en la propiedad no se representará.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Para corregir este problema, cambie los valores de ruta de acceso que se especifican en las propiedades *StaticPopOutImageUrl* y *DynamicPopOutImageUrl* para usar barras diagonales (/). En el ejemplo siguiente se muestra este cambio:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Tenga en cuenta que las aplicaciones que se migraron desde versiones anteriores de ASP.NET a ASP.NET 4 también podrían verse afectadas, ya que se ha cambiado la propiedad *MenuItem. PopOutImageUrl* . Para obtener más información, vea [la propiedad MenuItem. PopOutImageUrl no puede representar una imagen en ASP.net 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem. PopOutImageUrl_Propert") en otro lugar de este documento.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Declinación de responsabilidades

Este documento es preliminar y puede cambiar considerablemente antes de la versión comercial final del software que se describe en él.

La información que incluye este documento representa el punto de vista actual de Microsoft Corporation sobre las cuestiones descritas en la fecha de la publicación. Debido a que Microsoft debe responder a las condiciones de mercado cambiantes, no se debe interpretar como un compromiso por parte de Microsoft y Microsoft no puede garantizar la precisión de la información presentada después de la fecha de publicación.

Estas notas del producto solo tienen fines informativos. MICROSOFT NO OTORGA NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O ESTUTARIAS, EN LA INFORMACIÓN DE ESTE DOCUMENTO.

Es responsabilidad del usuario el cumplimiento de toda la legislación aplicable en materia de copyright. Sin limitación de los derechos protegidos por copyright, ninguna parte del presente documento podrá ser reproducida, almacenada o introducida en un sistema de recuperación, o bien transmitida en ninguna forma o medio (electrónico, mecánico, mediante fotocopia o grabación, etc.), ni con ningún propósito, sin la autorización expresa y por escrito de Microsoft Corporation.

Microsoft puede ser titular de patentes, solicitudes de patentes, marcas, derechos de autor u otros derechos de propiedad intelectual sobre los contenidos de este documento. Este documento no otorga ninguna licencia sobre estas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que se indique expresamente en un contrato escrito de licencia de Microsoft.

A menos que se indique lo contrario, las compañías, organizaciones, productos, nombres de dominio, direcciones de correo electrónico, logotipos, personas, lugares y eventos que se describen aquí son ficticios y no tienen ninguna asociación con ninguna empresa, organización, producto, nombre de dominio y correo electrónico reales. la dirección, el logotipo, la persona, el lugar o el evento está pensado o debe deducirse.

© 2010 Microsoft Corporation. All rights reserved.

Microsoft y Windows son marcas registradas o marcas comerciales de Microsoft Corporation en Estados Unidos y en otros países.

Los nombres de las compañías y productos reales mencionados en el presente documento pueden ser marcas comerciales de sus respectivos propietarios.
