---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Controles de origen de datos | Microsoft Docs
author: microsoft
description: El control DataGrid de ASP.NET 1. x marcó una gran mejora en el acceso a datos en las aplicaciones Web. Sin embargo, no es tan fácil como podría haber sido...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: a2e2cfbec3e5aebf42a2de30bab7d45b4b610298
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519355"
---
# <a name="data-source-controls"></a>Controles de origen de datos

por [Microsoft](https://github.com/microsoft)

> El control DataGrid de ASP.NET 1. x marcó una gran mejora en el acceso a datos en las aplicaciones Web. Sin embargo, no es tan fácil como podría haber sido. Todavía se requiere una cantidad considerable de código para obtener una funcionalidad mucho más útil. Este es el modelo de todos los esfuerzos de acceso a datos en 1. x.

El control DataGrid de ASP.NET 1. x marcó una gran mejora en el acceso a datos en las aplicaciones Web. Sin embargo, no es tan fácil como podría haber sido. Todavía se requiere una cantidad considerable de código para obtener una funcionalidad mucho más útil. Este es el modelo de todos los esfuerzos de acceso a datos en 1. x.

ASP.NET 2,0 lo soluciona en parte con los controles de origen de datos. Los controles de origen de datos de ASP.NET 2,0 proporcionan a los desarrolladores un modelo declarativo para recuperar datos, Mostrar datos y editar datos. El propósito de los controles de origen de datos es proporcionar una representación coherente de los datos a los controles enlazados a datos, independientemente del origen de dichos datos. En el núcleo de los controles de origen de datos en ASP.NET 2,0 es la clase abstracta DataSourceControl. La clase DataSourceControl proporciona una implementación base de la interfaz IDataSource y la interfaz IListSource, que permite asignar el control de origen de datos como el origen de datos de un control enlazado a datos (a través de la nueva propiedad DataSourceId se describe más adelante) y se exponen los datos en la lista. Cada lista de datos de un control de origen de datos se expone como un objeto DataSourceView. La interfaz IDataSource proporciona el acceso a las instancias de DataSourceView. Por ejemplo, el método GetViewNames devuelve un ICollection que le permite enumerar los elementos datasourceviews asociados a un control de origen de datos determinado y el método GetView (permite tener acceso a una instancia determinada de DataSourceView por nombre.

Los controles de origen de datos no tienen interfaz de usuario. Se implementan como controles de servidor para que puedan admitir la sintaxis declarativa y para que tengan acceso al estado de la página si se desea. Los controles de origen de datos no representan ninguna marca HTML en el cliente.

> [!NOTE]
> Como verá más adelante, también hay ventajas de almacenamiento en caché que se obtienen mediante el uso de controles de origen de datos.

## <a name="storing-connection-strings"></a>Almacenar cadenas de conexión

Antes de ver cómo configurar los controles de origen de datos, debemos tratar una nueva funcionalidad en ASP.NET 2,0 con respecto a las cadenas de conexión. ASP.NET 2,0 introduce una nueva sección en el archivo de configuración que le permite almacenar fácilmente cadenas de conexión que se pueden leer dinámicamente en tiempo de ejecución. La sección &lt;connectionStrings&gt; facilita el almacenamiento de cadenas de conexión.

El siguiente fragmento de código agrega una nueva cadena de conexión.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Al igual que con la sección &lt;appSettings&gt;, la sección &lt;connectionStrings&gt; se muestra fuera de la sección &lt;System. Web&gt; del archivo de configuración.

Para usar esta cadena de conexión, puede usar la siguiente sintaxis al establecer el atributo ConnectionString de un control de servidor.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

También se puede cifrar la sección &lt;connectionStrings&gt; para que no se exponga información confidencial. Esta capacidad se tratará en un módulo posterior.

## <a name="caching-data-sources"></a>Almacenar en caché orígenes de datos

Cada DataSourceControl proporciona cuatro propiedades para configurar el almacenamiento en caché; EnableCaching, CacheDuration, CacheExpirationPolicy y CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching es una propiedad booleana que determina si el almacenamiento en caché está habilitado o no para el control de origen de datos.

## <a name="cacheduration-property"></a>CacheDuration (propiedad)

La propiedad CacheDuration establece el número de segundos que la memoria caché sigue siendo válida. Al establecer esta propiedad en **0** , la memoria caché sigue siendo válida hasta que se invalida explícitamente.

## <a name="cacheexpirationpolicy-property"></a>Propiedad CacheExpirationPolicy

La propiedad CacheExpirationPolicy se puede establecer en **Absolute** o en **variable**. Si se establece en absoluto, significa que la cantidad máxima de tiempo que los datos se almacenarán en la memoria caché es el número de segundos especificado por la propiedad CacheDuration. Al establecerlo en variable, se restablece la hora de expiración cuando se realiza cada operación.

## <a name="cachekeydependency-property"></a>Propiedad CacheKeyDependency

Si se especifica un valor de cadena para la propiedad CacheKeyDependency, ASP.NET configurará una nueva dependencia de caché basada en esa cadena. Esto le permite invalidar la memoria caché explícitamente simplemente cambiando o quitando el CacheKeyDependency.

**Importante**: Si la suplantación está habilitada y el acceso al origen de datos o al contenido de los datos se basa en la identidad del cliente, se recomienda deshabilitar el almacenamiento en caché estableciendo EnableCaching en false. Si el almacenamiento en caché está habilitado en este escenario y un usuario que no sea el usuario que solicitó originalmente los datos emite una solicitud, no se aplica la autorización para el origen de datos. Los datos simplemente se servirán desde la memoria caché.

## <a name="the-sqldatasource-control"></a>Control SqlDataSource

El control SqlDataSource permite a los desarrolladores tener acceso a los datos almacenados en cualquier base de datos relacional que admita ADO.NET. Puede usar el proveedor System. Data. SqlClient para tener acceso a una base de datos SQL Server, al proveedor System. Data. OleDb, al proveedor System. Data. ODBC o al proveedor System. Data. OracleClient para tener acceso a Oracle. Por lo tanto, no solo se usa SqlDataSource para tener acceso a los datos de una base de datos de SQL Server.

Para usar SqlDataSource, simplemente proporcione un valor para la propiedad ConnectionString y especifique un comando SQL o un procedimiento almacenado. El control SqlDataSource se encarga de trabajar con la arquitectura de ADO.NET subyacente. Se abre la conexión, se consulta el origen de datos o se ejecuta el procedimiento almacenado, se devuelven los datos y, a continuación, se cierra la conexión.

> [!NOTE]
> Dado que la clase DataSourceControl cierra automáticamente la conexión, debe reducir el número de llamadas de cliente que se generan al filtrar las conexiones de base de datos.

El siguiente fragmento de código enlaza un control DropDownList a un control SqlDataSource mediante la cadena de conexión que se almacena en el archivo de configuración, tal y como se mostró anteriormente.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Como se muestra arriba, la propiedad DataSourceMode de SqlDataSource especifica el modo para el origen de datos. En el ejemplo anterior, DataSourceMode se establece en DataReader. En ese caso, SqlDataSource devolverá un objeto IDataReader mediante un cursor de solo avance y de solo lectura. El proveedor que se usa controla el tipo de objeto especificado que se devuelve. En este caso, se usa el proveedor System. Data. SqlClient tal como se especifica en la sección &lt;connectionStrings&gt; del archivo Web. config. Por lo tanto, el objeto devuelto será de tipo SqlDataReader. Al especificar un valor de DataSourceMode de DataSet, los datos se pueden almacenar en un conjunto de datos en el servidor. Este modo le permite agregar características como ordenación, paginación, etc. Si hubiese enlazado a datos el objeto SqlDataSource a un control GridView, habría elegido el modo DataSet. Sin embargo, en el caso de un DropDownList, el modo DataReader es la opción correcta.

> [!NOTE]
> Al almacenar en caché un objeto SqlDataSource o AccessDataSource, la propiedad DataSourceMode debe establecerse en DataSet. Se producirá una excepción si habilita el almacenamiento en caché con un DataSourceMode de DataReader.

## <a name="sqldatasource-properties"></a>Propiedades de SqlDataSource

A continuación se muestran algunas de las propiedades del control SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Valor booleano que especifica si se cancela un comando SELECT Si uno de los parámetros es NULL. De forma predeterminada es True.

### <a name="conflictdetection"></a>ConflictDetection

En una situación en la que varios usuarios pueden estar actualizando un origen de datos al mismo tiempo, la propiedad ConflictDetection determina el comportamiento del control SqlDataSource. Esta propiedad se evalúa como uno de los valores de la enumeración ConflictOptions. Esos valores son **CompareAllValues** y **OverwriteChanges**. Si se establece en OverwriteChanges, la última persona que escriba los datos en el origen de datos sobrescribirá los cambios anteriores. Sin embargo, si la propiedad ConflictDetection se establece en CompareAllValues, los parámetros que se crean para las columnas devueltas por SelectCommand y los parámetros también se crean para contener los valores originales en cada una de esas columnas, lo que permite que SqlDataSource Determine si los valores han cambiado desde que se ejecutó SelectCommand.

### <a name="deletecommand"></a>DeleteCommand

Establece u obtiene la cadena de SQL utilizada al eliminar filas de la base de datos. Puede ser una consulta SQL o un nombre de procedimiento almacenado.

### <a name="deletecommandtype"></a>DeleteCommandType

Establece u obtiene el tipo de comando DELETE, ya sea una consulta SQL (texto) o un procedimiento almacenado (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Devuelve los parámetros que usa el DeleteCommand del objeto SqlDataSourceView asociado al control SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

Esta propiedad se usa para especificar el formato de los parámetros de valor original en los casos en los que la propiedad ConflictDetection se establece en CompareAllValues. El valor predeterminado es {0}, lo que significa que los parámetros de valor original tomarán el mismo nombre que el parámetro original. En otras palabras, si el nombre del campo es EmployeeID, el parámetro de valor original sería @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Establece u obtiene la cadena de SQL que se utiliza para recuperar datos de la base de datos. Puede ser una consulta SQL o un nombre de procedimiento almacenado.

### <a name="selectcommandtype"></a>SelectCommandType

Establece u obtiene el tipo de comando SELECT, ya sea una consulta SQL (texto) o un procedimiento almacenado (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Devuelve los parámetros que usa SelectCommand del objeto SqlDataSourceView asociado al control SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Obtiene o establece el nombre de un parámetro de procedimiento almacenado que se utiliza al ordenar los datos recuperados por el control de origen de datos. Solo es válido cuando SelectCommandType se establece en StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Cadena delimitada por signos de punto y coma que especifica las bases de datos y tablas que se usan en una SQL Server dependencia de caché. (Las dependencias de caché de SQL se tratarán en un módulo posterior).

### <a name="updatecommand"></a>UpdateCommand

Establece u obtiene la cadena de SQL que se utiliza al actualizar los datos de la base de datos. Puede ser una consulta SQL o un nombre de procedimiento almacenado.

### <a name="updatecommandtype"></a>UpdateCommandType

Establece u obtiene el tipo de comando de actualización, ya sea una consulta SQL (texto) o un procedimiento almacenado (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Devuelve los parámetros utilizados por UpdateCommand del objeto SqlDataSourceView asociado al control SqlDataSource.

## <a name="the-accessdatasource-control"></a>Control AccessDataSource

El control AccessDataSource deriva de la clase SqlDataSource y se utiliza para enlazar datos a una base de datos de Microsoft Access. La propiedad ConnectionString del control AccessDataSource es una propiedad de solo lectura. En lugar de usar la propiedad ConnectionString, la propiedad archivo de datos se utiliza para apuntar a la base de datos de Access como se muestra a continuación.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

El AccessDataSource siempre establecerá el ProviderName de la base SqlDataSource en System. Data. OleDb y se conectará a la base de datos mediante el proveedor de OLE DB Microsoft. Jet. OLEDB. 4.0. No se puede usar el control AccessDataSource para conectarse a una base de datos de acceso protegido por contraseña. Si tiene que conectarse a una base de datos protegida por contraseña, debe utilizar el control SqlDataSource.

> [!NOTE]
> Las bases de datos de Access almacenadas en el sitio web deben colocarse en el directorio App\_Data. ASP.NET no permite examinar los archivos de este directorio. Deberá conceder a la cuenta de proceso permisos de lectura y escritura en el directorio de datos de la aplicación\_al usar las bases de datos de Access.

## <a name="the-xmldatasource-control"></a>El control XmlDataSource

XmlDataSource se utiliza para enlazar datos XML a los controles enlazados a datos. Puede enlazar a un archivo XML mediante la propiedad archivo de datos o puede enlazar a una cadena XML mediante la propiedad data. XmlDataSource expone los atributos XML como campos enlazables. En los casos en los que necesite enlazar a valores que no están representados como atributos, deberá usar una transformación XSL. También puede utilizar expresiones XPath para filtrar los datos XML.

Considere el siguiente archivo XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Tenga en cuenta que XmlDataSource usa una propiedad XPath de *People/Person* para filtrar solo los nodos &lt;Person&gt;. A continuación, el DropDownList enlaza los datos al atributo LastName mediante la propiedad DataTextField.

Aunque el control XmlDataSource se utiliza principalmente para enlazar datos a datos XML de solo lectura, es posible editar el archivo de datos XML. Tenga en cuenta que, en estos casos, la inserción, actualización y eliminación automática de información en el archivo XML no se realiza automáticamente como sucede con otros controles de origen de datos. En su lugar, tendrá que escribir código para editar manualmente los datos mediante los siguientes métodos del control XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Recupera un objeto XmlDocument que contiene el código XML recuperado por XmlDataSource.

### <a name="save"></a>Guardar

Vuelve a guardar el objeto XmlDocument en memoria en el origen de datos.

Es importante saber que el método Save solo funcionará cuando se cumplan las dos condiciones siguientes:

1. XmlDataSource está utilizando la propiedad archivo de datos para enlazar con un archivo XML en lugar de la propiedad data para enlazar a datos XML en memoria.
2. No se especifica ninguna transformación a través de la propiedad Transform o TransformFile.

Tenga en cuenta también que el método Save puede producir resultados inesperados cuando lo llaman varios usuarios simultáneamente.

## <a name="the-objectdatasource-control"></a>Control ObjectDataSource

Los controles de origen de datos que hemos tratado hasta este punto son excelentes opciones para las aplicaciones de dos niveles en las que el control de origen de datos se comunica directamente con el almacén de datos. Sin embargo, muchas aplicaciones del mundo real son aplicaciones de niveles múltiples en las que un control de origen de datos puede necesitar comunicarse con un objeto comercial que, a su vez, se comunica con la capa de datos. En estas situaciones, el ObjectDataSource rellena la factura de buena. ObjectDataSource funciona junto con un objeto de origen. El control ObjectDataSource creará una instancia del objeto de origen, llamará al método especificado y eliminará la instancia de objeto en el ámbito de una única solicitud, si el objeto tiene métodos de instancia en lugar de métodos estáticos (compartidos en Visual Basic). Por lo tanto, el objeto debe ser sin estado. Es decir, el objeto debe adquirir y liberar todos los recursos necesarios en el intervalo de una única solicitud. Puede controlar cómo se crea el objeto de origen mediante el control del evento ObjectCreating del control ObjectDataSource. Puede crear una instancia del objeto de origen y, a continuación, establecer la propiedad ObjectInstance de la clase ObjectDataSourceEventArgs en esa instancia. El control ObjectDataSource usará la instancia que se crea en el evento ObjectCreating en lugar de crear una instancia por sí misma.

Si el objeto de origen de un control ObjectDataSource expone métodos estáticos públicos (compartidos en Visual Basic) a los que se puede llamar para recuperar y modificar datos, un control ObjectDataSource llamará directamente a esos métodos. Si un control ObjectDataSource debe crear una instancia del objeto de origen para realizar llamadas a métodos, el objeto debe incluir un constructor público que no tome ningún parámetro. El control ObjectDataSource llamará a este constructor cuando cree una nueva instancia del objeto de origen.

Si el objeto de origen no contiene un constructor público sin parámetros, puede crear una instancia del objeto de origen que usará el control ObjectDataSource en el evento ObjectCreating.

## <a name="specifying-object-methods"></a>Especificar métodos de objeto

El objeto de origen de un control ObjectDataSource puede contener cualquier número de métodos que se usan para seleccionar, insertar, actualizar o eliminar datos. El control ObjectDataSource llama a estos métodos basándose en el nombre del método, como se identifica mediante la propiedad SelectMethod, InsertMethod, UpdateMethod o DeleteMethod del control ObjectDataSource. El objeto de origen también puede incluir un método SelectCount opcional, que se identifica mediante el control ObjectDataSource mediante la propiedad SelectCountMethod, que devuelve el recuento del número total de objetos en el origen de datos. El control ObjectDataSource llamará al método SelectCount una vez que se haya llamado a un método SELECT para recuperar el número total de registros en el origen de datos que se utilizarán al paginar.

## <a name="lab-using-data-source-controls"></a>Laboratorio usando controles de origen de datos

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Ejercicio 1: Mostrar datos con el control SqlDataSource

En el siguiente ejercicio se usa el control SqlDataSource para conectarse a la base de datos Northwind. Se supone que tiene acceso a la base de datos Northwind en una instancia de SQL Server 2000.

1. Cree un nuevo sitio web ASP.NET.
2. Agregue un nuevo archivo Web. config.

    1. Haga clic con el botón derecho en el proyecto en Explorador de soluciones y haga clic en Agregar nuevo elemento.
    2. Elija archivo de configuración Web en la lista de plantillas y haga clic en Agregar.
3. Edite la sección &lt;connectionStrings&gt; de la siguiente manera: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Cambie a la vista de código y agregue un atributo ConnectionString y un atributo SelectCommand al control &lt;ASP: SqlDataSource&gt; como se indica a continuación: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. En Vista de diseño, agregue un nuevo control GridView.
6. En la lista desplegable elegir origen de datos en el menú tareas de GridView, elija SqlDataSource1.
7. Haga clic con el botón secundario en default. aspx y elija ver en el explorador en el menú. Haga clic en sí cuando se le pida que guarde.
8. GridView muestra los datos de la tabla Products.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Ejercicio 2-edición de datos con el control SqlDataSource

En el ejercicio siguiente se muestra cómo enlazar datos a un control DropDownList mediante la sintaxis declarativa y permite editar los datos presentados en el control DropDownList.

1. En Vista de diseño, elimine el control GridView de default. aspx. 

    **Importante**: deje el control SqlDataSource en la página.
2. Agregue un control DropDownList a default. aspx.
3. Cambie a la vista Código fuente.
4. Agregue un atributo DataSourceId, DataTextField y DataValueField al control &lt;ASP: DropDownList&gt; como se indica a continuación: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Guarde default. aspx y verlo en el explorador. Tenga en cuenta que DropDownList contiene todos los productos de la base de datos Northwind.
6. Cierre el explorador.
7. En la vista Código fuente de default. aspx, agregue un nuevo control TextBox debajo del control DropDownList. Cambie la propiedad ID del cuadro de texto a txtProductName.
8. En el control de cuadro de texto, agregue un nuevo control de botón. Cambie la propiedad ID del botón a btnUpdate y la propiedad Text para **actualizar el nombre del producto**.
9. En la vista Código fuente de default. aspx, agregue una propiedad UpdateCommand y dos nuevos UpdateParameters a la etiqueta SqlDataSource de la manera siguiente: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Tenga en cuenta que en este código se agregan dos parámetros de actualización (ProductName y ProductID). Estos parámetros se asignan a la propiedad text del cuadro de texto txtProductName y la propiedad SelectedValue del DropDownList ddlProducts.
10. Cambie a Vista de diseño y haga doble clic en el control de botón para agregar un controlador de eventos.
11. Agregue el código siguiente a btnUpdate\_haga clic en código: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Haga clic con el botón secundario en default. aspx y elija verlo en el explorador. Haga clic en sí cuando se le pida que guarde todos los cambios.
13. Las clases parciales de ASP.NET 2,0 permiten la compilación en tiempo de ejecución. No es necesario compilar una aplicación para ver que los cambios de código surten efecto.
14. Seleccione un producto de la DropDownList.
15. Escriba un nuevo nombre para el producto seleccionado en el cuadro de texto y, a continuación, haga clic en el botón actualizar.
16. El nombre del producto se actualiza en la base de datos.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Ejercicio 3 uso del control ObjectDataSource

En este ejercicio se muestra cómo usar el control ObjectDataSource y un objeto de origen para interactuar con la base de datos Northwind.

1. Haga clic con el botón derecho en el proyecto en Explorador de soluciones y haga clic en Agregar nuevo elemento.
2. Seleccione Web Forms en la lista de plantillas. Cambie el nombre a Object. aspx y haga clic en Agregar.
3. Haga clic con el botón derecho en el proyecto en Explorador de soluciones y haga clic en Agregar nuevo elemento.
4. Seleccione clase en la lista de plantillas. Cambie el nombre de la clase a NorthwindData.cs y haga clic en Agregar.
5. Haga clic en sí cuando se le pida que agregue la clase a la carpeta de código del\_de la aplicación.
6. Agregue el código siguiente al archivo NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Agregue el código siguiente a la vista de origen de Object. aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Guarde todos los archivos y examine Object. aspx.
9. Interactúe con la interfaz viendo los detalles, editando los empleados, agregando empleados y eliminando empleados.
