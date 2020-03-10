---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: Descripción de las capacidades de depuración de ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La capacidad de depurar código es una habilidad que cada desarrollador debe tener en su arsenal independientemente de la tecnología que estén usando. Aunque muchos desarrolladores...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 08ced380f3551407d757524dbc84b5feeeb5482b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510865"
---
# <a name="understanding-aspnet-ajax-debugging-capabilities"></a>Descripción de las capacidades de depuración de ASP.NET AJAX

por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> La capacidad de depurar código es una habilidad que cada desarrollador debe tener en su arsenal independientemente de la tecnología que estén usando. Aunque muchos desarrolladores están acostumbrados a usar Visual Studio .NET o Web Developer Express para depurar aplicaciones de ASP.NET que C# usan VB.net o el código, algunos no saben que también es muy útil para depurar código de cliente como JavaScript. El mismo tipo de técnicas que se usan para depurar aplicaciones .NET también se puede aplicar a aplicaciones habilitadas para AJAX y, más concretamente, aplicaciones AJAX ASP.NET.

## <a name="debugging-aspnet-ajax-applications"></a>Depuración de aplicaciones de AJAX de ASP.NET

Dan Wahlin

La capacidad de depurar código es una habilidad que cada desarrollador debe tener en su arsenal independientemente de la tecnología que estén usando. No indica que comprender las distintas opciones de depuración disponibles puede ahorrar una cantidad enorme de tiempo en un proyecto y quizás incluso algunos quebraderos de cabeza. Aunque muchos desarrolladores están acostumbrados a usar Visual Studio .NET o Web Developer Express para depurar aplicaciones de ASP.NET que C# usan VB.net o el código, algunos no saben que también es muy útil para depurar código de cliente como JavaScript. El mismo tipo de técnicas que se usan para depurar aplicaciones .NET también se puede aplicar a aplicaciones habilitadas para AJAX y, más concretamente, aplicaciones AJAX ASP.NET.

En este artículo, verá cómo se pueden usar Visual Studio 2008 y otras herramientas para depurar aplicaciones de ASP.NET AJAX con el fin de localizar rápidamente errores y otros problemas. Esta discusión incluirá información sobre cómo habilitar Internet Explorer 6 o una versión posterior para la depuración, mediante Visual Studio 2008 y el explorador de scripts para recorrer el código, así como para usar otras herramientas gratuitas como la aplicación auxiliar de desarrollo web. También aprenderá a depurar aplicaciones de ASP.NET AJAX en Firefox mediante una extensión denominada Firebug que le permite recorrer el código JavaScript directamente en el explorador sin ninguna otra herramienta. Por último, se introducirá en las clases de la biblioteca ASP.NET AJAX, que puede ayudar con diversas tareas de depuración, como las instrucciones de seguimiento y de aserción de código.

Antes de intentar depurar las páginas que se ven en Internet Explorer, hay algunos pasos básicos que debe realizar para habilitarla para la depuración. Echemos un vistazo a algunos requisitos básicos de configuración que deben realizarse para comenzar.

## <a name="configuring-internet-explorer-for-debugging"></a>Configuración de Internet Explorer para la depuración

La mayoría de los usuarios no le interesa ver los problemas de JavaScript que se encuentran en un sitio web visto con Internet Explorer. De hecho, el usuario medio todavía no sabría qué hacer si vio un mensaje de error. Como resultado, las opciones de depuración están desactivadas de forma predeterminada en el explorador. Sin embargo, es muy sencillo activar la depuración y ponerla en uso al desarrollar nuevas aplicaciones AJAX.

Para habilitar la funcionalidad de depuración, vaya a herramientas opciones de Internet en el menú de Internet Explorer y seleccione la pestaña avanzadas. En la sección exploración, asegúrese de que los siguientes elementos están desactivados:

- Deshabilitar la depuración de scripts (Internet Explorer)
- Deshabilitar la depuración de scripts (otros)

Aunque no es necesario, si está intentando depurar una aplicación, es probable que desee que los errores de JavaScript de la página sean visibles inmediatamente y obvios. Para forzar que se muestren todos los errores con un cuadro de mensaje, active la casilla "Mostrar una notificación sobre cada error de script". Aunque esta es una opción excelente para activar mientras se desarrolla una aplicación, puede resultar molesto si solo se usan otros sitios web, ya que las posibilidades de encontrar errores de JavaScript son bastante buenas.

La figura 1 muestra el aspecto que debe tener el cuadro de diálogo Opciones avanzadas de Internet Explorer Después de haberse configurado correctamente para la depuración.

[![configurar Internet Explorer para la depuración.](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**Figura 1**: configuración de Internet Explorer para la depuración.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))

Una vez que se haya activado la depuración, verá que aparece un nuevo elemento de menú en el menú Ver denominado depurador de scripts. Tiene dos opciones disponibles, entre las que se incluyen abrir e interrumpir en la siguiente instrucción. Cuando esté seleccionada la opción abrir, se le pedirá que depure la página en Visual Studio 2008 (tenga en cuenta que Visual Web Developer Express también puede usarse para la depuración). Si Visual Studio .NET se está ejecutando actualmente, puede elegir usar esa instancia o crear una nueva instancia. Cuando se selecciona interrumpir en la siguiente instrucción, se le pedirá que depure la página cuando se ejecute código JavaScript. Si el código de JavaScript se ejecuta en el evento onLoad de la página, puede actualizar la página para desencadenar una sesión de depuración. Si se ejecuta código JavaScript después de hacer clic en un botón, el depurador se ejecutará inmediatamente después de hacer clic en el botón.

> [!NOTE]
> Si está ejecutando en Windows Vista con la Access Control de usuario (UAC) habilitada y tiene Visual Studio 2008 establecido para ejecutarse como administrador, Visual Studio no podrá asociarse al proceso cuando se le pida que se adjunte. Para solucionar este problema, inicie Visual Studio primero y use esa instancia para depurar.

Aunque en la siguiente sección se muestra cómo depurar una página de ASP.NET AJAX directamente desde Visual Studio 2008, el uso de la opción del depurador de scripts de Internet Explorer es útil cuando una página ya está abierta y le gustaría inspeccionarla por completo.

## <a name="debugging-with-visual-studio-2008"></a>Depuración con Visual Studio 2008

Visual Studio 2008 proporciona funcionalidad de depuración que los desarrolladores de todo el mundo confían a diario para depurar aplicaciones .NET. El depurador integrado le permite recorrer paso a paso el código, ver los datos de los objetos, inspeccionar las variables específicas, supervisar la pila de llamadas más mucho más. Además de depurar VB.NET o C# código, el depurador también es útil para depurar aplicaciones de ASP.NET AJAX y le permitirá recorrer el código JavaScript línea a línea. Los detalles que se indican a continuación se centran en las técnicas que se pueden usar para depurar archivos de script en el cliente en lugar de proporcionar un Discourse en el proceso general de depuración de aplicaciones con Visual Studio 2008.

El proceso de depuración de una página en Visual Studio 2008 se puede iniciar de varias maneras diferentes. En primer lugar, puede usar la opción del depurador de scripts de Internet Explorer mencionada en la sección anterior. Esto funciona bien cuando una página ya está cargada en el explorador y desea iniciar la depuración. Como alternativa, puede hacer clic con el botón secundario en una página. aspx en el Explorador de soluciones y seleccionar establecer como página de inicio en el menú. Si está acostumbrado a depurar páginas de ASP.NET, es probable que lo haya hecho antes. Una vez presionada la tecla F5, se puede depurar la página. Sin embargo, aunque normalmente puede establecer un punto de interrupción en cualquier lugar que desee C# en VB.net o en el código, no siempre es el caso con JavaScript, como verá a continuación.

*Scripts incrustados frente a externos*

El depurador de Visual Studio 2008 trata JavaScript Embedded en una página distinta de la de los archivos JavaScript externos. Con archivos de script externos, puede abrir el archivo y establecer un punto de interrupción en cualquier línea que elija. Los puntos de interrupción se pueden establecer haciendo clic en el área de bandeja gris situada a la izquierda de la ventana del editor de código. Cuando JavaScript se incrusta directamente en una página mediante la etiqueta `<script>`, no es posible establecer un punto de interrupción haciendo clic en el área de la bandeja gris. Los intentos de establecer un punto de interrupción en una línea de script incrustado producirán una advertencia que indica "esta no es una ubicación válida para un punto de interrupción".

Para solucionar este problema, puede mover el código a un archivo. js externo y hacer referencia a él mediante el atributo src del script &lt;&gt; etiqueta:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

¿Qué ocurre si mover el código a un archivo externo no es una opción o requiere más trabajo del que merece? Aunque no se puede establecer un punto de interrupción mediante el editor, puede agregar directamente la instrucción del depurador en el código en el que desea iniciar la depuración. También puede usar la clase sys. Debug disponible en la biblioteca de ASP.NET AJAX para forzar el inicio de la depuración. Más adelante en este artículo obtendrá más información sobre la clase sys. Debug.

Un ejemplo del uso de la palabra clave `debugger` se muestra en la lista 1. Este ejemplo obliga al depurador a interrumpir justo antes de que se realice una llamada a una función de actualización.

**Lista 1. Usar la palabra clave Debugger para forzar la interrupción del depurador de Visual Studio .NET.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Una vez que se llega a la instrucción del depurador, se le pedirá que depure la página con Visual Studio .NET y puede empezar a recorrer el código. Al hacerlo, puede encontrar un problema con el acceso a los archivos de script de la biblioteca de ASP.NET AJAX usados en la página. echemos un vistazo a uso de Visual Studio. Explorador de scripts de la red.

## <a name="using-visual-studio-net-windows-to-debug"></a>Usar Windows de Visual Studio .NET para depurar

Una vez que se inicia una sesión de depuración y se empieza a recorrer el código con la tecla F11 predeterminada, puede aparecer el cuadro de diálogo de error que aparece en la figura 2, a menos que todos los archivos de script usados en la página estén abiertos y disponibles para la depuración.

[![cuadro de diálogo de error que se muestra cuando no hay código fuente disponible para la depuración.](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**Figura 2**: cuadro de diálogo de error que se muestra cuando no hay código fuente disponible para la depuración.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))

Este cuadro de diálogo se muestra porque Visual Studio .NET no está seguro de cómo obtener el código fuente de algunos de los scripts a los que hace referencia la página. Aunque esto puede ser bastante frustrante en primer lugar, hay una solución sencilla. Una vez que haya iniciado una sesión de depuración y haya alcanzado un punto de interrupción, vaya a la ventana Depurar explorador de Windows scripts de Windows en el menú de Visual Studio 2008 o use la tecla de acceso rápido Ctrl + Alt + N.

> [!NOTE]
> Si no puede ver la lista del menú Explorador de scripts, vaya a **herramientas** > **personalizar** > **comandos** en el menú de Visual Studio .net. Busque la entrada de **depuración** en la sección de categorías y haga clic en ella para mostrar todas las entradas de menú disponibles. En la lista comandos, desplácese hacia abajo hasta el explorador de scripts y arrástrelo hasta el menú Depurar ventanas de la mencionada anteriormente. Esto hará que la entrada de menú Explorador de scripts esté disponible cada vez que ejecute Visual Studio .NET.

El explorador de scripts se puede usar para ver todos los scripts que se usan en una página y abrirlos en el editor de código. Una vez que el explorador de scripts está abierto, haga doble clic en la página. aspx que se está depurando actualmente para abrirla en la ventana del editor de código. Realice la misma acción para todos los demás scripts que se muestran en el explorador de scripts. Una vez que todos los scripts están abiertos en la ventana de código, puede presionar F11 (y usar las otras teclas de acceso rápido de depuración) para recorrer el código. En la figura 3 se muestra un ejemplo del explorador de scripts. Muestra el archivo actual que se está depurando (demo. aspx), así como dos scripts personalizados y dos scripts insertados dinámicamente en la página mediante el ScriptManager de AJAX de ASP.NET.

[![el explorador de scripts proporciona acceso sencillo a los scripts usados en una página.](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**Figura 3**. El explorador de scripts proporciona acceso sencillo a los scripts usados en una página.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))

También se pueden usar otras ventanas para proporcionar información útil a medida que se recorre el código en una página. Por ejemplo, puede usar la ventana variables locales para ver los valores de las distintas variables usadas en la página, la ventana inmediato para evaluar las variables o condiciones específicas y ver la salida. También puede usar la ventana de salida para ver las instrucciones de seguimiento escritas mediante la función sys. Debug. Trace (que se tratará más adelante en este artículo) o la función Debug. writeln de Internet Explorer.

A medida que recorre el código mediante el depurador, puede pasar el mouse sobre las variables del código para ver el valor que están asignados. Sin embargo, en ocasiones el depurador de script no mostrará nada al pasar el puntero sobre una variable de JavaScript determinada. Para ver el valor, resalte la instrucción o variable que está intentando ver en la ventana del editor de código y, a continuación, haga clic en él. Aunque esta técnica no funciona en todas las situaciones, muchas veces podrá ver el valor sin tener que buscar en una ventana de depuración diferente, como la ventana variables locales.

Puede ver un tutorial en vídeo en el que se muestran algunas de las características que se describen aquí en [http://www.xmlforasp.net](http://www.xmlforasp.net).

## <a name="debugging-with-web-development-helper"></a>Depurar con aplicación auxiliar de desarrollo web

Aunque Visual Studio 2008 (y Visual Web Developer Express 2008) son herramientas de depuración muy compatibles, hay opciones adicionales que se pueden usar, y que son más ligeras. Una de las herramientas más recientes que se van a publicar es la aplicación auxiliar de desarrollo web. Microsoft Nikhil Kothari (uno de los arquitectos principales de ASP.NET AJAX en Microsoft) escribió esta excelente herramienta, que puede realizar muchas tareas diferentes a partir de una depuración sencilla para ver los mensajes de solicitud y respuesta HTTP. La aplicación auxiliar de desarrollo web se puede descargar en [http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx).

La aplicación auxiliar de desarrollo web se puede usar directamente dentro de Internet Explorer, lo que facilita su uso. Se inicia seleccionando Herramientas aplicación auxiliar de desarrollo web en el menú de Internet Explorer. Se abrirá la herramienta en la parte inferior del explorador, que es muy útil, ya que no tiene que salir del explorador para realizar varias tareas, como el registro de mensajes de respuesta y solicitudes HTTP. En la figura 4 se muestra la apariencia de la aplicación auxiliar de desarrollo web en acción.

[Aplicación auxiliar de desarrollo web ![](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**Figura 4**: aplicación auxiliar de desarrollo web ([haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))

La aplicación auxiliar de desarrollo web no es una herramienta que se usará para recorrer el código línea a línea como con Visual Studio 2008. Sin embargo, se puede usar para ver el resultado del seguimiento, evaluar fácilmente las variables de un script o explorar los datos en un objeto JSON. También es muy útil para ver los datos que se pasan a y desde una página de ASP.NET AJAX y un servidor.

Una vez que la aplicación auxiliar de desarrollo web está abierta en Internet Explorer, la depuración de scripts debe estar habilitada si se selecciona script habilitar depuración de scripts en el menú auxiliar de desarrollo web, como se muestra anteriormente en la figura 4. Esto permite a la herramienta interceptar los errores que se producen cuando se ejecuta una página. También permite un acceso fácil a los mensajes de seguimiento que se muestran en la página. Para ver la información de seguimiento o ejecutar comandos de script para probar diferentes funciones dentro de una página, seleccione incluir consola de script para mostrar en el menú auxiliar de desarrollo web. Esto proporciona acceso a una ventana de comandos y una ventana inmediata simple.

*Ver mensajes de seguimiento y datos de objetos JSON*

La ventana inmediato se puede usar para ejecutar comandos de script o incluso cargar o guardar scripts que se usan para probar diferentes funciones en una página. La ventana comandos muestra los mensajes de seguimiento o de depuración escritos por la página que se está viendo. En la lista 2 se muestra cómo escribir un mensaje de seguimiento mediante la función Debug. writeln de Internet Explorer.

**Lista 2. Escribir un mensaje de seguimiento del lado cliente mediante la clase Debug.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Si la propiedad LastName contiene un valor de Doe, el ayudante de desarrollo web mostrará el mensaje "Person Name: DOE" en la ventana de comandos de la consola de script (suponiendo que se haya habilitado la depuración). La aplicación auxiliar de desarrollo web también agrega un objeto debugService de nivel superior en las páginas que se pueden usar para escribir información de seguimiento o ver el contenido de los objetos JSON. La lista 3 muestra un ejemplo del uso de la función de seguimiento de la clase debugService.

**Lista 3. Usar la clase debugService de la aplicación auxiliar de desarrollo web para escribir un mensaje de seguimiento.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

Una característica agradable de la clase debugService es que funcionará incluso si la depuración no está habilitada en Internet Explorer facilitando el acceso siempre a los datos de seguimiento cuando se ejecuta la aplicación auxiliar de desarrollo web. Cuando la herramienta no se utiliza para depurar una página, se omitirán las instrucciones de seguimiento, ya que la llamada a Window. debugService devolverá FALSE.

La clase debugService también permite ver los datos del objeto JSON mediante la ventana del inspector del ayudante de desarrollo web. La lista 4 crea un objeto JSON simple que contiene los datos de la persona. Una vez creado el objeto, se realiza una llamada a la función de inspección de la clase debugService para permitir que el objeto JSON se inspeccione visualmente.

**Lista 4. Usar la función debugService. inspeccionar para ver los datos del objeto JSON.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

La llamada a la función GetPerson () en la página o a través de la ventana inmediato hará que aparezca la ventana del cuadro de diálogo inspector de objetos, tal como se muestra en la figura 5. Las propiedades dentro del objeto se pueden cambiar dinámicamente resaltándolo, cambiando el valor mostrado en el cuadro de texto valor y haciendo clic en el vínculo actualizar. El uso del inspector de objetos facilita la visualización de datos de objetos JSON y el experimento con la aplicación de valores diferentes a las propiedades.

*Errores de depuración*

Además de permitir que se muestren los datos de seguimiento y los objetos JSON, la aplicación auxiliar de desarrollo web también puede ayudar en la depuración de errores en una página. Si se produce un error, se le pedirá que continúe con la siguiente línea de código o depure el script (vea la figura 6). La ventana cuadro de diálogo error de script muestra la pila de llamadas completa, así como los números de línea, para que pueda identificar fácilmente dónde se encuentran los problemas dentro de un script.

[![mediante la ventana del inspector de objetos para ver un objeto JSON.](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**Figura 5**: uso de la ventana del inspector de objetos para ver un objeto JSON.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))

La selección de la opción de depuración permite ejecutar instrucciones de script directamente en la ventana inmediato del ayudante de desarrollo web para ver el valor de las variables, escribir objetos JSON, etc. Si se vuelve a realizar la misma acción que desencadenó el error y Visual Studio 2008 está disponible en el equipo, se le pedirá que inicie una sesión de depuración para que pueda recorrer el código línea por línea como se describe en la sección anterior.

[![cuadro de diálogo de error de script del ayudante de desarrollo web](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**Figura 6**: cuadro de diálogo de error de script del ayudante de desarrollo web ([haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))

*Inspección de mensajes de solicitud y respuesta*

Al depurar páginas de ASP.NET AJAX, a menudo resulta útil ver los mensajes de solicitud y respuesta enviados entre una página y un servidor. Ver el contenido de los mensajes le permite ver si se pasan los datos adecuados, así como el tamaño de los mensajes. La aplicación auxiliar de desarrollo web proporciona una excelente característica de registro de mensajes HTTP que facilita la visualización de datos como texto sin formato o en un formato más legible.

Para ver los mensajes de solicitud y respuesta de AJAX de ASP.NET, el registrador HTTP debe estar habilitado seleccionando HTTP habilitar registro HTTP en el menú de la aplicación auxiliar de desarrollo web. Una vez habilitado, todos los mensajes enviados desde la página actual se pueden ver en el visor de registros HTTP al que se puede tener acceso seleccionando HTTP Mostrar registros HTTP.

Aunque ver el texto sin formato enviado en cada mensaje de solicitud/respuesta es ciertamente útil (y una opción en la aplicación auxiliar de desarrollo web), a menudo resulta más fácil ver los datos del mensaje en un formato más gráfico. Una vez que se ha habilitado el registro HTTP y se han registrado los mensajes, los datos del mensaje se pueden ver haciendo doble clic en el mensaje en el visor de registros HTTP. Esto le permite ver todos los encabezados asociados con un mensaje, así como el contenido real del mensaje. En la figura 7 se muestra un ejemplo de un mensaje de solicitud y un mensaje de respuesta que se ven en la ventana Visor de registros HTTP.

[![uso del visor de registros HTTP para ver los datos de los mensajes de solicitud y respuesta.](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**Figura 7**: uso del visor de registros http para ver los datos de los mensajes de solicitud y respuesta.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))

El visor de registros HTTP analiza automáticamente los objetos JSON y los muestra mediante una vista de árbol que facilita y agiliza la visualización de los datos de la propiedad del objeto. Cuando se usa un UpdatePanel en una página de ASP.NET AJAX, el visor divide cada parte del mensaje en partes individuales, tal como se muestra en la figura 8. Se trata de una característica excelente que hace que sea mucho más fácil ver y entender lo que hay en el mensaje en comparación con la visualización de los datos de mensaje sin procesar.

[![un mensaje de respuesta UpdatePanel visualizado mediante el visor de registros HTTP.](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**Figura 8**: un mensaje de respuesta UpdatePanel visualizado mediante el visor de registros http.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))

Hay otras herramientas que se pueden usar para ver los mensajes de solicitud y respuesta, además de la aplicación auxiliar de desarrollo web. Otra buena opción es Fiddler, que está disponible de forma gratuita en [http://www.fiddlertool.com](http://www.fiddlertool.com). Aunque Fiddler no se tratará aquí, también es una buena opción si necesita Inspeccionar exhaustivamente los encabezados y datos de los mensajes.

## <a name="debugging-with-firefox-and-firebug"></a>Depuración con Firefox y Firebug

Aunque Internet Explorer sigue siendo el explorador más usado, otros exploradores, como Firefox, se han vuelto bastante populares y se usan más y más. Como resultado, querrá ver y depurar las páginas de ASP.NET AJAX en Firefox, así como Internet Explorer, para asegurarse de que las aplicaciones funcionan correctamente. Aunque Firefox no se puede vincular directamente a Visual Studio 2008 para la depuración, tiene una extensión denominada Firebug que se puede usar para depurar páginas. Firebug puede descargarse de forma gratuita si va a [http://www.getfirebug.com](http://www.getfirebug.com).

Firebug proporciona un entorno de depuración completo que se puede usar para recorrer el código línea por línea, tener acceso a todos los scripts usados en una página, ver estructuras DOM, Mostrar estilos CSS e incluso realizar un seguimiento de los eventos que se producen en una página. Una vez instalado, se puede tener acceso a Firebug seleccionando Herramientas Firebug Open Firebug en el menú Firefox. Al igual que el ayudante para el desarrollo web, Firebug se usa directamente en el explorador, aunque también se puede usar como una aplicación independiente.

Una vez que Firebug se está ejecutando, los puntos de interrupción se pueden establecer en cualquier línea de un archivo JavaScript tanto si el script está incrustado en una página como si no. Para establecer un punto de interrupción, primero cargue la página adecuada que desea depurar en Firefox. Una vez cargada la página, seleccione el script que desea depurar en la lista desplegable scripts de Firebug. Se mostrarán todos los scripts usados por la página. Para establecer un punto de interrupción, haga clic en el área de la bandeja gris de Firebug en la línea donde debe ir el punto de interrupción, como haría en Visual Studio 2008.

Una vez que se ha establecido un punto de interrupción en Firebug, puede realizar la acción necesaria para ejecutar el script que debe depurarse, como hacer clic en un botón o actualizar el explorador para desencadenar el evento onLoad. La ejecución se detendrá automáticamente en la línea que contiene el punto de interrupción. En la ilustración 9 se muestra un ejemplo de un punto de interrupción que se ha desencadenado en Firebug.

[![control de los puntos de interrupción en Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**Figura 9**: control de puntos de interrupción en Firebug.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))

Una vez que se visita un punto de interrupción, puede depurar paso a paso por procedimientos o paso a paso para salir del código con los botones de flecha. A medida que recorre el código, las variables de script se muestran en la parte derecha del depurador, lo que le permite ver los valores y explorar en profundidad los objetos. Firebug también incluye una lista desplegable pila de llamadas para ver los pasos de ejecución del script que produjeron hasta la línea actual que se está depurando.

Firebug también incluye una ventana de consola que se puede usar para probar distintas instrucciones de script, evaluar variables y ver los resultados de un seguimiento. Para acceder a él, haga clic en la pestaña consola en la parte superior de la ventana Firebug. La página que se está depurando también se puede "inspeccionar" para ver la estructura y el contenido de DOM haciendo clic en la pestaña inspeccionar. Al pasar el mouse por encima de los distintos elementos DOM mostrados en la ventana del inspector, se resaltará la parte correspondiente de la página, lo que facilitará la tarea de ver dónde se usa el elemento en la página. Los valores de atributo asociados a un elemento determinado se pueden cambiar "en directo" para experimentar con la aplicación de distintos anchos, estilos, etc. a un elemento. Se trata de una característica agradable que evita tener que cambiar constantemente entre el editor de código fuente y el explorador Firefox para ver cómo afectan los cambios sencillos a una página.

En la figura 10 se muestra un ejemplo del uso del inspector DOM para buscar un cuadro de texto denominado txtCountry en la página. El inspector Firebug también se puede usar para ver los estilos CSS que se usan en una página, así como los eventos que se producen, como el seguimiento de los movimientos del mouse, los clics de botón, etc.

[![el uso del inspector DOM de Firebug.](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**Figura 10**: uso del inspector DOM de Firebug.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))

Firebug proporciona una manera ligera de depurar rápidamente una página directamente en Firefox, así como una excelente herramienta para inspeccionar distintos elementos de la página.

## <a name="debugging-support-in-aspnet-ajax"></a>Compatibilidad con la depuración en ASP.NET AJAX

La biblioteca de ASP.NET AJAX incluye muchas clases diferentes que se pueden usar para simplificar el proceso de agregar funcionalidades de AJAX en una página web. Puede utilizar estas clases para buscar elementos en una página y manipularlos, agregar nuevos controles, llamar a servicios web e incluso controlar eventos. La biblioteca de AJAX de ASP.NET también contiene clases que se pueden usar para mejorar el proceso de depuración de páginas. En esta sección se le presentará la clase sys. Debug y verá cómo se puede usar en las aplicaciones.

*Usar la clase sys. Debug*

La clase sys. debug (una clase de JavaScript que se encuentra en el espacio de nombres sys) se puede usar para realizar varias funciones diferentes, incluida la escritura de la salida del seguimiento, la realización de aserciones de código y la aplicación de errores en el código para que se pueda depurar. Se usa exhaustivamente en los archivos de depuración de la biblioteca ASP.NET AJAX (instalado en C:\Archivos de Programa\microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 de forma predeterminada) para realizar pruebas condicionales ( se denominan aserciones) que garantizan que los parámetros se pasan correctamente a las funciones, que los objetos contienen los datos esperados y escriben instrucciones de seguimiento.

La clase sys. Debug expone varias funciones diferentes que se pueden usar para controlar el seguimiento, las aserciones de código o los errores, como se muestra en la tabla 1.

**Tabla 1. Funciones de la clase sys. Debug.**

| **Nombre de la función** | **Descripción** |
| --- | --- |
| Assert (condición, mensaje, displayCaller) | Valida que el parámetro Condition sea true. Si la condición que se está probando es falsa, se usará un cuadro de mensaje para mostrar el valor del parámetro de mensaje. Si el parámetro displayCaller es true, el método también muestra información sobre el llamador. |
| clearTrace() | Borra los resultados de las instrucciones de las operaciones de seguimiento. |
| error (mensaje) | Hace que el programa detenga la ejecución y se interrumpa en el depurador. El parámetro Message se puede utilizar para proporcionar un motivo del error. |
| seguimiento (mensaje) | Escribe el parámetro de mensaje en el resultado del seguimiento. |
| traceDump (objeto, nombre) | Devuelve los datos de un objeto en un formato legible. El parámetro name se puede usar para proporcionar una etiqueta para el volcado de seguimiento. Todos los subobjetos del objeto que se va a volcar se escribirán de forma predeterminada. |

El seguimiento del lado cliente se puede usar de forma muy similar a la funcionalidad de seguimiento disponible en ASP.NET. Permite que se vean fácilmente diferentes mensajes sin interrumpir el flujo de la aplicación. En la lista 5 se muestra un ejemplo del uso de la función sys. Debug. Trace para escribir en el registro de seguimiento. Esta función simplemente toma el mensaje que se debe escribir como parámetro.

**Lista 5. Usar la función sys. Debug. Trace.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

Si ejecuta el código que se muestra en la lista 5, no verá ningún resultado del seguimiento en la página. La única manera de verlo es usar una ventana de consola disponible en Visual Studio .NET, aplicación auxiliar de desarrollo web o Firebug. Si desea ver la salida del seguimiento en la página, tendrá que agregar una etiqueta TextArea y asignarle un identificador de TraceConsole, como se muestra a continuación:

[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

Cualquier instrucción sys. Debug. Trace de la página se escribirá en el TextArea TraceConsole.

En los casos en los que desee ver los datos contenidos en un objeto JSON, puede usar la función traceDump de la clase sys. Debug. Esta función toma dos parámetros, incluido el objeto que se debe volcar en la consola de seguimiento y un nombre que se puede utilizar para identificar el objeto en el resultado del seguimiento. En el listado 6 se muestra un ejemplo del uso de la función traceDump.

**Listado 6. Usar la función sys. Debug. traceDump.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

En la figura 11 se muestra el resultado de llamar a la función sys. Debug. traceDump. Observe que, además de escribir los datos del objeto Person, también escribe los datos del subobjeto Address.

Además del seguimiento, la clase sys. Debug también se puede utilizar para realizar aserciones de código. Las aserciones se usan para probar que se cumplen determinadas condiciones mientras se ejecuta una aplicación. La versión de depuración de los scripts de la biblioteca de ASP.NET AJAX contiene varias instrucciones Assert para probar una variedad de condiciones.

En el listado 7 se muestra un ejemplo del uso de la función sys. Debug. Assert para probar una condición. El código comprueba si el objeto address es NULL antes de actualizar un objeto person.

[![salida de la función sys. Debug. traceDump.](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**Figura 11**: salida de la función sys. Debug. traceDump.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))

**Listado 7. Usar la función Debug. Assert.**

[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

Se pasan tres parámetros, incluida la condición que se va a evaluar, el mensaje que se muestra si la aserción devuelve false y si se debe mostrar o no información sobre el llamador. En los casos en que se produce un error en una aserción, se mostrará el mensaje, así como la información del autor de la llamada si el tercer parámetro era true. En la ilustración 12 se muestra un ejemplo del cuadro de diálogo de error que aparece si se produce un error en la aserción mostrada en la enumeración 7.

La función final que se va a cubrir es sys. Debug. Fail. Si desea forzar el código para que produzca un error en una línea determinada de un script, puede Agregar una llamada sys. Debug. FAIL en lugar de la instrucción del depurador que se usa normalmente en aplicaciones JavaScript. La función sys. Debug. FAIL acepta un parámetro de cadena único que representa el motivo del error, como se muestra a continuación:

[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]

[![un mensaje de error sys. Debug. Assert.](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**Figura 12**: un mensaje de error sys. Debug. Assert.  ([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))

Cuando se encuentra una instrucción sys. Debug. FAIL mientras se está ejecutando un script, el valor del parámetro de mensaje se mostrará en la consola de una aplicación de depuración como Visual Studio 2008 y se le pedirá que depure la aplicación. Un caso en el que esto puede resultar bastante útil es cuando no se puede establecer un punto de interrupción con Visual Studio 2008 en un script en línea, pero se desea que el código se detenga en una línea determinada para que pueda inspeccionar el valor de las variables.

*Descripción de la propiedad ScriptMode del control ScriptManager*

La biblioteca de ASP.NET AJAX incluye versiones de script de depuración y de versión que se instalan en C:\Archivos de Programa\microsoft ASP. NET\ASP.NET 2,0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 de forma predeterminada. Los scripts de depuración tienen un formato correcto, son fáciles de leer y tienen varias llamadas sys. Debug. Assert dispersas a lo largo de ellos, mientras que los scripts de versión tienen un espacio en blanco eliminado y usan la clase sys. Debug con moderación para minimizar el tamaño total.

El control ScriptManager agregado a las páginas de ASP.NET AJAX lee el atributo Debug del elemento de compilación en Web. config para determinar qué versiones de los scripts de biblioteca se van a cargar. Sin embargo, puede controlar si se cargan los scripts de depuración o de versión (scripts de biblioteca o sus propios scripts personalizados) cambiando la propiedad ScriptMode. ScriptMode acepta una enumeración ScriptMode cuyos miembros incluyen auto, Debug, release y inherit.

El valor predeterminado de ScriptMode es auto, lo que significa que el ScriptManager comprobará el atributo debug en Web. config. Cuando Debug es false, el ScriptManager cargará la versión de lanzamiento de los scripts de la biblioteca de ASP.NET AJAX. Cuando Debug es true, se cargará la versión de depuración de los scripts. Al cambiar la propiedad ScriptMode a Release o debug, se forzará que el ScriptManager cargue los scripts adecuados, independientemente del valor que tenga el atributo debug en Web. config. En la lista 8 se muestra un ejemplo del uso del control ScriptManager para cargar scripts de depuración de la biblioteca ASP.NET AJAX.

**Listado 8. Cargar scripts de depuración mediante ScriptManager**.

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

También puede cargar diferentes versiones (depuración o lanzamiento) de sus propios scripts personalizados mediante la propiedad scripts del ScriptManager junto con el componente ScriptReference, tal como se muestra en la lista 9.

**Listado 9. Cargar scripts personalizados mediante ScriptManager.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> Si va a cargar scripts personalizados mediante el componente ScriptReference, debe notificar a ScriptManager cuando el script haya terminado de cargarse agregando el código siguiente en la parte inferior del script:

[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

El código que se muestra en la lista 9 indica a ScriptManager que busque una versión de depuración del script person para que busque de forma automática person. Debug. js en lugar de person. js. Si no se encuentra el archivo person. Debug. js, se generará un error.

En los casos en los que desea que se cargue una versión de depuración o de lanzamiento de un script personalizado en función del valor de la propiedad ScriptMode establecida en el control ScriptManager, puede establecer la propiedad ScriptMode del control ScriptReference en inherit. Esto hará que se cargue la versión correcta del script personalizado en función de la propiedad ScriptMode del ScriptManager, tal como se muestra en la lista 10. Dado que la propiedad ScriptMode del control ScriptManager está establecida en depurar, el script person. Debug. js se cargará y usará en la página.

**Listado 10. Heredar el ScriptMode del ScriptManager para los scripts personalizados.**

[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

Al usar la propiedad ScriptMode correctamente, puede depurar aplicaciones más fácilmente y simplificar el proceso general. Los scripts de versión de la biblioteca de AJAX de ASP.NET son más difíciles de examinar y leer, ya que se ha quitado el formato de código mientras que los scripts de depuración tienen formato específico para la depuración.

## <a name="conclusion"></a>Conclusión

La tecnología de ASP.NET AJAX de Microsoft proporciona una base sólida para la creación de aplicaciones habilitadas para AJAX que pueden mejorar la experiencia global del usuario final. Sin embargo, como sucede con cualquier tecnología de programación, se producirán errores y otros problemas de aplicación. Conocer las distintas opciones de depuración disponibles puede ahorrar mucho tiempo y producir un producto más estable.

En este artículo se han presentado varias técnicas diferentes para depurar páginas de ASP.NET AJAX, como Internet Explorer con Visual Studio 2008, aplicación auxiliar de desarrollo web y Firebug. Estas herramientas pueden simplificar el proceso de depuración global, ya que puede tener acceso a los datos variables, recorrer el código línea por línea y ver las instrucciones de seguimiento. Además de las distintas herramientas de depuración descritas, también ha visto cómo se puede usar la clase sys. Debug de la biblioteca de ASP.NET AJAX en una aplicación y cómo se puede usar la clase ScriptManager para cargar versiones de depuración o lanzamiento de scripts.

## <a name="bio"></a>Disponibilidad

Dan Wahlin (Microsoft Most Valuable Professional for ASP.NET and XML Web Services) es un instructor de desarrollo de .NET y consultor de arquitectura en interface Technical Training ([www.interfacett.com)](http://www.interfacett.com). Dan fundado el sitio web de XML para desarrolladores de ASP.NET ([www.XMLforASP.net](http://www.XMLforASP.NET)), se encuentra en la oficina del hablante de INETA y habla en varias conferencias. Dan coautoría profesional Windows DNA (Wrox), ASP.NET: Tips, tutoriales y código (SAMS), ASP.NET 1,1 Insider Solutions, Professional ASP.NET 2,0 AJAX (Wrox), ASP.NET 2,0 MVP y XML creado para desarrolladores de ASP.NET (SAMS). Cuando no está escribiendo código, artículos o libros, Juan disfruta de escribir y grabar música y jugar con golf y baloncesto con su esposa y niños.

Scott Cate ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el Presidente de myKB.com ([www.myKB.com](http://www.myKB.com)), donde se especializa en escribir aplicaciones basadas en ASP.net centradas en las soluciones de software de la base de conocimiento. Puede ponerse en contacto con Scott por correo electrónico en [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-web-services.md)
