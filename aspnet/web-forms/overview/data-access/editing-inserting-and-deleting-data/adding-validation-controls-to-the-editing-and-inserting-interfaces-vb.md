---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
title: Agregar controles de validación a las interfaces de edición e inserción (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos lo fácil que es agregar controles de validación a la EditItemTemplate e InsertItemTemplate de un control Web de datos para proporcionar más...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: e3d7028a-7a22-4a4f-babe-d53afc41c0e2
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb
msc.type: authoredcontent
ms.openlocfilehash: 5c5ad110ee0836f0a464b02a2b29254e2e06381e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78479437"
---
# <a name="adding-validation-controls-to-the-editing-and-inserting-interfaces-vb"></a>Agregar controles de validación a las interfaces de edición e inserción (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_19_VB.exe) de la aplicación de ejemplo o [descarga de PDF](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/datatutorial19vb1.pdf)

> En este tutorial veremos lo fácil que es agregar controles de validación a la EditItemTemplate e InsertItemTemplate de un control Web de datos para proporcionar una interfaz de usuario más infalible.

## <a name="introduction"></a>Introducción

Los controles GridView y DetailsView de los ejemplos que hemos examinado en los tres últimos tutoriales se han compuesto de BoundFields y CheckBoxFields (los tipos de campo que Visual Studio agrega automáticamente al enlazar un control GridView o DetailsView a un origen de datos control a través de la etiqueta inteligente). Al editar una fila en GridView o DetailsView, las BoundFields que no son de solo lectura se convierten en cuadros de texto, desde los que el usuario final puede modificar los datos existentes. Del mismo modo, al insertar un nuevo registro en un control DetailsView, los BoundFields cuya propiedad `InsertVisible` esté establecida en `True` (el valor predeterminado) se representan como cuadros de texto vacíos, en los que el usuario puede proporcionar los valores de campo del nuevo registro. Del mismo modo, CheckBoxFields, que están deshabilitados en la interfaz estándar de solo lectura, se convierten en casillas habilitadas en las interfaces de edición e inserción.

Aunque las interfaces de edición e inserción predeterminadas para BoundField y CheckBoxField pueden ser útiles, la interfaz carece de cualquier tipo de validación. Si un usuario comete un error en la entrada de datos (por ejemplo, si se omite el campo `ProductName` o si se escribe un valor no válido para `UnitsInStock` (por ejemplo,-50), se generará una excepción desde las profundidades de la arquitectura de la aplicación. Aunque esta excepción se puede administrar correctamente tal como se muestra en el [tutorial anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md), lo ideal es que la interfaz de usuario de edición o inserción incluya controles de validación para evitar que un usuario escriba datos no válidos en primer lugar.

Para proporcionar una interfaz personalizada de edición o inserción, es necesario reemplazar el objeto BoundField o CheckBoxField con TemplateField. TemplateFields, que eran el tema de discusión sobre el [uso de TemplateFields en el control GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) y el [uso de TemplateFields en los](../custom-formatting/using-templatefields-in-the-detailsview-control-vb.md) tutoriales del control DetailsView, pueden constar de varias plantillas que definen interfaces independientes para diferentes Estados de fila. La `ItemTemplate` de TemplateField se utiliza para representar campos o filas de solo lectura en los controles DetailsView o GridView, mientras que los `EditItemTemplate` y `InsertItemTemplate` indican las interfaces que se van a usar para los modos de edición e inserción, respectivamente.

En este tutorial veremos lo fácil que es agregar controles de validación al `EditItemTemplate` de TemplateField y `InsertItemTemplate` para proporcionar una interfaz de usuario más infalible. En concreto, este tutorial toma el ejemplo creado en el tutorial [examen de los eventos asociados con la inserción, actualización y eliminación,](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) y aumenta las interfaces de edición e inserción para incluir la validación adecuada.

## <a name="step-1-replicating-the-example-fromexamining-the-events-associated-with-inserting-updating-and-deleting"></a>Paso 1: replicar el ejemplo desde el[examen de los eventos asociados a la inserción, actualización y eliminación](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md)

En el tutorial [examinar los eventos asociados con la inserción, actualización y eliminación](examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) , hemos creado una página con los nombres y los precios de los productos en un control GridView modificable. Además, la página incluye un DetailsView cuya propiedad `DefaultMode` se estableció en `Insert`, por lo que siempre se representa en modo de inserción. Desde este DetailsView, el usuario puede escribir el nombre y el precio de un nuevo producto, hacer clic en Insertar y agregarlo al sistema (consulte la figura 1).

[![el ejemplo anterior permite a los usuarios agregar nuevos productos y editar los existentes](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image2.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image1.png)

**Figura 1**: el ejemplo anterior permite a los usuarios agregar nuevos productos y editar los existentes ([haga clic para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image3.png))

Nuestro objetivo de este tutorial es aumentar el control DetailsView y GridView para proporcionar controles de validación. En concreto, nuestra lógica de validación hará lo siguiente:

- Requerir que se proporcione el nombre al insertar o editar un producto
- Requiere que se proporcione el precio al insertar un registro. al editar un registro, seguiremos requiriendo un precio, pero usará la lógica de programación en el controlador de eventos `RowUpdating` de GridView que ya está presente en el tutorial anterior.
- Asegúrese de que el valor especificado para el precio sea un formato de moneda válido

Antes de que podamos ver el aumento del ejemplo anterior para incluir la validación, primero es necesario replicar el ejemplo de la página `DataModificationEvents.aspx` en la página de este tutorial, `UIValidation.aspx`. Para lograr esto, es necesario copiar tanto el marcado declarativo de la página `DataModificationEvents.aspx` como su código fuente. En primer lugar, copie el marcado declarativo realizando los pasos siguientes:

1. Abrir la página de `DataModificationEvents.aspx` en Visual Studio
2. Vaya al marcado declarativo de la página (haga clic en el botón origen en la parte inferior de la página).
3. Copie el texto dentro de las etiquetas `<asp:Content>` y `</asp:Content>` (líneas 3 a 44), como se muestra en la figura 2.

[![copiar el texto del control &lt;ASP: Content&gt;](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image5.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image4.png)

**Figura 2**: copiar el texto del control `<asp:Content>` ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image6.png))

1. Abrir la página `UIValidation.aspx`
2. Ir al marcado declarativo de la página
3. Pegue el texto dentro del control `<asp:Content>`.

Para copiar sobre el código fuente, abra la página `DataModificationEvents.aspx.vb` y copie solo el texto *dentro* de la clase `EditInsertDelete_DataModificationEvents`. Copie los tres controladores de eventos (`Page_Load`, `GridView1_RowUpdating`y `ObjectDataSource1_Inserting`), pero **no** copie la declaración de clase. Pegue el texto copiado *dentro* de la clase `EditInsertDelete_UIValidation` en `UIValidation.aspx.vb`.

Después de desplazarse por el contenido y el código de `DataModificationEvents.aspx` a `UIValidation.aspx`, dedique un momento a probar el progreso en un explorador. Debería ver la misma salida y experimentar la misma funcionalidad en cada una de estas dos páginas (consulte la figura 1 para obtener una captura de pantalla de `DataModificationEvents.aspx` en acción).

## <a name="step-2-converting-the-boundfields-into-templatefields"></a>Paso 2: convertir BoundFields en TemplateFields

Para agregar controles de validación a las interfaces de edición e inserción, los BoundFields utilizados por los controles DetailsView y GridView deben convertirse en TemplateFields. Para ello, haga clic en los vínculos editar columnas y editar campos de las etiquetas inteligentes GridView y DetailsView, respectivamente. Allí, seleccione cada uno de los BoundFields y haga clic en el vínculo "convertir este campo en TemplateField".

[![convertir cada BoundFields de DetailsView y de GridView en TemplateFields](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image8.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image7.png)

**Figura 3**: conversión de cada uno de los BoundFields de DetailsView y GridView en TemplateFields ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image9.png))

Al convertir un BoundField en un TemplateField a través del cuadro de diálogo campos, se genera un TemplateField que exhibe las mismas interfaces de solo lectura, edición e inserción, que el propio BoundField. En el marcado siguiente se muestra la sintaxis declarativa para el campo `ProductName` en DetailsView después de convertirse en TemplateField:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample1.aspx)]

Tenga en cuenta que este TemplateField tenía tres plantillas creadas automáticamente `ItemTemplate`, `EditItemTemplate`y `InsertItemTemplate`. En el `ItemTemplate` se muestra un valor de campo de datos único (`ProductName`) mediante un control Web de etiqueta, mientras que los `EditItemTemplate` y `InsertItemTemplate` presentan el valor del campo de datos en un control Web TextBox que asocia el campo de datos a la propiedad `Text` del cuadro de texto mediante el enlace de datos bidireccional. Puesto que solo usamos DetailsView en esta página para insertar, puede quitar el `ItemTemplate` y `EditItemTemplate` de los dos TemplateFields, aunque no hay ningún daño en dejarlos.

Dado que GridView no es compatible con las características integradas de inserción de DetailsView, convertir el campo de `ProductName` de GridView en un TemplateField solo produce una `ItemTemplate` y `EditItemTemplate`:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample2.aspx)]

Al hacer clic en "convertir este campo en TemplateField", Visual Studio ha creado un TemplateField cuyas plantillas imitan la interfaz de usuario del BoundField convertido. Para comprobarlo, visite esta página a través de un explorador. Observará que la apariencia y el comportamiento de TemplateFields son idénticos a la experiencia cuando se usaron BoundFields en su lugar.

> [!NOTE]
> No dude en personalizar las interfaces de edición en las plantillas según sea necesario. Por ejemplo, es posible que deseemos que el cuadro de texto del `UnitPrice` TemplateFields se represente como un cuadro de texto más pequeño que el cuadro de texto `ProductName`. Para ello, puede establecer la propiedad `Columns` del cuadro de texto en un valor adecuado o proporcionar un ancho absoluto a través de la propiedad `Width`. En el siguiente tutorial, veremos cómo personalizar por completo la interfaz de edición reemplazando el cuadro de texto con un control Web de entrada de datos alternativo.

## <a name="step-3-adding-the-validation-controls-to-the-gridviewsedititemtemplate-s"></a>Paso 3: agregar los controles de validación a los`EditItemTemplate` de GridView

Al construir formularios de entrada de datos, es importante que los usuarios escriban los campos obligatorios y que todas las entradas proporcionadas tengan valores con el formato correcto. Para asegurarse de que las entradas de un usuario son válidas, ASP.NET proporciona cinco controles de validación integrados que están diseñados para su uso con el fin de validar el valor de un solo control de entrada:

- [RequiredFieldValidator](https://msdn.microsoft.com/library/5hbw267h(VS.80).aspx) garantiza que se ha proporcionado un valor
- [CompareValidator](https://msdn.microsoft.com/library/db330ayw(VS.80).aspx) valida un valor con respecto a otro valor de control Web o un valor constante, o bien garantiza que el formato del valor es válido para un tipo de datos especificado.
- [RangeValidator](https://msdn.microsoft.com/library/f70d09xt.aspx) garantiza que un valor está dentro de un intervalo de valores
- [RegularExpressionValidator](https://msdn.microsoft.com/library/eahwtc9e.aspx) valida un valor con respecto a una [expresión regular](http://en.wikipedia.org/wiki/Regular_expression)
- [CustomValidator](https://msdn.microsoft.com/library/9eee01cx(VS.80).aspx) valida un valor con un método personalizado definido por el usuario

Para obtener más información sobre estos cinco controles, consulte la [sección de controles de validación](https://quickstarts.asp.net/quickstartv20/aspnet/doc/ctrlref/validation/default.aspx) de los [tutoriales de inicio rápido de ASP.net](https://asp.net/QuickStart/aspnet/).

En nuestro tutorial, tendremos que usar un RequiredFieldValidator en el `ProductName` TemplateFields y DetailsView de GridView y un RequiredFieldValidator en la `UnitPrice` TemplateField de DetailsView. Además, tendremos que agregar un control CompareValidator a ambos controles ' `UnitPrice` TemplateFields que garantiza que el valor del precio especificado tiene un valor mayor o igual que 0 y se presenta en un formato de moneda válido.

> [!NOTE]
> Mientras que ASP.NET 1. x tenía estos cinco mismos controles de validación, ASP.NET 2,0 ha agregado varias mejoras, las dos principales que son la compatibilidad con scripts de cliente para exploradores distintos de Internet Explorer y la capacidad de crear particiones de los controles de validación de una página en grupos de validación. Para obtener más información sobre las nuevas características de control de validación en 2,0, consulte [dessección de los controles de validación en ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx).

Comencemos agregando los controles de validación necesarios a los `EditItemTemplate` s en el TemplateFields de GridView. Para ello, haga clic en el vínculo editar plantillas de la etiqueta inteligente de GridView para abrir la interfaz de edición de plantillas. Desde aquí, puede seleccionar la plantilla que se va a editar en la lista desplegable. Puesto que queremos aumentar la interfaz de edición, es necesario agregar controles de validación al `ProductName` y `UnitPrice``EditItemTemplate` s.

[![necesitamos ampliar el EditItemTemplates de ProductName y UnitPrice](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image11.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image10.png)

**Figura 4**: es necesario extender el `ProductName` y `UnitPrice``EditItemTemplate` s ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image12.png))

En el `EditItemTemplate``ProductName`, agregue un RequiredFieldValidator arrastrándolo desde el cuadro de herramientas a la interfaz de edición de plantillas, colocando después del cuadro de texto.

[![agregar un RequiredFieldValidator a ProductName EditItemTemplate](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image14.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image13.png)

**Figura 5**: agregar un RequiredFieldValidator al `EditItemTemplate` de `ProductName` ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image15.png))

Todos los controles de validación funcionan validando la entrada de un único control Web ASP.NET. Por lo tanto, es necesario indicar que el RequiredFieldValidator que se acaba de agregar debe validarse con el cuadro de texto del `EditItemTemplate`; Esto se logra estableciendo la [propiedad ControlToValidate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.controltovalidate(VS.80).aspx) del control de validación en el `ID` del control Web adecuado. Actualmente, el cuadro de texto tiene el `ID` en su lugar nondescript de `TextBox1`, pero vamos a cambiarlo a algo más adecuado. Haga clic en el cuadro de texto de la plantilla y, a continuación, en el ventana Propiedades, cambie el `ID` de `TextBox1` a `EditProductName`.

[![cambiar el identificador del cuadro de texto a EditProductName](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image17.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image16.png)

**Ilustración 6**: cambiar el `ID` del cuadro de texto a `EditProductName` ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image18.png))

A continuación, establezca la propiedad `ControlToValidate` del RequiredFieldValidator en `EditProductName`. Por último, establezca la [propiedad ErrorMessage](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.errormessage(VS.80).aspx) en "debe proporcionar el nombre del producto" y la [propiedad Text](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basevalidator.text(VS.80).aspx) en "\*". El valor de la propiedad `Text`, si se proporciona, es el texto que muestra el control de validación si se produce un error en la validación. El control ValidationSummary usa el valor de la propiedad `ErrorMessage`, que es necesario. Si se omite el valor de la propiedad `Text`, el valor de la propiedad `ErrorMessage` es también el texto mostrado por el control de validación en entradas no válidas.

Después de establecer estas tres propiedades del RequiredFieldValidator, la pantalla debe tener un aspecto similar al de la figura 7.

[![establecer las propiedades ControlToValidate, ErrorMessage y text del RequiredFieldValidator](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image20.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image19.png)

**Figura 7**: establecimiento de las propiedades `ControlToValidate`, `ErrorMessage`y `Text` del RequiredFieldValidator ([haga clic para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image21.png))

Con el RequiredFieldValidator agregado al `EditItemTemplate``ProductName`, todo lo que queda es agregar la validación necesaria al `EditItemTemplate``UnitPrice`. Como hemos decidido que, en esta página, el `UnitPrice` es opcional al editar un registro, no es necesario agregar un RequiredFieldValidator. Sin embargo, es necesario agregar un CompareValidator para asegurarse de que el `UnitPrice`, si se proporciona, tiene el formato correcto como moneda y es mayor o igual que 0.

Antes de agregar el control CompareValidator al `EditItemTemplate``UnitPrice`, vamos a cambiar el identificador del control Web TextBox de `TextBox2` a `EditUnitPrice`. Después de realizar este cambio, agregue CompareValidator, estableciendo su propiedad `ControlToValidate` en `EditUnitPrice`, su `ErrorMessage` propiedad en "el precio debe ser mayor o igual que cero y no puede incluir el símbolo de divisa", y su propiedad `Text` en "\*".

Para indicar que el valor de `UnitPrice` debe ser mayor o igual que 0, establezca la propiedad de [operador](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.operator(VS.80).aspx) de CompareValidator en `GreaterThanEqual`, su [propiedad ValueToCompare](https://msdn.microsoft.com/library/system.web.ui.webcontrols.comparevalidator.valuetocompare(VS.80).aspx) en "0" y su [propiedad Type](https://msdn.microsoft.com/library/system.web.ui.webcontrols.basecomparevalidator.type.aspx) en `Currency`. La sintaxis declarativa siguiente muestra el `UnitPrice` `EditItemTemplate` de TemplateField una vez realizados estos cambios:

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample3.aspx)]

Después de realizar estos cambios, abra la página en un explorador. Si intenta omitir el nombre o escribir un valor de precio no válido al editar un producto, aparecerá un asterisco junto al cuadro de texto. Como se muestra en la figura 8, se considera que un valor de precio que incluye el símbolo de moneda como $19,95 es no válido. El `Type` de `Currency` del CompareValidator permite separadores de dígitos (como comas o puntos, en función de la configuración de la referencia cultural) y un signo más o menos inicial, pero *no* permite un símbolo de moneda. Este comportamiento puede perplex a los usuarios, ya que la interfaz de edición representa actualmente el `UnitPrice` con el formato de moneda.

> [!NOTE]
> Recuerde que en los *eventos asociados con la inserción, actualización y eliminación* del tutorial, establecemos la propiedad `DataFormatString` de BoundField en `{0:c}` para darle formato de moneda. Además, se establece la propiedad `ApplyFormatInEditMode` en true, lo que hace que la interfaz de edición de GridView formatee el `UnitPrice` como moneda. Al convertir BoundField en TemplateField, Visual Studio indicó esta configuración y dio formato a la propiedad `Text` del cuadro de texto como moneda mediante la sintaxis de DataBinding `<%# Bind("UnitPrice", "{0:c}") %>`.

[![aparece un asterisco junto a los cuadros de texto con una entrada no válida](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image23.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image22.png)

**Figura 8**: aparece un asterisco junto a los cuadros de texto con una entrada no válida ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image24.png))

Aunque la validación funciona tal cual, el usuario tiene que quitar manualmente el símbolo de moneda al editar un registro, lo que no es aceptable. Para solucionar este comportamiento, tenemos tres opciones:

1. Configure el `EditItemTemplate` para que el valor `UnitPrice` no tenga el formato de moneda.
2. Permita que el usuario escriba un símbolo de divisa quitando el control CompareValidator y reemplazándolo por un RegularExpressionValidator que compruebe correctamente si hay un valor de moneda con el formato correcto. El problema es que la expresión regular para validar un valor de moneda no es bastante y requeriría escribir código si deseáramos incorporar la configuración de referencia cultural.
3. Quite completamente el control de validación y confíe en la lógica de validación del lado servidor en el controlador de eventos `RowUpdating` de GridView.

Vamos a usar la opción #1 para este ejercicio. Actualmente, el `UnitPrice` tiene el formato de moneda debido a la expresión de enlace de enlaces para el cuadro de texto en el `EditItemTemplate`: `<%# Bind("UnitPrice", "{0:c}") %>`. Cambie la instrucción BIND a `Bind("UnitPrice", "{0:n2}")`, que da formato al resultado como un número con dos dígitos de precisión. Esto se puede hacer directamente a través de la sintaxis declarativa o haciendo clic en el vínculo editar enlaces de enlace del cuadro de texto `EditUnitPrice` en el `EditItemTemplate` de `UnitPrice` TemplateField (vea las figuras 9 y 10).

[![haga clic en el vínculo editar DataBindings del cuadro de texto](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image26.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image25.png)

**Figura 9**: haga clic en el vínculo editar DataBindings del cuadro de texto ([haga clic para ver la imagen a tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image27.png))

[![especificar el especificador de formato en la instrucción BIND](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image29.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image28.png)

**Figura 10**: especifique el especificador de formato en la instrucción `Bind` ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image30.png))

Con este cambio, el precio con formato en la interfaz de edición incluye comas como separador de grupos y un punto como separador decimal, pero sale del símbolo de moneda.

> [!NOTE]
> El `EditItemTemplate` `UnitPrice` no incluye un RequiredFieldValidator, lo que permite que el PostBack se represente y se inicie la lógica de actualización. Sin embargo, el controlador de eventos `RowUpdating` copiado del tutorial *examinar los eventos asociados con la inserción, actualización y eliminación* incluye una comprobación mediante programación que garantiza que se proporciona el `UnitPrice`. No dude en quitar esta lógica, déjela tal cual, o bien agregue un RequiredFieldValidator al `EditItemTemplate``UnitPrice`.

## <a name="step-4-summarizing-data-entry-problems"></a>Paso 4: Resumen de los problemas de entrada de datos

Además de los cinco controles de validación, ASP.NET incluye el [control ValidationSummary](https://msdn.microsoft.com/library/f9h59855(VS.80).aspx), que muestra los `ErrorMessage` s de los controles de validación que detectaron datos no válidos. Estos datos de resumen se pueden mostrar como texto en la página web o a través de un cuadro de mensajes modal del lado cliente. Vamos a mejorar este tutorial para incluir un cuadro de mensajes del lado cliente que resuma los problemas de validación.

Para ello, arrastre un control ValidationSummary desde el cuadro de herramientas hasta el diseñador. La ubicación del control de validación no importa realmente, ya que vamos a configurarlo para que solo se muestre el resumen como un cuadro de mensajes. Después de agregar el control, establezca la [propiedad ShowSummary](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showsummary(VS.80).aspx) en `False` y su [propiedad método ShowMessageBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.validationsummary.showmessagebox(VS.80).aspx) en `True`. Con esta adición, cualquier error de validación se resume en un cuadro de mensajes del lado cliente.

[![los errores de validación se resumen en un cuadro de mensajes del lado cliente](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image32.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image31.png)

**Figura 11**: los errores de validación se resumen en un cuadro de mensajes del lado cliente ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image33.png))

## <a name="step-5-adding-the-validation-controls-to-the-detailsviewsinsertitemtemplate"></a>Paso 5: agregar los controles de validación al`InsertItemTemplate` de DetailsView

Lo único que queda para este tutorial es agregar los controles de validación a la interfaz de inserción de DetailsView. El proceso de agregar controles de validación a las plantillas de DetailsView es idéntico al que se examinó en el paso 3; por lo tanto, vamos a usar la tarea en este paso. Como hicimos con los `EditItemTemplate` de GridView, recomiendo cambiar el nombre de los `ID` s de los cuadros de texto del `TextBox1` de nondescript y `TextBox2` a `InsertProductName` y `InsertUnitPrice`.

Agregue un RequiredFieldValidator al `InsertItemTemplate``ProductName`. Establezca el `ControlToValidate` en el `ID` del cuadro de texto de la plantilla, su propiedad `Text` en "\*" y su propiedad `ErrorMessage` en "debe proporcionar el nombre del producto".

Puesto que el `UnitPrice` es necesario para esta página al agregar un nuevo registro, agregue un RequiredFieldValidator al `InsertItemTemplate`de `UnitPrice`, estableciendo las propiedades `ControlToValidate`, `Text`y `ErrorMessage` adecuadamente. Por último, agregue también un control CompareValidator al `UnitPrice` `InsertItemTemplate`, configurando sus propiedades `ControlToValidate`, `Text`, `ErrorMessage`, `Type`, `Operator`y `ValueToCompare` igual que hicimos con el control CompareValidator del `UnitPrice`en la `EditItemTemplate`de GridView.

Después de agregar estos controles de validación, no se puede Agregar un nuevo producto al sistema si no se proporciona su nombre o si su precio es un número negativo o tiene un formato incorrecto.

[![se ha agregado la lógica de validación a la interfaz de inserción de DetailsView](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image35.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image34.png)

**Figura 12**: se ha agregado la lógica de validación a la interfaz de inserción de DetailsView ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image36.png))

## <a name="step-6-partitioning-the-validation-controls-into-validation-groups"></a>Paso 6: crear particiones de los controles de validación en grupos de validación

Nuestra página consta de dos conjuntos lógicamente dispares de controles de validación: los que corresponden a la interfaz de edición de GridView y los que corresponden a la interfaz de inserción de DetailsView. De forma predeterminada, cuando se produce un postback, se comprueban *todos* los controles de validación de la página. Sin embargo, al editar un registro, no queremos que se validen los controles de validación de la interfaz de inserción de DetailsView. En la figura 13 se muestra nuestro dilema actual cuando un usuario edita un producto con valores perfectamente válidos, al hacer clic en actualizar se produce un error de validación porque los valores de nombre y precio de la interfaz de inserción están en blanco.

[![la actualización de un producto provoca la activación de los controles de validación de la interfaz de inserción](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image38.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image37.png)

**Figura 13**: la actualización de un producto provoca la activación de los controles de validación de la interfaz de inserción ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image39.png))

Los controles de validación de ASP.NET 2,0 se pueden particionar en grupos de validación a través de su propiedad `ValidationGroup`. Para asociar un conjunto de controles de validación en un grupo, basta con establecer su propiedad `ValidationGroup` en el mismo valor. En nuestro tutorial, establezca las propiedades de `ValidationGroup` de los controles de validación en el TemplateFields de GridView en `EditValidationControls` y las propiedades `ValidationGroup` de TemplateFields de DetailsView en `InsertValidationControls`. Estos cambios se pueden realizar directamente en el marcado declarativo o a través del ventana Propiedades cuando se usa la interfaz de edición de plantillas del diseñador.

Además de los controles de validación, el botón y los controles relacionados con los botones de ASP.NET 2,0 también incluyen una propiedad `ValidationGroup`. Solo se comprueba la validez de los validadores de un grupo de validación cuando un botón que tiene el mismo `ValidationGroup` configuración de la propiedad. Por ejemplo, para que el botón Insertar de DetailsView desencadene el grupo de validación `InsertValidationControls`, es necesario establecer la propiedad `ValidationGroup` de CommandField en `InsertValidationControls` (vea la figura 14). Además, establezca la propiedad CommandField's `ValidationGroup` de GridView en `EditValidationControls`.

[![establecer la propiedad CommandField's ValidationGroup de DetailsView en InsertValidationControls](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image41.png)](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image40.png)

**Figura 14**: establezca la propiedad CommandField's `ValidationGroup` de DetailsView en `InsertValidationControls` ([haga clic para ver la imagen de tamaño completo](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/_static/image42.png))

Después de estos cambios, los TemplateFields y CommandFields de DetailsView y GridView deben ser similares a los siguientes:

TemplateFields y CommandField de DetailsView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample4.aspx)]

CommandField y TemplateFields de GridView

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample5.aspx)]

En este momento, los controles de validación específicos de la edición se activan solo cuando se hace clic en el botón actualizar de GridView y los controles de validación específicos de la inserción se activan solo cuando se hace clic en el botón Insertar de DetailsView, resolviendo el problema resaltado en la figura 13. Sin embargo, con este cambio, el control ValidationSummary ya no aparece cuando se escriben datos no válidos. El control ValidationSummary también contiene una propiedad `ValidationGroup` y solo muestra información de resumen para los controles de validación de su grupo de validación. Por lo tanto, es necesario tener dos controles de validación en esta página, uno para el grupo de validación `InsertValidationControls` y otro para `EditValidationControls`.

[!code-aspx[Main](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb/samples/sample6.aspx)]

Con esta adición, nuestro tutorial está completo.

## <a name="summary"></a>Resumen

Aunque BoundFields puede proporcionar una interfaz de inserción y edición, la interfaz no es personalizable. Normalmente, queremos agregar controles de validación a la interfaz de edición e inserción para asegurarse de que el usuario escriba las entradas necesarias en un formato válido. Para ello, debemos convertir BoundFields en TemplateFields y agregar los controles de validación a las plantillas adecuadas. En este tutorial se amplió el ejemplo del tutorial *examinando los eventos asociados con la inserción, actualización y eliminación* , agregando controles de validación a la interfaz de inserción de DetailsView y a la interfaz de edición de GridView. Además, vimos cómo Mostrar información de validación de Resumen mediante el control ValidationSummary y cómo crear particiones de los controles de validación de la página en grupos de validación distintos.

Como vimos en este tutorial, TemplateFields permite aumentar las interfaces de edición e inserción para incluir controles de validación. TemplateFields también se puede extender para incluir controles Web de entrada adicionales, lo que permite reemplazar el cuadro de texto por un control Web más adecuado. En el tutorial siguiente, veremos cómo reemplazar el control TextBox con un control DropDownList enlazado a datos, que es ideal para editar una clave externa (como `CategoryID` o `SupplierID` en la tabla `Products`).

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Liz Shulok y Zack Jones. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md)
> [Siguiente](customizing-the-data-modification-interface-vb.md)
