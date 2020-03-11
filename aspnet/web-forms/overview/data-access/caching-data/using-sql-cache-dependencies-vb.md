---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
title: Usar dependencias de caché de SQL (VB) | Microsoft Docs
author: rick-anderson
description: La estrategia de almacenamiento en caché más sencilla consiste en permitir que los datos almacenados en caché expiren después de un período de tiempo especificado. Pero este sencillo enfoque significa que los datos almacenados en caché maintai...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: bd347d93-4251-4532-801c-a36f2dfa7f96
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-vb
msc.type: authoredcontent
ms.openlocfilehash: 7d095538bd92d50675e5fce44f5ca68e8ee6c0e8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78442909"
---
# <a name="using-sql-cache-dependencies-vb"></a>Usar dependencias de caché de SQL (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_VB.zip) o [Descargar PDF](using-sql-cache-dependencies-vb/_static/datatutorial61vb1.pdf)

> La estrategia de almacenamiento en caché más sencilla consiste en permitir que los datos almacenados en caché expiren después de un período de tiempo especificado. Pero este sencillo enfoque significa que los datos almacenados en caché no mantienen ninguna asociación con su origen de datos subyacente, lo que da lugar a datos obsoletos que se retienen demasiado largos o datos actuales que han expirado demasiado pronto. Un mejor enfoque consiste en usar la clase SqlCacheDependency para que los datos permanezcan en la memoria caché hasta que los datos subyacentes se hayan modificado en la base de datos SQL. En este tutorial se muestra cómo realizar las siguientes acciones.

## <a name="introduction"></a>Introducción

Las técnicas de almacenamiento en caché examinadas en los [datos de almacenamiento en caché con los datos ObjectDataSource](caching-data-with-the-objectdatasource-vb.md) y [caching en los tutoriales de arquitectura](caching-data-in-the-architecture-vb.md) usaban una expiración basada en el tiempo para expulsar los datos de la memoria caché después de un período especificado. Este enfoque es la manera más sencilla de equilibrar las ventajas de rendimiento del almacenamiento en caché frente a la obsolescencia de los datos. Al seleccionar un tiempo de expiración de *x* segundos, un desarrollador de páginas conviene disfrutar de las ventajas de rendimiento del almacenamiento en caché durante solo *x* segundos, pero puede resultar fácil que los datos nunca queden obsoletos más que un máximo de *x* segundos. Por supuesto, para los datos estáticos, *x* puede extenderse hasta la duración de la aplicación Web, como se examinó en el tutorial de [Inicio de datos de almacenamiento en caché al iniciar la aplicación](caching-data-at-application-startup-vb.md) .

Al almacenar en caché los datos de la base de datos, a menudo se elige una expiración basada en el tiempo para facilitar su uso, pero con frecuencia es una solución inadecuada. Idealmente, los datos de la base de datos permanecerán en caché hasta que se hayan modificado los datos subyacentes en la base de datos. solo entonces se expulsará la memoria caché. Este enfoque maximiza las ventajas de rendimiento del almacenamiento en caché y minimiza la duración de los datos obsoletos. Sin embargo, para disfrutar de estas ventajas, debe existir algún sistema que sepa cuándo se han modificado los datos de la base de datos subyacente y expulsa los elementos correspondientes de la memoria caché. Antes de ASP.NET 2,0, los desarrolladores de páginas eran responsables de implementar este sistema.

ASP.NET 2,0 proporciona una [clase`SqlCacheDependency`](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx) y la infraestructura necesaria para determinar cuándo se ha producido un cambio en la base de datos, de modo que se puedan desalojar los elementos almacenados en caché correspondientes. Hay dos técnicas para determinar cuándo han cambiado los datos subyacentes: notificación y sondeo. Después de analizar las diferencias entre notificaciones y sondeo, crearemos la infraestructura necesaria para admitir el sondeo y, después, exploraremos cómo usar la clase `SqlCacheDependency` en escenarios declarativos y mediante programación.

## <a name="understanding-notification-and-polling"></a>Descripción de las notificaciones y el sondeo

Existen dos técnicas que se pueden utilizar para determinar cuándo se han modificado los datos de una base de datos: notificación y sondeo. Con la notificación, la base de datos alerta automáticamente al tiempo de ejecución de ASP.NET cuando se han cambiado los resultados de una consulta determinada desde la última vez que se ejecutó la consulta, momento en el que se expulsan los elementos almacenados en caché asociados a la consulta. Con el sondeo, el servidor de base de datos mantiene información acerca de Cuándo se han actualizado por última vez las tablas concretas. El tiempo de ejecución de ASP.NET sondea periódicamente la base de datos para comprobar qué tablas han cambiado desde que se introdujeron en la memoria caché. Las tablas cuyos datos se han modificado tienen sus elementos de caché asociados expulsados.

La opción de notificación requiere menos configuración que el sondeo y es más granular, ya que realiza un seguimiento de los cambios en el nivel de consulta en lugar de en el nivel de tabla. Desafortunadamente, las notificaciones solo están disponibles en las ediciones completas de Microsoft SQL Server 2005 (es decir, las ediciones que no son Express). Sin embargo, la opción de sondeo se puede usar para todas las versiones de Microsoft SQL Server de 7,0 a 2005. Dado que estos tutoriales usan la edición Express de SQL Server 2005, nos centraremos en la configuración y el uso de la opción de sondeo. Consulte la sección lecturas adicionales al final de este tutorial para obtener más recursos sobre las capacidades de notificación de SQL Server 2005 s.

Con el sondeo, la base de datos debe configurarse para incluir una tabla denominada `AspNet_SqlCacheTablesForChangeNotification` que tenga tres columnas: `tableName`, `notificationCreated`y `changeId`. Esta tabla contiene una fila para cada tabla que tiene datos que pueden ser necesarios para usarse en una dependencia de caché de SQL en la aplicación Web. La columna `tableName` especifica el nombre de la tabla mientras `notificationCreated` indica la fecha y la hora en que se agregó la fila a la tabla. La columna `changeId` es de tipo `int` y tiene un valor inicial de 0. Su valor se incrementa con cada modificación de la tabla.

Además de la tabla de `AspNet_SqlCacheTablesForChangeNotification`, la base de datos también necesita incluir desencadenadores en cada una de las tablas que pueden aparecer en una dependencia de caché de SQL. Estos desencadenadores se ejecutan cada vez que se inserta, actualiza o elimina una fila y se incrementa el valor de la tabla s `changeId` en `AspNet_SqlCacheTablesForChangeNotification`.

El tiempo de ejecución de ASP.NET realiza un seguimiento de la `changeId` actual para una tabla al almacenar datos en caché mediante un objeto `SqlCacheDependency`. La base de datos se comprueba periódicamente y los objetos de `SqlCacheDependency` cuyo `changeId` difiere del valor de la base de datos se expulsan, ya que un valor de `changeId` diferente indica que se ha producido un cambio en la tabla desde que se almacenaron en caché los datos.

## <a name="step-1-exploring-theaspnet_regsqlexecommand-line-program"></a>Paso 1: explorar el programa de línea de comandos`aspnet_regsql.exe`

Con el enfoque de sondeo, la base de datos debe configurarse para que contenga la infraestructura descrita anteriormente: una tabla predefinida (`AspNet_SqlCacheTablesForChangeNotification`), una serie de procedimientos almacenados y desencadenadores en cada una de las tablas que se pueden usar en las dependencias de caché de SQL en la aplicación Web. Estas tablas, procedimientos almacenados y desencadenadores se pueden crear a través del programa de línea de comandos `aspnet_regsql.exe`, que se encuentra en la carpeta `$WINDOWS$\Microsoft.NET\Framework\version`. Para crear la tabla de `AspNet_SqlCacheTablesForChangeNotification` y los procedimientos almacenados asociados, ejecute lo siguiente desde la línea de comandos:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample1.cmd)]

> [!NOTE]
> Para ejecutar estos comandos, el inicio de sesión de base de datos especificado debe estar en los roles [`db_securityadmin`](https://msdn.microsoft.com/library/ms188685.aspx) y [`db_ddladmin`](https://msdn.microsoft.com/library/ms190667.aspx) . Para examinar el T-SQL enviado a la base de datos mediante el programa de línea de comandos `aspnet_regsql.exe`, consulte [esta entrada de blog](http://scottonwriting.net/sowblog/posts/10709.aspx).

Por ejemplo, para agregar la infraestructura de sondeo a una base de datos de Microsoft SQL Server denominada `pubs` en un servidor de base de datos denominado `ScottsServer` con la autenticación de Windows, navegue hasta el directorio adecuado y, desde la línea de comandos, escriba:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample2.cmd)]

Una vez agregada la infraestructura de nivel de base de datos, es necesario agregar los desencadenadores a las tablas que se usarán en las dependencias de caché de SQL. Vuelva a usar el programa de línea de comandos `aspnet_regsql.exe`, pero especifique el nombre de la tabla con el modificador `-t` y, en lugar de usar el modificador `-ed` use `-et`, como se indica a continuación:

[!code-html[Main](using-sql-cache-dependencies-vb/samples/sample3.html)]

Para agregar los desencadenadores a las tablas `authors` y `titles` en la base de datos de `pubs` en `ScottsServer`, use:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample4.cmd)]

En este tutorial, agregue los desencadenadores a las tablas `Products`, `Categories`y `Suppliers`. Veremos la sintaxis de línea de comandos concreta en el paso 3.

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inapp_data"></a>Paso 2: hacer referencia a una base de datos de Microsoft SQL Server 2005 Express Edition en`App_Data`

El programa de línea de comandos `aspnet_regsql.exe` requiere la base de datos y el nombre del servidor para poder agregar la infraestructura de sondeo necesaria. Pero ¿cuál es el nombre del servidor y la base de datos de una base de datos Microsoft SQL Server 2005 Express que se encuentra en la carpeta `App_Data`? En lugar de tener que detectar cuáles son los nombres de los servidores y las bases de datos, encontramos que el enfoque más sencillo consiste en adjuntar la base de datos a la instancia de `localhost\SQLExpress` Database y cambiar el nombre de los datos mediante [SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx). Si tiene una de las versiones completas de SQL Server 2005 instaladas en el equipo, es probable que ya tenga SQL Server Management Studio instalado en el equipo. Si solo tiene la edición Express, puede descargar el [Microsoft SQL Server gratuito Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796).

Empiece por cerrar Visual Studio. Después, abra SQL Server Management Studio y elija conectarse al servidor de `localhost\SQLExpress` mediante la autenticación de Windows.

![Adjuntar al servidor de localhost\SQLExpress](using-sql-cache-dependencies-vb/_static/image1.gif)

**Figura 1**: asociar al servidor de `localhost\SQLExpress`

Después de conectarse al servidor, Management Studio mostrará el servidor y tendrá subcarpetas para las bases de datos, la seguridad, etc. Haga clic con el botón derecho en la carpeta bases de datos y elija la opción adjuntar. Se abrirá el cuadro de diálogo Adjuntar bases de datos (consulte la figura 2). Haga clic en el botón Agregar y seleccione la carpeta `NORTHWND.MDF` base de datos en la carpeta de `App_Data` de la aplicación Web.

[![adjunte el archivo NORTHWND. Base de datos MDF de la carpeta App_Data](using-sql-cache-dependencies-vb/_static/image2.gif)](using-sql-cache-dependencies-vb/_static/image1.png)

**Figura 2**: Adjunte la base de datos de `NORTHWND.MDF` desde la carpeta `App_Data` ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image2.png))

Esto agregará la base de datos a la carpeta de bases de datos. El nombre de la base de datos puede ser la ruta de acceso completa al archivo de base de datos o la ruta de acceso completa antepuesto con un [GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier). Para evitar tener que escribir este nombre de base de datos larga al usar la herramienta de línea de comandos ASPNET\_regsql. exe, cambie el nombre de la base de datos a un nombre más descriptivo; para ello, haga clic con el botón secundario en base de datos adjunta y elija cambiar nombre. He cambiado el nombre de mi base de datos a tutoriales de datos.

![Cambiar el nombre de la base de datos adjunta a un nombre más descriptivo](using-sql-cache-dependencies-vb/_static/image3.gif)

**Figura 3**: cambiar el nombre de la base de datos adjunta a un nombre más descriptivo

## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>Paso 3: agregar la infraestructura de sondeo a la base de datos Northwind

Ahora que hemos adjuntado la base de datos de `NORTHWND.MDF` desde la carpeta `App_Data`, se ha vuelto a preparar para agregar la infraestructura de sondeo. Suponiendo que ha cambiado el nombre de la base de datos a los tutoriales de datos, ejecute los cuatro comandos siguientes:

[!code-console[Main](using-sql-cache-dependencies-vb/samples/sample5.cmd)]

Después de ejecutar estos cuatro comandos, haga clic con el botón derecho en el nombre de la base de datos en Management Studio, vaya al submenú tareas y elija Desasociar. Después, cierre Management Studio y vuelva a abrir Visual Studio.

Una vez que Visual Studio se ha reabierto, profundice en la base de datos a través de la Explorador de servidores. Tenga en cuenta la nueva tabla (`AspNet_SqlCacheTablesForChangeNotification`), los nuevos procedimientos almacenados y los desencadenadores en las tablas `Products`, `Categories`y `Suppliers`.

![La base de datos ahora incluye la infraestructura de sondeo necesaria](using-sql-cache-dependencies-vb/_static/image4.gif)

**Figura 4**: la base de datos ahora incluye la infraestructura de sondeo necesaria

## <a name="step-4-configuring-the-polling-service"></a>Paso 4: configurar el servicio de sondeo

Después de crear las tablas, los desencadenadores y los procedimientos almacenados necesarios en la base de datos, el último paso es configurar el servicio de sondeo, que se realiza a través de `Web.config` especificando las bases de datos que se van a usar y la frecuencia de sondeo en milisegundos. El marcado siguiente sondea la base de datos Northwind una vez por segundo.

[!code-xml[Main](using-sql-cache-dependencies-vb/samples/sample6.xml)]

El valor `name` del elemento `<add>` (NorthwindDB) asocia un nombre legible con una base de datos determinada. Al trabajar con dependencias de caché de SQL, es necesario hacer referencia al nombre de la base de datos que se define aquí, así como a la tabla en la que se basan los datos almacenados en caché. Veremos cómo usar la clase `SqlCacheDependency` para asociar mediante programación las dependencias de caché de SQL con los datos almacenados en caché en el paso 6.

Una vez establecida una dependencia de caché de SQL, el sistema de sondeo se conectará a las bases de datos definidas en la `<databases>` elementos cada `pollTime` milisegundos y ejecutará el `AspNet_SqlCachePollingStoredProcedure` procedimiento almacenado. Este procedimiento almacenado, que se volvió a agregar en el paso 3 con la herramienta de línea de comandos `aspnet_regsql.exe`, devuelve los valores `tableName` y `changeId` de cada registro de `AspNet_SqlCacheTablesForChangeNotification`. Las dependencias de caché de SQL obsoletas se expulsan de la memoria caché.

El valor `pollTime` presenta un equilibrio entre el rendimiento y la obsolescencia de los datos. Un valor de `pollTime` pequeño aumenta el número de solicitudes a la base de datos, pero expulsa más rápidamente los datos obsoletos de la memoria caché. Un valor `pollTime` mayor reduce el número de solicitudes de base de datos, pero aumenta el retraso que transcurre entre el momento en que cambia el servidor back-end y el momento en que se expulsan los elementos de caché relacionados. Afortunadamente, la solicitud de base de datos está ejecutando un procedimiento almacenado simple que devuelve solo unas pocas filas de una tabla simple y ligera. Pero experimente con distintos valores de `pollTime` para encontrar un equilibrio ideal entre el acceso a la base de datos y la obsolescencia de los datos de la aplicación. El valor de `pollTime` más pequeño permitido es 500.

> [!NOTE]
> En el ejemplo anterior se proporciona un único `pollTime` valor en el elemento `<sqlCacheDependency>`, pero se puede especificar opcionalmente el valor `pollTime` en el elemento `<add>`. Esto es útil si tiene varias bases de datos especificadas y desea personalizar la frecuencia de sondeo por base de datos.

## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>Paso 5: trabajar de forma declarativa con las dependencias de caché de SQL

En los pasos 1 a 4, analizamos cómo configurar la infraestructura de base de datos necesaria y configurar el sistema de sondeo. Con esta infraestructura implementada, ahora se pueden agregar elementos a la memoria caché de datos con una dependencia de caché de SQL asociada mediante técnicas de programación o declarativas. En este paso, examinaremos cómo trabajar mediante declaración con las dependencias de caché de SQL. En el paso 6 veremos el enfoque de programación.

Los [datos de almacenamiento en caché con el](caching-data-with-the-objectdatasource-vb.md) tutorial de ObjectDataSource exploraron las capacidades de almacenamiento en caché declarativo de ObjectDataSource. Con solo establecer la propiedad `EnableCaching` en `True` y la propiedad `CacheDuration` en un intervalo de tiempo, ObjectDataSource almacenará automáticamente en caché los datos devueltos de su objeto subyacente para el intervalo especificado. ObjectDataSource también puede usar una o varias dependencias de caché de SQL.

Para demostrar el uso de las dependencias de caché de SQL mediante declaración, abra la página `SqlCacheDependencies.aspx` en la carpeta `Caching` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador. Establezca el `ID` de GridView en `ProductsDeclarative` y, desde su etiqueta inteligente, elija enlazarlo a un nuevo ObjectDataSource denominado `ProductsDataSourceDeclarative`.

[![crear un nuevo ObjectDataSource denominado ProductsDataSourceDeclarative](using-sql-cache-dependencies-vb/_static/image5.gif)](using-sql-cache-dependencies-vb/_static/image3.png)

**Figura 5**: crear un nuevo ObjectDataSource denominado `ProductsDataSourceDeclarative` ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image4.png))

Configure ObjectDataSource para usar la clase `ProductsBLL` y establezca la lista desplegable de la pestaña seleccionar en `GetProducts()`. En la pestaña actualizar, elija la sobrecarga `UpdateProduct` con tres parámetros de entrada: `productName`, `unitPrice`y `productID`. Establezca las listas desplegables en (ninguno) en las pestañas insertar y eliminar.

[![usar la sobrecarga UpdateProduct con tres parámetros de entrada](using-sql-cache-dependencies-vb/_static/image6.gif)](using-sql-cache-dependencies-vb/_static/image5.png)

**Figura 6**: uso de la sobrecarga UpdateProduct con tres parámetros de entrada ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image6.png))

[![establecer la lista desplegable en (ninguno) para las pestañas insertar y eliminar](using-sql-cache-dependencies-vb/_static/image7.gif)](using-sql-cache-dependencies-vb/_static/image7.png)

**Figura 7**: establezca la lista desplegable en (ninguno) para las pestañas insertar y eliminar ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image8.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y CheckBoxFields en GridView para cada uno de los campos de datos. Quite todos los campos, excepto `ProductName`, `CategoryName`y `UnitPrice`, y dé formato a estos campos como considere oportunos. En la etiqueta inteligente de GridView s, active las casillas habilitar paginación, habilitar ordenación y habilitar edición. Visual Studio establecerá la propiedad ObjectDataSource s `OldValuesParameterFormatString` en `original_{0}`. Para que la característica de edición de GridView funcione correctamente, quite esta propiedad completamente de la sintaxis declarativa o vuelva a establecerla en su valor predeterminado, `{0}`.

Por último, agregue un control Web Label sobre GridView y establezca su propiedad `ID` en `ODSEvents` y su propiedad `EnableViewState` en `False`. Después de realizar estos cambios, el marcado declarativo de la página debe ser similar al siguiente. Tenga en cuenta que he realizado varias personalizaciones estéticas en los campos de GridView que no son necesarios para mostrar la funcionalidad de dependencia de caché de SQL.

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample7.aspx)]

A continuación, cree un controlador de eventos para el evento ObjectDataSource s `Selecting` y agregue el código siguiente:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample8.vb)]

Recuerde que el evento ObjectDataSource s `Selecting` solo se activa cuando se recuperan datos de su objeto subyacente. Si ObjectDataSource obtiene acceso a los datos desde su propia memoria caché, este evento no se desencadena.

Ahora, visite esta página a través de un explorador. Puesto que todavía estamos implementando cualquier almacenamiento en caché, cada vez que se pagina, ordena o edita la cuadrícula, la página debe mostrar el texto, seleccionando evento desencadenado, como se muestra en la figura 8.

[![el evento ObjectDataSource s Selecting se activa cada vez que se pagina, edita o ordena el control GridView](using-sql-cache-dependencies-vb/_static/image8.gif)](using-sql-cache-dependencies-vb/_static/image9.png)

**Figura 8**: el evento ObjectDataSource s `Selecting` se activa cada vez que se pagina, edita o ordena el control GridView ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image10.png))

Como vimos en los [datos de almacenamiento en caché con el](caching-data-with-the-objectdatasource-vb.md) tutorial de ObjectDataSource, establecer la propiedad `EnableCaching` en `True` hace que ObjectDataSource almacene en caché sus datos durante la duración especificada por su propiedad `CacheDuration`. ObjectDataSource también tiene una [propiedad`SqlCacheDependency`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx), que agrega una o más dependencias de caché SQL a los datos almacenados en caché mediante el patrón:

[!code-css[Main](using-sql-cache-dependencies-vb/samples/sample9.css)]

Donde *DatabaseName* es el nombre de la base de datos tal y como se especifica en el atributo `name` del elemento `<add>` de `Web.config`y *TableName* es el nombre de la tabla de base de datos. Por ejemplo, para crear un ObjectDataSource que almacene en memoria caché los datos de forma indefinida según una dependencia de caché de SQL en la tabla de `Products` de Northwind, establezca la propiedad ObjectDataSource s `EnableCaching` en `True` y su propiedad `SqlCacheDependency` en NorthwindDB: Products.

> [!NOTE]
> Puede usar una dependencia de caché de SQL *y* una expiración basada en tiempo estableciendo `EnableCaching` en `True`, `CacheDuration` en el intervalo de tiempo y `SqlCacheDependency` a los nombres de base de datos y tabla. ObjectDataSource expulsará sus datos cuando se alcance la expiración basada en el tiempo o cuando el sistema de sondeo tenga en cuenta que los datos de la base de datos subyacente han cambiado, lo que suceda primero.

GridView en `SqlCacheDependencies.aspx` muestra datos de dos tablas: `Products` y `Categories` (el campo Product s `CategoryName` se recupera a través de un `JOIN` en `Categories`). Por lo tanto, queremos especificar dos dependencias de caché de SQL: NorthwindDB: Products; NorthwindDB: categorías.

[![configurar ObjectDataSource para admitir el almacenamiento en caché mediante dependencias de caché de SQL en productos y categorías](using-sql-cache-dependencies-vb/_static/image9.gif)](using-sql-cache-dependencies-vb/_static/image11.png)

**Figura 9**: configuración de ObjectDataSource para admitir el almacenamiento en caché mediante dependencias de caché de SQL en `Products` y `Categories` ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image12.png))

Después de configurar ObjectDataSource para admitir el almacenamiento en caché, vuelva a visitar la página a través de un explorador. Una vez más, el texto que seleccione evento desencadenado debe aparecer en la primera visita de la página, pero debería desaparecer al paginar, ordenar o hacer clic en los botones editar o cancelar. Esto se debe a que, una vez que los datos se cargan en la memoria caché de ObjectDataSource, permanece allí hasta que se modifican las tablas `Products` o `Categories` o los datos se actualizan a través de GridView.

Después de la paginación a través de la cuadrícula y de anotar la falta de texto desencadenado por el evento, abra una nueva ventana del explorador y vaya al tutorial de conceptos básicos de la sección edición, inserción y eliminación (`~/EditInsertDelete/Basics.aspx`). Actualice el nombre o el precio de un producto. A continuación, desde hasta la primera ventana del explorador, vea una página de datos diferente, ordene la cuadrícula o haga clic en el botón de edición de una fila. Esta vez, el evento seleccionado debe volver a aparecer, ya que se han modificado los datos de la base de datos subyacente (vea la figura 10). Si el texto no aparece, espere unos instantes e inténtelo de nuevo. Recuerde que el servicio de sondeo está comprobando los cambios en la tabla `Products` cada `pollTime` milisegundos, por lo que hay un retraso entre el momento en que se actualizan los datos subyacentes y el momento en que se expulsan los datos almacenados en caché.

[![modificación de la tabla Products expulsa los datos del producto almacenados en caché](using-sql-cache-dependencies-vb/_static/image10.gif)](using-sql-cache-dependencies-vb/_static/image13.png)

**Figura 10**: modificación de la tabla de productos expulsa los datos de producto almacenados en caché ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image14.png))

## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>Paso 6: trabajar mediante programación con la clase`SqlCacheDependency`

Los [datos de almacenamiento en caché en el](caching-data-in-the-architecture-vb.md) tutorial de arquitectura examinaron las ventajas de usar una capa de almacenamiento en caché independiente en la arquitectura en lugar de acoplar estrechamente el almacenamiento en caché con ObjectDataSource. En ese tutorial, creamos una clase `ProductsCL` para mostrar cómo trabajar con la caché de datos mediante programación. Para usar las dependencias de caché de SQL en la capa de almacenamiento en caché, use la clase `SqlCacheDependency`.

Con el sistema de sondeo, un objeto de `SqlCacheDependency` debe estar asociado a una base de datos y un par de tablas determinados. El código siguiente, por ejemplo, crea un objeto `SqlCacheDependency` basado en la tabla de `Products` de datos Northwind:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample10.vb)]

Los dos parámetros de entrada del constructor `SqlCacheDependency` s son los nombres de base de datos y tabla, respectivamente. Al igual que con la propiedad ObjectDataSource s `SqlCacheDependency`, el nombre de la base de datos utilizado es el mismo que el valor especificado en el atributo `name` del elemento `<add>` en `Web.config`. El nombre de la tabla es el nombre real de la tabla de la base de datos.

Para asociar un `SqlCacheDependency` con un elemento agregado a la memoria caché de datos, utilice una de las sobrecargas del método `Insert` que acepta una dependencia. El código siguiente agrega un *valor* a la memoria caché de datos durante un tiempo indefinido, pero lo asocia a un `SqlCacheDependency` en la tabla `Products`. En Resumen, el *valor* permanece en la memoria caché hasta que se expulsa debido a las restricciones de memoria o porque el sistema de sondeo ha detectado que la tabla de `Products` ha cambiado desde que se almacenó en caché.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample11.vb)]

La clase de `ProductsCL` de la capa de almacenamiento en caché almacena actualmente los datos de la tabla de `Products` con una expiración basada en el tiempo de 60 segundos. Permita actualizar esta clase para que use las dependencias de caché de SQL en su lugar. El método de `AddCacheItem` de `ProductsCL` clase, que es responsable de agregar los datos a la memoria caché, actualmente contiene el código siguiente:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample12.vb)]

Actualice este código para usar un objeto de `SqlCacheDependency` en lugar de la dependencia de caché de `MasterCacheKeyArray`:

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample13.vb)]

Para probar esta funcionalidad, agregue un control GridView a la página bajo el `ProductsDeclarative` GridView existente. Establezca este nuevo `ID` de GridView en `ProductsProgrammatic` y, a través de su etiqueta inteligente, enlácelo a un nuevo ObjectDataSource denominado `ProductsDataSourceProgrammatic`. Configure ObjectDataSource para usar la clase `ProductsCL`, estableciendo las listas desplegables de las pestañas seleccionar y actualizar en `GetProducts` y `UpdateProduct`, respectivamente.

[![configurar ObjectDataSource para usar la clase ProductsCL](using-sql-cache-dependencies-vb/_static/image11.gif)](using-sql-cache-dependencies-vb/_static/image15.png)

**Figura 11**: configuración de ObjectDataSource para usar la clase `ProductsCL` ([haga clic para ver la imagen de tamaño completo](using-sql-cache-dependencies-vb/_static/image16.png))

[![Seleccione el método GetProducts en la lista desplegable seleccionar s.](using-sql-cache-dependencies-vb/_static/image12.gif)](using-sql-cache-dependencies-vb/_static/image17.png)

**Figura 12**: seleccione el método de `GetProducts` en la lista desplegable seleccionar s Tab ([haga clic para ver la imagen a tamaño completo](using-sql-cache-dependencies-vb/_static/image18.png))

[![elegir el método UpdateProduct en la lista desplegable actualizar pestaña s](using-sql-cache-dependencies-vb/_static/image13.gif)](using-sql-cache-dependencies-vb/_static/image19.png)

**Figura 13**: elija el método UpdateProduct en la lista desplegable actualizar pestaña s ([haga clic para ver la imagen a tamaño completo](using-sql-cache-dependencies-vb/_static/image20.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio creará BoundFields y CheckBoxFields en GridView para cada uno de los campos de datos. Al igual que con el primer GridView agregado a esta página, quite todos los campos, pero `ProductName`, `CategoryName`y `UnitPrice`, y dé formato a estos campos como considere oportunos. En la etiqueta inteligente de GridView s, active las casillas habilitar paginación, habilitar ordenación y habilitar edición. Al igual que con el `ProductsDataSourceDeclarative` ObjectDataSource, Visual Studio establecerá la propiedad `ProductsDataSourceProgrammatic` ObjectDataSource s `OldValuesParameterFormatString` en `original_{0}`. Para que la característica de edición de GridView funcione correctamente, vuelva a establecer esta propiedad en `{0}` (o quite la asignación de propiedad de la sintaxis declarativa por completo).

Después de completar estas tareas, el marcado declarativo de GridView y ObjectDataSource resultante debe ser similar al siguiente:

[!code-aspx[Main](using-sql-cache-dependencies-vb/samples/sample14.aspx)]

Para probar la dependencia de caché de SQL en la capa de almacenamiento en caché, establezca un punto de interrupción en el método `ProductCL` Class s `AddCacheItem` y, a continuación, inicie la depuración. La primera vez que visita `SqlCacheDependencies.aspx`, se debe alcanzar el punto de interrupción, ya que los datos se solicitan por primera vez y se colocan en la memoria caché. A continuación, desplácese a otra página de GridView u ordene una de las columnas. Esto hace que GridView reconsulte sus datos, pero los datos se deben encontrar en la memoria caché, ya que la tabla de base de datos de `Products` no se ha modificado. Si los datos no se encuentran repetidamente en la memoria caché, asegúrese de que hay suficiente memoria disponible en el equipo e inténtelo de nuevo.

Después de paginar a través de algunas páginas de GridView, abra una segunda ventana del explorador y vaya al tutorial de conceptos básicos de la sección edición, inserción y eliminación (`~/EditInsertDelete/Basics.aspx`). Actualice un registro de la tabla productos y, a continuación, desde la primera ventana del explorador, vea una nueva página o haga clic en uno de los encabezados de ordenación.

En este escenario, verá una de estas dos cosas: se alcanzará el punto de interrupción, lo que indica que los datos almacenados en caché se expulsón debido al cambio en la base de datos. o bien, no se alcanzará el punto de interrupción, lo que significa que `SqlCacheDependencies.aspx` ahora muestra los datos obsoletos. Si no se alcanza el punto de interrupción, es probable que el servicio de sondeo todavía no se haya desencadenado desde que se cambiaron los datos. Recuerde que el servicio de sondeo está comprobando los cambios en la tabla `Products` cada `pollTime` milisegundos, por lo que hay un retraso entre el momento en que se actualizan los datos subyacentes y el momento en que se expulsan los datos almacenados en caché.

> [!NOTE]
> Es más probable que este retraso aparezca al editar uno de los productos a través de GridView en `SqlCacheDependencies.aspx`. En los [datos de almacenamiento en caché en el tutorial de arquitectura](caching-data-in-the-architecture-vb.md) se ha agregado la `MasterCacheKeyArray` dependencia de caché para asegurarse de que los datos que se editan mediante el método `ProductsCL` clase s `UpdateProduct` se expulsó de la memoria caché. Sin embargo, reemplazamos esta dependencia de caché al modificar el método `AddCacheItem` anteriormente en este paso y, por lo tanto, la clase `ProductsCL` continuará mostrando los datos almacenados en caché hasta que el sistema de sondeo apunte el cambio a la tabla `Products`. Veremos cómo volver a introducir la dependencia de caché `MasterCacheKeyArray` en el paso 7.

## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>Paso 7: asociar varias dependencias a un elemento almacenado en caché

Recuerde que la dependencia de caché de `MasterCacheKeyArray` se usa para asegurarse de que *todos los* datos relacionados con el producto se expulsan de la memoria caché cuando se actualiza un único elemento asociado en él. Por ejemplo, el método `GetProductsByCategoryID(categoryID)` almacena en caché `ProductsDataTables` instancias para cada valor *CategoryID* único. Si se expulsa uno de estos objetos, el `MasterCacheKeyArray` dependencia de caché garantiza que los demás también se quiten. Sin esta dependencia de caché, cuando se modifican los datos en caché, existe la posibilidad de que otros datos de productos almacenados en caché no estén actualizados. Por lo tanto, es importante mantener el `MasterCacheKeyArray` dependencia de caché al usar las dependencias de caché de SQL. Sin embargo, el método de caché de datos s `Insert` solo permite un objeto de dependencia.

Además, al trabajar con dependencias de caché de SQL, es posible que necesitemos asociar varias tablas de base de datos como dependencias. Por ejemplo, la `ProductsDataTable` almacenada en la memoria caché de la clase `ProductsCL` contiene los nombres de categoría y proveedor de cada producto, pero el método `AddCacheItem` solo utiliza una dependencia en `Products`. En esta situación, si el usuario actualiza el nombre de una categoría o proveedor, los datos del producto almacenados en caché permanecerán en la memoria caché y no estarán actualizados. Por lo tanto, deseamos que los datos del producto almacenados en caché dependan no solo de la tabla `Products`, sino también de las tablas `Categories` y `Suppliers`.

La [clase`AggregateCacheDependency`](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx) proporciona un medio para asociar varias dependencias a un elemento de la memoria caché. Empiece por crear una instancia de `AggregateCacheDependency`. A continuación, agregue el conjunto de dependencias mediante el método `AggregateCacheDependency` s `Add`. Al insertar el elemento en la memoria caché de datos después, pase la instancia de `AggregateCacheDependency`. Cuando *alguna* de las dependencias de la instancia de `AggregateCacheDependency` cambie, se expulsará el elemento almacenado en caché.

A continuación se muestra el código actualizado para el método de `AddCacheItem` de clase `ProductsCL`. El método crea el `MasterCacheKeyArray` dependencia de caché junto con `SqlCacheDependency` objetos para las tablas `Products`, `Categories`y `Suppliers`. Todos ellos se combinan en uno `AggregateCacheDependency` objeto denominado `aggregateDependencies`, que se pasa al método `Insert`.

[!code-vb[Main](using-sql-cache-dependencies-vb/samples/sample15.vb)]

Pruebe este nuevo código. Ahora, los cambios en las tablas `Products`, `Categories`o `Suppliers` provocan que se expulsen los datos almacenados en caché. Además, el método de `UpdateProduct` de `ProductsCL` clase, al que se llama al editar un producto a través de GridView, expulsa la dependencia de la caché de `MasterCacheKeyArray`, lo que hace que se expulsen los `ProductsDataTable` almacenados en caché y que los datos se vuelvan a recuperar en la solicitud siguiente.

> [!NOTE]
> Las dependencias de caché de SQL también se pueden usar con el [almacenamiento en caché de resultados](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx). Para obtener una demostración de esta funcionalidad, vea: [usar el almacenamiento en caché de resultados de ASP.net con SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx).

## <a name="summary"></a>Resumen

Al almacenar en caché los datos de la base de datos, los datos se conservarán idealmente en la memoria caché hasta que se modifiquen en la base de datos. Con ASP.NET 2,0, se pueden crear y usar dependencias de caché de SQL en escenarios declarativos y de programación. Uno de los desafíos de este enfoque es detectar cuándo se han modificado los datos. Las versiones completas de Microsoft SQL Server 2005 proporcionan funciones de notificación que pueden avisar a una aplicación cuando ha cambiado el resultado de una consulta. En el caso de la edición Express de SQL Server 2005 y versiones anteriores de SQL Server, se debe usar en su lugar un sistema de sondeo. Afortunadamente, la configuración de la infraestructura de sondeo necesaria es bastante sencilla.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Uso de notificaciones de consulta en Microsoft SQL Server 2005](https://msdn.microsoft.com/library/ms175110.aspx)
- [Crear una notificación de consulta](https://msdn.microsoft.com/library/ms188669.aspx)
- [Almacenar en caché en ASP.NET con la clase `SqlCacheDependency`](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [Herramienta de registro de SQL Server de ASP.NET (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [Información general de `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Marko Rangel, Teresa Murphy y Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](caching-data-at-application-startup-vb.md)
