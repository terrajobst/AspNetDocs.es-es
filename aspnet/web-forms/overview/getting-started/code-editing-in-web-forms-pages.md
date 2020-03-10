---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Edición de código ASP.NET de formularios Web Forms en Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 3473ad476fbbebc58e12586334b4600f57cf17ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78513643"
---
# <a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Edición del código de formularios Web Forms de ASP.NET en Visual Studio 2013

por [Erik Reitan](https://github.com/Erikre)

En muchas páginas de formularios Web Forms de ASP.NET, se escribe C#código en Visual Basic, u otro lenguaje. El editor de código de Visual Studio puede ayudarle a escribir código rápidamente mientras ayuda a evitar errores. Además, el editor proporciona maneras de crear código reutilizable para ayudar a reducir la cantidad de trabajo que debe realizar.

En este tutorial se muestran varias características del editor de código de Visual Studio.

Durante este tutorial aprenderá a:

- Corrija los errores de codificación en línea.
- Refactorizar y cambiar el nombre del código.
- Cambiar el nombre de variables y objetos.
- Inserte fragmentos de código.

## <a name="prerequisites"></a>Requisitos previos

Para poder completar este tutorial, necesitará:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 para web](https://www.microsoft.com/visualstudio/11/downloads#express-web). El .NET Framework se instala automáticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 y Microsoft Visual Studio Express 2013 para Web, a menudo se denominarán Visual Studio en esta serie de tutoriales.  
    >   
    > Si usa Visual Studio, en este tutorial se supone que ha seleccionado la colección de configuraciones de **desarrollo web** la primera vez que inicia Visual Studio. Para obtener más información, consulte [Cómo: seleccionar la configuración del entorno de desarrollo web](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Crear un proyecto de aplicación web y una página

<a id="sectionToggle0"></a>

En esta parte del tutorial, creará un proyecto de aplicación web y le agregará una nueva página.

### <a name="to-create-a-web-application-project"></a>Para crear un proyecto de aplicación Web

1. Abra Microsoft Visual Studio.
2. En el menú **Archivo**, seleccione **Nuevo proyecto**.  
    ![menú Archivo](code-editing-in-web-forms-pages/_static/image1.png)

    Aparecerá el cuadro de diálogo **Nuevo proyecto** .
3. Seleccione el grupo **plantillas -&gt;**  **C# visual** -&gt; plantillas **Web** de la izquierda.
4. Elija la plantilla **Aplicación web ASP.NET** en la columna central.
5. Asigne al proyecto el nombre ***BasicWebApp*** y haga clic en el botón **Aceptar** .   
![Cuadro de diálogo Nuevo proyecto](code-editing-in-web-forms-pages/_static/image2.png)
6. A continuación, seleccione la plantilla de **formularios Web Forms** y haga clic en el botón **Aceptar** para crear el proyecto.  
![Cuadro de diálogo New ASP.NET Project](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio crea un nuevo proyecto que incluye funcionalidad pregenerada basada en la plantilla de formularios Web Forms.

## <a name="creating-a-new-aspnet-web-forms-page"></a>Crear una nueva página de formularios Web Forms ASP.NET

Cuando se crea una nueva aplicación de formularios Web Forms mediante la plantilla de proyecto de **aplicación web ASP.net** , Visual Studio agrega una página ASP.net (página de formularios Web Forms) denominada *default. aspx*, así como otros archivos y carpetas. Puede usar la página *default. aspx* como página principal de la aplicación Web. Sin embargo, para este tutorial, creará y trabajará con una nueva página.

### <a name="to-add-a-page-to-the-web-application"></a>Para agregar una página a la aplicación Web

1. En **Explorador de soluciones**, haga clic con el botón secundario en el nombre de la aplicación web (en este tutorial, el nombre de la aplicación es **BasicWebSite**) y, a continuación, haga clic en **Agregar** -&gt; **nuevo elemento**.   
Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el grupo plantillas **Web** de **Visual C#**  -&gt; a la izquierda. A continuación, seleccione **Web Forms** en la lista central y asígnele el nombre *FirstWebPage. aspx*.   
    ![Cuadro de diálogo Agregar nuevo elemento](code-editing-in-web-forms-pages/_static/image4.png)
3. Haga clic en **Agregar** para agregar la página de formularios Web Forms al proyecto.  
 Visual Studio crea la nueva página y la abre.
4. A continuación, establezca esta nueva página como la página de inicio predeterminada. En **Explorador de soluciones**, haga clic con el botón secundario en la nueva página denominada *FirstWebPage. aspx* y seleccione **establecer como página de inicio**. La próxima vez que ejecute esta aplicación para probar nuestro progreso, verá automáticamente esta nueva página en el explorador.

## <a name="correcting-inline-coding-errors"></a>Corrección de errores de codificación en línea

El editor de código de Visual Studio le ayuda a evitar errores mientras escribe código y, si ha realizado un error, el editor de código le ayuda a corregir el error. En esta parte del tutorial, escribirá una línea de código que muestra las características de corrección de errores en el editor.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Para corregir errores de codificación simples en Visual Studio

1. En la vista **diseño** , haga doble clic en la página en blanco para crear un controlador para el evento de **carga** de la página.   
   Solo se usa el controlador de eventos como un lugar para escribir código.
2. Dentro del controlador, escriba la siguiente línea que contiene un error y presione **entrar**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Al presionar **entrar**, el editor de código coloca el subrayado verde y rojo (normalmente, se llama &quot;&quot; líneas) en las áreas del código que tienen problemas. Un subrayado verde indica una advertencia. Un subrayado rojo indica un error que debe corregir. 

    Mantenga el puntero del mouse sobre `myStr` para ver una información sobre herramientas que le indica la advertencia. Además, mantenga presionado el puntero del mouse sobre el subrayado rojo para ver el mensaje de error.

    En la imagen siguiente se muestra el código con el subrayado.

    ![Texto de bienvenida en Vista de diseño](code-editing-in-web-forms-pages/_static/image5.png "Texto de bienvenida de la vista Diseño")  
   El error se debe corregir agregando un punto y coma `;` al final de la línea. La advertencia simplemente le informa de que aún no ha usado la variable `myStr`.  

    > [!NOTE] 
    > 
    > Para ver la configuración actual de formato de código en Visual Studio, seleccione **herramientas** -&gt; **Opciones** -&gt; **fuentes y colores**.

## <a name="refactoring-and-renaming"></a>Refactorización y cambio de nombre

La refactorización es una metodología de software que implica la reestructuración del código para que sea más fácil de entender y mantener, a la vez que conserva su funcionalidad. Un ejemplo sencillo podría ser que escriba código en un controlador de eventos para obtener datos de una base de datos. A medida que desarrolla la página, descubre que necesita acceder a los datos desde varios controladores diferentes. Por lo tanto, se refactoriza el código de la página creando un método de acceso a datos en la página e insertando llamadas al método en los controladores.

El editor de código incluye herramientas que ayudan a realizar diversas tareas de refactorización. En este tutorial, trabajará con dos técnicas de refactorización: cambiar el nombre de las variables y extraer métodos. Otras opciones de refactorización son encapsular campos, promover variables locales a parámetros de método y administrar parámetros de método. La disponibilidad de estas opciones de refactorización depende de la ubicación en el código.

### <a name="refactoring-code"></a>Refactorizar código

Un escenario de refactorización común consiste en crear (extraer) un método a partir del código que se encuentra dentro de otro miembro, como un método. Esto reduce el tamaño del miembro original y hace que el código extraído se pueda volver a usar.

En esta parte del tutorial, escribirá un código simple y, a continuación, extraerá un método de él. La refactorización es compatible C#con, por lo que creará una página C# que usa como lenguaje de programación.

### <a name="to-extract-a-method-in-a-c-page"></a>Para extraer un método en una C# página

1. Cambie a la vista **diseño** .
2. En el **cuadro de herramientas**, en la pestaña **estándar** , arrastre un control de [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) hasta la página.
3. Haga doble clic en el control de **botón** para crear un controlador para el evento [click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) y, a continuación, agregue el siguiente código resaltado:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   El código crea un objeto **ArrayList** , usa un bucle para cargarlo con valores y, a continuación, usa otro bucle para mostrar el contenido del objeto **ArrayList** .
4. Presione **Ctrl + F5** para ejecutar la página y, a continuación, haga clic en el **botón** para asegurarse de que ve el siguiente resultado:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Vuelva al editor de código y, a continuación, seleccione las líneas siguientes en el controlador de eventos.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Haga clic con el botón secundario en la selección, haga clic en **refactorizar**y, a continuación, elija **Extraer método**. 

    Aparecerá el cuadro de diálogo **Extraer método** .
7. En el cuadro **nombre del método nuevo** , escriba **DisplayArray**y, a continuación, haga clic en **Aceptar**. 

    El editor de código crea un nuevo método denominado `DisplayArray`y coloca una llamada al nuevo método en el controlador de **clics** en el que el bucle se encontraba originalmente.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Presione **Ctrl + F5** para volver a ejecutar la página y haga clic en el **botón**.

    La página funciona igual que antes. Ahora se puede llamar al método `DisplayArray` desde cualquier parte de la clase Page.

## <a name="renaming-variables"></a>Cambiar el nombre de variables

Al trabajar con variables, así como con objetos, es posible que desee cambiarles el nombre después de que ya se haga referencia a ellas en el código. Sin embargo, el cambio de nombre de variables y objetos puede hacer que el código se interrumpa si se pierde el cambio de nombre de una de las referencias. Por lo tanto, puede usar la refactorización para realizar el cambio de nombre.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Para usar la refactorización para cambiar el nombre de una variable

1. En el controlador de eventos **click** , busque la línea siguiente:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Haga clic con el botón secundario en el nombre de la variable `alist`, elija **refactorizar**y, a continuación, elija **cambiar nombre**.

    Aparece el cuadro de diálogo **cambiar nombre** .
3. En el cuadro **nombre nuevo** , escriba **ArrayList1** y asegúrese de que se ha seleccionado la casilla **vista previa de los cambios de referencia** . A continuación, haga clic en **Aceptar**.

    Aparece el cuadro de diálogo **vista previa de los cambios** y muestra un árbol que contiene todas las referencias a la variable cuyo nombre se está cambiando.
4. Haga clic en **aplicar** para cerrar el cuadro de diálogo **vista previa de cambios** .

    Se cambia el nombre de las variables que hacen referencia específicamente a la instancia seleccionada. No obstante, tenga en cuenta que no se cambia el nombre de la variable `alist` en la línea siguiente.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    No se cambia el nombre de la variable `alist` en esta línea porque no representa el mismo valor que la variable `alist` que cambió de nombre. La variable `alist` en la declaración de `DisplayArray` es una variable local para ese método. Esto muestra que el uso de la refactorización para cambiar el nombre de las variables es diferente que simplemente realizar una acción de buscar y reemplazar en el editor; la refactorización cambia el nombre de las variables con conocimiento de la semántica de la variable con la que está trabajando.

## <a name="inserting-snippets"></a>Insertar fragmentos de código

Dado que hay muchas tareas de codificación que los desarrolladores de formularios Web Forms suelen necesitar realizar, el editor de código proporciona una biblioteca de fragmentos de código o bloques de código escrito previamente. Puede insertar estos fragmentos de código en la página.

Cada lenguaje que se usa en Visual Studio tiene ligeras diferencias en la forma de insertar fragmentos de código. Para obtener información sobre la inserción de fragmentos de [código, vea Visual Basic fragmentos de código de IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx). Para obtener información sobre cómo insertar fragmentos de código C#en visual, vea [fragmentos de código Visual C# ](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Pasos siguientes

En este tutorial se han ilustrado las características básicas del editor de código de Visual Studio 2010 para corregir errores en el código, refactorizar el código, cambiar el nombre de las variables e insertar fragmentos de código en el código. Las características adicionales del editor pueden agilizar y simplificar el desarrollo de aplicaciones. Por ejemplo, puedes:

- Obtenga más información sobre las características de IntelliSense, como modificar opciones de IntelliSense, administrar fragmentos de código y buscar fragmentos de código en línea. Para obtener más información, vea [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Aprenda a crear sus propios fragmentos de código. Para obtener más información, vea [crear y usar fragmentos de código de IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx) .
- Obtenga más información sobre las características específicas de Visual Basic de los fragmentos de código de IntelliSense, como la personalización de los fragmentos de código y la solución de problemas. Para obtener más información, vea [Visual Basic fragmentos de código de IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx) .
- Obtenga más información sobre C#las características específicas de IntelliSense, como la refactorización y los fragmentos de código. Para obtener más información, [vea C# visual IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
