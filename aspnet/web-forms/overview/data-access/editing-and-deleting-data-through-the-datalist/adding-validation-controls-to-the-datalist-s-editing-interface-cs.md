---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
title: Agregar controles de validación a la interfaz de edición de DataListC#() | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos lo fácil que es agregar controles de validación a la EditItemTemplate de DataList para proporcionar un usuario de edición más infalible...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 3ecc21c5-da0e-40ab-abb4-fac1e47398ad
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/adding-validation-controls-to-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: e3c14b7098da832bd28f57026e81dcb7f7ba7130
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74640497"
---
# <a name="adding-validation-controls-to-the-datalists-editing-interface-c"></a>Agregar controles de validación a la interfaz de edición de DataList (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_39_CS.exe) de la aplicación de ejemplo o [descarga de PDF](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/datatutorial39cs1.pdf)

> En este tutorial veremos lo fácil que es agregar controles de validación a la EditItemTemplate de DataList para proporcionar una interfaz de usuario de edición más infalible.

## <a name="introduction"></a>Introducción

Hasta ahora en los tutoriales de edición de DataList, las interfaces de edición de los datos de edición no han incluido ninguna validación de entradas de usuario proactiva, aunque la entrada de usuario no válida, como un nombre de producto o un precio negativo, produce una excepción. En el [tutorial anterior](handling-bll-and-dal-level-exceptions-cs.md) , hemos examinado cómo agregar código de control de excepciones al controlador de eventos `UpdateCommand` de DataList para detectar y mostrar correctamente la información sobre las excepciones que se generaron. Sin embargo, lo ideal es que la interfaz de edición incluya controles de validación para evitar que un usuario escriba tales datos no válidos en primer lugar.

En este tutorial veremos lo fácil que es agregar controles de validación a la `EditItemTemplate` DataList s para proporcionar una interfaz de usuario de edición más infalible. En concreto, este tutorial toma el ejemplo creado en el tutorial anterior y aumenta la interfaz de edición para incluir la validación adecuada.

## <a name="step-1-replicating-the-example-fromhandling-bll--and-dal-level-exceptionshandling-bll-and-dal-level-exceptions-csmd"></a>Paso 1: replicar el ejemplo desde el control de las[excepciones de nivel BLL y Dal](handling-bll-and-dal-level-exceptions-cs.md)

En el tutorial [control de excepciones de nivel BLL y Dal](handling-bll-and-dal-level-exceptions-cs.md) , hemos creado una página que enumera los nombres y los precios de los productos de una lista de controles de dos columnas y editables. Nuestro objetivo de este tutorial es aumentar la interfaz de edición de DataList para incluir controles de validación. En concreto, nuestra lógica de validación hará lo siguiente:

- Requerir que se proporcione el nombre del producto
- Asegúrese de que el valor especificado para el precio sea un formato de moneda válido
- Asegúrese de que el valor especificado para el precio sea mayor o igual que cero, ya que un valor de `UnitPrice` negativo no es válido.

Antes de que podamos ver el aumento del ejemplo anterior para incluir la validación, primero es necesario replicar el ejemplo de la página `ErrorHandling.aspx` de la carpeta `EditDeleteDataList` en la página de este tutorial, `UIValidation.aspx`. Para lograr esto, es necesario copiar tanto el marcado declarativo de la página `ErrorHandling.aspx` como su código fuente. En primer lugar, copie el marcado declarativo realizando los pasos siguientes:

1. Abrir la página de `ErrorHandling.aspx` en Visual Studio
2. Vaya al marcado declarativo de la página s (haga clic en el botón origen en la parte inferior de la página).
3. Copie el texto dentro de las etiquetas `<asp:Content>` y `</asp:Content>` (líneas 3 a 32), como se muestra en la figura 1.

[![copiar el texto del control &lt;ASP: Content&gt;](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image2.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: copiar el texto del control `<asp:Content>` ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image3.png))

1. Abrir la página `UIValidation.aspx`
2. Ir al marcado declarativo de la página
3. Pegue el texto dentro del control `<asp:Content>`.

Para copiar sobre el código fuente, abra la página `ErrorHandling.aspx.vb` y copie solo el texto *dentro* de la clase `EditDeleteDataList_ErrorHandling`. Copie los tres controladores de eventos (`Products_EditCommand`, `Products_CancelCommand`y `Products_UpdateCommand`) junto con el método de `DisplayExceptionDetails`, pero **no** Copie las instrucciones de declaración de clase o `using`. Pegue el texto copiado *dentro* de la clase `EditDeleteDataList_UIValidation` en `UIValidation.aspx.vb`.

Después de desplazarse por el contenido y el código de `ErrorHandling.aspx` a `UIValidation.aspx`, dedique un momento a probar las páginas en un explorador. Debería ver la misma salida y experimentar la misma funcionalidad en cada una de estas dos páginas (consulte la figura 2).

[![la página UIValidation. aspx imita la funcionalidad de ErrorHandling. aspx](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image5.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: la página `UIValidation.aspx` imita la funcionalidad de `ErrorHandling.aspx` ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image6.png))

## <a name="step-2-adding-the-validation-controls-to-the-datalist-s-edititemtemplate"></a>Paso 2: agregar los controles de validación a la EditItemTemplate de DataList s

Al construir formularios de entrada de datos, es importante que los usuarios escriban los campos obligatorios y que todas las entradas proporcionadas tengan valores con el formato correcto. Para ayudar a garantizar que las entradas de un usuario son válidas, ASP.NET proporciona cinco controles de validación integrados que están diseñados para validar el valor de un solo control Web de entrada:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantiza que se ha proporcionado un valor
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida un valor con respecto a otro valor de control Web o un valor constante, o garantiza que el formato del valor s es válido para un tipo de datos especificado.
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantiza que un valor está dentro de un intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida un valor con respecto a una [expresión regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida un valor con un método personalizado definido por el usuario

Para obtener más información sobre estos cinco controles, vuelva a consultar el tutorial [adición de controles de validación a la edición e inserción de interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) o consulte la [sección controles de validación](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/validation/default.aspx) de los [tutoriales de inicio rápido de ASP.net](https://quickstarts.asp.net).

En nuestro tutorial, tendremos que usar un RequiredFieldValidator para asegurarse de que se ha proporcionado un valor para el nombre del producto y un CompareValidator para asegurarse de que el precio especificado tiene un valor mayor o igual que 0 y se presenta en un formato de moneda válido.

> [!NOTE]
> Mientras que ASP.NET 1. x tenía estos cinco mismos controles de validación, ASP.NET 2,0 ha agregado varias mejoras, las dos principales compatibilidades con scripts de cliente para exploradores, además de Internet Explorer y la capacidad de crear particiones de controles de validación en una página en grupos de validación. Para obtener más información sobre las nuevas características de control de validación en 2,0, consulte [dessección de los controles de validación en ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Permita que s Comience agregando los controles de validación necesarios al `EditItemTemplate`de DataList. Esta tarea se puede realizar a través del diseñador haciendo clic en el vínculo editar plantillas de la etiqueta inteligente de DataList s o a través de la sintaxis declarativa. Deje pasar por el proceso mediante la opción editar plantillas del Vista de diseño. Después de elegir editar el `EditItemTemplate`de DataList, agregue un RequiredFieldValidator arrastrándolo desde el cuadro de herramientas a la interfaz de edición de plantillas, colocándolo después del cuadro de texto `ProductName`.

[![agregar un RequiredFieldValidator a EditItemTemplate después del cuadro de texto ProductName](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image8.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: agregar un RequiredFieldValidator al `EditItemTemplate After` el cuadro de texto `ProductName` ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image9.png))

Todos los controles de validación funcionan validando la entrada de un único control Web ASP.NET. Por lo tanto, es necesario indicar que el RequiredFieldValidator que se acaba de agregar debe validarse en el cuadro de texto `ProductName`; Esto se hace estableciendo la propiedad control de validación s [`ControlToValidate`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) en el `ID` del control Web adecuado (`ProductName`, en esta instancia). A continuación, establezca la [propiedad`ErrorMessage`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) en debe proporcionar el nombre del producto y la [propiedad`Text`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) a \*. El valor de la propiedad `Text`, si se proporciona, es el texto que muestra el control de validación si se produce un error en la validación. El control ValidationSummary usa el valor de la propiedad `ErrorMessage`, que es necesario. Si se omite el valor de la propiedad `Text`, el control de validación muestra el valor de la propiedad `ErrorMessage` en una entrada no válida.

Después de establecer estas tres propiedades del RequiredFieldValidator, la pantalla debe tener un aspecto similar al de la figura 4.

[![establecer las propiedades ControlToValidate, ErrorMessage y Text de RequiredFieldValidator s](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image11.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: establecimiento de las propiedades RequiredFieldValidator s `ControlToValidate`, `ErrorMessage`y `Text` ([haga clic para ver la imagen a tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image12.png))

Con el RequiredFieldValidator agregado al `EditItemTemplate`, todo lo que queda es agregar la validación necesaria para el cuadro de texto Product s Price. Como el `UnitPrice` es opcional al editar un registro, no es necesario agregar un RequiredFieldValidator. Sin embargo, es necesario agregar un CompareValidator para asegurarse de que el `UnitPrice`, si se proporciona, tiene el formato correcto como moneda y es mayor o igual que 0.

Agregue el método CompareValidator en el `EditItemTemplate` y establezca su propiedad `ControlToValidate` en `UnitPrice`, su propiedad `ErrorMessage` al precio debe ser mayor o igual que cero y no puede incluir el símbolo de divisa y su propiedad `Text` en \*. Para indicar que el valor de `UnitPrice` debe ser mayor o igual que 0, establezca la propiedad CompareValidator s [`Operator`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) en `GreaterThanEqual`, su [propiedad`ValueToCompare`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) en 0 y su [propiedad`Type`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) en `Currency`.

Después de agregar estos dos controles de validación, la sintaxis declarativa de DataList s `EditItemTemplate` s debe ser similar a la siguiente:

[!code-aspx[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

Después de realizar estos cambios, abra la página en un explorador. Si intenta omitir el nombre o escribir un valor de precio no válido al editar un producto, aparecerá un asterisco junto al cuadro de texto. Como se muestra en la figura 5, un valor de precio que incluye el símbolo de moneda como $19,95 se considera no válido. El `Type` CompareValidator s `Currency` permite separadores de dígitos (como comas o puntos, según la configuración de la referencia cultural) y un signo más o menos inicial, pero *no* permite un símbolo de moneda. Este comportamiento puede perplex a los usuarios, ya que la interfaz de edición representa actualmente el `UnitPrice` con el formato de moneda.

[![aparece un asterisco junto a los cuadros de texto con una entrada no válida](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image14.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: aparece un asterisco junto a los cuadros de texto con una entrada no válida ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image15.png))

Aunque la validación funciona tal cual, el usuario tiene que quitar manualmente el símbolo de moneda al editar un registro, lo que no es aceptable. Además, si hay entradas no válidas en la interfaz de edición ni los botones actualizar ni cancelar, cuando se hace clic, invocarán un postback. Lo ideal es que el botón Cancelar devuelva la lista de objetos a su estado de edición previa, independientemente de la validez de las entradas del usuario. Además, es necesario asegurarse de que los datos de la página son válidos antes de actualizar la información del producto en el controlador de eventos de DataList s `UpdateCommand`, ya que los usuarios cuyos exploradores no admiten JavaScript o deshabilitan su compatibilidad pueden omitir la lógica del lado cliente.

## <a name="removing-the-currency-symbol-from-the-edititemtemplate-s-unitprice-textbox"></a>Quitar el símbolo de divisa del cuadro de texto EditItemTemplate s UnitPrice

Cuando se usa el `Currency``Type`CompareValidator s, la entrada que se está validando no debe incluir ningún símbolo de moneda. La presencia de estos símbolos hace que CompareValidator Marque la entrada como no válida. Sin embargo, nuestra interfaz de edición incluye actualmente un símbolo de moneda en el cuadro de texto `UnitPrice`, lo que significa que el usuario debe quitar explícitamente el símbolo de moneda antes de guardar los cambios. Para solucionar este comportamiento tenemos tres opciones:

1. Configure el `EditItemTemplate` para que el valor del cuadro de texto `UnitPrice` no tenga el formato de moneda.
2. Permita que el usuario escriba un símbolo de divisa quitando el control CompareValidator y reemplazándolo por un RegularExpressionValidator que compruebe si hay un valor de moneda con el formato correcto. El desafío aquí es que la expresión regular para validar un valor de moneda no es tan sencilla como el CompareValidator y requeriría escribir código si deseáramos incorporar la configuración de referencia cultural.
3. Quite completamente el control de validación y confíe en la lógica de validación del lado servidor personalizada en el controlador de eventos `RowUpdating` de GridView.

Vamos a usar la opción 1 para este tutorial. Actualmente, el `UnitPrice` tiene el formato de valor de moneda debido a la expresión de enlace de enlaces para el cuadro de texto en el `EditItemTemplate`: `<%# Eval("UnitPrice", "{0:c}") %>`. Cambie la instrucción `Eval` a `Eval("UnitPrice", "{0:n2}")`, que da formato al resultado como un número con dos dígitos de precisión. Esto se puede hacer directamente a través de la sintaxis declarativa o haciendo clic en el vínculo editar DataBindings del cuadro de texto `UnitPrice` del `EditItemTemplate`DataList s.

Con este cambio, el precio con formato en la interfaz de edición incluye comas como separador de grupos y un punto como separador decimal, pero sale del símbolo de moneda.

> [!NOTE]
> Al quitar el formato de moneda de la interfaz editable, me resulta útil colocar el símbolo de divisa como texto fuera del cuadro de texto. Esto sirve como sugerencia al usuario de que no es necesario proporcionar el símbolo de moneda.

## <a name="fixing-the-cancel-button"></a>Corregir el botón Cancelar

De forma predeterminada, los controles Web de validación emiten JavaScript para realizar la validación en el lado cliente. Cuando se hace clic en un botón, LinkButton o ImageButton, los controles de validación de la página se comprueban en el lado cliente antes de que se produzca el PostBack. Si hay datos no válidos, se cancela el PostBack. Sin embargo, para determinados botones, la validez de los datos puede ser inmaterial; en tal caso, el hecho de que se cancele el PostBack debido a datos no válidos es una molestia.

El botón Cancelar es un ejemplo de este tipo. Imagine que un usuario escribe datos no válidos, como por ejemplo, si se omite el nombre del producto y, a continuación, decide que no quiere guardar el producto después de todos y llega al botón Cancelar. Actualmente, el botón Cancelar desencadena los controles de validación en la página, lo que informa de que falta el nombre del producto y evita el PostBack. El usuario tiene que escribir texto en el cuadro de texto `ProductName` solo para cancelar el proceso de edición.

Afortunadamente, el botón, el LinkButton y el ImageButton tienen una [propiedad`CausesValidation`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.causesvalidation.aspx) que puede indicar si al hacer clic en el botón debe iniciarse la lógica de validación (el valor predeterminado es `True`). Establezca la propiedad botón Cancelar `CausesValidation` en `False`.

## <a name="ensuring-the-inputs-are-valid-in-the-updatecommand-event-handler"></a>Asegurarse de que las entradas son válidas en el controlador de eventos UpdateCommand

Debido al script del lado cliente que emiten los controles de validación, si un usuario escribe una entrada no válida, los controles de validación cancelan los postbacks iniciados por los controles Button, LinkButton o ImageButton cuyas propiedades `CausesValidation` son `True` (valor predeterminado). Sin embargo, si un usuario visita con un explorador de antiquated o uno cuya compatibilidad con JavaScript se ha deshabilitado, las comprobaciones de validación del lado cliente no se ejecutarán.

Todos los controles de validación ASP.NET repiten la lógica de validación inmediatamente tras el PostBack y notifican la validez general de las entradas de la página a través de la [propiedad`Page.IsValid`](https://msdn.microsoft.com/library/system.web.ui.page.isvalid.aspx). Sin embargo, el flujo de página no se interrumpe ni se detiene de ningún modo según el valor de `Page.IsValid`. Como desarrolladores, es nuestra responsabilidad asegurarse de que la propiedad `Page.IsValid` tiene un valor de `True` antes de continuar con el código que supone que los datos de entrada son válidos.

Si un usuario tiene JavaScript deshabilitado, visita nuestra página, edita un producto, introduce un valor de precio de demasiado caro y hace clic en el botón actualizar, se omitirá la validación del lado cliente y se producirá un postback. En el postback, la página ASP.NET s `UpdateCommand` controlador de eventos se ejecuta y se produce una excepción al intentar analizar un `Decimal`demasiado caro. Dado que tenemos el control de excepciones, este tipo de excepción se controlará correctamente, pero podríamos evitar que los datos no válidos se retrasen en primer lugar con el `UpdateCommand` controlador de eventos si `Page.IsValid` tiene un valor de `True`.

Agregue el código siguiente al inicio del controlador de eventos `UpdateCommand`, inmediatamente antes del bloque de `Try`:

[!code-csharp[Main](adding-validation-controls-to-the-datalist-s-editing-interface-cs/samples/sample2.cs)]

Con esta adición, el producto solo intentará actualizarse si los datos enviados son válidos. La mayoría de los usuarios no podrán devolver datos no válidos debido a los scripts del lado cliente de los controles de validación, pero los usuarios cuyos exploradores no admitan JavaScript o que tengan la compatibilidad con JavaScript deshabilitada, pueden omitir las comprobaciones del lado cliente y enviar datos no válidos.

> [!NOTE]
> El lector astutos recordará que al actualizar los datos con GridView, no es necesario comprobar explícitamente la propiedad `Page.IsValid` en la clase de código subyacente de la página. Esto se debe a que GridView consulta la propiedad `Page.IsValid` para nosotros y solo continúa con la actualización si devuelve un valor de `True`.

## <a name="step-3-summarizing-data-entry-problems"></a>Paso 3: resumir problemas de entrada de datos

Además de los cinco controles de validación, ASP.NET incluye el [control ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que muestra los `ErrorMessage` s de los controles de validación que detectaron datos no válidos. Estos datos de resumen se pueden mostrar como texto en la página web o a través de un cuadro de mensajes modal del lado cliente. Vamos a mejorar este tutorial para incluir un cuadro de mensajes del lado cliente que resuma los problemas de validación.

Para ello, arrastre un control ValidationSummary desde el cuadro de herramientas hasta el diseñador. En realidad, la ubicación del control ValidationSummary no importa, ya que se va a configurar para que solo se muestre el resumen como un cuadro de mensajes. Después de agregar el control, establezca su [propiedad`ShowSummary`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) en `False` y su [propiedad`ShowMessageBox`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) en `True`. Con esta adición, los errores de validación se resumen en un cuadro de mensajes del lado cliente (consulte la figura 6).

[![los errores de validación se resumen en un cuadro de mensajes del lado cliente](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image17.png)](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: los errores de validación se resumen en un cuadro de mensajes del lado cliente ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-datalist-s-editing-interface-cs/_static/image18.png))

## <a name="summary"></a>Resumen

En este tutorial, vimos cómo reducir la probabilidad de que se produzcan excepciones mediante el uso de controles de validación para garantizar proactivamente que las entradas de los usuarios son válidas antes de intentar usarlas en el flujo de trabajo de actualización. ASP.NET proporciona cinco controles Web de validación que están diseñados para inspeccionar una entrada de control Web determinada e informar sobre la validez de la entrada. En este tutorial, usamos dos de esos cinco controles: RequiredFieldValidator y CompareValidator para asegurarse de que se proporcionó el nombre del producto y que el precio tenía un formato de moneda con un valor mayor o igual que cero.

Agregar controles de validación a la interfaz de edición de DataList es tan sencillo como arrastrarlos al `EditItemTemplate` desde el cuadro de herramientas y establecer unas cuantas propiedades. De forma predeterminada, los controles de validación emiten automáticamente el script de validación del lado cliente. también proporcionan la validación del lado servidor en el postback, almacenando el resultado acumulado en la propiedad `Page.IsValid`. Para omitir la validación del lado cliente cuando se hace clic en un botón, LinkButton o ImageButton, establezca la propiedad Button s `CausesValidation` en `False`. Además, antes de realizar cualquier tarea con los datos enviados en el postback, asegúrese de que la propiedad `Page.IsValid` devuelve `True`.

Todos los tutoriales de edición de DataList que hemos examinado hasta ahora han tenido interfaces de edición muy simples, un cuadro de texto para el nombre del producto y otro para el precio. La interfaz de edición, sin embargo, puede contener una combinación de controles Web diferentes, como DropDownLists, calendarios, botones de radiomedida, casillas, etc. En el siguiente tutorial, veremos cómo crear una interfaz que use diversos controles Web.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Dennis Patterson, Ken Pespisa y Liz Shulok. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](handling-bll-and-dal-level-exceptions-cs.md)
> [Siguiente](customizing-the-datalist-s-editing-interface-cs.md)
