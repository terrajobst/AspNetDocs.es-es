---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
title: Agregar columnas adicionales de DataTableC#() | Microsoft Docs
author: rick-anderson
description: Al utilizar el Asistente para TableAdapter para crear un conjunto de datos con tipo, el DataTable correspondiente contiene las columnas devueltas por la consulta de base de datos principal. Pero...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 615f3361-f21f-4338-8bc1-fce8ae071de9
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/adding-additional-datatable-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: a96f254aa54e7077456ac1a9bd6c5e2a17619d96
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78444499"
---
# <a name="adding-additional-datatable-columns-c"></a>Agregar columnas adicionales de DataTable (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_70_CS.zip) o [Descargar PDF](adding-additional-datatable-columns-cs/_static/datatutorial70cs1.pdf)

> Al utilizar el Asistente para TableAdapter para crear un conjunto de datos con tipo, el DataTable correspondiente contiene las columnas devueltas por la consulta de base de datos principal. Pero hay ocasiones en las que la DataTable necesita incluir columnas adicionales. En este tutorial se explica por qué se recomiendan los procedimientos almacenados cuando se necesitan columnas DataTable adicionales.

## <a name="introduction"></a>Introducción

Al agregar un TableAdapter a un conjunto de tipos, el esquema DataTable s correspondiente viene determinado por la consulta principal de TableAdapter s. Por ejemplo, si la consulta principal devuelve los campos *de datos a*, *b*y *c*, la DataTable tendrá tres columnas correspondientes denominadas *a*, *b*y *c*. Además de su consulta principal, un TableAdapter puede incluir consultas adicionales que devuelven, quizás, un subconjunto de los datos basándose en algún parámetro. Por ejemplo, además de la consulta principal `ProductsTableAdapter` s, que devuelve información sobre todos los productos, también contiene métodos como `GetProductsByCategoryID(categoryID)` y `GetProductByProductID(productID)`, que devuelven información específica del producto en función de un parámetro proporcionado.

El modelo de tener el esquema DataTable s refleja la consulta principal de TableAdapter s funciona bien si todos los métodos TableAdapter s devuelven los mismos campos de datos o menos que los especificados en la consulta principal. Si un método TableAdapter necesita devolver campos de datos adicionales, deberíamos expandir el esquema DataTable s en consecuencia. En la [página maestra/detalle mediante una lista con viñetas de registros maestros con un tutorial de DataList de detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) , agregamos un método a la `CategoriesTableAdapter` que devolvía los campos de datos `CategoryID`, `CategoryName`y `Description` definidos en la consulta principal más `NumberOfProducts`, un campo de datos adicional que indica el número de productos asociados a cada categoría. Agregamos manualmente una nueva columna a la `CategoriesDataTable` para capturar el valor del campo de datos `NumberOfProducts` de este nuevo método.

Como se describe en el tutorial de [carga de archivos](../working-with-binary-files/uploading-files-cs.md) , se debe tener especial cuidado con los TableAdapters que usan instrucciones SQL ad hoc y tienen métodos cuyos campos de datos no coinciden exactamente con la consulta principal. Si se vuelve a ejecutar el Asistente para la configuración de TableAdapter, se actualizarán todos los métodos de TableAdapter para que su lista de campos de datos coincida con la consulta principal. Por consiguiente, cualquier método con listas de columnas personalizadas revertirá a la lista de columnas de la consulta principal y no devolverá los datos esperados. Este problema no se produce cuando se usan procedimientos almacenados.

En este tutorial, veremos cómo extender un esquema DataTable s para incluir columnas adicionales. Debido a la fragilidad del TableAdapter cuando se usan instrucciones SQL ad hoc, en este tutorial usaremos procedimientos almacenados. Consulte [crear nuevos procedimientos almacenados para los TableAdapters del conjunto de datos con tipo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) y [usar procedimientos almacenados existentes para los tutoriales de TableAdapters del conjunto](https://data-access/tutorials/using-existing-stored-procedures-for-the-typed-dataset-amp-rsquo-s-tableadapters-cs) de datos con tipo para obtener más información sobre la configuración de un TableAdapter para usar procedimientos almacenados.

## <a name="step-1-adding-apricequartilecolumn-to-theproductsdatatable"></a>Paso 1: agregar una columna de`PriceQuartile`al`ProductsDataTable`

En el tutorial *crear nuevos procedimientos almacenados para el conjunto de objetos de datasets con tipo* , hemos creado un conjunto de un conjunto de tipos denominado `NorthwindWithSprocs`. Este conjunto de contenido contiene actualmente dos DataTables: `ProductsDataTable` y `EmployeesDataTable`. El `ProductsTableAdapter` tiene los tres métodos siguientes:

- `GetProducts`: la consulta principal, que devuelve todos los registros de la tabla `Products`
- `GetProductsByCategoryID(categoryID)`: devuelve todos los productos con el *IdCategoría*especificado.
- `GetProductByProductID(productID)`: devuelve el producto concreto con el *ProductID*especificado.

La consulta principal y los dos métodos adicionales devuelven el mismo conjunto de campos de datos, es decir, todas las columnas de la tabla `Products`. No hay subconsultas correlacionadas ni `JOIN` extraer datos relacionados de las tablas `Categories` o `Suppliers`. Por lo tanto, el `ProductsDataTable` tiene una columna correspondiente para cada campo de la tabla de `Products`.

En este tutorial, vamos a agregar un método a la `ProductsTableAdapter` denominada `GetProductsWithPriceQuartile` que devuelve todos los productos. Además de los campos de datos de producto estándar, `GetProductsWithPriceQuartile` también incluirá un `PriceQuartile` campo de datos que indica en qué cuartil está el precio del producto. Por ejemplo, aquellos productos cuyos precios estén en el 25% más caro tendrán un valor de `PriceQuartile` de 1, mientras que los que se encuentran en el 25% inferior tendrán un valor de 4. Sin embargo, antes de preocuparnos por la creación del procedimiento almacenado para devolver esta información, primero es necesario actualizar el `ProductsDataTable` para incluir una columna que contenga los resultados del `PriceQuartile` cuando se use el método `GetProductsWithPriceQuartile`.

Abra el conjunto de `NorthwindWithSprocs` y haga clic con el botón derecho en el `ProductsDataTable`. Elija Agregar en el menú contextual y, a continuación, seleccione columna.

[![agregar una nueva columna a ProductsDataTable](adding-additional-datatable-columns-cs/_static/image2.png)](adding-additional-datatable-columns-cs/_static/image1.png)

**Figura 1**: agregar una nueva columna a la `ProductsDataTable` ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image3.png))

Esto agregará una nueva columna a la DataTable denominada Column1 de tipo `System.String`. Necesitamos actualizar el nombre de esta columna a PriceQuartile y su tipo a `System.Int32`, ya que se usará para contener un número entre 1 y 4. Seleccione la columna recién agregada en el `ProductsDataTable` y, en el ventana Propiedades, establezca la propiedad `Name` en PriceQuartile y la propiedad `DataType` en `System.Int32`.

[![establecer las propiedades de nombre y tipo de columna de las nuevas columnas](adding-additional-datatable-columns-cs/_static/image5.png)](adding-additional-datatable-columns-cs/_static/image4.png)

**Figura 2**: establecer las propiedades de la nueva columna `Name` y `DataType` ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image6.png))

Como se muestra en la figura 2, hay propiedades adicionales que se pueden establecer, por ejemplo, si los valores de la columna deben ser únicos, si la columna es una columna de incremento automático, si se permiten o no los valores de `NULL` de la base de datos, etc. Deje estos valores establecidos en sus valores predeterminados.

## <a name="step-2-creating-thegetproductswithpricequartilemethod"></a>Paso 2: crear el método`GetProductsWithPriceQuartile`

Ahora que el `ProductsDataTable` se ha actualizado para incluir la columna `PriceQuartile`, estamos preparados para crear el método `GetProductsWithPriceQuartile`. Para empezar, haga clic con el botón derecho en el TableAdapter y elija Agregar consulta en el menú contextual. Se abre el Asistente para configuración de consultas de TableAdapter, que primero nos pregunta si deseamos usar instrucciones SQL ad hoc o un procedimiento almacenado nuevo o existente. Dado que todavía no tenemos un procedimiento almacenado que devuelva los datos del cuartil de precios, permita que el TableAdapter cree este procedimiento almacenado para nosotros. Seleccione la opción Crear nuevo procedimiento almacenado y haga clic en siguiente.

[![indicar al asistente de TableAdapter que cree el procedimiento almacenado para nosotros](adding-additional-datatable-columns-cs/_static/image8.png)](adding-additional-datatable-columns-cs/_static/image7.png)

**Figura 3**: indicar al asistente de TableAdapter que cree el procedimiento almacenado para nosotros ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image9.png))

En la pantalla siguiente, que se muestra en la figura 4, el asistente nos pregunta qué tipo de consulta agregar. Dado que el método `GetProductsWithPriceQuartile` devolverá todas las columnas y los registros de la tabla `Products`, seleccione la opción seleccionar qué devuelve filas y haga clic en siguiente.

[![nuestra consulta será una instrucción SELECT que devuelva varias filas](adding-additional-datatable-columns-cs/_static/image11.png)](adding-additional-datatable-columns-cs/_static/image10.png)

**Figura 4**: nuestra consulta será una instrucción `SELECT` que devuelve varias filas ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image12.png))

A continuación, se le pedirá el `SELECT` consulta. En el asistente, escriba la siguiente consulta:

[!code-sql[Main](adding-additional-datatable-columns-cs/samples/sample1.sql)]

La consulta anterior utiliza SQL Server [función new`NTILE`](https://msdn.microsoft.com/library/ms175126.aspx) de 2005 s para dividir los resultados en cuatro grupos en los que los grupos vienen determinados por los valores de `UnitPrice` ordenados en orden descendente.

Desafortunadamente, el Generador de consultas no sabe cómo analizar la palabra clave `OVER` y mostrará un error al analizar la consulta anterior. Por lo tanto, escriba la consulta anterior directamente en el cuadro de texto del asistente sin usar el Generador de consultas.

> [!NOTE]
> Para obtener más información sobre NTILE y SQL Server 2005 s otras funciones de categoría, vea [devolver resultados clasificados con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml) y la [sección funciones de categoría](https://msdn.microsoft.com/library/ms189798.aspx) en los [libros en pantalla de SQL Server 2005](https://msdn.microsoft.com/library/ms189798.aspx).

Después de escribir la consulta de `SELECT` y hacer clic en siguiente, el asistente nos pide que proporcione un nombre para el procedimiento almacenado que creará. Asigne un nombre al nuevo procedimiento almacenado `Products_SelectWithPriceQuartile` y haga clic en siguiente.

[![el nombre del procedimiento almacenado Products_SelectWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image14.png)](adding-additional-datatable-columns-cs/_static/image13.png)

**Figura 5**: asignar un nombre al procedimiento almacenado `Products_SelectWithPriceQuartile` ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image15.png))

Por último, se le pedirá que asigne un nombre a los métodos de TableAdapter. Deje las casillas rellenar un DataTable y devolver un objeto DataTable activada y asigne a los métodos el nombre `FillWithPriceQuartile` y `GetProductsWithPriceQuartile`.

[![nombre de los métodos de TableAdapter y haga clic en finalizar](adding-additional-datatable-columns-cs/_static/image17.png)](adding-additional-datatable-columns-cs/_static/image16.png)

**Figura 6**: asigne un nombre a los métodos de TableAdapter y haga clic en finalizar ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image18.png))

Con la consulta `SELECT` especificada y el procedimiento almacenado y los métodos TableAdapter denominados, haga clic en finalizar para completar el asistente. En este momento, puede recibir una advertencia o dos del asistente que indica que no se admite la construcción o instrucción de SQL `OVER`. Estas advertencias se pueden omitir.

Después de completar el asistente, el TableAdapter debe incluir los métodos `FillWithPriceQuartile` y `GetProductsWithPriceQuartile` y la base de datos debe incluir un procedimiento almacenado denominado `Products_SelectWithPriceQuartile`. Dedique un momento a comprobar que el TableAdapter realmente contiene este nuevo método y que el procedimiento almacenado se ha agregado correctamente a la base de datos. Al comprobar la base de datos, si no ve el procedimiento almacenado, haga clic con el botón secundario en la carpeta procedimientos almacenados y elija actualizar.

![Comprobar que se ha agregado un nuevo método a TableAdapter](adding-additional-datatable-columns-cs/_static/image19.png)

**Figura 7**: comprobación de que se ha agregado un nuevo método a TableAdapter

[![asegurarse de que la base de datos contiene el Products_SelectWithPriceQuartile procedimiento almacenado](adding-additional-datatable-columns-cs/_static/image21.png)](adding-additional-datatable-columns-cs/_static/image20.png)

**Figura 8**: Asegúrese de que la base de datos contiene el `Products_SelectWithPriceQuartile` procedimiento almacenado ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image22.png))

> [!NOTE]
> Una de las ventajas de utilizar procedimientos almacenados en lugar de instrucciones SQL ad hoc es que volver a ejecutar el Asistente para la configuración de TableAdapter no modificará las listas de columnas de procedimientos almacenados. Para comprobarlo, haga clic con el botón secundario en el TableAdapter, elija la opción configurar en el menú contextual para iniciar el asistente y, a continuación, haga clic en finalizar para completarlo. A continuación, vaya a la base de datos y vea el `Products_SelectWithPriceQuartile` procedimiento almacenado. Tenga en cuenta que la lista de columnas no se ha modificado. Si hubiéramos usado instrucciones SQL ad-hoc, volver a ejecutar el Asistente para configuración de TableAdapter habría revertido esta lista de columnas de consulta para que coincida con la lista de columnas de consulta principal, con lo que se quitará la instrucción NTILE de la consulta utilizada por el método `GetProductsWithPriceQuartile`.

Cuando se invoca el método Data Access Layer s `GetProductsWithPriceQuartile`, el TableAdapter ejecuta el `Products_SelectWithPriceQuartile` procedimiento almacenado y agrega una fila a la `ProductsDataTable` para cada registro devuelto. Los campos de datos devueltos por el procedimiento almacenado se asignan a las columnas `ProductsDataTable` s. Dado que hay un `PriceQuartile` campo de datos devuelto por el procedimiento almacenado, su valor se asigna a la columna `ProductsDataTable` s `PriceQuartile`.

En el caso de los métodos de TableAdapter cuyas consultas no devuelven un `PriceQuartile` campo de datos, el valor de la `PriceQuartile` columna s es el valor especificado por su propiedad `DefaultValue`. Como se muestra en la figura 2, este valor se establece en `DBNull`, el valor predeterminado. Si prefiere un valor predeterminado diferente, basta con establecer la propiedad `DefaultValue` en consecuencia. Solo tiene que asegurarse de que el valor de `DefaultValue` sea válido dado que la columna s `DataType` (es decir, `System.Int32` para la columna de `PriceQuartile`).

En este punto, hemos realizado los pasos necesarios para agregar una columna adicional a una DataTable. Para comprobar que esta columna adicional funciona según lo previsto, cree una página ASP.NET que muestre cada nombre de producto, precio y cuartil de precios. Sin embargo, antes de hacerlo, primero es necesario actualizar la capa de lógica de negocios para incluir un método que llama al método de `GetProductsWithPriceQuartile` de la capa DAL. Actualizaremos la capa BLL siguiente, en el paso 3 y, a continuación, crearemos la página ASP.NET en el paso 4.

## <a name="step-3-augmenting-the-business-logic-layer"></a>Paso 3: aumentar el nivel de lógica de negocios

Antes de usar el método New `GetProductsWithPriceQuartile` del nivel de presentación, primero debemos agregar el método correspondiente a la capa BLL. Abra el archivo de clase `ProductsBLLWithSprocs` y agregue el código siguiente:

[!code-csharp[Main](adding-additional-datatable-columns-cs/samples/sample2.cs)]

Al igual que los demás métodos de recuperación de datos en `ProductsBLLWithSprocs`, el método `GetProductsWithPriceQuartile` simplemente llama al método `GetProductsWithPriceQuartile` de la capa DAL correspondiente y devuelve sus resultados.

## <a name="step-4-displaying-the-price-quartile-information-in-an-aspnet-web-page"></a>Paso 4: visualización de la información del cuartil de precios en una página web de ASP.NET

Una vez completada la adición de BLL, hemos vuelto a preparar la creación de una página de ASP.NET que muestra el cuartil de precios de cada producto. Abra la página `AddingColumns.aspx` en la carpeta `AdvancedDAL` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador, estableciendo su propiedad `ID` en `Products`. En la etiqueta inteligente de GridView s, enlácelo a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configure ObjectDataSource para usar el método de `GetProductsWithPriceQuartile` de `ProductsBLLWithSprocs` Class s. Dado que se trata de una cuadrícula de solo lectura, establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna).

[![configurar ObjectDataSource para usar la clase ProductsBLLWithSprocs](adding-additional-datatable-columns-cs/_static/image24.png)](adding-additional-datatable-columns-cs/_static/image23.png)

**Figura 9**: configuración de ObjectDataSource para usar la clase `ProductsBLLWithSprocs` ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image25.png))

[![recuperar información del producto del método GetProductsWithPriceQuartile](adding-additional-datatable-columns-cs/_static/image27.png)](adding-additional-datatable-columns-cs/_static/image26.png)

**Figura 10**: recuperación de la información del producto del método `GetProductsWithPriceQuartile` ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image28.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio agregará automáticamente un CheckBoxField a GridView para cada uno de los campos de datos devueltos por el método. Uno de estos campos de datos es `PriceQuartile`, que es la columna que agregamos al `ProductsDataTable` en el paso 1.

Edite los campos de GridView, quitando todos excepto el `ProductName`, `UnitPrice`y `PriceQuartile` BoundFields. Configure el `UnitPrice` BoundField para dar formato de moneda a su valor y tener los `UnitPrice` y `PriceQuartile` BoundFields alineados a la derecha y al centro, respectivamente. Por último, actualice el resto de las propiedades de BoundFields `HeaderText` a producto, precio y cuartil del precio, respectivamente. Además, active la casilla habilitar ordenación de la etiqueta inteligente de GridView s.

Después de estas modificaciones, el marcado declarativo de GridView y ObjectDataSource s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](adding-additional-datatable-columns-cs/samples/sample3.aspx)]

En la figura 11 se muestra esta página cuando se visita a través de un explorador. Tenga en cuenta que, inicialmente, los productos se ordenan por su precio en orden descendente con cada producto asignado a un valor de `PriceQuartile` adecuado. Por supuesto, estos datos se pueden ordenar por otros criterios con el valor de la columna del cuartil del precio, que sigue reflejando la clasificación del producto en relación con el precio (consulte la figura 12).

[![los productos se ordenan por sus precios](adding-additional-datatable-columns-cs/_static/image30.png)](adding-additional-datatable-columns-cs/_static/image29.png)

**Figura 11**: los productos se ordenan por sus precios ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image31.png))

[![los productos se ordenan por sus nombres](adding-additional-datatable-columns-cs/_static/image33.png)](adding-additional-datatable-columns-cs/_static/image32.png)

**Figura 12**: los productos se ordenan por sus nombres ([haga clic para ver la imagen de tamaño completo](adding-additional-datatable-columns-cs/_static/image34.png))

> [!NOTE]
> Con unas pocas líneas de código, podríamos aumentar el GridView para que el color de las filas del producto se represente en función de su valor de `PriceQuartile`. Es posible que el color de los productos del primer cuartil sea verde claro, los del segundo cuartil, un amarillo claro, etc. Le recomiendo que dedique un momento a agregar esta funcionalidad. Si necesita un actualizador para dar formato a un control GridView, consulte el tutorial sobre el [formato personalizado basado en datos](../custom-formatting/custom-formatting-based-upon-data-cs.md) .

## <a name="an-alternative-approach---creating-another-tableadapter"></a>Un enfoque alternativo: crear otro TableAdapter

Como vimos en este tutorial, al agregar un método a un TableAdapter que devuelve campos de datos distintos de los escritos por la consulta principal, se pueden agregar las columnas correspondientes a la DataTable. Sin embargo, este enfoque solo funciona bien si hay un pequeño número de métodos en el TableAdapter que devuelven campos de datos diferentes y los campos de datos alternativos no varían demasiado de la consulta principal.

En lugar de Agregar columnas a la DataTable, en su lugar, puede agregar otro TableAdapter al conjunto de datos que contiene los métodos del primer TableAdapter que devuelven campos de datos diferentes. En este tutorial, en lugar de agregar la columna `PriceQuartile` a la `ProductsDataTable` (donde solo la usa el método `GetProductsWithPriceQuartile`), podríamos haber agregado un TableAdapter adicional al conjunto de objetos denominado `ProductsWithPriceQuartileTableAdapter` que usaba el `Products_SelectWithPriceQuartile` procedimiento almacenado como consulta principal. Las páginas de ASP.NET que necesitaban obtener información del producto con el cuartil de precios usarían el `ProductsWithPriceQuartileTableAdapter`, mientras que los que no podían seguir usando la `ProductsTableAdapter`.

Al agregar un nuevo TableAdapter, los objetos DataTable permanecen untarnished y sus columnas reflejan exactamente los campos de datos devueltos por sus métodos TableAdapter s. Sin embargo, los TableAdapters adicionales pueden introducir tareas y funciones repetitivas. Por ejemplo, si las páginas de ASP.NET que muestran la columna `PriceQuartile` también necesitan proporcionar compatibilidad para insertar, actualizar y eliminar, el `ProductsWithPriceQuartileTableAdapter` tendría que tener las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` configuradas correctamente. Aunque estas propiedades reflejarían el `ProductsTableAdapter` s, esta configuración introduce un paso adicional. Además, ahora hay dos maneras de actualizar, eliminar o agregar un producto a la base de datos, a través de las clases `ProductsTableAdapter` y `ProductsWithPriceQuartileTableAdapter`.

La descarga de este tutorial incluye una clase `ProductsWithPriceQuartileTableAdapter` en el conjunto de `NorthwindWithSprocs` que ilustra este enfoque alternativo.

## <a name="summary"></a>Resumen

En la mayoría de los escenarios, todos los métodos de un TableAdapter devolverán el mismo conjunto de campos de datos, pero hay ocasiones en las que un determinado método o dos pueden necesitar devolver un campo adicional. Por ejemplo, en la [página maestra/detalle mediante una lista con viñetas de registros maestros con un tutorial de DataList de detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) , se ha agregado un método a la `CategoriesTableAdapter` que, además de los campos de datos principales de la consulta, devolvía un campo `NumberOfProducts` que ha comunicado el número de productos asociados a cada categoría. En este tutorial, hemos examinado cómo agregar un método en la `ProductsTableAdapter` que devolvía un campo `PriceQuartile` además de los campos de datos principales de la consulta. Para capturar campos de datos adicionales devueltos por los métodos de TableAdapter, es necesario agregar las columnas correspondientes a la DataTable.

Si planea agregar columnas manualmente a la DataTable, se recomienda que el TableAdapter Use procedimientos almacenados. Si el TableAdapter utiliza instrucciones SQL ad hoc, siempre que se ejecute el Asistente para configuración de TableAdapter, todos los métodos listas de campos de datos volverán a los campos de datos devueltos por la consulta principal. Este problema no se extiende a los procedimientos almacenados, por lo que se recomiendan y se usaron en este tutorial.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Randy Schmidt, Jacky Goor, Bernadette Leigh y Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](updating-the-tableadapter-to-use-joins-cs.md)
> [Siguiente](working-with-computed-columns-cs.md)
