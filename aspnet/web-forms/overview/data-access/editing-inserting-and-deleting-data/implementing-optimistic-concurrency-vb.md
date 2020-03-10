---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
title: Implementar la simultaneidad optimista (VB) | Microsoft Docs
author: rick-anderson
description: En el caso de una aplicación web que permite a varios usuarios editar datos, existe el riesgo de que dos usuarios puedan editar los mismos datos al mismo tiempo. En este tutori...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 2646968c-2826-4418-b1d0-62610ed177e3
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb
msc.type: authoredcontent
ms.openlocfilehash: 28c39fe2a290cc3a5b093fdd09de341630606137
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78492367"
---
# <a name="implementing-optimistic-concurrency-vb"></a>Implementar la simultaneidad optimista (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_21_VB.exe) de la aplicación de ejemplo o [descarga de PDF](implementing-optimistic-concurrency-vb/_static/datatutorial21vb1.pdf)

> En el caso de una aplicación web que permite a varios usuarios editar datos, existe el riesgo de que dos usuarios puedan editar los mismos datos al mismo tiempo. En este tutorial, vamos a implementar el control de simultaneidad optimista para controlar este riesgo.

## <a name="introduction"></a>Introducción

En el caso de las aplicaciones web que solo permiten a los usuarios ver datos, o para aquellos que incluyen solo un usuario que puede modificar datos, no hay ninguna amenaza de que dos usuarios simultáneos sobrescriban accidentalmente los cambios de los demás. Sin embargo, para las aplicaciones web que permiten a varios usuarios actualizar o eliminar datos, existe la posibilidad de que las modificaciones de un usuario entren en conflicto con otro usuario simultáneo. Si no hay ninguna directiva de simultaneidad implementada, cuando dos usuarios están editando un único registro simultáneamente, el usuario que realiza la confirmación de la última modificación reemplazará los cambios realizados por el primero.

Por ejemplo, Imagine que dos usuarios, Jisun y Sam, visitan una página de la aplicación que permitía a los visitantes actualizar y eliminar los productos a través de un control GridView. Ambos clics en el botón Editar de GridView aproximadamente al mismo tiempo. Jisun cambia el nombre del producto a "Chai té" y hace clic en el botón actualizar. El resultado neto es una instrucción `UPDATE` que se envía a la base de datos, que establece *todos* los campos actualizables del producto (aunque Jisun solo haya actualizado un campo, `ProductName`). En este momento, la base de datos tiene los valores "Chai té", la categoría Beverages, el proveedor exótico Liquiders, etc. para este producto en particular. Sin embargo, la pantalla GridView en el Sam sigue mostrando el nombre del producto en la fila de GridView modificable como "Chai". Unos segundos después de que se hayan confirmado los cambios de JISUN, Sam actualiza la categoría a los condimentos y hace clic en actualizar. Esto da como resultado una `UPDATE` instrucción enviada a la base de datos que establece el nombre del producto en "Chai", el `CategoryID` en el identificador de categoría de bebidas correspondiente, etc. Se han sobrescrito los cambios de Jisun en el nombre del producto. En la figura 1 se muestra gráficamente esta serie de eventos.

[![cuando dos usuarios actualizan simultáneamente un registro, es posible que los cambios de un usuario sobrescriban el](implementing-optimistic-concurrency-vb/_static/image2.png)](implementing-optimistic-concurrency-vb/_static/image1.png)

**Figura 1**: cuando dos usuarios actualizan simultáneamente un registro, es posible que los cambios de un usuario sobrescriban el otro ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image3.png))

Del mismo modo, cuando dos usuarios visitan una página, un usuario podría estar en medio de actualizar un registro cuando lo eliminó otro usuario. O bien, entre cuando un usuario carga una página y cuando hace clic en el botón eliminar, es posible que otro usuario haya modificado el contenido de ese registro.

Hay tres estrategias de [control de simultaneidad](http://en.wikipedia.org/wiki/Concurrency_control) disponibles:

- **No hacer nada** : si los usuarios simultáneos están modificando el mismo registro, permita que la última confirmación gane (el comportamiento predeterminado)
- [**Simultaneidad optimista**](http://en.wikipedia.org/wiki/Optimistic_concurrency_control) : Supongamos que aunque puede haber conflictos de simultaneidad cada vez y, a continuación, la mayoría del tiempo, estos conflictos no surgirán. por lo tanto, si se produce un conflicto, simplemente informe al usuario de que no se pueden guardar los cambios porque otro usuario ha modificado los mismos datos
- **Simultaneidad pesimista** : Supongamos que los conflictos de simultaneidad son comunes y que los usuarios no toleran que sus cambios no se hayan guardado debido a la actividad simultánea de otro usuario. por lo tanto, cuando un usuario empieza a actualizar un registro, lo bloquea, lo que impide que otros usuarios editen o eliminen ese registro hasta que el usuario confirme sus modificaciones.

En la actualidad, todos nuestros tutoriales han usado la estrategia de resolución de simultaneidad predeterminada: es decir, se permite que la última escritura gane. En este tutorial, examinaremos cómo implementar el control de simultaneidad optimista.

> [!NOTE]
> En esta serie de tutoriales no veremos ejemplos de simultaneidad pesimista. Rara vez se usa la simultaneidad pesimista porque tales bloqueos, si no se han renunciado correctamente, pueden impedir que otros usuarios actualicen los datos. Por ejemplo, si un usuario bloquea un registro para su edición y, a continuación, sale del día antes de desbloquearlo, ningún otro usuario podrá actualizar dicho registro hasta que el usuario original devuelva y complete su actualización. Por lo tanto, en situaciones en las que se usa la simultaneidad pesimista, normalmente hay un tiempo de espera que, si se alcanza, cancela el bloqueo. Sitios web de ventas de entradas, que bloquean una ubicación de plazas determinada durante un período de tiempo corto mientras el usuario completa el proceso de pedido, es un ejemplo de control de simultaneidad pesimista.

## <a name="step-1-looking-at-how-optimistic-concurrency-is-implemented"></a>Paso 1: ver cómo se implementa la simultaneidad optimista

El control de simultaneidad optimista funciona asegurándose de que el registro que se está actualizando o eliminando tiene los mismos valores que cuando se inició el proceso de actualización o eliminación. Por ejemplo, al hacer clic en el botón Editar en un GridView modificable, los valores del registro se leen de la base de datos y se muestran en cuadros de texto y otros controles Web. GridView guarda estos valores originales. Más adelante, después de que el usuario realice los cambios y haga clic en el botón actualizar, los valores originales más los nuevos valores se enviarán a la capa de lógica de negocios y, a continuación, a la capa de acceso a datos. La capa de acceso a datos debe emitir una instrucción SQL que solo actualizará el registro si los valores originales que el usuario ha empezado a editar son idénticos a los valores de la base de datos. En la ilustración 2 se muestra esta secuencia de eventos.

[![para que la actualización o eliminación se realice correctamente, los valores originales deben ser iguales a los valores de la base de datos actual.](implementing-optimistic-concurrency-vb/_static/image5.png)](implementing-optimistic-concurrency-vb/_static/image4.png)

**Figura 2**: para que la actualización o eliminación se realice correctamente, los valores originales deben ser iguales a los valores de la base de datos actual ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image6.png))

Existen varios enfoques para implementar la simultaneidad optimista (vea la [lógica de actualización de simultaneidad optimista](http://www.eggheadcafe.com/articles/20050719.asp) de [Peter a. Bromberg](http://peterbromberg.net/)para ver una breve descripción de una serie de opciones). El conjunto de ADO.NET con tipo proporciona una implementación que se puede configurar con solo el paso de una casilla. Al habilitar la simultaneidad optimista para un TableAdapter en el DataSet con tipo, se amplían las instrucciones `UPDATE` y `DELETE` del TableAdapter para incluir una comparación de todos los valores originales en la cláusula `WHERE`. Por ejemplo, la siguiente instrucción `UPDATE` actualiza el nombre y el precio de un producto solo si los valores de la base de datos actual son iguales a los valores que se recuperaron originalmente al actualizar el registro en GridView. Los parámetros `@ProductName` y `@UnitPrice` contienen los nuevos valores introducidos por el usuario, mientras que `@original_ProductName` y `@original_UnitPrice` contienen los valores que se cargaron originalmente en el control GridView cuando se hizo clic en el botón Editar:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample1.sql)]

> [!NOTE]
> Esta instrucción `UPDATE` se ha simplificado para facilitar la lectura. En la práctica, la `UnitPrice` proteger la cláusula de `WHERE` sería más complicada, ya que `UnitPrice` puede contener `NULL` s y comprobar si `NULL = NULL` devuelve siempre false (en su lugar, debe usar `IS NULL`).

Además de utilizar una instrucción de `UPDATE` subyacente diferente, la configuración de un TableAdapter para usar la simultaneidad optimista también modifica la firma de los métodos directos de la base de de. Recuerde que en el primer tutorial, [*la creación de una capa de acceso a datos*](../introduction/creating-a-data-access-layer-cs.md), los métodos directos de DB eran los que aceptan una lista de valores escalares como parámetros de entrada (en lugar de una instancia de DataRow o DataTable fuertemente tipada). Al usar la simultaneidad optimista, los métodos de `Update()` y `Delete()` de la base de datos directa también incluyen los parámetros de entrada de los valores originales. Además, el código de la capa BLL para usar el patrón de actualización por lotes (las sobrecargas del método `Update()` que aceptan DataRows y DataTables en lugar de valores escalares) también debe cambiarse.

En lugar de ampliar nuestros TableAdapters de la capa DAL existentes para usar la simultaneidad optimista (lo que requeriría cambiar la capa BLL para acomodarse), vamos a crear un nuevo conjunto de objetos con tipo denominado `NorthwindOptimisticConcurrency`, al que agregaremos un `Products` TableAdapter que use la simultaneidad optimista. A continuación, crearemos una clase de capa de lógica de negocios `ProductsOptimisticConcurrencyBLL` que tenga las modificaciones adecuadas para admitir la simultaneidad optimista DAL. Una vez que se haya establecido esta preparación, vamos a crear la página ASP.NET.

## <a name="step-2-creating-a-data-access-layer-that-supports-optimistic-concurrency"></a>Paso 2: creación de una capa de acceso a datos que admita la simultaneidad optimista

Para crear un nuevo conjunto de contenido con tipo, haga clic con el botón derecho en la carpeta `DAL` dentro de la carpeta `App_Code` y agregue un nuevo conjunto de tipos denominado `NorthwindOptimisticConcurrency`. Como vimos en el primer tutorial, al hacerlo, se agregará un nuevo TableAdapter al conjunto de DataSet con tipo, con lo que se iniciará automáticamente el Asistente para configuración de TableAdapter. En la primera pantalla, se le pedirá que especifique la base de datos a la que conectarse: Conéctese a la misma base de datos Northwind con la configuración `NORTHWNDConnectionString` de `Web.config`.

[![conectarse a la misma base de datos Northwind](implementing-optimistic-concurrency-vb/_static/image8.png)](implementing-optimistic-concurrency-vb/_static/image7.png)

**Figura 3**: conexión a la misma base de datos Northwind ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image9.png))

A continuación, se le pedirá que consulte Cómo consultar los datos: a través de una instrucción SQL ad hoc, un nuevo procedimiento almacenado o un procedimiento almacenado existente. Como hemos usado consultas SQL ad hoc en la capa DAL original, use esta opción aquí también.

[![especificar los datos que se van a recuperar mediante una instrucción SQL ad hoc](implementing-optimistic-concurrency-vb/_static/image11.png)](implementing-optimistic-concurrency-vb/_static/image10.png)

**Figura 4**: especificación de los datos que se van a recuperar mediante una instrucción SQL ad hoc ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image12.png))

En la siguiente pantalla, escriba la consulta SQL que se va a usar para recuperar la información del producto. Vamos a usar la misma consulta SQL exacta utilizada para el `Products` TableAdapter de la capa DAL original, que devuelve todas las columnas `Product` junto con los nombres de categoría y proveedor del producto:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample2.sql)]

[![usar la misma consulta SQL de los productos TableAdapter en la capa DAL original](implementing-optimistic-concurrency-vb/_static/image14.png)](implementing-optimistic-concurrency-vb/_static/image13.png)

**Figura 5**: uso de la misma consulta SQL desde el `Products` TableAdapter en la capa Dal original ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image15.png))

Antes de pasar a la siguiente pantalla, haga clic en el botón Opciones avanzadas. Para que este TableAdapter emplee el control de simultaneidad optimista, solo tiene que activar la casilla "usar simultaneidad optimista".

[![habilitar el control de simultaneidad optimista activando la casilla de verificación &quot;usar&quot; de simultaneidad optimista](implementing-optimistic-concurrency-vb/_static/image17.png)](implementing-optimistic-concurrency-vb/_static/image16.png)

**Figura 6**: habilitación del control de simultaneidad optimista activando la casilla "usar simultaneidad optimista" ([haga clic para ver la imagen a tamaño completo](implementing-optimistic-concurrency-vb/_static/image18.png))

Por último, indique que el TableAdapter debe utilizar los patrones de acceso a datos que rellenan un objeto DataTable y devuelven una DataTable. indique también que se deben crear los métodos directos de la base de de. Cambie el nombre del método para devolver un modelo DataTable de GetData a GetProducts, de modo que se reflejen las convenciones de nomenclatura que se usan en la capa DAL original.

[![que TableAdapter Use todos los patrones de acceso a datos](implementing-optimistic-concurrency-vb/_static/image20.png)](implementing-optimistic-concurrency-vb/_static/image19.png)

**Figura 7**: hacer que TableAdapter Use todos los patrones de acceso a datos ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image21.png))

Después de completar el asistente, el diseñador de DataSet incluirá un `Products` DataTable y un TableAdapter fuertemente tipados. Dedique un momento a cambiar el nombre de la tabla de texto de `Products` a `ProductsOptimisticConcurrency`, que puede hacer haciendo clic con el botón secundario en la barra de título de la DataTable y eligiendo cambiar nombre en el menú contextual.

[![DataTable y TableAdapter se han agregado al DataSet con tipo](implementing-optimistic-concurrency-vb/_static/image23.png)](implementing-optimistic-concurrency-vb/_static/image22.png)

**Figura 8**: se han agregado un DataTable y un TableAdapter al conjunto de tipos ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image24.png))

Para ver las diferencias entre las consultas `UPDATE` y `DELETE` entre la `ProductsOptimisticConcurrency` TableAdapter (que usa la simultaneidad optimista) y los productos TableAdapter (que no), haga clic en el TableAdapter y vaya al ventana Propiedades. En las subpropiedades de `CommandText` de propiedades `DeleteCommand` y `UpdateCommand`, puede ver la sintaxis SQL real que se envía a la base de datos cuando se invocan los métodos relacionados con la actualización o eliminación de DAL. Para el `ProductsOptimisticConcurrency` TableAdapter, la instrucción `DELETE` utilizada es:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample3.sql)]

Mientras que la instrucción `DELETE` para el TableAdapter del producto en la capa DAL original es mucho más sencilla:

[!code-sql[Main](implementing-optimistic-concurrency-vb/samples/sample4.sql)]

Como puede ver, la cláusula `WHERE` de la instrucción `DELETE` para el TableAdapter que utiliza la simultaneidad optimista incluye una comparación entre cada uno de los valores de columna existentes de la tabla `Product` y los valores originales en el momento en que se rellenó GridView (o DetailsView o FormView) por última vez. Dado que todos los campos distintos de `ProductID`, `ProductName`y `Discontinued` pueden tener `NULL` valores, se incluyen parámetros y comprobaciones adicionales para comparar correctamente `NULL` valores en la cláusula `WHERE`.

No vamos a agregar ninguna tabla de datos adicional al conjunto de datos con la simultaneidad optimista habilitada para este tutorial, ya que nuestra página de ASP.NET solo proporcionará la actualización y eliminación de la información del producto. Sin embargo, todavía es necesario agregar el método `GetProductByProductID(productID)` a la `ProductsOptimisticConcurrency` TableAdapter.

Para ello, haga clic con el botón secundario en la barra de título del TableAdapter (el área que se encuentra encima de los nombres de método `Fill` y `GetProducts`) y elija Agregar consulta en el menú contextual. Se iniciará el Asistente para la configuración de consultas de TableAdapter. Al igual que con la configuración inicial de su TableAdapter, opte por crear el método de `GetProductByProductID(productID)` mediante una instrucción SQL ad hoc (consulte la figura 4). Dado que el método `GetProductByProductID(productID)` devuelve información acerca de un producto determinado, indique que esta consulta es un tipo de consulta `SELECT` que devuelve filas.

[![marcar el tipo de consulta como &quot;SELECT que devuelve las filas&quot;](implementing-optimistic-concurrency-vb/_static/image26.png)](implementing-optimistic-concurrency-vb/_static/image25.png)

**Figura 9**: marcar el tipo de consulta como "`SELECT` que devuelve filas" ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image27.png))

En la siguiente pantalla se le pedirá la consulta SQL que se va a usar, con la consulta predeterminada del TableAdapter cargada previamente. Aumente la consulta existente para incluir la cláusula `WHERE ProductID = @ProductID`, como se muestra en la figura 10.

[![agregar una cláusula WHERE a la consulta cargada previamente para devolver un registro de producto específico](implementing-optimistic-concurrency-vb/_static/image29.png)](implementing-optimistic-concurrency-vb/_static/image28.png)

**Figura 10**: agregar una cláusula `WHERE` a la consulta precargada para devolver un registro de producto específico ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image30.png))

Por último, cambie los nombres de método generados por `FillByProductID` y `GetProductByProductID`.

[![cambiar el nombre de los métodos a FillByProductID y GetProductByProductID](implementing-optimistic-concurrency-vb/_static/image32.png)](implementing-optimistic-concurrency-vb/_static/image31.png)

**Figura 11**: cambiar el nombre de los métodos a `FillByProductID` y `GetProductByProductID` ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image33.png))

Con este asistente completo, el TableAdapter ahora contiene dos métodos para recuperar datos: `GetProducts()`, que devuelve *todos* los productos; y `GetProductByProductID(productID)`, que devuelve el producto especificado.

## <a name="step-3-creating-a-business-logic-layer-for-the-optimistic-concurrency-enabled-dal"></a>Paso 3: crear una capa de lógica de negocios para la DAL con simultaneidad optimista habilitada

Nuestra clase de `ProductsBLL` existente tiene ejemplos de uso de los modelos de actualización por lotes y de DB Direct. El método `AddProduct` y las sobrecargas de `UpdateProduct` usan el patrón de actualización por lotes, pasando una instancia de `ProductRow` al método Update de TableAdapter. Por otro lado, el método `DeleteProduct` usa el patrón de base de BD Direct, que llama al método `Delete(productID)` de TableAdapter.

Con el nuevo `ProductsOptimisticConcurrency` TableAdapter, los métodos de DB Direct ahora requieren que se pasen los valores originales. Por ejemplo, el método `Delete` espera ahora diez parámetros de entrada: el `ProductID`original, `ProductName`, `SupplierID`, `CategoryID`, `QuantityPerUnit`, `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`, `ReorderLevel`y `Discontinued`. Usa estos valores de parámetros de entrada adicionales en `WHERE` cláusula de la instrucción `DELETE` enviada a la base de datos, eliminando solo el registro especificado si los valores actuales de la base de datos se asignan a los originales.

Aunque no ha cambiado la firma de método del método `Update` del TableAdapter usado en el patrón de actualización por lotes, el código necesario para registrar los valores originales y nuevos tiene. Por lo tanto, en lugar de intentar usar la capa DAL habilitada para simultaneidad optimista con nuestra clase de `ProductsBLL` existente, vamos a crear una nueva clase de capa de lógica de negocios para trabajar con la nueva capa DAL.

Agregue una clase denominada `ProductsOptimisticConcurrencyBLL` a la carpeta `BLL` en la carpeta `App_Code`.

![Agregar la clase ProductsOptimisticConcurrencyBLL a la carpeta BLL](implementing-optimistic-concurrency-vb/_static/image34.png)

**Figura 12**: adición de la clase `ProductsOptimisticConcurrencyBLL` a la carpeta BLL

A continuación, agregue el siguiente código a la clase `ProductsOptimisticConcurrencyBLL`:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample5.vb)]

Observe la instrucción using `NorthwindOptimisticConcurrencyTableAdapters` encima del inicio de la declaración de clase. El espacio de nombres `NorthwindOptimisticConcurrencyTableAdapters` contiene la clase `ProductsOptimisticConcurrencyTableAdapter`, que proporciona los métodos de la capa DAL. También antes de la declaración de clase encontrará el `System.ComponentModel.DataObject` atributo, que indica a Visual Studio que incluya esta clase en la lista desplegable del asistente ObjectDataSource.

La propiedad `Adapter` del `ProductsOptimisticConcurrencyBLL`proporciona acceso rápido a una instancia de la clase `ProductsOptimisticConcurrencyTableAdapter` y sigue el patrón que se usa en las clases de BLL originales (`ProductsBLL`, `CategoriesBLL`, etc.). Por último, el método `GetProducts()` simplemente llama al método `GetProducts()` de la capa DAL y devuelve un objeto `ProductsOptimisticConcurrencyDataTable` rellenado con una instancia de `ProductsOptimisticConcurrencyRow` para cada registro del producto en la base de datos.

## <a name="deleting-a-product-using-the-db-direct-pattern-with-optimistic-concurrency"></a>Eliminación de un producto mediante el patrón de base de BD Direct con simultaneidad optimista

Cuando se usa el patrón de base de BD Direct en una capa DAL que usa la simultaneidad optimista, se deben pasar los valores nuevos y originales a los métodos. Para eliminar, no hay valores nuevos, por lo que solo es necesario pasar los valores originales. En nuestro BLL, debemos aceptar todos los parámetros originales como parámetros de entrada. Vamos a tener el método de `DeleteProduct` en la clase `ProductsOptimisticConcurrencyBLL` usar el método de DB Direct. Esto significa que este método debe tomar los diez campos de datos de producto como parámetros de entrada y pasarlos a la capa DAL, tal como se muestra en el código siguiente:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample6.vb)]

Si los valores originales (los valores que se cargaron por última vez en GridView (o DetailsView o FormView) difieren de los valores de la base de datos cuando el usuario hace clic en el botón eliminar, la cláusula `WHERE` no coincidirá con ningún registro de base de datos y no se verán afectados los registros. Por lo tanto, el método `Delete` del TableAdapter devolverá `0` y el método `DeleteProduct` de la capa BLL devolverá `false`.

## <a name="updating-a-product-using-the-batch-update-pattern-with-optimistic-concurrency"></a>Actualización de un producto mediante el patrón de actualización por lotes con simultaneidad optimista

Como se indicó anteriormente, el método `Update` del TableAdapter para el patrón de actualización por lotes tiene la misma firma de método independientemente de si se emplea o no la simultaneidad optimista. A saber, el método `Update` espera una DataRow, una matriz de DataRows, una DataTable o un DataSet con tipo. No hay parámetros de entrada adicionales para especificar los valores originales. Esto es posible porque la DataTable realiza un seguimiento de los valores originales y modificados de sus DataRow (s). Cuando la capa DAL emite su instrucción `UPDATE`, los parámetros de `@original_ColumnName` se rellenan con los valores originales de DataRow, mientras que los parámetros de `@ColumnName` se rellenan con los valores modificados de DataRow.

En la clase `ProductsBLL` (que usa nuestra capa de simultaneidad original, no optimista), al usar el patrón de actualización por lotes para actualizar la información del producto, nuestro código realiza la siguiente secuencia de eventos:

1. Leer la información actual del producto de la base de datos en una instancia de `ProductRow` mediante el método `GetProductByProductID(productID)` del TableAdapter
2. Asignar los nuevos valores a la instancia de `ProductRow` del paso 1
3. Llame al método `Update` del TableAdapter, pasando la instancia de `ProductRow`.

Sin embargo, esta secuencia de pasos no admitirá correctamente la simultaneidad optimista porque los `ProductRow` rellenados en el paso 1 se rellenan directamente a partir de la base de datos, lo que significa que los valores originales utilizados por DataRow son los que existen actualmente en la base de datos, y no los que estaban enlazados a GridView al comienzo del proceso de edición. En su lugar, cuando se usa una capa DAL habilitada para simultaneidad optimista, es necesario modificar las sobrecargas del método `UpdateProduct` para usar los siguientes pasos:

1. Leer la información actual del producto de la base de datos en una instancia de `ProductsOptimisticConcurrencyRow` mediante el método `GetProductByProductID(productID)` del TableAdapter
2. Asigne los valores *originales* a la instancia de `ProductsOptimisticConcurrencyRow` del paso 1.
3. Llame al método `AcceptChanges()` de la instancia de `ProductsOptimisticConcurrencyRow`, que indica al DataRow que sus valores actuales son los "originales".
4. Asignar los *nuevos* valores a la instancia de `ProductsOptimisticConcurrencyRow`
5. Llame al método `Update` del TableAdapter, pasando la instancia de `ProductsOptimisticConcurrencyRow`.

En el paso 1 se leen todos los valores de la base de datos actual del registro de producto especificado. Este paso es superfluo en la sobrecarga `UpdateProduct` que actualiza *todas* las columnas Product (ya que estos valores se sobrescriben en el paso 2), pero es esencial para las sobrecargas en las que solo un subconjunto de los valores de columna se pasan como parámetros de entrada. Una vez asignados los valores originales a la instancia de `ProductsOptimisticConcurrencyRow`, se llama al método `AcceptChanges()`, que marca los valores de DataRow actuales como los valores originales que se van a usar en los parámetros `@original_ColumnName` de la instrucción `UPDATE`. A continuación, se asignan los nuevos valores de parámetro al `ProductsOptimisticConcurrencyRow` y, por último, se invoca el método `Update`, pasando el DataRow.

En el código siguiente se muestra la sobrecarga `UpdateProduct` que acepta todos los campos de datos de producto como parámetros de entrada. Aunque no se muestra aquí, la clase `ProductsOptimisticConcurrencyBLL` incluida en la descarga para este tutorial también contiene una sobrecarga de `UpdateProduct` que acepta solo el nombre y el precio del producto como parámetros de entrada.

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample7.vb)]

## <a name="step-4-passing-the-original-and-new-values-from-the-aspnet-page-to-the-bll-methods"></a>Paso 4: pasar los valores originales y nuevos de la página ASP.NET a los métodos BLL

Con DAL y BLL completado, todo lo que queda es crear una página ASP.NET que pueda emplear la lógica de simultaneidad optimista integrada en el sistema. En concreto, el control Web de datos (GridView, DetailsView o FormView) debe recordar sus valores originales y el ObjectDataSource debe pasar ambos conjuntos de valores a la capa de lógica de negocios. Además, la página ASP.NET debe estar configurada para controlar correctamente las infracciones de simultaneidad.

Para empezar, abra la página `OptimisticConcurrency.aspx` en la carpeta `EditInsertDelete` y agregue un control GridView al diseñador y establezca su propiedad `ID` en `ProductsGrid`. En la etiqueta inteligente de GridView, opte por crear un nuevo ObjectDataSource denominado `ProductsOptimisticConcurrencyDataSource`. Dado que deseamos que ObjectDataSource use la capa DAL que admite la simultaneidad optimista, configúrela para usar el objeto `ProductsOptimisticConcurrencyBLL`.

[![que ObjectDataSource use el objeto ProductsOptimisticConcurrencyBLL](implementing-optimistic-concurrency-vb/_static/image36.png)](implementing-optimistic-concurrency-vb/_static/image35.png)

**Figura 13**: hacer que ObjectDataSource use el objeto de `ProductsOptimisticConcurrencyBLL` ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image37.png))

Elija los métodos `GetProducts`, `UpdateProduct`y `DeleteProduct` en las listas desplegables del asistente. Para el método UpdateProduct, use la sobrecarga que acepta todos los campos de datos del producto.

## <a name="configuring-the-objectdatasource-controls-properties"></a>Configurar las propiedades del control ObjectDataSource

Después de completar el asistente, el marcado declarativo de ObjectDataSource debería tener un aspecto similar al siguiente:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample8.aspx)]

Como puede ver, la colección `DeleteParameters` contiene una instancia de `Parameter` para cada uno de los diez parámetros de entrada en el método `DeleteProduct` de la clase `ProductsOptimisticConcurrencyBLL`. Del mismo modo, la colección de `UpdateParameters` contiene una instancia de `Parameter` para cada uno de los parámetros de entrada en `UpdateProduct`.

En el caso de los tutoriales anteriores que implican la modificación de los datos, quitaremos la propiedad `OldValuesParameterFormatString` de ObjectDataSource en este momento, ya que esta propiedad indica que el método BLL espera que se pasen los valores antiguos (o originales), así como los nuevos valores. Además, este valor de propiedad indica los nombres de los parámetros de entrada de los valores originales. Dado que pasamos los valores originales a la capa BLL, *no* Quite esta propiedad.

> [!NOTE]
> El valor de la propiedad `OldValuesParameterFormatString` debe asignarse a los nombres de los parámetros de entrada de la capa BLL que esperan los valores originales. Como hemos denominado estos parámetros `original_productName`, `original_supplierID`, etc., puede dejar el valor de la propiedad `OldValuesParameterFormatString` como `original_{0}`. Sin embargo, si los parámetros de entrada de los métodos BLL tenían nombres como `old_productName`, `old_supplierID`, etc., tendría que actualizar la propiedad `OldValuesParameterFormatString` a `old_{0}`.

Hay una configuración de propiedad final que se debe realizar para que ObjectDataSource pase correctamente los valores originales a los métodos de BLL. ObjectDataSource tiene una [propiedad ConflictDetection](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.conflictdetection.aspx) que se puede asignar a [uno de dos valores](https://msdn.microsoft.com/library/system.web.ui.conflictoptions.aspx):

- `OverwriteChanges`: el valor predeterminado; no envía los valores originales a los parámetros de entrada originales de los métodos de BLL
- `CompareAllValues`-envía los valores originales a los métodos de BLL; Elija esta opción cuando use la simultaneidad optimista

Dedique un momento a establecer la propiedad `ConflictDetection` en `CompareAllValues`.

## <a name="configuring-the-gridviews-properties-and-fields"></a>Configurar las propiedades y los campos de GridView

Una vez que las propiedades de ObjectDataSource estén configuradas correctamente, vamos a activar la atención para configurar GridView. En primer lugar, como queremos que GridView admita la edición y eliminación, haga clic en las casillas habilitar edición y habilitar eliminación de la etiqueta inteligente de GridView. Esto agregará un CommandField cuyo `ShowEditButton` y `ShowDeleteButton` están establecidos en `true`.

Cuando se enlaza a la `ProductsOptimisticConcurrencyDataSource` ObjectDataSource, GridView contiene un campo para cada uno de los campos de datos del producto. Aunque este tipo de GridView se puede editar, la experiencia del usuario es algo que es aceptable. Los `CategoryID` y `SupplierID` BoundFields se representarán como cuadros de texto, lo que requiere que el usuario especifique la categoría y el proveedor adecuados como números de identificador. No habrá ningún formato para los campos numéricos y ningún control de validación para asegurarse de que se ha proporcionado el nombre del producto y de que el precio unitario, las unidades en existencias, las unidades en el pedido y los valores de nivel de reordenación son valores numéricos correctos y son mayores o iguales en cero.

Como se explicó en los tutoriales *Agregar controles de validación a las interfaces de edición e inserción* y *Personalización de la interfaz de modificación de datos* , se puede personalizar la interfaz de usuario reemplazando BoundFields por TemplateFields. He modificado este GridView y su interfaz de edición de las siguientes maneras:

- Se han quitado los `ProductID`, `SupplierName`y `CategoryName` BoundFields
- Se ha convertido el `ProductName` BoundField en TemplateField y se ha agregado un control RequiredFieldValidation.
- Se ha convertido el `CategoryID` y `SupplierID` BoundFields en TemplateFields y se ha ajustado la interfaz de edición para usar DropDownLists en lugar de cuadros de texto. En estos `ItemTemplates`de TemplateFields, se muestran los campos de datos `CategoryName` y `SupplierName`.
- Se han convertido los `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`y `ReorderLevel` BoundFields en TemplateFields y se han agregado controles CompareValidator.

Como ya hemos examinado cómo llevar a cabo estas tareas en los tutoriales anteriores, solo mostraré la sintaxis declarativa final y dejaremos la implementación como práctica.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample9.aspx)]

Estamos muy cerca de tener un ejemplo totalmente funcional. Sin embargo, hay algunos matices que se expedirán y provocarán problemas. Además, todavía necesitamos una interfaz que alerta al usuario cuando se ha producido una infracción de simultaneidad.

> [!NOTE]
> Para que un control Web de datos pase correctamente los valores originales a ObjectDataSource (que luego se pasan a la capa BLL), es fundamental que la propiedad `EnableViewState` de GridView esté establecida en `true` (el valor predeterminado). Si deshabilita el estado de vista, los valores originales se pierden en el PostBack.

## <a name="passing-the-correct-original-values-to-the-objectdatasource"></a>Pasar los valores originales correctos a ObjectDataSource

Hay un par de problemas con la forma en que se ha configurado GridView. Si la propiedad `ConflictDetection` de ObjectDataSource está establecida en `CompareAllValues` (tal cual es nuestra), cuando GridView (o DetailsView o FormView) invoca los métodos de la `Update()` o `Delete()` de ObjectDataSource, el ObjectDataSource intenta copiar los valores originales de GridView en sus instancias de `Parameter` adecuadas. Consulte la figura 2 para obtener una representación gráfica de este proceso.

En concreto, a los valores originales de GridView se les asignan los valores de las instrucciones de enlace de datos bidireccionales cada vez que los datos están enlazados a GridView. Por lo tanto, es esencial que todos los valores originales necesarios se capturen a través de un enlace de los dos sentidos y que se proporcionen en un formato convertible.

Para ver por qué esto es importante, dedique un momento a visitar la página en un explorador. Como se esperaba, el control GridView muestra cada producto con un botón Editar y eliminar en la columna situada más a la izquierda.

[![los productos se muestran en un control GridView](implementing-optimistic-concurrency-vb/_static/image39.png)](implementing-optimistic-concurrency-vb/_static/image38.png)

**Figura 14**: los productos se muestran en un control GridView ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image40.png))

Si hace clic en el botón eliminar de cualquier producto, se produce una `FormatException`.

[![intentar eliminar los resultados de un producto en FormatException](implementing-optimistic-concurrency-vb/_static/image42.png)](implementing-optimistic-concurrency-vb/_static/image41.png)

**Figura 15**: intento de eliminar los resultados de un producto en un `FormatException` ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image43.png))

El `FormatException` se genera cuando ObjectDataSource intenta leer en el valor de `UnitPrice` original. Como el `ItemTemplate` tiene el `UnitPrice` con formato de moneda (`<%# Bind("UnitPrice", "{0:C}") %>`), incluye un símbolo de moneda, como $19,95. El `FormatException` se produce como ObjectDataSource intenta convertir esta cadena en un `decimal`. Para evitar este problema, tenemos varias opciones:

- Quite el formato de moneda de la `ItemTemplate`. Es decir, en lugar de usar `<%# Bind("UnitPrice", "{0:C}") %>`, simplemente use `<%# Bind("UnitPrice") %>`. La desventaja es que ya no se da formato al precio.
- Muestre el `UnitPrice` con formato de moneda en el `ItemTemplate`, pero use la palabra clave `Eval` para lograrlo. Recuerde que `Eval` realiza el enlace de una sola dirección. Todavía es necesario proporcionar el valor de `UnitPrice` para los valores originales, por lo que seguiremos necesitando una instrucción de enlace de los dos sentidos en el `ItemTemplate`, pero se puede colocar en un control Web de etiqueta cuya propiedad `Visible` esté establecida en `false`. Podríamos usar el marcado siguiente en ItemTemplate:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample10.aspx)]

- Quite el formato de moneda del `ItemTemplate`mediante `<%# Bind("UnitPrice") %>`. En el controlador de eventos `RowDataBound` de GridView, acceda mediante programación al control Web Label en el que se muestra el valor `UnitPrice` y establezca su propiedad `Text` en la versión con formato.
- Deje el `UnitPrice` con formato de moneda. En el controlador de eventos `RowDeleting` de GridView, reemplace el valor de `UnitPrice` original existente ($19,95) por un valor decimal real mediante `Decimal.Parse`. Vimos cómo conseguir algo similar en el controlador de eventos `RowUpdating` en el [*control de excepciones de nivel BLL y Dal en un tutorial de página de ASP.net*](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) .

En mi ejemplo, decidí usar el segundo enfoque, agregar un control Web de etiqueta oculto cuya propiedad `Text` sea datos bidireccionales enlazados al valor de `UnitPrice` sin formato.

Después de resolver este problema, intente hacer clic de nuevo en el botón eliminar de cualquier producto. Esta vez obtendrá un `InvalidOperationException` cuando ObjectDataSource intente invocar el método de `UpdateProduct` de la capa BLL.

[![ObjectDataSource no encuentra un método con los parámetros de entrada que desea enviar](implementing-optimistic-concurrency-vb/_static/image45.png)](implementing-optimistic-concurrency-vb/_static/image44.png)

**Figura 16**: ObjectDataSource no encuentra un método con los parámetros de entrada que desea enviar ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image46.png))

Al examinar el mensaje de la excepción, es evidente que ObjectDataSource desea invocar un método BLL `DeleteProduct` que incluye los parámetros de entrada `original_CategoryName` y `original_SupplierName`. Esto se debe a que los `ItemTemplate` s para el `CategoryID` y `SupplierID` TemplateFields contienen actualmente instrucciones de enlace bidireccionales con los campos de datos `CategoryName` y `SupplierName`. En su lugar, es necesario incluir `Bind` instrucciones con los campos de datos `CategoryID` y `SupplierID`. Para ello, reemplace las instrucciones BIND existentes por `Eval` instrucciones y, a continuación, agregue controles de etiqueta ocultos cuyas propiedades `Text` estén enlazadas a los campos de datos `CategoryID` y `SupplierID` mediante el enlace de datos bidireccional, como se muestra a continuación:

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample11.aspx)]

Con estos cambios, ahora podemos eliminar y editar correctamente la información del producto. En el paso 5 veremos cómo comprobar que se detectan infracciones de simultaneidad. Pero por ahora, dedique unos minutos a intentar actualizar y eliminar algunos registros para asegurarse de que la actualización y la eliminación de un solo usuario funcionan según lo previsto.

## <a name="step-5-testing-the-optimistic-concurrency-support"></a>Paso 5: probar la compatibilidad con la simultaneidad optimista

Con el fin de comprobar que se detectan infracciones de simultaneidad (en lugar de que se sobrescriban los datos), es necesario abrir dos ventanas del explorador en esta página. En ambas instancias del explorador, haga clic en el botón Editar para Chai. A continuación, en solo uno de los exploradores, cambie el nombre a "Chai té" y haga clic en actualizar. La actualización debe realizarse correctamente y devolver GridView a su estado de edición previa, con "Chai té" como el nuevo nombre del producto.

En la otra instancia de la ventana del explorador, sin embargo, el cuadro de texto nombre del producto sigue mostrando "Chai". En esta segunda ventana del explorador, actualice el `UnitPrice` a `25.00`. Sin la compatibilidad con la simultaneidad optimista, al hacer clic en actualizar en la segunda instancia del explorador se cambia el nombre del producto de nuevo a "Chai", con lo que se sobrescriben los cambios realizados por la primera instancia del explorador. Sin embargo, con la simultaneidad optimista empleada, al hacer clic en el botón actualizar de la segunda instancia del explorador, se produce una [DBConcurrencyException](https://msdn.microsoft.com/library/system.data.dbconcurrencyexception.aspx).

[![cuando se detecta una infracción de simultaneidad, se produce una excepción DBConcurrencyException](implementing-optimistic-concurrency-vb/_static/image48.png)](implementing-optimistic-concurrency-vb/_static/image47.png)

**Figura 17**: cuando se detecta una infracción de simultaneidad, se produce una `DBConcurrencyException` ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image49.png))

El `DBConcurrencyException` solo se produce cuando se emplea el modelo de actualización por lotes de la capa DAL. El patrón de DB Direct no genera una excepción, simplemente indica que no se ha visto afectada ninguna fila. Para ilustrar esto, devuelva el valor de GridView de ambas instancias del explorador a su estado de edición previa. Después, en la primera instancia del explorador, haga clic en el botón Editar y cambie el nombre del producto de "Chai té" a "Chai" y haga clic en actualizar. En la segunda ventana del explorador, haga clic en el botón eliminar de Chai.

Al hacer clic en eliminar, la página se devuelve, el control GridView invoca el método `Delete()` de ObjectDataSource y el ObjectDataSource llama al método `DeleteProduct` de la clase `ProductsOptimisticConcurrencyBLL`, pasando a lo largo de los valores originales. El valor de `ProductName` original para la segunda instancia del explorador es "Chai té", que no coincide con el valor de `ProductName` actual de la base de datos. Por lo tanto, la instrucción `DELETE` emitida a la base de datos afecta a cero filas, ya que no hay ningún registro en la base de datos que cumpla la cláusula `WHERE`. El método `DeleteProduct` devuelve `false` y los datos de ObjectDataSource se reenlazan a GridView.

Desde la perspectiva del usuario final, al hacer clic en el botón Eliminar para el té de Chai en la segunda ventana del explorador se produjo que la pantalla parpadee y, al volver, el producto sigue estando allí, aunque ahora aparece como "Chai" (el cambio de nombre de producto realizado por el primer explorador instancia). Si el usuario vuelve a hacer clic en el botón eliminar, la eliminación se realizará correctamente, ya que el valor de `ProductName` original de GridView ("Chai") coincide ahora con el valor de la base de datos.

En ambos casos, la experiencia del usuario está lejos del ideal. No queremos mostrar al usuario los detalles esenciales de la `DBConcurrencyException` excepción al usar el patrón de actualización por lotes. Y el comportamiento cuando se usa el patrón de DB Direct es algo confuso a medida que se produce un error en el comando de los usuarios, pero no hay ninguna indicación precisa de por qué.

Para solucionar estos dos problemas, podemos crear controles Web de etiqueta en la página que proporcionan una explicación de por qué se produjo un error en una actualización o una eliminación. Para el patrón de actualización por lotes, podemos determinar si se produjo o no una excepción `DBConcurrencyException` en el controlador de eventos de nivel posterior de GridView, mostrando la etiqueta de advertencia según sea necesario. En el método de DB Direct, podemos examinar el valor devuelto del método BLL (que es `true` si una fila se ve afectada, `false` de otro modo) y mostrar un mensaje informativo según sea necesario.

## <a name="step-6-adding-informational-messages-and-displaying-them-in-the-face-of-a-concurrency-violation"></a>Paso 6: agregar mensajes informativos y mostrarlos en caso de que se produzca una infracción de simultaneidad

Cuando se produce una infracción de simultaneidad, el comportamiento que se exhibe depende de si se ha utilizado la actualización por lotes o el patrón de base de BD de la capa DAL. En nuestro tutorial se usan ambos patrones, con el patrón de actualización por lotes que se usa para la actualización y el patrón de DB Direct que se usa para la eliminación. Para empezar, vamos a agregar dos controles Web Label a nuestra página que explican que se ha producido una infracción de simultaneidad al intentar eliminar o actualizar los datos. Establezca las propiedades `Visible` y `EnableViewState` del control de etiqueta en `false`; Esto hará que se oculten en cada visita de página, excepto en las visitas de página concretas en las que la propiedad `Visible` se establece mediante programación en `true`.

[!code-aspx[Main](implementing-optimistic-concurrency-vb/samples/sample12.aspx)]

Además de establecer las propiedades `Visible`, `EnabledViewState`y `Text`, también se ha establecido la propiedad `CssClass` en `Warning`, lo que hace que la etiqueta se muestre en una fuente grande, roja, cursiva y negrita. Esta clase de `Warning` CSS se definió y se agregó a Styles. CSS de nuevo en el tutorial *examinar los eventos asociados con la inserción, actualización y eliminación* .

Después de agregar estas etiquetas, el diseñador de Visual Studio debería tener un aspecto similar al de la figura 18.

[![se han agregado dos controles de etiqueta a la página](implementing-optimistic-concurrency-vb/_static/image51.png)](implementing-optimistic-concurrency-vb/_static/image50.png)

**Figura 18**: se han agregado dos controles de etiqueta a la página ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image52.png))

Con estos controles Web de etiqueta en su lugar, estamos preparados para examinar cómo determinar cuándo se ha producido una infracción de simultaneidad, momento en el que la propiedad `Visible` de la etiqueta adecuada se puede establecer en `true`, mostrando el mensaje informativo.

## <a name="handling-concurrency-violations-when-updating"></a>Controlar infracciones de simultaneidad al actualizar

Veamos primero cómo administrar las infracciones de simultaneidad al usar el patrón de actualización por lotes. Puesto que estas infracciones con el patrón de actualización por lotes provocan que se inicie una excepción `DBConcurrencyException`, es necesario agregar código a la página ASP.NET para determinar si se ha producido una excepción `DBConcurrencyException` durante el proceso de actualización. Si es así, se debe mostrar un mensaje al usuario que explique que sus cambios no se guardaron porque otro usuario modificó los mismos datos entre el momento en que comenzó a editar el registro y cuando hizo clic en el botón actualizar.

Como hemos visto en el artículo sobre cómo *controlar las excepciones de nivel BLL y Dal en un tutorial de páginas de ASP.net* , estas excepciones se pueden detectar y suprimir en los controladores de eventos de nivel posterior del control Web de datos. Por lo tanto, es necesario crear un controlador de eventos para el evento de `RowUpdated` de GridView que comprueba si se ha producido una excepción de `DBConcurrencyException`. A este controlador de eventos se le pasa una referencia a cualquier excepción que se provocó durante el proceso de actualización, como se muestra en el código del controlador de eventos siguiente:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample13.vb)]

En el caso de una excepción `DBConcurrencyException`, este controlador de eventos muestra el control etiqueta de `UpdateConflictMessage` e indica que se ha controlado la excepción. Con este código en su lugar, cuando se produce una infracción de simultaneidad al actualizar un registro, se pierden los cambios del usuario, ya que se habrían sobrescrito las modificaciones de otro usuario al mismo tiempo. En concreto, el control GridView se devuelve a su estado de edición previa y se enlaza a los datos de la base de datos actual. Esto actualizará la fila de GridView con los cambios del otro usuario, que antes no eran visibles. Además, el control etiqueta de `UpdateConflictMessage` le explicará al usuario lo que ha sucedido. Esta secuencia de eventos se detalla en la figura 19.

[![se pierden las actualizaciones de un usuario en caso de que se produzca una infracción de simultaneidad](implementing-optimistic-concurrency-vb/_static/image54.png)](implementing-optimistic-concurrency-vb/_static/image53.png)

**Figura 19**: las actualizaciones de un usuario se pierden en el caso de una infracción de simultaneidad ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image55.png))

> [!NOTE]
> Como alternativa, en lugar de devolver GridView al estado de edición previa, podríamos dejar GridView en su estado de edición estableciendo la propiedad `KeepInEditMode` del objeto `GridViewUpdatedEventArgs` pasado en true. Sin embargo, si adopta este enfoque, asegúrese de volver a enlazar los datos a GridView (invocando su método `DataBind()`) para que los valores del otro usuario se carguen en la interfaz de edición. El código disponible para su descarga con este tutorial tiene dos líneas de código en el `RowUpdated` controlador de eventos comentado. simplemente quite la marca de comentario de estas líneas de código para que GridView permanezca en modo de edición después de una infracción de simultaneidad.

## <a name="responding-to-concurrency-violations-when-deleting"></a>Responder a infracciones de simultaneidad al eliminar

Con el patrón de DB Direct, no se produce ninguna excepción en el caso de una infracción de simultaneidad. En su lugar, la instrucción de base de datos simplemente afecta a los registros, ya que la cláusula WHERE no coincide con ningún registro. Todos los métodos de modificación de datos creados en el BLL se han diseñado de forma que devuelvan un valor booleano que indique si afectan exactamente a un registro. Por lo tanto, para determinar si se produjo una infracción de simultaneidad al eliminar un registro, podemos examinar el valor devuelto del método `DeleteProduct` de la capa BLL.

El valor devuelto para un método BLL se puede examinar en los controladores de eventos de nivel posterior de ObjectDataSource a través de la propiedad `ReturnValue` del `ObjectDataSourceStatusEventArgs` objeto pasado al controlador de eventos. Como estamos interesados en determinar el valor devuelto del método `DeleteProduct`, es necesario crear un controlador de eventos para el evento de `Deleted` de ObjectDataSource. La propiedad `ReturnValue` es de tipo `object` y se puede `null` si se ha producido una excepción y se ha interrumpido el método antes de que pueda devolver un valor. Por lo tanto, primero debemos asegurarnos de que la propiedad `ReturnValue` no sea `null` y sea un valor booleano. Suponiendo que esta comprobación se supere, mostramos el control etiqueta `DeleteConflictMessage` si el `ReturnValue` es `false`. Esto puede realizarse mediante el código siguiente:

[!code-vb[Main](implementing-optimistic-concurrency-vb/samples/sample14.vb)]

En el caso de una infracción de simultaneidad, se cancela la solicitud de eliminación del usuario. Se actualiza GridView, que muestra los cambios que se produjeron para ese registro entre el momento en que el usuario cargó la página y cuando hizo clic en el botón Eliminar. Cuando se produzca una infracción de este tipo, se muestra la etiqueta de `DeleteConflictMessage`, en la que se explica lo que sucede (vea la figura 20).

[![una eliminación de usuario se cancela en el caso de una infracción de simultaneidad](implementing-optimistic-concurrency-vb/_static/image57.png)](implementing-optimistic-concurrency-vb/_static/image56.png)

**Figura 20**: la eliminación de un usuario se cancela en el caso de una infracción de simultaneidad ([haga clic para ver la imagen de tamaño completo](implementing-optimistic-concurrency-vb/_static/image58.png))

## <a name="summary"></a>Resumen

Existen oportunidades de infracciones de simultaneidad en todas las aplicaciones que permiten a varios usuarios simultáneos actualizar o eliminar datos. Si estas infracciones no se tienen en cuenta, cuando dos usuarios actualizan simultáneamente los mismos datos que ganan en la última escritura "gana", se sobrescriben los cambios del otro usuario. Como alternativa, los desarrolladores pueden implementar el control de simultaneidad optimista o pesimista. El control de simultaneidad optimista supone que las infracciones de simultaneidad son poco frecuentes y simplemente no permiten un comando UPDATE o DELETE que constituya una infracción de simultaneidad. El control de simultaneidad pesimista supone que las infracciones de simultaneidad son frecuentes y simplemente rechazar el comando UPDATE o DELETE de un usuario no es aceptable. Con el control de simultaneidad pesimista, la actualización de un registro implica su bloqueo, lo que impide que otros usuarios modifiquen o eliminen el registro mientras está bloqueado.

El DataSet con tipo en .NET proporciona funcionalidad para admitir el control de simultaneidad optimista. En concreto, las instrucciones `UPDATE` y `DELETE` que se emiten a la base de datos incluyen todas las columnas de la tabla, lo que garantiza que la actualización o eliminación solo se producirá si los datos actuales del registro coinciden con los datos originales que tenía el usuario al realizar la actualización o la eliminación. Una vez configurada la capa DAL para admitir la simultaneidad optimista, los métodos de BLL deben actualizarse. Además, la página ASP.NET que llama a la capa BLL debe estar configurada de modo que ObjectDataSource recupere los valores originales de su control Web de datos y los pase a la capa BLL.

Como hemos visto en este tutorial, la implementación del control de simultaneidad optimista en una aplicación Web ASP.NET implica la actualización de la capa DAL y BLL y la adición de compatibilidad en la página ASP.NET. Si este trabajo agregado es una inversión razonable de su tiempo y esfuerzo depende de su aplicación. Si tiene pocos usuarios simultáneos que actualizan datos, o si los datos que se están actualizando son diferentes entre sí, el control de simultaneidad no es un problema clave. Sin embargo, si habitualmente tiene varios usuarios en el sitio que trabajan con los mismos datos, el control de simultaneidad puede ayudar a evitar que las actualizaciones o eliminaciones de un usuario sobrescriban de forma involuntaria otras.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](customizing-the-data-modification-interface-vb.md)
> [Siguiente](adding-client-side-confirmation-when-deleting-vb.md)
