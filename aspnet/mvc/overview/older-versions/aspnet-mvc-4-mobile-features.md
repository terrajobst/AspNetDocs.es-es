---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Características móviles de MVC 4 de ASP.NET | Microsoft Docs
author: Rick-Anderson
description: Ahora hay una versión de MVC 5 de este tutorial con ejemplos de código en implementación de una aplicación Web de ASP.NET MVC 5 Mobile en sitios web de Azure.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 9716def069ca9f7115af32e16381f41bd4d13342
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457653"
---
# <a name="aspnet-mvc-4-mobile-features"></a>Características para móviles de ASP.NET MVC 4

por [Rick Anderson](https://twitter.com/RickAndMSFT)

> Ahora hay una versión de MVC 5 de este tutorial con ejemplos de código en [implementación de una aplicación Web de ASP.NET MVC 5 Mobile en sitios web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).

Este tutorial le enseñará los aspectos básicos de cómo trabajar con las características móviles en una aplicación Web de ASP.NET MVC 4. En este tutorial, puede usar [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o visual web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer o vWD existente&quot;). Puede usar la versión Professional de Visual Studio si ya la tiene.

Antes de empezar, asegúrese de que ha instalado los requisitos previos que se enumeran a continuación.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (recomendado) o Visual Studio Web Developer Express SP1. Visual Studio 2012 contiene ASP.NET MVC 4. Si usa Visual Web Developer 2010, debe instalar [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

También necesitará un emulador de explorador móvil. Funcionará cualquiera de las siguientes opciones:

- [Emulador de Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Este es el emulador que se usa en la mayoría de las capturas de pantalla de este tutorial).
- Cambie la cadena de agente de usuario para emular un iPhone. Vea [esta](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) entrada de blog.
- [Emulador móvil de opera](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) con el agente de usuario establecido en iPhone. Para obtener instrucciones sobre cómo establecer el agente de usuario en Safari en "iPhone", consulte [Cómo dejar que Safari simule que es Internet Explorer en el](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) blog de David Alison.

Para este tema hay disponibles proyectos de Visual Studio con código fuente en C#:

- [Descarga del proyecto inicial](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Descarga de proyecto completada](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Lo que creará

En este tutorial, agregará características móviles a la aplicación de lista de conferencias simple que se proporciona en el [proyecto de inicio](https://go.microsoft.com/fwlink/?LinkId=228307). En la captura de pantalla siguiente se muestra la página etiquetas de la aplicación completada, tal como se muestra en el [emulador de Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Consulte [asignación de teclado para Windows Phone emulador](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) para simplificar la entrada de teclado.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Puede usar Internet Explorer versión 9 o 10, FireFox o Chrome para desarrollar la aplicación móvil mediante el establecimiento de la [cadena de agente de usuario](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). En la imagen siguiente se muestra el tutorial completado con Internet Explorer emulación de un iPhone. Puede usar las herramientas de desarrollo de Internet Explorer F-12 y la [herramienta Fiddler](http://www.fiddler2.com/fiddler2/) para ayudar a depurar la aplicación.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Habilidades que aprenderá

Aprenderá lo siguiente:

- Cómo usan las plantillas de ASP.NET MVC 4 el atributo HTML5 `viewport` y la representación adaptable para mejorar la presentación en dispositivos móviles.
- Cómo crear vistas específicas para dispositivos móviles.
- Cómo crear un modificador de vista que permita a los usuarios alternar entre una vista móvil y una vista del escritorio de la aplicación.

### <a name="getting-started"></a>Introducción

Descargue la aplicación de lista de conferencias para el proyecto de inicio con el siguiente vínculo: [Descargar](https://go.microsoft.com/fwlink/?LinkId=228307). A continuación, en el explorador de Windows, haga clic con el botón secundario en el archivo *MvcMobile. zip* y elija **propiedades**. En el cuadro de diálogo **propiedades de MvcMobile. zip** , elija el botón **desbloquear** . (El desbloqueo evita recibir una advertencia de seguridad que se produce al intentar usar un archivo *.zip* que ha descargado de la web).

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Haga clic con el botón secundario en el archivo *MvcMobile. zip* y seleccione **extraer todo** para descomprimir el archivo. En Visual Studio, abra el archivo *MvcMobile. sln* .

Presione CTRL + F5 para ejecutar la aplicación, que la mostrará en el explorador de escritorio. Inicie el emulador de explorador móvil, copie la dirección URL de la aplicación de conferencia en el emulador y, a continuación, haga clic en el vínculo **Buscar por etiqueta** . Si usa el emulador de Windows Phone, haga clic en la barra de dirección URL y presione la tecla Pausa para obtener acceso al teclado. En la imagen siguiente se muestra la vista *AllTags* (desde el elegir **examinar por etiqueta**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

La pantalla es muy fácil de leer en un dispositivo móvil. Elija el vínculo ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

La vista de etiquetas ASP.NET está muy abarrotada. Por ejemplo, la columna **Date** es muy difícil de leer. Más adelante en el tutorial, creará una versión de la vista *AllTags* que es específica para los exploradores móviles y que hará que la pantalla sea legible.

Nota: actualmente existe un error en el motor de almacenamiento en caché móvil. En el caso de las aplicaciones de producción, debe instalar el paquete [fixed DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) . Consulte [ASP.NET MVC 4 Mobile Caching error corregido](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) para obtener más información sobre la corrección.

## <a name="css-media-queries"></a>Consultas de medios CSS

[Las consultas de elementos multimedia de CSS](http://www.w3.org/TR/css3-mediaqueries/) son una extensión de CSS para los tipos de medios. Permiten crear reglas que invalidan las reglas predeterminadas de CSS para exploradores específicos (agentes de usuario). Una regla común para CSS que tiene como destino exploradores móviles es definir el tamaño máximo de la pantalla. El archivo *Content\Site.CSS* que se crea al crear un nuevo proyecto de Internet de ASP.NET MVC 4 contiene la siguiente consulta de medios:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Si la ventana del explorador tiene 850 píxeles de ancho o menos, utilizará las reglas de CSS dentro de este bloque multimedia. Puede usar consultas multimedia de CSS como esta para proporcionar una mejor visualización del contenido HTML en exploradores pequeños (como los exploradores móviles) que las reglas predeterminadas de CSS que están diseñadas para las presentaciones más amplias de los exploradores de escritorio.

## <a name="the-viewport-meta-tag"></a>Etiqueta meta de la ventanilla

La mayoría de los exploradores móviles definen un ancho de la ventana del explorador virtual (la *ventanilla*) mucho mayor que el ancho real del dispositivo móvil. Esto permite que los exploradores móviles se ajusten a toda la página web dentro de la pantalla virtual. Después, los usuarios pueden acercar el contenido interesante. Sin embargo, si establece el ancho de la ventanilla en el ancho real del dispositivo, no es necesario hacer zoom, ya que el contenido cabe en el explorador móvil.

La ventanilla `<meta>` etiqueta del archivo de diseño ASP.NET MVC 4 establece la ventanilla en el ancho del dispositivo. En la línea siguiente se muestra la ventanilla `<meta>` etiqueta en el archivo de diseño de ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Examinar el efecto de las consultas multimedia de CSS y la etiqueta meta de la ventanilla

Abra el archivo *Views\Shared\\_Layout. cshtml* en el editor y comente la etiqueta de la ventanilla `<meta>`. El marcado siguiente muestra la línea comentada.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Abra el archivo *MvcMobile\Content\Site.CSS* en el editor y cambie el ancho máximo de la consulta de medios a cero píxeles. Esto impedirá que las reglas de CSS se utilicen en exploradores móviles. En la línea siguiente se muestra la consulta de medios modificada:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Guarde los cambios y vaya a la aplicación de conferencia en un emulador de explorador móvil. El texto pequeño de la imagen siguiente es el resultado de quitar la ventanilla `<meta>` etiqueta. Sin ventanilla `<meta>` etiqueta, el explorador está alejando el ancho predeterminado de la ventanilla (850 píxeles o más en la mayoría de los exploradores móviles).

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Deshacer los cambios: Quite los comentarios de la ventanilla `<meta>` etiqueta en el archivo de diseño y restaure la consulta de medios en 850 píxeles en el archivo *site. CSS* . Guarde los cambios y actualice el explorador móvil para comprobar que se ha restaurado la presentación para dispositivos móviles.

La ventanilla `<meta>` etiqueta y la consulta multimedia de CSS no son específicas de ASP.NET MVC 4, y puede aprovechar estas características en cualquier aplicación Web. Pero ahora están integrados en los archivos que se generan al crear un nuevo proyecto de ASP.NET MVC 4.

Para obtener más información acerca de la ventanilla `<meta>` etiqueta, vea [un testigo de dos ventanillas, parte dos](http://www.quirksmode.org/mobile/viewports2.html).

En la sección siguiente, verá cómo proporcionar vistas específicas de explorador móvil.

## <a name="overriding-views-layouts-and-partial-views"></a>Reemplazar vistas, diseños y vistas parciales

Una nueva característica importante de ASP.NET MVC 4 es un mecanismo sencillo que permite invalidar cualquier vista (incluidos diseños y vistas parciales) para exploradores móviles en general, para un explorador móvil individual o para cualquier explorador específico. Para proporcionar una vista específica para móviles, puede copiar un archivo de vista y agregar *.Mobile* al nombre del archivo. Por ejemplo, para crear una vista de *Índice* móvil, copie *Views\Home\Index.cshtml* en *Views\Home\Index.Mobile.cshtml*.

En esta sección, creará un archivo de diseño específico para móviles.

Para empezar, copie *Views\Shared\\_Layout. cshtml* en *Views\Shared\\_Layout. Mobile. cshtml*. Abra *\_layout. Mobile. cshtml* y cambie el título de **MVC4 Conference** a **Conference (Mobile)** .

En cada llamada `Html.ActionLink`, quite "Buscar por" en cada vínculo *ActionLink*. En el código siguiente se muestra la sección del cuerpo completado del archivo de diseño móvil.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copie el archivo *Views\Home\AllTags.cshtml* en *Views\Home\AllTags.Mobile.cshtml*. Abra el archivo nuevo y cambie el elemento `<h2>` de "Tags" a "Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Diríjase a la página de etiquetas usando un explorador de escritorio y un emulador de explorador móvil. El emulador de explorador móvil muestra los dos cambios realizados.

[![p2m_layoutTags. Mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

En cambio, la pantalla del escritorio no ha cambiado.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Vistas específicas del explorador

Además de vistas específicas de explorador móvil y de explorador de escritorio, puede crear vistas para un explorador individual. Por ejemplo, puede crear vistas específicas del explorador de iPhone. En esta sección, creará un diseño para el explorador de iPhone y una versión de iPhone de la vista *AllTags* .

Abra el archivo *global. asax* y agregue el código siguiente al método `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Este código define un nuevo modo de visualización llamado "iPhone" que se comparará con cada solicitud entrante. Si la solicitud entrante coincide con la condición definida (es decir, si el agente de usuario contiene la cadena "iPhone"), ASP.NET MVC buscará vistas cuyo nombre contenga el sufijo "iPhone".

En el código, haga clic con el botón derecho en `DefaultDisplayMode`, elija **Resolver** y, luego, `using System.Web.WebPages;`. De este modo, se agrega una referencia al espacio de nombres `System.Web.WebPages`, que es donde se definen los tipos `DisplayModes` y `DefaultDisplayMode`.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Como alternativa, puede agregar manualmente la siguiente línea a la sección `using` del archivo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

A continuación se muestra el contenido completo del archivo *global. asax* .

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Guarde los cambios. Copie el archivo *MvcMobile\Views\Shared\\_Layout. Mobile. cshtml* en *MvcMobile\Views\Shared\\_Layout. iPhone. cshtml*. Abra el nuevo archivo y, a continuación, cambie el encabezado `h1` de `Conference (Mobile)` a `Conference (iPhone)`.

Copie el archivo *MvcMobile\Views\Home\AllTags.Mobile.cshtml* en *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. En el archivo nuevo, cambie el elemento `<h2>` de "Tags (M)" a "Tags (iPhone)".

Ejecute la aplicación. Ejecute el emulador de explorador móvil, asegúrese de que su agente de usuario esté establecido en "iPhone" y diríjase a la vista *AllTags* . En la captura de pantalla siguiente se muestra la vista *AllTags* representada en el explorador [Safari](http://www.apple.com/safari/download/) . Puede descargar Safari para Windows [aquí](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

En esta sección, hemos visto cómo crear vistas y diseños móviles y cómo crear vistas y diseños para dispositivos específicos como el iPhone. En la sección siguiente, verá cómo aprovechar jQuery Mobile para ver vistas móviles más atractivas.

## <a name="using-jquery-mobile"></a>Usar jQuery Mobile

La biblioteca de [jQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) proporciona un marco de interfaz de usuario que funciona en todos los exploradores móviles principales. jQuery Mobile aplica la *mejora progresiva* a los exploradores móviles que admiten CSS y JavaScript. La mejora progresiva permite que todos los exploradores muestren el contenido básico de una página web, a la vez que permite que los exploradores y dispositivos más eficaces tengan una pantalla más enriquecida. Los archivos JavaScript y CSS que se incluyen con jQuery Mobile Style tienen muchos elementos para ajustarse a los exploradores móviles sin realizar ningún cambio de marcado.

En esta sección, instalará el paquete NuGet *jQuery. Mobile. Mvc* , que instala jQuery Mobile y un widget de selector de vistas.

Para empezar, elimine los archivos *\\Compartidos _Layout. Mobile. cshtml* y *\\compartidos _Layout. iPhone. cshtml* que creó anteriormente.

Cambie el nombre de los archivos *Views\Home\AllTags.Mobile.cshtml* y *Views\Home\AllTags.iPhone.cshtml* a *Views\Home\AllTags.iPhone.cshtml.Hide* y *Views\Home\AllTags.Mobile.cshtml.Hide*. Dado que los archivos ya no tienen la extensión *. cshtml* , el tiempo de ejecución de ASP.NET MVC no los usará para representar la vista *AllTags* .

Instale el paquete NuGet *jQuery. Mobile. Mvc* de la siguiente manera:

1. En el menú **herramientas** , seleccione **Administrador de paquetes NuGet**y, a continuación, seleccione **consola del administrador de paquetes**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. En la **consola del administrador de paquetes**, escriba `Install-Package jQuery.Mobile.MVC -version 1.0.0`

En la imagen siguiente se muestran los archivos agregados y modificados en el proyecto MvcMobile por el paquete NuGet jQuery. Mobile. MVC. Los archivos que se agregan tienen [Add] anexados después del nombre de archivo. La imagen no muestra los archivos GIF y PNG agregados a la carpeta *Content\images*

![](aspnet-mvc-4-mobile-features/_static/image21.png)

El paquete NuGet jQuery. Mobile. MVC instala lo siguiente:

- La *aplicación\_archivo Start\BundleMobileConfig.CS* , que es necesario para hacer referencia a los archivos de jQuery JavaScript y CSS agregados. Debe seguir las instrucciones que se indican a continuación y hacer referencia al paquete móvil definido en este archivo.
- archivos CSS móviles de jQuery.
- Un widget de controlador de `ViewSwitcher` (*Controllers\ViewSwitcherController.CS*).
- archivos de JavaScript para móviles de jQuery.
- Un archivo de diseño de estilo móvil jQuery (*Views\Shared\\_Layout. Mobile. cshtml*).
- Vista parcial de un modificador de vista *(MvcMobile\Views\Shared\\_ViewSwitcher. cshtml*) que proporciona un vínculo en la parte superior de cada página para cambiar de la vista de escritorio a la vista móvil y viceversa.
- Varios archivos de imagen<em>. png</em> y <em>. gif</em> en la carpeta <em>Content\images</em>

Abra el archivo *global. asax* y agregue el código siguiente como la última línea del método `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

En el código siguiente se muestra el archivo *global. asax* completo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Si usa Internet Explorer 9 y no ve la línea de `BundleMobileConfig` anterior en el resaltado amarillo, haga clic en la imagen del [botón Vista compatibilidad](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![del botón Vista compatibilidad (desactivado)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Imagen del botón vista de compatibilidad (desactivado)") en IE para que el icono cambie de una ![imagen del contorno del botón vista de compatibilidad (desactivado)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Imagen del botón vista de compatibilidad (desactivado)") a una ![imagen de color sólido del botón vista de compatibilidad (en)](https://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "Imagen del botón vista de compatibilidad (en)"). También puede ver este tutorial en FireFox o Chrome.

Abra el archivo *MvcMobile\Views\Shared\\_Layout. Mobile. cshtml* y agregue el marcado siguiente directamente después de la llamada a `Html.Partial`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

A continuación se muestra el archivo *MvcMobile\Views\Shared\\_Layout. Mobile. cshtml* completo:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Compile la aplicación y, en el emulador de explorador móvil, vaya a la vista *AllTags* . Verá lo siguiente:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Para depurar el código específico de Mobile, [establezca la cadena de agente de usuario](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) para IE o Chrome en iPhone y, a continuación, use las herramientas de desarrollo de F-12. Si el explorador móvil no muestra los vínculos **Inicio**, **orador**, **etiqueta**y **fecha** como botones, es probable que las referencias a los archivos CSS y scripts de jQuery Mobile no sean correctas.

Además de los cambios de estilo, verá **Mostrar vista móvil** y un vínculo que le permite cambiar de la vista móvil a la vista de escritorio. Elija el vínculo **vista de escritorio** y se mostrará la vista de escritorio.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

La vista de escritorio no proporciona una manera de navegar directamente a la vista móvil. Lo corregirá ahora. Abra el archivo *Views\Shared\\_Layout. cshtml* . Justo debajo de la página `body` elemento, agregue el siguiente código, que representa el widget de selector de vistas:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Actualice la vista *AllTags* en el explorador móvil. Ahora puede desplazarse entre las vistas de escritorio y móviles.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Nota de depuración: puede Agregar el código siguiente al final de Views\Shared\\_ViewSwitcher. cshtml para ayudar a depurar vistas al usar un explorador la cadena de agente de usuario establecida en un dispositivo móvil.
>
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
>
> y agregue el encabezado siguiente al archivo *Views\Shared\\_Layout. cshtml* .
>
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]

Vaya a la página *AllTags* en un explorador de escritorio. El widget View-Switcher no se muestra en un explorador de escritorio porque solo se agrega a la página de diseño móvil. Más adelante en el tutorial, verá cómo puede Agregar el widget de selector de vistas a la vista de escritorio.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Mejora de la lista de oradores

En el explorador móvil, seleccione el vínculo **Oradores** . Dado que no hay ninguna vista móvil (*AllSpeakers. Mobile. cshtml*), la pantalla predeterminada de los altavoces (*AllSpeakers. cshtml*) se representa mediante la vista de diseño móvil ( *\_layout. Mobile. cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Puede deshabilitar globalmente una vista predeterminada (no móvil) de la representación dentro de un diseño móvil estableciendo `RequireConsistentDisplayMode` en `true` en las *vistas\\archivo _ViewStart. cshtml* , como se muestra a continuación:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Cuando `RequireConsistentDisplayMode` se establece en `true`, el diseño móvil (<em>\_layout. Mobile. cshtml</em>) se usa solo para vistas móviles. (Es decir, el archivo de vista tiene el formato <em>* * ViewName</em><em>. Mobile. cshtml</em>.) es posible que desee establecer `RequireConsistentDisplayMode` en `true` si el diseño móvil no funciona bien con las vistas que no son móviles. En la captura de pantalla siguiente se muestra cómo se representa la página <em>oradores</em> cuando `RequireConsistentDisplayMode` está establecida en `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Puede deshabilitar el modo de presentación coherente en una vista si establece `RequireConsistentDisplayMode` en `false` en el archivo de vista. El siguiente marcado en el archivo *Views\Home\AllSpeakers.cshtml* establece `RequireConsistentDisplayMode` en `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Creación de una vista de altavoces móviles

Como acabamos de ver, la vista *Oradores* es legible, pero los vínculos son pequeños y difíciles de pulsar en un dispositivo móvil. En esta sección, creará una vista de *oradores* específicos para dispositivos móviles que se parece a una aplicación móvil moderna; muestra vínculos grandes y fáciles de tocar y contiene un cuadro de búsqueda para encontrar rápidamente los oradores.

Copie *AllSpeakers. cshtml* en *AllSpeakers. Mobile. cshtml*. Abra el archivo *AllSpeakers. Mobile. cshtml* y quite el elemento de encabezado `<h2>`.

En la etiqueta `<ul>`, agregue el atributo `data-role` y establezca su valor en `listview`. Al igual que otros [atributos de`data-*`](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` hace que los elementos de la lista de gran tamaño sean más fáciles de puntear. Este es el aspecto del marcado completo:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Actualice el explorador móvil. La vista actualizada es la siguiente:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Aunque la vista móvil ha mejorado, es difícil navegar por la larga lista de oradores. Para solucionarlo, en la etiqueta `<ul>`, agregue el atributo `data-filter` y establézcalo en `true`. En el código siguiente se muestra el marcado de `ul`.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

En la imagen siguiente se muestra el cuadro de filtro de búsqueda de la parte superior de la página resultante del atributo `data-filter`.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

A medida que escribe cada letra en el cuadro de búsqueda, jQuery Mobile filtra la lista mostrada, tal como se muestra en la imagen siguiente.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Mejorar la lista de etiquetas

Al igual que la vista *oradores* predeterminada, la vista *etiquetas* es legible, pero los vínculos son pequeños y difíciles de pulsar en un dispositivo móvil. En esta sección, corregirá la vista de *etiquetas* de la misma forma que la vista *oradores* .

Quite la &quot;oculte el sufijo de&quot; al archivo *Views\Home\AllTags.Mobile.cshtml.Hide* para que el nombre sea *Views\Home\AllTags.Mobile.cshtml*. Abra el archivo cuyo nombre ha cambiado y quite el elemento `<h2>`.

Agregue los atributos `data-role` y `data-filter` a la etiqueta `<ul>`, como se muestra aquí:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

En la imagen siguiente se muestra el filtrado de páginas de etiquetas en la letra `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Mejorar la lista de fechas

Puede mejorar la vista *fechas* como la mejora de las vistas *oradores* y *etiquetas* para que sea más fácil de usar en un dispositivo móvil.

Copie el archivo *Views\Home\AllDates.cshtml* en *Views\Home\AllDates.Mobile.cshtml*. Abra el nuevo archivo y quite el elemento `<h2>`.

Agregue `data-role="listview"` a la etiqueta de `<ul>`, de la siguiente manera:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

En la imagen siguiente se muestra el aspecto de la página de **fechas** con el atributo de `data-role` en su lugar.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Reemplace el contenido del archivo *Views\Home\AllDates.Mobile.cshtml* por el código siguiente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Este código agrupa todas las sesiones por días. Crea un divisor de lista para cada día nuevo y muestra todas las sesiones para cada día en un divisor. Este es su aspecto cuando se ejecuta este código:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Mejora de la vista SessionsTable

En esta sección, creará una vista específica del móvil de las sesiones. Los cambios que hacemos serán más extensos que en otras vistas que hemos creado.

En el explorador móvil, puntee en el botón **altavoz** y escriba `Sc` en el cuadro de búsqueda.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Pulse el vínculo **Scott Hanselman** .

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Como puede ver, la visualización es difícil de leer en un explorador móvil. La columna Date es difícil de leer y la columna Tags está fuera de la vista. Para corregir esto, copie *Views\Home\SessionsTable.cshtml* en *Views\Home\SessionsTable.Mobile.cshtml*y, a continuación, reemplace el contenido del archivo por el código siguiente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

El código quita las columnas Room y Tags, y da formato al título, al orador y a la fecha verticalmente, de modo que toda esta información sea legible en un explorador móvil. La imagen siguiente refleja los cambios en el código.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Mejora de la vista SessionByCode

Por último, creará una vista específica para móviles de la vista *SessionByCode* . En el explorador móvil, puntee en el botón **altavoz** y escriba `Sc` en el cuadro de búsqueda.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Pulse el vínculo **Scott Hanselman** . Se muestran las sesiones de Scott Hanselman.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Elija la **información general del vínculo de la pila de Microsoft Web de Love** .

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

La vista de escritorio predeterminada está bien, pero puede mejorarla.

Copie *Views\Home\SessionByCode.cshtml* en *Views\Home\SessionByCode.Mobile.cshtml* y reemplace el contenido del archivo *Views\Home\SessionByCode.Mobile.cshtml* por el marcado siguiente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

El nuevo marcado utiliza el atributo `data-role` para mejorar el diseño de la vista.

Actualice el explorador móvil. La siguiente imagen refleja los cambios en el código que acaba de hacer:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup y revisión

En este tutorial se han incorporado las nuevas características móviles de ASP.NET MVC 4 Developer Preview. Las características móviles incluyen:

- La capacidad de invalidar el diseño, las vistas y las vistas parciales, tanto globalmente como para una vista individual.
- Control sobre el diseño y la aplicación de invalidación parcial mediante el `RequireConsistentDisplayMode` propiedad.
- Un widget de selector de vistas para vistas móviles que también se puede mostrar en las vistas de escritorio.
- Compatibilidad con exploradores específicos, como el explorador de iPhone.

## <a name="see-also"></a>Consulte también

- sitio [móvil de jQuery](http://jquerymobile.com) .
- [Información general sobre jQuery Mobile](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Prácticas recomendadas y recomendaciones de W3C para aplicaciones web móviles](http://www.w3.org/TR/mwabp/)
- [Recomendación de candidatos de W3C para consultas multimedia](http://www.w3.org/TR/css3-mediaqueries/)
