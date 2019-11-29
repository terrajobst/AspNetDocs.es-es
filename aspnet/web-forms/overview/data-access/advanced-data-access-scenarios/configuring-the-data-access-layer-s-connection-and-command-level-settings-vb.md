---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: Configurar la conexión y la configuración del nivel de comando de la capa de acceso a datos (VB) | Microsoft Docs
author: rick-anderson
description: Los TableAdapters dentro de un conjunto de datos con tipo se encargan automáticamente de conectarse a la base de datos, emitir comandos y rellenar un objeto DataTable con los resultados...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: fa2868fc0dd8acd76f600b47d92adb984ce8d105
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74573658"
---
# <a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>Configurar las opciones de nivel comando y de conexión de la capa de acceso a datos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip) o [Descargar PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> Los TableAdapters dentro de un conjunto de datos con tipo se encargan automáticamente de conectarse a la base de datos, emitir comandos y rellenar un objeto DataTable con los resultados. Sin embargo, hay ocasiones en las que queremos encargarnos de estos detalles y en este tutorial aprendemos a acceder a la configuración de conexión de base de datos y de nivel de comando en el TableAdapter.

## <a name="introduction"></a>Introducción

A lo largo de la serie de tutoriales, hemos usado conjuntos de datos con tipo para implementar el nivel de acceso a datos y objetos comerciales de nuestra arquitectura en capas. Como se describe en el [primer tutorial](../introduction/creating-a-data-access-layer-vb.md), los objetos DataTable de los conjuntos de datos con tipo sirven como repositorios de datos, mientras que los TableAdapters actúan como contenedores para comunicarse con la base de datos para recuperar y modificar los datos subyacentes. Los TableAdapters encapsulan la complejidad que conlleva trabajar con la base de datos y nos evita tener que escribir código para conectarse a la base de datos, emitir un comando o rellenar los resultados en un DataTable.

Sin embargo, hay ocasiones en las que es necesario Burrow a las profundidades del TableAdapter y escribir código que funcione directamente con los objetos ADO.NET. En el tutorial de ajuste de las [modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) , por ejemplo, se han agregado métodos a TableAdapter para iniciar, confirmar y revertir las transacciones ADO.net. Estos métodos usaban un objeto de `SqlTransaction` interno creado manualmente que se asignó a los objetos de `SqlCommand` de TableAdapter.

En este tutorial, veremos cómo acceder a la configuración de conexión de base de datos y de nivel de comando en el TableAdapter. En concreto, agregaremos funcionalidad a la `ProductsTableAdapter` que permite el acceso a la cadena de conexión subyacente y a la configuración de tiempo de espera del comando.

## <a name="working-with-data-using-adonet"></a>Trabajar con datos mediante ADO.NET

El marco de Microsoft .NET contiene una gran cantidad de clases diseñadas específicamente para trabajar con datos. Estas clases, que se encuentran dentro del [espacio de nombres`System.Data`](https://msdn.microsoft.com/library/system.data.aspx), se conocen como clases *ADO.net* . Algunas de las clases del paraguas ADO.NET están asociadas a un *proveedor de datos*determinado. Puede pensar en un proveedor de datos como un canal de comunicación que permite que la información fluya entre las clases ADO.NET y el almacén de datos subyacente. Hay proveedores generalizados, como OleDb y ODBC, así como proveedores que están especialmente diseñados para un sistema de base de datos determinado. Por ejemplo, aunque es posible conectarse a una base de datos de Microsoft SQL Server mediante el proveedor OleDb, el proveedor SqlClient es mucho más eficaz, ya que se diseñó y optimizó específicamente para SQL Server.

Al tener acceso a los datos mediante programación, se usa normalmente el siguiente patrón:

1. Establezca una conexión con la base de datos.
2. Emita un comando.
3. En el caso de las consultas de `SELECT`, trabaje con los registros resultantes.

Hay clases ADO.NET independientes para realizar cada uno de estos pasos. Para conectarse a una base de datos mediante el proveedor SqlClient, por ejemplo, utilice la [clase`SqlConnection`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx). Para emitir un comando `INSERT`, `UPDATE`, `DELETE`o `SELECT` a la base de datos, utilice la [clase`SqlCommand`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx).

Excepto en el tutorial sobre el ajuste de las [modificaciones de base de datos dentro de una transacción](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md) , no tuvimos que escribir ningún código de ADO.net de bajo nivel, ya que el código generado automáticamente por TableAdapters incluye la funcionalidad necesaria para conectarse a la base de datos, emitir comandos, recuperar datos y rellenar los datos en DataTables. Sin embargo, puede haber ocasiones en las que sea necesario personalizar estos valores de bajo nivel. En los siguientes pasos, examinaremos los objetos ADO.NET utilizados internamente por los TableAdapters.

## <a name="step-1-examining-with-the-connection-property"></a>Paso 1: examinar con la propiedad de conexión

Cada clase TableAdapter tiene una propiedad `Connection` que especifica la información de conexión a la base de datos. El tipo de datos de la propiedad y el valor `ConnectionString` vienen determinados por las selecciones realizadas en el Asistente para configuración de TableAdapter. Recuerde que cuando agregamos primero un TableAdapter a un conjunto de datos con tipo, este asistente nos pide el origen de la base de datos (consulte la figura 1). La lista desplegable de este primer paso incluye las bases de datos especificadas en el archivo de configuración y otras bases de datos de las conexiones de datos de Explorador de servidores s. Si la base de datos que deseamos usar no existe en la lista desplegable, puede especificar una nueva conexión de base de datos haciendo clic en el botón nueva conexión y proporcionando la información de conexión necesaria.

[![el primer paso del Asistente para configuración de TableAdapter](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**Figura 1**: el primer paso del Asistente para configuración de TableAdapter ([haga clic para ver la imagen de tamaño completo](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))

Espere unos instantes para inspeccionar el código de la propiedad de `Connection` de TableAdapter s. Como se indicó en el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) , podemos ver el código de TableAdapter generado automáticamente en la ventana de vista de clases, profundizando en la clase adecuada y, a continuación, haciendo doble clic en el nombre del miembro.

Desplácese a la ventana de Vista de clases; para ello, vaya al menú Ver y seleccione Vista de clases (o presione Ctrl + Mayús + C). En la mitad superior de la ventana de Vista de clases, explore en profundidad hasta el espacio de nombres `NorthwindTableAdapters` y seleccione la clase `ProductsTableAdapter`. Se mostrarán los miembros `ProductsTableAdapter` s en la mitad inferior del Vista de clases, como se muestra en la figura 2. Haga doble clic en la propiedad `Connection` para ver su código.

![Haga doble clic en la propiedad de conexión en el Vista de clases para ver su código generado automáticamente.](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**Figura 2**: haga doble clic en la propiedad de conexión en el vista de clases para ver su código generado automáticamente

La propiedad `Connection` de TableAdapter y otro código relacionado con la conexión es el siguiente:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

Cuando se crea una instancia de la clase TableAdapter, la variable miembro `_connection` es igual a `Nothing`. Cuando se tiene acceso a la propiedad `Connection`, primero se comprueba para ver si se ha creado una instancia de la variable miembro `_connection`. Si no es así, se invoca el método de `InitConnection`, que crea una instancia de `_connection` y establece su propiedad `ConnectionString` en el valor de la cadena de conexión especificado en el primer paso del Asistente para configuración de TableAdapter.

También se puede asignar la propiedad `Connection` a un objeto `SqlConnection`. Al hacerlo, se asocia el nuevo objeto `SqlConnection` con cada uno de los objetos TableAdapter s `SqlCommand`.

## <a name="step-2-exposing-connection-level-settings"></a>Paso 2: exponer la configuración del nivel de conexión

La información de conexión debe permanecer encapsulada en el TableAdapter y no ser accesible para otras capas de la arquitectura de la aplicación. Sin embargo, puede haber escenarios en los que la información de nivel de conexión de TableAdapter s deba ser accesible o personalizable para una consulta, un usuario o una página de ASP.NET.

Vamos a ampliar el `ProductsTableAdapter` en el conjunto de `Northwind` para incluir una propiedad `ConnectionString` que pueda usar el nivel de lógica de negocios para leer o cambiar la cadena de conexión utilizada por el TableAdapter.

> [!NOTE]
> Una *cadena de conexión* es una cadena que especifica la información de conexión a la base de datos, como el proveedor que se va a usar, la ubicación de la base de datos, las credenciales de autenticación y otros valores de configuración relacionados con la base de datos. Para obtener una lista de los patrones de cadena de conexión utilizados por una variedad de proveedores y almacenes de datos, vea [connectionStrings.com](http://www.connectionstrings.com/).

Como se describe en el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) , las clases generadas automáticamente por el conjunto de datos con tipo se pueden extender mediante el uso de clases parciales. En primer lugar, cree una nueva subcarpeta en el proyecto denominada `ConnectionAndCommandSettings` bajo la carpeta `~/App_Code/DAL`.

![Agregue una subcarpeta denominada ConnectionAndCommandSettings](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**Figura 3**: agregar una subcarpeta denominada `ConnectionAndCommandSettings`

Agregue un nuevo archivo de clase denominado `ProductsTableAdapter.ConnectionAndCommandSettings.vb` y escriba el código siguiente:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

Esta clase parcial agrega una propiedad `Public` denominada `ConnectionString` a la clase `ProductsTableAdapter` que permite a cualquier capa leer o actualizar la cadena de conexión para la conexión subyacente de TableAdapter s.

Con esta clase parcial creada (y guardada), abra la clase `ProductsBLL`. Vaya a uno de los métodos existentes y escriba `Adapter` y, a continuación, presione la tecla de punto para abrir IntelliSense. Debería ver la nueva propiedad `ConnectionString` disponible en IntelliSense, lo que significa que puede leer o ajustar este valor mediante programación desde el BLL.

## <a name="exposing-the-entire-connection-object"></a>Exponer todo el objeto de conexión

Esta clase parcial expone solo una propiedad del objeto de conexión subyacente: `ConnectionString`. Si desea que todo el objeto de conexión esté disponible más allá de los límites del TableAdapter, también puede cambiar el nivel de protección `Connection` propiedad s. El código generado automáticamente que hemos examinado en el paso 1 mostró que la propiedad TableAdapter s `Connection` está marcada como `Friend`, lo que significa que solo las clases del mismo ensamblado pueden tener acceso a ella. No obstante, esto se puede cambiar a través de la propiedad `ConnectionModifier` de TableAdapter s.

Abra el conjunto de `Northwind`, haga clic en el `ProductsTableAdapter` en el diseñador y navegue hasta el ventana Propiedades. Allí verá el `ConnectionModifier` establecido en su valor predeterminado, `Assembly`. Para que la propiedad `Connection` esté disponible fuera del ensamblado del conjunto de elementos de tipo, cambie la propiedad `ConnectionModifier` a `Public`.

[![el nivel de accesibilidad de la propiedad de conexión se puede configurar mediante la propiedad ConnectionModifier](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**Figura 4**: el nivel de accesibilidad de la propiedad `Connection` se puede configurar mediante la propiedad `ConnectionModifier` ([haga clic para ver la imagen de tamaño completo](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))

Guarde el conjunto de resultados y, a continuación, vuelva a la clase `ProductsBLL`. Como antes, vaya a uno de los métodos existentes y escriba en `Adapter` y, a continuación, presione la tecla de punto para abrir IntelliSense. La lista debe incluir una propiedad `Connection`, lo que significa que ahora puede leer o asignar mediante programación cualquier configuración de nivel de conexión de la capa BLL.

## <a name="step-3-examining-the-command-related-properties"></a>Paso 3: examinar las propiedades relacionadas con el comando

Un TableAdapter está formado por una consulta principal que, de forma predeterminada, tiene instrucciones de `INSERT`, `UPDATE`y `DELETE` generadas automáticamente. Las instrucciones de `INSERT`, `UPDATE`y `DELETE` de consultas principales se implementan en el código de TableAdapter como un objeto de adaptador de datos ADO.NET a través de la propiedad `Adapter`. Al igual que con su propiedad `Connection`, el tipo de datos `Adapter` Property s viene determinado por el proveedor de datos utilizado. Dado que estos tutoriales usan el proveedor SqlClient, la propiedad `Adapter` es de tipo [`SqlDataAdapter`](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx).

La propiedad `Adapter` TableAdapter s tiene tres propiedades de tipo `SqlCommand` que usa para emitir las instrucciones `INSERT`, `UPDATE`y `DELETE`:

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

Un objeto `SqlCommand` es responsable de enviar una consulta determinada a la base de datos y tiene propiedades como: [`CommandText`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx), que contiene la instrucción SQL ad hoc o el procedimiento almacenado que se va a ejecutar; y [`Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx), que es una colección de objetos `SqlParameter`. Como hemos visto en el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) , estos objetos de comando se pueden personalizar mediante el ventana Propiedades.

Además de su consulta principal, el TableAdapter puede incluir un número variable de métodos que, cuando se invoca, envían un comando especificado a la base de datos. El objeto de comando principal query s y los objetos de comando de todos los métodos adicionales se almacenan en la propiedad TableAdapter s `CommandCollection`.

Tómese un momento para examinar el código generado por el `ProductsTableAdapter` en el conjunto de `Northwind` para estas dos propiedades y sus variables miembro auxiliares y métodos auxiliares:

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

El código de las propiedades `Adapter` y `CommandCollection` imita estrechamente el de la propiedad `Connection`. Hay variables de miembro que contienen los objetos utilizados por las propiedades. Las propiedades `Get` los descriptores de acceso se inician comprobando si la variable miembro correspondiente está `Nothing`. Si es así, se llama a un método de inicialización que crea una instancia de la variable miembro y asigna las propiedades principales relacionadas con el comando.

## <a name="step-4-exposing-command-level-settings"></a>Paso 4: exponer la configuración de nivel de comando

Idealmente, la información de nivel de comando debe permanecer encapsulada dentro de la capa de acceso a datos. Sin embargo, si se necesita esta información en otros niveles de la arquitectura, puede exponerse a través de una clase parcial, al igual que con la configuración de nivel de conexión.

Dado que TableAdapter solo tiene una propiedad de `Connection` única, el código para exponer la configuración del nivel de conexión es bastante sencillo. Las cosas son un poco más complicadas al modificar la configuración del nivel de comando, ya que el TableAdapter puede tener varios objetos de comando: un `InsertCommand`, `UpdateCommand`y `DeleteCommand`, junto con un número variable de objetos de comando en la propiedad `CommandCollection`. Al actualizar la configuración de nivel de comando, esta configuración tendrá que propagarse a todos los objetos de comando.

Por ejemplo, Imagine que había algunas consultas en el TableAdapter que tardaron mucho tiempo en ejecutarse. Al usar el TableAdapter para ejecutar una de esas consultas, es posible que deseemos aumentar la propiedad Command Object s [`CommandTimeout`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx). Esta propiedad especifica el número de segundos que se debe esperar para que se ejecute el comando y su valor predeterminado es 30.

Para permitir que la capa BLL ajuste la propiedad `CommandTimeout`, agregue el siguiente método de `Public` a la `ProductsDataTable` con el archivo de clase parcial creado en el paso 2 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):

[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

Este método se puede invocar desde el nivel de presentación o BLL para establecer el tiempo de espera del comando para todos los problemas de la instancia de TableAdapter.

> [!NOTE]
> Las propiedades `Adapter` y `CommandCollection` se marcan como `Private`, lo que significa que solo se puede tener acceso a ellas desde el código de TableAdapter. A diferencia de la propiedad `Connection`, estos modificadores de acceso no son configurables. Por lo tanto, si necesita exponer propiedades de nivel de comando a otras capas de la arquitectura, debe usar el enfoque de clase parcial descrito anteriormente para proporcionar una `Public` método o propiedad que lea o escriba en los objetos de comando `Private`.

## <a name="summary"></a>Resumen

Los TableAdapters dentro de un conjunto de datos con tipo sirven para encapsular los detalles y la complejidad del acceso a datos. Con TableAdapters, no es necesario preocuparse por escribir código de ADO.NET para conectarse a la base de datos, emitir un comando o rellenar los resultados en un DataTable. Todos ellos se controlan automáticamente.

Sin embargo, puede haber ocasiones en las que sea necesario personalizar los detalles de ADO.NET de bajo nivel, como cambiar la cadena de conexión o los valores predeterminados de tiempo de espera de la conexión o el comando. El TableAdapter tiene propiedades de `Connection`, `Adapter`y `CommandCollection` generadas automáticamente, pero estas son `Friend` o `Private`, de forma predeterminada. Esta información interna se puede exponer extendiendo el TableAdapter usando clases parciales para incluir `Public` métodos o propiedades. Como alternativa, el modificador de acceso de propiedad TableAdapter s `Connection` se puede configurar a través de la propiedad `ConnectionModifier` TableAdapter s.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Burnadette Leigh, S Ren Jacob Lauritsen, Teresa Murphy y Hilton Geisenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](working-with-computed-columns-vb.md)
> [Siguiente](protecting-connection-strings-and-other-configuration-information-vb.md)
