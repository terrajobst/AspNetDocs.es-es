---
uid: web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
title: Crear un extensor de control personalizado de AJAX control ToolkitC#() | Microsoft Docs
author: microsoft
description: Los extensores personalizados le permiten personalizar y ampliar las capacidades de los controles ASP.NET sin tener que crear clases nuevas.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 96b56eca-a892-45a4-96b4-67e61178650a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/creating-a-custom-ajax-control-toolkit-control-extender-cs
msc.type: authoredcontent
ms.openlocfilehash: 7850e745f5985688c95fc7f649ccbb06b2f66e20
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78430291"
---
# <a name="creating-a-custom-ajax-control-toolkit-control-extender-c"></a>Crear un extensor de control personalizado de AJAX Control Toolkit (C#)

por [Microsoft](https://github.com/microsoft)

> Los extensores personalizados le permiten personalizar y ampliar las capacidades de los controles ASP.NET sin tener que crear clases nuevas.

En este tutorial, aprenderá a crear un extensor de control personalizado de AJAX control Toolkit. Creamos un extensor sencillo pero útil nuevo que cambia el estado de un botón de deshabilitado a habilitado al escribir texto en un cuadro de texto. Después de leer este tutorial, podrá ampliar el kit de herramientas de ASP.NET AJAX con sus propios extensores de control.

Puede crear extensores de controles personalizados con Visual Studio o Visual Web Developer (Asegúrese de que tiene la versión más reciente de Visual Web Developer).

## <a name="overview-of-the-disabledbutton-extender"></a>Información general sobre el extensor de DisabledButton

Nuestro nuevo extensor de control se denomina DisabledButton extender. Este extensor tendrá tres propiedades:

- TargetControlID: el cuadro de texto que extiende el control.
- TargetButtonIID: el botón que está deshabilitado o habilitado.
- DisabledText: el texto que se muestra inicialmente en el botón. Al empezar a escribir, el botón muestra el valor de la propiedad texto del botón.

El extensor DisabledButton se enlaza a un control de cuadro de texto y botón. Antes de escribir cualquier texto, el botón está deshabilitado y el cuadro de texto y el botón tienen el siguiente aspecto:

[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image2.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image1.png)

([Haga clic para ver la imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image3.png))

Después de empezar a escribir texto, el botón se habilita y el cuadro de texto y el botón tienen el siguiente aspecto:

[![](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image5.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image4.png)

([Haga clic para ver la imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image6.png))

Para crear nuestro extensor de control, es necesario crear los tres archivos siguientes:

- DisabledButtonExtender.cs: este archivo es la clase de control de servidor que administrará la creación del objeto extender y permite establecer las propiedades en tiempo de diseño. También define las propiedades que se pueden establecer en el dispositivo extender. Estas propiedades son accesibles a través del código y en tiempo de diseño y coinciden con las propiedades definidas en el archivo DisableButtonBehavior. js.
- DisabledButtonBehavior. js: este archivo es donde agregará toda la lógica de script de cliente.
- DisabledButtonDesigner.cs: esta clase habilita la funcionalidad en tiempo de diseño. Necesita esta clase si desea que el extensor de control funcione correctamente con el diseñador de Visual Studio o Visual Web Developer.

Por lo tanto, un extensor de control está compuesto de un control de servidor, un comportamiento del lado cliente y una clase de diseñador del lado servidor. Aprenderá a crear los tres archivos en las secciones siguientes.

## <a name="creating-the-custom-extender-website-and-project"></a>Crear el sitio web y el proyecto del extensor personalizado

El primer paso es crear un proyecto de biblioteca de clases y un sitio web en Visual Studio o Visual Web Developer. Se crea el extensor personalizado en el proyecto de biblioteca de clases y se prueba el extensor personalizado en el sitio Web.

Deje que se inicie con el sitio Web. Siga estos pasos para crear el sitio web:

1. Seleccione el archivo de opciones **de menú, nuevo sitio web**.
2. Seleccione la plantilla **sitio Web ASP.net** .
3. Asigne al nuevo sitio web el nombre *Website1*.
4. Haga clic en el botón **Aceptar** .

A continuación, es necesario crear el proyecto de biblioteca de clases que contendrá el código para el extensor del control:

1. Seleccione el archivo de opción de menú **, agregar, nuevo proyecto**.
2. Seleccione la plantilla **biblioteca de clases** .
3. Asigne a la nueva biblioteca de clases el nombre **CustomExtenders**.
4. Haga clic en el botón **Aceptar** .

Después de completar estos pasos, la ventana de Explorador de soluciones debe ser similar a la de la ilustración 1.

[![solución con un sitio web y un proyecto de biblioteca de clases](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image8.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image7.png)

**Figura 01**: solución con el sitio web y el proyecto de biblioteca de clases ([haga clic para ver la imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image9.png))

A continuación, debe agregar todas las referencias de ensamblado necesarias al proyecto de biblioteca de clases:

1. Haga clic con el botón derecho en el proyecto CustomExtenders y seleccione la opción de menú **Agregar referencia**.
2. Seleccione la pestaña .NET.
3. Agregue referencias a los siguientes ensamblados:

    1. System.Web.dll
    2. System.Web.Extensions.dll
    3. System.Design.dll
    4. System.Web.Extensions.Design.dll
4. Seleccione la pestaña examinar.
5. Agregue una referencia al ensamblado AjaxControlToolkit. dll. Este ensamblado se encuentra en la carpeta donde ha descargado el kit de herramientas de control de AJAX.

Después de completar estos pasos, la carpeta de referencias del proyecto de biblioteca de clases debe ser similar a la de la figura 2.

[![carpeta References con referencias requeridas](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image11.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image10.png)

**Figura 02**: carpeta References con las referencias requeridas ([haga clic para ver la imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image12.png))

## <a name="creating-the-custom-control-extender"></a>Crear el extensor del control personalizado

Ahora que tenemos nuestra biblioteca de clases, podemos empezar a crear nuestro control extensor. Permita que s comience con los huesos de una clase de control extensor personalizada (vea la lista 1).

**Lista 1-MyCustomExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample1.cs)]

Hay varios aspectos que se deben tener en cuenta sobre la clase extensor de control en la lista 1. En primer lugar, observe que la clase hereda de la clase base ExtenderControlBase. Todos los controles extensores de AJAX control Toolkit derivan de esta clase base. Por ejemplo, la clase base incluye la propiedad TargetID, que es una propiedad necesaria de cada extensor de control.

A continuación, observe que la clase incluye los dos atributos siguientes relacionados con el script de cliente:

- WebResource: hace que un archivo se incluya como un recurso incrustado en un ensamblado.
- ClientScriptResource: hace que se recupere un recurso de script de un ensamblado.

El atributo WebResource se usa para incrustar el archivo JavaScript de MyControlBehavior. js en el ensamblado cuando se compila el extensor personalizado. El atributo ClientScriptResource se usa para recuperar el script MyControlBehavior. js del ensamblado cuando se usa el extensor personalizado en una página web.

Para que los atributos WebResource y ClientScriptResource funcionen, debe compilar el archivo JavaScript como un recurso incrustado. Seleccione el archivo en la ventana de Explorador de soluciones, abra la hoja de propiedades y asigne el valor de *recurso incrustado* a la propiedad **acción de compilación** .

Observe que el extensor del control también incluye un atributo TargetControlType. Este atributo se usa para especificar el tipo de control que extiende el extensor del control. En el caso de la lista 1, el extensor del control se utiliza para extender un cuadro de texto.

Por último, tenga en cuenta que el extensor personalizado incluye una propiedad denominada propiedad. La propiedad se marca con el atributo ExtenderControlProperty. Los métodos GetPropertyValue () y SetPropertyValue () se utilizan para pasar el valor de propiedad desde el extensor del control del servidor al comportamiento del lado cliente.

Vamos a continuar e implementar el código para nuestro extensor de DisabledButton. El código de este extensor se puede encontrar en la lista 2.

**Lista 2-DisabledButtonExtender.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample2.cs)]

El extensor DisabledButton de la lista 2 tiene dos propiedades denominadas TargetButtonID y DisabledText. El IDReferenceProperty aplicado a la propiedad TargetButtonID impide que se asigne a esta propiedad ningún elemento que no sea el identificador de un control de botón.

Los atributos WebResource y ClientScriptResource asocian un comportamiento del lado cliente ubicado en un archivo denominado DisabledButtonBehavior. js con este extensor. Trataremos este archivo JavaScript en la sección siguiente.

## <a name="creating-the-custom-extender-behavior"></a>Crear el comportamiento del extensor personalizado

El componente del lado cliente de un extensor de control se denomina comportamiento. La lógica real para deshabilitar y habilitar el botón está contenida en el comportamiento de DisabledButton. El código JavaScript para el comportamiento se incluye en la lista 3.

**Lista 3: DisabledButton. js**

[!code-javascript[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample3.js)]

El archivo JavaScript de la lista 3 contiene una clase de cliente denominada DisabledButtonBehavior. Esta clase, como el gemela del lado servidor, incluye dos propiedades denominadas TargetButtonID y DisabledText, a las que se puede tener acceso mediante Get\_TargetButtonID/Set\_TargetButtonID y Get\_DisabledText/Set\_DisabledText.

El método Initialize () asocia un controlador de eventos KeyUp con el elemento de destino para el comportamiento. Cada vez que escribe una letra en el cuadro de texto asociado a este comportamiento, se ejecuta el controlador KeyUp. El controlador KeyUp habilita o deshabilita el botón dependiendo de si el cuadro de texto asociado con el comportamiento contiene cualquier texto.

Recuerde que debe compilar el archivo JavaScript de la lista 3 como un recurso incrustado. Seleccione el archivo en la ventana de Explorador de soluciones, abra la hoja de propiedades y asigne el valor de *recurso incrustado* a la propiedad **acción de compilación** (vea la figura 3). Esta opción está disponible en Visual Studio y Visual Web Developer.

[![agregar un archivo JavaScript como un recurso incrustado](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image14.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image13.png)

**Figura 03**: agregar un archivo de JavaScript como un recurso incrustado ([haga clic para ver la imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image15.png))

## <a name="creating-the-custom-extender-designer"></a>Crear el diseñador extensor personalizado

Hay una última clase que es necesario crear para completar el extensor. Es necesario crear la clase de diseñador en la lista 4. Esta clase es necesaria para que el dispositivo extender se comporte correctamente con el diseñador de Visual Studio o Visual Web Developer.

**Lista 4: DisabledButtonDesigner.cs**

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample4.cs)]

Asocie el diseñador en la lista 4 con el extensor DisabledButton con el atributo Designer. Debe aplicar el atributo de diseñador a la clase DisabledButtonExtender de la siguiente manera:

[!code-csharp[Main](creating-a-custom-ajax-control-toolkit-control-extender-cs/samples/sample5.cs)]

## <a name="using-the-custom-extender"></a>Usar el extensor personalizado

Ahora que hemos terminado de crear el extensor de control DisabledButton, es el momento de usarlo en nuestro sitio web de ASP.NET. En primer lugar, es necesario agregar el extensor personalizado al cuadro de herramientas. Siga estos pasos:

1. Abra una página de ASP.NET haciendo doble clic en la página en la ventana de Explorador de soluciones.
2. Haga clic con el botón derecho en el cuadro de herramientas y seleccione los **elementos**de la opción de menú.
3. En el cuadro de diálogo Elegir elementos del cuadro de herramientas, busque el ensamblado CustomExtenders. dll.
4. Haga clic en el botón **Aceptar** para cerrar el cuadro de diálogo.

Después de completar estos pasos, el extensor del control DisabledButton debe aparecer en el cuadro de herramientas (vea la figura 4).

[![DisabledButton en el cuadro de herramientas](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image17.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image16.png)

**Figura 04**: DisabledButton en el cuadro de herramientas ([haga clic para ver la imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image18.png))

A continuación, es necesario crear una nueva página de ASP.NET. Siga estos pasos:

1. Cree una nueva página de ASP.NET denominada ShowDisabledButton. aspx.
2. Arrastre un ScriptManager hasta la página.
3. Arrastre un control TextBox hasta la página.
4. Arrastre un control de botón hasta la página.
5. En el ventana Propiedades, cambie la propiedad ID. del botón por el valor <em>btnSave</em> y la propiedad Text por el valor *Save\** .

Hemos creado una página con un control de botón y un cuadro de texto de ASP.NET estándar.

A continuación, es necesario extender el control TextBox con el extensor DisabledButton:

1. Seleccione la opción **Agregar tarea extender** para abrir el cuadro de diálogo Asistente para extender (vea la figura 5). Observe que el cuadro de diálogo incluye nuestro extensor personalizado de DisabledButton.
2. Seleccione el extensor DisabledButton y haga clic en el botón **Aceptar** .

[![el cuadro de diálogo Asistente para extender](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image20.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image19.png)

**Figura 05**: cuadro de diálogo del Asistente para extender ([haga clic para ver la imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image21.png))

Por último, podemos establecer las propiedades del extensor DisabledButton. Puede modificar las propiedades del extensor DisabledButton modificando las propiedades del control TextBox:

1. Seleccione el cuadro de texto en el diseñador.
2. En el ventana Propiedades, expanda el nodo Extenders (vea la ilustración 6).
3. Asigne el valor *Save* a la propiedad DisabledText y el valor *btnSave* a la propiedad TargetButtonID.

[![configuración de las propiedades del extensor](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image23.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image22.png)

**Figura 06**: establecimiento de las propiedades del extensor ([haga clic para ver la imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image24.png))

Al ejecutar la página (presionando F5), el control de botón se deshabilita inicialmente. En cuanto empiece a escribir texto en el cuadro de texto, el control de botón estará habilitado (vea la figura 7).

[![el extensor DisabledButton en acción](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image26.png)](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image25.png)

**Figura 07**: el extensor DisabledButton en acción ([haga clic para ver la imagen de tamaño completo](creating-a-custom-ajax-control-toolkit-control-extender-cs/_static/image27.png))

## <a name="summary"></a>Resumen

El objetivo de este tutorial era explicar cómo puede extender el kit de herramientas de control de AJAX con controles extensores personalizados. En este tutorial, creamos un extensor de control DisabledButton simple. Hemos implementado este extensor mediante la creación de una clase DisabledButtonExtender, un comportamiento DisabledButtonBehavior JavaScript y una clase DisabledButtonDesigner. Siga un conjunto de pasos similar cada vez que cree un extensor de control personalizado.

> [!div class="step-by-step"]
> [Anterior](using-ajax-control-toolkit-controls-and-control-extenders-cs.md)
> [Siguiente](get-started-with-the-ajax-control-toolkit-vb.md)
