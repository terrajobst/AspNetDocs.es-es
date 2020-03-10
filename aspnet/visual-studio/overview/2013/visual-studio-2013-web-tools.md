---
uid: visual-studio/overview/2013/visual-studio-2013-web-tools
title: 'Laboratorio práctico: Visual Studio 2013 herramientas web | Microsoft Docs'
author: rick-anderson
description: Visual Studio es un entorno de desarrollo excelente para. Proyectos de Windows y Web basados en .net. Incluye un eficaz editor de texto que se puede usar fácilmente para...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 09e82351-816b-402d-acd1-0f9ac6901d16
msc.legacyurl: /visual-studio/overview/2013/visual-studio-2013-web-tools
msc.type: authoredcontent
ms.openlocfilehash: 2fb987dd9b26ad9f0e8a88fd881bde4505ec4148
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78505009"
---
# <a name="hands-on-lab-visual-studio-2013-web-tools"></a>Laboratorio práctico: Visual Studio 2013 herramientas web

por [equipo de grupos de web](https://twitter.com/webcamps)

[Descargar el kit de aprendizaje de Web.](https://aka.ms/webcamps-training-kit)

> Visual Studio es un entorno de desarrollo excelente para. Proyectos de Windows y Web basados en .net. Incluye un eficaz editor de texto que se puede usar fácilmente para editar archivos independientes sin un proyecto.
> 
> Visual Studio mantiene un árbol de análisis completo mientras edita cada archivo. Esto permite que Visual Studio proporcione acciones de finalización automática y basadas en documentos sin parangón, a la vez que hace que la experiencia de desarrollo sea mucho más rápida y agradable. Estas características son especialmente eficaces en documentos HTML y CSS.
> 
> Toda esta potencia también está disponible para las extensiones, lo que facilita la ampliación de los editores con nuevas y eficaces características que se adaptan a sus necesidades. Web Essentials es una colección de mejoras (principalmente) relacionadas con la Web de Visual Studio. Incluye muchas finalizaciones de IntelliSense nuevas (especialmente para CSS), nuevas características de vínculo del explorador, JSHint automáticas para archivos JavaScript, nuevas advertencias para HTML y CSS, y muchas otras características que son esenciales para el desarrollo web moderno.
> 
> Todo el código de ejemplo y los fragmentos de código se incluyen en el kit de aprendizaje de Web., disponible en [https://aka.ms/webcamps-training-kit](https://aka.ms/webcamps-training-kit).

<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá a:

- Usar las nuevas características del editor HTML incluidas en Web Essentials, como fragmentos de código HTML5 enriquecidos y codificación Zen
- Usar las nuevas características del editor de CSS incluidas en Web Essentials como la información sobre herramientas del selector de colores y la matriz del explorador
- Usar las nuevas características del editor de JavaScript incluidas en Web Essentials como extraer a archivo e IntelliSense para todos los elementos HTML
- Intercambio de datos entre el explorador y Visual Studio mediante el vínculo del explorador

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio práctico, es necesario lo siguiente:

- [Microsoft Visual Studio Professional 2013](https://www.microsoft.com/visualstudio/) o superior
- [Web Essentials 2013](http://vswebessentials.com/)
- [Google Chrome](https://www.google.com/chrome/)

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

Con el fin de ejecutar los ejercicios de este laboratorio práctico, deberá configurar el entorno primero.

1. Abra una ventana del explorador de Windows y vaya a la carpeta de **origen** del laboratorio.
2. Haga clic con el botón secundario en **setup. cmd** y seleccione **Ejecutar como administrador** para iniciar el proceso de instalación que configurará el entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.
3. Si se muestra el cuadro de diálogo control de cuentas de usuario, confirme la acción para continuar.

> [!NOTE]
> Asegúrese de que ha comprobado todas las dependencias de este laboratorio antes de ejecutar el programa de instalación.

<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Usar los fragmentos de código

En todo el documento de laboratorio, se le indicará que inserte bloques de código. Para su comodidad, la mayor parte de este código se proporciona como fragmentos de Visual Studio Code, a los que puede acceder desde Visual Studio 2013 para evitar tener que agregarlo manualmente.

> [!NOTE]
> Cada ejercicio va acompañado de una solución de inicio que se encuentra en la carpeta **Inicio** del ejercicio que le permite seguir cada ejercicio con independencia de los demás. Tenga en cuenta que los fragmentos de código que se agregan durante un ejercicio no se encuentran en estas soluciones de inicio y puede que no funcionen hasta que haya completado el ejercicio. Dentro del código fuente de un ejercicio, también encontrará una carpeta **final** que contiene una solución de Visual Studio con el código que es el resultado de completar los pasos del ejercicio correspondiente. Puede usar estas soluciones como guía si necesita ayuda adicional a medida que trabaja en este laboratorio práctico.

---

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los siguientes ejercicios:

1. [Trabajar con vínculos del explorador y Web Essentials](#Exercise1)
2. [Aprovechar las ventajas de los fragmentos de código e IntelliSense](#Exercise2)

> [!NOTE]
> Al iniciar Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñada para coincidir con un estilo de desarrollo determinado y determina los diseños de ventana, el comportamiento del editor, los fragmentos de código de IntelliSense y las opciones del cuadro de diálogo. En los procedimientos de este laboratorio se describen las acciones necesarias para realizar una tarea determinada en Visual Studio cuando se usa la colección de **configuración de desarrollo general** . Si elige una colección de valores de configuración diferente para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.

<a id="Exercise1"></a>
### <a name="exercise-1-working-with-browser-link-and-web-essentials"></a>Ejercicio 1: trabajar con vínculos de explorador y Web Essentials

**Web Essentials** es una extensión de Visual Studio que agrega una gran variedad de características útiles para el desarrollo web moderno, principalmente centradas en facilitar la experiencia de desarrollo web mucho más rápida y agradable. Puede instalar Web Essentials desde la galería de extensiones en Visual Studio.

El **vínculo del explorador** es una característica nueva incluida en Visual Studio 2013 que proporciona un canal entre el IDE de Visual Studio y cualquier explorador abierto para intercambiar datos entre la aplicación web y Visual Studio. Web Essentials amplía el vínculo del explorador con herramientas para manipular el modelo de objetos DOM y los estilos CSS de las páginas web directamente desde el explorador.

En este ejercicio, explorará algunas de las características compatibles con **Web Essentials** y el **vínculo del explorador** para mejorar una sencilla página de cuestionario.

<a id="Ex1Task1"></a>
#### <a name="task-1---running-the-project-in-multiple-browsers"></a>Tarea 1: ejecutar el proyecto en varios exploradores

En esta tarea, configurará la aplicación web para que se ejecute en varios exploradores a la vez, lo que resulta útil para las pruebas entre exploradores.

1. Abra **Microsoft Visual Studio**.
2. En el menú **archivo** , seleccione **abrir | Proyecto o solución..** . y vaya a **EX1-WorkingwithBrowserLinkandWebEssentials\Begin** en la carpeta de **origen** del laboratorio (C:\WebCampsTK\HOL\VSWebTooling\Source). Seleccione **Begin. sln** y haga clic en **abrir**.
3. En la barra de herramientas de Visual Studio, expanda el menú del explorador y seleccione **examinar con..** ..

    ![Opción de menú explorar con](visual-studio-2013-web-tools/_static/image1.png "Examinar con... en el menú del explorador")

    *Opción de menú explorar con*
4. En el cuadro de diálogo **explorar con** , seleccione **Google Chrome** e **Internet Explorer** manteniendo presionada la tecla **Ctrl** y haga clic en **establecer como predeterminado**.

    ![Cuadro de diálogo explorar con](visual-studio-2013-web-tools/_static/image2.png "Cuadro de diálogo explorar con")

    *Seleccionar varios exploradores predeterminados*
5. Google Chrome e Internet Explorer deben aparecer ahora como exploradores predeterminados. Haga clic en **Cancelar** para cerrar el cuadro de diálogo.

    ![Google Chrome e Internet Explorer como exploradores predeterminados](visual-studio-2013-web-tools/_static/image3.png "Exploradores predeterminados de Google Chrome e Internet Explorer")

    *Google Chrome e Internet Explorer como exploradores predeterminados*

    > [!NOTE]
    > Después de configurar los exploradores predeterminados, se selecciona la opción **varios exploradores** en el menú del explorador.
    > 
    > ![Varios exploradores](visual-studio-2013-web-tools/_static/image4.png "Varios exploradores")
6. Presione **CTRL** + **F5** para ejecutar la aplicación sin depuración.
7. Cuando se abran las ventanas del explorador, coloque una de ellas por encima de la otra para ver las actualizaciones en ambos exploradores simultáneamente. Los exploradores deben mostrar una página web con un rectángulo azul claro.

    ![Colocar un explorador sobre el otro](visual-studio-2013-web-tools/_static/image5.png "Colocar un explorador sobre el otro")

    *Colocar un explorador sobre el otro*
8. No cierre los exploradores. Los usará en la siguiente tarea.

<a id="Ex1Task2"></a>
#### <a name="task-2---using-zen-coding-to-create-html-elements"></a>Tarea 2: uso de código Zen para crear elementos HTML

La **codificación de Zen** es un complemento de editor para la codificación y edición de HTML de alta velocidad, XML, XSL (o cualquier otro formato de código estructurado). El núcleo de este complemento es un eficaz motor de abreviaturas que permite expandir expresiones, similares a los selectores de CSS, en código HTML. La codificación Zen es una forma rápida de escribir HTML mediante una sintaxis de selector de estilo CSS.

En este ejercicio, usará la característica de codificación Zen proporcionada por Web Essentials para generar los botones HTML que representan las opciones de la pregunta.

1. Vuelva a Visual Studio.
2. Abra el archivo **index. cshtml** que se encuentra en la carpeta **views** | **Home** .
3. Reemplace el **&lt;!--todo: agregar opciones aquí--&gt;** comentario con el código siguiente y presione **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample1.css)]
4. El código debe expandirse a HTML.

    ![HTML expandido](visual-studio-2013-web-tools/_static/image6.png "HTML expandido")

    *HTML expandido*

    > [!NOTE]
    > Para más información sobre la sintaxis de codificación de Zen, consulte el [artículo](http://www.johnpapa.net/zen-coding-in-visual-studio-2012/)siguiente.
5. Haga clic en el botón **Actualizar exploradores vinculados** para actualizar los dos exploradores.

    ![Actualizar exploradores vinculados](visual-studio-2013-web-tools/_static/image7.png "Actualizar exploradores vinculados")

    *Actualizar exploradores vinculados*

    ![Internet Explorer-Página actualizada con cuatro botones](visual-studio-2013-web-tools/_static/image8.png "Internet Explorer-Página actualizada con cuatro botones")

    *Internet Explorer-Página actualizada con cuatro botones*

    ![Google Chrome-Página actualizada con cuatro botones](visual-studio-2013-web-tools/_static/image9.png "Google Chrome-Página actualizada con cuatro botones")

    *Google Chrome-Página actualizada con cuatro botones*
6. Vuelva a Visual Studio.
7. Ha agregado los botones a la página, pero todavía necesita agregar una pregunta de plantilla. Para ello, utilizará una nueva característica de Web Essentials denominada de **Lorem ipsum generator**. Busque el elemento **div** con el atributo de **clase** **delante**.
8. Agregue el código siguiente como primer elemento secundario del **div**y presione **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample2.css)]
9. El código debe expandirse a HTML.

    ![Lorem Ipsum generado automáticamente](visual-studio-2013-web-tools/_static/image10.png "Lorem Ipsum generado automáticamente")

    *Lorem Ipsum generado automáticamente*

    > [!NOTE]
    > Como parte de la codificación de Zen, ahora puede generar código Lorem ipsum directamente en el editor HTML. Simplemente escriba **Lorem** y pulse **Tab** y se insertará un texto de 30 palabras Lorem ipsum. P. ej., *lorem10* inserta 10 palabras ipsum ipsum.
10. Agregará un logotipo en la parte superior de la pregunta mediante otra característica nueva en Web Essentials denominada generador de **píxeles Lorem**. Agregue el código siguiente como primer elemento secundario del elemento **div** con **Container** como valor de **clase** y presione **Tab**.

    [!code-css[Main](visual-studio-2013-web-tools/samples/sample3.css)]
11. El código debe expandirse a HTML.

    ![Lorem pixel generado automáticamente](visual-studio-2013-web-tools/_static/image11.png "Lorem pixel generado automáticamente")

    *Lorem pixel generado automáticamente*

    > [!NOTE]
    > Como parte de la codificación de Zen, también puede generar código Lorem píxel directamente en el editor HTML. Simplemente escriba **PIX-200 x 200-Animals** y presione la **tecla TAB** y se insertará una etiqueta **IMG** con una imagen de 200 x 200 de un animal. Para obtener más información, consulte [Lorem pixel](http://www.lorempixel.com).
12. Haga clic en el botón **Actualizar exploradores vinculados** para actualizar los dos exploradores.

    ![Internet Explorer: imagen y texto generados automáticamente](visual-studio-2013-web-tools/_static/image12.png "Internet Explorer: imagen y texto generados automáticamente")

    *Internet Explorer: imagen y texto generados automáticamente*

    ![Google Chrome: imagen y texto generados automáticamente](visual-studio-2013-web-tools/_static/image13.png "Google Chrome: imagen y texto generados automáticamente")

    *Google Chrome: imagen y texto generados automáticamente*

    > [!NOTE]
    > Dado que la imagen se selecciona aleatoriamente al agregar el fragmento de código, la imagen que se muestra en los exploradores puede ser diferente.
13. No cierre los exploradores. Los usará en la siguiente tarea.

<a id="Ex1Task3"></a>
#### <a name="task-3---updating-a-style-property"></a>Tarea 3: actualizar una propiedad de estilo

En esta tarea, usará la característica de **modo inspeccionar** del vínculo del explorador para detectar la ubicación exacta en la que se genera el elemento DOM específico y, a continuación, actualizar la propiedad color de ese elemento mediante un selector de colores proporcionado por Web Essentials.

1. En el explorador de Internet Explorer, presione **CTRL** + **Alt** + **I** habilitar el modo de inspección.
2. Mueva el puntero sobre el borde azul claro y haga clic en.

    ![Mover el puntero sobre el borde de color azul claro](visual-studio-2013-web-tools/_static/image14.png "Mover el puntero sobre el borde de color azul claro")

    *Mover el puntero sobre el borde de color azul claro*
3. Vuelva a Visual Studio. Observe cómo el elemento HTML que seleccionó en el explorador también está seleccionado en el editor HTML de Visual Studio.

    ![Elemento HTML seleccionado en el editor HTML de Visual Studio](visual-studio-2013-web-tools/_static/image15.png "Elemento HTML seleccionado en el editor HTML de Visual Studio")

    *Elemento HTML seleccionado en el editor HTML de Visual Studio*
4. Ahora se actualizará la clase CSS **frontal** para cambiar el estilo del elemento seleccionado. Para ello, presione **CTRL** ** + para** abrir el cuadro de búsqueda **navegar a** . Escriba **site. CSS** y presione **entrar** para abrir el archivo.

    ![Abriendo archivo site. CSS](visual-studio-2013-web-tools/_static/image16.png "Abriendo archivo site. CSS")

    *Abriendo archivo site. CSS*
5. Presione **CTRL** + **F** y escriba **. Flip-container. Front** para buscar el selector de CSS.
6. Haga clic en el cuadrado azul claro en la propiedad border de la clase para abrir el selector de colores.

    ![Abrir el selector de colores](visual-studio-2013-web-tools/_static/image17.png "Abrir el selector de colores")

    *Abrir el selector de colores*
7. Expanda el selector de colores haciendo clic en el botón de contenido adicional y seleccione un nuevo color.

    ![Expandir el selector de colores](visual-studio-2013-web-tools/_static/image18.png "Expandir el selector de colores")

    *Expandir el selector de colores*
8. Presione **CTRL** + **Alt** + **entrar** para actualizar los exploradores vinculados.
9. Cambie a Internet Explorer y observe cómo ha cambiado el color del borde.

    ![Internet Explorer-color actualizado del borde](visual-studio-2013-web-tools/_static/image19.png "Internet Explorer-color actualizado del borde")

    *Internet Explorer-color actualizado del borde*
10. Cambie a Google Chrome y observe cómo ha cambiado el color del borde.

    ![Google Chrome: color de borde actualizado](visual-studio-2013-web-tools/_static/image20.png "Google Chrome: color de borde actualizado")

    *Google Chrome: color de borde actualizado*
11. Vuelva a Visual Studio.
12. Vaya al final del archivo **site. CSS** y presione **Ctrl** + **F** para buscar el selector **. BTN** .
13. Observe que la propiedad **-webkit-border-radius** está subrayada en verde.

    ![propiedad-webkit-border-radius del selector BTN](visual-studio-2013-web-tools/_static/image21.png "propiedad-webkit-border-radius del selector BTN")

    *propiedad-webkit-border-radius del selector BTN*
14. Colocar el símbolo de intercalación en la propiedad **-webkit-border-radius** . Debe aparecer una línea azul debajo de la primera letra de la primera palabra de la propiedad. Esta es la **etiqueta inteligente**.
15. Presione **CTRL** +  **.** para abrir el menú sugerencias y haga clic en **Agregar propiedad estándar que falta (Border-RADIUS)** .

    ![Agregar sugerencia de propiedad estándar que falta](visual-studio-2013-web-tools/_static/image22.png "Agregar sugerencia de propiedad estándar que falta")

    *Agregar sugerencia de propiedad estándar que falta*
16. La propiedad **border-radius** se agrega automáticamente a la regla CSS.

    ![Falta la propiedad estándar agregada](visual-studio-2013-web-tools/_static/image23.png "Falta la propiedad estándar agregada")

    *Falta la propiedad estándar agregada*
17. Mueva el puntero sobre la propiedad **border-radius** para mostrar la **información sobre herramientas**de la matriz de explorador. La **información sobre herramientas** de la matriz del explorador muestra la disponibilidad de la propiedad en cada explorador.

    ![Información sobre herramientas de la matriz de explorador](visual-studio-2013-web-tools/_static/image24.png "Información sobre herramientas de la matriz de explorador")

    *Información sobre herramientas de la matriz de explorador*
18. Observe que el valor de la propiedad **border-radius** todavía está subrayado. Mueva el puntero sobre el valor para ver el mensaje de advertencia.

    ![ADVERTENCIA de valor de propiedad border-radius](visual-studio-2013-web-tools/_static/image25.png "ADVERTENCIA de valor de propiedad border-radius")

    *ADVERTENCIA de valor de propiedad border-radius*
19. Quite la unidad del valor de propiedad **border-radius** como se sugiere en la información sobre herramientas.
20. Como **border-radius** es la propiedad estándar para definir el modo en que se redondean las esquinas de los bordes redondeados, puede quitar la propiedad **-webkit-border-radius** y el valor de la regla de CSS.
21. Coloque el símbolo de intercalación en la propiedad de **ajuste automático de texto** y observe que la etiqueta inteligente también aparece a continuación.
22. Abra el menú y haga clic en **Agregar información específica del proveedor que falta**.

    ![Agregar sugerencia de detalles de proveedor que faltan](visual-studio-2013-web-tools/_static/image26.png "Agregar sugerencia de detalles de proveedor que faltan")

    *Agregar sugerencia de detalles de proveedor que faltan*
23. La propiedad **-MS-Word-Wrap** se agrega automáticamente a la regla CSS.

    ![Propiedad específica del proveedor agregada](visual-studio-2013-web-tools/_static/image27.png "Propiedad específica del proveedor agregada")

    *Propiedad específica del proveedor agregada*

<a id="Ex1Task4"></a>
#### <a name="task-4---updating-the-html-code-from-the-browser"></a>Tarea 4: actualización del código HTML desde el explorador

En esta tarea, usará la característica de **modo de diseño** del vínculo del explorador para editar el objeto DOM desde el explorador y transferir los cambios al archivo de código fuente HTML en Visual Studio.

1. En Google Chrome, presione **CTRL** + **Alt** + **D** para habilitar el modo de diseño.
2. Mueva el puntero sobre la etiqueta **Lorem ipsum dolor sit amet** y haga clic en.

    ![Editar la pregunta](visual-studio-2013-web-tools/_static/image28.png "Editar la pregunta")

    *Editar la pregunta*
3. Debería aparecer un cursor. Reemplace el texto original por el *que aparece cuando escribo una pregunta más larga*y, a continuación, presione **ESC** para salir del modo de diseño.

    ![Pregunta modificada](visual-studio-2013-web-tools/_static/image29.png "Pregunta modificada")

    *Pregunta modificada*
4. Vuelva a Visual Studio y Abra **index. cshtml**, si aún no está abierto. Observe que se ha actualizado el texto interno del elemento **&lt;p&gt;** .

    ![Pregunta actualizada en la página HTML](visual-studio-2013-web-tools/_static/image30.png "Pregunta actualizada en la página HTML")

    *Pregunta actualizada en la página HTML*

<a id="Ex1Task5"></a>
#### <a name="task-5---reviewing-seo-related-warnings"></a>Tarea 5: revisar las advertencias relacionadas con SEO

La **optimización del motor de búsqueda** (SEO) es el proceso de crear una clasificación de sitio web superior en la lista de resultados de un motor de búsqueda. Cuanto mayor sea el número de sitios y más constantemente se muestren, más visitantes obtendrá el sitio de ese motor de búsqueda. Web Essentials incorpora una herramienta de análisis que examina HTML, notifica los problemas encontrados y proporciona ayuda para corregirlos.

1. Vaya al menú **Ver** y haga clic en **lista de errores** para abrir la ventana de **lista de errores** .

    ![Lista de errores en el menú Ver](visual-studio-2013-web-tools/_static/image31.png "Lista de errores en el menú Ver")

    *Lista de errores en el menú Ver*
2. Observe que hay una advertencia de SEO que notifica que falta un **&lt;etiqueta meta&gt;** de la descripción de la página. Haga doble clic en la entrada de advertencia de SEO para corregirlo.

    ![Lista de errores (ventana)](visual-studio-2013-web-tools/_static/image32.png "Lista de errores (ventana)")

    *Lista de errores (ventana)*
3. En el cuadro de diálogo **Web Essentials** , haga clic en **sí** para insertar una descripción &lt;etiqueta meta&gt;.

    ![Web Essentials (cuadro de diálogo)](visual-studio-2013-web-tools/_static/image33.png "Web Essentials (cuadro de diálogo)")

    *Web Essentials (cuadro de diálogo)*
4. Se abre el editor de **\_layout. cshtml** y la etiqueta **&lt;meta&gt;** se agrega automáticamente a la sección de **encabezado** del archivo HTML.

    ![Etiqueta meta agregada automáticamente en _Layout página](visual-studio-2013-web-tools/_static/image34.png "Etiqueta meta agregada automáticamente en _Layout página")

    *Etiqueta meta agregada automáticamente a \_página de diseño*
5. Cambie el valor del atributo **Content** a *GeekQuiz* y guarde el archivo.

<a id="Exercise2"></a>
### <a name="exercise-2-taking-advantage-of-code-snippets-and-intellisense"></a>Ejercicio 2: sacar partido de los fragmentos de código e IntelliSense

Con Web Essentials, el editor HTML se ha ampliado con funcionalidad adicional. En este ejercicio, verá algunas características nuevas que son útiles al desarrollar aplicaciones Web.

<a id="Ex2Task1"></a>
#### <a name="task-1---using-intellisense-in-html-documents"></a>Tarea 1: uso de IntelliSense en documentos HTML

La primera característica nueva que verá en esta tarea se denomina **IntelliSense dinámico**. IntelliSense dinámico Lee otras etiquetas y atributos para deducir los posibles identificadores que se usarán.

En esta tarea, creará un nuevo elemento de formulario HTML que contiene una etiqueta y un campo de entrada. A continuación, agregará un atributo **for** a la etiqueta para enlazarlo a la entrada y verá sugerencias de IntelliSense basadas en los identificadores de las entradas en el ámbito.

1. Abra **Visual Studio Express 2013 para web** y la solución **Begin. sln** que se encuentra en la carpeta **source/Ex2-TakingAdvantageofCodeSnippetsandIntelliSense/Begin** . Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.
2. En **Explorador de soluciones**, abra el archivo **index. cshtml** que se encuentra en la carpeta **views** | **Home** .
3. Agregue el siguiente formulario dentro de la **sección&lt;&gt;** elemento.

    (Fragmento de código- *VisualStudio2013WebTooling* - *Ex2* - *formulario*)

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample4.html)]
4. La etiqueta de entrada debe ir precedida de una etiqueta con alguna Descripción del campo. Agregue la siguiente etiqueta delante de la etiqueta de entrada.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample5.html)]
5. El atributo **for** de una **etiqueta de&lt;&gt;** especifica a qué elemento de formulario está enlazada una etiqueta. El valor del atributo debe ser igual al identificador del elemento relacionado. Agregue el atributo **for** al elemento **&lt;etiqueta&gt;** . Como se muestra en la ilustración siguiente, el nombre de &quot;&quot; valor aparece en el cuadro IntelliSense, en función del identificador de los elementos que se encuentran dentro del mismo ámbito (el **formulario de&lt;envolvente&gt;** ).

    ![Mostrar el identificador en IntelliSense](visual-studio-2013-web-tools/_static/image35.png "Mostrar el identificador en IntelliSense")

    *Mostrar el identificador en IntelliSense*
6. Elimine el **formulario&lt;** recientemente agregado&gt;elemento y su contenido.

<a id="Ex2Task2"></a>
#### <a name="task-2---using-html-code-snippets"></a>Tarea 2: usar fragmentos de código HTML

HTML5 presentó más de 25 etiquetas semánticas nuevas. Visual Studio ya tenía compatibilidad con IntelliSense para estas etiquetas, pero Visual Studio 2013 agiliza y facilita la escritura de marcado agregando nuevos fragmentos de código. Aunque estas etiquetas no son complicadas, incluyen algunos matices pequeños, como agregar las reservas de códec correctas para la etiqueta de *audio* . En esta tarea, verá los fragmentos de código HTML para la etiqueta de audio.

1. En el archivo **index. cshtml** , escriba **&lt;AUD** dentro de la **sección&lt;&gt;** elemento, tal y como se muestra en la ilustración siguiente.

    ![Insertar un elemento de audio](visual-studio-2013-web-tools/_static/image36.png "Insertar un elemento de audio")

    *Insertar un elemento de audio*
2. Presione la **tecla TAB** dos veces y observe cómo se agrega el código siguiente en la página y el cursor se coloca en el atributo **src** del primer origen.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample6.html)]

    > [!NOTE]
    > Al presionar la tecla **Tab** dos veces, se inserta el fragmento de código. El fragmento de código de audio muestra el uso estándar de la etiqueta de *audio* , con dos archivos de origen para mejorar la compatibilidad.
3. Elimine la segunda línea y actualice el origen de la primera línea con el siguiente vínculo a WebCampsTV Katana show: [http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3](http://media.ch9.ms/ch9/11d8/604b8163-fad3-4f12-9607-b404201211d8/KatanaProject.mp3). El código resultante se muestra a continuación.

    [!code-html[Main](visual-studio-2013-web-tools/samples/sample7.html)]

    > [!NOTE]
    > El archivo de código fuente *KatanaProject. mp3* se usa como ejemplo. Puede usar otro origen si lo prefiere.
4. Presione **CTRL** + **S** para guardar el archivo.
5. Presione **CTRL** + **F5** para iniciar la aplicación.
6. Observe que se ha agregado un reproductor de audio a la aplicación.

    ![Reproductor de audio en Internet Explorer](visual-studio-2013-web-tools/_static/image37.png "Reproductor de audio en Internet Explorer")

    *Reproductor de audio en Internet Explorer*

    ![Reproductor de audio en Google Chrome](visual-studio-2013-web-tools/_static/image38.png "Reproductor de audio en Google Chrome")

    *Reproductor de audio en Google Chrome*
7. No cierre los exploradores. Los usará en la siguiente tarea.

<a id="Ex2Task3"></a>
#### <a name="task-3---using-intellisense-in-javascript-documents"></a>Tarea 3: usar IntelliSense en documentos de JavaScript

Con Web Essentials 2013, hojas de estilos y páginas HTML, se genera una lista de identificadores y nombres de clase. En esta tarea, aprenderá cómo esas listas mejoran la compatibilidad con IntelliSense para JavaScript en Web Essentials 2013.

1. En el archivo **index. cshtml** , agregue el código siguiente para definir una etiqueta de **script** para el código JavaScript.

    [!code-cshtml[Main](visual-studio-2013-web-tools/samples/sample8.cshtml)]
2. Agregue el código siguiente dentro de la etiqueta de **script** para definir la función de devolución de llamada Ready.

    (Fragmento de código- *VisualStudio2013WebTooling* - *Ex2* - *ReadyFunction*)

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample9.js)]
3. Coloque el símbolo de intercalación en la etiqueta de **script** y presione **Ctrl** +  **.** para abrir el menú de sugerencias.
4. Haga clic en **extraer a archivo**.

    ![Sugerencia de extracción a archivo de JavaScript](visual-studio-2013-web-tools/_static/image39.png "Sugerencia de extracción a archivo de JavaScript")

    *Sugerencia de extracción a archivo de JavaScript*
5. En la ventana **Guardar como** , seleccione la carpeta **scripts** , asigne al archivo el nombre **init. js** y haga clic en **Guardar**.

    ![Guardar como ventana](visual-studio-2013-web-tools/_static/image40.png "Guardar como ventana")

    *Guardar como ventana*

    > [!NOTE]
    > Se crea el archivo **init. js** y el contenido del script se mueve al archivo.
    > 
    > ![Archivo init. js creado con el contenido incluido](visual-studio-2013-web-tools/_static/image41.png "Archivo init. js creado con el contenido incluido")
    > 
    > *Archivo init. js creado con el contenido incluido*
6. Abra el archivo **index. cshtml** y compruebe que la etiqueta de script se ha reemplazado por una referencia al archivo **init. js** .

    ![Referencia HTML de init. js](visual-studio-2013-web-tools/_static/image42.png "Referencia HTML de init. js")

    *Referencia HTML de init. js*
7. Vaya al **Explorador de soluciones** y observe que el archivo **init. js** se incluyó automáticamente en la solución.

    ![Archivo init. js incluido en la solución](visual-studio-2013-web-tools/_static/image43.png "Archivo init. js incluido en la solución")

    *Archivo init. js incluido en la solución*
8. Vuelva al archivo **init. js** para actualizar la devolución de llamada de función **preparada** .
9. Dentro de la definición de devolución de llamada de función que se pasa a *Ready*, agregue el código siguiente para obtener todos los elementos de un atributo de clase específico.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample10.js)]
10. Presione **CTRL** + **espacio** entre comillas dentro de la llamada a la función **getElementsByClassName** .

    ![Mostrar IntelliSense para la función getElementsByClassName](visual-studio-2013-web-tools/_static/image44.png "Mostrar IntelliSense para la función getElementsByClassName")

    *Mostrar IntelliSense para la función getElementsByClassName*

    > [!NOTE]
    > Observe que IntelliSense muestra las clases definidas en las hojas de estilos del proyecto.
11. Reemplace la línea que ha creado con el código siguiente.

    [!code-javascript[Main](visual-studio-2013-web-tools/samples/sample11.js)]
12. Coloque el cursor después de **au** dentro de las comillas en la función **getElementsByTagName** y presione **Ctrl** + **espacio**.

    ![Mostrar IntelliSense para el método getElementByTagName](visual-studio-2013-web-tools/_static/image45.png "Mostrar IntelliSense para el método getElementByTagName")

    *Mostrar IntelliSense para el método getElementsByTagName*
13. Seleccione **&quot;&quot;de audio** de la lista y presione **entrar**. El resultado se muestra en la ilustración siguiente.

    ![Recuperación de elementos de audio](visual-studio-2013-web-tools/_static/image46.png "Recuperación de elementos de audio")

    *Recuperación de elementos de audio*
14. En **Explorador de soluciones**, haga clic con el botón derecho en el archivo **init. js** de la carpeta **scripts** y seleccione **minimizar JavaScript files** en el menú **Web Essentials** .

    ![Archivos de minimizar JavaScript](visual-studio-2013-web-tools/_static/image47.png "Archivos JavaScript de minimizar")

    *Archivos de minimizar JavaScript*
15. Cuando se le pida que habilite minificación automática cuando el archivo de código fuente cambie, haga clic en **sí**.

    ![Habilitar la advertencia automática de minificación](visual-studio-2013-web-tools/_static/image48.png "Habilitar la advertencia automática de minificación")

    *Habilitar la advertencia automática de minificación*

    > [!NOTE]
    > **Init. min. js** se crea y se agrega como una dependencia del archivo **init. js** .
    > 
    > ![Archivo init. min. js creado](visual-studio-2013-web-tools/_static/image49.png "Archivo init. min. js creado")
    > 
    > *Archivo init. min. js creado*
16. Abra el archivo **init. min. js** y observe que el archivo es reducida.

    ![Contenido del archivo init. min. js](visual-studio-2013-web-tools/_static/image50.png "Contenido del archivo init. min. js")

    *Contenido del archivo init. min. js*
17. En el archivo **init. js** , agregue el siguiente código debajo de la llamada de la función **getElementsByTagName** para reproducir todos los elementos de audio.

    (Fragmento de código- *VisualStudio2013WebTooling* - *Ex2* - *PlayAudioElements*)

    [!code-csharp[Main](visual-studio-2013-web-tools/samples/sample12.cs)]
18. Haga clic en **CTRL** + **S** para guardar el archivo. Puesto que el archivo reducida ya está abierto, verá un cuadro de diálogo que indica que el archivo se modificó fuera del editor de código fuente. Haga clic en **Sí**.

    ![Microsoft Visual Studio ADVERTENCIA](visual-studio-2013-web-tools/_static/image51.png "Microsoft Visual Studio ADVERTENCIA")

    *Microsoft Visual Studio ADVERTENCIA*
19. Vuelva al archivo **init. min. js** para comprobar que el archivo se actualizó con el código nuevo.

    ![Archivo init. min. js actualizado](visual-studio-2013-web-tools/_static/image52.png "Archivo init. min. js actualizado")

    *Archivo init. min. js actualizado*
20. Haga clic en el botón **actualizar vínculo del explorador** .
21. Una vez actualizados los dos exploradores, los reproductores de audio que vio en la tarea anterior comenzarán a reproducirse automáticamente.

    ![Reproductor de audio incluido en la vista](visual-studio-2013-web-tools/_static/image53.png "Reproductor de audio incluido en la vista")

    *Reproductor de audio incluido en la vista*

---

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico, ha aprendido a:

- Usar las nuevas características del editor HTML incluidas en Web Essentials, como fragmentos de código HTML5 enriquecidos y codificación Zen
- Usar las nuevas características del editor de CSS incluidas en Web Essentials como la información sobre herramientas del selector de colores y la matriz del explorador
- Usar las nuevas características del editor de JavaScript incluidas en Web Essentials como extraer a archivo e IntelliSense para todos los elementos HTML
- Intercambio de datos entre el explorador y Visual Studio mediante el vínculo del explorador
