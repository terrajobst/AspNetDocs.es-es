---
uid: web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
title: Usar Inspector de página para Visual Studio 2012 en formularios Web Forms de ASP.NET | Microsoft Docs
author: rick-anderson
description: Inspector de página para Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 2ece0bf4-aae5-4ff4-8f62-28e0819d4f86
msc.legacyurl: /web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project
msc.type: authoredcontent
ms.openlocfilehash: c165bbea505b4cb8eae1312cdd587f4ed36541a0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464947"
---
# <a name="using-page-inspector-for-visual-studio-2012-in-aspnet-web-forms"></a>Usar el Inspector de página para Visual Studio 2012 en los formularios Web Forms de ASP.NET

por Tim Ammann

> Inspector de página para Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página resaltará al instante el origen y el código CSS del elemento. Puede examinar cualquier página de la aplicación, buscar rápidamente los orígenes de marcado representado y usar las herramientas del explorador directamente en el entorno de Visual Studio.
> 
> En este tutorial se muestra cómo habilitar Modo de inspección y, a continuación, ubicar y editar rápidamente reglas y texto de CSS en el proyecto Web. En el tutorial se usa un proyecto de aplicación de formularios Web Forms, pero también puede usar Inspector de página para proyectos de sitios web y aplicaciones [MVC](https://go.microsoft.com/?linkid=9802002) .
> 
> El tutorial consta de las siguientes secciones:
> 
> [Requisitos previos](#_1_prerequisites)
> 
> [Crear una aplicación Web](#_2_creating_a)
> 
> [Usar Inspector de página para ver la aplicación](#_3_using_page)
> 
> [Habilitar Modo de inspección](#_4_inspection_mode)
> 
> [Usar Inspector de página para realizar cambios en el marcado](#_5_using_page)
> 
> [Modo de inspección y la ventana HTML](#_6_inspection_mode)
> 
> [Vista previa de los cambios de CSS en la ventana estilos](#_7_previewing_css)
> 
> [Sincronización automática de CSS](#css_auto_sync)
> 
> [Usar el selector de colores de CSS](#css_color_picker)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Para obtener la versión más reciente de Inspector de página, use el [instalador de plataforma web](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar el SDK de Azure para .net 2,0.

Inspector de página se incluye con Microsoft Web Developer Tools. La versión más reciente es 1,3. Para comprobar qué versión tiene, ejecute Visual Studio y seleccione **acerca de Microsoft Visual Studio** en el menú **ayuda** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Crear una aplicación Web

En primer lugar, creará una aplicación web que utilizará Inspector de página con. En Visual Studio, elija **archivo** &gt; **nuevo proyecto**. A la izquierda, expanda **visualmente C#** , seleccione **Web**y, a continuación, seleccione **ASP.NET Web Forms Application**.

![Nueva aplicación de formularios Web Forms](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image1.png)

Haga clic en **Aceptar**.

La aplicación se abre en la vista **código fuente** .

![Nueva aplicación de formularios Web Forms en la vista Código fuente](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image2.png)

Ahora que tiene una aplicación con la que trabajar, puede usar Inspector de página para examinarla y modificarla.

<a id="_starting_page_inspector"></a><a id="_3_starting_page"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-view-the-application"></a>Usar Inspector de página para ver la aplicación

A continuación, verá la aplicación con Inspector de página. En **Explorador de soluciones**, haga clic con el botón derecho en el proyecto y elija **ver en inspector de página**.

![Ver en Inspector de páginas](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image3.png)

De forma predeterminada, cuando Inspector de página se inicia por primera vez, se acopla como una ventana estrecha en el lado izquierdo del entorno de Visual Studio. Déjelo acoplado en el lado izquierdo y establézcalo en un ancho que le resulte cómodo, o bien acoplelo en una de las áreas de herramientas de la parte superior, inferior o derecha:

![Inspector de página posiciones de acoplamiento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image4.png)

Si desacopla la ventana de Inspector de página, puede colocarla fuera de Visual Studio, o incluso en un segundo monitor si tiene una. Sin embargo, para presionar ALT + TAB entre Inspector de página y Visual Studio cuando la ventana de Inspector de página está desacoplada, vaya a **herramientas** &gt; **opciones** &gt; **entorno** &gt; **pestañas y ventanas**y, en **pestañas**, desactive la casilla de verificación las ventanas de herramientas **flotantes permanecen siempre en la parte superior de la ventana principal**:

![Desactive la casilla flotante ventanas de herramientas en ALT + TAB entre Visual Studio y la ventana de Inspector de página desacoplada.](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image5.png)

En el panel superior de la ventana de Inspector de página se muestra la página actual en una ventana del explorador. En el panel inferior se muestra la página en formato HTML a la izquierda y algunas de las pestañas de la derecha que permiten inspeccionar distintos aspectos de la página. El panel inferior es similar al [herramientas de desarrollo F12](https://msdn.microsoft.com/ie/aa740478) en Internet Explorer. (Sin embargo, a diferencia de las herramientas de desarrollo, puede usar Inspector de página directamente en Visual Studio).

![Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image6.png)

En este tutorial, usará el panel Explorador de Inspector de página y las pestañas **HTML** y **estilos** para ayudarle a navegar rápidamente y realizar cambios en la aplicación.

<a id="_4_inspection_mode"></a>
## <a name="enable-inspection-mode"></a>Habilitar Modo de inspección

A continuación, verá cómo funciona el Modo de inspección de Inspector de página. En la ventana de Inspector de página, haga clic en el botón **inspeccionar** .

![Inspeccionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image7.png)

Para ver el modo de inspección en acción, mueva el mouse sobre diferentes partes de la página en la Inspector de página ventana del explorador. Como lo hace, el puntero del mouse cambia a un signo más grande y se resalta el elemento debajo:

![Mantener el puntero sobre div. Content-wrapper](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image8.png)

Cuando mueva el puntero del mouse, tenga en cuenta que

- El contenido de la vista **código fuente** cambia para mostrar el marcado correspondiente al elemento seleccionado en la página. Se resalta el marcado pertinente. Si el origen está en otro archivo, ese archivo se abre en la vista de código fuente con el marcado correspondiente resaltado.

- El marcado que se muestra en la pestaña **HTML** en inspector de página también cambia para que se corresponda con el elemento seleccionado en la página. En la pestaña **HTML** , se describe el marcado pertinente.

- En la pestaña **estilos** se muestran las reglas de CSS relevantes para la selección actual.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Usar Inspector de página para realizar cambios en el marcado

Ahora verá cómo puede usar Inspector de página para buscar y realizar cambios en el marcado o en el texto cuya ubicación podría no ser obvia inmediatamente.

Coloque Inspector de página en Modo de inspección y, a continuación, desplácese hasta la parte inferior de la Página principal.

En cuanto se escribe el área de pie de página, Inspector de página abre el archivo de diseño *site. Master* en la vista **código fuente** en una pestaña temporal situada a la derecha de las demás pestañas y resalta la sección de la página maestra que ha seleccionado. Esto muestra cómo Inspector de página puede buscar y mostrar el contenido de una página que podría proceder realmente de un archivo diferente al que abrió originalmente.

![Resaltado de pie de página en Modo de inspección](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image9.png)

En la ventana del explorador Inspector de página, mueva el puntero del mouse sobre la línea con <a id="a"> </a>el aviso de propiedad intelectual.

En la página *site. Master* , se resalta la línea correspondiente.

![Línea de copyright de pie de página resaltada](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image10.png)

Agregue texto al final de la línea en el archivo *site. Master* .

&lt;p&gt;&amp;Copy; &lt;%: DateTime. Now. Year%&gt;-My ASP.NET Application Rocks!&lt;/p&gt;

Ahora, presione Ctrl + Alt + entrar o haga clic en la barra de actualización para ver los resultados en la ventana del explorador Inspector de página.

![¡ Mi aplicación ASP.NET Rock!](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image11.png)

Es posible que haya pensado que el pie de página se encontraba en la página *default. aspx* , pero que se encontraba en la página de diseño maestra y inspector de página lo encontró.

<a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modo de inspección y la ventana HTML

A continuación, tendrá una vista rápida de la ventana HTML y cómo asigna los elementos.

Coloque Inspector de página en Modo de inspección.

![Inspeccionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image12.png)

Haga clic en la parte superior de la página que dice "su logotipo aquí". Está examinando un elemento determinado con más detalle, de modo que la presentación en la ventana del explorador ya no cambia a medida que mueve el puntero del mouse.

Ahora, mueva el puntero del mouse a la ventana **HTML** . A medida que mueve el puntero del mouse, Inspector de página describe el elemento en la ventana **HTML** y resalta el elemento correspondiente en la ventana del explorador.

![Ventana HTML](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image13.png)

Como antes, Inspector de página abre el archivo *site. Master* en una pestaña temporal. Haga clic en la pestaña site. Master y el marcado correspondiente se resalta en el encabezado de &lt;&gt; sección:

![Marcado resaltado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image14.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Vista previa de los cambios de CSS en la ventana estilos

A continuación, verá cómo puede utilizar la ventana **estilos** de inspector de página para obtener una vista previa de los cambios en CSS.

Haga clic en el botón **inspeccionar** para colocar Inspector de página en modo de inspección.

En la ventana del explorador Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que aparezca la etiqueta **div. Content-wrapper** .

![Mantener el mouse sobre los elementos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image15.png)

Haga clic en la sección div. Content-wrapper una vez y, a continuación, mueva el puntero del mouse a la ventana **estilos** . En el selector de clases. destacado. Content-wrapper, desactive y active la casilla de la propiedad background-color.

![Borrar color de fondo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image16.png)

Observe cómo la vista previa de cambios se muestra al instante en la ventana del explorador Inspector de página.

Vuelva a activar la casilla de verificación y, a continuación, haga doble clic en el valor de la propiedad y cámbielo a `red`. El cambio se muestra inmediatamente:

![Color de fondo rojo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image17.png)

La ventana **estilos** facilita la prueba y la vista previa de los cambios de CSS antes de confirmar los cambios en la propia hoja de estilos.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronización automática de CSS

> [!NOTE]
> Esta característica requiere la versión 1,3 de Inspector de página.

La característica de sincronización automática de CSS le permite editar un archivo CSS directamente y ver los cambios inmediatamente en el explorador de Inspector de página.

Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección.

En el explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que aparezca la etiqueta **div. Content-wrapper** . Haga clic en una vez para seleccionar este elemento.

La ventana **estilos** muestra todas las reglas de CSS para este elemento. Desplácese hacia abajo para buscar el selector de clases. destacado. Content-wrapper. Haga clic en ". destacado. Content-wrapper". Inspector de página abre el archivo CSS que define este estilo (site. CSS) y resalta el estilo CSS correspondiente.

![Archivo CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image18.png)

Ahora, cambie el valor de `background-color` por "rojo". El cambio aparece inmediatamente en el explorador de Inspector de página.

![Explorador Inspector de página](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image19.png)

<a id="css_color_picker"></a>

## <a name="using-the-css-color-picker"></a>Usar el selector de colores de CSS

A continuación, aprenderá a usar Inspector de página para buscar y cambiar rápidamente el CSS del texto resaltado en la aplicación predeterminada. En este ejemplo, ha decidido que no le gusta el resaltado azul y desea cambiarlo a otro color.

Haga clic en el botón **inspeccionar** .

![Inspeccionar elemento](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image20.png)

En la ventana del explorador Inspector de página, mueva el puntero del mouse sobre el texto "vídeos, tutoriales y muestras" resaltado para que aparezca la etiqueta de "marca" de CSS.

![Mantener el puntero sobre el elemento Mark](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image21.png)

Haga clic en el texto para seleccionarlo. El selector de marcas de CSS correspondiente aparece en la parte inferior de la ventana **estilos** .

![Selector de marcas en la ventana estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image22.png)

Haga clic en el selector de marcas. Se abrirá el archivo *site. CSS* de la aplicación Web. Haga clic en la pestaña site. CSS y se resalta el CSS correspondiente para el selector:

![Selector de marcas en la hoja de estilos](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image23.png)

Seleccione y quite la línea con la propiedad background-color.

Ahora usará el nuevo selector de colores CSS de Visual Studio 2012 para elegir un nuevo color para la propiedad **marcar** fondo-color.

<a id="_using_the_visual"></a>

### <a name="using-the-visual-studio-2012-css-color-picker"></a>Usar el selector de colores CSS de Visual Studio 2012

El editor de CSS de Visual Studio 2012 tiene un selector de colores que facilita la elección y la inserción de colores. Tiene una barra de colores simple y un selector "emergente" que ofrece un mayor control.

El selector de colores incluye una paleta estándar de colores, admite los nombres de colores estándar, códigos hash, RGB, RGBA, HSL y HSLA, y mantiene una lista de los colores usados más recientemente en el documento.

En la línea donde se encuentra la propiedad background-color, escriba "BC" y presione la flecha abajo una vez.

Al escribir el primer carácter de cada palabra en una propiedad separada por guiones como "background-color", IntelliSense filtra la lista para que solo se muestren las propiedades que coinciden con:

![Valores filtrados de IntelliSense](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image24.png)

Ahora escriba un signo de dos puntos. Cuando lo haga, se insertará el nombre completo de la propiedad de color de fondo. Escriba **#** o **RGB (** y aparecerá la barra de selector de colores:

![Barra de selector de colores de CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image25.png)

Para ver cómo funciona la barra del selector de colores, haga clic en sus colores con el puntero del mouse o presione la tecla flecha abajo y, a continuación, use las teclas de flecha izquierda y derecha para recorrer los colores. Al visitar un color, se muestra una vista previa del valor correspondiente de la propiedad de color de fondo:

![vista previa del valor de propiedad de color de fondo](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image26.png)

En este momento, puede presionar entrar para seleccionar el valor y, a continuación, un punto y coma (;) para completar la entrada de CSS. Por ahora, vaya a la sección siguiente para que pueda ver cómo funciona la ventana emergente del selector de colores.

#### <a name="using-the-color-picker-pop-down"></a>Usar la ventana emergente del selector de colores

Si la barra de colores no tiene el color exacto que está buscando, puede usar la ventana emergente selector de colores.

Para abrirlo, haga clic en el botón de contenido adicional en el extremo derecho de la barra de colores o presione la flecha hacia abajo una o dos veces en el teclado.

![Cuadro emergente del selector de colores de CSS](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image27.png)

Haga clic en un color de la barra vertical de la derecha. Esto muestra un degradado para ese color en la ventana principal. Elija un color directamente en la barra vertical presionando entrar o haga clic en cualquier punto de la ventana principal para elegir con mayor precisión.

Si hay un color en la pantalla del equipo que desea utilizar (no tiene que estar dentro de la interfaz de usuario de Visual Studio), puede capturar su valor mediante la herramienta Cuentagotas de la parte inferior derecha.

También puede cambiar la opacidad de un color moviendo el control deslizante situado en la parte inferior del selector de colores. Al hacerlo, se cambian los valores de color a los valores RGBA porque el formato RGBA puede representar la opacidad.

Después de elegir un color, presione entrar y, a continuación, escriba un punto y coma para completar la entrada de color de fondo en el archivo *site. CSS* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Barra de actualización Inspector de página

Inspector de página detecta inmediatamente el cambio en el archivo *site. CSS* (o en cualquier archivo de la aplicación) y muestra una alerta en una barra de actualización.

![Barra de actualización](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image28.png)

Para guardar todos los archivos y actualizar el explorador de Inspector de página, presione Ctrl + Alt + entrar o haga clic en la barra de actualización. El cambio en el color de resaltado aparece en el explorador:

![Color de resaltado cambiado](using-page-inspector-in-a-visual-studio-11-beta-web-forms-project/_static/image29.png)

<a id="_using_page_inspector_1"></a>Tenga en cuenta que se ha actualizado de forma cómoda el Inspector de página explorador directamente desde el entorno de Visual Studio. El uso de Inspector de página en lugar de un explorador externo le permite permanecer en el editor al desarrollar sus aplicaciones Web.

## <a name="see-also"></a>Consulte también

[Presentación de inspector de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector) (vídeo de Channel 9)

[Mensajes de error de inspector de página](https://go.microsoft.com/?linkid=9813062) (MSDN)
