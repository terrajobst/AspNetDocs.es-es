---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: ¿Cómo usar el control ComboBox? (VB) | Microsoft Docs
author: microsoft
description: ComboBox es un control de AJAX de ASP.NET que combina la flexibilidad de un cuadro de texto con una lista de opciones entre las que los usuarios pueden elegir.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 468063a72253cce55a02bfaef1219bff03d06418
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446575"
---
# <a name="how-do-i-use-the-combobox-control-vb"></a>¿Cómo usar el control ComboBox? (VB)

por [Microsoft](https://github.com/microsoft)

> ComboBox es un control de AJAX de ASP.NET que combina la flexibilidad de un cuadro de texto con una lista de opciones entre las que los usuarios pueden elegir.

El objetivo de este tutorial es explicar el control ComboBox de AJAX control Toolkit. ComboBox funciona como una combinación entre un control de DropDownList de ASP.NET estándar y un control de cuadro de texto. Puede seleccionar de una lista de elementos preexistente o escribir un nuevo elemento.

ComboBox es similar al extensor del control Autocompletar, pero los controles se usan en escenarios diferentes. El extensor Autocompletar consulta un servicio web para obtener entradas coincidentes. En cambio, el control ComboBox se inicializa con un conjunto de elementos. Usar el extensor Autocompletar tiene sentido cuando se trabaja con un conjunto grande de datos (millones de partes del automóvil) mientras se usa el control ComboBox tiene sentido cuando se trabaja con un pequeño conjunto de datos (docenas de piezas del automóvil).

## <a name="selecting-from-a-static-list-of-items"></a>Seleccionar en una lista estática de elementos

Comencemos con un ejemplo sencillo del uso del control ComboBox. Imagine que desea mostrar una lista estática de elementos en una lista desplegable. Sin embargo, desea dejar abierta la posibilidad de que la lista no esté completa. Desea permitir que un usuario escriba un valor personalizado en la lista.

Se crea una nueva página de formularios Web Forms ASP.NET y se usa el control ComboBox en la página. Agregue la nueva página ASP.NET al proyecto y cambie a Vista de diseño.

Si desea usar el control ComboBox en la página, debe agregar un control ScriptManager a la página. Arrastre el control ScriptManager desde debajo de la pestaña extensiones AJAX hasta la superficie del diseñador. Debe agregar el control ScriptManager en la parte superior de la página; puede agregarla inmediatamente debajo de la etiqueta&gt; formulario de &lt;del lado servidor.

A continuación, arrastre el control ComboBox hasta la página. Puede encontrar el control ComboBox en el cuadro de herramientas con los demás controles de AJAX control Toolkit y los extensores de control (vea Figura 1).

[![forma sencilla para crear una tarjeta de presentación](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Figura 01**: selección del control ComboBox desde el cuadro de herramientas ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image2.png))

El control ComboBox se usa para mostrar una lista estática de opciones. El usuario puede seleccionar un nivel determinado de picante para su comida en una lista de tres opciones: leve, medio y activo (consulte la figura 2).

[![seleccionar de una lista estática de elementos](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Figura 02**: selección de una lista estática de elementos ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image4.png))

Hay dos maneras de agregar estas opciones al control ComboBox. En primer lugar, seleccione la opción de la tarea Editar opciones al mantener el mouse sobre el control en Vista de diseño y abrir el editor de elementos (vea la figura 3).

[![edición de elementos ComboBox](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Figura 03**: edición de elementos de cuadro combinado ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image6.png))

La segunda opción consiste en agregar la lista de elementos entre las etiquetas de apertura y cierre &lt;ASP: ComboBox&gt; en la vista Código fuente. La página de la lista 1 contiene el cuadro combinado actualizado que tiene la lista de elementos.

**Listado 1: Static. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Al abrir la página en la lista 1, puede seleccionar una de las opciones preexistentes en el cuadro combinado. En otras palabras, el ComboBox funciona igual que un control DropDownList.

Sin embargo, también tiene la opción de escribir una nueva opción (por ejemplo, un súper picante) que no esté en la lista existente. Por lo tanto, el cuadro combinado también funciona como un control TextBox.

Independientemente de si elige un elemento ya existente o especifica un elemento personalizado, al enviar el formulario, su elección aparece en el control etiqueta. Cuando se envía el formulario, el controlador de btnSubmit\_clic en ejecuta y actualiza la etiqueta (vea la figura 4).

[![mostrar el elemento seleccionado](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Figura 04**: mostrar el elemento seleccionado ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image8.png))

ComboBox admite las mismas propiedades que el control DropDownList para recuperar el elemento seleccionado después de enviar un formulario:

- SelectedItem. Text: muestra el valor de la propiedad texto del elemento seleccionado.
- SelectedItem. Value: muestra el valor de la propiedad Value del elemento seleccionado o muestra el texto escrito en el ComboBox.
- SelectedValue: igual que SelectedItem. Value, salvo que esta propiedad permite especificar el elemento seleccionado (inicial) predeterminado.

Si escribe una opción personalizada en el cuadro combinado, se asignará la opción personalizada a las propiedades SelectedItem. Text y SelectedItem. Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Seleccionar la lista de elementos de la base de datos

Puede recuperar la lista de elementos que el cuadro combinado muestra de una base de datos. Por ejemplo, puede enlazar el cuadro combinado a un control SqlDataSource, un control ObjectDataSource, un LinqDataSource o un EntityDataSource.

Imagine que desea que se muestre una lista de películas en un cuadro combinado. Desea recuperar la lista de películas de la tabla de la base de datos de películas. Siga estos pasos:

1. Crear una página denominada movies. aspx
2. Agregue un control ScriptManager a la página arrastrando el ScriptManager desde la pestaña extensiones AJAX del cuadro de herramientas a la página.
3. Agregue un control ComboBox a la página arrastrando el control ComboBox hasta la página.
4. En Vista de diseño, mantenga el mouse sobre el control ComboBox y seleccione la opción de tarea **elegir origen de datos** (vea la figura 5). Se inicia el Asistente para la configuración de orígenes de datos.
5. En el paso **elegir un origen de datos** , seleccione la opción &lt;nuevo origen de datos&gt;.
6. En el paso **elegir un tipo de origen de datos** , seleccione base de datos.
7. En el paso **elegir la conexión de datos** , seleccione la base de datos (por ejemplo, MoviesDB. MDF).
8. En el paso **guardar la cadena de conexión en el archivo de configuración de la aplicación** , seleccione la opción para guardar la cadena de conexión.
9. En el paso **configurar la instrucción SELECT** , seleccione la tabla de la base de datos películas y seleccione todas las columnas.
10. En el paso **consulta de prueba** , haga clic en el botón Finalizar.
11. De nuevo en el paso **elegir origen de datos** , seleccione la columna título para el campo que se va a mostrar y la columna ID para el campo de datos (vea la ilustración).
12. Haga clic en el botón Aceptar para cerrar el asistente.

[![elegir un origen de datos](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Figura 05**: elegir un origen de datos ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image10.png))

[![elegir los campos texto de datos y valor](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Figura 06**: selección de los campos texto de datos y valor ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image12.png))

Después de completar los pasos anteriores, el cuadro combinado se enlaza a un control SqlDataSource que representa las películas de la tabla de base de datos de películas. El origen de la página es similar al de la lista 2 (he limpiado el formato un poco).

**Lista 2-movies. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Observe que el control ComboBox tiene una propiedad DataSourceID que señala al control SqlDataSource. Al abrir la página en un explorador, se muestra la lista de películas de la base de datos (consulte la figura 7). Puede seleccionar una película de la lista o escribir una nueva película escribiendo la película en el cuadro combinado.

[![de mostrar una lista de películas](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Figura 07**: mostrar una lista de películas ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image14.png))

## <a name="setting-the-dropdownstyle"></a>Establecer el valor de DropDownStyle

Puede usar la propiedad de ComboBox DropDownStyle para cambiar el comportamiento del ComboBox. Esta propiedad acepta los valores posibles:

- DropDown: (valor predeterminado) el cuadro combinado muestra una lista desplegable al hacer clic en la flecha y puede especificar un valor personalizado.
- Simple: el cuadro combinado muestra automáticamente una lista desplegable y puede especificar un valor personalizado.
- DropDownList: el ComboBox funciona igual que un control DropDownList.

La diferencia entre DropDown y simple es cuando se muestra la lista de elementos. En el caso de simple, la lista se muestra inmediatamente cuando se mueve el foco al cuadro combinado. En el caso de la lista desplegable, debe hacer clic en la flecha para ver la lista de elementos.

El valor DropDownList hace que el control ComboBox funcione igual que un control DropDownList estándar. Sin embargo, aquí hay una diferencia importante. En las versiones anteriores de Internet Explorer se muestra un control DropDownList con un índice z infinito para que el control aparezca delante de cualquier control situado delante de él. Dado que el cuadro combinado representa una etiqueta HTML &lt;div&gt; en lugar de una etiqueta HTML &lt;Select&gt;, el cuadro combinado respeta correctamente el orden z.

## <a name="setting-the-autocompletemode"></a>Establecer AutoCompleteMode

La propiedad ComboBox AutoCompleteMode se usa para especificar lo que ocurre cuando alguien escribe texto en el cuadro combinado. Esta propiedad acepta los siguientes valores posibles:

- Ninguno: (valor predeterminado) el cuadro combinado no proporciona ningún comportamiento de autocompletar.
- Sugerir: el cuadro combinado muestra la lista y resalta el elemento coincidente en la lista (vea la figura 8).
- Anexar: el cuadro combinado no muestra la lista y anexa el elemento coincidente de la lista a lo que ha escrito (consulte la figura 9).
- SuggestAppend: el cuadro combinado muestra la lista y anexa el elemento coincidente de la lista a lo que ha escrito (vea la figura 10).

[![el cuadro combinado hace una sugerencia](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Figura 08**: el cuadro combinado hace una sugerencia ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image16.png))

[![ComboBox anexa texto coincidente](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Figura 09**: ComboBox anexa texto coincidente ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image18.png))

[![el cuadro combinado sugiere y anexa](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Figura 10**: el cuadro combinado sugiere y anexa ([haga clic para ver la imagen de tamaño completo](how-do-i-use-the-combobox-control-vb/_static/image20.png))

## <a name="summary"></a>Resumen

En este tutorial, aprendió a usar el control ComboBox para mostrar un conjunto fijo de elementos. Enlazamos el control ComboBox a un conjunto estático de elementos y a una tabla de base de datos. Por último, aprendió a modificar el comportamiento del control ComboBox estableciendo sus propiedades DropDownStyle y AutoCompleteMode.

> [!div class="step-by-step"]
> [Anterior](how-do-i-use-the-combobox-control-cs.md)
