---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
title: Insertar, actualizar y eliminar datos con SqlDataSource (VB) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores se ha aprendido cómo se permitía el control ObjectDataSource para insertar, actualizar y eliminar datos. El control SqlDataSource admite t...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 9673bef3-892c-45ba-a7d8-0da3d6f48ec5
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 4f7b282b09769272df8ff3a32aa4c509c8917481
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78508339"
---
# <a name="inserting-updating-and-deleting-data-with-the-sqldatasource-vb"></a>Insertar, actualizar y eliminar datos con SqlDataSource (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_VB.exe) de la aplicación de ejemplo o [descarga de PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/datatutorial49vb1.pdf)

> En los tutoriales anteriores se ha aprendido cómo se permitía el control ObjectDataSource para insertar, actualizar y eliminar datos. El control SqlDataSource admite las mismas operaciones, pero el enfoque es diferente, y en este tutorial se muestra cómo configurar SqlDataSource para insertar, actualizar y eliminar datos.

## <a name="introduction"></a>Introducción

Como se describe en [información general sobre la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), el control GridView proporciona funciones integradas de actualización y eliminación, mientras que los controles DetailsView y FormView incluyen compatibilidad de inserción junto con la funcionalidad de edición y eliminación. Estas funciones de modificación de datos se pueden conectar directamente a un control de origen de datos sin necesidad de escribir una línea de código. [Información general sobre la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) examinada mediante el origen de los mismos para facilitar la inserción, actualización y eliminación con los controles GridView, DetailsView y FormView. Como alternativa, se puede usar SqlDataSource en lugar de ObjectDataSource.

Recuerde que para admitir las operaciones de inserción, actualización y eliminación, con el ObjectDataSource que necesitábamos especificar los métodos de nivel de objeto que se van a invocar para realizar la acción de inserción, actualización o eliminación. Con SqlDataSource, es necesario proporcionar `INSERT`, `UPDATE`y `DELETE` instrucciones SQL (o procedimientos almacenados) que se van a ejecutar. Como veremos en este tutorial, estas instrucciones se pueden crear manualmente o se pueden generar automáticamente mediante el Asistente para configurar el origen de datos de SqlDataSource.

> [!NOTE]
> Como ya hemos analizado las funcionalidades de inserción, edición y eliminación de los controles GridView, DetailsView y FormView, este tutorial se centrará en la configuración del control SqlDataSource para admitir estas operaciones. Si necesita un pincel para implementar estas características en GridView, DetailsView y FormView, vuelva a los tutoriales de edición, inserción y eliminación de datos, empezando por [una introducción a la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md).

## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>Paso 1: especificar instrucciones`INSERT`,`UPDATE`y`DELETE`

Como hemos aprendido en los dos últimos tutoriales, para recuperar datos de un control SqlDataSource es necesario establecer dos propiedades:

1. `ConnectionString`, que especifica a qué base de datos se va a enviar la consulta, y
2. `SelectCommand`, que especifica la instrucción SQL ad hoc o el nombre del procedimiento almacenado que se va a ejecutar para devolver los resultados.

Para `SelectCommand` valores con parámetros, los valores de los parámetros se especifican a través de la colección SqlDataSource s `SelectParameters` y pueden incluir valores codificados de forma rígida, valores de origen de parámetros comunes (campos QueryString, variables de sesión, valores de control Web, etc.) o se pueden asignar mediante programación. Cuando el método SqlDataSource control s `Select()` se invoca mediante programación o automáticamente desde un control Web de datos se establece una conexión con la base de datos, los valores de los parámetros se asignan a la consulta y el comando se desplaza a la base de datos. Los resultados se devuelven como un conjunto de elementos o un objeto DataReader, dependiendo del valor de la propiedad control s `DataSourceMode`.

Junto con la selección de datos, el control SqlDataSource se puede usar para insertar, actualizar y eliminar datos proporcionando `INSERT`, `UPDATE`y `DELETE` instrucciones SQL de forma muy parecida. Simplemente asigne las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` las instrucciones SQL `INSERT`, `UPDATE`y `DELETE` que se van a ejecutar. Si las instrucciones tienen parámetros (como lo hacen siempre), se incluyen en las colecciones `InsertParameters`, `UpdateParameters`y `DeleteParameters`.

Una vez que se ha especificado un valor de `InsertCommand`, `UpdateCommand`o `DeleteCommand`, la opción Habilitar inserción, habilitar edición o habilitar eliminación en la etiqueta inteligente correspondiente del control Web de datos estará disponible. Para ilustrar esto, echemos un ejemplo de la página `Querying.aspx` que creamos en el tutorial de [consulta de datos con el control SqlDataSource](querying-data-with-the-sqldatasource-control-vb.md) y lo aumentamos para incluir capacidades de eliminación.

Para empezar, abra las páginas `InsertUpdateDelete.aspx` y `Querying.aspx` de la carpeta `SqlDataSource`. En el diseñador de la página `Querying.aspx`, seleccione SqlDataSource y GridView en el primer ejemplo (los controles `ProductsDataSource` y `GridView1`). Después de seleccionar los dos controles, vaya al menú edición y elija Copiar (o simplemente presione Ctrl + C). A continuación, vaya al diseñador de `InsertUpdateDelete.aspx` y péguelo en los controles. Una vez que haya colocado los dos controles sobre `InsertUpdateDelete.aspx`, pruebe la página en un explorador. Debería ver los valores de las columnas `ProductID`, `ProductName`y `UnitPrice` de todos los registros de la tabla de base de datos `Products`.

[![todos los productos se muestran, ordenados por ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image1.png)

**Figura 1**: todos los productos se enumeran, ordenados por `ProductID` ([haga clic para ver la imagen de tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.png))

## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>Agregar las propiedades de`DeleteCommand`y`DeleteParameters`de SqlDataSource

En este momento, tenemos un SqlDataSource que simplemente devuelve todos los registros de la tabla `Products` y un control GridView que representa estos datos. Nuestro objetivo es ampliar este ejemplo para permitir que el usuario elimine productos a través de GridView. Para ello, es necesario especificar los valores de las propiedades de control SqlDataSource `DeleteCommand` y `DeleteParameters` y, a continuación, configurar GridView para admitir la eliminación.

Las propiedades `DeleteCommand` y `DeleteParameters` se pueden especificar de varias maneras:

- Mediante la sintaxis declarativa
- Del ventana Propiedades en el diseñador
- En la pantalla especificar una instrucción SQL personalizada o un procedimiento almacenado en el Asistente para configurar orígenes de datos
- Mediante el botón Opciones avanzadas de la pantalla especificar columnas de una tabla de vista del Asistente para configurar orígenes de datos, que generará automáticamente la `DELETE` instrucción SQL y la colección de parámetros que se usan en las propiedades `DeleteCommand` y `DeleteParameters`

Examinaremos cómo se crea automáticamente la instrucción `DELETE` creada en el paso 2. Por ahora, vamos a usar el ventana Propiedades en el diseñador, aunque la opción del Asistente para configurar orígenes de datos o la sintaxis declarativa también funcionaría.

En el diseñador de `InsertUpdateDelete.aspx`, haga clic en el `ProductsDataSource` SqlDataSource y, a continuación, abra el ventana Propiedades (en el menú Ver, elija ventana Propiedades o simplemente presione F4). Seleccione la propiedad DeleteQuery, que mostrará un conjunto de puntos suspensivos.

![Seleccione la propiedad DeleteQuery en la ventana Propiedades.](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image2.gif)

**Figura 2**: seleccione la propiedad DeleteQuery en la ventana Propiedades.

> [!NOTE]
> SqlDataSource no tiene una propiedad DeleteQuery. En su lugar, DeleteQuery es una combinación de las propiedades `DeleteCommand` y `DeleteParameters` y solo se muestra en el ventana Propiedades al ver la ventana a través del diseñador. Si está viendo el ventana Propiedades en la vista de código fuente, encontrará la propiedad `DeleteCommand` en su lugar.

Haga clic en los puntos suspensivos de la propiedad DeleteQuery para abrir el cuadro de diálogo Editor de parámetros y comandos (vea la figura 3). En este cuadro de diálogo puede especificar el `DELETE` instrucción SQL y especificar los parámetros. Escriba la siguiente consulta en el cuadro de texto `DELETE` comando (manualmente o mediante el Generador de consultas, si lo prefiere):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample1.sql)]

A continuación, haga clic en el botón actualizar parámetros para agregar el parámetro `@ProductID` a la lista de parámetros que aparece a continuación.

[![seleccione la propiedad DeleteQuery en la ventana Propiedades.](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image3.png)

**Figura 3**: seleccione la propiedad DeleteQuery en la ventana Propiedades ([haga clic para ver la imagen a tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.png))

*No* proporcione un valor para este parámetro (deje el origen del parámetro en ninguno). Una vez que se agrega compatibilidad con la eliminación a GridView, el control GridView proporcionará automáticamente este valor de parámetro usando el valor de su colección `DataKeys` para la fila en la que se hizo clic en el botón Eliminar.

> [!NOTE]
> El nombre del parámetro que se usa en la consulta `DELETE` *debe* ser el mismo que el nombre del valor `DataKeyNames` de GridView, DetailsView o FormView. Es decir, el parámetro de la instrucción `DELETE` se denomina de forma intencionada `@ProductID` (en lugar de, por ejemplo, `@ID`), porque el nombre de la columna de clave principal de la tabla Products (y, por tanto, el valor de DataKeyNames en GridView) es `ProductID`.

Si el nombre del parámetro y el valor de `DataKeyNames` no coinciden, el control GridView no puede asignar automáticamente el parámetro al valor de la colección `DataKeys`.

Después de escribir la información relacionada con la eliminación en el cuadro de diálogo Editor de parámetros y comandos, haga clic en aceptar y vaya a la vista Código fuente para examinar el marcado declarativo resultante:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample2.aspx)]

Observe la adición de la propiedad `DeleteCommand`, así como la sección `<DeleteParameters>` y el objeto Parameter denominado `productID`.

## <a name="configuring-the-gridview-for-deleting"></a>Configurar GridView para eliminar

Con la propiedad `DeleteCommand` agregada, la etiqueta inteligente GridView s ahora contiene la opción Habilitar eliminación. Continúe y Active esta casilla. Como se describe en [información general sobre la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), esto hace que GridView agregue un CommandField con su `ShowDeleteButton` propiedad establecida en `True`. Como se muestra en la figura 4, cuando se visita la página a través de un explorador, se incluye un botón Eliminar. Para probar esta página, elimine algunos productos.

[![cada fila de GridView incluye ahora un botón eliminar](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.png)

**Figura 4**: cada fila de GridView incluye ahora un botón Eliminar ([haga clic para ver la imagen de tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.png))

Al hacer clic en el botón eliminar, se produce un postback, el control GridView asigna al parámetro `ProductID` el valor del valor de la colección `DataKeys` para la fila en la que se hizo clic en el botón Eliminar e invoca el método SqlDataSource s `Delete()`. A continuación, el control SqlDataSource se conecta a la base de datos y ejecuta la instrucción `DELETE`. A continuación, GridView se vuelve a enlazar a SqlDataSource, devolviendo y mostrando el conjunto actual de productos (que ya no incluye el registro recién eliminado).

> [!NOTE]
> Dado que GridView usa su colección de `DataKeys` para rellenar los parámetros de SqlDataSource, es fundamental que la propiedad `DataKeyNames` de GridView se establezca en las columnas que constituyen la clave principal y que el `SelectCommand` SqlDataSource s devuelva estas columnas. Además, es importante que el nombre del parámetro en el `DeleteCommand` SqlDataSource s se establezca en `@ProductID`. Si no se establece la propiedad `DataKeyNames` o si el parámetro no se denomina `@ProductsID`, al hacer clic en el botón eliminar se producirá un postback, pero realmente no se eliminará ningún registro.

La figura 5 muestra esta interacción de forma gráfica. Consulte el tutorial [examinar los eventos asociados con la inserción, actualización y eliminación](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-vb.md) para obtener una explicación más detallada sobre la cadena de eventos asociada a la inserción, actualización y eliminación de un control Web de datos.

![Al hacer clic en el botón eliminar de GridView, se invoca el método SqlDataSource s Delete ()](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image5.gif)

**Figura 5**: hacer clic en el botón eliminar de GridView invoca el método SqlDataSource s `Delete()`

## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>Paso 2: generar automáticamente las instrucciones`INSERT`,`UPDATE`y`DELETE`

En el paso 1 examinado, se pueden especificar `INSERT`, `UPDATE`y `DELETE` instrucciones SQL a través de la sintaxis declarativa de control s o ventana Propiedades. Sin embargo, este enfoque requiere que se escriban manualmente las instrucciones SQL a mano, lo que puede ser más monótono y propenso a errores. Afortunadamente, el Asistente para configurar orígenes de datos proporciona una opción para que las instrucciones `INSERT`, `UPDATE`y `DELETE` se generen automáticamente al usar la pantalla especificar columnas de una tabla de vista.

Permita explorar esta opción de generación automática. Agregue DetailsView al diseñador en `InsertUpdateDelete.aspx` y establezca su propiedad `ID` en `ManageProducts`. A continuación, desde la etiqueta inteligente de DetailsView s, elija crear un nuevo origen de datos y crear un SqlDataSource denominado `ManageProductsDataSource`.

[![crear un nuevo SqlDataSource denominado ManageProductsDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.png)

**Figura 6**: cree un nuevo SqlDataSource denominado `ManageProductsDataSource` ([haga clic para ver la imagen de tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.png))

En el Asistente para configurar orígenes de datos, elija usar la cadena de conexión `NORTHWINDConnectionString` y haga clic en siguiente. En la pantalla configurar la instrucción SELECT, deje seleccionado el botón de radio especificar columnas de una tabla o vista y elija la tabla `Products` de la lista desplegable. Seleccione las columnas `ProductID`, `ProductName`, `UnitPrice`y `Discontinued` de la lista de casillas.

[![mediante la tabla Products, devolver las columnas ProductID, ProductName, UnitPrice y Discontinued](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.png)

**Figura 7**: uso de la tabla `Products`, devolver las columnas `ProductID`, `ProductName`, `UnitPrice`y `Discontinued` ([haga clic para ver la imagen de tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image10.png))

Para generar automáticamente `INSERT`, `UPDATE`y `DELETE` instrucciones basadas en la tabla y las columnas seleccionadas, haga clic en el botón avanzadas y active la casilla generar las instrucciones de `INSERT`, `UPDATE`y `DELETE`.

![Active la casilla generar instrucciones INSERT, UPDATE y DELETE](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image8.gif)

**Figura 8**: Active las casillas generar `INSERT`, `UPDATE`y `DELETE`

La casilla generar `INSERT`, `UPDATE`y `DELETE` solo se puede comprobar si la tabla seleccionada tiene una clave principal y la columna (o columnas) de clave principal se incluye en la lista de columnas devueltas. La casilla usar simultaneidad optimista, que se convierte en seleccionable una vez que se ha comprobado la casilla generar `INSERT`, `UPDATE`y instrucciones `DELETE`, aumentará las cláusulas `WHERE` en las instrucciones `UPDATE` y `DELETE` resultantes para proporcionar un control de simultaneidad optimista. Por ahora, deje la casilla desactivada. examinaremos la simultaneidad optimista con el control SqlDataSource en el siguiente tutorial.

Después de activar la casilla generar `INSERT`, `UPDATE`y `DELETE`, haga clic en Aceptar para volver a la pantalla configurar instrucción SELECT y, a continuación, haga clic en siguiente y en finalizar para completar el Asistente para configurar orígenes de datos. Al finalizar el asistente, Visual Studio agregará BoundFields a DetailsView para las columnas `ProductID`, `ProductName`y `UnitPrice` y CheckBoxField para la columna `Discontinued`. En la etiqueta inteligente de DetailsView s, active la opción Habilitar paginación para que el usuario que visita esta página pueda recorrer los productos. También puede desactivar las propiedades `Width` y `Height` de DetailsView.

Tenga en cuenta que la etiqueta inteligente tiene disponibles las opciones habilitar inserción, habilitar edición y habilitar eliminación. Esto se debe a que SqlDataSource contiene valores para su `InsertCommand`, `UpdateCommand`y `DeleteCommand`, como se muestra en la sintaxis declarativa siguiente:

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/samples/sample3.aspx)]

Observe cómo el control SqlDataSource ha tenido valores establecidos automáticamente para sus propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand`. El conjunto de columnas al que se hace referencia en las propiedades `InsertCommand` y `UpdateCommand` se basa en los de la instrucción `SELECT`. Es decir, en lugar de tener *cada* columna Products en el `InsertCommand` y `UpdateCommand`, solo hay las columnas especificadas en el `SelectCommand` (menos `ProductID`, lo que se omite porque es una [columna`IDENTITY`](http://www.sqlteam.com/item.asp?ItemID=102), cuyo valor no se puede cambiar cuando se edita y que se asigna automáticamente al insertar). Además, para cada parámetro de las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` hay parámetros correspondientes en las colecciones `InsertParameters`, `UpdateParameters`y `DeleteParameters`.

Para activar las características de modificación de datos de DetailsView s, active las opciones habilitar inserción, habilitar edición y habilitar eliminación en su etiqueta inteligente. Esto agrega un CommandField con sus propiedades `ShowInsertButton`, `ShowEditButton`y `ShowDeleteButton` establecidas en `True`.

Visite la página en un explorador y anote los botones de edición, eliminación y nuevo incluidos en DetailsView. Al hacer clic en el botón Editar, DetailsView se convierte en el modo de edición, que muestra cada BoundField cuya propiedad `ReadOnly` está establecida en `False` (valor predeterminado) como un cuadro de texto y CheckBoxField como casilla.

[![la interfaz de edición predeterminada de DetailsView s](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image11.png)

**Figura 9**: interfaz de edición predeterminada de DetailsView s ([haga clic para ver la imagen de tamaño completo](inserting-updating-and-deleting-data-with-the-sqldatasource-vb/_static/image12.png))

Del mismo modo, puede eliminar el producto seleccionado actualmente o agregar un nuevo producto al sistema. Dado que la instrucción `InsertCommand` solo funciona con las columnas `ProductName`, `UnitPrice`y `Discontinued`, las demás columnas tienen `NULL` o su valor predeterminado asignado por la base de datos al insertar. Al igual que con ObjectDataSource, si en la `InsertCommand` falta alguna columna de tabla de base de datos que no permita `NULL` s y Don t tenga un valor predeterminado, se producirá un error de SQL al intentar ejecutar la instrucción `INSERT`.

> [!NOTE]
> Las interfaces de inserción y edición de DetailsView no tienen ningún tipo de personalización o validación. Para agregar controles de validación o personalizar las interfaces, debe convertir BoundFields en TemplateFields. Consulte [Agregar controles de validación a las interfaces de edición e inserción](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) y personalizar los tutoriales de [la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) para obtener más información.

Además, tenga en cuenta que para actualizar y eliminar, DetailsView usa el valor actual de `DataKey` del producto, que solo está presente si se ha configurado la propiedad `DataKeyNames`. Si la edición o eliminación parece no tener ningún efecto, asegúrese de que la propiedad `DataKeyNames` esté establecida.

## <a name="limitations-of-automatically-generating-sql-statements"></a>Limitaciones de la generación automática de instrucciones SQL

Dado que la opción generar `INSERT`, `UPDATE`y `DELETE` solo está disponible cuando se seleccionan columnas de una tabla, para consultas más complejas tendrá que escribir sus propias instrucciones `INSERT`, `UPDATE`y `DELETE` como hicimos en el paso 1. Normalmente, las instrucciones de SQL `SELECT` usan `JOIN` s para devolver datos de una o varias tablas de búsqueda con fines de visualización (como devolver el campo `Categories` tabla s `CategoryName` al mostrar la información del producto). Al mismo tiempo, es posible que desee permitir que el usuario edite, actualice o inserte datos en la tabla principal (`Products`, en este caso).

Mientras que las instrucciones `INSERT`, `UPDATE`y `DELETE` se pueden escribir manualmente, tenga en cuenta la siguiente sugerencia de ahorro de tiempo. Configure inicialmente SqlDataSource para que extraiga los datos solo de la tabla `Products`. Use el Asistente para configurar orígenes de datos para especificar las columnas de una pantalla de tabla o vista, de modo que pueda generar automáticamente las instrucciones `INSERT`, `UPDATE`y `DELETE`. Después, después de completar el asistente, elija configurar el SelectQuery desde el ventana Propiedades (o bien, vuelva al Asistente para configurar el origen de datos, pero use la opción especificar una instrucción SQL personalizada o un procedimiento almacenado). A continuación, actualice la instrucción `SELECT` para incluir la sintaxis de `JOIN`. Esta técnica ofrece las ventajas de ahorro de tiempo de las instrucciones SQL generadas automáticamente y permite una instrucción `SELECT` más personalizada.

Otra limitación de la generación automática de las instrucciones `INSERT`, `UPDATE`y `DELETE` es que las columnas de las instrucciones `INSERT` y `UPDATE` se basan en las columnas devueltas por la instrucción `SELECT`. Sin embargo, es posible que tenga que actualizar o insertar más o menos campos. Por ejemplo, en el ejemplo del paso 2, es posible que deseemos que el `UnitPrice` BoundField sea de solo lectura. En ese caso, no debe aparecer en el `UpdateCommand`. O bien, es posible que desee establecer el valor de un campo de tabla que no aparezca en GridView. Por ejemplo, al agregar un nuevo registro, es posible que le interese el valor de `QuantityPerUnit` establecido en TODO.

Si se requieren estas personalizaciones, deberá realizarlas manualmente, bien mediante el ventana Propiedades, la opción especificar una instrucción SQL personalizada o un procedimiento almacenado en el asistente o a través de la sintaxis declarativa.

> [!NOTE]
> Al agregar parámetros que no tienen campos correspondientes en el control Web de datos, tenga en cuenta que es necesario asignar valores a estos parámetros de alguna manera. Estos valores pueden ser: codificado de forma rígida directamente en el `InsertCommand` o `UpdateCommand`; puede proviene de algún origen predefinido (la cadena QueryString, el estado de la sesión, los controles Web de la página, etc.); o se puede asignar mediante programación, como vimos en el tutorial anterior.

## <a name="summary"></a>Resumen

Para que los controles Web de datos utilicen sus capacidades integradas de inserción, edición y eliminación, el control de origen de datos con el que están enlazados debe ofrecer dicha funcionalidad. En el caso de SqlDataSource, esto significa que `INSERT`, `UPDATE`y `DELETE` instrucciones SQL deben asignarse a las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand`. Estas propiedades y las colecciones de parámetros correspondientes se pueden agregar manualmente o generar automáticamente a través del Asistente para configurar orígenes de datos. En este tutorial, hemos examinado ambas técnicas.

Hemos examinado el uso de la simultaneidad optimista con ObjectDataSource en el tutorial de [implementación de simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) . El control SqlDataSource también proporciona compatibilidad con simultaneidad optimista. Como se indicó en el paso 2, al generar automáticamente las instrucciones `INSERT`, `UPDATE`y `DELETE`, el asistente ofrece una opción usar simultaneidad optimista. Como veremos en el siguiente tutorial, el uso de la simultaneidad optimista con SqlDataSource modifica las cláusulas `WHERE` en las instrucciones `UPDATE` y `DELETE` para asegurarse de que los valores de las demás columnas no hayan cambiado desde la última vez que se mostraban los datos en la página.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](using-parameterized-queries-with-the-sqldatasource-vb.md)
> [Siguiente](implementing-optimistic-concurrency-with-the-sqldatasource-vb.md)
