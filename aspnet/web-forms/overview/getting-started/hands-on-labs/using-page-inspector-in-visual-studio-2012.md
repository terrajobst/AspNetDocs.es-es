---
uid: web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
title: Usar Inspector de página en Visual Studio 2012 | Microsoft Docs
author: rick-anderson
description: 'En este laboratorio práctico, detectará una nueva herramienta para buscar y corregir problemas de páginas web en Visual Studio: el Inspector de página. Inspector de página es una nueva herramienta que b...'
ms.author: riande
ms.date: 02/18/2013
ms.assetid: 73232292-a5fe-4720-82a1-8f6553effd1f
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/using-page-inspector-in-visual-studio-2012
msc.type: authoredcontent
ms.openlocfilehash: f42b1be2697ba7d1145b3e334fe8f4ebf019cd12
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78473803"
---
# <a name="using-page-inspector-in-visual-studio-2012"></a>Usar el Inspector de página en Visual Studio 2012

por [equipo de grupos de web](https://twitter.com/webcamps)

> En este laboratorio práctico, detectará una nueva herramienta para buscar y corregir problemas de páginas web en Visual Studio: el Inspector de página.
> 
> Inspector de página es una nueva herramienta que incorpora herramientas de diagnóstico de explorador a Visual Studio y proporciona una experiencia integrada entre el explorador, ASP.NET y el código fuente. Presenta una página web (HTML, formularios Web Forms, ASP.NET MVC o Web pages) directamente en el IDE de Visual Studio y le permite examinar el código fuente y la salida resultante. Inspector de página le permite descomponer fácilmente un sitio web, compilar rápidamente páginas desde el principio y diagnosticar rápidamente los problemas.
> 
> En la actualidad, tenemos una serie de Marcos web que crean sitios Web flexibles y escalables de manera oportuna, como ASP.NET MVC y WebForms. Por otro lado, resulta más difícil encontrar problemas en las páginas porque el IDE no admite la vista de diseñador en las páginas basadas en plantillas y en el contenido dinámico. Por lo tanto, estos sitios web deben abrirse en un explorador para ver cómo aparecen para un usuario.
> 
> Los desarrolladores web usan herramientas externas para encontrar problemas que se ejecutan con regularidad en el explorador. A continuación, vuelven al IDE y empiezan a corregirse. Esta actividad entre el IDE, el explorador y las herramientas de generación de perfiles puede ser ineficaz y, en ocasiones, requiere una nueva implementación y limpieza de la memoria caché cada vez que se desea reproducir un problema.
> 
> Inspector de página une un hueco en el desarrollo web entre el cliente (herramientas del explorador) y el servidor (ASP.NET y el código fuente) reuniendo lo mejor de ambos mundos mediante un conjunto combinado de características.
> 
> Con Inspector de página, puede ver qué elementos de los archivos de código fuente (incluido el código del servidor) han generado el marcado HTML que se va a representar en el explorador. Inspector de página también le permite modificar propiedades de CSS y atributos de elementos DOM para ver los cambios reflejados inmediatamente en el explorador.
> 
> Este laboratorio práctico le guiará a través de las características Inspector de página y le mostrará cómo puede usarlas para solucionar problemas en aplicaciones Web. **Este laboratorio contiene dos ejercicios que usan flujos similares pero que tienen como destino distintas tecnologías. Si es un desarrollador de ASP.NET MVC, siga el ejercicio uno; Si es un desarrollador de WebForms, siga el ejercicio dos**.
> 
> Este laboratorio le guía a través de las mejoras y las nuevas características descritas anteriormente mediante la aplicación de cambios menores en una aplicación Web de ejemplo que se proporciona en la carpeta de origen.
> 
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Descomponer un sitio web mediante Inspector de página
- Inspeccionar y obtener una vista previa de los cambios de estilos CSS con Inspector de página
- Detección y corrección de problemas en las páginas web con Inspector de página

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los siguientes elementos para completar este laboratorio:

- [Microsoft Visual Studio Express 2012 para web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (Lea el [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).
- Internet Explorer 9 o posterior

---

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los siguientes ejercicios:

1. [Ejercicio 1: uso de Inspector de página en proyectos de MVC de ASP.NET](#Exercise1)
2. [Ejercicio 2: usar Inspector de página en proyectos de WebForms](#Exercise2)

> [!NOTE]
> Cada ejercicio va acompañado de una solución de inicio, ubicada en la carpeta Inicio del ejercicio, que le permite seguir cada ejercicio con independencia de los demás. Dentro del código fuente de un ejercicio, también encontrará una carpeta final que contiene una solución de Visual Studio con el código que es el resultado de completar los pasos del ejercicio correspondiente. Puede usar estas soluciones como guía si necesita ayuda adicional a medida que trabaja en este laboratorio práctico.

Tiempo estimado para completar este laboratorio: **30 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Using_Page_Inspector_in_ASPNET_MVC_Projects"></a>
### <a name="exercise-1-using-page-inspector-in-aspnet-mvc-projects"></a>Ejercicio 1: uso de Inspector de página en proyectos de MVC de ASP.NET

En este ejercicio, obtendrá información sobre cómo obtener una vista previa y depurar una solución **ASP.NET MVC 4** mediante **Inspector de página**. En primer lugar, realizará una breve vuelta alrededor de la herramienta para conocer las características que facilitan el proceso de depuración web. A continuación, trabajará en una página web que contenga problemas de estilo. Aprenderá a usar Inspector de página para buscar el código fuente que genera el problema y corregirlo.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarea 1: explorar Inspector de página

En esta tarea, aprenderá a usar el Inspector de página en el contexto de un proyecto de ASP.NET MVC 4 que muestra una galería fotográfica.

1. Abra la carpeta **Begin** Solution ubicada en **source/EX1-MVC4/Begin/** .

   1. Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. En el Explorador de soluciones, busque la vista **index. cshtml** en la carpeta del proyecto **/views/Home** , haga clic con el botón secundario en ella y seleccione **ver en inspector de página**.

    ![Selección de un archivo para obtener una vista previa en Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image1.png "Selección de un archivo para obtener una vista previa en Inspector de página")

    *Selección de un archivo para obtener una vista previa en Inspector de página*
3. La ventana Inspector de página mostrará la dirección URL de */home/index* asignada a la vista de código fuente seleccionada.

    ![Primer contacto con PageInspector](using-page-inspector-in-visual-studio-2012/_static/image2.png)

    *Primer contacto con Inspector de página*

    La herramienta de Inspector de página se integra en el entorno de Visual Studio. El inspector contiene un explorador incrustado, junto con un eficaz generador de perfiles HTML. Tenga en cuenta que no tiene que ejecutar la solución para ver el aspecto de las páginas.

    > [!NOTE]
    > Cuando el ancho de Inspector de página explorador es menor que el ancho de la página abierta, no verá la página correctamente. Si esto ocurre, ajuste el ancho del Inspector de página.
4. Haga clic en la pestaña **archivos** en inspector de página.

    Verá todos los archivos de código fuente que componen la página de índice. Esta característica ayuda a identificar todos los elementos de un vistazo, especialmente cuando se trabaja con vistas y plantillas parciales. Tenga en cuenta que también puede abrir cada uno de los archivos si hace clic en los vínculos.

    ![Ficha-archivos-](using-page-inspector-in-visual-studio-2012/_static/image3.png)

    *Pestaña archivos*
5. Haga clic en el botón **alternar modo de inspección** , situado a la izquierda de las pestañas.

    Esta herramienta le permitirá seleccionar cualquier elemento de la página y ver su código HTML y Razor.

    ![Botón de alternancia-inspección-modo](using-page-inspector-in-visual-studio-2012/_static/image4.png)

    *Botón alternar Modo de inspección*
6. En el explorador de Inspector de página, mueva el puntero del mouse sobre los elementos de la página. Mientras mueve el puntero del mouse sobre cualquier parte de la página representada, se muestra el tipo de elemento y el código fuente o el marcado de origen correspondiente se resaltan en el editor de Visual Studio.

    ![Modo de inspección en acción](using-page-inspector-in-visual-studio-2012/_static/image5.png)

    *Modo de inspección en acción*

    > [!NOTE]
    > No Maximice la ventana de Inspector de página o no podrá ver la pestaña de vista previa que muestra el código fuente. Si hace clic en el elemento en Inspector de página cuando está maximizado, aparecerá el código fuente de la selección, pero se ocultará la ventana Inspector de página.

    Si presta atención al archivo **index. cshtml** , observará que se resalta la parte del código fuente que genera el elemento seleccionado. Esta característica facilita la edición de archivos de código fuente largos, lo que proporciona una manera directa y rápida de acceder al código.

    ![Inspeccionar elementos](using-page-inspector-in-visual-studio-2012/_static/image6.png)

    *Inspeccionar elementos*
7. Haga clic en el botón **alternar modo de inspección** (![Seleccione la pestaña HTML para mostrar el código HTML representado en el explorador de inspector de página.](using-page-inspector-in-visual-studio-2012/_static/image7.png "Seleccione la pestaña HTML para mostrar el código HTML representado en el explorador de Inspector de página.") ) para deshabilitar el cursor.
8. Seleccione la pestaña **HTML** para mostrar el código HTML representado en el explorador de inspector de página.
9. En el formato HTML, busque el elemento de lista con el vínculo Koala y selecciónelo.

    Tenga en cuenta que al seleccionar el código, el resultado correspondiente se resalta automáticamente en el explorador. Esta característica es útil para ver cómo se representa un bloque HTML en la página.

    ![Selección de un elemento HTML en la página](using-page-inspector-in-visual-studio-2012/_static/image8.png "Selección de un elemento HTML en la página")

    *Selección de un elemento HTML en la página*
10. Haga clic en el botón **alternar modo de inspección** para habilitar *modo de inspección* y haga clic en la barra de navegación. A la derecha del código HTML, en el panel estilos, verá una lista con los estilos CSS aplicados al elemento seleccionado.

    > [!NOTE]
    > Como el encabezado forma parte del diseño del sitio, Inspector de página también abrirá \_archivo layout. cshtml y resaltará el segmento de código afectado.

    ![Detectar estilos](using-page-inspector-in-visual-studio-2012/_static/image9.png)

    *Detectar estilos y archivos de código fuente de un elemento seleccionado*
11. Con el puntero de inspección de alternancia habilitado, mueva el puntero del mouse debajo de la barra destacada azul y haga clic en el círculo medio.

    ![Seleccionar un elemento](using-page-inspector-in-visual-studio-2012/_static/image10.png "Seleccionar un elemento")

    *Seleccionar un elemento*
12. En el panel estilos, busque el elemento **background-image** en el grupo de **contenido principal** . **Desactive** la **imagen de fondo** y vea lo que sucede. Observará que el explorador reflejará los cambios inmediatamente y el círculo está oculto.

    > [!NOTE]
    > Los cambios que se aplican en la pestaña estilos de Inspector de página no afectan a la hoja de estilos original. Puede desproteger los estilos o cambiar sus valores tantas veces como desee, pero se restaurarán después de actualizar la página.

    ![Habilitar y deshabilitar estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image11.png "Habilitar y deshabilitar estilos CSS")

    *Habilitar y deshabilitar estilos CSS*
13. Ahora, haga clic en el texto "**su logotipo aquí**" en el encabezado mediante el modo de inspección.
14. En la pestaña **estilos** , busque el atributo CSS **de tamaño de fuente** en el grupo **. sitio-título** . Haga doble clic en el valor del atributo y reemplace el valor 2,3 em por **3 em**y, a continuación, presione **entrar**. Observe que el título es más grande.

    ![Cambiar los valores CSS en el Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image12.png "Cambiar los valores CSS en el Inspector de página")

    *Cambiar los valores CSS en el Inspector de página*
15. Haga clic en la pestaña **estilos de seguimiento** , que se encuentra en el panel derecho de inspector de página. Se trata de una forma alternativa de ver todos los estilos aplicados a la selección, ordenados por nombre de atributo.

    ![Seguimiento de estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image13.png)

    *Seguimiento de estilos CSS del elemento seleccionado*
16. Otra característica de Inspector de página es el panel de diseño. Con el modo de inspección, seleccione la barra de navegación y, a continuación, haga clic en la pestaña **diseño** en el panel derecho. Verá el tamaño exacto del elemento seleccionado, así como su desplazamiento, margen, relleno y tamaño del borde. Tenga en cuenta que también puede modificar los valores de esta vista.

    ![Diseño de elementos en Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image14.png "Diseño de elementos en Inspector de página")

    *Diseño de elementos en Inspector de página*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarea 2: detectar y corregir problemas de estilo en la Galería fotográfica de

¿Cómo diagnosticaría problemas de páginas web con versiones anteriores de Visual Studio? Probablemente esté familiarizado con las herramientas de depuración web que se ejecutan fuera del IDE de Visual Studio, como Internet Explorer Herramientas de desarrollo o Firebug. Los exploradores solo entienden HTML, scripting y estilos, mientras que un marco subyacente genera el código HTML que se va a representar. Por ese motivo, a menudo es necesario implementar todo el sitio para ver el aspecto de las páginas Web.

Probablemente siguió estos pasos cuando deseaba detectar y corregir un problema en el sitio web:

1. Ejecute la solución desde Visual Studio o implemente la página en el servidor Web.
2. En el explorador, abra las herramientas de desarrollo que use, o simplemente abra el código fuente y los estilos y trate de hacer coincidir el problema. Para encontrar los archivos implicados, habría usado el&quot; de búsqueda de &quot;o &quot;buscar en los archivos&quot; características con el nombre de las clases de estilo.
3. Una vez detectado el error, detenga el explorador Web y el servidor.
4. Borre la memoria caché de exploración.
5. Vuelva a Visual Studio para aplicar una corrección. Repita todos los pasos para realizar la prueba.

Como no hay ningún WYSIWYG real en ASP.NET MVC 4, la mayoría de los problemas de estilo se detectan en una fase posterior, después de ejecutar o implementar la aplicación Web. Ahora, con Inspector de página, es posible obtener una vista previa de cualquier página sin ejecutar la solución.

En esta tarea, usará el inspector de página y corregirá algunos problemas con la aplicación de la Galería fotográfica de.

1. Con Inspector de página, busque los vínculos registro e **Inicio de sesión** en el lado izquierdo del encabezado.

    Tenga en cuenta que los vínculos no se muestran en el lugar esperado de la derecha y se muestran como una lista con viñetas. Ahora alineará los vínculos a la derecha y los cambiará de estilo en consecuencia.

    ![Búsqueda de los vínculos registro e inicio de sesión](using-page-inspector-in-visual-studio-2012/_static/image15.png "Búsqueda de los vínculos registro e inicio de sesión")

    *Búsqueda de los vínculos registro e inicio de sesión*
2. Con alternar Modo de inspección seleccionada, haga clic en cerrar para, pero no en, en el vínculo registrar para abrir su código.

    Tenga en cuenta que el código fuente de los vínculos se encuentra en el archivo **\_LoginPartial. cshtml** , no en index. cshtml ni en el \_layout. cshtml, que son los lugares en los que puede buscar en primer lugar. Se ha colocado directamente en el archivo de código fuente correcto.
3. En la pestaña **estilos** , busque y haga clic en la **sección\<> elemento #login** , que es el contenedor HTML de estos vínculos.

    Observe que el estilo de **#login** se encuentra automáticamente en **site. CSS** después de hacer clic en. Además, el código ya está resaltado.

    ![Seleccionar los estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image16.png "Seleccionar los estilos CSS")

    *Seleccionar los estilos CSS*
4. Quite los comentarios del atributo **Text-align** en el código resaltado quitando los caracteres de apertura y cierre y guarde el archivo **site. CSS** .

    Inspector de página es consciente de los distintos archivos que componen la página actual y puede detectar cuándo cambia cualquiera de estos archivos. Le avisa cada vez que la página actual del explorador no está sincronizada con los archivos de origen.
5. En el explorador de Inspector de página, haga clic en la barra situada debajo de la barra de direcciones para volver a cargar la página.

    ![Volver a cargar la página](using-page-inspector-in-visual-studio-2012/_static/image17.png)

    *Volver a cargar la página*

    Los vínculos están ahora a la derecha, pero siguen siendo similares a una lista con viñetas. Ahora, quitará las viñetas y alineará los vínculos horizontalmente.

    ![Página actualizada](using-page-inspector-in-visual-studio-2012/_static/image18.png)

    *Página actualizada*
6. Con el modo de inspección, seleccione cualquiera de los elementos de **&lt;li&gt;** que contengan el &quot;registrar&quot; y &quot;registro en&quot; vínculos. A continuación, haga clic en la **sección&lt;&gt; #login** elemento para tener acceso al código **styles. CSS** .

    ![Buscar el estilo](using-page-inspector-in-visual-studio-2012/_static/image19.png "Buscar el estilo")

    *Buscar el estilo*
7. En **style. CSS**, quite el comentario del código de **#login elementos Li** . El estilo que va a agregar ocultará la viñeta y mostrará los elementos horizontalmente.

    ![Reestilo de los vínculos de inicio de sesión](using-page-inspector-in-visual-studio-2012/_static/image20.png "Reestilo de los vínculos de inicio de sesión")

    *Reestilo de los vínculos de inicio de sesión*
8. Guarde el archivo **style. CSS** y haga clic en una vez en la barra que se encuentra debajo de la dirección para volver a cargar la página. Observe que los vínculos aparecen correctamente.

    ![Vínculos alineados en el lado derecho](using-page-inspector-in-visual-studio-2012/_static/image21.png "Vínculos alineados en el lado derecho")

    *Vínculos alineados en el lado derecho*
9. Por último, cambiará el título del encabezado. Use el modo de inspección para hacer clic **aquí** en el texto y llegar al código fuente que lo genera.
10. Ahora está en **\_layout. cshtml**, reemplace el texto "**su logotipo aquí**" por la**Galería fotográfica**. Guarde y actualice el explorador de Inspector de página.

    ![Asignar un nuevo título](using-page-inspector-in-visual-studio-2012/_static/image22.png "Asignar un nuevo título")

    *Asignar un nuevo título*

    ![Página de la Galería fotográfica](using-page-inspector-in-visual-studio-2012/_static/image23.png)

    *Página de la Galería fotográfica actualizada*
11. Por último, seleccione el proyecto de la **Galería Fotogalería** y presione **F5** para ejecutar la aplicación. Compruebe que todos los cambios funcionan según lo previsto.

---

<a id="Exercise2"></a>

<a id="Exercise_2_Using_Page_Inspector_in_WebForms_Projects"></a>
### <a name="exercise-2-using-page-inspector-in-webforms-projects"></a>Ejercicio 2: usar Inspector de página en proyectos de WebForms

En este ejercicio, obtendrá información sobre cómo obtener una vista previa y depurar una solución de WebForms mediante Inspector de página. En primer lugar, realizará una breve vuelta alrededor de la herramienta para obtener información sobre las Inspector de página características que facilitan el proceso de depuración web. A continuación, trabajará en una página web que contenga problemas de estilo. Aprenderá a usar Inspector de página para buscar el código fuente que genera el problema y corregirlo.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Exploring_Page_Inspector"></a>
#### <a name="task-1---exploring-page-inspector"></a>Tarea 1: explorar Inspector de página

En esta tarea, aprenderá a usar las características de Inspector de página en el contexto de un proyecto de WebForms que muestra una galería fotográfica.

1. Abra la solución de **Inicio** ubicada en **source/Ex2-WebForms/Begin/** Folder.

   1. Tendrá que descargar algunos paquetes NuGet que faltan antes de continuar. Para ello, haga clic en el menú **proyecto** y seleccione **administrar paquetes NuGet**.
   2. En el cuadro de diálogo **administrar paquetes NuGet** , haga clic en **restaurar** para descargar los paquetes que faltan.
   3. Por último, compile la solución haciendo clic en **Compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tiene que enviar todas las bibliotecas del proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, especificando las versiones de paquete en el archivo packages. config, podrá descargar todas las bibliotecas necesarias la primera vez que ejecute el proyecto. Esta es la razón por la que tendrá que ejecutar estos pasos después de abrir una solución existente desde este laboratorio.
2. En el Explorador de soluciones, busque la página **default. aspx** , haga clic con el botón secundario en ella y seleccione **ver en inspector de página**.

    ![Abrir default. aspx con Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image24.png "Abrir default. aspx con Inspector de página")

    *Abrir default. aspx con Inspector de página*
3. La ventana Inspector de página mostrará default. aspx.

    ![Ver default. aspx en Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image25.png "Ver default. aspx en Inspector de página")

    *Ver default. aspx en Inspector de página*

    La herramienta de Inspector de página se integra en el entorno de Visual Studio. El inspector contiene un explorador incrustado, junto con un eficaz generador de perfiles HTML que mostrará el código seleccionado. Tenga en cuenta que no tiene que ejecutar la solución para ver el aspecto de las páginas.

    > [!NOTE]
    > Cuando el ancho de Inspector de página explorador es menor que el ancho de la página abierta, no verá la página correctamente. Si esto ocurre, ajuste el ancho del Inspector de página.
4. Haga clic en la pestaña **archivos** en inspector de página.

    Verá todos los archivos de código fuente que están redactando la página predeterminada representada. Se trata de una característica útil para identificar todos los elementos de un vistazo, especialmente cuando se trabaja con controles de usuario y páginas maestras. Tenga en cuenta que también puede navegar a cada uno de los archivos.

    ![Pestaña archivos](using-page-inspector-in-visual-studio-2012/_static/image26.png "Pestaña archivos")

    *Pestaña archivos*
5. Haga clic en el botón **alternar modo de inspección** , situado a la izquierda de las pestañas.

    Esta herramienta le permitirá seleccionar cualquier elemento de la página y ver su código HTML y el origen. aspx.

    ![Botón alternar Modo de inspección](using-page-inspector-in-visual-studio-2012/_static/image27.png "Botón alternar Modo de inspección")

    *Botón alternar Modo de inspección*
6. En el explorador de Inspector de página, mueva el mouse sobre los elementos de la página. Mientras mueve el puntero del mouse sobre cualquier parte de la página representada, se muestra el tipo de elemento y el código fuente o el marcado de origen correspondiente se resaltan en el editor de Visual Studio.

    ![Modo de inspección en acción](using-page-inspector-in-visual-studio-2012/_static/image28.png "Modo de inspección en acción")

    *Modo de inspección en acción*

    > [!NOTE]
    > No Maximice la ventana de Inspector de página o no podrá ver la pestaña de vista previa que muestra el código fuente. Si hace clic en el elemento en Inspector de página cuando está maximizado, aparecerá el código fuente de la selección, pero se ocultará la ventana Inspector de página.

    Si presta atención al archivo **default. aspx** , observará que se resalta la parte del código fuente que genera el elemento seleccionado. Esta característica facilita la edición de archivos de código fuente largos, lo que proporciona una manera directa y rápida de acceder al código.

    ![Inspeccionar elementos](using-page-inspector-in-visual-studio-2012/_static/image29.png "Inspeccionar elementos")

    *Inspeccionar elementos*
7. Haga clic en el botón **alternar modo de inspección** (![Seleccione-HTML-Tab-to-display-The-HTML-Code-representated-in-the-page-inspector-browser).](using-page-inspector-in-visual-studio-2012/_static/image30.png "Seleccione-HTML-Tab-to-display-The-HTML-Code-representated-in-the-page-inspector-browser.") ), situado en Inspector de página pestañas, para deshabilitar el cursor.
8. Seleccione la pestaña **HTML** para mostrar el código HTML representado en el explorador de inspector de página.
9. En el código HTML, busque el elemento de lista con el vínculo Koala y selecciónelo.

    Tenga en cuenta que al seleccionar el código, la salida correspondiente se resalta automáticamente en el explorador. Esta característica es útil para ver cómo se representa un bloque HTML en la página.

    ![Seleccionar un elemento HTML en la página](using-page-inspector-in-visual-studio-2012/_static/image31.png "Seleccionar un elemento HTML en la página")

    *Seleccionar un elemento HTML en la página*
10. Haga clic en el botón **alternar modo de inspección** para habilitar *modo de inspección* y haga clic en la barra de navegación. A la derecha del código HTML, en el panel estilos, verá una lista con los estilos CSS aplicados al elemento seleccionado.

    > [!NOTE]
    > como el encabezado forma parte del diseño del sitio, Inspector de página también abrirá el archivo site. Master y resaltará el segmento de código afectado.

    ![Detectar WebForms de estilos](using-page-inspector-in-visual-studio-2012/_static/image32.png "Detectar estilos y archivos de código fuente de un elemento seleccionado")

    *Detectar estilos y archivos de código fuente de un elemento seleccionado*
11. Con el puntero de inspección de alternancia habilitado, mueva el puntero del mouse debajo de la barra de menús y haga clic en el círculo en blanco.

    ![Seleccionar un elemento](using-page-inspector-in-visual-studio-2012/_static/image33.png "Seleccionar un elemento")

    *Seleccionar un elemento*
12. En el panel estilos, busque el elemento **background-image** en el grupo de **contenido principal** . **Desactive** la **imagen de fondo** y vea lo que sucede. Observará que el explorador reflejará los cambios inmediatamente y el círculo está oculto.

    > [!NOTE]
    > Los cambios que se aplican en la pestaña estilos de Inspector de página no afectan a la hoja de estilos original. Puede desproteger los estilos o cambiar sus valores tantas veces como desee, pero se restaurarán después de actualizar la página.

    ![Habilitar y deshabilitar CSS styles2](using-page-inspector-in-visual-studio-2012/_static/image34.png "Habilitar y deshabilitar estilos CSS")

    *Habilitar y deshabilitar estilos CSS*
13. Ahora, haga clic en el texto "**su** **logotipo aquí"** en el encabezado mediante el modo de inspección.
14. En la pestaña **estilos** , busque el atributo CSS **de tamaño de fuente** en el grupo **. sitio-título** . Haga doble clic en el atributo una vez para editar su valor. Reemplace el valor 2.3 em por **3EM**y, a continuación, presione Entrar. Observe que el título es más grande.

    ![Cambiar valores CSS en la página Inspector2](using-page-inspector-in-visual-studio-2012/_static/image35.png "Cambiar los valores CSS en el Inspector de página")

    *Cambiar los valores CSS en el Inspector de página*
15. Haga clic en la pestaña **estilos de seguimiento** , que se encuentra en el panel derecho de inspector de página. Se trata de una forma alternativa de ver todos los estilos aplicados a la selección, ordenados por nombre de atributo.

    ![Seguimiento de estilos CSS del elemento seleccionado](using-page-inspector-in-visual-studio-2012/_static/image36.png "Seguimiento de estilos CSS del elemento seleccionado")

    *Seguimiento de estilos CSS del elemento seleccionado*
16. Otra característica de Inspector de página es el panel de diseño. Con el modo de inspección, seleccione la barra de navegación y, a continuación, haga clic en la pestaña **diseño** en el panel derecho. Verá el tamaño exacto del elemento seleccionado, así como su desplazamiento, margen, relleno y tamaño del borde. Tenga en cuenta que también puede modificar los valores de esta vista.

    ![Diseño de elementos en Inspector de página](using-page-inspector-in-visual-studio-2012/_static/image37.png "Diseño de elementos en Inspector de página")

    *Diseño de elementos en Inspector de página*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Finding_and_Fixing_Style_Issues_in_the_Photo_Gallery"></a>
#### <a name="task-2---finding-and-fixing-style-issues-in-the-photo-gallery"></a>Tarea 2: detectar y corregir problemas de estilo en la Galería fotográfica de

¿Cómo diagnosticaría problemas de páginas web con versiones anteriores de Visual Studio? Probablemente esté familiarizado con las herramientas de depuración web que se ejecutan fuera del IDE de Visual Studio, como Internet Explorer Herramientas de desarrollo o Firebug. Los exploradores solo entienden HTML, scripting y estilos, mientras que un marco subyacente genera el código HTML que se va a representar. Por ese motivo, a menudo es necesario implementar todo el sitio para ver el aspecto de las páginas Web.

Probablemente siguió estos pasos cuando deseaba detectar y corregir un problema en el sitio web:

1. Ejecute la solución desde Visual Studio o implemente la página en el servidor Web.
2. En el explorador, abra las herramientas de desarrollo que use, o simplemente abra el código fuente y los estilos y trate de hacer coincidir el problema. Para encontrar los archivos implicados, habría usado el&quot; de búsqueda de &quot;o &quot;buscar en los archivos&quot; características con el nombre de las clases de estilo.
3. Una vez detectado el error, detenga el explorador Web y el servidor.
4. Borre la memoria caché de exploración.
5. Vuelva a Visual Studio para aplicar una corrección. Repita todos los pasos para realizar la prueba.

Como no hay ningún WYSIWYG real en ASP.NET WebForms, algunos problemas de estilo se detectan en una fase posterior, después de ejecutarse o implementarse. Ahora, con Inspector de página, es posible obtener una vista previa de cualquier página sin ejecutar la solución.

En esta tarea, usará el inspector de página para corregir algunos problemas de la aplicación de la Galería fotográfica de. En los pasos siguientes, detectará y corregirá rápidamente un problema de estilo sencillo en el encabezado.

1. Mediante la inspección de páginas, busque los vínculos **registro e** **Inicio de sesión** en el lado izquierdo del encabezado.

    Tenga en cuenta que el vínculo no se muestra en el lugar esperado de la derecha. Ahora alineará el vínculo a la derecha y lo cambiará de estilo en consecuencia.

    ![Vínculo de inicio de sesión situado a la izquierda](using-page-inspector-in-visual-studio-2012/_static/image38.png "Vínculo de inicio de sesión situado a la izquierda")

    *Vínculo de inicio de sesión situado a la izquierda*
2. Con alternancia Modo de inspección seleccionada, seleccione el vínculo iniciar sesión para abrir su código.

    Observe que el código fuente del vínculo se encuentra en el archivo **site. Master** , no en la página default. aspx, que es el lugar en el que se puede buscar en primer lugar; se ha colocado directamente en el archivo de código fuente correcto.
3. En la pestaña **estilos** , busque y haga clic en la **sección&lt;&gt; elemento #login** , que es el contenedor HTML de estos vínculos.

    Observe que el estilo de **#login** se encuentra automáticamente en **site. CSS** después de hacer clic en. Además, el código ya está resaltado.

    ![Seleccionar los estilos CSS](using-page-inspector-in-visual-studio-2012/_static/image39.png "Seleccionar los estilos CSS")

    *Seleccionar los estilos CSS*
4. Quite los comentarios del atributo **Text-align** en el código resaltado quitando los caracteres de apertura y cierre y guarde el archivo **site. CSS** .

    Inspector de página es consciente de los distintos archivos que componen la página actual y puede detectar cuándo cambia cualquiera de estos archivos. Le avisa cada vez que la página actual del explorador no está sincronizada con los archivos de origen.
5. En el explorador de Inspector de página, haga clic en la barra situada debajo de la barra de direcciones para guardar los cambios y volver a cargar la página.

    ![Volver a cargar la página](using-page-inspector-in-visual-studio-2012/_static/image40.png)

    *Volver a cargar la página*

    Los vínculos están ahora a la derecha, pero siguen siendo similares a una lista con viñetas. Ahora, quitará las viñetas y alineará los vínculos horizontalmente.

    ![Página actualizada](using-page-inspector-in-visual-studio-2012/_static/image41.png)

    *Página actualizada*
6. Con el modo de inspección, seleccione cualquiera de los elementos de **&lt;li&gt;** que contengan el &quot;registrar&quot; y &quot;registro en&quot; vínculos. A continuación, haga clic en la **sección&lt;&gt; #login** elemento para tener acceso al código **styles. CSS** .

    ![Buscar el estilo](using-page-inspector-in-visual-studio-2012/_static/image42.png "Buscar el estilo")

    *Buscar el estilo*
7. En **style. CSS**, quite el comentario del código de **#login elementos Li** . El estilo que va a agregar ocultará la viñeta y mostrará los elementos horizontalmente.

    ![Reestilo de los vínculos de inicio de sesión](using-page-inspector-in-visual-studio-2012/_static/image43.png "Reestilo de los vínculos de inicio de sesión")

    *Reestilo de los vínculos de inicio de sesión*
8. Guarde el archivo **style. CSS** y haga clic en una vez en la barra que se encuentra debajo de la dirección para volver a cargar la página. Observe que los vínculos aparecen correctamente.

    ![Vínculos alineados en el lado derecho](using-page-inspector-in-visual-studio-2012/_static/image44.png "Vínculos alineados en el lado derecho")

    *Vínculos alineados en el lado derecho*
9. Por último, cambiará el título del encabezado. En lugar de buscar el texto "**su logotipo aquí"** en todos los archivos, use el modo de inspección para hacer clic en el texto y obtener el código fuente que lo genera.

    ![Buscar el título del sitio](using-page-inspector-in-visual-studio-2012/_static/image45.png "Buscar el título del sitio")

    *Buscar el título del sitio*
10. Ahora está en **site. Master**, reemplace el texto "**su logotipo aquí**" por "**Galería fotográfica**". Guarde y actualice el explorador de Inspector de página.

    ![Página de la Galería fotográfica actualizada](using-page-inspector-in-visual-studio-2012/_static/image46.png "Página de la Galería fotográfica actualizada")

    *Página de la Galería fotográfica actualizada*
11. Por último, presione **F5** para ejecutar la aplicación. Compruebe que todos los cambios funcionan según lo previsto.

---

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico, aprenderá a usar Inspector de página para obtener una vista previa de la aplicación web sin tener que volver a generar y ejecutar el sitio web en un explorador. Además, ha aprendido cómo encontrar y corregir errores rápidamente accediendo directamente desde la salida representada al código fuente.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para web** u otra versión de &quot;Express&quot; con el **[instalador de plataforma web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)** . Las instrucciones siguientes le guían por los pasos necesarios para instalar *Visual Studio Express 2012 para web* mediante *instalador de plataforma web de Microsoft*.

1. Vaya a [[https://go.microsoft.com/?linkid=9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya tiene instalado el instalador de plataforma web, puede abrirlo y buscar el producto &quot;<em>Visual Studio Express 2012 para web con el SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene el **instalador de plataforma web** , se le redirigirá para que lo descargue e instale primero.
3. Una vez que el **instalador de plataforma web** está abierto, haga clic en **instalar** para iniciar la instalación.

    ![Instalar Visual Studio Express](using-page-inspector-in-visual-studio-2012/_static/image47.png "Instalar Visual Studio Express")

    *Instalar Visual Studio Express*
4. Lea todos los términos y licencias de los productos y **haga clic en Acepto para** continuar.

    ![Aceptación de los términos de licencia](using-page-inspector-in-visual-studio-2012/_static/image48.png)

    *Aceptación de los términos de licencia*
5. Espere hasta que se complete el proceso de descarga e instalación.

    ![Progreso de la instalación](using-page-inspector-in-visual-studio-2012/_static/image49.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **Finalizar**.

    ![Instalación completada](using-page-inspector-in-visual-studio-2012/_static/image50.png)

    *Instalación completada*
7. Haga clic en **salir** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la pantalla **Inicio** y empiece a escribir &quot;&quot;**vs Express** y, a continuación, haga clic en el icono de **vs Express para web** .

    ![VS Express para Web icono](using-page-inspector-in-visual-studio-2012/_static/image51.png)

    *VS Express para Web icono*
