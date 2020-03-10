---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
title: Trabajar con columnas calculadas (C#) | Microsoft Docs
author: rick-anderson
description: Al crear una tabla de base de datos, Microsoft SQL Server permite definir una columna calculada cuyo valor se calcula a partir de una expresión que normalmente hace referencia a...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 57459065-ed7c-4dfe-ac9c-54c093abc261
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/working-with-computed-columns-cs
msc.type: authoredcontent
ms.openlocfilehash: ad6a96f2721510c2478f707c8eed018ae797f27a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78427123"
---
# <a name="working-with-computed-columns-c"></a>Trabajar con columnas calculadas (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_71_CS.zip) o [Descargar PDF](working-with-computed-columns-cs/_static/datatutorial71cs1.pdf)

> Al crear una tabla de base de datos, Microsoft SQL Server permite definir una columna calculada cuyo valor se calcula a partir de una expresión que normalmente hace referencia a otros valores del mismo registro de base de datos. Estos valores son de solo lectura en la base de datos, lo que requiere consideraciones especiales al trabajar con TableAdapters. En este tutorial, aprenderá a cumplir los desafíos planteados por las columnas calculadas.

## <a name="introduction"></a>Introducción

Microsoft SQL Server permite *[columnas calculadas](https://msdn.microsoft.com/library/ms191250.aspx)* , que son columnas cuyos valores se calculan a partir de una expresión que normalmente hace referencia a los valores de otras columnas de la misma tabla. Por ejemplo, un modelo de datos de seguimiento de tiempo puede tener una tabla denominada `ServiceLog` con columnas que incluyen `ServicePerformed`, `EmployeeID`, `Rate`y `Duration`, entre otros. Mientras que la cantidad debida por elemento de servicio (que se multiplica por el número de la duración) puede calcularse a través de una página web o de otra interfaz de programación, puede resultar útil incluir una columna en la tabla de `ServiceLog` denominada `AmountDue` que ha comunicado esta información. Esta columna se puede crear como una columna normal, pero debe actualizarse en cualquier momento en que cambien los valores de las columnas `Rate` o `Duration`. Un mejor enfoque sería convertir la columna de `AmountDue` en una columna calculada mediante el `Rate * Duration`de expresión. Si lo hace, SQL Server calcular automáticamente el valor de la columna `AmountDue` cada vez que se hace referencia a él en una consulta.

Dado que el valor de una columna calculada está determinado por una expresión, estas columnas son de solo lectura y, por lo tanto, no pueden tener valores asignados en `INSERT` o `UPDATE` instrucciones. Sin embargo, cuando las columnas calculadas forman parte de la consulta principal para un TableAdapter que usa instrucciones SQL ad hoc, se incluyen automáticamente en las instrucciones de `UPDATE` y `INSERT` generadas automáticamente. Por consiguiente, las consultas `INSERT` y `UPDATE` de TableAdapter s y las propiedades `InsertCommand` y `UpdateCommand` deben actualizarse para quitar las referencias a las columnas calculadas.

Un desafío del uso de las columnas calculadas con un TableAdapter que usa instrucciones SQL ad hoc es que las consultas de TableAdapter s `INSERT` y `UPDATE` se vuelven a generar automáticamente cada vez que se completa el Asistente para configuración de TableAdapter. Por lo tanto, las columnas calculadas quitadas manualmente de las consultas `INSERT` y `UPDATE` volverán a aparecer si se vuelve a ejecutar el asistente. Aunque los TableAdapters que usan procedimientos almacenados no sufren esta fragilidad, tienen sus propias peculiaridades que abordaremos en el paso 3.

En este tutorial se agregará una columna calculada a la tabla `Suppliers` de la base de datos Northwind y, a continuación, se creará el TableAdapter correspondiente para trabajar con esta tabla y su columna calculada. El TableAdapter utilizará procedimientos almacenados en lugar de instrucciones SQL ad hoc para que las personalizaciones no se pierdan cuando se use el Asistente para configuración de TableAdapter.

Vamos a empezar a trabajar.

## <a name="step-1-adding-a-computed-column-to-thesupplierstable"></a>Paso 1: agregar una columna calculada a la tabla`Suppliers`

La base de datos Northwind no tiene ninguna columna calculada, por lo que tendremos que agregar una. En este tutorial, vamos a agregar una columna calculada a la tabla `Suppliers` denominada `FullContactName` que devuelve el nombre de contacto, el título y la empresa para la que trabajan en el siguiente formato: `ContactName` (`ContactTitle`, `CompanyName`). Esta columna calculada se puede utilizar en los informes cuando se muestra información acerca de los proveedores.

Para empezar, abra la definición de la tabla `Suppliers` haciendo clic con el botón secundario en la tabla de `Suppliers` de la Explorador de servidores y elija Abrir definición de tabla en el menú contextual. Se mostrarán las columnas de la tabla y sus propiedades, como su tipo de datos, si permiten `NULL` s, etc. Para agregar una columna calculada, empiece por escribir el nombre de la columna en la definición de la tabla. A continuación, escriba su expresión en el cuadro de texto (fórmula) bajo la sección especificación de la columna calculada en la columna ventana Propiedades (vea la ilustración 1). Asigne a la columna calculada el nombre `FullContactName` y utilice la siguiente expresión:

[!code-sql[Main](working-with-computed-columns-cs/samples/sample1.sql)]

Tenga en cuenta que las cadenas se pueden concatenar en SQL mediante el operador `+`. La instrucción `CASE` se puede usar como un condicional en un lenguaje de programación tradicional. En la expresión anterior, la instrucción `CASE` se puede leer como: si `ContactTitle` no `NULL` a continuación, se genera el valor de `ContactTitle` concatenado con una coma, de lo contrario no se emite nada. Para obtener más información sobre la utilidad de la instrucción `CASE`, vea [la eficacia de las instrucciones SQL `CASE`](http://www.4guysfromrolla.com/webtech/102704-1.shtml).

> [!NOTE]
> En lugar de usar una instrucción `CASE` aquí, podríamos haber usado de forma alternativa `ISNULL(ContactTitle, '')`. [`ISNULL(checkExpression, replacementValue)`](https://msdn.microsoft.com/library/ms184325.aspx) devuelve *checkExpression* si no es null; en caso contrario, devuelve *replacementValue*. Aunque `ISNULL` o `CASE` funcionarán en esta instancia, hay escenarios más complejos en los que la flexibilidad de la instrucción `CASE` no puede coincidir con `ISNULL`.

Después de agregar esta columna calculada, la pantalla debe ser similar a la captura de pantalla de la ilustración 1.

[![agregar una columna calculada denominada FullContactName a la tabla Suppliers](working-with-computed-columns-cs/_static/image2.png)](working-with-computed-columns-cs/_static/image1.png)

**Figura 1**: agregar una columna calculada denominada `FullContactName` a la tabla `Suppliers` ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image3.png))

Después de asignar un nombre a la columna calculada y escribir su expresión, guarde los cambios en la tabla haciendo clic en el icono guardar en la barra de herramientas; para ello, presione Ctrl + S o vaya al menú Archivo y elija guardar `Suppliers`.

Al guardar la tabla, se debe actualizar el Explorador de servidores, incluida la columna recién agregada de la lista de columnas de la tabla `Suppliers`. Además, la expresión especificada en el cuadro de texto (fórmula) se ajustará automáticamente a una expresión equivalente que elimine los espacios en blanco innecesarios, rodea los nombres de columna entre corchetes (`[]`) e incluye paréntesis para mostrar más explícitamente el orden de las operaciones:

[!code-sql[Main](working-with-computed-columns-cs/samples/sample2.sql)]

Para obtener más información sobre las columnas calculadas en Microsoft SQL Server, consulte la [documentación técnica](https://msdn.microsoft.com/library/ms191250.aspx)de. Consulte también [Cómo: especificar columnas calculadas](https://msdn.microsoft.com/library/ms188300.aspx) para obtener un tutorial paso a paso sobre la creación de columnas calculadas.

> [!NOTE]
> De forma predeterminada, las columnas calculadas no se almacenan físicamente en la tabla, sino que se vuelven a calcular cada vez que se hace referencia a ellas en una consulta. No obstante, si activa la casilla es persistente, puede indicar a SQL Server que almacene físicamente la columna calculada en la tabla. Esto permite crear un índice en la columna calculada, lo que puede mejorar el rendimiento de las consultas que usan el valor de la columna calculada en sus cláusulas `WHERE`. Vea [crear índices en columnas calculadas](https://msdn.microsoft.com/library/ms189292.aspx) para obtener más información.

## <a name="step-2-viewing-the-computed-column-s-values"></a>Paso 2: ver los valores de las columnas calculadas

Antes de empezar a trabajar en el nivel de acceso a datos, deje que se tarde un minuto en ver los valores de `FullContactName`. En el Explorador de servidores, haga clic con el botón derecho en el nombre de la tabla `Suppliers` y elija nueva consulta en el menú contextual. Se abrirá una ventana de consulta que le pide que elija las tablas que desea incluir en la consulta. Agregue el `Suppliers` tabla y haga clic en cerrar. A continuación, compruebe las columnas `CompanyName`, `ContactName`, `ContactTitle`y `FullContactName` de la tabla proveedores. Por último, haga clic en el icono de signo de exclamación rojo en la barra de herramientas para ejecutar la consulta y ver los resultados.

Como se muestra en la figura 2, los resultados incluyen `FullContactName`, que enumera las columnas `CompanyName`, `ContactName`y `ContactTitle` con el formato ldquo;`ContactName` (`ContactTitle`, `CompanyName`).

[![FullContactName usa el formato ContactName (del que se va a usar, CompanyName)](working-with-computed-columns-cs/_static/image5.png)](working-with-computed-columns-cs/_static/image4.png)

**Figura 2**: el `FullContactName` usa el formato `ContactName` (`ContactTitle`, `CompanyName`) ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image6.png))

## <a name="step-3-adding-thesupplierstableadapterto-the-data-access-layer"></a>Paso 3: agregar el`SuppliersTableAdapter`a la capa de acceso a datos

Para trabajar con la información del proveedor en nuestra aplicación, es necesario crear primero un TableAdapter y DataTable en la capa DAL. Idealmente, esto se llevaría a cabo con los mismos pasos sencillos examinados en los tutoriales anteriores. Sin embargo, trabajar con columnas calculadas presenta algunas arrugas que merecen la explicación.

Si utiliza un TableAdapter que usa instrucciones SQL ad hoc, puede incluir simplemente la columna calculada en la consulta principal de TableAdapter s a través del Asistente para configuración de TableAdapter. Sin embargo, esto generará automáticamente `INSERT` y `UPDATE` instrucciones que incluyan la columna calculada. Si intenta ejecutar uno de estos métodos, una `SqlException` con el mensaje la columna *columnName* no se puede modificar porque es una columna calculada o es el resultado de un operador Union. Aunque la instrucción `INSERT` y `UPDATE` se puede ajustar manualmente mediante las propiedades `InsertCommand` y `UpdateCommand` de TableAdapter, se perderán estas personalizaciones cada vez que se vuelva a ejecutar el Asistente para configuración de TableAdapter.

Debido a la fragilidad de los TableAdapters que usan instrucciones SQL ad hoc, se recomienda usar procedimientos almacenados al trabajar con columnas calculadas. Si está utilizando procedimientos almacenados existentes, simplemente configure el TableAdapter como se describe en el tutorial [uso de procedimientos almacenados existentes para los TableAdapters del conjunto de](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) elementos con tipo. Sin embargo, si el Asistente para TableAdapter crea los procedimientos almacenados, es importante omitir inicialmente las columnas calculadas de la consulta principal. Si incluye una columna calculada en la consulta principal, el Asistente para configuración de TableAdapter le informará de que, al finalizar, no podrá crear los procedimientos almacenados correspondientes. En Resumen, es necesario configurar inicialmente el TableAdapter con una consulta principal sin columnas calculadas y, a continuación, actualizar manualmente el procedimiento almacenado correspondiente y el `SelectCommand` TableAdapter s para incluir la columna calculada. Este enfoque es similar al que se usa en el tutorial [actualizar el TableAdapter para usar](updating-the-tableadapter-to-use-joins-cs.md)`JOIN`*s* .

En este tutorial, vamos a agregar un nuevo TableAdapter y hacer que cree automáticamente los procedimientos almacenados para nosotros. Por lo tanto, es necesario omitir inicialmente la `FullContactName` columna calculada de la consulta principal.

Para empezar, abra el conjunto de `NorthwindWithSprocs` en la carpeta `~/App_Code/DAL`. Haga clic con el botón secundario en el diseñador y, en el menú contextual, elija Agregar un nuevo TableAdapter. Se iniciará el Asistente para la configuración de TableAdapter. Especifique la base de datos de la que se van a consultar los datos (`NORTHWNDConnectionString` de `Web.config`) y haga clic en siguiente. Puesto que aún no hemos creado ningún procedimiento almacenado para consultar o modificar la tabla `Suppliers`, seleccione la opción crear nuevos procedimientos almacenados para que el asistente las cree para nosotros y haga clic en siguiente.

[![elegir la opción crear nuevos procedimientos almacenados](working-with-computed-columns-cs/_static/image8.png)](working-with-computed-columns-cs/_static/image7.png)

**Figura 3**: elija la opción crear nuevos procedimientos almacenados ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image9.png))

El paso siguiente nos solicita la consulta principal. Escriba la siguiente consulta, que devuelve las columnas `SupplierID`, `CompanyName`, `ContactName`y `ContactTitle` de cada proveedor. Tenga en cuenta que esta consulta omite intencionadamente la columna calculada (`FullContactName`); actualizaremos el procedimiento almacenado correspondiente para incluir esta columna en el paso 4.

[!code-sql[Main](working-with-computed-columns-cs/samples/sample3.sql)]

Después de escribir la consulta principal y hacer clic en siguiente, el asistente nos permite nombrar los cuatro procedimientos almacenados que generará. Asigne a estos procedimientos almacenados el nombre `Suppliers_Select`, `Suppliers_Insert`, `Suppliers_Update`y `Suppliers_Delete`, como se muestra en la figura 4.

[![personalizar los nombres de los procedimientos almacenados generados automáticamente](working-with-computed-columns-cs/_static/image11.png)](working-with-computed-columns-cs/_static/image10.png)

**Figura 4**: personalización de los nombres de los procedimientos almacenados generados automáticamente ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image12.png))

El siguiente paso del asistente nos permite asignar un nombre a los métodos de TableAdapter y especificar los patrones que se usan para obtener acceso a los datos y actualizarlos. Deje activadas las tres casillas, pero cambie el nombre del método `GetData` a `GetSuppliers`. Haga clic en Finalizar para completar el asistente.

[![cambiar el nombre del método GetData a GetSuppliers](working-with-computed-columns-cs/_static/image14.png)](working-with-computed-columns-cs/_static/image13.png)

**Figura 5**: cambiar el nombre del método `GetData` a `GetSuppliers` ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image15.png))

Al hacer clic en finalizar, el asistente creará los cuatro procedimientos almacenados y agregará el TableAdapter y el DataTable correspondiente al conjunto de DataSet con tipo.

## <a name="step-4-including-the-computed-column-in-the-tableadapter-s-main-query"></a>Paso 4: incluir la columna calculada en la consulta principal de TableAdapter s

Ahora es necesario actualizar el TableAdapter y el DataTable creados en el paso 3 para incluir la `FullContactName` columna calculada. Esto conlleva dos pasos:

1. Actualizar el `Suppliers_Select` procedimiento almacenado para devolver el `FullContactName` columna calculada y
2. Actualizar DataTable para incluir una columna de `FullContactName` correspondiente.

Para empezar, vaya a la Explorador de servidores y explore en profundidad la carpeta procedimientos almacenados. Abra el `Suppliers_Select` procedimiento almacenado y actualice el `SELECT` consulta para incluir la `FullContactName` columna calculada:

[!code-sql[Main](working-with-computed-columns-cs/samples/sample4.sql)]

Guarde los cambios realizados en el procedimiento almacenado; para ello, haga clic en el icono guardar de la barra de herramientas, presione Ctrl + S o elija la opción Guardar `Suppliers_Select` en el menú archivo.

A continuación, vuelva al diseñador de DataSet, haga clic con el botón derecho en el `SuppliersTableAdapter`y elija configurar en el menú contextual. Tenga en cuenta que la columna `Suppliers_Select` incluye ahora la columna `FullContactName` en su colección de columnas de datos.

[![ejecutar el Asistente para configuración de TableAdapter s para actualizar las columnas DataTable s](working-with-computed-columns-cs/_static/image17.png)](working-with-computed-columns-cs/_static/image16.png)

**Figura 6**: ejecutar el Asistente para la configuración de TableAdapter s para actualizar las columnas DataTable s ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image18.png))

Haga clic en Finalizar para completar el asistente. Esto agregará automáticamente una columna correspondiente a la `SuppliersDataTable`. El Asistente para TableAdapter es lo suficientemente inteligente como para detectar que el `FullContactName` columna es una columna calculada y, por tanto, es de solo lectura. Por consiguiente, establece la propiedad Column s `ReadOnly` en `true`. Para comprobarlo, seleccione la columna en el `SuppliersDataTable` y, a continuación, vaya al ventana Propiedades (consulte la figura 7). Tenga en cuenta que las propiedades `FullContactName` Column s `DataType` y `MaxLength` también se establecen como corresponda.

[![la columna FullContactName está marcada como de solo lectura](working-with-computed-columns-cs/_static/image20.png)](working-with-computed-columns-cs/_static/image19.png)

**Figura 7**: el `FullContactName` columna está marcado como de solo lectura ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image21.png))

## <a name="step-5-adding-agetsupplierbysupplieridmethod-to-the-tableadapter"></a>Paso 5: agregar un método`GetSupplierBySupplierID`a TableAdapter

En este tutorial, se creará una página ASP.NET que muestra los proveedores en una cuadrícula actualizable. En los tutoriales anteriores hemos actualizado un solo registro de la capa de lógica de negocios mediante la recuperación de ese registro concreto de la capa DAL como DataTable fuertemente tipada, la actualización de sus propiedades y, a continuación, el envío de la DataTable actualizada de nuevo a la capa DAL para propagar los cambios a la base de datos. Para realizar este primer paso: recuperar el registro que se está actualizando desde la capa DAL, primero es necesario agregar un método `GetSupplierBySupplierID(supplierID)` a la capa DAL.

Haga clic con el botón derecho en el `SuppliersTableAdapter` en el diseño del conjunto de DataSet y elija la opción Agregar consulta en el menú contextual. Como hicimos en el paso 3, deje que el asistente genere un nuevo procedimiento almacenado para nosotros seleccionando la opción Crear nuevo procedimiento almacenado (consulte la figura 3 para obtener una captura de pantalla de este paso del asistente). Dado que este método devolverá un registro con varias columnas, indique que deseamos usar una consulta SQL que sea una selección que devuelva filas y haga clic en siguiente.

[![elegir la opción seleccionar qué devuelve filas](working-with-computed-columns-cs/_static/image23.png)](working-with-computed-columns-cs/_static/image22.png)

**Figura 8**: elija la opción seleccionar qué devuelve las filas ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image24.png))

El paso siguiente nos solicita la consulta que se va a usar para este método. Escriba lo siguiente, que devuelve los mismos campos de datos que la consulta principal, pero para un proveedor determinado.

[!code-sql[Main](working-with-computed-columns-cs/samples/sample5.sql)]

La siguiente pantalla nos pide asignar un nombre al procedimiento almacenado que se generará automáticamente. Asigne a este procedimiento almacenado el nombre `Suppliers_SelectBySupplierID` y haga clic en siguiente.

[![el nombre del procedimiento almacenado Suppliers_SelectBySupplierID](working-with-computed-columns-cs/_static/image26.png)](working-with-computed-columns-cs/_static/image25.png)

**Figura 9**: asigne un nombre al procedimiento almacenado `Suppliers_SelectBySupplierID` ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image27.png))

Por último, el asistente nos pide los patrones de acceso a datos y los nombres de método que se usarán para el TableAdapter. Deje activadas las dos casillas, pero cambie el nombre de los métodos `FillBy` y `GetDataBy` a `FillBySupplierID` y `GetSupplierBySupplierID`, respectivamente.

[![el nombre de los métodos de TableAdapter FillBySupplierID y GetSupplierBySupplierID](working-with-computed-columns-cs/_static/image29.png)](working-with-computed-columns-cs/_static/image28.png)

**Figura 10**: asigne un nombre a los métodos de TableAdapter `FillBySupplierID` y `GetSupplierBySupplierID` ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image30.png))

Haga clic en Finalizar para completar el asistente.

## <a name="step-6-creating-the-business-logic-layer"></a>Paso 6: crear el nivel de lógica de negocios

Antes de crear una página ASP.NET que usa la columna calculada creada en el paso 1, primero debemos agregar los métodos correspondientes en el BLL. Nuestra página de ASP.NET, que crearemos en el paso 7, permitirá a los usuarios ver y editar proveedores. Por lo tanto, necesitamos que el BLL proporcione, como mínimo, un método para obtener todos los proveedores y otro para actualizar un proveedor determinado.

Cree un nuevo archivo de clase denominado `SuppliersBLLWithSprocs` en la carpeta `~/App_Code/BLL` y agregue el código siguiente:

[!code-csharp[Main](working-with-computed-columns-cs/samples/sample6.cs)]

Al igual que las demás clases de BLL, `SuppliersBLLWithSprocs` tiene una propiedad `protected` `Adapter` que devuelve una instancia de la clase `SuppliersTableAdapter` junto con dos métodos `public`: `GetSuppliers` y `UpdateSupplier`. El método `GetSuppliers` llama a y devuelve el `SuppliersDataTable` devuelto por el método `GetSupplier` correspondiente en la capa de acceso a datos. El método `UpdateSupplier` recupera información sobre el proveedor determinado que se está actualizando a través de una llamada al método de `GetSupplierBySupplierID(supplierID)` de DAL. A continuación, actualiza las propiedades `CategoryName`, `ContactName`y `ContactTitle` y confirma estos cambios en la base de datos llamando al método Data Access Layer s `Update`, pasando el objeto `SuppliersRow` modificado.

> [!NOTE]
> A excepción de `SupplierID` y `CompanyName`, todas las columnas de la tabla Suppliers permiten valores `NULL`. Por lo tanto, si los parámetros pass-in `contactName` o `contactTitle` son `null` es necesario establecer las propiedades `ContactName` y `ContactTitle` correspondientes en un valor de base de datos `NULL` mediante los métodos `SetContactNameNull` y `SetContactTitleNull`, respectivamente.

## <a name="step-7-working-with-the-computed-column-from-the-presentation-layer"></a>Paso 7: trabajar con la columna calculada desde el nivel de presentación

Con la columna calculada agregada a la tabla de `Suppliers` y la capa DAL y BLL actualizada en consecuencia, estamos preparados para compilar una página ASP.NET que funcione con la `FullContactName` columna calculada. Comience abriendo la página `ComputedColumns.aspx` en la carpeta `AdvancedDAL` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador. Establezca la propiedad GridView s `ID` en `Suppliers` y, desde su etiqueta inteligente, enlácelo a un nuevo ObjectDataSource denominado `SuppliersDataSource`. Configure el origen de datos para que use la clase `SuppliersBLLWithSprocs` que hemos agregado en el paso 6 y haga clic en siguiente.

[![configurar ObjectDataSource para usar la clase SuppliersBLLWithSprocs](working-with-computed-columns-cs/_static/image32.png)](working-with-computed-columns-cs/_static/image31.png)

**Figura 11**: configuración de ObjectDataSource para usar la clase `SuppliersBLLWithSprocs` ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image33.png))

Solo hay dos métodos definidos en la clase `SuppliersBLLWithSprocs`: `GetSuppliers` y `UpdateSupplier`. Asegúrese de que estos dos métodos se especifican en las pestañas seleccionar y actualizar, respectivamente, y haga clic en finalizar para completar la configuración de ObjectDataSource.

Una vez finalizado el Asistente para la configuración de orígenes de datos, Visual Studio agregará BoundField para cada uno de los campos de datos devueltos. Quite el `SupplierID` BoundField y cambie las propiedades `HeaderText` de los `CompanyName`, `ContactName`, `ContactTitle`y `FullContactName` BoundFields a Company, nombre de contacto, título y nombre de contacto completo, respectivamente. En la etiqueta inteligente, active la casilla habilitar edición para activar las funciones de edición integradas de GridView.

Además de agregar BoundFields a GridView, la finalización del Asistente para orígenes de datos también hace que Visual Studio establezca la propiedad ObjectDataSource s `OldValuesParameterFormatString` en el {0}de\_original. Vuelva a revertir esta configuración a su valor predeterminado, {0}.

Después de hacer estas modificaciones en GridView e ObjectDataSource, su marcado declarativo debe ser similar al siguiente:

[!code-aspx[Main](working-with-computed-columns-cs/samples/sample7.aspx)]

A continuación, visite esta página a través de un explorador. Como se muestra en la figura 12, cada proveedor aparece en una cuadrícula que incluye el `FullContactName` columna, cuyo valor es simplemente la concatenación de las otras tres columnas con el formato `ContactName` (`ContactTitle`, `CompanyName`).

[![cada proveedor aparece en la cuadrícula](working-with-computed-columns-cs/_static/image35.png)](working-with-computed-columns-cs/_static/image34.png)

**Figura 12**: cada proveedor aparece en la cuadrícula ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image36.png))

Al hacer clic en el botón Editar de un proveedor determinado, se genera un postback y esa fila se representa en la interfaz de edición (vea la figura 13). Las tres primeras columnas se representan en su interfaz de edición predeterminada: un control de cuadro de texto cuya propiedad `Text` está establecida en el valor del campo de datos. Sin embargo, la columna `FullContactName` permanece como texto. Cuando el BoundFields se agregó a GridView al finalizar el Asistente para la configuración de orígenes de datos, la propiedad `FullContactName` BoundField s `ReadOnly` se estableció en `true` porque la columna de `FullContactName` correspondiente del `SuppliersDataTable` tiene su propiedad `ReadOnly` establecida en `true`. Como se indicó en el paso 4, la propiedad `FullContactName` s `ReadOnly` se estableció en `true` porque el TableAdapter detectó que la columna era una columna calculada.

[![la columna FullContactName no es editable](working-with-computed-columns-cs/_static/image38.png)](working-with-computed-columns-cs/_static/image37.png)

**Figura 13**: la columna `FullContactName` no es editable ([haga clic para ver la imagen de tamaño completo](working-with-computed-columns-cs/_static/image39.png))

Continúe y actualice el valor de una o varias columnas editables y haga clic en actualizar. Observe cómo se actualiza automáticamente el valor de `FullContactName` s para reflejar el cambio.

> [!NOTE]
> GridView actualmente usa BoundFields para los campos modificables, lo que da lugar a la interfaz de edición predeterminada. Dado que el campo `CompanyName` es obligatorio, debe convertirse en TemplateField que incluya un RequiredFieldValidator. Dejo esto como un ejercicio para el lector interesado. Consulte el tutorial [Agregar controles de validación al editor de interfaces de edición e inserción](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md) para obtener instrucciones paso a paso sobre cómo convertir un BoundField en TemplateField y agregar controles de validación.

## <a name="summary"></a>Resumen

Al definir el esquema de una tabla, Microsoft SQL Server permite la inclusión de columnas calculadas. Se trata de columnas cuyos valores se calculan a partir de una expresión que normalmente hace referencia a los valores de otras columnas del mismo registro. Dado que los valores de las columnas calculadas se basan en una expresión, son de solo lectura y no se puede asignar un valor a una instrucción `INSERT` o `UPDATE`. Esto presenta desafíos cuando se usa una columna calculada en la consulta principal de un TableAdapter que intenta generar automáticamente las instrucciones `INSERT`, `UPDATE`y `DELETE` correspondientes.

En este tutorial se han analizado técnicas para eludir los desafíos planteados por las columnas calculadas. En concreto, usamos procedimientos almacenados en el TableAdapter para superar la fragilidad inherente a los TableAdapters que usan instrucciones SQL ad hoc. Cuando el Asistente para TableAdapter crea nuevos procedimientos almacenados, es importante que la consulta principal omita inicialmente las columnas calculadas porque su presencia impide que se generen los procedimientos almacenados de modificación de datos. Una vez configurado inicialmente el TableAdapter, su `SelectCommand` procedimiento almacenado se puede volver a usar para incluir cualquier columna calculada.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Hilton Geisenow y Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](adding-additional-datatable-columns-cs.md)
> [Siguiente](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
