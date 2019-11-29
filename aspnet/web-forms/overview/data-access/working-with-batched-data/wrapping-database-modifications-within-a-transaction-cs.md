---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: Ajustar las modificaciones de base de datosC#dentro de una transacción () | Microsoft Docs
author: rick-anderson
description: Este tutorial es el primero de cuatro que examina la actualización, eliminación e inserción de lotes de datos. En este tutorial se explica cómo permiten las transacciones de base de datos...
ms.author: riande
ms.date: 06/26/2007
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: da69e466a5b506b869dc8fc0624f3e6a479199a8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74624694"
---
# <a name="wrapping-database-modifications-within-a-transaction-c"></a>Ajustar las modificaciones de base de datos dentro de una transacción (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip) o [Descargar PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> Este tutorial es el primero de cuatro que examina la actualización, eliminación e inserción de lotes de datos. En este tutorial, aprenderá cómo las transacciones de base de datos permiten realizar modificaciones de lotes como una operación atómica, lo que garantiza que todos los pasos se realizan correctamente o que se produce un error en todos los pasos.

## <a name="introduction"></a>Introducción

Como vimos a partir de la [información general sobre la inserción, actualización y eliminación de tutoriales de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md) , GridView proporciona compatibilidad integrada para la edición y eliminación de filas. Con unos pocos clics del mouse, es posible crear una interfaz de modificación de datos enriquecida sin escribir una línea de código, siempre y cuando el contenido se modifique y se elimine por fila. Sin embargo, en ciertos escenarios esto no es suficiente y es necesario proporcionar a los usuarios la capacidad de editar o eliminar un lote de registros.

Por ejemplo, la mayoría de los clientes de correo electrónico basados en web usan una cuadrícula para mostrar cada mensaje en el que cada fila incluye una casilla junto con la información de correo electrónico (asunto, remitente, etc.). Esta interfaz permite al usuario eliminar varios mensajes mediante su comprobación y, a continuación, hacer clic en el botón Eliminar mensajes seleccionados. Una interfaz de edición de lotes es ideal en situaciones en las que los usuarios suelen editar muchos registros diferentes. En lugar de forzar al usuario a hacer clic en editar, realizar el cambio y, a continuación, hacer clic en actualizar para cada registro que deba modificarse, una interfaz de edición por lotes representa cada fila con su interfaz de edición. El usuario puede modificar rápidamente el conjunto de filas que se deben cambiar y, a continuación, guardar estos cambios haciendo clic en el botón Actualizar todo. En este conjunto de tutoriales, examinaremos cómo crear interfaces para insertar, editar y eliminar lotes de datos.

Al realizar operaciones por lotes, es importante determinar si es posible que algunas de las operaciones del lote se realicen correctamente mientras se producen errores en otros. Considere la posibilidad de usar una interfaz de eliminación de lotes: ¿Qué ocurrirá si el primer registro seleccionado se elimina correctamente, pero se produce un error en el segundo, por ejemplo, debido a una infracción de restricción de clave externa? ¿Se debe revertir la eliminación del primer registro o es aceptable que el primer registro permanezca eliminado?

Si desea que la operación por lotes se trate como una [operación atómica](http://en.wikipedia.org/wiki/Atomic_operation), una en la que todos los pasos se realicen correctamente o se produzca un error en todos los pasos, la capa de acceso a datos debe aumentarse para incluir la compatibilidad con [las transacciones de base](http://en.wikipedia.org/wiki/Database_transaction)de datos. Las transacciones de base de datos garantizan la atomicidad para el conjunto de instrucciones `INSERT`, `UPDATE`y `DELETE` que se ejecutan bajo el paraguas de la transacción y son una característica admitida por la mayoría de los sistemas de bases de datos modernos.

En este tutorial veremos cómo extender la capa DAL para usar transacciones de base de datos. En los tutoriales siguientes se examinará la implementación de páginas web para las interfaces de inserción, actualización y eliminación de lotes. Vamos a empezar a trabajar.

> [!NOTE]
> Cuando se modifican datos en una transacción por lotes, no siempre se necesita atomicidad. En algunos escenarios, puede ser aceptable tener algunas modificaciones de datos correctas y otras en el mismo lote producen un error, como cuando se elimina un conjunto de correos electrónicos de un cliente de correo electrónico basado en Web. Si se produce un error de base de datos a la mitad del proceso de eliminación, es probable que no se eliminen los registros procesados sin errores. En tales casos, no es necesario modificar la capa DAL para admitir transacciones de base de datos. Sin embargo, hay otros escenarios de operaciones por lotes en los que la atomicidad es fundamental. Cuando un cliente mueve sus fondos de una cuenta bancaria a otra, se deben realizar dos operaciones: los fondos se deben deducir de la primera cuenta y agregarse después al segundo. Aunque es posible que el Banco no tenga en cuenta que el primer paso se realiza correctamente, pero se produce un error en el segundo paso, los clientes podrían interferir. Le recomiendo que siga este tutorial e implemente las mejoras en la capa DAL para admitir transacciones de base de datos, incluso si no tiene previsto usarlas en las interfaces de inserción, actualización y eliminación de Batch que se van a crear en los tres tutoriales siguientes.

## <a name="an-overview-of-transactions"></a>Información general sobre transacciones

La mayoría de las bases de datos son compatibles con *las transacciones*, que permiten agrupar varios comandos de base de datos en una única unidad lógica de trabajo. Se garantiza que los comandos de base de datos que componen una transacción son atómicos, lo que significa que se producirá un error en todos los comandos o todos se realizarán correctamente.

En general, las transacciones se implementan a través de instrucciones SQL con el siguiente patrón:

1. Indica el inicio de una transacción.
2. Ejecutar las instrucciones SQL que componen la transacción.
3. Si hay un error en una de las instrucciones del paso 2, revierta la transacción.
4. Si todas las instrucciones del paso 2 se completan sin errores, confirme la transacción.

Las instrucciones SQL utilizadas para crear, confirmar y revertir la transacción se pueden escribir manualmente al escribir scripts SQL o crear procedimientos almacenados, o mediante programación, mediante ADO.NET o las clases del espacio de [nombres`System.Transactions`](https://msdn.microsoft.com/library/system.transactions.aspx). En este tutorial solo se examinará la administración de transacciones mediante ADO.NET. En un futuro tutorial, veremos cómo usar los procedimientos almacenados en el nivel de acceso a datos, en cuyo momento exploraremos las instrucciones SQL para crear, revertir y confirmar transacciones. Mientras tanto, consulte [Administración de transacciones en SQL Server procedimientos almacenados](http://www.4guysfromrolla.com/webtech/080305-1.shtml) para obtener más información.

> [!NOTE]
> La [clase`TransactionScope`](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx) del espacio de nombres `System.Transactions` permite a los programadores encapsular mediante programación una serie de instrucciones en el ámbito de una transacción e incluye compatibilidad con transacciones complejas que implican varios orígenes, como dos bases de datos diferentes o incluso tipos heterogéneos de almacenes de datos, como una base de datos de Microsoft SQL Server, una base de datos de Oracle y un servicio Web. He decidido usar las transacciones de ADO.NET para este tutorial en lugar de la clase `TransactionScope` porque ADO.NET es más específico para las transacciones de base de datos y, en muchos casos, es mucho menos intensivo de recursos. Además, en ciertos escenarios, la clase `TransactionScope` usa el Coordinador de transacciones distribuidas de Microsoft (MSDTC). Los problemas de configuración, implementación y rendimiento relacionados con MSDTC lo convierten en un tema bastante especializado y avanzado y más allá del ámbito de estos tutoriales.

Cuando se trabaja con el proveedor SqlClient en ADO.NET, las transacciones se inician a través de una llamada al método [`SqlConnection` Class](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) s [`BeginTransaction`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx), que devuelve un [objeto`SqlTransaction`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx). Las instrucciones de modificación de datos que estructuran la transacción se colocan dentro de un bloque `try...catch`. Si se produce un error en una instrucción del bloque `try`, la ejecución se transfiere al bloque `catch` en el que se puede revertir la transacción a través del método `SqlTransaction` Object s [`Rollback`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx). Si todas las instrucciones se completan correctamente, una llamada al método `SqlTransaction` Object s [`Commit`](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx) al final del bloque `try` confirma la transacción. En el fragmento de código siguiente se muestra este patrón. Consulte mantenimiento de la coherencia de la [base de datos con transacciones](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx) para obtener más sintaxis y ejemplos del uso de transacciones con ADO.net.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

De forma predeterminada, los TableAdapters de un conjunto de un DataSet con tipo no usan transacciones. Para proporcionar compatibilidad con las transacciones, necesitamos aumentar las clases de TableAdapter para incluir métodos adicionales que usan el patrón anterior para realizar una serie de instrucciones de modificación de datos dentro del ámbito de una transacción. En el paso 2 veremos cómo usar las clases parciales para agregar estos métodos.

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>Paso 1: crear páginas web de trabajo con datos por lotes

Antes de empezar a explorar cómo aumentar la capa DAL para admitir las transacciones de base de datos, deje que primero dedique un momento a crear las páginas web de ASP.NET que necesitamos para este tutorial y las tres siguientes. Comience agregando una nueva carpeta denominada `BatchData` y, a continuación, agregue las siguientes páginas de ASP.NET, asociando cada página con la página maestra de `Site.master`.

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`

![Agregue las páginas ASP.NET para los tutoriales relacionados con SqlDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**Figura 1**: agregar las páginas ASP.net para los tutoriales relacionados con SqlDataSource

Al igual que con las demás carpetas, `Default.aspx` utilizará el control de usuario `SectionLevelTutorialListing.ascx` para enumerar los tutoriales de la sección. Por lo tanto, agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta la página s Vista de diseño.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**Figura 2**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))

Por último, agregue estas cuatro páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después de la personalización del mapa del sitio `<siteMapNode>`:

[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para trabajar con tutoriales de datos por lotes.

![El mapa del sitio ahora incluye entradas para los tutoriales sobre cómo trabajar con datos por lotes](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**Figura 3**: ahora, el mapa del sitio incluye entradas para los tutoriales sobre cómo trabajar con datos por lotes

## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>Paso 2: actualización de la capa de acceso a datos para admitir transacciones de base de datos

Como hemos comentado en el primer tutorial, la [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md), el conjunto de datos con tipo de la capa Dal se compone de objetos DataTable y TableAdapters. Los objetos DataTable contienen datos mientras que los TableAdapters proporcionan la funcionalidad para leer los datos de la base de datos en las DataTables, actualizar la base de datos con los cambios realizados en los objetos DataTable, etc. Recuerde que los TableAdapters proporcionan dos patrones para actualizar los datos, a los que se hace referencia como actualización por lotes y DB-Direct. Con el patrón de actualización por lotes, el TableAdapter pasa un DataSet, DataTable o colección de DataRows. Estos datos se enumeran y, para cada fila insertada, modificada o eliminada, se ejecutan los `InsertCommand`, `UpdateCommand`o `DeleteCommand`. Con el patrón DB-Direct, el TableAdapter pasa en su lugar los valores de las columnas necesarias para insertar, actualizar o eliminar un solo registro. A continuación, el método de modelo directo DB utiliza esos valores pasados para ejecutar la instrucción `InsertCommand`, `UpdateCommand`o `DeleteCommand` adecuada.

Independientemente del patrón de actualización utilizado, los métodos generados automáticamente por TableAdapters no usan transacciones. De forma predeterminada, las operaciones de inserción, actualización o eliminación realizadas por el TableAdapter se tratan como una única operación discreta. Por ejemplo, Imagine que el modelo de DB-Direct es utilizado por algún código de la capa BLL para insertar diez registros en la base de datos. Este código llamaría al método `Insert` TableAdapter s diez veces. Si las primeras cinco inserciones se realizan correctamente, pero la sexta produjo una excepción, los cinco primeros registros insertados permanecerían en la base de datos. Del mismo modo, si se usa el patrón de actualización por lotes para realizar inserciones, actualizaciones y eliminaciones en las filas insertadas, modificadas y eliminadas en un objeto DataTable, si las primeras modificaciones se han realizado correctamente pero una posterior ha encontrado un error, esas modificaciones anteriores Completed permanece en la base de datos.

En ciertos escenarios queremos asegurar la atomicidad en una serie de modificaciones. Para lograr esto, debemos extender manualmente el TableAdapter agregando nuevos métodos que ejecuten los `InsertCommand`, `UpdateCommand`y `DeleteCommand` s bajo el Paragua de una transacción. Al [crear una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) , analizamos el uso de [clases parciales](http://en.wikipedia.org/wiki/Partial_type) para extender la funcionalidad de las tablas de datos en el conjunto de datos con tipo. Esta técnica también se puede utilizar con TableAdapters.

El `Northwind.xsd` del conjunto de elementos con tipo se encuentra en la subcarpeta `App_Code` de `DAL` carpeta. Cree una subcarpeta en la carpeta `DAL` denominada `TransactionSupport` y agregue un nuevo archivo de clase denominado `ProductsTableAdapter.TransactionSupport.cs` (consulte la figura 4). Este archivo contendrá la implementación parcial de la `ProductsTableAdapter` que incluye métodos para realizar modificaciones de datos mediante una transacción.

![Agregue una carpeta denominada TransactionSupport y un archivo de clase denominado ProductsTableAdapter.TransactionSupport.cs](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**Figura 4**: agregue una carpeta denominada `TransactionSupport` y un archivo de clase denominado `ProductsTableAdapter.TransactionSupport.cs`

Escriba el siguiente código en el archivo de `ProductsTableAdapter.TransactionSupport.cs`:

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

La palabra clave `partial` en la declaración de clase aquí indica al compilador que los miembros agregados en se van a agregar a la clase `ProductsTableAdapter` en el espacio de nombres `NorthwindTableAdapters`. Observe la instrucción `using System.Data.SqlClient` en la parte superior del archivo. Puesto que el TableAdapter se configuró para usar el proveedor SqlClient, utiliza internamente un objeto [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx) para emitir sus comandos a la base de datos. Por lo tanto, es necesario usar la clase `SqlTransaction` para iniciar la transacción y, a continuación, confirmarla o revertirla. Si usa un almacén de datos distinto de Microsoft SQL Server, deberá usar el proveedor adecuado.

Estos métodos proporcionan los bloques de creación necesarios para iniciar, revertir y confirmar una transacción. Se marcan `public`, lo que les permite usarse desde el `ProductsTableAdapter`, desde otra clase de la capa DAL o desde otra capa de la arquitectura, como la capa BLL. `BeginTransaction` abre el `SqlConnection` interno de TableAdapter (si es necesario), comienza la transacción y la asigna a la propiedad `Transaction` y adjunta la transacción a los objetos de `SqlCommand` de `SqlDataAdapter` s internos. `CommitTransaction` y `RollbackTransaction` llamar a los métodos `Commit` y `Rollback` del objeto `Transaction`, respectivamente, antes de cerrar el objeto `Connection` interno.

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>Paso 3: agregar métodos para actualizar y eliminar datos bajo el paraguas de una transacción

Una vez completados estos métodos, se ha preparado para agregar métodos a `ProductsDataTable` o el BLL que realiza una serie de comandos bajo el Paragua de una transacción. El método siguiente utiliza el patrón de actualización por lotes para actualizar una instancia de `ProductsDataTable` mediante una transacción. Inicia una transacción llamando al método `BeginTransaction` y, a continuación, usa un bloque `try...catch` para emitir las instrucciones de modificación de datos. Si la llamada al método `Adapter` Object s `Update` produce una excepción, la ejecución se transferirá al bloque `catch` donde se revertirá la transacción y se volverá a producir la excepción. Recuerde que el método `Update` implementa el patrón de actualización por lotes enumerando las filas del `ProductsDataTable` proporcionado y realizando los `InsertCommand`, `UpdateCommand`y `DeleteCommand` necesarios. Si alguno de estos comandos produce un error, se revierte la transacción y se deshacen las modificaciones anteriores realizadas durante la duración de la transacción. Si la instrucción de `Update` se completa sin errores, la transacción se confirma en su totalidad.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

Agregue el método `UpdateWithTransaction` a la clase `ProductsTableAdapter` a través de la clase parcial en `ProductsTableAdapter.TransactionSupport.cs`. También puede Agregar este método a la clase de `ProductsBLL` de la capa de lógica de negocios con unos pocos cambios sintácticos. Es decir, la palabra clave this en `this.BeginTransaction()`, `this.CommitTransaction()`y `this.RollbackTransaction()` tendría que reemplazarse por `Adapter` (Recuerde que `Adapter` es el nombre de una propiedad en `ProductsBLL` de tipo `ProductsTableAdapter`).

El método `UpdateWithTransaction` usa el patrón de actualización por lotes, pero también se puede usar una serie de llamadas de DB-Direct en el ámbito de una transacción, como se muestra en el siguiente método. El método `DeleteProductsWithTransaction` acepta como entrada una `List<T>` de tipo `int`, que son las `ProductID` s que se van a eliminar. El método inicia la transacción a través de una llamada a `BeginTransaction` y, a continuación, en el bloque de `try`, recorre en iteración la lista proporcionada que llama al método `Delete` de DB-Direct para cada valor de `ProductID`. Si se produce un error en alguna de las llamadas a `Delete`, el control se transfiere al bloque de `catch` en el que se revierte la transacción y se vuelve a producir la excepción. Si todas las llamadas a `Delete` se realizan correctamente, se confirma la transacción. Agregue este método a la clase `ProductsBLL`.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>Aplicar transacciones en varios TableAdapters

El código relacionado con la transacción examinado en este tutorial permite que varias instrucciones del `ProductsTableAdapter` se traten como una operación atómica. Pero ¿qué ocurre si hay varias modificaciones en las tablas de bases de datos diferentes que deben realizarse de forma atómica? Por ejemplo, al eliminar una categoría, es posible que primero quiera reasignar sus productos actuales a alguna otra categoría. Estos dos pasos que reasignan los productos y eliminan la categoría deben ejecutarse como una operación atómica. Pero el `ProductsTableAdapter` solo incluye métodos para modificar la tabla `Products` y el `CategoriesTableAdapter` solo incluye métodos para modificar la tabla `Categories`. Entonces, ¿cómo puede una transacción abarcar ambos TableAdapters?

Una opción consiste en agregar un método al `CategoriesTableAdapter` denominado `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)` y hacer que ese método llame a un procedimiento almacenado que reasigne los productos y elimine la categoría dentro del ámbito de una transacción definida en el procedimiento almacenado. Veremos cómo iniciar, confirmar y revertir las transacciones en procedimientos almacenados en un tutorial futuro.

Otra opción consiste en crear una clase auxiliar en la capa DAL que contenga el método `DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`. Este método crearía una instancia de la `CategoriesTableAdapter` y el `ProductsTableAdapter` y, a continuación, establecería estos dos TableAdapters `Connection` propiedades en la misma instancia de `SqlConnection`. En ese momento, cualquiera de los dos TableAdapters iniciaría la transacción con una llamada a `BeginTransaction`. Los métodos de TableAdapter para reasignar los productos y eliminar la categoría se invocarían en un bloque `try...catch` con la transacción confirmada o revertida según sea necesario.

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>Paso 4: agregar el método`UpdateWithTransaction`a la capa de lógica de negocios

En el paso 3 se ha agregado un método `UpdateWithTransaction` a la `ProductsTableAdapter` de la capa DAL. Debemos agregar un método correspondiente a la capa BLL. Aunque el nivel de presentación podría llamar directamente a la capa DAL para invocar el método de `UpdateWithTransaction`, estos tutoriales han luchado por definir una arquitectura en capas que aísla la capa DAL del nivel de presentación. Por lo tanto, nos importa para continuar este enfoque.

Abra el archivo de clase `ProductsBLL` y agregue un método denominado `UpdateWithTransaction` que simplemente llama al método DAL correspondiente. Ahora debería haber dos nuevos métodos en `ProductsBLL`: `UpdateWithTransaction`, que acaba de agregar y `DeleteProductsWithTransaction`, que se agregó en el paso 3.

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> Estos métodos no incluyen el atributo `DataObjectMethodAttribute` asignado a la mayoría de los demás métodos de la clase `ProductsBLL` porque vamos a invocar estos métodos directamente desde las clases de código subyacente de páginas de ASP.NET. Recuerde que `DataObjectMethodAttribute` se usa para marcar qué métodos deben aparecer en el Asistente para configurar origen de datos de ObjectDataSource s y en la pestaña (SELECT, UPDATE, INSERT o DELETE). Dado que GridView carece de compatibilidad integrada para la edición o eliminación de lotes, tendremos que invocar estos métodos mediante programación en lugar de usar el enfoque declarativo sin código.

## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>Paso 5: actualizar atómicamente los datos de la base de datos desde el nivel de presentación

Para ilustrar el efecto que tiene la transacción al actualizar un lote de registros, cree una interfaz de usuario que haga una lista de todos los productos de GridView e incluya un control Web de botón que, cuando se hace clic en él, reasigna los productos `CategoryID` valores. En concreto, la reasignación de categoría progresará de modo que se asigne a los primeros productos un valor de `CategoryID` válido, mientras que a otros se les asigna un valor `CategoryID` que no existe. Si intentamos actualizar la base de datos con un producto cuyo `CategoryID` no coincide con una categoría existente `CategoryID`, se producirá una infracción de la restricción de clave externa y se producirá una excepción. Lo que veremos en este ejemplo es que, cuando se usa una transacción, la excepción generada por la infracción de la restricción de clave externa hará que se reviertan los cambios de `CategoryID` válidos anteriores. Sin embargo, cuando no se utiliza una transacción, las modificaciones de las categorías iniciales permanecerán.

Comience abriendo la página `Transactions.aspx` en la carpeta `BatchData` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador. Establezca su `ID` en `Products` y, desde su etiqueta inteligente, enlácelo a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configure ObjectDataSource para que extraiga sus datos del método `ProductsBLL` Class s `GetProducts`. Este será un control GridView de solo lectura, así que establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) y haga clic en finalizar.

[![Figura 5: configuración de ObjectDataSource para usar el método ProductsBLL de la clase GetProducts](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**Figura 5**: Figura 5: configuración de ObjectDataSource para usar el método `ProductsBLL` Class s `GetProducts` ([haga clic para ver la imagen de tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))

[![establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna)](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**Figura 6**: establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y CheckBoxField para los campos de datos del producto. Quite todos estos campos, excepto `ProductID`, `ProductName`, `CategoryID`y `CategoryName`, y cambie el nombre de las propiedades `ProductName` y `CategoryName` BoundFields `HeaderText` a product y Category, respectivamente. En la etiqueta inteligente, active la opción Habilitar paginación. Después de realizar estas modificaciones, el marcado declarativo GridView y ObjectDataSource s debe ser similar al siguiente:

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

A continuación, agregue tres controles Web Button por encima de GridView. Establezca la propiedad de texto del primer botón en actualizar cuadrícula, la segunda para modificar las categorías (con transacción) y la tercera para modificar las categorías (sin transacción).

[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

En este momento, el Vista de diseño en Visual Studio debe ser similar a la captura de pantalla que se muestra en la figura 7.

[![la página contiene un control GridView y tres controles Web Button](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**Figura 7**: la página contiene un control GridView y tres controles Web Button ([haga clic para ver la imagen de tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))

Cree controladores de eventos para cada uno de los tres eventos Button `Click` y use el código siguiente:

[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

El controlador de eventos del botón actualizar s `Click` simplemente vuelve a enlazar los datos a GridView llamando al método `Products` GridView s `DataBind`.

El segundo controlador de eventos reasigna los productos `CategoryID` s y usa el nuevo método de transacción de la capa BLL para realizar las actualizaciones de la base de datos bajo el Paragua de una transacción. Tenga en cuenta que cada producto s `CategoryID` se establece arbitrariamente en el mismo valor que su `ProductID`. Esto funcionará bien para los primeros productos, ya que estos productos tienen `ProductID` valores que se asignan a `CategoryID` s válidos. Pero una vez que el `ProductID` s comienza a obtener un tamaño demasiado grande, esta superposición una coincidencia de `ProductID` s y `CategoryID` s ya no se aplica.

El tercer controlador de eventos `Click` actualiza los productos `CategoryID` s de la misma manera, pero envía la actualización a la base de datos mediante el método de `Update` predeterminado `ProductsTableAdapter` s. Este método `Update` no encapsula la serie de comandos dentro de una transacción, por lo que los cambios realizados antes del primer error de infracción de la restricción de clave externa encontrada se conservarán.

Para mostrar este comportamiento, visite esta página a través de un explorador. Inicialmente debería ver la primera página de datos, tal como se muestra en la figura 8. A continuación, haga clic en el botón modificar categorías (con transacción). Esto producirá un postback e intentará actualizar todos los productos `CategoryID` valores, pero producirá una infracción de la restricción de clave externa (consulte la figura 9).

[![los productos se muestran en un control GridView paginable](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**Figura 8**: los productos se muestran en un control GridView paginable ([haga clic para ver la imagen de tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))

[![reasignar las categorías da como resultado una infracción de restricción de clave externa](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**Figura 9**: reasignación de las categorías da como resultado una infracción de restricción de clave externa ([haga clic para ver la imagen de tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))

Ahora, presione el botón atrás del explorador y, a continuación, haga clic en el botón actualizar cuadrícula. Tras actualizar los datos, debería ver el mismo resultado que se muestra en la figura 8. Es decir, aunque algunos de los productos `CategoryID` s se cambiaron a valores legales y se actualizaron en la base de datos, se revirtieron cuando se produjo la infracción de la restricción de clave externa.

Ahora, intente hacer clic en el botón modificar categorías (sin transacción). Esto producirá el mismo error de infracción de la restricción de clave externa (vea la ilustración 9), pero esta vez no se revertirán los productos cuyos valores de `CategoryID` se cambiaron a un valor válido. Presione el botón atrás del explorador y, después, el botón actualizar cuadrícula. Como se muestra en la figura 10, se han reasignado los `CategoryID` s de los ocho primeros productos. Por ejemplo, en la figura 8, Chang tenía una `CategoryID` de 1, pero en la figura 10 se ha reasignado a 2.

[![algunos productos los valores CategoryID se actualizaron mientras que otros no](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**Figura 10**: algunos productos `CategoryID` valores se actualizaron mientras que otros no estaban ([haga clic para ver la imagen de tamaño completo](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))

## <a name="summary"></a>Resumen

De forma predeterminada, los métodos de TableAdapter s no encapsulan las instrucciones de base de datos ejecutadas dentro del ámbito de una transacción, pero con un poco de trabajo podemos agregar métodos que crearán, confirmarán y revierten una transacción. En este tutorial, hemos creado tres métodos de este tipo en la clase `ProductsTableAdapter`: `BeginTransaction`, `CommitTransaction`y `RollbackTransaction`. Vimos cómo usar estos métodos junto con un bloque `try...catch` para realizar una serie de instrucciones de modificación de datos atómicas. En concreto, creamos el método `UpdateWithTransaction` en el `ProductsTableAdapter`, que usa el patrón de actualización por lotes para realizar las modificaciones necesarias en las filas de un `ProductsDataTable`proporcionado. También hemos agregado el método `DeleteProductsWithTransaction` a la clase `ProductsBLL` de la capa BLL, que acepta un `List` de `ProductID` valores como entrada y llama al método de patrón DB-Direct `Delete` para cada `ProductID`. Ambos métodos comienzan por crear una transacción y, después, ejecutar las instrucciones de modificación de datos dentro de un bloque `try...catch`. Si se produce una excepción, la transacción se revierte; de lo contrario, se confirma.

En el paso 5 se ilustra el efecto de las actualizaciones de lotes transaccionales frente a las actualizaciones por lotes que no han podido usar una transacción. En los tres tutoriales siguientes se basará en la base que se proporciona en este tutorial y creará interfaces de usuario para realizar actualizaciones, eliminaciones e inserciones por lotes.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Mantener la coherencia de la base de datos con transacciones](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [Administrar transacciones en SQL Server procedimientos almacenados](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [Transacciones simplificadas: `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope y DataAdapters](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [Usar transacciones de Oracle Database en .NET](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Dave Gardner, Hilton Giesenow y Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](batch-updating-cs.md)
