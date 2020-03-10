---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
title: Actualizar TableAdapter para usar combinaciones (C#) | Microsoft Docs
author: rick-anderson
description: Cuando se trabaja con una base de datos, es habitual solicitar datos distribuidos en varias tablas. Para recuperar datos de dos tablas diferentes, se puede usar...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: 675531a7-cb54-4dd6-89ac-2636e4c285a5
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/updating-the-tableadapter-to-use-joins-cs
msc.type: authoredcontent
ms.openlocfilehash: 24ff3645783dabfcdef5ac313a2d4833e4998efc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78427777"
---
# <a name="updating-the-tableadapter-to-use-joins-c"></a>Actualizar TableAdapter para usar JOIN (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_69_CS.zip) o [Descargar PDF](updating-the-tableadapter-to-use-joins-cs/_static/datatutorial69cs1.pdf)

> Cuando se trabaja con una base de datos, es habitual solicitar datos distribuidos en varias tablas. Para recuperar datos de dos tablas diferentes, se puede usar una subconsulta correlacionada o una operación de combinación. En este tutorial se comparan las subconsultas correlacionadas y la sintaxis de JOIN antes de ver cómo crear un TableAdapter que incluya una combinación en su consulta principal.

## <a name="introduction"></a>Introducción

Con las bases de datos relacionales, los datos que estamos interesados en trabajar con se suelen distribuir en varias tablas. Por ejemplo, al mostrar la información del producto, es probable que desee enumerar los nombres correspondientes de las categorías y los proveedores de cada producto. La tabla `Products` tiene valores `CategoryID` y `SupplierID`, pero los nombres de categoría y proveedor reales están en las tablas `Categories` y `Suppliers`, respectivamente.

Para recuperar información de otra tabla relacionada, podemos usar *subconsultas* correlacionadas o `JOIN`*s*. Una subconsulta correlacionada es una consulta `SELECT` anidada que hace referencia a las columnas de la consulta externa. Por ejemplo, en el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md) usamos dos subconsultas correlacionadas en la consulta principal `ProductsTableAdapter` s para devolver los nombres de categoría y proveedor de cada producto. Una `JOIN` es una construcción de SQL que combina filas relacionadas de dos tablas diferentes. Usamos un `JOIN` en el tutorial de [consultas de datos con el control SqlDataSource](../accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs.md) para mostrar información de categoría junto con cada producto.

La razón por la que tenemos abstained de usar `JOIN` s con TableAdapters es debido a las limitaciones del asistente de TableAdapter s para generar automáticamente las instrucciones `INSERT`, `UPDATE`y `DELETE` correspondientes. Más concretamente, si la consulta principal de TableAdapter s contiene cualquier `JOIN`, el TableAdapter no puede crear automáticamente las instrucciones SQL o los procedimientos almacenados ad hoc para sus propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand`.

En este tutorial, compararemos brevemente y contrastan las subconsultas correlacionadas y `JOIN` s antes de explorar cómo crear un TableAdapter que incluya `JOIN` s en su consulta principal.

## <a name="comparing-and-contrasting-correlated-subqueries-andjoin-s"></a>Comparar y contrastar las subconsultas correlacionadas y`JOIN` s

Recuerde que los `ProductsTableAdapter` creados en el primer tutorial del conjunto de datos de `Northwind` usan subconsultas correlacionadas para devolver cada producto de la categoría correspondiente y el nombre del proveedor. A continuación se muestra la consulta principal `ProductsTableAdapter` s.

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample1.sql)]

Las dos subconsultas correlacionadas: `(SELECT CategoryName FROM Categories WHERE Categories.CategoryID = Products.CategoryID)` y `(SELECT CompanyName FROM Suppliers WHERE Suppliers.SupplierID = Products.SupplierID)` son `SELECT` consultas que devuelven un único valor por producto como columna adicional en la lista de columnas de la instrucción de `SELECT` externa.

Como alternativa, se puede usar un `JOIN` para devolver cada proveedor y nombre de categoría del producto. La consulta siguiente devuelve el mismo resultado que el anterior, pero usa `JOIN` s en lugar de subconsultas:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample2.sql)]

Un `JOIN` combina los registros de una tabla con registros de otra tabla en función de algunos criterios. En la consulta anterior, por ejemplo, el `LEFT JOIN Categories ON Categories.CategoryID = Products.CategoryID` indica a SQL Server que combine cada registro de producto con el registro de categoría cuyo valor de `CategoryID` coincide con el valor de `CategoryID` del producto. El resultado combinado nos permite trabajar con los campos de categoría correspondientes para cada producto (como `CategoryName`).

> [!NOTE]
> los `JOIN` s suelen usarse para consultar datos de bases de datos relacionales. Si no está familiarizado con la sintaxis de `JOIN` o necesita resaltar un poco en su uso, se recomienda el [tutorial de combinación de SQL](http://www.w3schools.com/sql/sql_join.asp) en [escuelas de W3](http://www.w3schools.com/). También merece la pena leer las secciones [aspectos básicos del`JOIN`](https://msdn.microsoft.com/library/ms191517.aspx) y [subconsultas](https://msdn.microsoft.com/library/ms189575.aspx) de los [libros en pantalla de SQL](https://msdn.microsoft.com/library/ms130214.aspx).

Puesto que `JOIN` s y las subconsultas correlacionadas se pueden usar para recuperar los datos relacionados de otras tablas, muchos desarrolladores dejan el arañazo de sus cabezales y se preguntan qué enfoque usar. Todos los gurús de SQL a los que he hablado han mencionado aproximadamente lo mismo, que no es tan importante para el rendimiento, ya que SQL Server producirá planes de ejecución aproximadamente idénticos. A continuación, le aconsejamos que use la técnica con la que usted y su equipo se sienten más cómodos. Merece la pena tener en cuenta que, después de que se informan estos consejos, estos expertos expresan inmediatamente su preferencia de `JOIN` s en las subconsultas correlacionadas.

Al compilar una capa de acceso a datos mediante conjuntos de datos con tipo, las herramientas funcionan mejor cuando se usan subconsultas. En concreto, el Asistente de TableAdapter s no generará automáticamente las instrucciones `INSERT`, `UPDATE`y `DELETE` correspondientes si la consulta principal contiene `JOIN` s, pero generará automáticamente estas instrucciones cuando se utilicen subconsultas correlacionadas.

Para explorar este defecto, cree un conjunto de un conjunto de tipos temporal en la carpeta `~/App_Code/DAL`. Durante el Asistente para la configuración de TableAdapter, elija usar instrucciones SQL ad hoc y escriba la siguiente consulta de `SELECT` (consulte la figura 1):

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample3.sql)]

[![escriba una consulta principal que contenga combinaciones](updating-the-tableadapter-to-use-joins-cs/_static/image2.png)](updating-the-tableadapter-to-use-joins-cs/_static/image1.png)

**Figura 1**: escriba una consulta principal que contenga `JOIN` s ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image3.png))

De forma predeterminada, el TableAdapter creará automáticamente `INSERT`, `UPDATE`y `DELETE` instrucciones basadas en la consulta principal. Si hace clic en el botón avanzadas, puede ver que esta característica está habilitada. A pesar de esta configuración, el TableAdapter no podrá crear las instrucciones `INSERT`, `UPDATE`y `DELETE` porque la consulta principal contiene una `JOIN`.

![Escriba una consulta principal que contenga combinaciones](updating-the-tableadapter-to-use-joins-cs/_static/image4.png)

**Figura 2**: escriba una consulta principal que contenga `JOIN` s

Haga clic en Finalizar para completar el asistente. En este momento, el diseñador de DataSet s incluirá un solo TableAdapter con un DataTable con columnas para cada uno de los campos devueltos en la lista de columnas de `SELECT` consulta. Esto incluye el `CategoryName` y `SupplierName`, como se muestra en la figura 3.

![La DataTable incluye una columna para cada campo devuelto en la lista de columnas.](updating-the-tableadapter-to-use-joins-cs/_static/image5.png)

**Figura 3**: la DataTable incluye una columna para cada campo devuelto en la lista de columnas

Aunque el objeto DataTable tiene las columnas adecuadas, el TableAdapter carece de valores para sus propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand`. Para confirmarlo, haga clic en el TableAdapter en el diseñador y, a continuación, vaya a la ventana Propiedades. Allí verá que las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` están establecidas en (ninguno).

[![las propiedades InsertCommand, UpdateCommand y DeleteCommand están establecidas en (ninguna)](updating-the-tableadapter-to-use-joins-cs/_static/image7.png)](updating-the-tableadapter-to-use-joins-cs/_static/image6.png)

**Figura 4**: las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` están establecidas en (ninguno) ([haga clic para ver la imagen a tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image8.png))

Para solucionar este inconveniente, podemos proporcionar manualmente las instrucciones y los parámetros de SQL para las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` a través del ventana Propiedades. Como alternativa, podríamos empezar configurando la consulta principal de TableAdapter s para que *no* incluya ningún `JOIN` s. Esto permitirá que se generen automáticamente las instrucciones `INSERT`, `UPDATE`y `DELETE`. Después de completar el asistente, podríamos actualizar manualmente los `SelectCommand` TableAdapter s desde el ventana Propiedades para que incluya la sintaxis de `JOIN`.

Aunque este enfoque funciona, es muy complicado cuando se usan consultas SQL ad hoc, ya que cada vez que se reconfigura la consulta principal de TableAdapter s a través del asistente, se vuelven a crear las instrucciones de `INSERT`, `UPDATE`y `DELETE` generadas automáticamente. Esto significa que todas las personalizaciones que se han realizado posteriormente se perderían si hacemos clic con el botón derecho en el TableAdapter, seleccionamos configurar en el menú contextual y completamos el Asistente de nuevo.

La fragilidad de las instrucciones de `INSERT`, `UPDATE`y `DELETE` generadas automáticamente por TableAdapter se limita a las instrucciones SQL ad hoc. Si el TableAdapter utiliza procedimientos almacenados, puede personalizar los procedimientos almacenados `SelectCommand`, `InsertCommand`, `UpdateCommand`o `DeleteCommand` y volver a ejecutar el Asistente para configuración de TableAdapter sin tener que preocuparse de que se modifiquen los procedimientos almacenados.

En los siguientes pasos se creará un TableAdapter que, inicialmente, usa una consulta principal que omite cualquier `JOIN` s para que los procedimientos almacenados de inserción, actualización y eliminación correspondientes se generen automáticamente. A continuación, actualizaremos el `SelectCommand` para que use una `JOIN` que devuelva columnas adicionales de las tablas relacionadas. Por último, crearemos una clase de capa de lógica de negocios correspondiente y mostraremos el uso de TableAdapter en una página web de ASP.NET.

## <a name="step-1-creating-the-tableadapter-using-a-simplified-main-query"></a>Paso 1: crear el TableAdapter con una consulta principal simplificada

En este tutorial, vamos a agregar un TableAdapter y un DataTable fuertemente tipado para la tabla `Employees` del conjunto de `NorthwindWithSprocs`. La tabla `Employees` contiene un campo `ReportsTo` que especifica la `EmployeeID` del Director de empleados. Por ejemplo, el empleado Anne Dodsworth tiene un valor `ReportTo` de 5, que es el `EmployeeID` de Steven Buchanan. Por lo tanto, Cecilia informa a Steven, su director. Junto con la notificación de cada empleado `ReportsTo` valor, es posible que también quiera recuperar el nombre de su administrador. Esto puede realizarse mediante un `JOIN`. Pero el uso de un `JOIN` al crear el TableAdapter inicialmente impide que el asistente genere automáticamente las funciones de inserción, actualización y eliminación correspondientes. Por lo tanto, comenzaremos por crear un TableAdapter cuya consulta principal no contenga ningún `JOIN` s. A continuación, en el paso 2, actualizaremos el procedimiento almacenado de consulta principal para recuperar el nombre del administrador a través de un `JOIN`.

Para empezar, abra el conjunto de `NorthwindWithSprocs` en la carpeta `~/App_Code/DAL`. Haga clic con el botón secundario en el diseñador, seleccione la opción Agregar en el menú contextual y elija el elemento de menú TableAdapter. Se iniciará el Asistente para la configuración de TableAdapter. Como se muestra en la figura 5, haga que el asistente cree nuevos procedimientos almacenados y haga clic en siguiente. Para obtener un actualizador sobre cómo crear nuevos procedimientos almacenados desde el Asistente de TableAdapter s, consulte el tutorial [creación de nuevos procedimientos almacenados para el conjunto de objetos de tipo TableAdapters](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) .

[![seleccionar la opción crear nuevos procedimientos almacenados](updating-the-tableadapter-to-use-joins-cs/_static/image10.png)](updating-the-tableadapter-to-use-joins-cs/_static/image9.png)

**Figura 5**: seleccione la opción crear nuevos procedimientos almacenados ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image11.png))

Use la siguiente instrucción `SELECT` para la consulta principal de TableAdapter s:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample4.sql)]

Puesto que esta consulta no incluye ningún `JOIN` s, el Asistente para TableAdapter creará automáticamente procedimientos almacenados con las instrucciones `INSERT`, `UPDATE`y `DELETE` correspondientes, así como un procedimiento almacenado para ejecutar la consulta principal.

El siguiente paso nos permite nombrar los procedimientos almacenados de TableAdapter s. Use los nombres `Employees_Select`, `Employees_Insert`, `Employees_Update`y `Employees_Delete`, como se muestra en la figura 6.

[![nombre de los procedimientos almacenados de TableAdapter s](updating-the-tableadapter-to-use-joins-cs/_static/image13.png)](updating-the-tableadapter-to-use-joins-cs/_static/image12.png)

**Ilustración 6**: asignar un nombre a los procedimientos almacenados de TableAdapter s ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image14.png))

El paso final nos pide asignar un nombre a los métodos de TableAdapter. Utilice `Fill` y `GetEmployees` como nombres de método. Asegúrese también de dejar activada la casilla crear métodos para enviar actualizaciones directamente a la base de datos (GenerateDBDirectMethods).

[![el nombre de los métodos de TableAdapter s Fill y GetEmployees](updating-the-tableadapter-to-use-joins-cs/_static/image16.png)](updating-the-tableadapter-to-use-joins-cs/_static/image15.png)

**Figura 7**: asigne un nombre a los métodos de TableAdapter `Fill` y `GetEmployees` ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image17.png))

Después de completar el asistente, dedique un momento a examinar los procedimientos almacenados en la base de datos. Debería ver cuatro nuevos: `Employees_Select`, `Employees_Insert`, `Employees_Update`y `Employees_Delete`. A continuación, inspeccione el `EmployeesDataTable` y `EmployeesTableAdapter` que acaba de crear. DataTable contiene una columna para cada campo devuelto por la consulta principal. Haga clic en el TableAdapter y, a continuación, vaya a la ventana Propiedades. Allí verá que las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` están configuradas correctamente para llamar a los procedimientos almacenados correspondientes.

[![TableAdapter incluye funcionalidades de inserción, actualización y eliminación](updating-the-tableadapter-to-use-joins-cs/_static/image19.png)](updating-the-tableadapter-to-use-joins-cs/_static/image18.png)

**Figura 8**: el TableAdapter incluye funcionalidades de inserción, actualización y eliminación ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image20.png))

Con los procedimientos almacenados INSERT, Update y DELETE creados automáticamente y las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand` configuradas correctamente, estamos preparados para personalizar el procedimiento almacenado `SelectCommand` s con el fin de devolver información adicional sobre cada director de empleados. En concreto, es necesario actualizar el `Employees_Select` procedimiento almacenado para usar una `JOIN` y devolver los valores de `FirstName` y `LastName` de administrador. Una vez actualizado el procedimiento almacenado, será necesario actualizar el objeto DataTable para que incluya estas columnas adicionales. Abordaremos estas dos tareas en los pasos 2 y 3.

## <a name="step-2-customizing-the-stored-procedure-to-include-ajoin"></a>Paso 2: personalizar el procedimiento almacenado para incluir un`JOIN`

Para empezar, vaya a la Explorador de servidores, explore en profundidad la carpeta de procedimientos almacenados de la base de datos Northwind y abra el procedimiento almacenado `Employees_Select`. Si no ve este procedimiento almacenado, haga clic con el botón secundario en la carpeta procedimientos almacenados y elija actualizar. Actualice el procedimiento almacenado para que use un `LEFT JOIN` para devolver el nombre y el apellido del administrador:

[!code-sql[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample5.sql)]

Después de actualizar la instrucción `SELECT`, guarde los cambios; para ello, vaya al menú Archivo y elija guardar `Employees_Select`. Como alternativa, puede hacer clic en el icono guardar en la barra de herramientas o presionar CTRL + S. Después de guardar los cambios, haga clic con el botón derecho en el `Employees_Select` procedimiento almacenado en el Explorador de servidores y elija Ejecutar. Se ejecutará el procedimiento almacenado y se mostrarán los resultados en la ventana de salida (vea la ilustración 9).

[![los resultados de los procedimientos almacenados se muestran en el Ventana de salida](updating-the-tableadapter-to-use-joins-cs/_static/image22.png)](updating-the-tableadapter-to-use-joins-cs/_static/image21.png)

**Figura 9**: los resultados de los procedimientos almacenados se muestran en el ventana de salida ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image23.png))

## <a name="step-3-updating-the-datatable-s-columns"></a>Paso 3: actualizar las columnas de DataTable s

En este momento, el `Employees_Select` procedimiento almacenado devuelve `ManagerFirstName` y `ManagerLastName`, pero el `EmployeesDataTable` no tiene estas columnas. Estas columnas que faltan se pueden agregar a la DataTable de una de estas dos maneras:

- **Manualmente** : haga clic con el botón derecho en DataTable en el diseñador de DataSet y, en el menú Agregar, elija columna. Después, puede asignar un nombre a la columna y establecer sus propiedades en consecuencia.
- **Automáticamente** : el Asistente para configuración de TableAdapter actualizará las columnas DataTable s para reflejar los campos devueltos por el `SelectCommand` procedimiento almacenado. Cuando se utilizan instrucciones SQL ad hoc, el Asistente también quitará las propiedades `InsertCommand`, `UpdateCommand`y `DeleteCommand`, ya que el `SelectCommand` contiene ahora una `JOIN`. Pero cuando se usan procedimientos almacenados, estas propiedades de comando permanecen intactas.

Hemos explorado manualmente la adición de columnas DataTable en los tutoriales anteriores, incluidos el maestro y los [detalles, con una lista con viñetas de registros maestros con una lista de datos de detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-cs.md) y la carga de [archivos](../working-with-binary-files/uploading-files-cs.md), y veremos este proceso de nuevo con más detalle en el siguiente tutorial. Sin embargo, para este tutorial, se permite usar el enfoque automático mediante el Asistente para configuración de TableAdapter.

Para empezar, haga clic con el botón derecho en el `EmployeesTableAdapter` y seleccione configurar en el menú contextual. Se abre el Asistente para configuración de TableAdapter, que enumera los procedimientos almacenados que se usan para seleccionar, insertar, actualizar y eliminar, junto con los valores devueltos y los parámetros (si existen). La figura 10 muestra este asistente. Aquí podemos ver que el procedimiento almacenado de `Employees_Select` ahora devuelve los campos `ManagerFirstName` y `ManagerLastName`.

[![el asistente muestra la lista de columnas actualizada para el procedimiento almacenado Employees_Select](updating-the-tableadapter-to-use-joins-cs/_static/image25.png)](updating-the-tableadapter-to-use-joins-cs/_static/image24.png)

**Figura 10**: el asistente muestra la lista de columnas actualizada para el `Employees_Select` procedimiento almacenado ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image26.png))

Para completar el asistente, haga clic en finalizar. Al volver al diseñador de DataSet, el `EmployeesDataTable` incluye dos columnas adicionales: `ManagerFirstName` y `ManagerLastName`.

[![EmployeesDataTable contiene dos nuevas columnas](updating-the-tableadapter-to-use-joins-cs/_static/image28.png)](updating-the-tableadapter-to-use-joins-cs/_static/image27.png)

**Figura 11**: el `EmployeesDataTable` contiene dos nuevas columnas ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image29.png))

Para ilustrar que el procedimiento almacenado `Employees_Select` actualizado está en vigor y que las funciones de inserción, actualización y eliminación de TableAdapter siguen siendo funcionales, permita crear una página web que permita a los usuarios ver y eliminar empleados. Sin embargo, antes de crear este tipo de página, es necesario crear primero una nueva clase en la capa de lógica de negocios para trabajar con los empleados del conjunto de `NorthwindWithSprocs`. En el paso 4, crearemos una clase `EmployeesBLLWithSprocs`. En el paso 5, usaremos esta clase desde una página de ASP.NET.

## <a name="step-4-implementing-the-business-logic-layer"></a>Paso 4: implementación de la capa de lógica de negocios

Cree un nuevo archivo de clase en la carpeta `~/App_Code/BLL` denominada `EmployeesBLLWithSprocs.cs`. Esta clase imita la semántica de la clase de `EmployeesBLL` existente, solo esta nueva proporciona menos métodos y utiliza el conjunto de `NorthwindWithSprocs` (en lugar de `Northwind` conjunto de caracteres). Agregue el código siguiente a la clase `EmployeesBLLWithSprocs` .

[!code-csharp[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample6.cs)]

La propiedad `EmployeesBLLWithSprocs` Class s `Adapter` devuelve una instancia de la `NorthwindWithSprocs` DataSet s `EmployeesTableAdapter`. Lo usan los métodos `GetEmployees` y `DeleteEmployee` de la clase s. El método `GetEmployees` llama al método `GetEmployees` `EmployeesTableAdapter` correspondiente, que invoca el procedimiento almacenado `Employees_Select` y rellena sus resultados en un `EmployeeDataTable`. El método `DeleteEmployee` llama de forma similar al método `EmployeesTableAdapter` s `Delete`, que invoca el procedimiento almacenado `Employees_Delete`.

## <a name="step-5-working-with-the-data-in-the-presentation-layer"></a>Paso 5: trabajar con los datos de la capa de presentación

Una vez completada la clase `EmployeesBLLWithSprocs`, se vuelve a preparar para trabajar con los datos de los empleados a través de una página de ASP.NET. Abra la página `JOINs.aspx` en la carpeta `AdvancedDAL` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador, estableciendo su propiedad `ID` en `Employees`. A continuación, desde la etiqueta inteligente de GridView s, enlace la cuadrícula a un nuevo control ObjectDataSource denominado `EmployeesDataSource`.

Configure ObjectDataSource para usar la clase `EmployeesBLLWithSprocs` y, en las pestañas seleccionar y eliminar, asegúrese de que los métodos `GetEmployees` y `DeleteEmployee` estén seleccionados en las listas desplegables. Haga clic en finalizar para completar la configuración de ObjectDataSource s.

[![configurar ObjectDataSource para usar la clase EmployeesBLLWithSprocs](updating-the-tableadapter-to-use-joins-cs/_static/image31.png)](updating-the-tableadapter-to-use-joins-cs/_static/image30.png)

**Figura 12**: configuración de ObjectDataSource para usar la clase `EmployeesBLLWithSprocs` ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image32.png))

[![que ObjectDataSource use los métodos GetEmployees y DeleteEmployee](updating-the-tableadapter-to-use-joins-cs/_static/image34.png)](updating-the-tableadapter-to-use-joins-cs/_static/image33.png)

**Figura 13**: hacer que ObjectDataSource use los métodos `GetEmployees` y `DeleteEmployee` ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image35.png))

Visual Studio agregará BoundField a GridView para cada una de las columnas de `EmployeesDataTable` s. Quite todas estas BoundFields, excepto `Title`, `LastName`, `FirstName`, `ManagerFirstName`y `ManagerLastName` y cambiar el nombre de las propiedades de `HeaderText` de los cuatro últimos BoundFields a apellidos, nombre, nombre de administrador y apellidos de administrador, respectivamente.

Para permitir que los usuarios eliminen los empleados de esta página, es necesario hacer dos cosas. En primer lugar, indique a GridView que proporcione capacidades de eliminación mediante la comprobación de la opción Habilitar eliminación de su etiqueta inteligente. En segundo lugar, cambie la propiedad ObjectDataSource s `OldValuesParameterFormatString` del valor establecido por el Asistente para ObjectDataSource (`original_{0}`) a su valor predeterminado (`{0}`). Después de realizar estos cambios, el marcado declarativo de GridView y ObjectDataSource s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](updating-the-tableadapter-to-use-joins-cs/samples/sample7.aspx)]

Pruebe la página visitando el explorador. Como se muestra en la figura 14, la página enumerará cada empleado y su nombre de administrador (suponiendo que tienen uno).

[![la combinación en el Employees_Select procedimiento almacenado devuelve el nombre del administrador](updating-the-tableadapter-to-use-joins-cs/_static/image37.png)](updating-the-tableadapter-to-use-joins-cs/_static/image36.png)

**Figura 14**: el `JOIN` en el `Employees_Select` procedimiento almacenado devuelve el nombre del administrador ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image38.png))

Al hacer clic en el botón eliminar, se inicia el flujo de trabajo de eliminación, que culmina en la ejecución del `Employees_Delete` procedimiento almacenado. Sin embargo, el intento de `DELETE` instrucción en el procedimiento almacenado produce un error debido a una infracción de la restricción de clave externa (vea la figura 15). En concreto, cada empleado tiene uno o varios registros en la tabla `Orders`, lo que provocará un error en la eliminación.

[![eliminar un empleado que tiene pedidos correspondientes tiene como resultado una infracción de restricción de clave externa](updating-the-tableadapter-to-use-joins-cs/_static/image40.png)](updating-the-tableadapter-to-use-joins-cs/_static/image39.png)

**Figura 15**: eliminación de un empleado que tiene pedidos correspondientes da como resultado una infracción de restricción de clave externa ([haga clic para ver la imagen de tamaño completo](updating-the-tableadapter-to-use-joins-cs/_static/image41.png))

Para permitir que se elimine un empleado, puede:

- Actualice la restricción FOREIGN KEY para las eliminaciones en cascada.
- Elimine manualmente los registros de la tabla `Orders` para los empleados que desea eliminar, o bien
- Actualice el `Employees_Delete` procedimiento almacenado para eliminar primero los registros relacionados de la tabla de `Orders` antes de eliminar el registro de `Employees`. Hemos explicado esta técnica en el tutorial [uso de procedimientos almacenados existentes para el conjunto de objetos del mismo tipo](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) .

Dejo esto como un ejercicio para el lector.

## <a name="summary"></a>Resumen

Cuando se trabaja con bases de datos relacionales, es habitual que las consultas extraigan sus datos de varias tablas relacionadas. Las subconsultas correlacionadas y `JOIN` s proporcionan dos técnicas diferentes para tener acceso a los datos de las tablas relacionadas en una consulta. En los tutoriales anteriores, normalmente se usan subconsultas correlacionadas porque el TableAdapter no puede generar automáticamente instrucciones `INSERT`, `UPDATE`y `DELETE` para las consultas que implican `JOIN` s. Aunque estos valores se pueden proporcionar manualmente, al usar las instrucciones SQL ad hoc, se sobrescribirán las personalizaciones cuando se complete el Asistente para configuración de TableAdapter.

Afortunadamente, los TableAdapters creados con procedimientos almacenados no sufren la misma fragilidad que los creados mediante instrucciones SQL ad-hoc. Por lo tanto, es factible crear un TableAdapter cuya consulta principal utiliza un `JOIN` cuando se usan procedimientos almacenados. En este tutorial, vimos cómo crear este tipo de TableAdapter. Iniciamos con una consulta `SELECT` de `JOIN`menos para la consulta principal de TableAdapter s de modo que los procedimientos almacenados de inserción, actualización y eliminación correspondientes se crearan automáticamente. Una vez completada la configuración inicial de TableAdapter s, hemos aumentado el `SelectCommand` procedimiento almacenado para usar una `JOIN` y volver a ejecutar el Asistente para configuración de TableAdapter para actualizar las columnas de `EmployeesDataTable` s.

Volver a ejecutar el Asistente para configuración de TableAdapter actualizó automáticamente el `EmployeesDataTable` columnas para reflejar los campos de datos devueltos por el procedimiento almacenado `Employees_Select`. Como alternativa, podríamos haber agregado estas columnas manualmente a la DataTable. Veremos cómo agregar columnas manualmente a la DataTable en el siguiente tutorial.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Hilton Geisenow, David Suru y Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)
> [Siguiente](adding-additional-datatable-columns-cs.md)
