---
uid: web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
title: Actualización por lotes (VB) | Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo actualizar varios registros de base de datos en una sola operación. En la capa de la interfaz de usuario, se crea un GridView donde cada fila es editable. En los datos...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: d191a204-d7ea-458d-b81c-0b9049ecb55f
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/batch-updating-vb
msc.type: authoredcontent
ms.openlocfilehash: f0bb83b17585876dd6d28a5893a223cce15da31d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78501313"
---
# <a name="batch-updating-vb"></a>Actualización por lotes (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_64_VB.zip) o [Descargar PDF](batch-updating-vb/_static/datatutorial64vb1.pdf)

> Obtenga información sobre cómo actualizar varios registros de base de datos en una sola operación. En la capa de la interfaz de usuario, se crea un GridView donde cada fila es editable. En la capa de acceso a datos se ajustan las distintas operaciones de actualización dentro de una transacción para garantizar que todas las actualizaciones se realizan correctamente o se revierten todas las actualizaciones.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](wrapping-database-modifications-within-a-transaction-vb.md) , vimos cómo extender el nivel de acceso a datos para agregar compatibilidad con transacciones de bases de datos. Las transacciones de base de datos garantizan que una serie de instrucciones de modificación de datos se tratará como una operación atómica, lo que garantiza que se producirá un error en todas las modificaciones o todas se realizarán correctamente. Gracias a esta funcionalidad DAL de bajo nivel, ya se ha preparado para activar la atención de la creación de interfaces de modificación de datos por lotes.

En este tutorial, vamos a crear un GridView en el que cada fila es editable (vea la ilustración 1). Puesto que cada fila se representa en su interfaz de edición, no se necesita una columna de botones editar, actualizar y cancelar. En su lugar, hay dos botones actualizar productos en la página que, cuando se hace clic en ellos, enumeran las filas de GridView y actualizan la base de datos.

[![cada fila de GridView es editable](batch-updating-vb/_static/image1.gif)](batch-updating-vb/_static/image1.png)

**Figura 1**: cada fila de GridView es editable ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image2.png))

Vamos a empezar a trabajar.

> [!NOTE]
> En el tutorial [realización de actualizaciones por lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) , hemos creado una interfaz de edición de lotes mediante el control DataList. Este tutorial difiere del anterior en que usa GridView y la actualización por lotes se realiza en el ámbito de una transacción. Después de completar este tutorial, le recomendamos que vuelva al tutorial anterior y lo actualice para usar la funcionalidad relacionada con la transacción de base de datos agregada en el tutorial anterior.

## <a name="examining-the-steps-for-making-all-gridview-rows-editable"></a>Examinar los pasos para que todas las filas de GridView sean editables

Como se describe en [información general sobre la inserción, actualización y eliminación de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , el control GridView ofrece compatibilidad integrada para editar los datos subyacentes por fila. Internamente, el control GridView anota qué fila es editable a través de su [propiedad`EditIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.editindex(VS.80).aspx). A medida que GridView se enlaza a su origen de datos, comprueba cada fila para ver si el índice de la fila es igual al valor de `EditIndex`. Si es así, los campos de fila se representan mediante sus interfaces de edición. En el caso de BoundFields, la interfaz de edición es un cuadro de texto cuya propiedad `Text` tiene asignado el valor del campo de datos especificado por la propiedad BoundField `DataField`. En el caso de TemplateFields, se utiliza el `EditItemTemplate` en lugar del `ItemTemplate`.

Recuerde que el flujo de trabajo de edición se inicia cuando un usuario hace clic en el botón Editar de una fila. Esto hace que un postback, establezca la propiedad GridView s `EditIndex` en el índice de la fila en la que se hizo clic y reenlace los datos a la cuadrícula. Cuando se hace clic en un botón Cancelar fila, en el PostBack se establece el `EditIndex` en un valor de `-1` antes de reenlazar los datos a la cuadrícula. Dado que las filas de GridView empiezan a indizar a cero, el establecimiento de `EditIndex` en `-1` tiene el efecto de mostrar GridView en modo de solo lectura.

La propiedad `EditIndex` funciona bien para la edición por fila, pero no está diseñada para la edición por lotes. Para que el GridView completo sea editable, es necesario que cada fila se represente mediante su interfaz de edición. La forma más fácil de hacerlo es crear el lugar en el que cada campo editable se implementa como TemplateField con su interfaz de edición definida en el `ItemTemplate`.

En los siguientes pasos, vamos a crear un GridView totalmente editable. En el paso 1 comenzaremos por crear GridView y su ObjectDataSource y convertiremos sus BoundFields y CheckBoxField en TemplateFields. En los pasos 2 y 3, trasladaremos las interfaces de edición de TemplateFields `EditItemTemplate` s a sus `ItemTemplate` s.

## <a name="step-1-displaying-product-information"></a>Paso 1: Mostrar la información del producto

Antes de preocuparnos por la creación de un control GridView, donde las filas son editables, deje que se empiece por mostrar simplemente la información del producto. Abra la página `BatchUpdate.aspx` en la carpeta `BatchData` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador. Establezca el `ID` de GridView en `ProductsGrid` y, desde su etiqueta inteligente, elija enlazarlo a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configure ObjectDataSource para recuperar sus datos del método `ProductsBLL` Class s `GetProducts`.

[![configurar ObjectDataSource para usar la clase ProductsBLL](batch-updating-vb/_static/image2.gif)](batch-updating-vb/_static/image3.png)

**Figura 2**: configuración de ObjectDataSource para usar la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image4.png))

[![recuperar los datos del producto mediante el método GetProducts](batch-updating-vb/_static/image3.gif)](batch-updating-vb/_static/image5.png)

**Figura 3**: recuperación de los datos del producto con el método `GetProducts` ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image6.png))

Al igual que GridView, las características de modificación de ObjectDataSource s están diseñadas para funcionar por fila. Para actualizar un conjunto de registros, es necesario escribir un poco de código en la clase de código subyacente de la página ASP.NET que procesa por lotes los datos y los pasa a la capa BLL. Por lo tanto, establezca las listas desplegables de las pestañas de la actualización, inserción y eliminación de ObjectDataSource en (ninguna). Haga clic en Finalizar para completar el asistente.

[![establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna)](batch-updating-vb/_static/image4.gif)](batch-updating-vb/_static/image7.png)

**Figura 4**: establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](batch-updating-vb/_static/image8.png))

Después de completar el Asistente para configurar el origen de datos, el marcado declarativo de ObjectDataSource s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](batch-updating-vb/samples/sample1.aspx)]

Al completar el Asistente para configurar origen de datos, también se hace que Visual Studio cree BoundFields y un CheckBoxField para los campos de datos del producto en GridView. Para este tutorial, permita que el usuario solo permita ver y editar el nombre del producto, la categoría, el precio y el estado no incluida. Quite todos los campos excepto los `ProductName`, `CategoryName`, `UnitPrice`y `Discontinued`, y cambie el nombre de las propiedades `HeaderText` de los tres primeros campos a producto, categoría y precio, respectivamente. Por último, active las casillas habilitar paginación y habilitar ordenación en la etiqueta inteligente de GridView s.

En este momento, GridView tiene tres BoundFields (`ProductName`, `CategoryName`y `UnitPrice`) y CheckBoxField (`Discontinued`). Necesitamos convertir estos cuatro campos en TemplateFields y, a continuación, pasar la interfaz de edición de la `EditItemTemplate` TemplateField s a su `ItemTemplate`.

> [!NOTE]
> Hemos explorado la creación y la personalización de TemplateFields en el tutorial [Personalización de la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) . Le guiaremos por los pasos necesarios para convertir BoundFields y CheckBoxField en TemplateFields y definir sus interfaces de edición en sus `ItemTemplate` s, pero si se queda atascado o necesita un actualizador, no dude en volver a consultar este tutorial anterior.

En la etiqueta inteligente de GridView s, haga clic en el vínculo editar columnas para abrir el cuadro de diálogo campos. A continuación, seleccione cada campo y haga clic en el vínculo convertir este campo en un TemplateField.

![Convertir los BoundFields existentes y CheckBoxField en TemplateFields](batch-updating-vb/_static/image5.gif)

**Figura 5**: conversión de los BoundFields existentes y CheckBoxField en TemplateFields

Ahora que cada campo es TemplateField, se vuelve a preparar para pasar la interfaz de edición de los `EditItemTemplate` s al `ItemTemplate` s.

## <a name="step-2-creating-theproductnameunitprice-anddiscontinuedediting-interfaces"></a>Paso 2: crear las interfaces de edición de`ProductName`,`UnitPrice`y`Discontinued`

Crear las interfaces de edición de `ProductName`, `UnitPrice`y `Discontinued` es el tema de este paso y son bastante sencillos, ya que cada interfaz ya está definida en el `EditItemTemplate`de TemplateField. Crear la interfaz de edición de `CategoryName` es un poco más complicado, ya que necesitamos crear un DropDownList de las categorías aplicables. Esta interfaz de edición de `CategoryName` se aborda en el paso 3.

Comencemos con la `ProductName` TemplateField. Haga clic en el vínculo editar plantillas de la etiqueta inteligente de GridView s y explore en profundidad hasta el `ProductName` TemplateField s `EditItemTemplate`. Seleccione el cuadro de texto, cópielo en el portapapeles y, a continuación, péguelo en el `ProductName` TemplateField s `ItemTemplate`. Cambie la propiedad TextBox s `ID` a `ProductName`.

A continuación, agregue un RequiredFieldValidator al `ItemTemplate` para asegurarse de que el usuario proporciona un valor para cada nombre de producto. Establezca la propiedad `ControlToValidate` en ProductName, la propiedad `ErrorMessage` en debe proporcionar el nombre del producto. y la propiedad `Text` en \*. Después de hacer estas adiciones al `ItemTemplate`, la pantalla debe tener un aspecto similar al de la figura 6.

[![el TemplateField de ProductName incluye ahora un TextBox y un RequiredFieldValidator](batch-updating-vb/_static/image6.gif)](batch-updating-vb/_static/image9.png)

**Figura 6**: el `ProductName` TemplateField ahora incluye un cuadro de texto y un RequiredFieldValidator ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image10.png))

Para la interfaz de edición de `UnitPrice`, empiece por copiar el cuadro de texto del `EditItemTemplate` al `ItemTemplate`. Después, coloque un $ delante del cuadro de texto y establezca su propiedad `ID` en UnitPrice y su propiedad `Columns` en 8.

Agregue también un CompareValidator al `ItemTemplate` `UnitPrice` s para asegurarse de que el valor especificado por el usuario es un valor de moneda válido mayor o igual que $0,00. Establezca la propiedad validadores s `ControlToValidate` en UnitPrice, su propiedad `ErrorMessage` en debe especificar un valor de moneda válido. Omita los símbolos de divisa., su propiedad `Text` en \*, su propiedad `Type` en `Currency`, su `Operator` propiedad `GreaterThanEqual`y su propiedad `ValueToCompare` en 0.

[![agregar un CompareValidator para asegurarse de que el precio introducido es un valor de moneda no negativo](batch-updating-vb/_static/image7.gif)](batch-updating-vb/_static/image11.png)

**Figura 7**: agregar un CompareValidator para asegurarse de que el precio introducido es un valor de moneda no negativo ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image12.png))

En el `Discontinued` TemplateField puede usar la casilla ya definida en el `ItemTemplate`. Simplemente establezca su `ID` en Discontinued y su propiedad `Enabled` en `True`.

## <a name="step-3-creating-thecategorynameediting-interface"></a>Paso 3: creación de la interfaz de edición de`CategoryName`

La interfaz de edición del `CategoryName` TemplateField s `EditItemTemplate` contiene un cuadro de texto que muestra el valor del campo de datos `CategoryName`. Necesitamos reemplazarlo por un DropDownList que enumere las categorías posibles.

> [!NOTE]
> El tutorial [Personalización de la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) contiene una explicación más detallada y completa sobre cómo personalizar una plantilla para incluir un DropDownList en lugar de un cuadro de texto. Aunque los pasos descritos aquí están completos, se presentan tersely. Para obtener información más detallada sobre la creación y configuración de las categorías DropDownList, consulte el tutorial [Personalización de la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) .

Arrastre un DropDownList desde el cuadro de herramientas hasta el `CategoryName` TemplateField s `ItemTemplate`y establezca su `ID` en `Categories`. En este punto, normalmente se definirá el origen de datos DropDownLists s a través de su etiqueta inteligente, creando un nuevo ObjectDataSource. Sin embargo, esto agregará el origen ObjectDataSource en el `ItemTemplate`, lo que producirá una instancia de ObjectDataSource creada para cada fila de GridView. En su lugar, permita que s cree el origen ObjectDataSource fuera del TemplateFields de GridView. Finalice la edición de la plantilla y arrastre un ObjectDataSource desde el cuadro de herramientas hasta el diseñador debajo del `ProductsDataSource` ObjectDataSource. Asigne a la nueva `CategoriesDataSource` de origen el nombre y configúrela para que use el método de `GetCategories` `CategoriesBLL` Class s.

[![configurar ObjectDataSource para usar la clase CategoriesBLL](batch-updating-vb/_static/image8.gif)](batch-updating-vb/_static/image13.png)

**Figura 8**: configuración de ObjectDataSource para usar la clase `CategoriesBLL` ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image14.png))

[![recuperar los datos de la categoría mediante el método GetCategories](batch-updating-vb/_static/image9.gif)](batch-updating-vb/_static/image15.png)

**Figura 9**: recuperación de los datos de la categoría mediante el método `GetCategories` ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image16.png))

Como este ObjectDataSource se usa simplemente para recuperar datos, establezca las listas desplegables de las pestañas actualizar y eliminar en (ninguno). Haga clic en Finalizar para completar el asistente.

[![establecer las listas desplegables de las pestañas actualizar y eliminar en (ninguna)](batch-updating-vb/_static/image10.gif)](batch-updating-vb/_static/image17.png)

**Figura 10**: establecer las listas desplegables de las pestañas actualizar y eliminar en (ninguna) ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image18.png))

Después de completar el asistente, el marcado declarativo `CategoriesDataSource` s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](batch-updating-vb/samples/sample2.aspx)]

Con el `CategoriesDataSource` creado y configurado, vuelva al `CategoryName` TemplateField s `ItemTemplate` y, en la etiqueta inteligente DropDownList s, haga clic en el vínculo elegir origen de datos. En el Asistente para la configuración de orígenes de datos, seleccione la opción `CategoriesDataSource` de la primera lista desplegable y elija que se usen `CategoryName` para la presentación y `CategoryID` como valor.

[![enlazar el DropDownList a CategoriesDataSource](batch-updating-vb/_static/image11.gif)](batch-updating-vb/_static/image19.png)

**Figura 11**: enlace de DropDownList al `CategoriesDataSource` ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image20.png))

En este momento, la `Categories` DropDownList enumera todas las categorías, pero aún no selecciona automáticamente la categoría adecuada para el producto enlazado a la fila de GridView. Para ello, es necesario establecer el `Categories` DropDownList s `SelectedValue` en el valor Product s `CategoryID`. Haga clic en el vínculo editar DataBindings de la etiqueta inteligente DropDownList s y asocie la propiedad `SelectedValue` al campo `CategoryID` Data como se muestra en la figura 12.

![Enlazar el valor de Product s CategoryID a la propiedad de DropDownList s SelectedValue](batch-updating-vb/_static/image12.gif)

**Figura 12**: enlace del valor de `CategoryID` del producto a la propiedad DropDownList s `SelectedValue`

Se mantiene un último problema: Si el producto no tiene un valor `CategoryID` especificado, la instrucción DataBinding en `SelectedValue` producirá una excepción. Esto se debe a que DropDownList solo contiene elementos para las categorías y no ofrece una opción para los productos que tienen un valor de base de datos `NULL` para `CategoryID`. Para solucionarlo, establezca la propiedad DropDownList s `AppendDataBoundItems` en `True` y agregue un nuevo elemento a DropDownList, omitiendo la propiedad `Value` de la sintaxis declarativa. Es decir, asegúrese de que la sintaxis declarativa `Categories` DropDownList s tenga el aspecto siguiente:

[!code-aspx[Main](batch-updating-vb/samples/sample3.aspx)]

Observe cómo el `<asp:ListItem Value="">`: Seleccione uno: tiene su atributo `Value` establecido explícitamente en una cadena vacía. Consulte el tutorial [Personalización de la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) para obtener una explicación más detallada sobre por qué se necesita este elemento DropDownList adicional para controlar el caso `NULL` y por qué la asignación de la propiedad `Value` a una cadena vacía es fundamental.

> [!NOTE]
> Hay un posible problema de rendimiento y escalabilidad aquí que merece la pena mencionar. Puesto que cada fila tiene un DropDownList que utiliza el `CategoriesDataSource` como origen de datos, el método `CategoriesBLL` clase s `GetCategories` se denominará *n* veces por página visita, donde *n* es el número de filas de GridView. Estas *n* llamadas a `GetCategories` producen *n* consultas en la base de datos. Este impacto en la base de datos podría reducirse mediante el almacenamiento en caché de las categorías devueltas en una caché por solicitud o a través de la capa de almacenamiento en caché mediante una dependencia de almacenamiento en caché de SQL o una expiración basada en tiempo muy breve. Para obtener más información sobre la opción de almacenamiento en caché por solicitud, consulte [`HttpContext.Items` de un almacén de caché por solicitud](http://aspnet.4guysfromrolla.com/articles/060904-1.aspx).

## <a name="step-4-completing-the-editing-interface"></a>Paso 4: finalización de la interfaz de edición

Hemos realizado una serie de cambios en las plantillas de GridView sin pausar para ver el progreso. Dedique un momento a ver nuestro progreso a través de un explorador. Como se muestra en la figura 13, cada fila se representa mediante su `ItemTemplate`, que contiene la interfaz de edición de la celda.

[![cada fila de GridView es editable](batch-updating-vb/_static/image13.gif)](batch-updating-vb/_static/image21.png)

**Figura 13**: cada fila de GridView es editable ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image22.png))

Hay algunos problemas de formato poco importantes que debemos tener en cuenta en este momento. En primer lugar, tenga en cuenta que el valor de `UnitPrice` contiene cuatro puntos decimales. Para corregir esto, vuelva al `UnitPrice` TemplateField s `ItemTemplate` y, en la etiqueta inteligente TextBox s, haga clic en el vínculo editar DataBindings. A continuación, especifique que la propiedad `Text` debe tener el formato de un número.

![Dar formato a la propiedad de texto como un número](batch-updating-vb/_static/image14.gif)

**Figura 14**: dar formato a la propiedad `Text` como un número

En segundo lugar, vamos a centrar la casilla en la columna de `Discontinued` (en lugar de alinearla a la izquierda). Haga clic en Editar columnas de la etiqueta inteligente de GridView s y seleccione el `Discontinued` TemplateField en la lista de campos en la esquina inferior izquierda. Explore en profundidad `ItemStyle` y establezca la propiedad `HorizontalAlign` en Center, tal como se muestra en la figura 15.

![Centrar la casilla descontinuada](batch-updating-vb/_static/image15.gif)

**Figura 15**: centrar la `Discontinued` casilla

Después, agregue un control ValidationSummary a la página y establezca su propiedad `ShowMessageBox` en `True` y su propiedad `ShowSummary` en `False`. Agregue también los controles Button web que, al hacer clic en él, actualizará los cambios de los usuarios. En concreto, agregue dos controles Web de botón, uno encima del GridView y otro debajo, estableciendo ambos controles `Text` propiedades para actualizar productos.

Dado que la interfaz de edición de GridView se define en su TemplateFields `ItemTemplate` s, las `EditItemTemplate` s son superfluas y se pueden eliminar.

Después de realizar los cambios de formato mencionados anteriormente, agregar los controles de botón y quitar los `EditItemTemplate` innecesarios, la sintaxis declarativa de la página debe ser similar a la siguiente:

[!code-aspx[Main](batch-updating-vb/samples/sample4.aspx)]

La figura 16 muestra esta página cuando se ve a través de un explorador después de haber agregado los controles de botón y los cambios de formato.

[![la página ahora incluye dos botones actualizar productos](batch-updating-vb/_static/image16.gif)](batch-updating-vb/_static/image23.png)

**Figura 16**: la página ahora incluye dos botones actualizar productos ([haga clic para ver la imagen de tamaño completo](batch-updating-vb/_static/image24.png))

## <a name="step-5-updating-the-products"></a>Paso 5: actualización de los productos

Cuando un usuario visita esta página, realizará sus modificaciones y, a continuación, hace clic en uno de los dos botones actualizar productos. En ese momento, es necesario guardar de algún modo los valores especificados por el usuario para cada fila en una instancia de `ProductsDataTable` y, a continuación, pasarlos a un método BLL que, a continuación, pasará esa instancia de `ProductsDataTable` al método `UpdateWithTransaction` de DAL. El método `UpdateWithTransaction`, que hemos creado en el [tutorial anterior](wrapping-database-modifications-within-a-transaction-vb.md), garantiza que el lote de cambios se actualizará como una operación atómica.

Cree un método denominado `BatchUpdate` en `BatchUpdate.aspx.vb` y agregue el código siguiente:

[!code-vb[Main](batch-updating-vb/samples/sample5.vb)]

Este método empieza por obtener todos los productos en un `ProductsDataTable` a través de una llamada al método de `GetProducts` de BLL. A continuación, se enumera la colección `ProductGrid` GridView s [`Rows`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rows(VS.80).aspx). La colección `Rows` contiene una [instancia de`GridViewRow`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewrow.aspx) para cada fila que se muestra en GridView. Como estamos mostrando como máximo diez filas por página, la colección GridView s `Rows` no tendrá más de diez elementos.

Para cada fila, el `ProductID` se captura de la colección `DataKeys` y se selecciona el `ProductsRow` adecuado en el `ProductsDataTable`. Se hace referencia mediante programación a los cuatro controles de entrada TemplateField y sus valores se asignan a las propiedades de `ProductsRow` instancia. Una vez que se han usado los valores de cada fila de GridView para actualizar el `ProductsDataTable`, se pasa al método BLL s `UpdateWithTransaction` que, como vimos en el tutorial anterior, simplemente llama al método de `UpdateWithTransaction` de DAL.

El algoritmo de actualización por lotes utilizado para este tutorial actualiza cada fila del `ProductsDataTable` que corresponde a una fila de GridView, independientemente de si se ha cambiado la información del producto. Aunque estas actualizaciones ciegas no suelen ser un problema de rendimiento, pueden conducir a registros superfluos si se vuelven a auditar los cambios en la tabla de base de datos. En el tutorial [realización de actualizaciones por lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) se ha explorado una interfaz de actualización por lotes con la lista de datos y se ha agregado código que solo actualizaría los registros que el usuario modificó realmente. No dude en usar las técnicas para [realizar actualizaciones por lotes](../editing-and-deleting-data-through-the-datalist/performing-batch-updates-vb.md) para actualizar el código de este tutorial, si lo desea.

> [!NOTE]
> Al enlazar el origen de datos a GridView a través de su etiqueta inteligente, Visual Studio asigna automáticamente los valores de la clave principal de los orígenes de datos a la propiedad `DataKeyNames` de GridView. Si no ha enlazado ObjectDataSource a GridView a través de la etiqueta inteligente de GridView, tal y como se describe en el paso 1, tendrá que establecer manualmente la propiedad GridView s `DataKeyNames` en ProductID para tener acceso al valor `ProductID` de cada fila a través de la colección `DataKeys`.

El código utilizado en `BatchUpdate` es similar al que se usa en los métodos de `UpdateProduct` de BLL, la diferencia principal es que en los métodos `UpdateProduct` solo se recupera de la arquitectura una única instancia de `ProductRow`. El código que asigna las propiedades del `ProductRow` es el mismo entre los métodos `UpdateProducts` y el código dentro del bucle `For Each` en `BatchUpdate`, como es el patrón global.

Para completar este tutorial, es necesario tener el método `BatchUpdate` invocado cuando se hace clic en cualquiera de los botones actualizar productos. Cree controladores de eventos para los eventos de `Click` de estos dos controles de botón y agregue el código siguiente en los controladores de eventos:

[!code-vb[Main](batch-updating-vb/samples/sample6.vb)]

En primer lugar, se realiza una llamada a `BatchUpdate`. A continuación, se usa la [propiedad`ClientScript`](https://msdn.microsoft.com/library/system.web.ui.page.clientscript(VS.80).aspx) para insertar JavaScript que mostrará un cuadro de mensajes que indica que los productos se han actualizado.

Tómese un minuto para probar este código. Visite `BatchUpdate.aspx` a través de un explorador, edite un número de filas y haga clic en uno de los botones actualizar productos. Suponiendo que no hay ningún error de validación de entrada, debería ver un cuadro de mensajes que indica que los productos se han actualizado. Para comprobar la atomicidad de la actualización, considere la posibilidad de agregar una restricción `CHECK` aleatoria, como una que no permita `UnitPrice` valores de 1234,56. A continuación, desde `BatchUpdate.aspx`, edite varios registros y asegúrese de establecer uno de los valores de `UnitPrice` del producto en el valor prohibido (1234,56). Esto debería producir un error al hacer clic en actualizar productos con los demás cambios durante la operación por lotes que se revierte a sus valores originales.

## <a name="an-alternativebatchupdatemethod"></a>Método`BatchUpdate`alternativo

El método `BatchUpdate` que se acaba de examinar recupera *todos* los productos del método de `GetProducts` BLL y, a continuación, actualiza solo los registros que aparecen en GridView. Este enfoque es idóneo si GridView no usa la paginación, pero si lo hace, puede haber cientos, miles o decenas de miles de productos, pero solo diez filas en GridView. En tal caso, obtener todos los productos de la base de datos solo para modificar 10 de ellos es menor que el ideal.

En el caso de estos tipos de situaciones, considere la posibilidad de usar en su lugar el siguiente método de `BatchUpdateAlternate`:

[!code-vb[Main](batch-updating-vb/samples/sample7.vb)]

`BatchMethodAlternate` comienza creando un nuevo `ProductsDataTable` vacío denominado `products`. A continuación, se recorre la colección de `Rows` de GridView y, para cada fila, se obtiene la información de producto determinada mediante el método de `GetProductByProductID(productID)` BLL s. La instancia de `ProductsRow` recuperada tiene sus propiedades actualizadas de la misma manera que `BatchUpdate`, pero después de actualizar la fila se importa en el `products` `ProductsDataTable` a través del método DataTable s [`ImportRow(DataRow)`](https://msdn.microsoft.com/library/system.data.datatable.importrow(VS.80).aspx).

Una vez completado el bucle de `For Each`, `products` contiene una instancia de `ProductsRow` para cada fila de GridView. Dado que cada una de las instancias de `ProductsRow` se han agregado al `products` (en lugar de actualizarse), si se pasa a un método `UpdateWithTransaction`, la `ProductsTableAdapter` intentará Insertar cada uno de los registros en la base de datos. En su lugar, es necesario especificar que cada una de estas filas se ha modificado (no se ha agregado).

Esto se puede lograr agregando un nuevo método a la capa BLL denominada `UpdateProductsWithTransaction`. `UpdateProductsWithTransaction`, que se muestra a continuación, establece el `RowState` de cada una de las instancias de `ProductsRow` de la `ProductsDataTable` en `Modified` y, a continuación, pasa el `ProductsDataTable` al método `UpdateWithTransaction` de DAL.

[!code-vb[Main](batch-updating-vb/samples/sample8.vb)]

## <a name="summary"></a>Resumen

GridView proporciona funciones de edición por fila integradas, pero carece de compatibilidad para crear interfaces totalmente modificables. Como vimos en este tutorial, estas interfaces son posibles, pero requieren un poco de trabajo. Para crear un GridView en el que cada fila sea editable, es necesario convertir los campos de GridView s en TemplateFields y definir la interfaz de edición en el `ItemTemplate` s. Además, se deben agregar los controles Web Button All-Type a la página, independientes de GridView. Estos botones `Click` los controladores de eventos tienen que enumerar la colección de `Rows` de GridView, almacenar los cambios en un `ProductsDataTable`y pasar la información actualizada al método BLL adecuado.

En el siguiente tutorial, veremos cómo crear una interfaz para la eliminación por lotes. En concreto, cada fila de GridView incluirá una casilla y, en lugar de los botones actualizar todos los tipos, tendremos los botones eliminar filas seleccionadas.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Teresa Murphy y David Suru. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](wrapping-database-modifications-within-a-transaction-vb.md)
> [Siguiente](batch-deleting-vb.md)
