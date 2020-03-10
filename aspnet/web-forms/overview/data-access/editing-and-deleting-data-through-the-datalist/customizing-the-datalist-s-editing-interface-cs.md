---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
title: Personalización de la interfaz de edición de DataListC#() | Microsoft Docs
author: rick-anderson
description: En este tutorial, se creará una interfaz de edición más enriquecida para la lista de controles, una que incluye DropDownLists y una casilla.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: a5d13067-ddfb-4c36-8209-0f69fd40e45c
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/customizing-the-datalist-s-editing-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 81f5a7f6737f544f577447f263dbd37dbc8279d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78480547"
---
# <a name="customizing-the-datalists-editing-interface-c"></a>Personalizar la interfaz de edición de DataList (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_40_CS.exe) de la aplicación de ejemplo o [descarga de PDF](customizing-the-datalist-s-editing-interface-cs/_static/datatutorial40cs1.pdf)

> En este tutorial, se creará una interfaz de edición más enriquecida para la lista de controles, una que incluye DropDownLists y una casilla.

## <a name="introduction"></a>Introducción

El marcado y los controles Web de los `EditItemTemplate` de lista de elementos definen su interfaz editable. En todos los ejemplos de DataList editables que se han examinado hasta el momento, la interfaz editable se ha compuesto de controles Web de cuadro de texto. En el [tutorial anterior](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md) , hemos mejorado la experiencia del usuario en tiempo de edición mediante la adición de controles de validación.

El `EditItemTemplate` se puede expandir más para incluir controles Web distintos del cuadro de texto, como DropDownLists, RadioButtonLists, calendarios, etc. Al igual que con los cuadros de texto, al personalizar la interfaz de edición para incluir otros controles Web, emplee los pasos siguientes:

1. Agregue el control Web al `EditItemTemplate`.
2. Use la sintaxis de DataBinding para asignar el valor del campo de datos correspondiente a la propiedad adecuada.
3. En el controlador de eventos `UpdateCommand`, obtenga acceso mediante programación al valor de control Web y páselo al método BLL adecuado.

En este tutorial, se creará una interfaz de edición más enriquecida para la lista de controles, una que incluye DropDownLists y una casilla. En concreto, vamos a crear una lista de datos que muestra la información del producto y permite actualizar el nombre del producto, el proveedor, la categoría y el estado descontinuado (consulte la figura 1).

[![la interfaz de edición incluye un cuadro de texto, dos DropDownLists y una casilla](customizing-the-datalist-s-editing-interface-cs/_static/image2.png)](customizing-the-datalist-s-editing-interface-cs/_static/image1.png)

**Figura 1**: la interfaz de edición incluye un cuadro de texto, dos DropDownLists y una casilla ([haga clic para ver la imagen de tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image3.png))

## <a name="step-1-displaying-product-information"></a>Paso 1: Mostrar la información del producto

Antes de poder crear la interfaz editable de DataList s, primero es necesario compilar la interfaz de solo lectura. Para empezar, abra la página de `CustomizedUI.aspx` de la carpeta `EditDeleteDataList` y, en el diseñador, agregue una lista de propiedades a la página y establezca su propiedad `ID` en `Products`. En la etiqueta inteligente DataList s, cree un nuevo ObjectDataSource. Asigne a este nuevo `ProductsDataSource` de origen de datos el nombre y configúrelo para recuperar datos del método `ProductsBLL` de clase s `GetProducts`. Al igual que con los tutoriales de DataList editables anteriores, actualizaremos la información de los productos editados yendo directamente a la capa de lógica de negocios. En consecuencia, establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna).

[![establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna)](customizing-the-datalist-s-editing-interface-cs/_static/image5.png)](customizing-the-datalist-s-editing-interface-cs/_static/image4.png)

**Figura 2**: establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image6.png))

Después de configurar ObjectDataSource, Visual Studio creará una `ItemTemplate` predeterminada para la lista de datos que muestra el nombre y el valor de cada uno de los campos de datos devueltos. Modifique el `ItemTemplate` para que la plantilla muestre el nombre del producto en un elemento `<h4>` junto con el nombre de la categoría, el nombre del proveedor, el precio y el estado no incluida. Además, agregue un botón Editar, asegurándose de que su propiedad `CommandName` está establecida en Editar. A continuación se muestra el marcado declarativo de My `ItemTemplate`:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample1.aspx)]

El marcado anterior diseña la información del producto mediante un encabezado &lt;H4&gt; para el nombre del producto y un `<table>` de cuatro columnas para los campos restantes. En los tutoriales anteriores se han explicado las clases CSS `ProductPropertyLabel` y `ProductPropertyValue`, definidas en `Styles.css`. En la figura 3 se muestra nuestro progreso cuando se ve a través de un explorador.

[![el nombre, el proveedor, la categoría, el estado descontinuado y el precio de cada producto se muestran](customizing-the-datalist-s-editing-interface-cs/_static/image8.png)](customizing-the-datalist-s-editing-interface-cs/_static/image7.png)

**Figura 3**: se muestra el nombre, el proveedor, la categoría, el estado descontinuado y el precio de cada producto ([haga clic para ver la imagen de tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image9.png))

## <a name="step-2-adding-the-web-controls-to-the-editing-interface"></a>Paso 2: agregar los controles Web a la interfaz de edición

El primer paso para crear la interfaz de edición de DataList personalizada es agregar los controles Web necesarios a la `EditItemTemplate`. En concreto, necesitamos un DropDownList para la categoría, otro para el proveedor y una casilla para el estado suspendido. Dado que el precio del producto no es editable en este ejemplo, podemos seguir mostrándolo mediante un control Web de etiqueta.

Para personalizar la interfaz de edición, haga clic en el vínculo editar plantillas de la etiqueta inteligente DataList s y elija la opción `EditItemTemplate` de la lista desplegable. Agregue un DropDownList al `EditItemTemplate` y establezca su `ID` en `Categories`.

[![agregar un DropDownList para las categorías](customizing-the-datalist-s-editing-interface-cs/_static/image11.png)](customizing-the-datalist-s-editing-interface-cs/_static/image10.png)

**Figura 4**: agregar un DropDownList para las categorías ([haga clic para ver la imagen de tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image12.png))

A continuación, en la etiqueta inteligente DropDownList s, seleccione la opción elegir origen de datos y cree un nuevo ObjectDataSource denominado `CategoriesDataSource`. Configure esta ObjectDataSource para usar el método de `GetCategories()` de `CategoriesBLL` Class (consulte la figura 5). A continuación, el Asistente para la configuración de orígenes de datos de DropDownList s solicita los campos de datos que se van a usar para cada `ListItem` s `Text` y propiedades de `Value`. Haga que el DropDownList muestre el `CategoryName` campo de datos y use el `CategoryID` como el valor, como se muestra en la figura 6.

[![crear un nuevo ObjectDataSource denominado CategoriesDataSource](customizing-the-datalist-s-editing-interface-cs/_static/image14.png)](customizing-the-datalist-s-editing-interface-cs/_static/image13.png)

**Figura 5**: crear un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic para ver la imagen de tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image15.png))

[![configurar los campos de valor y visualización de DropDownList](customizing-the-datalist-s-editing-interface-cs/_static/image17.png)](customizing-the-datalist-s-editing-interface-cs/_static/image16.png)

**Figura 6**: configuración de los campos de valor y visualización de DropDownList ([haga clic para ver la imagen de tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image18.png))

Repita esta serie de pasos para crear un DropDownList para los proveedores. Establezca el `ID` de este DropDownList en `Suppliers` y asígnele el nombre `SuppliersDataSource`ObjectDataSource.

Después de agregar los dos DropDownLists, agregue una casilla para el estado descontinuado y un cuadro de texto para el nombre del producto. Establezca la `ID` s para la casilla y el cuadro de texto en `Discontinued` y `ProductName`, respectivamente. Agregue un RequiredFieldValidator para asegurarse de que el usuario proporciona un valor para el nombre del producto.

Por último, agregue los botones actualizar y cancelar. Recuerde que, para estos dos botones, es imperativo que sus propiedades de `CommandName` estén establecidas para actualizar y cancelar, respectivamente.

No dude en diseñar la interfaz de edición que desee. He optado por usar el mismo diseño de `<table>` de cuatro columnas de la interfaz de solo lectura, como se muestra en la siguiente sintaxis declarativa y captura de pantalla:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample2.aspx)]

[![la interfaz de edición se distribuye como la interfaz de solo lectura](customizing-the-datalist-s-editing-interface-cs/_static/image20.png)](customizing-the-datalist-s-editing-interface-cs/_static/image19.png)

**Figura 7**: la interfaz de edición se ha diseñado como la interfaz de solo lectura ([haga clic para ver la imagen de tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image21.png))

## <a name="step-3-creating-the-editcommand-and-cancelcommand-event-handlers"></a>Paso 3: crear los controladores de eventos EditCommand y CancelCommand

Actualmente, no hay ninguna sintaxis de enlace de enlaces en el `EditItemTemplate` (excepto en el `UnitPriceLabel`, que se copió a partir de la `ItemTemplate`). Vamos a agregar la sintaxis de DataBinding momentáneamente, pero primero vamos a crear los controladores de eventos para los eventos `EditCommand` y `CancelCommand` de DataList. Recuerde que la responsabilidad del controlador de eventos `EditCommand` es representar la interfaz de edición del elemento de lista de elementos en el que se hizo clic en el botón Editar, mientras que el trabajo `CancelCommand` s es devolver la lista de objetos a su estado de edición previa.

Cree estos dos controladores de eventos y pídales que usen el código siguiente:

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample3.cs)]

Con estos dos controladores de eventos en su lugar, al hacer clic en el botón Editar, se muestra la interfaz de edición y al hacer clic en el botón Cancelar se devuelve el elemento editado a su modo de solo lectura. En la figura 8 se muestra la lista de objetos una vez que se ha realizado clic en el botón Editar para la combinación de gumbo de chef Carlos s. Puesto que aún no se ha agregado ninguna sintaxis de enlace de los enlaces a la interfaz de edición, el cuadro de texto `ProductName` está en blanco, la casilla de `Discontinued` no está activada y los primeros elementos seleccionados desde el `Categories` y `Suppliers` DropDownLists.

[![al hacer clic en el botón Editar, se muestra la interfaz de edición](customizing-the-datalist-s-editing-interface-cs/_static/image23.png)](customizing-the-datalist-s-editing-interface-cs/_static/image22.png)

**Figura 8**: haga clic en el botón Editar para mostrar la interfaz de edición ([haga clic para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image24.png))

## <a name="step-4-adding-the-databinding-syntax-to-the-editing-interface"></a>Paso 4: agregar la sintaxis de DataBinding a la interfaz de edición

Para que la interfaz de edición muestre los valores actuales de los productos, es necesario usar la sintaxis de DataBinding para asignar los valores de los campos de datos a los valores de control Web adecuados. La sintaxis de DataBinding puede aplicarse a través del diseñador. para ello, vaya a la pantalla editar plantillas y seleccione el vínculo editar DataBindings de las etiquetas inteligentes controles Web. Como alternativa, la sintaxis de DataBinding puede agregarse directamente al marcado declarativo.

Asigne el valor del campo de datos `ProductName` a la propiedad `ProductName` TextBox s `Text`, los valores del campo de datos `CategoryID` y `SupplierID` en las propiedades `Categories` y `Suppliers` DropDownLists `SelectedValue` y el valor del campo de datos `Discontinued` en la propiedad `Discontinued` CheckBox s `Checked`. Después de realizar estos cambios, ya sea a través del diseñador o directamente a través del marcado declarativo, vuelva a visitar la página a través de un explorador y haga clic en el botón Editar de chef Anton s Gumbo Mix. Como se muestra en la figura 9, la sintaxis de DataBinding ha agregado los valores actuales a los cuadros de texto, DropDownLists y CheckBox.

[![al hacer clic en el botón Editar, se muestra la interfaz de edición](customizing-the-datalist-s-editing-interface-cs/_static/image26.png)](customizing-the-datalist-s-editing-interface-cs/_static/image25.png)

**Figura 9**: al hacer clic en el botón Editar, se muestra la interfaz de edición ([haga clic para ver la imagen de tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image27.png))

## <a name="step-5-saving-the-user-s-changes-in-the-updatecommand-event-handler"></a>Paso 5: guardar los cambios del usuario en el controlador de eventos UpdateCommand

Cuando el usuario edita un producto y hace clic en el botón actualizar, se produce un postback y se desencadena el evento DataList `UpdateCommand`. En el controlador de eventos, es necesario leer los valores de los controles Web en la `EditItemTemplate` y la interfaz con el BLL para actualizar el producto en la base de datos. Como se ve en los tutoriales anteriores, se puede acceder al `ProductID` del producto actualizado a través de la colección de `DataKeys`. Se obtiene acceso a los campos especificados por el usuario mediante programación para hacer referencia a los controles Web mediante `FindControl("controlID")`, como se muestra en el código siguiente:

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample4.cs)]

El código comienza consultando la propiedad `Page.IsValid` para asegurarse de que todos los controles de validación de la página sean válidos. Si `Page.IsValid` es `True`, se lee el valor `ProductID` de los productos editados de la colección de `DataKeys` y se hace referencia mediante programación a los controles Web de entrada de datos del `EditItemTemplate`. Después, los valores de estos controles Web se leen en variables que se pasan a la sobrecarga de `UpdateProduct` adecuada. Después de actualizar los datos, la lista de datos se vuelve a su estado de edición previa.

> [!NOTE]
> He omitido la lógica de control de excepciones agregada en el tutorial [control de excepciones de nivel BLL y Dal](handling-bll-and-dal-level-exceptions-cs.md) para mantener el código y este ejemplo centrados. Como ejercicio, agregue esta funcionalidad después de completar este tutorial.

## <a name="step-6-handling-null-categoryid-and-supplierid-values"></a>Paso 6: control de valores NULL CategoryID y SupplierID

La base de datos Northwind permite valores `NULL` para las columnas `Products` Table s `CategoryID` y `SupplierID`. Sin embargo, nuestra interfaz de edición no admite actualmente `NULL` valores. Si intentamos editar un producto que tiene un valor `NULL` para sus columnas `CategoryID` o `SupplierID`, obteneremos un `ArgumentOutOfRangeException` con un mensaje de error similar a: *' Categories ' tiene un SelectedValue que no es válido porque no existe en la lista de elementos.* Además, actualmente no hay ninguna manera de cambiar una categoría de producto o valor de proveedor de un valor no`NULL` a un `NULL` uno.

Para admitir `NULL` valores de la categoría y del proveedor DropDownLists, es necesario agregar un `ListItem`adicional. He elegido usar (ninguno) como valor `Text` para este `ListItem`, pero puede cambiarlo a otra cosa (por ejemplo, una cadena vacía) si lo desea. Por último, recuerde establecer el `AppendDataBoundItems` DropDownLists en `True`; Si olvida hacerlo, las categorías y los proveedores enlazados a DropDownList sobrescribirán la `ListItem`agregada estáticamente.

Después de realizar estos cambios, el marcado DropDownLists en la `EditItemTemplate` de lista de elementos debe ser similar al siguiente:

[!code-aspx[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample5.aspx)]

> [!NOTE]
> Los `ListItem` estáticos se pueden agregar a un DropDownList a través del diseñador o directamente a través de la sintaxis declarativa. Al agregar un elemento DropDownList para representar un valor de `NULL` de base de datos, asegúrese de agregar el `ListItem` a través de la sintaxis declarativa. Si usa el editor de la colección `ListItem` en el diseñador, la sintaxis declarativa generada omitirá la configuración de `Value`, cuando se le asigne una cadena en blanco, creando un marcado declarativo como: `<asp:ListItem>(None)</asp:ListItem>`. Aunque esto puede parecer inofensivo, el `Value` que falta hace que DropDownList use el valor de la propiedad `Text` en su lugar. Esto significa que si se selecciona esta `NULL` `ListItem`, se intentará asignar el valor (ninguno) al campo de datos del producto (`CategoryID` o `SupplierID`, en este tutorial), lo que producirá una excepción. Al establecer `Value=""`explícitamente, se asignará un valor de `NULL` al campo de datos del producto cuando se seleccione el `ListItem` de `NULL`.

Dedique un momento a ver nuestro progreso a través de un explorador. Al editar un producto, tenga en cuenta que los `Categories` y `Suppliers` DropDownLists tienen una opción (ninguno) al principio de DropDownList.

[![las categorías y proveedores DropDownLists incluyen una opción (ninguno)](customizing-the-datalist-s-editing-interface-cs/_static/image29.png)](customizing-the-datalist-s-editing-interface-cs/_static/image28.png)

**Figura 10**: el `Categories` y `Suppliers` DropDownLists incluyen una opción (ninguno) ([haga clic para ver la imagen a tamaño completo](customizing-the-datalist-s-editing-interface-cs/_static/image30.png))

Para guardar la opción (ninguno) como una base de datos `NULL` valor, es necesario volver al controlador de eventos `UpdateCommand`. Cambie las variables `categoryIDValue` y `supplierIDValue` para que sean enteros que aceptan valores NULL y asígneles un valor distinto de `Nothing` solo si el `SelectedValue` DropDownList no es una cadena vacía:

[!code-csharp[Main](customizing-the-datalist-s-editing-interface-cs/samples/sample6.cs)]

Con este cambio, se pasará un valor de `Nothing` al método `UpdateProduct` BLL si el usuario seleccionó la opción (ninguno) de cualquiera de las listas desplegables, que corresponde a un valor de base de datos `NULL`.

## <a name="summary"></a>Resumen

En este tutorial, hemos visto cómo crear una interfaz de edición de lista de datos más compleja que incluía tres controles Web de entrada diferentes: un cuadro de texto, dos DropDownLists y una casilla junto con controles de validación. Al compilar la interfaz de edición, los pasos son los mismos independientemente de los controles Web que se usen: Empiece agregando los controles Web al `EditItemTemplate`de DataList s; Use la sintaxis de DataBinding para asignar los valores de campo de datos correspondientes a las propiedades de control Web adecuadas; y, en el controlador de eventos `UpdateCommand`, tener acceso mediante programación a los controles Web y sus propiedades adecuadas, pasando sus valores a la capa BLL.

Al crear una interfaz de edición, tanto si se compone de solo cuadros de texto como de una colección de controles Web diferentes, asegúrese de controlar correctamente los valores de `NULL` de la base de datos. Al tener en cuenta `NULL` s, es imperativo que no solo muestre correctamente un valor de `NULL` existente en la interfaz de edición, sino que también ofrezca un medio para marcar un valor como `NULL`. En el caso de DropDownLists en las listas de propiedades, esto significa normalmente agregar una `ListItem` estática cuya propiedad `Value` se establece explícitamente en una cadena vacía (`Value=""`) y agregando un bit de código al controlador de eventos `UpdateCommand` para determinar si se ha seleccionado el `NULL``ListItem`.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Dennis Patterson, David Suru y Randy Schmidt. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-datalist-s-editing-interface-cs.md)
> [Siguiente](an-overview-of-editing-and-deleting-data-in-the-datalist-vb.md)
