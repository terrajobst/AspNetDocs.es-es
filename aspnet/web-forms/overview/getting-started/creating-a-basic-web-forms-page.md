---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: Uso de Visual Studio 2013 para crear una página de formularios Web Forms básica de ASP.NET 4,5
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 5d13a51128eecd92a82cfd06054448582a348e11
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78511081"
---
# <a name="using-visual-studio-2013-to-create-a-basic-aspnet-45-web-forms-page"></a>Uso de Visual Studio 2013 para crear una página de formularios Web Forms básica de ASP.NET 4,5

por [Erik Reitan](https://github.com/Erikre)

[!INCLUDE[](~/includes/rp.md)]

En este tutorial se proporciona una introducción al entorno de desarrollo web en [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) y en [Microsoft Visual Studio Express 2013 para web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Este tutorial le guía a través de la creación de una página de formularios Web Forms de ASP.NET sencilla y muestra las técnicas básicas para crear una nueva página, agregar controles y escribir código.

Las tareas ilustradas en este tutorial incluyen:

- Crear un proyecto de aplicación de formularios Web Forms del sistema de archivos.
- Familiarícese con Visual Studio.
- Crear una página de ASP.NET.
- Agregar controles.
- Agregar controladores de eventos.
- Ejecutar y probar una página desde Visual Studio.

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

En esta parte del tutorial, creará un proyecto de aplicación web y le agregará una nueva página. También agregará texto HTML y ejecutará la página en el explorador.

### <a name="to-create-a-web-application-project"></a>Para crear un proyecto de aplicación Web

1. Abra Microsoft Visual Studio.
2. En el menú **Archivo**, seleccione **Nuevo proyecto**.  
    ![menú Archivo](creating-a-basic-web-forms-page/_static/image1.png)

    Aparecerá el cuadro de diálogo **Nuevo proyecto** .
3. Seleccione el grupo **plantillas -&gt;**  **C# visual** -&gt; plantillas **Web** de la izquierda.
4. Elija la plantilla **Aplicación web ASP.NET** en la columna central.
5. Asigne al proyecto el nombre ***BasicWebApp*** y haga clic en el botón **Aceptar** .   
![Cuadro de diálogo Nuevo proyecto](creating-a-basic-web-forms-page/_static/image2.png)
6. A continuación, seleccione la plantilla de **formularios Web Forms** y haga clic en el botón **Aceptar** para crear el proyecto.  
![Cuadro de diálogo New ASP.NET Project](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio crea un nuevo proyecto que incluye funcionalidad pregenerada basada en la plantilla de formularios Web Forms. No solo proporciona una página *Home. aspx* , una página *About. aspx* , una página *Contact. aspx* , sino que también incluye funcionalidad de pertenencia que registra usuarios y guarda sus credenciales para que puedan iniciar sesión en su sitio Web. Cuando se crea una nueva página, Visual Studio muestra de forma predeterminada la página en la vista **código fuente** , donde puede ver los elementos HTML de la página. En la ilustración siguiente se muestra lo que se puede ver en la vista **código fuente** si se ha creado una nueva página web denominada *BasicWebApp. aspx*.  
    ![Vista Código fuente](creating-a-basic-web-forms-page/_static/image4.png)

### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Un paseo por el entorno de desarrollo web de Visual Studio

Antes de continuar modificando la página, resulta útil familiarizarse con el entorno de desarrollo de Visual Studio. En la ilustración siguiente se muestran las ventanas y las herramientas que están disponibles en Visual Studio y Visual Studio Express para Web.

> [!NOTE] 
> 
> Este diagrama muestra las ubicaciones predeterminadas de ventanas y ventanas. El menú **Ver** permite mostrar ventanas adicionales y reorganizar y cambiar el tamaño de las ventanas para que se adapten a sus preferencias. Si ya se han realizado cambios en la organización de ventanas, lo que se ve no coincidirá con la ilustración.

 El entorno de Visual Studio

![Entorno de Visual Studio](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Familiarícese con el Web Designer

Examine la ilustración anterior y haga coincidir el texto con la lista siguiente, donde se describen las ventanas y herramientas que se usan con más frecuencia. (No todas las ventanas y herramientas que vea aparecen aquí, solo las marcadas en la ilustración anterior).

- Barras. Proporcione comandos para dar formato al texto, buscar texto, etc. Algunas barras de herramientas solo están disponibles cuando se trabaja en la vista **diseño** .
- **Explorador de soluciones** ventana. Muestra los archivos y las carpetas de la aplicación Web.
- Ventana de documento. Muestra los documentos en los que se está trabajando en ventanas con pestañas. Puede cambiar entre documentos haciendo clic en pestañas.
- Ventana **propiedades** . Permite cambiar la configuración de la página, los elementos HTML, los controles y otros objetos.
- Ver pestañas. Presente diferentes vistas del mismo documento. La vista de **diseño** es una superficie de edición casi WYSIWYG. Vista **código fuente** es el editor HTML de la página. Vista en **dos paneles** muestra la vista **diseño** y la vista **código fuente** del documento. Trabajará con las vistas **diseño** y **código fuente** más adelante en este tutorial. Si prefiere abrir páginas web en la vista **diseño** , en el menú **herramientas** , haga clic en **Opciones**, seleccione el nodo **Diseñador HTML** y cambie la opción **iniciar páginas en** .
- **Cuadro de herramientas**. Proporciona controles y elementos HTML que puede arrastrar a la página. Los elementos del **cuadro de herramientas** se agrupan por función común.
- Explorador de S **ervidor**. Muestra las conexiones de base de datos. Si Explorador de servidores no está visible, en el menú Ver, haga clic en Explorador de servidores.

### <a name="creating-a-new-aspnet-web-forms-page"></a>Crear una nueva página de formularios Web Forms ASP.NET

Cuando se crea una nueva aplicación de formularios Web Forms mediante la plantilla de proyecto de **aplicación web ASP.net** , Visual Studio agrega una página ASP.net (página de formularios Web Forms) denominada *default. aspx*, así como otros archivos y carpetas. Puede usar la página *default. aspx* como página principal de la aplicación Web. Sin embargo, para este tutorial, creará y trabajará con una nueva página.

### <a name="to-add-a-page-to-the-web-application"></a>Para agregar una página a la aplicación Web

1. Cierre la página *default. aspx* . Para ello, haga clic en la pestaña que muestra el nombre de archivo y, a continuación, haga clic en la opción cerrar.
2. En **Explorador de soluciones**, haga clic con el botón secundario en el nombre de la aplicación web (en este tutorial, el nombre de la aplicación es **BasicWebSite**) y, a continuación, haga clic en **Agregar** -&gt; **nuevo elemento**.   
Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
3. Seleccione el grupo plantillas **Web** de **Visual C#**  -&gt; a la izquierda. A continuación, seleccione **Web Forms** en la lista central y asígnele el nombre *FirstWebPage. aspx*.   
    ![Cuadro de diálogo Agregar nuevo elemento](creating-a-basic-web-forms-page/_static/image6.png)
4. Haga clic en **Agregar** para agregar la página web al proyecto.  
Visual Studio crea la nueva página y la abre.

### <a name="adding-html-to-the-page"></a>Agregar HTML a la página

En esta parte del tutorial, agregará texto estático a la página.

### <a name="to-add-text-to-the-page"></a>Para agregar texto a la página

1. En la parte inferior de la ventana de documento, haga clic en la pestaña **diseño** para cambiar a la vista **diseño** .

    Vista de diseño muestra la página actual en modo WYSIWYG. En este momento, no tiene ningún texto o control en la página, por lo que la página está en blanco excepto una línea discontinua que describe un rectángulo. Este rectángulo representa un elemento **div** en la página.
2. Haga clic dentro del rectángulo que se indica mediante una línea discontinua.
3. Escriba **Bienvenido a Visual Web Developer** y presione **entrar** dos veces.

    En la ilustración siguiente se muestra el texto que escribió en la vista **diseño** .

    ![Texto de bienvenida en Vista de diseño](creating-a-basic-web-forms-page/_static/image7.png "Texto de bienvenida de la vista Diseño")
4. Cambie a la vista **código fuente** .

    Puede ver el código HTML en la vista **código fuente** que creó cuando escribió en la vista **diseño** .  
    ![página web con](creating-a-basic-web-forms-page/_static/image8.png) de texto estático

### <a name="running-the-page"></a>Ejecutar la página

Antes de continuar agregando controles a la página, puede ejecutarlo en primer lugar.

### <a name="to-run-the-page"></a>Para ejecutar la página

1. En **Explorador de soluciones**, haga clic con el botón secundario en *FirstWebPage. aspx* y seleccione **establecer como página de inicio**.
2. Presione **Ctrl + F5** para ejecutar la página.

    La página se muestra en el explorador. Aunque la página creada tiene una extensión de nombre de archivo *. aspx*, actualmente se ejecuta como cualquier página HTML.

    Para mostrar una página en el explorador, también puede hacer clic con el botón derecho en la página en **Explorador de soluciones** y seleccionar **ver en el explorador**.
3. Cierre el explorador para detener la aplicación Web.

## <a name="adding-and-programming-controls"></a>Agregar y programar controles

<a id="sectionToggle1"></a>

Ahora agregará controles de servidor a la página. Los controles de servidor, como botones, etiquetas, cuadros de texto y otros controles conocidos, proporcionan capacidades de procesamiento de formularios típicas para las páginas de formularios Web Forms. Sin embargo, puede programar los controles con el código que se ejecuta en el servidor, en lugar del cliente.

Agregará un control de [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , un control de [cuadro de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) y un control de [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) a la página y escribirá código para controlar el evento de [clic](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) del control de [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

### <a name="to-add-controls-to-the-page"></a>Para agregar controles a la página

1. Haga clic en la pestaña **diseño** para cambiar a la vista **diseño** .
2. Coloque el punto de inserción al final del texto **Bienvenido a Visual Web Developer** y presione **entrar** cinco o más veces para crear algo de espacio en el cuadro del elemento **div** .
3. En el **cuadro de herramientas**, expanda el grupo **estándar** si aún no está expandido.  
Tenga en cuenta que es posible que tenga que expandir la ventana **cuadro de herramientas** de la izquierda para verla.
4. Arrastre un control [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) a la página y colóquelo en el centro del cuadro del elemento **div** que tiene **Visual Web Developer** en la primera línea.
5. Arrastre un control de [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) hasta la página y colóquelo a la derecha del control de [cuadro de texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) .
6. Arrastre un control [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) a la página y colóquelo en una línea independiente debajo del control [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .
7. Coloque el punto de inserción sobre el control [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) y, a continuación, escriba **Escriba su nombre:** .

    Este texto HTML estático es el título del control de [cuadro](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) de texto. Puede mezclar controles HTML y de servidor estáticos en la misma página. En la ilustración siguiente se muestra cómo aparecen los tres controles en la vista **diseño** .

    ![Tres controles en Vista de diseño](creating-a-basic-web-forms-page/_static/image9.png "Tres controles de la vista Diseño")

### <a name="setting-control-properties"></a>Establecer propiedades de controles

Visual Studio ofrece varias maneras de establecer las propiedades de los controles en la página. En esta parte del tutorial, establecerá las propiedades en la vista de **diseño** y la vista de **código fuente** .

### <a name="to-set-control-properties"></a>Para establecer las propiedades del control

1. En primer lugar, muestre las ventanas **propiedades** . para ello, seleccione en el menú **Ver** ,&gt; **ventana Propiedades**de **Windows** -&gt;. Como alternativa, puede seleccionar **F4** para mostrar la ventana **propiedades** .
2. Seleccione el control de [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) y, a continuación, en la ventana **propiedades** , establezca el valor de **texto** en **nombre para mostrar**. El texto que ha escrito aparece en el botón del diseñador, tal como se muestra en la siguiente ilustración.

    ![Establecer texto del botón](creating-a-basic-web-forms-page/_static/image10.png "Establecer texto del botón")
3. Cambie a la vista **código fuente** .

    Vista **código fuente** muestra el código HTML de la página, incluidos los elementos que Visual Studio ha creado para los controles de servidor. Los controles se declaran utilizando la sintaxis de estilo HTML, con la excepción de que las etiquetas utilizan el prefijo **asp:** e incluyen el atributo **runat =&quot;Server&quot;** .

    Las propiedades del control se declaran como atributos. Por ejemplo, al establecer la propiedad [texto](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx) para el control de [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , en el paso 1, realmente estaba estableciendo el atributo de **texto** en el marcado del control.

    > [!NOTE] 
    > 
    > Todos los controles están dentro de un elemento **Form** , que también tiene el atributo **runat =&quot;Server&quot;** . El atributo **runat =&quot;server&quot;** y el prefijo **asp:** para las etiquetas de control marcan los controles para que los procese ASP.net en el servidor cuando se ejecuta la página. El código fuera de **&lt;formulario runat =&quot;server&quot;&gt;** y **&lt;script runat =&quot;Server**&quot;&gt;elementos se envía sin cambios al explorador, por lo que el código ASP.net debe estar dentro de un elemento cuya etiqueta de apertura contenga el atributo **&quot;del servidor runat =&quot;** .
4. A continuación, agregará una propiedad adicional al control [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Coloque el punto de inserción directamente después de **asp: etiqueta** en la etiqueta **&lt;asp: Label&gt;** y presione la **barra espaciadora**.

    Aparece una lista desplegable que muestra la lista de propiedades disponibles que puede establecer para un control [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Esta característica, denominada **IntelliSense**, le ayuda en la vista **código fuente** con la sintaxis de los controles de servidor, los elementos HTML y otros elementos de la página. En la ilustración siguiente se muestra la lista desplegable **IntelliSense** para el control [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

    ![Atributos de IntelliSense](creating-a-basic-web-forms-page/_static/image11.png "Atributos de IntelliSense")
5. Seleccione **ForeColor** y escriba un signo igual.

    IntelliSense muestra una lista de colores.

    > [!NOTE] 
    > 
    > Puede mostrar una lista desplegable de **IntelliSense** en cualquier momento presionando **Ctrl + J** al ver el código.
6. Seleccione un color para el texto del control de **[etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** . Asegúrese de seleccionar un color que sea lo suficientemente oscuro como para leer en un fondo blanco.

    El atributo **ForeColor** se completa con el color que ha seleccionado, incluida la comilla de cierre.

### <a name="programming-the-button-control"></a>Programar el control de botón

En este tutorial, escribirá código que lee el nombre que el usuario escribe en el cuadro de texto y, a continuación, muestra el nombre en el control [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .

### <a name="add-a-default-button-event-handler"></a>Agregar un controlador de eventos de botón predeterminado

1. Cambie a la vista **diseño** .
2. Haga doble clic en el control de [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) .

    De forma predeterminada, Visual Studio cambia a un archivo de código subyacente y crea un esqueleto de controlador de eventos para el evento predeterminado del control [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) , el evento [click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) . El archivo de código subyacente separa el marcado de la interfaz de usuario (por ejemplo, HTML) del código del C#servidor (como).   
   El cursor se coloca en el código agregado para este controlador de eventos.

    > [!NOTE] 
    > 
    > Hacer doble clic en un control en la vista **diseño** es solo una de las diversas formas en que puede crear controladores de eventos.
3. Dentro de **Button1\_** controlador de eventos Click, escriba **Label1** seguido de un punto ( **.** ).

    Al escribir el punto después del **identificador** de la etiqueta (**Label1**), Visual Studio muestra una lista de los miembros disponibles para el control [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) , como se muestra en la siguiente ilustración. Un miembro normalmente es una propiedad, un método o un evento.

    ![IntelliSense en la vista Código](creating-a-basic-web-forms-page/_static/image12.png "IntelliSense en la Vista de código")
4. Finalice el controlador de eventos **click** del botón para que lea como se muestra en el ejemplo de código siguiente.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. Vuelva a ver la vista de **código fuente** del código HTML; para ello, haga clic con el botón secundario en *FirstWebPage. aspx* en el **Explorador de soluciones** y seleccione **Ver marcado**.
6. Desplácese hasta el elemento **&lt;ASP: Button&gt;** . Tenga en cuenta que el elemento **&lt;ASP: Button&gt;** tiene ahora el atributo **onclick =&quot;button1\_haga clic en&quot;** .

    Este atributo enlaza el evento [click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) del botón al método de control que se ha codificado en el paso anterior.

    Los métodos de controlador de eventos pueden tener cualquier nombre; el nombre que se ve es el nombre predeterminado creado por Visual Studio. Lo importante es que el nombre que se usa para el atributo **onclick** en el código HTML debe coincidir con el nombre de un método definido en el código subyacente.

### <a name="running-the-page"></a>Ejecutar la página

Ahora puede probar los controles de servidor en la página.

### <a name="to-run-the-page"></a>Para ejecutar la página

1. Presione **Ctrl + F5** para ejecutar la página en el explorador. Si se produce un error, revise los pasos anteriores.
2. Escriba un nombre en el cuadro de texto y haga clic en el botón **nombre para mostrar** .

    El nombre especificado se muestra en el control [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) . Tenga en cuenta que al hacer clic en el botón, la página se envía al servidor Web. A continuación, ASP.NET vuelve a crear la página, ejecuta el código (en este caso, se ejecuta el controlador de eventos [click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) del control de [botón](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) ) y, a continuación, envía la nueva página al explorador. Si ve la barra de estado en el explorador, puede ver que la página está realizando un recorrido de ida y vuelta al servidor web cada vez que hace clic en el botón.
3. En el explorador, vea el origen de la página que se está ejecutando; para ello, haga clic con el botón derecho en la página y seleccione **Ver código fuente**.

    En el código fuente de la página, verá HTML sin ningún código de servidor. En concreto, no verá los elementos **&lt;ASP:&gt;** con los que estaba trabajando en la vista **código fuente** . Cuando se ejecuta la página, ASP.NET procesa los controles de servidor y representa los elementos HTML en la página que realiza las funciones que representan el control. Por ejemplo, el control **&lt;ASP: Button&gt;** se representa como el elemento HTML **&lt;input type =&quot;Submit&quot;** &gt;.
4. Cierre el explorador.

## <a name="working-with-additional-controls"></a>Trabajar con controles adicionales

<a id="sectionToggle2"></a>

En esta parte del tutorial, trabajará con el control de [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) , que muestra las fechas de un mes a la vez. El control de [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) es un control más complejo que el botón, el cuadro de texto y la etiqueta con la que ha estado trabajando y muestra algunas otras funciones de los controles de servidor.

En esta sección, agregará un control [System. Web. UI. WebControls. Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) a la página y le dará formato.

### <a name="to-add-a-calendar-control"></a>Para agregar un control de calendario

1. En Visual Studio, cambie a la vista **diseño** .
2. En la sección **estándar** del **cuadro de herramientas**, arrastre un control [Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) hasta la página y colóquelo debajo del elemento **div** que contiene los demás controles.

    Se muestra el panel de etiquetas inteligentes del calendario. El panel muestra comandos que facilitan la realización de las tareas más comunes para el control seleccionado. En la ilustración siguiente se muestra el control de [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) tal y como se representa en la vista **diseño** .

    ![Control de calendario en Vista de diseño](creating-a-basic-web-forms-page/_static/image13.png "Control Calendar de la Vista Diseño")
3. En el panel de etiquetas inteligentes, elija **formato automático**.

    Se muestra el cuadro de diálogo **formato automático** , que le permite seleccionar un esquema de formato para el calendario. En la ilustración siguiente se muestra el cuadro de diálogo **formato automático** para el control de [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    ![Formato automático (cuadro de diálogo, control Calendar)](creating-a-basic-web-forms-page/_static/image14.png "Cuadro de diálogo Formato automático (control Calendar)")
4. En la lista **seleccionar un esquema** , seleccione **simple** y haga clic en **Aceptar**.
5. Cambie a la vista **código fuente** .

    Puede ver el elemento **&lt;ASP: Calendar&gt;** . Este elemento es mucho más largo que los elementos de los controles simples que creó anteriormente. También incluye subelementos, como **&lt;WeekEndDayStyle&gt;** , que representan diversos valores de formato. En la ilustración siguiente se muestra el control de [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) en la vista **código fuente** . (El marcado exacto que se ve en la vista **código fuente** podría diferir ligeramente de la ilustración).

    ![Control de calendario en la vista Código fuente](creating-a-basic-web-forms-page/_static/image15.png "Control Calendar de la vista Código fuente")

### <a name="programming-the-calendar-control"></a>Programar el control de calendario

En esta sección, programará el control de [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) para mostrar la fecha seleccionada actualmente.

### <a name="to-program-the-calendar-control"></a>Para programar el control de calendario

1. En la vista **diseño** , haga doble clic en el control de [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) .

    Se crea un nuevo controlador de eventos y se muestra en el archivo de código subyacente denominado *FirstWebPage.aspx.CS*.
2. Finalice el controlador de eventos [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx) con el código siguiente.

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    El código anterior establece el texto del control etiqueta en la fecha seleccionada del control de calendario.

### <a name="running-the-page"></a>Ejecutar la página

Ahora puede probar el calendario.

### <a name="to-run-the-page"></a>Para ejecutar la página

1. Presione **Ctrl + F5** para ejecutar la página en el explorador.
2. Haga clic en una fecha en el calendario.

    La fecha en la que hizo clic se muestra en el control [etiqueta](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx) .
3. En el explorador, vea el código fuente de la página.

    Tenga en cuenta que el control de [calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx) se ha representado en la página como una **tabla**, donde cada día es un elemento **TD** .
4. Cierre el explorador.

## <a name="next-steps"></a>Pasos siguientes

<a id="nextStepsToggle"></a>

En este tutorial se han ilustrado las características básicas del diseñador de páginas de Visual Studio. Ahora que sabe cómo crear y editar una página de formularios Web Forms en Visual Studio, puede que desee explorar otras características. Por ejemplo, puede que desee hacer lo siguiente:

- Para obtener más información acerca de los formularios Web Forms de ASP.NET, siga la serie de tutoriales paso a paso [Introducción con ASP.NET 4,5 Web Forms y Visual Studio 2013](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md).
- Más información sobre las hojas de estilos en cascada (CSS). Para obtener más información, vea el [tema sobre cómo trabajar con CSS](https://msdn.microsoft.com/library/bb398931.aspx).
