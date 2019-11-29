---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
title: Personalizar la interfaz de modificación de datos (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos cómo personalizar la interfaz de un control GridView modificable, reemplazando los controles de cuadro de texto y casilla estándar con la función alternar...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 4830d984-bd2c-4a08-bfe5-2385599f1f7d
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb
msc.type: authoredcontent
ms.openlocfilehash: 85ec7bdde6b2bffbbda066b0441bbd36b7072197
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74584979"
---
# <a name="customizing-the-data-modification-interface-vb"></a>Personalizar la interfaz de modificación de datos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_20_VB.exe) de la aplicación de ejemplo o [descarga de PDF](customizing-the-data-modification-interface-vb/_static/datatutorial20vb1.pdf)

> En este tutorial veremos cómo personalizar la interfaz de un control GridView modificable, reemplazando los controles de cuadro de texto y casilla estándar con controles Web de entrada alternativos.

## <a name="introduction"></a>Introducción

BoundFields y CheckBoxFields que usan los controles GridView y DetailsView simplifican el proceso de modificación de datos debido a su capacidad de representar interfaces de solo lectura, editables e insertables. Estas interfaces se pueden representar sin necesidad de agregar código o marcado declarativo adicional. Sin embargo, las interfaces de BoundField y CheckBoxField carecen de la personalización que a menudo se necesita en escenarios del mundo real. Para personalizar la interfaz que se puede editar o insertar en un control GridView o DetailsView, se debe usar en su lugar un TemplateField.

En el [tutorial anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) , vimos cómo personalizar las interfaces de modificación de datos mediante la adición de controles Web de validación. En este tutorial veremos cómo personalizar los controles Web de recopilación de datos reales, reemplazando los controles de cuadro de texto y casillas estándar de BoundField y CheckBoxField con controles Web de entrada alternativos. En concreto, vamos a crear un GridView modificable que permite actualizar el nombre de un producto, la categoría, el proveedor y el estado descontinuado. Al editar una fila determinada, los campos de categoría y proveedor se representarán como DropDownLists, que contiene el conjunto de categorías y proveedores disponibles entre los que elegir. Además, reemplazaremos la casilla predeterminada de CheckBoxField con un control RadioButtonList que ofrece dos opciones: "Active" y "discontinued".

[![la interfaz de edición de GridView incluye DropDownLists y RadioButtons](customizing-the-data-modification-interface-vb/_static/image2.png)](customizing-the-data-modification-interface-vb/_static/image1.png)

**Figura 1**: la interfaz de edición de GridView incluye DropDownLists y RadioButtons ([haga clic para ver la imagen a tamaño completo](customizing-the-data-modification-interface-vb/_static/image3.png))

## <a name="step-1-creating-the-appropriateupdateproductoverload"></a>Paso 1: crear la sobrecarga de`UpdateProduct`adecuada

En este tutorial, se creará un GridView modificable que permite la edición del nombre, la categoría, el proveedor y el estado de un producto. Por lo tanto, se necesita una sobrecarga `UpdateProduct` que acepte cinco parámetros de entrada, estos cuatro valores de producto más el `ProductID`. Al igual que en las sobrecargas anteriores, este:

1. Recuperar la información del producto de la base de datos para el `ProductID`especificado,
2. Actualice los campos `ProductName`, `CategoryID`, `SupplierID`y `Discontinued`.
3. Envíe la solicitud de actualización a la capa DAL a través del método `Update()` del TableAdapter.

Por motivos de brevedad, para esta sobrecarga concreta, se ha omitido la comprobación de la regla de negocios que garantiza que un producto que se está marcando como discontinuo no es el único producto ofrecido por su proveedor. No dude en agregarlo en si lo prefiere o, idealmente, refactorizar la lógica en un método independiente.

En el código siguiente se muestra la nueva sobrecarga de `UpdateProduct` en la clase `ProductsBLL`:

[!code-vb[Main](customizing-the-data-modification-interface-vb/samples/sample1.vb)]

## <a name="step-2-crafting-the-editable-gridview"></a>Paso 2: crear el GridView modificable

Con la `UpdateProduct` sobrecarga agregada, estamos listos para crear nuestro GridView modificable. Abra la página `CustomizedUI.aspx` en la carpeta `EditInsertDelete` y agregue un control GridView al diseñador. A continuación, cree un nuevo ObjectDataSource a partir de la etiqueta inteligente de GridView. Configure ObjectDataSource para recuperar información del producto a través del método de `GetProducts()` de la clase `ProductBLL` y para actualizar los datos del producto mediante la sobrecarga de `UpdateProduct` que acabamos de crear. En las pestañas insertar y eliminar, seleccione (ninguno) en las listas desplegables.

[![configurar ObjectDataSource para usar la sobrecarga UpdateProduct que acaba de crear](customizing-the-data-modification-interface-vb/_static/image5.png)](customizing-the-data-modification-interface-vb/_static/image4.png)

**Figura 2**: configuración de ObjectDataSource para usar la `UpdateProduct` sobrecarga que se acaba de crear ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image6.png))

Como hemos visto en los tutoriales de modificación de datos, la sintaxis declarativa para el origen de datos de origen creado por Visual Studio asigna la propiedad `OldValuesParameterFormatString` a `original_{0}`. Esto, por supuesto, no funcionará con nuestro nivel de lógica de negocios, ya que nuestros métodos no esperan que el valor de `ProductID` original se pase. Por lo tanto, como hemos hecho en los tutoriales anteriores, dedique un momento a quitar esta asignación de propiedad de la sintaxis declarativa o, en su lugar, establezca el valor de esta propiedad en `{0}`.

Después de este cambio, el marcado declarativo de ObjectDataSource debería tener un aspecto similar al siguiente:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample2.aspx)]

Tenga en cuenta que se ha quitado la propiedad `OldValuesParameterFormatString` y que hay un `Parameter` en la colección `UpdateParameters` para cada uno de los parámetros de entrada que espera nuestra sobrecarga de `UpdateProduct`.

Mientras que ObjectDataSource está configurado para actualizar solo un subconjunto de valores del producto, GridView muestra actualmente *todos* los campos del producto. Dedique un momento a editar GridView para que:

- Solo incluye el `ProductName`, `SupplierName``CategoryName` BoundFields y el `Discontinued` CheckBoxField
- Los campos `CategoryName` y `SupplierName` que aparecen antes (a la izquierda) del `Discontinued` CheckBoxField
- La propiedad `CategoryName` y `SupplierName` BoundFields ' `HeaderText` está establecida en "categoría" y "proveedor", respectivamente.
- La compatibilidad con la edición está habilitada (Active la casilla habilitar edición en la etiqueta inteligente de GridView)

Después de estos cambios, el diseñador tendrá un aspecto similar al de la figura 3, con la sintaxis declarativa de GridView que se muestra a continuación.

[![quitar los campos innecesarios de GridView](customizing-the-data-modification-interface-vb/_static/image8.png)](customizing-the-data-modification-interface-vb/_static/image7.png)

**Figura 3**: quitar los campos innecesarios de GridView ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image9.png))

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample3.aspx)]

En este momento se completa el comportamiento de solo lectura de GridView. Al ver los datos, cada producto se representa como una fila en GridView, mostrando el nombre del producto, la categoría, el proveedor y el estado descontinuado.

[![se ha completado la interfaz de solo lectura de GridView](customizing-the-data-modification-interface-vb/_static/image11.png)](customizing-the-data-modification-interface-vb/_static/image10.png)

**Figura 4**: la interfaz de solo lectura de GridView se ha completado ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image12.png))

> [!NOTE]
> Como se describe en [información general sobre la inserción, actualización y eliminación de datos](an-overview-of-inserting-updating-and-deleting-data-cs.md), es importante que el estado de vista de GridView esté habilitado (comportamiento predeterminado). Si establece la propiedad GridView s `EnableViewState` en `false`, corre el riesgo de que los usuarios simultáneos eliminen o editen registros involuntariamente. Vea [ADVERTENCIA: problema de simultaneidad con ASP.NET 2,0 GridView/DetailsView/FormViews que admiten la edición o eliminación y cuyo estado de vista está deshabilitado](http://scottonwriting.net/sowblog/posts/10054.aspx) para obtener más información.

## <a name="step-3-using-a-dropdownlist-for-the-category-and-supplier-editing-interfaces"></a>Paso 3: usar un DropDownList para las interfaces de edición de categorías e proveedores

Recuerde que el objeto de `ProductsRow` contiene las propiedades `CategoryID`, `CategoryName`, `SupplierID`y `SupplierName`, que proporcionan los valores de identificador de clave externa reales en la tabla de base de datos `Products` y los valores `Name` correspondientes en las tablas `Categories` y `Suppliers`. Los `CategoryID` y `SupplierID` del `ProductRow`pueden leerse y escribirse en ellos, mientras que las propiedades `CategoryName` y `SupplierName` están marcadas como de solo lectura.

Debido al estado de solo lectura de las propiedades `CategoryName` y `SupplierName`, el BoundFields correspondiente ha tenido su propiedad `ReadOnly` establecida en `True`, lo que impide que estos valores se modifiquen cuando se edita una fila. Aunque podemos establecer la propiedad `ReadOnly` en `False`, representando el `CategoryName` y `SupplierName` BoundFields como cuadros de texto durante la edición, este método producirá una excepción cuando el usuario intente actualizar el producto, ya que no hay ninguna sobrecarga de `UpdateProduct` que toma `CategoryName` y `SupplierName` entradas. De hecho, no queremos crear este tipo de sobrecarga por dos motivos:

- La tabla `Products` no tiene `SupplierName` ni `CategoryName` campos, sino `SupplierID` y `CategoryID`. Por lo tanto, queremos que nuestro método pase estos valores de identificador concretos, no los valores de sus tablas de búsqueda.
- Requerir que el usuario escriba el nombre del proveedor o la categoría es menor que el ideal, ya que requiere que el usuario Conozca las categorías y los proveedores disponibles y sus correcciones correctas.

Los campos proveedor y categoría deben mostrar los nombres de las categorías y los proveedores en el modo de solo lectura (como ahora lo hace) y una lista desplegable de las opciones aplicables cuando se editan. Mediante el uso de una lista desplegable, el usuario final puede ver rápidamente qué categorías y proveedores están disponibles para elegir entre y puede realizar su selección más fácilmente.

Para proporcionar este comportamiento, es necesario convertir el `SupplierName` y `CategoryName` BoundFields en TemplateFields cuyo `ItemTemplate` emite los valores `SupplierName` y `CategoryName` y cuyo `EditItemTemplate` usa un control DropDownList para enumerar las categorías y los proveedores disponibles.

## <a name="adding-thecategoriesandsuppliersdropdownlists"></a>Agregar el`Categories`y`Suppliers`DropDownLists

Para empezar, convierta el `SupplierName` y `CategoryName` BoundFields en TemplateFields; para ello, haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView; seleccionar BoundField en la lista de la parte inferior izquierda; y haciendo clic en el vínculo "convertir este campo en TemplateField". El proceso de conversión creará un TemplateField con un `ItemTemplate` y un `EditItemTemplate`, tal y como se muestra en la sintaxis declarativa siguiente:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample4.aspx)]

Como BoundField se marcó como de solo lectura, tanto el `ItemTemplate` como el `EditItemTemplate` contienen un control Web Label cuya propiedad `Text` está enlazada al campo de datos aplicable (`CategoryName`, en la sintaxis anterior). Es necesario modificar el `EditItemTemplate`, reemplazando el control Web Label por un control DropDownList.

Como hemos visto en los tutoriales anteriores, la plantilla se puede editar a través del diseñador o directamente desde la sintaxis declarativa. Para editarlo a través del diseñador, haga clic en el vínculo editar plantillas de la etiqueta inteligente de GridView y elija trabajar con el `EditItemTemplate`del campo de la categoría. Quite el control Web Label y reemplácelo por un control DropDownList y establezca la propiedad ID de DropDownList en `Categories`.

[![quitar electrónicos y agregar un DropDownList a la EditItemTemplate](customizing-the-data-modification-interface-vb/_static/image14.png)](customizing-the-data-modification-interface-vb/_static/image13.png)

**Figura 5**: Quite electrónicos y agregue un DropDownList al `EditItemTemplate` ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image15.png))

A continuación, es necesario rellenar el DropDownList con las categorías disponibles. Haga clic en el vínculo elegir origen de datos de la etiqueta inteligente de DropDownList y opte por crear un nuevo ObjectDataSource denominado `CategoriesDataSource`.

[![crear un nuevo control ObjectDataSource denominado CategoriesDataSource](customizing-the-data-modification-interface-vb/_static/image17.png)](customizing-the-data-modification-interface-vb/_static/image16.png)

**Figura 6**: crear un nuevo control ObjectDataSource denominado `CategoriesDataSource` ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image18.png))

Para que esta ObjectDataSource Devuelva todas las categorías, enlácelo al método `GetCategories()` de la clase `CategoriesBLL`.

[![enlazar ObjectDataSource al método GetCategories () de CategoriesBLL](customizing-the-data-modification-interface-vb/_static/image20.png)](customizing-the-data-modification-interface-vb/_static/image19.png)

**Figura 7**: enlace del origen ObjectDataSource al método `GetCategories()` del `CategoriesBLL`([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image21.png))

Por último, configure las opciones de DropDownList de modo que el campo `CategoryName` se muestre en cada `ListItem` de DropDownList con el campo `CategoryID` que se usa como valor.

[![tener el campo CategoryName mostrado y el IdCategoría usado como valor](customizing-the-data-modification-interface-vb/_static/image23.png)](customizing-the-data-modification-interface-vb/_static/image22.png)

**Figura 8**: haga que se muestre el campo `CategoryName` y que el `CategoryID` se use como valor ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image24.png))

Después de realizar estos cambios, el marcado declarativo para el `EditItemTemplate` en la `CategoryName` TemplateField incluirá tanto DropDownList como ObjectDataSource:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample5.aspx)]

> [!NOTE]
> El DropDownList del `EditItemTemplate` debe tener habilitado su estado de vista. Pronto vamos a agregar sintaxis de enlace de los comandos a la sintaxis declarativa de DropDownList y a los comandos de enlace de los comandos, como `Eval()` y `Bind()` solo pueden aparecer en controles cuyo estado de vista está habilitado.

Repita estos pasos para agregar un DropDownList denominado `Suppliers` al `EditItemTemplate`de `SupplierName` TemplateField. Esto implicará agregar un DropDownList al `EditItemTemplate` y crear otro ObjectDataSource. Sin embargo, el ObjectDataSource de `Suppliers` DropDownList debe configurarse para invocar el método `GetSuppliers()` de la clase `SuppliersBLL`. Además, configure el `Suppliers` DropDownList para que muestre el campo `CompanyName` y use el campo `SupplierID` como valor para sus `ListItem` s.

Después de agregar el DropDownLists a los dos `EditItemTemplate` s, cargue la página en un explorador y haga clic en el botón Editar del producto Cajun Seasoning de chef Anton. Como se muestra en la figura 9, las columnas de categoría y proveedor del producto se representan como listas desplegables que contienen las categorías y proveedores disponibles entre los que elegir. Sin embargo, tenga en cuenta que los *primeros* elementos de ambas listas desplegables están seleccionados de forma predeterminada (bebidas para la categoría y los líquidos exóticos como proveedor), aunque la estación de Cajun de chef Anton es un condimento proporcionado por nuevas inluces de Orleans.

[![el primer elemento de las listas desplegables está seleccionado de forma predeterminada](customizing-the-data-modification-interface-vb/_static/image26.png)](customizing-the-data-modification-interface-vb/_static/image25.png)

**Figura 9**: el primer elemento de las listas desplegables está seleccionado de forma predeterminada ([haga clic para ver la imagen a tamaño completo](customizing-the-data-modification-interface-vb/_static/image27.png))

Además, si hace clic en actualizar, observará que los valores `CategoryID` y `SupplierID` del producto están establecidos en `NULL`. Ambos comportamientos no deseados se deben a que los DropDownLists de los `EditItemTemplate` s no están enlazados a ningún campo de datos de los datos de productos subyacentes.

## <a name="binding-the-dropdownlists-to-thecategoryidandsupplieriddata-fields"></a>Enlazar el DropDownLists con los campos de datos`CategoryID`y`SupplierID`

Para que las listas desplegables categoría y proveedor del producto editado se establezcan en los valores adecuados y para que estos valores se devuelvan al método `UpdateProduct` de la capa BLL al hacer clic en actualizar, es necesario enlazar las propiedades de `SelectedValue` DropDownLists a los campos de datos `CategoryID` y `SupplierID` mediante el enlace de datos bidireccional. Para lograr esto con el `Categories` DropDownList, puede Agregar `SelectedValue='<%# Bind("CategoryID") %>'` directamente a la sintaxis declarativa.

Como alternativa, puede establecer los enlaces de los DataBindings de DropDownList editando la plantilla a través del diseñador y haciendo clic en el vínculo editar DataBindings de la etiqueta inteligente de DropDownList. A continuación, indique que la propiedad `SelectedValue` se debe enlazar al campo `CategoryID` mediante el enlace de los dos sentidos (vea la figura 10). Repita el proceso declarativo o del diseñador para enlazar el `SupplierID` campo de datos al `Suppliers` DropDownList.

[![enlazar CategoryID a la propiedad SelectedValue de DropDownList mediante el enlace de los dos sentidos](customizing-the-data-modification-interface-vb/_static/image29.png)](customizing-the-data-modification-interface-vb/_static/image28.png)

**Figura 10**: enlace del `CategoryID` a la propiedad `SelectedValue` de DropDownList mediante el enlace de los dos sentidos ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image30.png))

Una vez que se han aplicado los enlaces a las propiedades de `SelectedValue` de los dos DropDownLists, la categoría del producto editada y las columnas de proveedor tendrán como valor predeterminado los valores del producto actual. Al hacer clic en actualizar, los valores `CategoryID` y `SupplierID` del elemento de lista desplegable seleccionado se pasarán al método `UpdateProduct`. En la figura 11 se muestra el tutorial una vez agregadas las instrucciones de DataBinding; Tenga en cuenta que los elementos de la lista desplegable seleccionados para la estacionalización de Chef Jesús son correctos y la Nueva Orleans Cajun se desenciende.

[![la categoría actual y los valores de proveedor del producto editado están seleccionados de forma predeterminada](customizing-the-data-modification-interface-vb/_static/image32.png)](customizing-the-data-modification-interface-vb/_static/image31.png)

**Figura 11**: los valores de categoría y proveedor actuales del producto editado se seleccionan de forma predeterminada ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image33.png))

## <a name="handlingnullvalues"></a>Controlar los valores de`NULL`

Las columnas `CategoryID` y `SupplierID` de la tabla `Products` se pueden `NULL`, pero el DropDownLists de la `EditItemTemplate` s no incluye un elemento de lista para representar un valor `NULL`. Esto tiene dos consecuencias:

- El usuario no puede usar nuestra interfaz para cambiar la categoría o el proveedor de un producto de un valor no`NULL` a un `NULL`
- Si un producto tiene un `NULL` `CategoryID` o `SupplierID`, al hacer clic en el botón Editar se producirá una excepción. Esto se debe a que el `NULL` valor devuelto por `CategoryID` (o `SupplierID`) en la instrucción `Bind()` no se asigna a un valor de DropDownList (el DropDownList inicia una excepción cuando su propiedad `SelectedValue` está establecida en un valor que *no* está en su colección de elementos de lista).

Con el fin de admitir `NULL` valores de `CategoryID` y `SupplierID`, es necesario agregar otro `ListItem` a cada DropDownList para representar el valor de `NULL`. En el tutorial [maestro y detalle filtrado con DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) , vimos cómo agregar un `ListItem` adicional a un DropDownList de DataBound, que implica establecer la propiedad `AppendDataBoundItems` de DropDownList en `True` y agregar manualmente el `ListItem`adicional. En ese tutorial anterior, sin embargo, agregamos un `ListItem` con un `Value` de `-1`. Sin embargo, la lógica de DataBinding en ASP.NET convertirá automáticamente una cadena en blanco en un valor `NULL` y viceversa. Por lo tanto, para este tutorial queremos que el `Value` del `ListItem`sea una cadena vacía.

Para empezar, establezca la propiedad DropDownLists ' `AppendDataBoundItems` en `True`. A continuación, agregue el `ListItem` `NULL` agregando el siguiente elemento `<asp:ListItem>` a cada DropDownList para que el marcado declarativo tenga el aspecto siguiente:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample6.aspx)]

He elegido usar "(ninguno)" como valor de texto para este `ListItem`, pero puede cambiarlo para que también sea una cadena en blanco si lo desea.

> [!NOTE]
> Como vimos en el tutorial *maestro y detalle filtrado con DropDownList* , `ListItem` s se pueden agregar a un DropDownList a través del diseñador haciendo clic en la propiedad `Items` de DropDownList en el ventana Propiedades (que mostrará el editor de la colección `ListItem`). Sin embargo, asegúrese de agregar el `ListItem` de `NULL` de este tutorial a través de la sintaxis declarativa. Si utiliza el editor de la colección de `ListItem`, la sintaxis declarativa generada omitirá la configuración de `Value`, cuando se le asigne una cadena en blanco, creando un marcado declarativo como: `<asp:ListItem>(None)</asp:ListItem>`. Aunque esto puede parecer inofensivo, el valor que falta hace que DropDownList use el valor de la propiedad `Text` en su lugar. Esto significa que si se selecciona esta `NULL` `ListItem`, se intentará asignar el valor "(ninguno)" al `CategoryID`, lo que producirá una excepción. Al establecer `Value=""`explícitamente, se asignará un valor de `NULL` a `CategoryID` cuando se seleccione el `ListItem` de `NULL`.

Repita estos pasos para el DropDownList de proveedores.

Con este `ListItem`adicional, la interfaz de edición puede asignar ahora `NULL` valores a los campos `CategoryID` y `SupplierID` de un producto, como se muestra en la figura 12.

[![elegir (ninguno) para asignar un valor nulo a la categoría o proveedor de un producto](customizing-the-data-modification-interface-vb/_static/image35.png)](customizing-the-data-modification-interface-vb/_static/image34.png)

**Figura 12**: seleccione (ninguno) para asignar un valor de `NULL` para la categoría o proveedor de un producto ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image36.png))

## <a name="step-4-using-radiobuttons-for-the-discontinued-status"></a>Paso 4: uso de RadioButtons para el estado de descontinuado

Actualmente, el campo de datos de los productos `Discontinued` se expresa mediante un CheckBoxField, que representa una casilla deshabilitada para las filas de solo lectura y una casilla habilitada para la fila que se está editando. Aunque esta interfaz de usuario suele ser adecuada, podemos personalizarla si es necesario con TemplateField. En este tutorial, vamos a cambiar CheckBoxField a TemplateField que usa un control RadioButtonList con dos opciones "Active" y "discontinued" desde las que el usuario puede especificar el valor de `Discontinued` del producto.

Empiece por convertir el `Discontinued` CheckBoxField en TemplateField, lo que creará un TemplateField con un `ItemTemplate` y `EditItemTemplate`. Ambas plantillas incluyen una casilla con su `Checked` propiedad enlazada al campo de datos `Discontinued`, la única diferencia entre las dos es que la propiedad `Enabled` de la casilla del `ItemTemplate`está establecida en `False`.

Reemplace la casilla en el `ItemTemplate` y `EditItemTemplate` con un control RadioButtonList, estableciendo ambas propiedades de RadioButtonLists ' `ID` en `DiscontinuedChoice`. A continuación, indique que el RadioButtonLists debe contener dos botones de radio, uno con la etiqueta "Active" con un valor de "false" y otro con la etiqueta "discontinued" con un valor de "true". Para ello, puede escribir los elementos `<asp:ListItem>` en directamente a través de la sintaxis declarativa o usar el editor de la colección `ListItem` desde el diseñador. En la figura 13 se muestra el editor de la colección `ListItem` una vez especificadas las dos opciones de botón de radio.

[![agregar opciones activas y no continuadas a RadioButtonList](customizing-the-data-modification-interface-vb/_static/image38.png)](customizing-the-data-modification-interface-vb/_static/image37.png)

**Figura 13**: agregar opciones activas y no continuadas a RadioButtonList ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image39.png))

Puesto que el RadioButtonList en el `ItemTemplate` no debe ser editable, establezca su propiedad `Enabled` en `False`, mientras que la propiedad `Enabled` se `True` (valor predeterminado) para RadioButtonList en el `EditItemTemplate`. Esto hará que los botones de radio de la fila no editada sean de solo lectura, pero permitirá al usuario cambiar los valores de RadioButton de la fila modificada.

Todavía necesitamos asignar las propiedades de `SelectedValue` de los controles RadioButtonList para que se seleccione el botón de radio adecuado en función del campo de datos `Discontinued` del producto. Como con el DropDownLists examinado anteriormente en este tutorial, esta sintaxis de enlace de los enlaces se puede agregar directamente en el marcado declarativo o a través del vínculo de edición de enlaces de DataBindings en las etiquetas inteligentes de RadioButtonLists.

Después de agregar los dos RadioButtonLists y configurarlos, el marcado declarativo `Discontinued` TemplateField debe ser similar al siguiente:

[!code-aspx[Main](customizing-the-data-modification-interface-vb/samples/sample7.aspx)]

Con estos cambios, la columna de `Discontinued` se ha transformado de una lista de casillas a una lista de pares de botones de radio (vea la figura 14). Al editar un producto, se selecciona el botón de radio correspondiente y el estado de descontinuado del producto puede actualizarse seleccionando el botón de radio otro y haciendo clic en actualizar.

[![las casillas no incluidas se han reemplazado por pares de botones de radio](customizing-the-data-modification-interface-vb/_static/image41.png)](customizing-the-data-modification-interface-vb/_static/image40.png)

**Figura 14**: las casillas no incluidas se han reemplazado por pares de botones de radio ([haga clic para ver la imagen de tamaño completo](customizing-the-data-modification-interface-vb/_static/image42.png))

> [!NOTE]
> Dado que la columna de `Discontinued` de la base de datos `Products` no puede tener valores `NULL`, no es necesario preocuparse por la captura de `NULL` información en la interfaz. Sin embargo, si `Discontinued` columna podría contener `NULL` valores, queremos agregar un tercer botón de radio a la lista con su `Value` establecido en una cadena vacía (`Value=""`), como con la categoría y el proveedor DropDownLists.

## <a name="summary"></a>Resumen

Aunque BoundField y CheckBoxField representan automáticamente interfaces de solo lectura, edición e inserción, carecen de la capacidad de personalización. Sin embargo, a menudo es necesario personalizar la interfaz de edición o inserción, quizás agregando controles de validación (como vimos en el tutorial anterior) o personalizando la interfaz de usuario de recopilación de datos (como vimos en este tutorial). La personalización de la interfaz con TemplateField se puede sumar en los pasos siguientes:

1. Agregue TemplateField o convierta un Existing o CheckBoxField existente en TemplateField
2. Aumentar la interfaz según sea necesario
3. Enlazar los campos de datos adecuados a los controles Web recién agregados mediante el enlace de datos bidireccional

Además de usar los controles Web ASP.NET integrados, también puede personalizar las plantillas de un TemplateField con controles de usuario y controles de servidor compilados personalizados.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)
> [Siguiente](implementing-optimistic-concurrency-vb.md)
