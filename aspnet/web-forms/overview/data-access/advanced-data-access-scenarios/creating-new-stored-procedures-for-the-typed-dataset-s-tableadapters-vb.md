---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
title: Crear nuevos procedimientos almacenados para los TableAdapters del conjunto de de tipos (VB) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores se han creado instrucciones SQL en el código y se han pasado las instrucciones a la base de datos que se va a ejecutar. Un enfoque alternativo es usar...
ms.author: riande
ms.date: 07/18/2007
ms.assetid: a5a4a9ba-d18d-489a-a6b0-a3c26d6b0274
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb
msc.type: authoredcontent
ms.openlocfilehash: a7cc890038e5bb4eb61c7c3b808154c196ab2423
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78428713"
---
# <a name="creating-new-stored-procedures-for-the-typed-datasets-tableadapters-vb"></a>Crear procedimientos almacenados para los TableAdapters del conjunto de datos con tipo (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_67_VB.zip) o [Descargar PDF](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/datatutorial67vb1.pdf)

> En los tutoriales anteriores se han creado instrucciones SQL en el código y se han pasado las instrucciones a la base de datos que se va a ejecutar. Un enfoque alternativo es usar procedimientos almacenados, donde las instrucciones SQL están predefinidas en la base de datos. En este tutorial se explica cómo hacer que el Asistente de TableAdapter genere nuevos procedimientos almacenados para nosotros.

## <a name="introduction"></a>Introducción

La capa de acceso a datos (DAL) para estos tutoriales utiliza conjuntos de datos con tipo. Como se describe en el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) , los conjuntos de datos con tipo constan de objetos DataTable y TableAdapter fuertemente tipados. Los objetos DataTable representan las entidades lógicas del sistema, mientras que la interfaz TableAdapters con la base de datos subyacente para realizar el trabajo de acceso a los datos. Esto incluye rellenar las tablas de datos con datos, ejecutar consultas que devuelven datos escalares e insertar, actualizar y eliminar registros de la base de datos.

Los comandos SQL que ejecutan TableAdapter pueden ser instrucciones SQL ad hoc, como `SELECT columnList FROM TableName`o procedimientos almacenados. Los TableAdapters de nuestra arquitectura usan instrucciones SQL ad hoc. Sin embargo, muchos desarrolladores y administradores de bases de datos prefieren procedimientos almacenados en las instrucciones SQL ad hoc por motivos de seguridad, mantenimiento y capacidad de actualización. Otros ardently prefieren instrucciones SQL ad hoc para su flexibilidad. En mi propio trabajo, se favorecen los procedimientos almacenados en las instrucciones SQL ad hoc, pero se decide usar instrucciones SQL ad-hoc para simplificar los tutoriales anteriores.

Al definir un TableAdapter o agregar nuevos métodos, el Asistente para TableAdapter hace que sea tan fácil crear nuevos procedimientos almacenados o usar procedimientos almacenados existentes, ya que se usan instrucciones SQL ad hoc. En este tutorial, examinaremos cómo hacer que el Asistente de TableAdapter s genere automáticamente los procedimientos almacenados. En el siguiente tutorial, veremos cómo configurar los métodos de TableAdapter s para usar procedimientos almacenados existentes o creados manualmente.

> [!NOTE]
> Vea la entrada de blog de Rob Howard ¿no [usar todavía los procedimientos almacenados?](http://grokable.com/2003/11/dont-use-stored-procedures-yet-must-be-suffering-from-nihs-not-invented-here-syndrome/) y los procedimientos almacenados de la entrada de blog de [Frans Bouma](https://weblogs.asp.net/fbouma/) s [son incorrectos, ¿M Kay?](https://weblogs.asp.net/fbouma/archive/2003/11/18/38178.aspx) para un debate animado sobre las ventajas y desventajas de los procedimientos almacenados y SQL ad hoc.

## <a name="stored-procedure-basics"></a>Conceptos básicos de los procedimientos almacenados

Las funciones son una construcción común a todos los lenguajes de programación. Una función es una colección de instrucciones que se ejecutan cuando se llama a la función. Las funciones pueden aceptar parámetros de entrada y, opcionalmente, pueden devolver un valor. *[Los procedimientos almacenados](http://en.wikipedia.org/wiki/Stored_procedure)* son construcciones de bases de datos que comparten muchas similitudes con las funciones de los lenguajes de programación. Un procedimiento almacenado se compone de un conjunto de instrucciones T-SQL que se ejecutan cuando se llama al procedimiento almacenado. Un procedimiento almacenado puede aceptar cero y muchos parámetros de entrada y puede devolver valores escalares, parámetros de salida o, normalmente, conjuntos de resultados de `SELECT` consultas.

> [!NOTE]
> A menudo, los procedimientos almacenados se conocen como procedimientos almacenados o SP.

Los procedimientos almacenados se crean mediante el [`CREATE PROCEDURE`](https://msdn.microsoft.com/library/aa258259(SQL.80).aspx) instrucción t-SQL. Por ejemplo, el siguiente script T-SQL crea un procedimiento almacenado denominado `GetProductsByCategoryID` que toma un único parámetro denominado `@CategoryID` y devuelve los campos `ProductID`, `ProductName`, `UnitPrice`y `Discontinued` de las columnas de la tabla `Products` que tienen un valor `CategoryID` coincidente:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample1.sql)]

Una vez creado este procedimiento almacenado, se puede llamar con la siguiente sintaxis:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample2.sql)]

> [!NOTE]
> En el tutorial siguiente, examinaremos la creación de procedimientos almacenados a través del IDE de Visual Studio. En este tutorial, sin embargo, vamos a permitir que el Asistente para TableAdapter genere automáticamente los procedimientos almacenados para nosotros.

Además de devolver simplemente los datos, los procedimientos almacenados suelen usarse para realizar varios comandos de base de datos dentro del ámbito de una única transacción. Por ejemplo, un procedimiento almacenado denominado `DeleteCategory`podría tomar un parámetro `@CategoryID` y realizar dos instrucciones `DELETE`: primero, una para eliminar los productos relacionados y otra que elimina la categoría especificada. Varias instrucciones dentro de un procedimiento almacenado *no* se ajustan automáticamente dentro de una transacción. Es necesario emitir comandos T-SQL adicionales para asegurarse de que el procedimiento almacenado s varios comandos se tratan como una operación atómica. Veremos cómo ajustar los comandos de un procedimiento almacenado en el ámbito de una transacción en el tutorial siguiente.

Cuando se usan procedimientos almacenados en una arquitectura, los métodos de capa de acceso a datos llaman a un procedimiento almacenado determinado en lugar de emitir una instrucción SQL ad hoc. Esto centraliza la ubicación de las instrucciones SQL ejecutadas (en la base de datos) en lugar de definirlas en la arquitectura de la aplicación. Esta centralización podría facilitar la búsqueda, el análisis y la optimización de las consultas y proporciona una imagen mucho más clara sobre dónde y cómo se utiliza la base de datos.

Para obtener más información sobre los conceptos básicos de los procedimientos almacenados, consulte los recursos en la sección de lecturas adicionales al final de este tutorial.

## <a name="step-1-creating-the-advanced-data-access-layer-scenarios-web-pages"></a>Paso 1: creación de las páginas web de escenarios avanzados de nivel de acceso a datos

Antes de comenzar nuestro debate sobre la creación de una capa DAL con procedimientos almacenados, vamos a crear las páginas de ASP.NET en nuestro proyecto de sitio web que necesitaremos para esto y los siguientes tutoriales. Comience agregando una nueva carpeta denominada `AdvancedDAL`. A continuación, agregue las siguientes páginas de ASP.NET a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `NewSprocs.aspx`
- `ExistingSprocs.aspx`
- `JOINs.aspx`
- `AddingColumns.aspx`
- `ComputedColumns.aspx`
- `EncryptingConfigSections.aspx`
- `ManagedFunctionsAndSprocs.aspx`

![Agregue las páginas ASP.NET para los tutoriales avanzados de escenarios de nivel de acceso a datos](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image1.png)

**Figura 1**: incorporación de las páginas de ASP.net para los tutoriales avanzados de escenarios de nivel de acceso a datos

Al igual que en las otras carpetas, `Default.aspx` en la carpeta `AdvancedDAL` mostrará los tutoriales en su sección. Recuerde que el control de usuario `SectionLevelTutorialListing.ascx` proporciona esta funcionalidad. Por lo tanto, agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta la página s Vista de diseño.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image3.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image2.png)

**Figura 2**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image4.png))

Por último, agregue estas páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después de trabajar con los datos por lotes `<siteMapNode>`:

[!code-xml[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample3.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para los tutoriales avanzados de escenarios de DAL.

![El mapa del sitio ahora incluye entradas para los tutoriales avanzados de escenarios de DAL](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image5.png)

**Figura 3**: ahora, el mapa del sitio incluye entradas para los tutoriales avanzados de escenarios de dal

## <a name="step-2-configuring-a-tableadapter-to-create-new-stored-procedures"></a>Paso 2: configurar un TableAdapter para crear nuevos procedimientos almacenados

Para mostrar la creación de una capa de acceso a datos que use procedimientos almacenados en lugar de instrucciones SQL ad hoc, cree un nuevo conjunto de datos con tipo en la carpeta `~/App_Code/DAL` denominada `NorthwindWithSprocs.xsd`. Como hemos realizado paso a paso este proceso en detalle en los tutoriales anteriores, continuaremos rápidamente a través de los pasos que se describen aquí. Si se bloquea o necesita instrucciones paso a paso para crear y configurar un conjunto de datos con tipo, consulte el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) .

Agregue un nuevo conjunto de elementos al proyecto; para ello, haga clic con el botón derecho en la carpeta `DAL`, elija Agregar nuevo elemento y seleccione la plantilla de conjunto de elementos como se muestra en la figura 4.

[![agregar un nuevo conjunto de tipos al proyecto denominado NorthwindWithSprocs. xsd](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image7.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image6.png)

**Ilustración 4**: agregar un nuevo conjunto de información con tipo al proyecto denominado `NorthwindWithSprocs.xsd` ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image8.png))

Se creará el nuevo conjunto de los tipos, se abrirá su diseñador, se creará un nuevo TableAdapter y se iniciará el Asistente para configuración de TableAdapter. El primer paso del Asistente para la configuración de TableAdapter nos pide que seleccione la base de datos con la que trabajar. La cadena de conexión a la base de datos Northwind debe aparecer en la lista desplegable. Seleccione esta casilla y haga clic en siguiente.

En la siguiente pantalla podemos elegir cómo el TableAdapter debe tener acceso a la base de datos. En los tutoriales anteriores, seleccionamos la primera opción, usar instrucciones SQL. En este tutorial, seleccione la segunda opción, crear nuevos procedimientos almacenados y haga clic en siguiente.

[![indicar al TableAdapter que cree nuevos procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image10.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image9.png)

**Figura 5**: indicar al TableAdapter que cree nuevos procedimientos almacenados ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image11.png))

Al igual que con las instrucciones SQL ad hoc, en el paso siguiente se le pedirá que proporcione la instrucción `SELECT` para la consulta principal de TableAdapter s. Pero en lugar de usar la instrucción `SELECT` especificada aquí para realizar una consulta ad hoc directamente, el Asistente para TableAdapter creará un procedimiento almacenado que contendrá esta `SELECT` consulta.

Use la siguiente consulta de `SELECT` para este TableAdapter:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample4.sql)]

[![especificar la consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image13.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image12.png)

**Figura 6**: escriba la `SELECT` consulta ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image14.png))

> [!NOTE]
> La consulta anterior difiere ligeramente de la consulta principal del `ProductsTableAdapter` en el `Northwind` DataSet con tipo. Recuerde que el `ProductsTableAdapter` en el `Northwind` conjunto de datos con tipo incluye dos subconsultas correlacionadas para devolver el nombre de la categoría y el nombre de la compañía de cada categoría de producto y proveedor. En la próxima [actualización del tutorial de TableAdapter para usar combinaciones](updating-the-tableadapter-to-use-joins-vb.md) , veremos agregar estos datos relacionados a este TableAdapter.

Dedique un momento a hacer clic en el botón Opciones avanzadas. Aquí podemos especificar si el asistente debe generar también instrucciones INSERT, Update y DELETE para el TableAdapter, si se va a usar la simultaneidad optimista y si la tabla de datos debe actualizarse después de las inserciones y las actualizaciones. La opción generar instrucciones INSERT, Update y DELETE está activada de forma predeterminada. Déjela activada. En este tutorial, deje desactivadas las opciones usar simultaneidad optimista.

Cuando el Asistente para TableAdapter crea automáticamente los procedimientos almacenados, parece que se omite la opción actualizar la tabla de datos. Independientemente de si esta casilla está activada, los procedimientos almacenados de inserción y actualización resultantes recuperan el registro recién insertado o el que se acaba de actualizar, como veremos en el paso 3.

![Deje activada la opción generar instrucciones INSERT, Update y DELETE](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image15.png)

**Figura 7**: dejar activada la opción generar instrucciones INSERT, Update y DELETE

> [!NOTE]
> Si la opción usar simultaneidad optimista está activada, el asistente agregará condiciones adicionales a la cláusula `WHERE` que impiden que los datos se actualicen si se produjeron cambios en otros campos. Consulte el tutorial [implementación de simultaneidad optimista](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-vb.md) para obtener más información sobre el uso de la característica de control de simultaneidad optimista integrada de TableAdapter s.

Después de escribir la consulta de `SELECT` y confirmar que la opción generar instrucciones INSERT, Update y DELETE está activada, haga clic en siguiente. En la siguiente pantalla, que se muestra en la figura 8, se solicitan los nombres de los procedimientos almacenados que el asistente creará para seleccionar, insertar, actualizar y eliminar datos. Cambie estos nombres de procedimientos almacenados a `Products_Select`, `Products_Insert`, `Products_Update`y `Products_Delete`.

[![cambiar el nombre de los procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image17.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image16.png)

**Figura 8**: cambiar el nombre de los procedimientos almacenados ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image18.png))

Para ver el T-SQL que utilizará el Asistente para TableAdapter para crear los cuatro procedimientos almacenados, haga clic en el botón vista previa de script SQL. En el cuadro de diálogo vista previa de script SQL, puede guardar el script en un archivo o copiarlo en el portapapeles.

![Obtener una vista previa del script SQL usado para generar los procedimientos almacenados](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image19.png)

**Figura 9**: vista previa del script SQL usado para generar los procedimientos almacenados

Después de nombrar los procedimientos almacenados, haga clic en siguiente para asignar un nombre a los métodos correspondientes de TableAdapter s. Al igual que cuando se usan instrucciones SQL ad hoc, se pueden crear métodos que rellenen un objeto DataTable existente o devuelvan uno nuevo. También podemos especificar si el TableAdapter debe incluir el patrón DB-Direct para insertar, actualizar y eliminar registros. Deje activadas las tres casillas, pero cambie el nombre del método de devolución de DataTable a `GetProducts` (como se muestra en la figura 10).

[![nombre de los métodos Fill y GetProducts](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image21.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image20.png)

**Figura 10**: asigne un nombre a los métodos `Fill` y `GetProducts` ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image22.png))

Haga clic en siguiente para ver un resumen de los pasos que realizará el asistente. Para completar el asistente, haga clic en el botón Finalizar. Una vez finalizado el asistente, se le devolverá al diseñador de DataSet s, que ahora debería incluir el `ProductsDataTable`.

[![el diseñador de DataSet s muestra el ProductsDataTable recién agregado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image24.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image23.png)

**Figura 11**: el diseñador de DataSet s muestra el `ProductsDataTable` recién agregado ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image25.png))

## <a name="step-3-examining-the-newly-created-stored-procedures"></a>Paso 3: examinar los procedimientos almacenados recién creados

El Asistente para TableAdapter usado en el paso 2 creó automáticamente los procedimientos almacenados para seleccionar, insertar, actualizar y eliminar datos. Estos procedimientos almacenados se pueden ver o modificar mediante Visual Studio; para ello, vaya a la Explorador de servidores y explore en profundidad la carpeta de procedimientos almacenados de la base de datos. Como se muestra en la figura 12, la base de datos Northwind contiene cuatro procedimientos almacenados nuevos: `Products_Delete`, `Products_Insert`, `Products_Select`y `Products_Update`.

![Los cuatro procedimientos almacenados creados en el paso 2 se pueden encontrar en la carpeta de procedimientos almacenados de la base de datos.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image26.png)

**Figura 12**: los cuatro procedimientos almacenados creados en el paso 2 se pueden encontrar en la carpeta de procedimientos almacenados de la base de datos.

> [!NOTE]
> Si no ve el Explorador de servidores, vaya al menú Ver y elija la opción Explorador de servidores. Si no ve los procedimientos almacenados relacionados con el producto agregados en el paso 2, intente hacer clic con el botón derecho en la carpeta procedimientos almacenados y elija actualizar.

Para ver o modificar un procedimiento almacenado, haga doble clic en su nombre en la Explorador de servidores o bien, también puede hacer clic con el botón secundario en el procedimiento almacenado y elegir abrir. En la figura 13 se muestra el `Products_Delete` procedimiento almacenado, cuando se abre.

[![procedimientos almacenados se pueden abrir y modificar desde Visual Studio](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image28.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image27.png)

**Figura 13**: los procedimientos almacenados se pueden abrir y modificar desde Visual Studio ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image29.png))

El contenido de los procedimientos almacenados `Products_Delete` y `Products_Select` es bastante sencillo. Por otro lado, los procedimientos almacenados `Products_Insert` y `Products_Update` garantizan una inspección más detallada, ya que ambos realizan una instrucción `SELECT` después de sus instrucciones `INSERT` y `UPDATE`. Por ejemplo, el siguiente código SQL constituye el `Products_Insert` procedimiento almacenado:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample5.sql)]

El procedimiento almacenado acepta como parámetros de entrada las `Products` columnas devueltas por la consulta `SELECT` especificada en el Asistente de TableAdapter s y estos valores se utilizan en una instrucción `INSERT`. Después de la instrucción `INSERT`, se usa una consulta `SELECT` para devolver los valores de columna `Products` (incluido el `ProductID`) del registro que se acaba de agregar. Esta capacidad de actualización es útil al agregar un nuevo registro mediante el patrón de actualización por lotes, ya que actualiza automáticamente las instancias de `ProductRow` recién agregadas `ProductID` las propiedades con los valores de incremento automático asignados por la base de datos.

En el código siguiente se muestra esta característica. Contiene un `ProductsTableAdapter` y `ProductsDataTable` creados para el `NorthwindWithSprocs` DataSet con tipo. Para agregar un nuevo producto a la base de datos, se crea una instancia de `ProductsRow`, se proporcionan sus valores y se llama al método `Update` de TableAdapter, pasando el `ProductsDataTable`. Internamente, el método TableAdapter s `Update` enumera las instancias de `ProductsRow` en la DataTable pasada (en este ejemplo solo hay una-la que acabamos de agregar) y realiza el comando de inserción, actualización o eliminación adecuado. En este caso, se ejecuta el `Products_Insert` procedimiento almacenado, que agrega un nuevo registro a la tabla `Products` y devuelve los detalles del registro que se acaba de agregar. A continuación, se actualiza el valor `ProductID` de la instancia de `ProductsRow`. Una vez completado el método de `Update`, podemos tener acceso al valor de `ProductID` de registros recién agregado a través de la propiedad `ProductsRow` s `ProductID`.

[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample6.vb)]

El procedimiento almacenado `Products_Update` incluye similar una instrucción `SELECT` después de la instrucción `UPDATE`.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample7.sql)]

Tenga en cuenta que este procedimiento almacenado incluye dos parámetros de entrada para `ProductID`: `@Original_ProductID` y `@ProductID`. Esta funcionalidad permite escenarios en los que se puede cambiar la clave principal. Por ejemplo, en una base de datos de empleados, cada registro de empleado podría usar el número de la seguridad social del empleado como su clave principal. Para cambiar el número de la seguridad social de un empleado existente, se debe proporcionar tanto el nuevo número de la seguridad social como el original. En el caso de la tabla `Products`, esta funcionalidad no es necesaria porque la `ProductID` columna es una columna `IDENTITY` y nunca debe cambiarse. De hecho, la instrucción `UPDATE` del procedimiento almacenado `Products_Update` no incluye la columna `ProductID` en su lista de columnas. Por lo tanto, mientras `@Original_ProductID` se usa en la cláusula `UPDATE` instrucción s `WHERE`, es superflua para la tabla de `Products` y podría reemplazarse por el parámetro `@ProductID`. Al modificar los parámetros de un procedimiento almacenado, es importante que también se actualicen los métodos de TableAdapter que usan ese procedimiento almacenado.

## <a name="step-4-modifying-a-stored-procedure-s-parameters-and-updating-the-tableadapter"></a>Paso 4: modificar los parámetros de un procedimiento almacenado y actualizar el TableAdapter

Dado que el parámetro `@Original_ProductID` es superfluo, deje que lo quite del `Products_Update` procedimiento almacenado de forma conjunta. Abra el `Products_Update` procedimiento almacenado, elimine el parámetro `@Original_ProductID` y, en la cláusula `WHERE` de la instrucción `UPDATE`, cambie el nombre del parámetro usado de `@Original_ProductID` a `@ProductID`. Después de realizar estos cambios, el T-SQL dentro del procedimiento almacenado debe ser similar al siguiente:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample8.sql)]

Para guardar estos cambios en la base de datos, haga clic en el icono guardar en la barra de herramientas o presione Ctrl + S. En este momento, el `Products_Update` procedimiento almacenado no espera un parámetro de entrada `@Original_ProductID`, pero el TableAdapter está configurado para pasar este tipo de parámetro. Puede ver los parámetros que el TableAdapter enviará al `Products_Update` procedimiento almacenado seleccionando el TableAdapter en el diseñador de DataSet, vaya al ventana Propiedades y haciendo clic en los puntos suspensivos de la colección `UpdateCommand` s `Parameters`. Se abrirá el cuadro de diálogo Editor de la colección de parámetros que se muestra en la figura 14.

![El editor de la colección de parámetros muestra los parámetros que se han pasado al procedimiento almacenado Products_Update](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image30.png)

**Figura 14**: el editor de la colección de parámetros muestra los parámetros que se han pasado al procedimiento almacenado `Products_Update`

Puede quitar este parámetro aquí simplemente seleccionando el parámetro `@Original_ProductID` de la lista de miembros y haciendo clic en el botón Quitar.

Como alternativa, puede actualizar los parámetros que se usan para todos los métodos; para ello, haga clic con el botón secundario en el TableAdapter en el diseñador y elija configurar. Se abrirá el Asistente para configuración de TableAdapter, donde se enumeran los procedimientos almacenados que se usan para seleccionar, insertar, actualizar y eliminar, junto con los parámetros que los procedimientos almacenados esperan recibir. Si hace clic en la lista desplegable actualizar, puede ver los parámetros de entrada `Products_Update` procedimientos almacenados esperados, que ahora ya no incluyen `@Original_ProductID` (consulte la figura 15). Simplemente haga clic en finalizar para actualizar automáticamente la colección de parámetros utilizada por el TableAdapter.

[![puede usar el Asistente para la configuración de TableAdapter s para actualizar sus colecciones de parámetros de métodos.](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image32.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image31.png)

**Figura 15**: también puede usar el Asistente para la configuración de TableAdapter s para actualizar sus colecciones de parámetros de métodos ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image33.png))

## <a name="step-5-adding-additional-tableadapter-methods"></a>Paso 5: agregar métodos TableAdapter adicionales

Como se muestra en el paso 2, cuando se crea un nuevo TableAdapter es fácil tener los procedimientos almacenados correspondientes generados automáticamente. Lo mismo sucede al agregar métodos adicionales a un TableAdapter. Para ilustrar esto, vamos a agregar un método de `GetProductByProductID(productID)` al `ProductsTableAdapter` creado en el paso 2. Este método tomará como entrada un valor `ProductID` y devolverá detalles sobre el producto especificado.

Para empezar, haga clic con el botón derecho en el TableAdapter y elija Agregar consulta en el menú contextual.

![Agregar una nueva consulta a TableAdapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image34.png)

**Figura 16**: agregar una nueva consulta a TableAdapter

Esto iniciará el Asistente para la configuración de consultas de TableAdapter, que primero solicita el modo en que el TableAdapter debe tener acceso a la base de datos. Para crear un nuevo procedimiento almacenado, elija la opción crear un nuevo procedimiento almacenado y haga clic en siguiente.

[![elegir la opción crear un nuevo procedimiento almacenado](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image36.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image35.png)

**Figura 17**: elija la opción crear un nuevo procedimiento almacenado ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image37.png))

En la pantalla siguiente se le pide que identifique el tipo de consulta que se va a ejecutar, si devolverá un conjunto de filas o un valor escalar único, o realizará una `UPDATE`, `INSERT`o `DELETE` instrucción. Dado que el método `GetProductByProductID(productID)` devolverá una fila, deje seleccionada la opción seleccionar la fila que devuelve el valor y presione siguiente.

[![elegir la opción seleccionar qué filas devuelve](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image39.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image38.png)

**Figura 18**: elija la opción seleccionar la fila que devuelve ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image40.png))

En la pantalla siguiente se muestra la consulta principal de TableAdapter s, que solo muestra el nombre del procedimiento almacenado (`dbo.Products_Select`). Reemplace el nombre del procedimiento almacenado por la siguiente instrucción de `SELECT`, que devuelve todos los campos de producto de un producto específico:

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample9.sql)]

[![reemplazar el nombre del procedimiento almacenado por una consulta SELECT](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image42.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image41.png)

**Figura 19**: Reemplace el nombre del procedimiento almacenado por un `SELECT` consulta ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image43.png))

En la pantalla siguiente se le pide el nombre del procedimiento almacenado que se va a crear. Escriba el nombre `Products_SelectByProductID` y haga clic en siguiente.

[![nombre al nuevo procedimiento almacenado Products_SelectByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image45.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image44.png)

**Figura 20**: asigne un nombre al nuevo procedimiento almacenado `Products_SelectByProductID` ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image46.png))

El paso final del asistente nos permite cambiar los nombres de método generados, así como indicar si se debe usar el patrón rellenar un DataTable, devolver un modelo DataTable o ambos. Para este método, deje ambas opciones seleccionadas, pero cambie el nombre de los métodos a `FillByProductID` y `GetProductByProductID`. Haga clic en siguiente para ver un resumen de los pasos que el asistente realizará y, a continuación, haga clic en finalizar para completar el asistente.

[![cambiar el nombre de los métodos de TableAdapter s a FillByProductID y GetProductByProductID](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image48.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image47.png)

**Figura 21**: cambiar el nombre de los métodos de TableAdapter s a `FillByProductID` y `GetProductByProductID` ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image49.png))

Después de completar el asistente, el TableAdapter tiene un nuevo método disponible, `GetProductByProductID(productID)` que, cuando se invoca, ejecutará el `Products_SelectByProductID` procedimiento almacenado que se acaba de crear. Dedique un momento a ver este nuevo procedimiento almacenado desde el Explorador de servidores mediante la obtención de detalles en la carpeta procedimientos almacenados y la apertura de `Products_SelectByProductID` (si no lo ve, haga clic con el botón secundario en la carpeta procedimientos almacenados y elija actualizar).

Tenga en cuenta que el procedimiento almacenado `SelectByProductID` toma `@ProductID` como parámetro de entrada y ejecuta la instrucción `SELECT` que se especifica en el asistente.

[!code-sql[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample10.sql)]

## <a name="step-6-creating-a-business-logic-layer-class"></a>Paso 6: crear una clase de capa de lógica de negocios

A lo largo de la serie de tutoriales hemos esforzado para mantener una arquitectura en capas en la que el nivel de presentación realizaba todas sus llamadas a la capa de lógica de negocios (BLL). Para adherirse a esta decisión de diseño, primero es necesario crear una clase BLL para el nuevo conjunto de datos con tipo antes de poder tener acceso a los datos del producto desde el nivel de presentación.

Cree un nuevo archivo de clase denominado `ProductsBLLWithSprocs.vb` en la carpeta `~/App_Code/BLL` y agréguele el código siguiente:

[!code-vb[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample11.vb)]

Esta clase imita la semántica de la clase `ProductsBLL` de los tutoriales anteriores, pero usa los objetos `ProductsTableAdapter` y `ProductsDataTable` del conjunto de `NorthwindWithSprocs`. Por ejemplo, en lugar de tener una instrucción `Imports NorthwindTableAdapters` al principio del archivo de clase como hace `ProductsBLL`, la clase `ProductsBLLWithSprocs` utiliza `Imports NorthwindWithSprocsTableAdapters`. Del mismo modo, los objetos `ProductsDataTable` y `ProductsRow` utilizados en esta clase tienen el prefijo del espacio de nombres `NorthwindWithSprocs`. La clase `ProductsBLLWithSprocs` proporciona dos métodos de acceso a datos, `GetProducts` y `GetProductByProductID`, y métodos para agregar, actualizar y eliminar una única instancia del producto.

## <a name="step-7-working-with-thenorthwindwithsprocsdataset-from-the-presentation-layer"></a>Paso 7: trabajar con el conjunto de`NorthwindWithSprocs`desde el nivel de presentación

En este punto, hemos creado un DAL que usa procedimientos almacenados para obtener acceso y modificar los datos de la base de datos subyacente. También hemos creado una capa BLL rudimentaria con métodos para recuperar todos los productos o un producto determinado junto con métodos para agregar, actualizar y eliminar productos. Para redondear este tutorial, cree una página ASP.NET que use la clase BLL s `ProductsBLLWithSprocs` para mostrar, actualizar y eliminar registros.

Abra la página `NewSprocs.aspx` en la carpeta `AdvancedDAL` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador, asignándole un nombre `Products`. En la etiqueta inteligente de GridView s, elija enlazarlo a un nuevo ObjectDataSource denominado `ProductsDataSource`. Configure ObjectDataSource para usar la clase `ProductsBLLWithSprocs`, como se muestra en la figura 22.

[![configurar ObjectDataSource para usar la clase ProductsBLLWithSprocs](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image51.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image50.png)

**Figura 22**: configuración de ObjectDataSource para usar la clase `ProductsBLLWithSprocs` ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image52.png))

La lista desplegable de la pestaña seleccionar tiene dos opciones, `GetProducts` y `GetProductByProductID`. Como queremos mostrar todos los productos en GridView, elija el método `GetProducts`. Las listas desplegables de las pestañas actualizar, insertar y eliminar tienen solo un método. Asegúrese de que cada una de estas listas desplegables tiene seleccionado el método adecuado y, a continuación, haga clic en finalizar.

Una vez completado el Asistente para ObjectDataSource, Visual Studio agregará BoundFields y CheckBoxField a GridView para los campos de datos del producto. Active las características de edición y eliminación integradas de GridView mediante la comprobación de las opciones habilitar edición y habilitar eliminación presentes en la etiqueta inteligente.

[![la página contiene un control GridView con compatibilidad de edición y eliminación habilitada](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image54.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image53.png)

**Figura 23**: la página contiene un control GridView con compatibilidad de edición y eliminación habilitada ([haga clic para ver la imagen de tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image55.png))

Como se explicó en los tutoriales anteriores, al finalizar el Asistente de ObjectDataSource s, Visual Studio establece la propiedad `OldValuesParameterFormatString` en el {0}de\_original. Se debe revertir a su valor predeterminado de {0} para que las características de modificación de datos funcionen correctamente con los parámetros esperados por los métodos de la capa BLL. Por lo tanto, asegúrese de establecer la propiedad `OldValuesParameterFormatString` en {0} o quite la propiedad de la sintaxis declarativa.

Después de completar el Asistente para configurar orígenes de datos, activar la compatibilidad de edición y eliminación en GridView y devolver la propiedad ObjectDataSource s `OldValuesParameterFormatString` a su valor predeterminado, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/samples/sample12.aspx)]

En este momento, podríamos preparar GridView personalizando la interfaz de edición para incluir la validación, con lo que las columnas `CategoryID` y `SupplierID` se representan como DropDownLists, y así sucesivamente. También se puede Agregar una confirmación del lado cliente al botón eliminar y se recomienda dedicar tiempo a implementar estas mejoras. Dado que estos temas se han tratado en los tutoriales anteriores, sin embargo, no se incluirán de nuevo aquí.

Independientemente de si mejora GridView o no, pruebe las características principales de la página en un explorador. Como se muestra en la Figura 24, la página muestra los productos en un control GridView que proporciona capacidades de edición y eliminación por fila.

[![los productos se pueden ver, editar y eliminar de GridView](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image57.png)](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image56.png)

**Figura 24**: los productos se pueden ver, editar y eliminar de GridView ([haga clic para ver la imagen a tamaño completo](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb/_static/image58.png))

## <a name="summary"></a>Resumen

Los TableAdapters de un conjunto de datos con tipo pueden tener acceso a los datos de la base de datos mediante instrucciones SQL ad hoc o mediante procedimientos almacenados. Al trabajar con procedimientos almacenados, se pueden usar los procedimientos almacenados existentes o se puede indicar al Asistente para TableAdapter que cree nuevos procedimientos almacenados basados en una consulta de `SELECT`. En este tutorial se ha explorado cómo se crean automáticamente los procedimientos almacenados para nosotros.

Aunque los procedimientos almacenados generados automáticamente ayudan a ahorrar tiempo, hay algunos casos en los que el procedimiento almacenado creado por el asistente no se alinea con lo que se habría creado en nuestro propio. Un ejemplo es el `Products_Update` procedimiento almacenado, que esperaba `@Original_ProductID` y `@ProductID` parámetros de entrada, aunque el parámetro `@Original_ProductID` era superfluo.

En muchos escenarios, es posible que los procedimientos almacenados ya se hayan creado, o bien, es posible que desee compilarlos manualmente para tener un mayor control sobre los comandos del procedimiento almacenado. En cualquier caso, queremos indicar al TableAdapter que use los procedimientos almacenados existentes para sus métodos. Veremos cómo hacerlo en el siguiente tutorial.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Crear y mantener procedimientos almacenados](https://msdn.microsoft.com/library/aa214299(SQL.80).aspx)
- [Recuperar datos escalares de un procedimiento almacenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)
- [Conceptos básicos de SQL Server procedimiento almacenado](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)
- [Procedimientos almacenados: información general](http://www.sqlteam.com/item.asp?ItemID=563)
- [Escribir un procedimiento almacenado](http://www.4guysfromrolla.com/webtech/111499-1.shtml)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Hilton Geisenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
> [Siguiente](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
