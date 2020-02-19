---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Generar perfiles y depurar la aplicación ASP.NET MVC con el fin de obtener | Microsoft Docs
author: Rick-Anderson
description: La perspectiva es una familia de paquetes de NuGet de código abierto que se encuentra en constante crecimiento y que proporciona información detallada sobre el rendimiento, la depuración y el diagnóstico de ASP.NET...
ms.author: riande
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: d3689147a3bc3aa1f4180c377d2483a94bdd95a9
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457666"
---
# <a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Crear un perfil y depurar la aplicación de ASP.NET MVC con Glimpse

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> La visión es una creciente familia de paquetes NuGet de código abierto que proporcionan información detallada sobre rendimiento, depuración y diagnóstico para aplicaciones ASP.NET. Es trivial instalar, ligera, ultra rápida y muestra las métricas de rendimiento clave en la parte inferior de cada página. Le permite profundizar en la aplicación cuando necesite averiguar lo que está ocurriendo en el servidor. El análisis proporciona una información muy valiosa que se recomienda para usarla durante todo el ciclo de desarrollo, incluido el entorno de prueba de Azure. Aunque [Fiddler](http://www.telerik.com/fiddler) y las [herramientas de desarrollo de F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) proporcionan una vista del lado cliente, el vistazo proporciona una vista detallada del servidor. Este tutorial se centrará en el uso de los paquetes de ASP.NET MVC y EF, pero hay muchos otros paquetes disponibles. Siempre que sea posible, crearé un vínculo a los documentos de la [perspectiva](http://getglimpse.com/Docs/) que ayuda a mantener. La visión es un proyecto de código abierto, que puede contribuir al código fuente y a los documentos.

- [Instalación de la visión](#ig)
- [Habilitar la visión para localhost](#eg)
- [La pestaña escala de tiempo](#Time)
- [Enlace de modelos](#mb)
- [Rutas](#route)
- [Uso de la visión en Azure](#da)
- [Recursos adicionales](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalación de la visión

Puede instalar la visión desde la consola del administrador de paquetes NuGet o desde la consola de **Administración de paquetes Nuget** . En esta demostración, instalaré los paquetes Mvc5 y EF6:

![instalación del vistazo desde NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Busque el *. EF.*

![Vistazo a. EF desde la instalación de NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Al seleccionar **paquetes instalados**, puede ver los módulos dependientes que se han instalado:

![Se han instalado los paquetes de visión desde DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

Los siguientes comandos instalan los módulos de MVC5 y EF6 desde la consola del administrador de paquetes:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Habilitar la visión para localhost

Vaya a http://localhost:&lt;p ordenar #&gt;/Glimpse.axd y haga clic en el botón <strong>Activar</strong> .

![Página de introducción a axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Si se muestra la barra de favoritos, puede arrastrar y colocar los botones de visión y agregarlos como bookmarklets:

![IE con bookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Ahora puede navegar por la aplicación y se muestra la **pantalla de seguimiento** (HUD) en la parte inferior de la página.

![Página Contact Manager con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

En la [página ver HUD](http://getglimpse.com/Docs/Heads-up-Display) se detalla la información de control de tiempo que se muestra anteriormente. Los datos de rendimiento discretos que muestra el HUD pueden avisarle de un problema inmediatamente, antes de llegar al ciclo de pruebas. Al hacer clic en la &quot;g&quot; en la esquina inferior derecha, se abre el panel de visión:

![Panel de visión](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

En la imagen anterior, se selecciona la [pestaña ejecución](http://getglimpse.com/Docs/Execution-Tab) , que muestra los detalles de tiempo de las acciones y los filtros de la canalización. En la fase 6 de la canalización, puede ver que el [temporizador de filtro de detención del reloj](http://www.nuget.org/packages/StopWatch/) se ha iniciado. Aunque el temporizador de peso claro puede proporcionar datos de perfil y control de tiempo útiles, se pierde todo el tiempo invertido en la autorización y en la representación de la vista. Puede leer sobre mi temporizador en el [perfil y la hora de la aplicación ASP.NET MVC en Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). La página [pestañas](http://getglimpse.com/Docs/Tabs) proporciona vínculos a información detallada sobre cada pestaña.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>La pestaña escala de tiempo

He modificado el [tutorial de EF 6/MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) pendiente de Tom Dykstra con el siguiente cambio de código en el controlador de instructores:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

El código anterior me permite pasar la cadena de consulta (`eager`) para controlar la carga diligente o explícita de los datos. En la imagen siguiente, se usa la carga explícita y en la página control de tiempo se muestra cada inscripción cargada en el método de acción `Index`:

![carga explícita](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

En el código siguiente, se especifica diligente y se captura cada inscripción después de que se llame a la vista `Index`:

![se especifica diligente](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Puede mantener el mouse sobre un segmento de tiempo para obtener información detallada sobre el tiempo:

![Mantenga el puntero para ver el tiempo detallado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Enlace de modelos

La [pestaña enlace de modelos](http://getglimpse.com/Docs/Model-Binding-Tab) proporciona una gran cantidad de información para ayudarle a entender cómo se enlazan las variables de formulario y por qué algunos no están enlazados como cabría esperar. En la imagen siguiente se muestra el **?** , en el que puede hacer clic en para abrir la página de ayuda de la característica.

![vista de enlace de modelos de visión](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Rutas

 La pestaña ver rutas puede ayudarle a depurar y comprender el enrutamiento. En la imagen siguiente, se selecciona la ruta del producto (y se muestra en verde, una Convención de visión). también se muestran ![nombre del producto seleccionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) restricciones de ruta, áreas y tokens de datos. Para obtener más información, consulte las [rutas](http://getglimpse.com/Docs/Routes-Tab) y el [enrutamiento de atributos en ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) . 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Uso de la visión en Azure

La Directiva de seguridad predeterminada de la visualización solo permite mostrar datos del host local. Puede cambiar esta directiva de seguridad para poder ver estos datos en un servidor remoto (por ejemplo, una aplicación web en Azure). Para entornos de prueba en Azure, agregue la marca resaltada a la parte inferior del archivo *Web. config* para habilitar el análisis:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Con este cambio solo, cualquier usuario puede ver los datos en un sitio remoto. Considere la posibilidad de agregar el marcado anterior a un perfil de publicación para que solo se implemente un aplicado cuando se use ese perfil de publicación (por ejemplo, su perfil de prueba de Azure). Para restringir la consulta de datos, agregaremos el rol `canViewGlimpseData` y solo permitiremos a los usuarios de este rol ver datos de visión.

Quite los comentarios del archivo *GlimpseSecurityPolicy.CS* y cambie la llamada de [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) de `Administrator` al rol de `canViewGlimpseData`:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Seguridad: los datos enriquecidos que se proporcionan en la vista pueden exponer la seguridad de la aplicación. Microsoft no ha realizado una auditoría de seguridad de la perspectiva de uso en las aplicaciones de producción.

Para más información sobre cómo agregar roles, consulte el tutorial de [implementación de una aplicación Web de ASP.NET MVC 5 en Azure deploy con pertenencia, OAuth y SQL Database en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) .

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Implementación de una aplicación ASP.NET MVC 5 segura con pertenencia, OAuth y SQL Database en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Visión](http://getglimpse.com/Docs/Configuration) de la página de documento sobre la configuración de pestañas, la Directiva de tiempo de ejecución, el registro y mucho más.
