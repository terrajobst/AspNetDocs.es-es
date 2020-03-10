---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Usar procedimientos almacenados existentes para los TableAdapters del conjunto de de tipos (VB) | Microsoft Docs
author: rick-anderson
description: En el tutorial anterior se ha aprendido a usar el Asistente para TableAdapter para generar nuevos procedimientos almacenados. En este tutorial, aprenderá cómo el mismo TableAdapter...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 2da25f6a-757e-4e7b-a812-1575288d8f7a
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: e35c3d6a98516a07f6119e6cb9dbeb99bc28fe33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78427129"
---
# <a name="using-existing-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Usar procedimientos almacenados existentes para los TableAdapters del conjunto de datos con tipo (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_68_VB.zip) o [Descargar PDF](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial68vb1.pdf)

> En el tutorial anterior se ha aprendido a usar el Asistente para TableAdapter para generar nuevos procedimientos almacenados. En este tutorial, aprenderá cómo el mismo asistente de TableAdapter puede trabajar con los procedimientos almacenados existentes. También aprenderá a agregar manualmente nuevos procedimientos almacenados a nuestra base de datos.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , vimos cómo los TableAdapters del conjunto de datos con tipo pueden configurarse para usar procedimientos almacenados con el fin de obtener acceso a los datos en lugar de a las instrucciones SQL ad hoc. En concreto, hemos examinado cómo el Asistente para TableAdapter crea automáticamente estos procedimientos almacenados. Al migrar una aplicación heredada a ASP.NET 2,0 o al compilar un sitio web de ASP.NET 2,0 en torno a un modelo de datos existente, lo más probable es que la base de datos ya contenga los procedimientos almacenados que necesitamos. Como alternativa, puede que prefiera crear los procedimientos almacenados de forma manual o a través de alguna herramienta distinta del Asistente para TableAdapter que genera automáticamente los procedimientos almacenados.

En este tutorial, veremos cómo configurar el TableAdapter para usar los procedimientos almacenados existentes. Dado que la base de datos Northwind solo tiene un pequeño conjunto de procedimientos almacenados integrados, también veremos los pasos necesarios para agregar manualmente nuevos procedimientos almacenados a la base de datos a través del entorno de Visual Studio. Vamos a empezar a trabajar.

> [!NOTE]
> En el tutorial de ajuste de las [modificaciones de base de datos dentro de una transacción](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) se han agregado métodos a TableAdapter para admitir transacciones (`BeginTransaction`, `CommitTransaction`, etc.). Como alternativa, las transacciones se pueden administrar completamente dentro de un procedimiento almacenado, que no requiere ninguna modificación en el código del nivel de acceso a los datos. En este tutorial exploraremos los comandos de T-SQL que se usan para ejecutar instrucciones de procedimientos almacenados en el ámbito de una transacción.

## <a name="step-1-adding-stored-procedures-to-the-northwind-database"></a>Paso 1: agregar procedimientos almacenados a la base de datos Northwind

Visual Studio facilita la tarea de agregar nuevos procedimientos almacenados a una base de datos. Permite agregar un nuevo procedimiento almacenado a la base de datos Northwind que devuelve todas las columnas de la tabla `Products` para las que tienen un valor `CategoryID` determinado. En la ventana Explorador de servidores, expanda la base de datos Northwind para que se muestren sus carpetas, diagramas de base de datos, tablas, vistas, etc. Como vimos en el tutorial anterior, la carpeta procedimientos almacenados contiene los procedimientos almacenados existentes de la base de datos. Para agregar un nuevo procedimiento almacenado, simplemente haga clic con el botón secundario en la carpeta procedimientos almacenados y elija la opción Agregar nuevo procedimiento almacenado en el menú contextual.

[![haga clic con el botón secundario en la carpeta procedimientos almacenados y agregue un nuevo procedimiento almacenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: haga clic con el botón secundario en la carpeta procedimientos almacenados y agregue un nuevo procedimiento almacenado ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png))

Como se muestra en la figura 1, al seleccionar la opción Agregar nuevo procedimiento almacenado, se abre una ventana de script en Visual Studio con el esquema de la secuencia de comandos SQL necesaria para crear el procedimiento almacenado. Es nuestro trabajo extraer este script y ejecutarlo, momento en el que el procedimiento almacenado se agregará a la base de datos.

Escriba el siguiente script:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Este script, cuando se ejecute, agregará un nuevo procedimiento almacenado a la base de datos Northwind denominada `Products_SelectByCategoryID`. Este procedimiento almacenado acepta un solo parámetro de entrada (`@CategoryID`, de tipo `int`) y devuelve todos los campos de los productos con un valor `CategoryID` coincidente.

Para ejecutar este script de `CREATE PROCEDURE` y agregar el procedimiento almacenado a la base de datos, haga clic en el icono guardar en la barra de herramientas o presione Ctrl + S. Después de hacerlo, se actualiza la carpeta procedimientos almacenados, mostrando el procedimiento almacenado recién creado. Además, el script de la ventana cambiará el detalle de `CREATE PROCEDURE dbo.Products_SelectProductByCategoryID` a `ALTER PROCEDURE` `dbo.Products_SelectProductByCategoryID`. `CREATE PROCEDURE` agrega un nuevo procedimiento almacenado a la base de datos, mientras `ALTER PROCEDURE` actualiza uno existente. Puesto que el inicio del script ha cambiado a `ALTER PROCEDURE`, al cambiar los parámetros de entrada o las instrucciones SQL de los procedimientos almacenados y al hacer clic en el icono de guardar se actualizará el procedimiento almacenado con estos cambios.

En la ilustración 2 se muestra Visual Studio una vez guardado el `Products_SelectByCategoryID` procedimiento almacenado.

[![el procedimiento almacenado Products_SelectByCategoryID se ha agregado a la base de datos](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png)

**Figura 2**: se ha agregado el procedimiento almacenado `Products_SelectByCategoryID` a la base de datos ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png))

## <a name="step-2-configuring-the-tableadapter-to-use-an-existing-stored-procedure"></a>Paso 2: configurar el TableAdapter para usar un procedimiento almacenado existente

Ahora que el `Products_SelectByCategoryID` procedimiento almacenado se ha agregado a la base de datos, podemos configurar nuestra capa de acceso a datos para que use este procedimiento almacenado cuando se invoque uno de sus métodos. En concreto, agregaremos un método `GetProductsByCategoryID(<_i22_>categoryID)<!--_i22_-->` a la `ProductsTableAdapter` en el `NorthwindWithSprocs` conjunto de datos con tipo que llama al procedimiento almacenado `Products_SelectByCategoryID` que acabamos de crear.

Para empezar, abra el conjunto de `NorthwindWithSprocs`. Haga clic con el botón derecho en el `ProductsTableAdapter` y elija Agregar consulta para iniciar el Asistente para configuración de consultas de TableAdapter. En el [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) , hemos optado por que TableAdapter creara un nuevo procedimiento almacenado para nosotros. Sin embargo, para este tutorial, queremos conectar el nuevo método TableAdapter al procedimiento almacenado de `Products_SelectByCategoryID` existente. Por lo tanto, elija la opción usar procedimiento almacenado existente en el primer paso del asistente y, a continuación, haga clic en siguiente.

[![elegir la opción usar procedimiento almacenado existente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)

**Figura 3**: elija la opción usar procedimiento almacenado existente ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png))

En la pantalla siguiente se proporciona una lista desplegable que se rellena con los procedimientos almacenados de la base de datos. Al seleccionar un procedimiento almacenado se enumeran sus parámetros de entrada a la izquierda y los campos de datos devueltos (si existen) a la derecha. Elija el `Products_SelectByCategoryID` procedimiento almacenado de la lista y haga clic en siguiente.

[![seleccionar el Products_SelectByCategoryID procedimiento almacenado](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)

**Figura 4**: selección del `Products_SelectByCategoryID` procedimiento almacenado ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png))

En la pantalla siguiente se nos pregunta qué tipo de datos devuelve el procedimiento almacenado y nuestra respuesta determina el tipo devuelto por el método TableAdapter s. Por ejemplo, si se indica que se devuelven datos tabulares, el método devolverá una `ProductsDataTable` instancia rellenada con los registros devueltos por el procedimiento almacenado. Por el contrario, si se indica que este procedimiento almacenado devuelve un valor único, TableAdapter devolverá un `Object` al que se asigna el valor en la primera columna del primer registro devuelto por el procedimiento almacenado.

Como el `Products_SelectByCategoryID` procedimiento almacenado devuelve todos los productos que pertenecen a una categoría determinada, elija la primera respuesta: datos tabulares y haga clic en siguiente.

[![indican que el procedimiento almacenado devuelve datos tabulares](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)

**Figura 5**: indicar que el procedimiento almacenado devuelve datos tabulares ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png))

Todo lo que queda es indicar qué patrones de método usar seguidos de los nombres de estos métodos. Deje activada las opciones rellenar un DataTable y devolver una tabla de tipos, pero cambie el nombre de los métodos a `FillByCategoryID` y `GetProductsByCategoryID`. A continuación, haga clic en siguiente para revisar un resumen de las tareas que realizará el asistente. Si todo es correcto, haga clic en finalizar.

[![nombre de los métodos FillByCategoryID y GetProductsByCategoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 6**: asignar un nombre a los métodos `FillByCategoryID` y `GetProductsByCategoryID` ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

> [!NOTE]
> Los métodos de TableAdapter que se acaban de crear, `FillByCategoryID` y `GetProductsByCategoryID`, esperan un parámetro de entrada de tipo `Integer`. Este valor de parámetro de entrada se pasa al procedimiento almacenado a través de su parámetro `@CategoryID`. Si modifica los parámetros del procedimiento almacenado de `Products_SelectByCategory`, deberá actualizar también los parámetros de estos métodos de TableAdapter. Tal y como se ha explicado en el [tutorial anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md), esto se puede hacer de dos maneras: mediante la adición o eliminación manual de parámetros de la colección de parámetros o la reejecución del asistente de TableAdapter.

## <a name="step-3-adding-agetproductsbycategoryidcategoryidmethod-to-the-bll"></a>Paso 3: agregar un método`GetProductsByCategoryID(categoryID)`a la capa BLL

Con el método `GetProductsByCategoryID` DAL completo, el siguiente paso es proporcionar acceso a este método en el nivel de lógica de negocios. Abra el archivo de clase `ProductsBLLWithSprocs` y agregue el siguiente método:

[!code-vb[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.vb)]

Este método BLL simplemente devuelve el `ProductsDataTable` devuelto desde el método `ProductsTableAdapter` s `GetProductsByCategoryID`. El atributo `DataObjectMethodAttribute` proporciona metadatos utilizados por el Asistente para configurar orígenes de datos de ObjectDataSource. En concreto, este método aparecerá en la lista desplegable Seleccionar pestaña s.

## <a name="step-4-displaying-products-by-category"></a>Paso 4: visualización de productos por categoría

Para probar el procedimiento almacenado `Products_SelectByCategoryID` recién agregado y los métodos DAL y BLL correspondientes, cree una página ASP.NET que contenga un control DropDownList y un control GridView. DropDownList mostrará todas las categorías en la base de datos, mientras que GridView mostrará los productos que pertenecen a la categoría seleccionada.

> [!NOTE]
> En los tutoriales anteriores, hemos creado interfaces de maestro y detalles con DropDownLists. Para obtener información más detallada sobre cómo implementar este tipo de informe maestro y detalles, consulte el tutorial de [filtrado de maestro y detalles con DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) .

Abra la página `ExistingSprocs.aspx` en la carpeta `AdvancedDAL` y arrastre un DropDownList desde el cuadro de herramientas hasta el diseñador. Establezca la propiedad DropDownList s `ID` en `Categories` y su propiedad `AutoPostBack` en `True`. A continuación, desde su etiqueta inteligente, enlace el DropDownList a un nuevo ObjectDataSource denominado `CategoriesDataSource`. Configure ObjectDataSource para que recupere sus datos del método `CategoriesBLL` Class s `GetCategories`. Establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna).

[![recuperar datos del método GetCategories de la clase CategoriesBLL](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 7**: recuperación de datos del método `CategoriesBLL` Class s `GetCategories` ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png))

[![establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png)

**Figura 8**: establecer las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguna) ([haga clic para ver la imagen a tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png))

Después de completar el Asistente de ObjectDataSource, configure el DropDownList para que muestre el `CategoryName` campo de datos y use el campo `CategoryID` como `Value` para cada `ListItem`.

En este momento, el marcado declarativo de DropDownList y ObjectDataSource es similar al siguiente:

[!code-aspx[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.aspx)]

A continuación, arrastre un control GridView hasta el diseñador, colocándolo debajo de DropDownList. Establezca el `ID` de GridView en `ProductsByCategory` y, desde su etiqueta inteligente, enlácelo a un nuevo ObjectDataSource denominado `ProductsByCategoryDataSource`. Configure el `ProductsByCategoryDataSource` ObjectDataSource para usar la clase `ProductsBLLWithSprocs`, de modo que recupere sus datos mediante el método `GetProductsByCategoryID(categoryID)`. Puesto que este GridView solo se usará para Mostrar datos, establezca las listas desplegables de las pestañas actualizar, insertar y eliminar en (ninguno) y haga clic en siguiente.

[![configurar ObjectDataSource para usar la clase ProductsBLLWithSprocs](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png)

**Figura 9**: configuración de ObjectDataSource para usar la clase `ProductsBLLWithSprocs` ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png))

[![recuperar datos del método GetProductsByCategoryID (categoryID)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)

**Figura 10**: recuperación de datos del método `GetProductsByCategoryID(categoryID)` ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png))

El método elegido en la pestaña seleccionar espera un parámetro, por lo que el paso final del asistente nos pide el origen del parámetro. Establezca la lista desplegable origen del parámetro en control y elija el control `Categories` de la lista desplegable ControlID. Haga clic en Finalizar para completar el asistente.

[![usar las categorías DropDownList como origen del parámetro categoryID](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 11**: Use el `Categories` DropDownList como origen del parámetro `categoryID` ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))

Al completar el Asistente de ObjectDataSource, Visual Studio agregará BoundFields y CheckBoxField para cada uno de los campos de datos del producto. Puede personalizar estos campos como considere oportuno.

Visite la página a través de un explorador. Al visitar la página, se selecciona la categoría bebidas y se muestran los productos correspondientes en la cuadrícula. Al cambiar la lista desplegable a una categoría alternativa, como se muestra en la figura 12, se genera un postback y se recarga la cuadrícula con los productos de la categoría recién seleccionada.

[![se muestran los productos de la categoría de producción](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 12**: se muestran los productos de la categoría de producción ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png))

## <a name="step-5-wrapping-a-stored-procedure-s-statements-within-the-scope-of-a-transaction"></a>Paso 5: encapsulado de instrucciones de procedimiento almacenado en el ámbito de una transacción

En el tutorial de ajuste de las [modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) , analizamos técnicas para realizar una serie de instrucciones de modificación de base de datos dentro del ámbito de una transacción. Recuerde que las modificaciones realizadas bajo el paraguas de una transacción se realizan correctamente o no, lo que garantiza la atomicidad. Las técnicas para el uso de transacciones incluyen:

- Usar las clases en el espacio de nombres `System.Transactions`,
- La capa de acceso a datos usa clases ADO.NET como `SqlTransaction`, y
- Agregar los comandos de transacción de T-SQL directamente en el procedimiento almacenado

En el tutorial *ajustar las modificaciones de base de datos dentro de una transacción* se usan las clases ADO.net en la capa dal. En el resto de este tutorial se examina cómo administrar una transacción mediante comandos de T-SQL desde un procedimiento almacenado.

Los tres comandos de SQL clave para iniciar, confirmar y revertir una transacción manualmente son `BEGIN TRANSACTION`, `COMMIT TRANSACTION`y `ROLLBACK TRANSACTION`, respectivamente. Al igual que con el enfoque ADO.NET, cuando se usan transacciones desde dentro de un procedimiento almacenado, es necesario aplicar el patrón siguiente:

1. Indica el inicio de una transacción.
2. Ejecutar las instrucciones SQL que componen la transacción.
3. Si hay un error en cualquiera de las instrucciones del paso 2, revierta la transacción.
4. Si todas las instrucciones del paso 2 se completan sin errores, confirme la transacción.

Este patrón se puede implementar en la sintaxis de T-SQL mediante la siguiente plantilla:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

La plantilla comienza por la definición de un bloque `TRY...CATCH`, una construcción nueva en SQL Server 2005. Al igual que con los bloques de `Try...Catch` en Visual Basic, el bloque de `TRY...CATCH` SQL ejecuta las instrucciones en el bloque de `TRY`. Si alguna instrucción produce un error, el control se transfiere inmediatamente al bloque `CATCH`.

Si no hay errores al ejecutar las instrucciones SQL que estructuran la transacción, la instrucción `COMMIT TRANSACTION` confirma los cambios y completa la transacción. Sin embargo, si una de las instrucciones produce un error, el `ROLLBACK TRANSACTION` del bloque `CATCH` devuelve la base de datos a su estado anterior al inicio de la transacción. El procedimiento almacenado también genera un error con el [comando RAISERROR](https://msdn.microsoft.com/library/ms178592.aspx), que hace que se genere un `SqlException` en la aplicación.

> [!NOTE]
> Dado que el bloque `TRY...CATCH` es nuevo en SQL Server 2005, la plantilla anterior no funcionará si usa versiones anteriores de Microsoft SQL Server. Si no utiliza SQL Server 2005, consulte Administración de [transacciones en SQL Server procedimientos almacenados](http://www.4guysfromrolla.com/webtech/080305-1.shtml) de una plantilla que funcionará con otras versiones de SQL Server.

Echemos un vistazo a un ejemplo concreto. Existe una restricción FOREIGN KEY entre las tablas `Categories` y `Products`, lo que significa que cada campo `CategoryID` de la tabla `Products` debe asignarse a un valor `CategoryID` de la tabla `Categories`. Cualquier acción que infrinja esta restricción, como intentar eliminar una categoría que tiene productos asociados, produce una infracción de la restricción FOREIGN KEY. Para comprobarlo, vuelva a visitar el ejemplo de actualización y eliminación de datos binarios existentes en la sección trabajar con datos binarios (`~/BinaryData/UpdatingAndDeleting.aspx`). En esta página se muestra una lista de cada categoría del sistema junto con los botones editar y eliminar (consulte la figura 13), pero si intenta eliminar una categoría que tiene productos asociados (como bebidas), se produce un error en la eliminación debido a una infracción de restricción de clave externa (consulte la figura 14).

[![cada categoría se muestra en un control GridView con botones de edición y eliminación](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png)

**Figura 13**: cada categoría se muestra en un control GridView con botones de edición y eliminación ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png))

[![no puede eliminar una categoría que tenga productos existentes](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png)

**Figura 14**: no se puede eliminar una categoría que tenga productos existentes ([haga clic para ver la imagen de tamaño completo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png))

No obstante, Imagine que queremos permitir la eliminación de las categorías independientemente de si tienen productos asociados. Si se elimina una categoría con productos, Imagine que queremos eliminar también sus productos existentes (aunque otra opción sería simplemente establecer sus productos `CategoryID` valores en `NULL`). Esta funcionalidad se puede implementar a través de las reglas en cascada de la restricción FOREIGN KEY. Como alternativa, podríamos crear un procedimiento almacenado que acepta un `@CategoryID` parámetro de entrada y, cuando se invoca, elimina explícitamente todos los productos asociados y, a continuación, la categoría especificada.

Nuestro primer intento en este tipo de procedimiento almacenado podría ser similar al siguiente:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

Aunque esto eliminará definitivamente los productos y la categoría asociados, no lo hace en el paraguas de una transacción. Imagine que hay alguna otra restricción FOREIGN KEY en `Categories` que prohíba la eliminación de un valor `@CategoryID` determinado. El problema es que, en tal caso, todos los productos se eliminarán antes de intentar eliminar la categoría. El resultado neto es que para este tipo de categoría, este procedimiento almacenado quitaría todos sus productos mientras la categoría permaneció porque todavía tiene registros relacionados en otra tabla.

Sin embargo, si el procedimiento almacenado se ajustó dentro del ámbito de una transacción, la eliminación en la tabla de `Products` se revertiría en el caso de una eliminación con errores en `Categories`. El siguiente script de procedimiento almacenado utiliza una transacción para garantizar la atomicidad entre las dos instrucciones de `DELETE`:

[!code-sql[Main](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.sql)]

Dedique un momento a agregar el `Categories_Delete` procedimiento almacenado a la base de datos Northwind. Consulte el paso 1 para obtener instrucciones sobre cómo agregar procedimientos almacenados a una base de datos.

## <a name="step-6-updating-thecategoriestableadapter"></a>Paso 6: actualización del`CategoriesTableAdapter`

Aunque se ha agregado el `Categories_Delete` procedimiento almacenado a la base de datos, la capa DAL está configurada actualmente para usar instrucciones SQL ad-hoc para realizar la eliminación. Necesitamos actualizar el `CategoriesTableAdapter` e indicarle que use el `Categories_Delete` procedimiento almacenado en su lugar.

> [!NOTE]
> Anteriormente en este tutorial, trabajamos con el conjunto de `NorthwindWithSprocs`. Pero el conjunto de cambios solo tiene una entidad única, `ProductsDataTable`y necesitamos trabajar con categorías. Por lo tanto, para el resto de este tutorial, cuando hablemos sobre la capa de acceso a datos I m que hace referencia a la `Northwind` conjunto de datos, la que primero creamos en el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) .

Abra el conjunto de de Northwind, seleccione el `CategoriesTableAdapter`y vaya al ventana Propiedades. En el ventana Propiedades se enumeran los `InsertCommand`, `UpdateCommand`, `DeleteCommand`y `SelectCommand` utilizados por TableAdapter, así como su nombre y la información de conexión. Expanda la propiedad `DeleteCommand` para ver sus detalles. Como se muestra en la figura 15, la propiedad `DeleteCommand` s `CommandType` está establecida en texto, lo que le indica que envíe el texto de la propiedad `CommandText` como una consulta SQL ad hoc.

![Seleccione CategoriesTableAdapter en el diseñador para ver sus propiedades en la ventana Propiedades.](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png)

**Figura 15**: seleccione el `CategoriesTableAdapter` en el diseñador para ver sus propiedades en la ventana Propiedades.

Para cambiar esta configuración, seleccione el texto (DeleteCommand) en el ventana Propiedades y elija (nuevo) en la lista desplegable. Esto borrará la configuración de las propiedades `CommandText`, `CommandType`y `Parameters`. A continuación, establezca la propiedad `CommandType` en `StoredProcedure` y escriba el nombre del procedimiento almacenado para el `CommandText` (`dbo.Categories_Delete`). Si no está seguro de escribir las propiedades en este orden, en primer lugar el `CommandType` y, a continuación, el `CommandText`-Visual Studio rellenará automáticamente la colección de parámetros. Si no especifica estas propiedades en este orden, tendrá que agregar manualmente los parámetros a través del editor de la colección de parámetros. En cualquier caso, es prudente hacer clic en los puntos suspensivos de la propiedad parameters para abrir el editor de la colección de parámetros para comprobar que se han realizado los cambios de configuración de parámetros correctos (vea la figura 16). Si no ve ningún parámetro en el cuadro de diálogo, agregue el parámetro `@CategoryID` manualmente (no es necesario agregar el parámetro `@RETURN_VALUE`).

![Asegúrese de que la configuración de los parámetros sea correcta](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 16**: Asegúrese de que la configuración de los parámetros sea correcta

Una vez actualizada la capa DAL, al eliminar una categoría se eliminarán automáticamente todos sus productos asociados y se realizará en el paraguas de una transacción. Para comprobarlo, vuelva a la página actualizar y eliminar los datos binarios existentes y haga clic en el botón eliminar de una de las categorías. Con un solo clic del mouse, se eliminarán la categoría y todos sus productos asociados.

> [!NOTE]
> Antes de probar el `Categories_Delete` procedimiento almacenado, que eliminará una serie de productos junto con la categoría seleccionada, puede ser prudente realizar una copia de seguridad de la base de datos. Si usa la base de datos de `NORTHWND.MDF` en `App_Data`, simplemente cierre Visual Studio y copie los archivos MDF y LDF de `App_Data` en otra carpeta. Después de probar la funcionalidad, puede restaurar la base de datos cerrando Visual Studio y reemplazando los archivos MDF y LDF actuales en `App_Data` con las copias de seguridad.

## <a name="summary"></a>Resumen

Aunque el Asistente de TableAdapter s generará automáticamente procedimientos almacenados para nosotros, hay ocasiones en las que es posible que ya se hayan creado estos procedimientos almacenados o que desee crearlos manualmente o con otras herramientas. Para dar cabida a estos escenarios, el TableAdapter también puede configurarse para que apunte a un procedimiento almacenado existente. En este tutorial, hemos examinado cómo agregar manualmente procedimientos almacenados a una base de datos a través del entorno de Visual Studio y cómo conectar los métodos de TableAdapter s a estos procedimientos almacenados. También hemos examinado los comandos de T-SQL y el patrón de scripts que se usan para iniciar, confirmar y revertir las transacciones desde dentro de un procedimiento almacenado.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Hilton Geisenow, S Ren Jacob Lauritsen y Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
> [Siguiente](updating-the-tableadapter-to-use-joins-vb.md)
