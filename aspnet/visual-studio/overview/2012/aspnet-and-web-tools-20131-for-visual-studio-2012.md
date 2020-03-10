---
uid: visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
title: Notas de la versión de ASP.NET and Web Tools 2013,1 para Visual Studio 2012 | Microsoft Docs
author: microsoft
description: En este documento se describe la versión de ASP.NET and Web Tools 2013,1 para Visual Studio 2012.
ms.author: riande
ms.date: 11/13/2013
ms.assetid: ca26e5bb-630e-41d2-8512-2a9386c431cb
msc.legacyurl: /visual-studio/overview/2012/aspnet-and-web-tools-20131-for-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 260af1018064d60d80cbb1002001f28c4daffffd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78467107"
---
# <a name="release-notes-for-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Notas de la versión de ASP.NET and Web Tools 2013.1 para Visual Studio 2012

por [Microsoft](https://github.com/microsoft)

> En este documento se describe la versión de ASP.NET and Web Tools 2013,1 para Visual Studio 2012.

## <a name="contents"></a>Contenido

- [Notas de instalación](#install)
- [Requisitos de software](#requirements)
- Nuevas características de ASP.NET and Web Tools 2013,1 para Visual Studio 2012

    - [Bootstrap](#bootstrap)
    - [Templates](#templates) (Plantillas [C++])

        - [Plantilla ASP.NET MVC 5](#mvc5template)
        - [Plantilla ASP.NET Web API 2](#apitemplate)
        - [Plantillas de elementos](#itemtemplate)
    - [Entity Framework 6](#ef6)
    - [Scaffolding de ASP.NET](#scaffold)
    - [Editor de Razor](#razor)
    - [NuGet 2.7](#nuget)
- Problemas conocidos y cambios importantes

    - [Scaffolding de ASP.NET](#issuescaffolding)

        - [Scaffolding de MVC y API Web: HTTP 404, error no encontrado](#404issue)
        - [Visual Studio Express 2012 para web deja de funcionar después de agregar un elemento con scaffolding](#expressissue)
    - [ASP.NET Razor 3](#issuerazor)

        - [Al ver el archivo cshtml con la exploración con o F5 se produce un error de servidor](#browseissue)
        - [Reescritura de URL y tilde (~)](#rewriteissue)
    - [Templates](#templateissue) (Plantillas [C++])

<a id="install"></a>
## <a name="installation-notes"></a>Notas de la instalación

[Instalar](https://www.microsoft.com/web/handlers/webpi.ashx/getinstaller/WebNode11Pack.appids) ASP.NET and Web Tools 2013,1 para Visual Studio 2012.

<a id="requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Debe tener Visual Studio 2012 o Visual Studio Express 2012 para Web.

## <a name="new-features-in-aspnet-and-web-tools-20131-for-visual-studio-2012"></a>Nuevas características de ASP.NET and Web Tools 2013,1 para Visual Studio 2012

<a id="bootstrap"></a>
### <a name="bootstrap"></a>Bootstrap

Al aplicar scaffolding a los controladores y vistas de MVC 5, el marcado de las vistas utiliza [bootstrap](http://getbootstrap.com/).

<a id="templates"></a>
### <a name="templates"></a>Plantillas

<a id="mvc5template"></a>
#### <a name="aspnet-mvc-5-template"></a>Plantilla ASP.NET MVC 5

Hemos agregado una nueva plantilla de MVC 5. Hace referencia a los paquetes de NuGet de MVC 5 más recientes, y puede usar scaffolding para agregar controladores y vistas.

<a id="apitemplate"></a>
#### <a name="aspnet-web-api-2-template"></a>Plantilla ASP.NET Web API 2

Hemos agregado una nueva plantilla de Web API 2. Hace referencia a los paquetes de NuGet de Web API 2 más recientes, y puede usar la técnica scaffolding para agregar controladores y vistas.

<a id="itemtemplate"></a>
#### <a name="item-templates"></a>Plantillas de elementos

Hemos agregado nuevas plantillas de elementos para las vistas de MVC 5, las páginas web (Razor 3) y los controladores de Web API 2. Instalan los paquetes de NuGet relacionados en el proyecto al agregar nuevos elementos.

<a id="ef6"></a>
### <a name="entity-framework-6"></a>Entity Framework 6

Al aplicar scaffolding a un controlador de MVC o API Web mediante Entity Framework, usamos el marco 6. Para obtener más información acerca de Entity Framework, vea el [historial de versiones de Entity Framework](https://msdn.com/data/jj574253).

También puede descargar e instalar las herramientas de Entity Framework 6 para Visual Studio 2012. Vea la [Entity Framework Get](https://msdn.com/data/ee712906#tooling).

<a id="scaffold"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

ASP.NET scaffolding es un marco de generación de código para aplicaciones Web de ASP.NET. Facilita la adición de código reutilizable al proyecto que interactúa con un modelo de datos.

En versiones anteriores de Visual Studio, el scaffolding estaba limitado a los proyectos de MVC de ASP.NET. Con esta actualización, ahora puede usar la técnica scaffolding para cualquier proyecto de ASP.NET, incluidos los formularios Web Forms. Esta actualización no admite la generación de páginas para un proyecto de formularios Web Forms, pero puede seguir usando la técnica scaffolding con formularios Web Forms agregando dependencias de MVC al proyecto. La compatibilidad con la generación de páginas para formularios Web Forms se agregará en una actualización futura.

Al usar la técnica scaffolding, garantizamos que todas las dependencias necesarias están instaladas en el proyecto. Por ejemplo, si comienza con un proyecto de formularios Web Forms de ASP.NET y, a continuación, usa scaffolding para agregar un controlador de API Web, los paquetes y las referencias de NuGet necesarios se agregan automáticamente al proyecto.

Para agregar scaffolding de MVC a un proyecto de formularios Web Forms, agregue un **nuevo elemento con scaffolding** y seleccione **dependencias de MVC 5** en la ventana del cuadro de diálogo. Hay dos opciones para la scaffolding de MVC; Mínima y completa. Si selecciona mínima, solo se agregan al proyecto los paquetes NuGet y las referencias para ASP.NET MVC. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC.

La compatibilidad con los controladores asincrónicos de scaffolding usa las nuevas características de Async de Entity Framework 6.

Para obtener más información y tutoriales, vea [información general sobre scaffolding de ASP.net](../2013/aspnet-scaffolding-overview.md). Estos tutoriales muestran el scaffolding con Visual Studio 2013, pero también son aplicables a ASP.NET and Web Tools 2013,1 para Visual Studio 2012.

<a id="razor"></a>
### <a name="razor-editor"></a>Editor de Razor

Con esta actualización, Visual Studio 2012 ahora es compatible con las herramientas y ediciones de Razor 3.

<a id="nuget"></a>
### <a name="nuget-27"></a>NuGet 2.7

NuGet 2,7 incluye un amplio conjunto de nuevas características que se describen en detalle en las notas de la [versión de nuget 2,7](http://docs.nuget.org/docs/release-notes/nuget-2.7).

Esta versión de NuGet elimina la necesidad de que los usuarios permitan explícitamente que NuGet restaure los paquetes que faltan. Al instalar NuGet 2,7, los usuarios aceptan implícitamente la restauración automática de los paquetes que faltan. Los usuarios pueden rechazar explícitamente la restauración de paquetes a través de la configuración de NuGet en Visual Studio. Este cambio simplifica el funcionamiento de la restauración de paquetes.

## <a name="known-issues-and-breaking-changes"></a>Problemas conocidos y cambios importantes

<a id="issuescaffolding"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

<a id="404issue"></a>
#### <a name="mvc-and-web-api-scaffolding---http-404-not-found-error"></a>Scaffolding de MVC y API Web: HTTP 404, error no encontrado

Si se produce un error al agregar un elemento con scaffolding a un proyecto, es posible que el proyecto se quede en un estado incoherente. Algunos de los cambios que se realizan son los scaffolding se revertirán, pero otros cambios, como los paquetes de NuGet instalados, no se revertirán. Si se revierten los cambios de configuración de enrutamiento, los usuarios recibirán un error HTTP 404 al navegar a elementos con scaffolding.

Para corregir este error para MVC, agregue un nuevo elemento con scaffolding y seleccione dependencias de MVC 5 (mínimo o completo). Este proceso agregará todos los cambios necesarios en el proyecto.

Para corregir este error para la API Web:

1. Agregue la siguiente clase WebApiConfig al proyecto.

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample1.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample2.vb)]
2. Configure WebApiConfig. Register en el método de inicio de la aplicación\_en global. asax de la siguiente manera:

    [!code-csharp[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample3.cs)]

    [!code-vb[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample4.vb)]

<a id="expressissue"></a>
#### <a name="visual-studio-express-2012-for-web-stops-working-after-adding-a-scaffolded-item"></a>Visual Studio Express 2012 para web deja de funcionar después de agregar un elemento con scaffolding

Si Visual Studio Express 2012 para web deja de funcionar después de agregar el elemento con scaffolding con Entity Framework (como el controlador Web API 2 con acciones, con Entity Framework), es posible que Visual Studio Express no pueda cargar la imagen nativa de un ensamblado. depende de System. Web. Extensions.

Para corregir este problema, configure Visual Studio Express para que funcione con la imagen de MSIL de System. Web. Extensions:

1. Abra el símbolo del sistema en el modo de administrador.
2. Vaya a%ProgramFiles%\Microsoft Visual Studio 11.0 \ Common7\IDE o% ProgramFiles (x86)% \ Microsoft Visual Studio 11.0 \ Common7\IDE (para Windows de 64 bits).
3. Abra VWDExpress. exe. config en un editor de texto.
4. Agregue la siguiente línea en el&gt;de configuración de &lt;/&lt;elemento&gt; en tiempo de ejecución:  

    [!code-xml[Main](aspnet-and-web-tools-20131-for-visual-studio-2012/samples/sample5.xml)]
5. Reinicie Visual Studio Express 2012 para Web.

<a id="issuerazor"></a>
### <a name="aspnet-razor-3"></a>ASP.NET Razor 3

<a id="browseissue"></a>
#### <a name="viewing-cshtml-file-with-browse-with-or-f5-causes-a-server-error"></a>Al ver el archivo cshtml con la exploración con o F5 se produce un error de servidor

Al crear un proyecto de MVC 5 en Visual Studio 2012 (o abrir en Visual Studio 2012 un proyecto de MVC 5 que se creó en Visual Studio 2013) e intentar ver un archivo cshtml con examinar con o F5, recibirá un error que indica: **error de servidor en la aplicación '/'** . El servidor intenta navegar a `http://localhost:XXXX/Views/../XXXX.cshtml`

Para resolver este problema, cambie la configuración de **acción de inicio** en el proyecto a una **página específica**. No es necesario proporcionar un valor para la página.

![](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image1.png)

Después de hacer este cambio, al seleccionar F5 se navega a la raíz de la aplicación (`http://localhost:XXXX`). Este comportamiento no es el mismo que el comportamiento de los proyectos de MVC 5 en Visual Studio 2013, donde la configuración de **Página actual** inicia la página abierta.

<a id="rewriteissue"></a>
#### <a name="url-rewrite-and-tilde"></a>Reescritura de URL y tilde (~)

Después de actualizar a ASP.NET Razor 3 o ASP.NET MVC 5, la notación de tilde (~) ya no funcionará correctamente si usa reescritura de direcciones URL. La reescritura de direcciones URL afecta a la tilde (~) en elementos HTML como &lt;A/&gt;, &lt;SCRIPT/&gt;, &lt;LINK/&gt;y, como resultado, la tilde ya no se asigna al directorio raíz.

Por ejemplo, si reescribe solicitudes de **ASP.net/Content** en **ASP.net**, el atributo href de &lt;a href = "~/Content/"/&gt; se resuelve en **/content/content/** en lugar de en **/** . Para suprimir este cambio, puede establecer el contexto de **IIS\_WasUrlRewritten** en false en cada página web o en **Application\_BeginRequest** en global. asax.

<a id="templateissue"></a>
### <a name="templates"></a>Plantillas

Al crear proyectos de ASP.NET MVC con Visual Studio 2012 en Windows 8.1 o Windows Server 2012 R2, Visual Studio muestra un mensaje de error que indica "error al configurar web [URL] para ASP.NET 4,5."

![error de configuración](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image2.png)

Verá este error porque Visual Studio 2012 no habilita la característica ASP.NET 4,5 cuando está instalado en esas versiones de Windows. Para habilitar ASP.NET 4,5, siga los pasos descritos en [activar o desactivar las características de Windows](https://windows.microsoft.com/windows-8/turn-windows-features-on-off).

![activar o desactivar las características de Windows](aspnet-and-web-tools-20131-for-visual-studio-2012/_static/image3.png)

Como alternativa, puede habilitar ASP.NET 4,5 a través de la línea de comandos.

1. Abra el símbolo del sistema en el modo de administrador.
2. Ejecute el siguiente comando para habilitar ASP.NET 4,5.  
    `dism /Online /Enable-Feature /FeatureName:NetFx4Extended-ASPNET45 /Quiet /NoRestart`
