---
uid: mvc/overview/views/using-page-inspector-in-aspnet-mvc
title: Usar Inspector de página en MVC de ASP.NET | Microsoft Docs
author: rick-anderson
description: Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página...
ms.author: riande
ms.date: 08/15/2012
ms.assetid: c7e4e1ab-4932-4614-9f53-aaf7c706d498
msc.legacyurl: /mvc/overview/views/using-page-inspector-in-aspnet-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5da3e142c52a770f59222c21d9f9a53cbbdbf498
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78432457"
---
# <a name="using-page-inspector-in-aspnet-mvc"></a>Usar el Inspector de página en ASP.NET MVC

por Tim Ammann

> Inspector de página en Visual Studio 2012 es una herramienta de desarrollo web con un explorador integrado. Seleccione cualquier elemento en el explorador integrado y Inspector de página resaltará al instante el origen y el código CSS del elemento. Puede examinar cualquier vista de MVC, buscar rápidamente los orígenes de marcado representado y usar las herramientas del explorador directamente en el entorno de Visual Studio.
> 
> [Ver el vídeo](../../videos/mvc-4/using-page-inspector-in-aspnet-mvc.md)
> 
> En este tutorial se muestra cómo habilitar Modo de inspección y, a continuación, ubicar y editar rápidamente el marcado y CSS en el proyecto Web. En el tutorial se usa un proyecto de MVC, pero también puede usar Inspector de página para [formularios Web Forms](https://go.microsoft.com/?linkid=9802001) y otras aplicaciones ASP.net.
> 
> El tutorial consta de las siguientes secciones:
> 
> - [Requisitos previos](#_1_prerequisites)
> - [Crear una aplicación Web](#_2_creating_a)
> - [Usar Inspector de página para ir a una vista](#_3_using_page)
> - [Habilitar Modo de inspección](#_4_inspection_mode)
> - [Usar Inspector de página para realizar cambios en el marcado](#_5_using_page)
> - [Modo de inspección y la ventana HTML](#_6_inspection_mode)
> - [Vista previa de los cambios de CSS en la ventana estilos](#_7_previewing_css)
> - [Sincronización automática de CSS](#css_auto_sync)
> - [Usar el selector de colores de CSS](#css_color_picker)
> - [Asignar elementos de página dinámicos a JavaScript](#map_dynamic_elements)

<a id="_prerequisites"></a><a id="_1_prerequisites"></a>

## <a name="prerequisites"></a>Requisitos previos

- [Visual Studio 2012](https://www.microsoft.com/visualstudio/11) o [Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/11/downloads#express-web).

> [!NOTE]
> Para obtener la versión más reciente de Inspector de página, use el [instalador de plataforma web](https://go.microsoft.com/fwlink/?LinkId=255386) para instalar el SDK de Windows Azure para .net 2,0.

Inspector de página se incluye con Microsoft Web Developer Tools. La versión más reciente es 1,3. Para comprobar qué versión tiene, ejecute Visual Studio y seleccione **acerca de Microsoft Visual Studio** en el menú **ayuda** .

<a id="_creating_a_web"></a><a id="_2_creating_a"></a>

## <a name="create-a-web-application"></a>Crear una aplicación Web

En primer lugar, cree una aplicación web que utilizará Inspector de página con. En Visual Studio, elija **archivo** &gt; **nuevo proyecto**. A la izquierda, expanda **C#visualmente**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.net MVC4**.

![Nueva aplicación ASP.NET MVC](using-page-inspector-in-aspnet-mvc/_static/image2.png)

Haga clic en **Aceptar**.

En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione **aplicación de Internet**. Deje **Razor** como el motor de vista predeterminado.

![Nuevo proyecto de ASP.NET MVC: aplicación de Internet](using-page-inspector-in-aspnet-mvc/_static/image4.png)

La aplicación se abre en la vista **código fuente** .

![Nueva aplicación ASP.NET MVC en la vista Código fuente](using-page-inspector-in-aspnet-mvc/_static/image6.png)

Ahora que tiene una aplicación con la que trabajar, puede usar Inspector de página para examinarla y modificarla.

<a id="_starting_page_inspector"></a><a id="_3_using_page"></a>

## <a name="use-page-inspector-to-browse-to-a-view"></a>Usar Inspector de página para ir a una vista

En Visual Studio 2012, puede hacer clic con el botón derecho en cualquier vista del proyecto, seleccionar **ver en inspector de página**y inspector de página desfigurará la ruta y mostrará la página.

En **Explorador de soluciones**, expanda la carpeta **vistas** y, a continuación, la carpeta **Inicio** . Haga clic con el botón derecho en el archivo index. cshtml y elija **ver en inspector de página**.

![Ver index. cshtml en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image8.png)

De forma predeterminada, Inspector de página se acopla como una ventana en el lado izquierdo del entorno de Visual Studio. Si lo prefiere, puede acoplarlo en otro lugar o desacoplar la ventana. Consulte [Cómo: organizar y acoplar ventanas](https://msdn.microsoft.com/library/z4y0hsax.aspx).

En el panel superior de la ventana de Inspector de página se muestra la página actual en una ventana del explorador. En el panel inferior se muestra la página en formato HTML, junto con algunas pestañas que permiten inspeccionar distintos aspectos de la página. El panel inferior es similar al [herramientas de desarrollo F12](https://msdn.microsoft.com/ie/aa740478) en Internet Explorer.

![Aplicación ASP.NET MVC en Inspector de página](using-page-inspector-in-aspnet-mvc/_static/image10.png)

En este tutorial, usará las pestañas **HTML** y **styles** para navegar rápidamente y realizar cambios en la aplicación.

<a id="_examining_(&quot;decomposing&quot;)_the"></a><a id="_inspection_mode_and"></a><a id="_4_inspection_mode"></a>

## <a name="enableinspection-mode"></a>Modo EnableInspection

Para colocar Inspector de página en Modo de inspección, haga clic en el botón **inspeccionar** . En Modo de inspección, cuando se mantiene presionado el puntero del mouse sobre cualquier parte de la página representada, se resalta el código fuente correspondiente.

![Alternar Modo de inspección](using-page-inspector-in-aspnet-mvc/_static/image12.png)

Mueva el mouse sobre distintas partes de la página dentro de Inspector de página. Como lo hace, el puntero del mouse cambia a un signo más grande y se resalta el elemento debajo:

![Mantener el puntero sobre div. Content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image14.png)

A medida que mueve el puntero del mouse, Visual Studio resalta el sintaxis Razor correspondiente en el archivo de código fuente. Si el elemento HTML procede de otro archivo de código fuente, Visual Studio abre automáticamente el archivo.

En Inspector de página, la pestaña **HTML** muestra el código HTML que se generó a partir del sintaxis Razor. A medida que mueve el puntero del mouse, se resaltan los elementos HTML. En la pestaña **estilos** se muestran las reglas de CSS para el elemento.

<a id="_5_using_page"></a>

## <a name="use-page-inspector-to-make-changes-to-markup"></a>Usar Inspector de página para realizar cambios en el marcado

Inspector de página permite buscar el marcado cuya ubicación podría no ser obvia. Después, puede modificar el marcado y ver los cambios resultantes.

Para verlo, haga clic en **inspeccionar** y, a continuación, desplácese hasta la parte inferior de la página en la ventana de inspector de página.

Al colocar el puntero del mouse en el área de pie de página, Inspector de página abre el archivo \_layout. cshtml y resalta la sección de la página de diseño que ha seleccionado. Como puede ver, el pie de página se define en el archivo de diseño y no en la propia vista.

![Pie de página](using-page-inspector-in-aspnet-mvc/_static/image16.png)

Ahora, mueva el puntero del mouse sobre la línea con <a id="a"> </a>el aviso de propiedad intelectual. En la página \_layout. cshtml, se resalta la línea correspondiente.

![Línea de copyright de pie de página resaltada](using-page-inspector-in-aspnet-mvc/_static/image18.png)

Agregue texto al final de la línea en el archivo \_layout. cshtml.

&lt;p&gt;&amp;Copy; @DateTime.Now.Year de la aplicación ASP.NET MVC Rocks!&lt;/p&gt;

Ahora, presione Ctrl + Alt + entrar o haga clic en la barra de actualización para ver los resultados en la ventana del explorador Inspector de página.

![¡ Mi aplicación ASP.NET Rock!](using-page-inspector-in-aspnet-mvc/_static/image20.png)

Es posible que haya pensado que el pie de página está definido en index. cshtml, pero que está en el \_layout. cshtml y Inspector de página lo ha encontrado.

<a id="_inspection_mode_and_1"></a><a id="_6_inspection_mode"></a>

## <a name="inspection-mode-and-the-html-window"></a>Modo de inspección y la ventana HTML

A continuación, tendrá una vista rápida de la ventana HTML y cómo asigna los elementos.

Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección.

Haga clic en la parte superior de la página que dice "su logotipo aquí". Está examinando un elemento determinado con más detalle, de modo que la presentación en la ventana del explorador ya no cambia a medida que mueve el puntero del mouse.

Ahora, mueva el puntero del mouse a la ventana **HTML** . A medida que mueve el puntero del mouse, Inspector de página describe el elemento en la ventana **HTML** y resalta el elemento correspondiente en la ventana del explorador.

![Ventana HTML](using-page-inspector-in-aspnet-mvc/_static/image22.png)

Como antes, Inspector de página abre el archivo \_layout. cshtml en una pestaña temporal. Haga clic en la ficha temporal \_layout. cshtml y el marcado correspondiente se resaltará en el encabezado &lt;&gt; sección:

![Marcado resaltado](using-page-inspector-in-aspnet-mvc/_static/image24.png)

<a id="_using_page_inspector"></a><a id="_7_previewing_css"></a>

## <a name="preview-css-changes-in-the-styles-window"></a>Vista previa de los cambios de CSS en la ventana estilos

A continuación, usará la ventana **estilos** de inspector de página para obtener una vista previa de los cambios en CSS.

Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección.

En la ventana del explorador Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que aparezca la etiqueta **div. Content-wrapper** .

![Mantener el puntero sobre div. Content-wrapper](using-page-inspector-in-aspnet-mvc/_static/image26.png)

Haga clic en la sección div. Content-wrapper una vez y, a continuación, mueva el puntero del mouse a la ventana **estilos** . La ventana **estilos** muestra todas las reglas de CSS para este elemento. Desplácese hacia abajo para buscar el selector de clases. destacado. Content-wrapper. Ahora desactive la casilla de la propiedad background-color.

![Borrar color de fondo](using-page-inspector-in-aspnet-mvc/_static/image28.png)

Observe cómo la vista previa de cambios se muestra al instante en la ventana del explorador Inspector de página.

Vuelva a activar la casilla de verificación y, a continuación, haga doble clic en el valor de la propiedad y cámbielo a rojo. El cambio se muestra inmediatamente:

![Color de fondo rojo](using-page-inspector-in-aspnet-mvc/_static/image30.png)

La ventana **estilos** facilita la prueba y la vista previa de los cambios de CSS antes de confirmar los cambios en la propia hoja de estilos.

<a id="css_auto_sync"></a>
## <a name="css-auto-sync"></a>Sincronización automática de CSS

> [!NOTE]
> Esta característica requiere la versión 1,3 de Inspector de página.

La característica de sincronización automática de CSS le permite editar un archivo CSS directamente y ver los cambios inmediatamente en el explorador de Inspector de página.

Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección.

En el explorador de Inspector de página, mueva el puntero del mouse sobre la sección "Página principal" hasta que aparezca la etiqueta **div. Content-wrapper** . Haga clic en una vez para seleccionar este elemento.

La ventana **estilos** muestra todas las reglas de CSS para este elemento. Desplácese hacia abajo para buscar el selector de clases. destacado. Content-wrapper. Haga clic en ". destacado. Content-wrapper". Inspector de página abre el archivo CSS que define este estilo (site. CSS) y resalta el estilo CSS correspondiente.

![](using-page-inspector-in-aspnet-mvc/_static/image32.png)

Ahora, cambie el valor de `background-color` por "rojo". El cambio aparece inmediatamente en el explorador de Inspector de página.

![](using-page-inspector-in-aspnet-mvc/_static/image34.png)

<a id="css_color_picker"></a>
## <a name="using-the-css-color-picker"></a>Usar el selector de colores de CSS

El editor de CSS de Visual Studio 2012 tiene un selector de colores que facilita la elección y la inserción de colores. El selector de colores incluye una paleta estándar de colores, admite los nombres de colores estándar, códigos hash, RGB, RGBA, HSL y HSLA, y mantiene una lista de los colores usados más recientemente en el documento.

En la sección anterior, cambió el valor de la propiedad `background-color`. Para invocar el selector de colores, coloque el punto de inserción después del nombre y el tipo de la propiedad **#** o **RGB (** .

![Barra de selector de colores de CSS](using-page-inspector-in-aspnet-mvc/_static/image36.png)

Haga clic en un color para seleccionarlo o presione la tecla flecha abajo y, a continuación, use las teclas de flecha izquierda y derecha para atravesar los colores. Al visitar un color, se muestra una vista previa del valor hexadecimal correspondiente:

![vista previa del valor de propiedad de color de fondo](using-page-inspector-in-aspnet-mvc/_static/image38.png)

Si la barra de colores no tiene el color exacto que desea, puede usar la ventana emergente selector de colores. Para abrirlo, haga clic en el botón de contenido adicional en el extremo derecho de la barra de colores o presione la flecha hacia abajo una o dos veces en el teclado.

![Cuadro emergente del selector de colores de CSS](using-page-inspector-in-aspnet-mvc/_static/image40.png)

Haga clic en un color de la barra vertical de la derecha. Esto muestra un degradado para ese color en la ventana principal. Elija un color directamente en la barra vertical presionando entrar o haga clic en cualquier punto de la ventana principal para elegir con mayor precisión.

Si hay un color en la pantalla del equipo que desea utilizar (no tiene que estar dentro de la interfaz de usuario de Visual Studio), puede capturar su valor mediante la herramienta Cuentagotas de la parte inferior derecha.

También puede cambiar la opacidad de un color moviendo el control deslizante situado en la parte inferior del selector de colores. Al hacerlo, se cambian los valores de color a los valores RGBA, ya que el formato RGBA puede representar la opacidad.

Después de elegir un color, presione entrar y, a continuación, escriba un punto y coma para completar la entrada de color de fondo en el archivo *site. CSS* .

<a id="_the_update_bar"></a>

### <a name="the-page-inspector-update-bar"></a>Barra de actualización Inspector de página

Inspector de página detecta inmediatamente el cambio en el archivo *site. CSS* y muestra una alerta en una barra de actualización.

![Barra de actualización](using-page-inspector-in-aspnet-mvc/_static/image42.png)

Para guardar todos los archivos y actualizar el explorador de Inspector de página, presione Ctrl + Alt + entrar o haga clic en la barra de actualización. El cambio en el color de resaltado aparece en el explorador.

<a id="map_dynamic_elements"></a>
## <a name="mapping-dynamic-page-elements-to-javascript"></a>Asignar elementos de página dinámicos a JavaScript

En las aplicaciones web modernas, los elementos de la página se suelen generar dinámicamente con JavaScript. Esto significa que no hay ningún marcado estático (HTML o Razor) que se corresponda con estos elementos de página.

Con la versión 1,3, Inspector de página ahora pueden asignar elementos que se agregaron dinámicamente a la página de nuevo al código JavaScript correspondiente. Para demostrar esta característica, usaremos la [plantilla de aplicación de una sola página (Spa)](../../../single-page-application/overview/introduction/knockoutjs-template.md).

> [!NOTE]
> La plantilla SPA requiere la actualización [ASP.NET and Web Tools 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650) .

En Visual Studio, elija **archivo** &gt; **nuevo proyecto**. A la izquierda, expanda **C#visualmente**, seleccione **Web**y, a continuación, seleccione **aplicación Web de ASP.net MVC4**. Haga clic en **Aceptar**.

En el cuadro de diálogo **nuevo proyecto de ASP.NET MVC 4** , seleccione **aplicación de una sola página**.

En Explorador de soluciones, expanda la carpeta **vistas** y, a continuación, la carpeta **Inicio** . Haga clic con el botón derecho en el archivo index. cshtml y elija **ver en inspector de página**.

Lo primero que se muestra en Inspector de página explorador es una página de inicio de sesión. Haga clic en "registrarse" y cree un nombre de usuario y una contraseña. Una vez que se suscribe, la aplicación inicia sesión y crea una lista de tareas pendientes con algunos elementos de ejemplo.

Haga clic en **inspeccionar** para colocar Inspector de página en modo de inspección. En el explorador de Inspector de página, haga clic en uno de los elementos pendientes. Observe que en lugar de resaltarse en azul, el elemento se resalta en color naranja, con "JS" junto al nombre del elemento. Esto indica que el elemento se ha creado dinámicamente a través de un script.

![](using-page-inspector-in-aspnet-mvc/_static/image44.png)

Además, aparece un subrayado naranja en la pestaña **pila de llamadas** . Esto indica que el panel **pila de llamadas** tiene más información sobre el elemento.

Haga clic en la pestaña **pila de llamadas** . El panel **pila de llamadas** muestra la pila de llamadas de la llamada de JavaScript que creó el elemento. Las llamadas a bibliotecas externas como jQuery están contraídas, por lo que puede ver fácilmente las llamadas al script de la aplicación.

![](using-page-inspector-in-aspnet-mvc/_static/image46.png)

Para ver la pila completa, incluidas las llamadas a bibliotecas externas, puede expandir los nodos con la etiqueta "bibliotecas externas":

![](using-page-inspector-in-aspnet-mvc/_static/image48.png)

Si hace clic en un elemento de la pila de llamadas, Visual Studio abre el archivo de código y resalta el script correspondiente.

![](using-page-inspector-in-aspnet-mvc/_static/image50.png)

## <a name="see-also"></a>Consulte también

[Introducción a ASP.NET MVC 4 con Visual Studio](../older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md) (sitio web de ASP.net)

[Presentación de inspector de página](https://channel9.msdn.com/posts/visual-studio-vnext-introducing-page-inspector/) (vídeo de Channel 9)

[Mensajes de error de inspector de página](https://go.microsoft.com/?linkid=9813062) (MSDN)
