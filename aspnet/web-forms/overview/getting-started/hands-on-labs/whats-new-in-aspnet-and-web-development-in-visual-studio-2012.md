---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
title: Novedades de ASP.NET y desarrollo web en Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: La nueva versión de Visual Studio incluye una serie de mejoras centradas en mejorar la experiencia y el rendimiento al trabajar con tecnologías Web...
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 6d40d276-1642-4a77-b6c9-02ac914f6805
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-aspnet-and-web-development-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: 80c77ec65ed86b06e417d3f6ba608e404c46768b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78422101"
---
# <a name="whats-new-in-aspnet-and-web-development-in-visual-studio-2012"></a>Novedades de Desarrollo de ASP.NET y web en Visual Studio 2012

por [equipo de grupos de web](https://twitter.com/webcamps)

> La nueva versión de Visual Studio incluye una serie de mejoras centradas en mejorar la experiencia y el rendimiento al trabajar con tecnologías Web. Los editores de Visual Studio para CSS, JavaScript y HTML se han renovado por completo para incluir muchas de las ayudas de código a petición más, como IntelliSense y la sangría automática. En cuanto al rendimiento, la agrupación y la minificación ahora se integran como características integradas para reducir fácilmente el tiempo de carga de la página.
> 
> Visual Studio le permite trabajar con las tecnologías de sitios web más recientes. Puede usar fragmentos de código CSS3 entre exploradores para asegurarse de que el sitio funciona independientemente de la plataforma cliente, al tiempo que se aprovechan las nuevas características y elementos de HTML5.
> 
> Escribir y generar perfiles de código JavaScript debe ser más fácil con esta versión de Visual Studio. Las listas de IntelliSense, la documentación XML integrada y las características de navegación ahora están disponibles para el código JavaScript. Ahora tiene el catálogo de JavaScript a su alcance. Además, puede comprobar la compatibilidad de ECMAScript5 con los scripts y detectar errores de sintaxis en una fase temprana.
> 
> Por último, pero no menos, esta versión de Visual Studio implementa la agrupación integrada y minificación. Los archivos de script y las hojas de estilos se empaquetarán y comprimirán para que el sitio se ejecute más rápido.
> 
> Este laboratorio le guía a través de las mejoras y las nuevas características descritas anteriormente mediante la aplicación de cambios menores en una aplicación Web de ejemplo que se proporciona en la carpeta de origen.
> 
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Usar las nuevas características y mejoras en el editor de CSS
- Usar las nuevas características y mejoras del editor HTML
- Usar las nuevas características y mejoras en el editor de JavaScript
- Configuración y uso de la agrupación y la minificación

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

- [Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).
- [Windows PowerShell](https://support.microsoft.com/kb/968930/) (para scripts de instalación, ya instalado en Windows 8 y windows Server 2008 R2)
- [Internet Explorer 10](https://windows.microsoft.com/internet-explorer/products/ie/home) o un explorador compatible con HTML5

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los siguientes ejercicios:

1. [Ejercicio 1: novedades del editor CSS](#Exercise1)
2. [Ejercicio 2: novedades del editor HTML](#Exercise2)
3. [Ejercicio 3: novedades en el editor de JavaScript](#Exercise3)
4. [Ejercicio 4: agrupación y Minificación](#Exercise4)

Tiempo estimado para completar este laboratorio: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Whats_New_in_the_CSS_Editor"></a>
### <a name="exercise-1-whats-new-in-the-css-editor"></a>Ejercicio 1: novedades del editor CSS

Los desarrolladores web deben estar familiarizados con muchas de las dificultades relacionadas con la edición de CSS. Uno de los mayores problemas de los estilos CSS es la compatibilidad entre exploradores. Suele ocurrir que, después de aplicar estilos al sitio, observa que tiene un aspecto diferente si lo abre en otro explorador o dispositivo. Por lo tanto, puede dedicar una gran cantidad de tiempo a corregir esos problemas visuales para saber que, cuando finalmente hace que funcione en un explorador, se rompe en los demás.

Visual Studio ahora incluye características que ayudan a los desarrolladores a obtener acceso, trabajar y organizar hojas de estilos CSS de manera eficaz. A lo largo de este ejercicio, se cumplirán las nuevas características de una organización y edición eficaces, así como de los fragmentos de código CSS3 para la compatibilidad entre exploradores.

<a id="Ex1Task1"></a>

<a id="Task_1_-_New_Editor_Features"></a>
#### <a name="task-1---new-editor-features"></a>Tarea 1: nuevas características del editor

En esta tarea, se detectarán las nuevas características del editor de CSS. Este nuevo editor le ayudará a aumentar su productividad aprovechando las ventajas de la nueva sangría inteligente, los comentarios de código mejorados y la lista mejorada de IntelliSense.

1. Inicie **Visual Studio** y abra la solución **WhatsNewASPNET. sln** que se encuentra en la carpeta **Source\WhatsNewASPNET** de este laboratorio.
2. En Explorador de soluciones, abra el archivo **site. CSS** que se encuentra en la carpeta **styles** . Asegúrese de que las herramientas del **Editor de texto** estén visibles en la barra de herramientas. Para ello, seleccione la opción de menú **ver** | **barras de herramientas** y compruebe las opciones del **Editor de texto** . Observará que, desde esta nueva versión, el botón de **Comentario** (![](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image1.png) de botón de comentario) y el botón sin **comentario** (![](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image2.png) sin comentario) también están habilitados para el editor de CSS.

    ![Habilitar herramientas de editor y CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image3.png "Habilitar herramientas de editor y CSS")

    *Habilitar herramientas de editor y CSS*
3. Desplácese por el código y seleccione cualquier definición de clase CSS. Haga clic en el botón **Comentario (![botón de comentario](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image4.png)** ) para comentar las líneas seleccionadas. A continuación, haga clic en el botón **quitar comentario** (![sin comentario-Button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image5.png)) para deshacer los cambios.
4. Haga clic en los botones **contraer** (![contraer](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image6.png)) y **expandir** (![expandir](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image7.png)) ubicados en el margen izquierdo del texto. Observe que ahora puede ocultar los estilos que no utiliza para tener una vista más clara.

    ![Contraer clases CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image8.png "Contraer clases CSS")

    *Contraer clases CSS*
5. Asegúrese de que la característica sangría inteligente está habilitada. Seleccione la opción de menú **herramientas** | **Opciones** y, a continuación, seleccione la página **Editor de texto** | **CSS** | **formato** en el panel izquierdo de la pantalla. Compruebe la opción de **sangría jerárquica** .

    ![Habilitación de la sangría jerárquica](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image9.png "Habilitación de la sangría jerárquica")

    *Habilitación de la sangría jerárquica*
6. Busque la definición de clase principal (. Main) y anexe un estilo a los elementos div. Observará que el código se alinea automáticamente y ayuda a los usuarios a encontrar las clases primarias de un vistazo.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample1.css)]

    ![Alineación jerárquica en CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image10.png "Alineación jerárquica en CSS")

    *Alineación jerárquica en CSS*
7. Dentro de **. Main** (clase), busque el cursor al final de **border: 0px;** y presione **entrar** para mostrar la lista de IntelliSense. Comience a escribir **Top** y observe cómo se filtra la lista a medida que escribe. La lista mostrará los elementos que contienen la **parte superior** en cualquier parte de la palabra (en versiones anteriores de Visual Studio, la lista se filtra por los elementos que *comienzan* por el término).

    ![Mejoras de IntelliSense en CSS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image11.png "Mejoras de IntelliSense en CSS")

    *Mejoras de IntelliSense en CSS*

<a id="Ex1Task2"></a>

<a id="Task_2_-_The_Color_Picker"></a>
#### <a name="task-2---the-color-picker"></a>Tarea 2: selector de colores

En esta tarea, detectará el nuevo selector de color de CSS integrado en Visual Studio IntelliSense.

1. En **site. CSS,** busque la definición de clase de encabezado (. Header) y coloque el cursor junto a atributo **background-color** , entre las &quot;:&quot; y &quot;#&quot; caracteres en esa línea de código **.**

    ![Buscar el cursor](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image12.png "Buscar el cursor")

    *Buscar el cursor*
2. Elimine los **dos puntos** (:) y vuelva a escribirlo para mostrar el selector de colores. Tenga en cuenta que los primeros colores que verá son los colores más frecuentes de su sitio. Si hace clic en el color blanco, su código de color HTML (#fff) reemplazará el código de color actual de la hoja de estilos.

    ![Selector de colores](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image13.png "Selector de colores")

    *Selector de colores*
3. Presione el botón **expandir** (![com](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image14.png)) en el selector de colores para mostrar el degradado de color y, a continuación, arrastre el cursor de degradado para seleccionar un color diferente. Después, haga clic en el botón **cuentagotas** y seleccione cualquier color de la pantalla. Observe que el valor de color de fondo cambia dinámicamente mientras se mueve el cursor.

    ![Degradado del selector de colores](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image15.png "Degradado del selector de colores")

    *Degradado del selector de colores*
4. En el control deslizante **opacidad** , mueva el selector al centro de la barra para reducir la opacidad. Observe que el valor de color de fondo ahora cambia su escala a RGBA.

    ![Opacidad del selector de colores](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image16.png "Opacidad del selector de colores")

    *Opacidad del selector de colores*

    > [!NOTE]
    > La definición de color RGBA (rojo, verde, azul, alfa) en CSS3 permite definir el valor de opacidad de color para un único elemento. A diferencia **de la opacidad,** un atributo CSS similar **-** colores RGBA también son compatibles con los exploradores más recientes.

<a id="Ex1Task3"></a>

<a id="Task_3_-_CSS_Compatible_Code_Snippets"></a>
#### <a name="task-3---css-compatible-code-snippets"></a>Tarea 3: fragmentos de código compatibles con CSS

En esta tarea, aprenderá a usar fragmentos de código CSS3 compatibles con el explorador para implementar algunas características en el sitio Web.

1. En el archivo **site. CSS** , busque la definición de clase CSS de **encabezado** (. Header) y coloque el cursor debajo de la **/\*radio de borde\*/** marcador de posición para agregar un nuevo fragmento de código. Presione **entrar** para mostrar la lista de IntelliSense y escriba **RADIUS** para filtrar la lista. Seleccione la opción **border-radius** de la lista con un doble clic y, a continuación, presione la tecla **Tab** para insertar el fragmento de código. A continuación, escriba un tamaño de radio en píxeles y presione **entrar**. Por ejemplo, escriba **15px**.

    Los atributos CSS3 agregados por el fragmento de código representarán los bordes redondeados en la mayoría de los exploradores de cumplimiento de HTML5, incluidos los exploradores basados en Mozilla y WebKit.

    ![Usar un fragmento de código de radio de borde](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image17.png "Usar un fragmento de código de radio de borde")

    *Usar un fragmento de código de radio de borde*
2. Aplique los mismos fragmentos de código de **borde** en el estilo de página (. Page).

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample2.css)]
3. Presione **F5** para ejecutar la solución. Observe que cada página tiene ahora bordes redondeados.

    ![Esquinas redondeadas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image18.png "Esquinas redondeadas")

    *Esquinas redondeadas*
4. Cierre el explorador y vuelva a Visual Studio.
5. Abra el archivo **custom. CSS** que se encuentra en la carpeta **styles** y coloque el cursor dentro de la definición de clase **div. images UL Li IMG** .
6. Presione Entrar para mostrar la lista de IntelliSense, escriba **Box-Shadow** y presione la tecla **Tab** dos veces para insertar el fragmento de código de sombra predeterminado dentro de la definición de clase. Establezca los valores de sombra en **10px 10px 5px #888**. A continuación, escriba **border-radius** e inserte el fragmento de código. Escriba **15px** para establecer el tamaño de RADIUS y presione **entrar**.

    ![Esquinas redondeadas con sombra](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image19.png "Esquinas redondeadas con sombra")

    *Esquinas redondeadas con sombra*

    > [!NOTE]
    > En este momento, el atributo Shadow se inserta con el prefijo correspondiente (moz, WebKit, o) para admitir los exploradores Mozilla y WebKit (Chrome, Safari, Konkeror).
7. Cree una nueva clase **div. images UL Li IMG: Mantenga el puntero** sobre la definición de clase **div. images UL Li IMG** y coloque el cursor dentro de los corchetes **.**

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample3.css)]
8. Escriba **Transform** y presione la tecla **Tab** dos veces para insertar el fragmento de código de transformación. A continuación, escriba **Rotate (-15deg)** para cambiar el valor de ángulo de giro cuando se mantenga el mouse en las imágenes.

    CSS

    [!code-css[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample4.css)]
9. Presione **F5** para ejecutar la solución y vaya a la página CSS3. Tenga en cuenta que las imágenes tienen esquinas redondeadas y sombras de cuadros. Mantenga el mouse sobre las imágenes y vea cómo girarlas.

    ![Transformar fragmentos rotando una imagen](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image20.png "Transformar fragmentos rotando una imagen")

    *Transformar fragmentos rotando una imagen*

    > [!NOTE]
    > Si usa Internet Explorer 10 y no puede ver las sombras, asegúrese de que el modo de documento está establecido en estándares de IE10. Presione **F12** para abrir las herramientas de desarrollo de Internet Explorer y haga clic en **modo de documento** para cambiar a los estándares de IE10.

    ![Acerca de-US](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image21.png)

<a id="Exercise2"></a>

<a id="Exercise_2_Whats_New_in_the_HTML_Editor"></a>
### <a name="exercise-2-whats-new-in-the-html-editor"></a>Ejercicio 2: novedades del editor HTML

Visual Studio tiene un editor HTML mejorado. Algunas de las mejoras incluidas en esta versión son la sangría inteligente en documentos HTML, fragmentos de código HTML5, coincidencia de etiquetas de inicio y finalización HTML y validación HTML. En este ejercicio, verá cómo estos cambios mejoran su fluidez al trabajar en el marcado del sitio Web.

Al igual que el editor de CSS, también se ha mejorado el editor HTML. La mayoría de estas mejoras son pequeñas que facilitan la vida del Desarrollador Web. Cosas como más fragmentos de código de HTML5, la sangría inteligente, las etiquetas de inicio y finalización coincidentes al editar y validar que tienen como destino el DOCTYPE del documento HTML son algunas de estas mejoras.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Improved_DOCTYPE_Validation"></a>
#### <a name="task-1---improved-doctype-validation"></a>Tarea 1: validación de DOCTYPE mejorada

El editor HTML tiene ahora la capacidad de comprobar el DOCTYPE de la página, aunque la definición podría estar en la página maestra. En función del DOCTYPE de la página, el editor HTML se validará con el conjunto de reglas correcto y filtrará la lista de IntelliSense que considere los elementos DOCTYPE.

En esta tarea, cambiará el DOCTYPE de una página para ver cómo cambia el comportamiento del editor HTML en consecuencia.

1. Si aún no está abierto, inicie **Visual Studio** y abra la solución **WhatsNewASPNET. sln** que se encuentra en la carpeta **Source\WhatsNewASPNET** de este laboratorio.
2. Abra la página **site. Master** .
3. Observe el esquema de destino de la barra de herramientas de validación. La forma en que se comporta el editor HTML (validación, IntelliSense, etc.) cambiará correctamente para ajustarse al DOCTYPE seleccionado.

    ![Usar DOCTYPE en la barra de herramientas de edición de código fuente HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image22.png "Usar DOCTYPE en la barra de herramientas de edición de código fuente HTML")

    *Usar DOCTYPE en la barra de herramientas de edición de código fuente HTML*
4. Cambie el esquema de destino a HTML 4,01.

    ![Cambiar DOCTYPE en la barra de herramientas de edición de código fuente HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image23.png "Cambiar DOCTYPE en la barra de herramientas de edición de código fuente HTML")

    *Cambiar DOCTYPE en la barra de herramientas de edición de código fuente HTML*
5. Coloque el cursor debajo del elemento **Body** y empiece a escribir el nombre de un elemento HTML5 (por ejemplo, **vídeo**). Observe que el elemento no está disponible en la lista de IntelliSense.

    ![Elementos HTML5 no enumerados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image24.png "Elementos HTML5 no enumerados")

    *Elementos HTML5 no enumerados*
6. Deshacer los cambios realizados en el esquema de destino de la barra de herramientas de validación y seleccionar DOCTYPE: XHTML5 en la lista desplegable.

    ![Usar DOCTYPE en la barra de herramientas de edición de código fuente HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image25.png "Usar DOCTYPE en la barra de herramientas de edición de código fuente HTML")

    *Restablecer DOCTYPE en la barra de herramientas de edición de código fuente HTML*
7. Coloque el cursor debajo del elemento **Body** y empiece a escribir un elemento HTML5 de nuevo (por ejemplo, como **vídeo**). Observe que los elementos HTML5 están ahora disponibles en la lista de IntelliSense.

    ![Elementos HTML5 que se muestran](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image26.png "Elementos HTML5 que se muestran")

    *Elementos HTML5 que se muestran*

<a id="Ex2Task2"></a>

<a id="Task_2_-_StartEnd_Tags_Automatic_Update"></a>
#### <a name="task-2---startend-tags-automatic-update"></a>Tarea 2: actualización automática de etiquetas de inicio y de cierre

Visual Studio ahora actualiza las etiquetas HTML de apertura o cierre del elemento que está editando para que coincidan entre sí. Esta nueva característica mejorará su productividad al editar etiquetas HTML.

1. En la página **default. aspx** , agregue un elemento **H3** con un título (por ejemplo, Visual Studio 2012 Rocks!).

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample5.aspx)]
2. Cambie la etiqueta **H3** y escriba **H2** o **H1.**

    Observe que la etiqueta de finalización se actualiza automáticamente. También puede modificar la etiqueta de cierre para ver que la etiqueta de inicio se actualiza en consecuencia.

    ![Actualización automática de la etiqueta final](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image27.png "Actualización automática de la etiqueta final")

    *Actualización automática de la etiqueta final*

<a id="Ex2Task3"></a>

<a id="Task_3_-_New_HTML5_Code_Snippets"></a>
#### <a name="task-3---new-html5-code-snippets"></a>Tarea 3: nuevos fragmentos de código HTML5

Visual Studio ahora incluye varios fragmentos de código HTML5. En esta tarea, usará algunos de estos fragmentos de código.

1. Agregue una nueva carpeta denominada **audio** a la raíz de la carpeta del sitio Web. Abra el explorador de Windows y copie cualquier archivo de audio en la carpeta **audio** de la solución **WhatsNewASPNET. sln** .
2. En la página **default. aspx** , busque el cursor en el Web11 Rocks!! Encabezado. Escriba **audio** y presione la tecla TAB.

    El nuevo editor HTML incluye fragmentos de código para el contenido de HTML5. Recuerde usar la definición de DOCTYPE adecuada para habilitar los fragmentos de código HTML5.

    ![Insertar fragmentos de código HTML5](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image28.png "Insertar fragmentos de código HTML5")

    *Insertar fragmentos de código HTML5*
3. Actualice el origen de audio para que apunte a un archivo de audio existente.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample6.aspx)]

    > [!NOTE]
    > Tendrá que agregar el archivo de audio a la solución.
4. Presione **F5** para ejecutar el sitio y reproducir el audio.

    ![Ejecutar el control de audio](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image29.png "Ejecutar el control de audio")

    *Ejecutar el control de audio*

    > [!NOTE]
    > También puede probar más fragmentos de código incluidos en Visual Studio, como vídeo, Ilustración, etc.
5. Ahora, intente insertar un control en parte de la página. Por ejemplo, intente insertar un control **GridView** , pero en lugar de escribir **&lt;GRI,** empiece a escribir **&lt;GV**. Observe que la lista de IntelliSense muestra el control **asp: GridView** .

    IntelliSense en el editor HTML proporciona ahora una búsqueda en mayúsculas y minúsculas, así como una coincidencia parcial (recuperando todos los elementos que contienen el término).

    ![Insertar un control GridView con listas de IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image30.png "Insertar un control GridView con listas de IntelliSense")

    *Insertar un control GridView con listas de IntelliSense*

    Si escribe **&lt;cuadrícula** obtendrá todos los elementos que coinciden con el término, pero Visual Studio sugerirá el control **GridView** :

    ![Insertar un control GridView con listas de IntelliSense y coincidencia parcial](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image31.png "Insertar un control GridView con listas de IntelliSense y coincidencia parcial")

    *Insertar un control GridView con listas de IntelliSense y coincidencia parcial*

<a id="Ex2Task4"></a>

<a id="Task_4_-_HTML_Editor_Smart_Tags"></a>
#### <a name="task-4---html-editor-smart-tags"></a>Tarea 4: etiquetas inteligentes del editor HTML

Otra mejora en el editor HTML es la característica de etiquetas inteligentes. Las etiquetas inteligentes facilitan la realización de tareas de desarrollo comunes o repetitivas por control. Esta característica ya estaba disponible en el diseñador HTML, pero no en el editor HTML.

1. Abra **site. Master** y busque el elemento de **menú ASP:** . Coloque el cursor en la etiqueta de apertura y observe que el glifo pequeño mostrado en la parte inferior del elemento: haga clic en él para abrir el menú tareas inteligentes. Tenga en cuenta que tiene acceso rápido a algunas tareas relacionadas con el control de menú.

    ![Tareas inteligentes para el control de menú](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image32.png "Tareas inteligentes para el control de menú")

    *Tareas inteligentes para el control de menú*

<a id="Ex2Task5"></a>

<a id="Task_5_-_Smart_Indentation"></a>
#### <a name="task-5---smart-indentation"></a>Tarea 5: sangría inteligente

Uno de los procedimientos recomendados en HTML es la sangría de los elementos anidados para que el código sea legible. En Visual Studio 2012, observará que el editor aplica automáticamente una sangría a los elementos mientras escribe el código.

> [!NOTE]
> En la versión anterior de Visual Studio, la sangría inteligente estaba disponible en el editor XML, pero no en el editor HTML.

1. Asegúrese de que la configuración de sangría en el editor HTML está establecida en sangría inteligente. Para ello, seleccione las **herramientas |** Opción de menú opciones y, a continuación, seleccione el **Editor de texto | HTML | Página de pestañas** en el panel izquierdo de la pantalla. Seleccione la opción sangría inteligente.

    ![Configuración del editor HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image33.png "Configuración del editor HTML")

    *Configuración del editor HTML*
2. En la página **default. aspx** , quite todo el contenido del elemento audio.
3. Coloque el cursor al final del elemento de **audio** de apertura y presione **entrar**.

    Observe que la nueva posición del cursor tiene un nivel de sangría adicional.

    ![Sangría inteligente en el editor HTML](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image34.png "Sangría inteligente en el editor HTML")

    *Sangría inteligente en el editor HTML*
4. Restaure la etiqueta de audio con el contenido que ha quitado o cierre **default. aspx** sin guardar los cambios.

<a id="Ex2Task6"></a>

<a id="Task_6_-_Extract_to_User_Control"></a>
#### <a name="task-6---extract-to-user-control"></a>Tarea 6: extraer a control de usuario

Las herramientas de refactorización incluidas en Visual Studio, como la extracción de una parte de código a una función, son excelentes características que facilitan la mejora y la refactorización del código existente. El homólogo de las páginas de ASP.NET sería la extracción de código HTML en un control de usuario. Hacerlo manualmente implicaría varios pasos, como la creación de un nuevo control de usuario, el movimiento de la sección de código al control de usuario, el registro de un prefijo de etiqueta para el control de usuario y, por último, la creación de instancias del control de usuario en las páginas. Ahora, la nueva herramienta *extraer a control de usuario* realiza automáticamente todos los pasos.

En esta tarea, usará la operación contextual nuevo extraer a control de usuario para generar un nuevo control de usuario a partir del código seleccionado.

1. En la página **default. aspx** , seleccione los elementos **H2** y **audio** .
2. Haga clic con el botón derecho y seleccione **extraer a control de usuario**.

    ![Opción de menú extraer a control de usuario](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image35.png "Opción de menú extraer a control de usuario")

    *Opción de menú extraer a control de usuario*
3. Escriba un nombre para el nuevo control de usuario. Por ejemplo, **Jukebox. ascx**y, a continuación, haga clic en **Aceptar**.

    ![Guardar el control de usuario extraído](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image36.png "Guardar el control de usuario extraído")

    *Guardar el control de usuario extraído*
4. Observe que el código seleccionado se extrajo a un control de usuario y la ubicación original del código seleccionado se ha reemplazado por una instancia del nuevo control de usuario.

    ![Página actualizada automáticamente para usar el nuevo control de usuario](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image37.png "Página actualizada automáticamente para usar el nuevo control de usuario")

    *Página actualizada automáticamente para usar el nuevo control de usuario*
5. Presione **F5** para ejecutar la página y compruebe que el control funciona.

<a id="Exercise3"></a>

<a id="Exercise_3_Whats_New_in_the_JavaScript_Editor"></a>
### <a name="exercise-3-whats-new-in-the-javascript-editor"></a>Ejercicio 3: novedades en el editor de JavaScript

Escribir o editar código JavaScript no es una tarea fácil, especialmente cuando la aplicación empieza a aumentar de tamaño y se encuentra trabajando con archivos largos y cientos de funciones. Normalmente, los desarrolladores de scripts deben realizar algún trabajo adicional para mantener la legibilidad del código y desplazarse por los archivos. Con la inclusión de bibliotecas de JavaScript como jQuery, la navegación por scripts se ha convertido en un desafío debido a la longitud del código.

Visual Studio ha renovado el editor de JavaScript con el compromiso de hacer que el modo de código sea accesible y organizado. Muchas de las características de Visual Studio que C# ya existían en o en los editores de VB se implementan ahora en el editor de JavaScript: ir a definición, sangría automática, documentación y validación cuando se escribe. Con la lista de IntelliSense renovada tendrá el catálogo de funciones de JavaScript a su alcance.

En este ejercicio, obtendrá información sobre algunas de las nuevas características y mejoras del editor de JavaScript. Examinará los archivos de ejemplo y detectará cada una de las nuevas características que harán que su programación de JavaScript sea más eficaz en Visual Studio 2012.

<a id="Ex3Task1"></a>

<a id="Task_1_-_JavaScript_Editor_New_Features"></a>
#### <a name="task-1---javascript-editor-new-features"></a>Tarea 1: características nuevas del editor de JavaScript

Esta tarea le presentará algunas de las nuevas características del editor de JavaScript, que se centran en organizar el código y ofrecer una mejor experiencia de usuario.

1. Si aún no está abierto, inicie **Visual Studio** y abra la solución **WhatsNewASPNET. sln** que se encuentra en la carpeta **Source\WhatsNewASPNET** de este laboratorio.
2. Presione **F5** para ejecutar la aplicación y, a continuación, haga clic en el vínculo JavaScript en la barra de navegación. Actualice la página varias veces y Compruebe cómo se incrementa el contador.

    ![Contador de páginas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image38.png "Contador de páginas")

    *Contador de páginas*
3. Cierre el explorador y vuelva a Visual Studio.
4. Abra la página **JavaScript. aspx** y busque el bloque de **&gt;de script de&lt;** (se muestra a continuación).

    En el código siguiente se usa el almacenamiento local de HTML5 para almacenar una variable *pageLoadCount* que almacena el número de veces que el usuario actual ha visitado la página. El almacenamiento local es una base de datos de clave-valor del lado cliente que se presentó con el estándar HTML5. Los datos se guardan en el equipo local, dentro del explorador del usuario.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample7.html)]

    > [!NOTE]
    > Asegúrese de que DOCTYPE está establecido en XHTML5 antes de continuar con los pasos siguientes.
5. Edite el código y observe que IntelliSense para JavaScript incluye características de HTML5, como el almacenamiento local, y sus métodos internos.

    ![Características de JavaScript de HTML5 en JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image39.png "Características de JavaScript de HTML5 en JavaScript")

    *Características de JavaScript de HTML5 en JavaScript*
6. Haga clic en cualquier corchete de apertura ( **{** ) del código de script y observe que los corchetes están resaltados.

    ![Los corchetes se resaltan](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image40.png "Los corchetes se resaltan")

    *Los corchetes se resaltan*
7. Quite la marca de comentario de la función **testAutoAlign ()** (seleccione las tres líneas y puede usar **Ctrl** + **K**; **CTRL** + **U**) y busque el cursor dentro del código de la función. Presione Entrar para anexar una segunda línea. Observe que el código ahora está **alineado** y **con sangría automática**.

    ![El código JavaScript está alineado automáticamente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image41.png "El código JavaScript está alineado automáticamente")

    *El código JavaScript está alineado automáticamente*

<a id="Ex3Task2"></a>

<a id="Task_2_-_Validating_JavaScript"></a>
#### <a name="task-2---validating-javascript"></a>Tarea 2: validación de JavaScript

En esta tarea, detectará la nueva validación de JavaScript para el estándar ECMAScript5. Esta característica le ayudará a escribir el código de JavaScript conforme, a la vez que evita problemas de scripting antes de la implementación del sitio.

> [!NOTE]
> Visual Studio 2010 implementó el cumplimiento de ECMAStript3, mientras que Visual Studio 2012 proporciona compatibilidad con ECMAScript5.

1. Abra **ECMA5script5. js** que se encuentra en la carpeta del proyecto **Scripts\custom** . Ahora probará la validación para ECMAScript5 Standard.

    [!code-html[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample8.html)]

    Puede desproteger el &quot; usar la dirección de &quot; **estricta** en la primera línea del archivo, lo que habilita el **modo STRICT**de ECMAScript5. Este modo se compone de un subconjunto del lenguaje que clarifica las ambigüedades de la edición anterior y agrega algunas características nuevas, como captadores y establecedores, compatibilidad de bibliotecas con JSON y una reflexión más completa en las propiedades del objeto.
2. Abra el **lista de errores** si aún no está abierto (menú**Ver** | **Lista de errores**). Observe que la declaración de **función** está subrayada. Esto se debe a que en las funciones estándar de ECMA5 no se pueden anidar dentro de estructuras de lenguaje. En la lista de errores siguiente verá los detalles de la advertencia.

    ![Mensaje de error de validación de JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image42.png "Mensaje de error de validación de JavaScript")

    *Mensaje de error de validación de JavaScript*
3. Comente el&quot;usar la dirección de **&quot;estricta** y observe que los errores desaparecen, pero permanecen las advertencias.
4. En la última línea del archivo, escriba cualquier cadena como **&quot;&quot;de prueba** (incluya las comillas para indicar que es una cadena). Escriba un punto junto a la cadena para mostrar la lista de IntelliSense y seleccione la opción de **recorte** .

    En el estándar ECMAScript5, los valores de cadena y las variables también tienen definidos métodos de cadena, como Trim, Uppercase, Search y Replace.

    ![Lista de IntelliSense en JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image43.png "Lista de IntelliSense en JavaScript")

    *Lista de IntelliSense en JavaScript*

<a id="Ex3Task3"></a>

<a id="Task_3_-_XML_Documentation_for_JavaScript"></a>
#### <a name="task-3---xml-documentation-for-javascript"></a>Tarea 3: documentación XML de JavaScript

En esta tarea, explorará las características de Visual Studio para la documentación XML en JavaScript. Verá que la lista de IntelliSense para JavaScript muestra la documentación XML de cada función. Además, detectará la característica de navegación en JavaScript.

1. Abra el archivo **XmlDoc. js** que se encuentra en **scripts/** carpeta de proyecto personalizada. Este archivo contiene documentación XML sobre cada una de las funciones de JavaScript.

    ![Documentación XML de JavaScript integrada en IntelliSense](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image44.png "Documentación XML de JavaScript integrada en IntelliSense")

    *Documentación XML de JavaScript integrada en IntelliSense*
2. Debajo de **Agregar** función en el archivo **XmlDoc. js** , cree una nueva función denominada **Test**.
3. En la función de **prueba** , llame a la función **Multiply** que recibe dos parámetros. Observe que el cuadro información sobre herramientas muestra la documentación de la función **Multiply** .

    [!code-javascript[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample9.js)]

    ![Documentación XML para las funciones de JavaScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image45.png "Documentación XML para las funciones de JavaScript")

    *Documentación XML para las funciones de JavaScript*
4. Complete la instrucción de llamada de función y escriba un *punto* para abrir la lista de IntelliSense en el valor devuelto. Observe que Visual Studio está detectando el valor **devuelto** en la documentación, tratando el valor como un número.

    ![Documentación XML para los tipos de valor devuelto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image46.png "Documentación XML para los tipos de valor devuelto")

    *Documentación XML para los tipos de valor devuelto*
5. Ahora, inserte una llamada a la función Agregar. Observe que el editor de JavaScript admite ahora sobrecargas de función. Al escribir un nombre de función, podrá seleccionar cualquiera de las sobrecargas disponibles que se especifican en la documentación.

    ![Documentación XML para sobrecargas](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image47.png "Documentación XML para sobrecargas")

    *Documentación XML para sobrecargas*
6. Abra el archivo **GotoDefinition. js** y busque la llamada de función **$ (). html ()** . Busque el cursor en **HTML**.
7. Presione **F12** y navegue hasta la definición. Tenga en cuenta que ahora puede acceder y examinar el código de JavaScript sin usar la herramienta **Buscar** .
8. Busque el cursor en la instrucción de jQuery antes del bloque de firma en la parte inferior del archivo de código. Presione **F12**. Se desplazará al archivo de biblioteca de jQuery. Tenga en cuenta que también puede navegar por los archivos de jQuery con **F12**.

    ![Navegar a las definiciones de jQuery](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image48.png "Navegar a las definiciones de jQuery")

    *Navegar a las definiciones de jQuery*

> [!NOTE]
> Asegúrese de que GotoDefinition. js no tiene errores de sintaxis antes de guardar el archivo.

<a id="Exercise4"></a>

<a id="Exercise_4_Bundling_and_Minification"></a>
### <a name="exercise-4-bundling-and-minification"></a>Ejercicio 4: agrupación y Minificación

¿Cuántas veces los sitios web incluyen más de un archivo JavaScript o CSS? Se trata de un escenario muy común en el que la Unión y minificación puede ayudar a reducir el tamaño del archivo y hacer que el sitio funcione más rápido. La nueva característica de agrupación de ASP.NET 4,5 empaqueta un conjunto de archivos. JS o CSS en un solo elemento, y reduce su tamaño minificar el contenido (es decir, quita los espacios en blanco no necesarios, quita los comentarios, reduce los identificadores).

La agrupación y minificación en ASP.NET 4,5 se realiza en tiempo de ejecución, de modo que el proceso puede identificar al agente de usuario (por ejemplo, Internet Explorer, Mozilla, etc.) y, por lo tanto, mejorar la compresión al tener como destino el explorador del usuario (por ejemplo, quitar el contenido específico de Mozilla). Cuando la solicitud proviene de IE).

En este ejercicio, obtendrá información sobre cómo habilitar y usar los distintos tipos de agrupación y minificación en ASP.NET 4,5.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Installing_the_Bundling_and_Minification_Package_from_NuGet"></a>
#### <a name="task-1---installing-the-bundling-and-minification-package-from-nuget"></a>Tarea 1: instalación del paquete de empaquetado y Minificación de NuGet

1. Si aún no está abierto, inicie **Visual Studio** y abra la solución **WhatsNewASPNET. sln** que se encuentra en la carpeta **Source\WhatsNewASPNET** de este laboratorio.
2. Abra la consola del administrador de paquetes NuGet. Para ello, use la **vista** de menú | **otra** consola del **administrador de paquetes**de Windows | .

    ![Abrir el administrador de paquetes file:///C:/Users/User/AppData/Local/Temp/Marker3744//media/44462/Multiple-Stylesheets-and-JavaScript-files-in-the-application.pngconsole](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image49.png "Abrir la consola del administrador de paquetes")

    *Abrir la consola del administrador de paquetes*
3. En la **consola del administrador de paquetes,** escriba **Install-Package Microsoft. Web. Optimization** y presione **entrar**.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Default_Bundles"></a>
#### <a name="task-2---default-bundles"></a>Tarea 2: agrupaciones predeterminadas

La manera más sencilla de usar la agrupación y minificación es habilitar los paquetes predeterminados. Este método usa convenciones para que pueda hacer referencia a la versión empaquetada y reducida para los archivos de JS y CSS en una carpeta.

En esta tarea, aprenderá a habilitar y hacer referencia a los archivos comreducidad y CSS de JS y CSS y a ver la salida resultante.

1. Si aún no está abierto, inicie **Visual Studio** y abra la solución **WhatsNewASPNET. sln** que se encuentra en la carpeta **Source\WhatsNewASPNET** de este laboratorio.
2. En el **Explorador de soluciones**, expanda las carpetas **styles**, **Scripts\custom** y **Scripts\bundle** .

    Tenga en cuenta que la aplicación está usando más de un archivo CSS y JS.

    ![Varias hojas de estilos y archivos JavaScript en la aplicación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image50.png "Varias hojas de estilos y archivos JavaScript en la aplicación")

    *Varias hojas de estilos y archivos JavaScript en la aplicación*
3. Abra el archivo **global.asax.CS** .

    Observe que el nuevo espacio de nombres **Microsoft. Web. Optimization** se marca como comentario al principio del archivo. Quite la marca de comentario de la directiva using para incluir las características de agrupación y minificación.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample10.cs)]
4. Busque el método de **Inicio de\_** de la aplicación.

    En este método, quite la marca de comentario de la llamada EnableDefaultBundles tal como se muestra en el siguiente fragmento de código. Esto nos permite hacer referencia a una colección empaquetada de archivos CSS en una carpeta mediante la ruta de acceso a esa carpeta, más el&quot; &quot;CSS o el sufijo &quot;JS&quot;.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample11.cs)]
5. Abra el archivo **. aspx de optimización** y busque el control de contenido de **HeadContent**.

    Observe que los archivos CSS y los archivos JS tienen una sola etiqueta a la que se hace referencia.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample12.aspx)]

    > [!NOTE]
    > Este código es para fines de demostración. Idealmente, hará referencia a las agrupaciones del archivo site. Master. En este código de ejemplo, verá que el archivo site. Master también hace referencia a algunos de los archivos agrupados, lo que hace que esta última referencia sea redundante.
6. Tenga en cuenta que los vínculos usan las convenciones de empaquetado en el atributo **href** para obtener todos los archivos CSS o JS de la carpeta Styles y Scripts\custom, respectivamente.

    Puede usar path **scripts/Custom/JS** como se muestra a continuación para agrupar y minimizar todos los archivos JS dentro de una carpeta **scripts/Custom** . Este es el comportamiento predeterminado con los paquetes predeterminados.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample13.aspx)]
7. Abra el archivo **Styles\Site.CSS** .

    Observe que el archivo CSS original contiene código con sangría, espacios en blanco y comentarios que amplían el archivo. (También el archivo JavaScript contiene espacios en blanco y comentarios).

    ![Uno de los archivos CSS originales en la carpeta scripts](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image51.png "Uno de los archivos CSS originales en la carpeta scripts")

    *Uno de los archivos CSS originales en la carpeta scripts*
8. Presione **F5** para ejecutar la aplicación y vaya a la página **optimización** .
9. Haga clic en el vínculo **agrupación de CSS** para descargar y abrir el archivo.

    Consulte el archivo agrupado reducida. Observará que se han quitado todos los espacios en blanco, los comentarios y los caracteres de sangría, lo que genera un archivo más pequeño.

    ![Archivos CSS agrupados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image52.png "Archivos CSS agrupados")

    *Archivos CSS agrupados*
10. Ahora haga clic en el vínculo de **agrupación de JS** para abrir el archivo agrupado de JavaScript. Puede omitir de forma segura la advertencia del explorador. Observe que los archivos JavaScript de la carpeta **personalizada** también están agrupados y reducida.

    ![Archivos de JavaScript agrupados](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image53.png "Archivos de JavaScript agrupados")

    *Archivos de JavaScript agrupados*

    La habilitación de la compresión para archivos CSS o JS era mucho más complicada en la versión anterior de ASP.NET. Ahora, como ha aprendido, solo tiene que agregar una línea en el archivo *global. asax* para habilitar la agrupación y, a continuación, hacer referencia a los archivos agrupados desde su sitio.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Static_Bundles"></a>
#### <a name="task-3---static-bundles"></a>Tarea 3: agrupaciones estáticas

El enfoque de agrupación estática permite personalizar el conjunto de archivos que se van a agrupar, la referencia y el método minificación que se usará.

En esta tarea, configurará una agrupación estática para definir un conjunto específico de archivos para agrupar y minimizar.

1. Cierre el explorador.
2. Abra el archivo **global.asax.CS** y busque el método de **Inicio de\_** de la aplicación.
3. Quite la marca de comentario del código de agrupación estática tal y como se muestra en el código siguiente.

    Está definiendo una agrupación estática a la que se hará referencia con la &quot; **~/StaticBundle**&quot; ruta de acceso virtual y use **JsMinify** para minificación de todos los archivos especificados con el método **AddFile** . Por último, agregará el paquete estático al **BundleTable** y lo habilitará.

    Tenga en cuenta que los archivos no se encuentran en el mismo lugar; Esta es otra ventaja sobre la agrupación predeterminada.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample14.cs)]
4. Abra el archivo. **aspx de optimización** .

    Tenga en cuenta que el vínculo a la **agrupación estática de JS** usa la ruta de acceso que ha declarado al configurar el paquete estático en el archivo global.asax.CS: **/StaticBundle**.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample15.aspx)]
5. Presione **F5** para ejecutar la aplicación y, a continuación, navegue hasta la página **optimización** .
6. Haga clic en el vínculo de **agrupación estática de JS** para abrir el archivo.

    Tenga en cuenta que el archivo JavaScript agrupado reducida es la salida de todos los archivos JavaScript configurados en el archivo de agrupación estática en la ruta de acceso &quot;/StaticBundle&quot;.

    ![Agrupación de archivos de JavaScript estáticos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image54.png "Agrupación de archivos de JavaScript estáticos")

    *Agrupación de archivos de JavaScript estáticos*
7. Cierre el explorador y vuelva a Visual Studio.

<a id="Ex4Task4"></a>

<a id="Task_4_-_Dynamic_Folder_Bundles"></a>
#### <a name="task-4---dynamic-folder-bundles"></a>Tarea 4: agrupaciones de carpetas dinámicas

En esta tarea, aprenderá a configurar paquetes de carpetas dinámicas. La eficacia de la Unión dinámica es que puede incluir JavaScript estático, así como otros archivos en lenguajes que se compila en JavaScript y, por lo tanto, requieren algún procesamiento antes de que se ejecute la agrupación.

En este ejemplo, aprenderá a usar la clase **DynamicFolderBundle** para crear un paquete dinámico para los archivos escritos en CofeeScript. CofeeScript es un lenguaje de programación que se compila en JavaScript y proporciona una sintaxis más sencilla para escribir código JavaScript y mejorar la brevedad y la legibilidad de JavaScript.

1. Abra el archivo **global.asax.CS** y busque el método de **Inicio de\_** de la aplicación.
2. Quite la marca de comentario del código de agrupación dinámica tal como se muestra en el código siguiente.

    Está definiendo un paquete de carpetas dinámicas que usará el procesador de minificación personalizado de **CoffeeMinify** que solo se aplicará a los archivos con la extensión &quot; **. Coffee**&quot; (archivos CoffeeScript). Tenga en cuenta que puede usar un patrón de búsqueda para seleccionar los archivos que se agruparán dentro de una carpeta, como "\*. Coffee".

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample16.cs)]
3. Abra la consola del administrador de paquetes NuGet. Para ello, use la **vista** de menú | **otra** consola del **administrador de paquetes**de Windows | .
4. En la **consola del administrador de paquetes,** escriba **Install-Package CoffeeSharp** y presione **entrar**.
5. Haga clic en el botón **Mostrar todos los archivos** de la ventana de **Explorador de soluciones**

    ![Mostrar todos los archivos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image55.png "Mostrar todos los archivos")

    *Mostrar todos los archivos*
6. Haga clic con el botón derecho en el archivo **CoffeeMinify.CS** en el **Explorador de soluciones** y seleccione **incluir en el proyecto** .

    ![Incluir el archivo CoffeeMinify.cs en el proyecto](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image56.png "Incluir el archivo CoffeeMinify.cs en el proyecto")

    *Incluir el archivo CoffeeMinify.cs en el proyecto*
7. Abra el archivo **CoffeeMinify.CS** .

    Esta clase hereda de JsMinify a minimizar la salida de JavaScript resultante de la compilación de código CoffeeScript. Llama al compilador CoffeeScript para generar primero el código de JavaScript y, a continuación, lo envía al método JsMinify. Process para minimizar el código resultante.

    [!code-csharp[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample17.cs)]
8. Abra los archivos **Script1. Coffee** y **Script2. Coffee** en la carpeta **scripts/bundle** .

    Estos archivos incluirán el código CoffeScript que se va a compilar mientras se realiza la agrupación con la clase CoffeeMinify.

    Por motivos de simplicidad, los archivos CoffeeScript proporcionados solo incluyen código de ejemplo CoffeeScript. Los comentarios los excluye el proceso JsMinify.

    ![Archivos CoffeeScript](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image57.png "Archivos CoffeeScript")

    *Archivos CoffeeScript*

    > [!NOTE]
    > [CofeeScript](https://github.com/jashkenas/coffeescript/) proporciona una sintaxis más sencilla para escribir código JavaScript, mejorar la brevedad y legibilidad de JavaScript, así como agregar otras características, como la comprensión de la matriz y la coincidencia de patrones.
9. Abra el archivo **. aspx de optimización** y busque los vínculos de agrupación.

    Tenga en cuenta que el vínculo a la **agrupación dinámica de JS** hace referencia a la carpeta **scripts/bundle** mediante el sufijo **/Coffee** configurado para el paquete de carpetas dinámicas.

    [!code-aspx[Main](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/samples/sample18.aspx)]
10. Presione **F5** para ejecutar la aplicación y, a continuación, navegue hasta la página **optimización** .
11. Haga clic en el vínculo **agrupación dinámica de JS** para abrir el archivo generado.

    Tenga en cuenta que el contenido incluido en este paquete solo contiene archivos **. Coffee** . También puede ver que el código CoffeeScript se compiló en JavaScript y que se han quitado las líneas con comentarios.

    ![Paquete de archivos dinámicos de JS](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image58.png "Paquete de archivos dinámicos de JS")

    *Paquete de archivos dinámicos de JS*

> [!NOTE]
> Además, puede implementar esta aplicación en sitios web de Windows Azure después del [Apéndice B: publicación de una aplicación de ASP.NET MVC 4 mediante Web deploy](#AppendixB).

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Este laboratorio le ayuda a comprender lo nuevo en ASP.NET y desarrollo web en Visual Studio 2012 y cómo aprovechar las ventajas de la variedad de mejoras en Visual Studio 2012.

Al completar este laboratorio práctico, aprenderá a usar las nuevas características y mejoras de los editores de Visual Studio 2012 para CSS, JavaScript y HTML. Además, ha aprendido cómo Visual Studio 2012 implementa la agrupación integrada y minificación.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image59.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image60.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image61.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image62.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image63.png)

    *VS Express para Web icono*

---

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación de Web Deploy proporcionada por Windows Azure.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio web desde el portal de Windows Azure

1. Vaya a la [portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas a su suscripción.

    > [!NOTE]
    > Con Windows Azure, puede hospedar 10 sitios web de ASP.NET de forma gratuita y, a continuación, escalar a medida que crezca el tráfico. Puede registrarse [aquí](https://aka.ms/aspnet-hol-azure).

    ![Iniciar sesión en Windows Azure Portal](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image64.png "Iniciar sesión en Windows Azure Portal")

    *Iniciar sesión en Windows Azure Portal de administración*
2. Haga clic en **nuevo** en la barra de comandos.

    ![Crear un nuevo sitio web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image65.png "Crear un nuevo sitio web")

    *Crear un nuevo sitio web*
3. Haga clic en **compute** | **sitio web**. A continuación, seleccione la opción **creación rápida** . Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio web**.

    > [!NOTE]
    > Un sitio web de Windows Azure es el host para una aplicación web que se ejecuta en la nube que puede controlar y administrar. La opción creación rápida permite implementar una aplicación web completada en el sitio web de Windows Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio web mediante creación rápida](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image66.png "Crear un nuevo sitio web mediante creación rápida")

    *Crear un nuevo sitio web mediante creación rápida*
4. Espere hasta que se cree el nuevo **sitio web** .
5. Una vez creado el sitio web, haga clic en el vínculo de la columna **URL** . Compruebe que el nuevo sitio web funciona.

    ![Ir al nuevo sitio web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image67.png "Ir al nuevo sitio web")

    *Ir al nuevo sitio web*

    ![Sitio web en ejecución](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image68.png "Sitio web en ejecución")

    *Sitio web en ejecución*
6. Vuelva al portal y haga clic en el nombre del sitio web en la columna **nombre** para mostrar las páginas de administración.

    ![Abrir las páginas de administración de sitios web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image69.png "Abrir las páginas de administración de sitios web")

    *Abrir las páginas de administración de sitios web*
7. En la página **Panel** , en la sección **vista rápida** , haga clic en el vínculo **Descargar Perfil de publicación** .

    > [!NOTE]
    > El *Perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse a todos los extremos para los que está habilitado un método de publicación y autenticarse en ellos. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para la publicación de aplicaciones web en sitios web de Windows Azure.

    ![Descargar el perfil de publicación del sitio web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image70.png "Descargar el perfil de publicación del sitio web")

    *Descargar el perfil de publicación del sitio web*
8. Descargue el archivo de Perfil de publicación en una ubicación conocida. Además, en este ejercicio verá cómo usar este archivo para publicar una aplicación web en sitios web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de Perfil de publicación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image71.png "Guardar el perfil de publicación")

    *Guardar el archivo de Perfil de publicación*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si su aplicación usa bases de datos de SQL Server, deberá crear un servidor de SQL Database. Si desea implementar una aplicación simple que no usa SQL Server podría omitir esta tarea.

1. Necesitará un servidor de SQL Database para almacenar la base de datos de la aplicación. Puede ver los servidores de SQL Database de su suscripción en el portal de administración de Windows Azure en **bases de datos SQL** | **servidores** | el **panel del servidor**. Si no tiene un servidor creado, puede crear uno mediante el botón **Agregar** de la barra de comandos. Anote el nombre del **servidor y la dirección URL, el nombre de inicio de sesión y la contraseña del administrador**, ya que los usará en las siguientes tareas. No cree todavía la base de datos, ya que se creará en una etapa posterior.

    ![Panel de SQL Database Server](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image72.png "Panel de SQL Database Server")

    *Panel de SQL Database Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo, debe incluir la dirección IP local en la lista de **direcciones IP permitidas**del servidor. Para ello, haga clic en **configurar**, seleccione la dirección IP de la **dirección IP del cliente actual** y péguela en los cuadros de texto **dirección IP inicial** y **dirección IP final** . Escriba un nombre para la regla y haga clic en el botón ![agregar-Client-IP-address-OK-Button](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image73.png).

    ![Agregando la dirección IP del cliente](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image74.png)

    *Agregando la dirección IP del cliente*
3. Una vez agregada la **dirección IP del cliente** a la lista direcciones IP permitidas, haga clic en **Guardar** para confirmar los cambios.

    ![Confirmar cambios](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image75.png)

    *Confirmar cambios*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicación de una aplicación de ASP.NET MVC 4 con Web Deploy

1. Vuelva a la solución ASP.NET MVC 4. En el **Explorador de soluciones**, haga clic con el botón secundario en el proyecto del sitio web y seleccione **publicar**.

    ![Publicar la aplicación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image76.png "Publicar la aplicación")

    *Publicar el sitio web*
2. Importe el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image77.png "Importar el perfil de publicación")

    *Importando Perfil de publicación*
3. Haga clic en **validar conexión**. Una vez finalizada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > La validación se completa una vez que aparece una marca de verificación verde junto al botón validar conexión.

    ![Validando conexión](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image78.png "Validando conexión")

    *Validando conexión*
4. En la página **configuración** , en la sección **bases** de datos, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de web deploy](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image79.png "Configuración de web deploy")

    *Configuración de web deploy*
5. Configure la conexión de base de datos de la siguiente manera:

   - En el **nombre del servidor** , escriba la dirección URL del servidor de SQL Database mediante el prefijo *TCP:* .
   - En **nombre de usuario** , escriba el nombre de inicio de sesión del administrador del servidor.
   - En **contraseña** , escriba la contraseña de inicio de sesión del administrador del servidor.
   - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

     ![Configuración de la cadena de conexión de destino](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image80.png "Configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pida que cree la base de datos, haga clic en **sí**.

    ![Crear la base de datos](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image81.png "Crear la cadena de base de datos")

    *Crear la base de datos*
7. La cadena de conexión que usará para conectarse a SQL Database en Windows Azure se muestra en el cuadro de texto conexión predeterminada. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunta a SQL Database](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image82.png "Cadena de conexión que apunta a SQL Database")

    *Cadena de conexión que apunta a SQL Database*
8. En la página **vista previa** , haga clic en **publicar**.

    ![Publicación de la aplicación Web](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image83.png "Publicación de la aplicación Web")

    *Publicación de la aplicación Web*
9. Una vez finalizado el proceso de publicación, el explorador predeterminado abrirá el sitio Web publicado.

    ![Aplicación publicada en Windows Azure](whats-new-in-aspnet-and-web-development-in-visual-studio-2012/_static/image84.png "Aplicación publicada en Windows Azure")

    *Aplicación publicada en Windows Azure*
