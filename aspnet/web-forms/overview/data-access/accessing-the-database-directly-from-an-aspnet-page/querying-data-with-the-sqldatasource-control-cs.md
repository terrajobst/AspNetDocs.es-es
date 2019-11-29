---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
title: Consultar datos con el control SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores usamos el control ObjectDataSource para separar completamente el nivel de presentación de la capa de acceso a datos. A partir de este tutor...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 60512d6a-b572-4b7a-beb3-3e44b4d2020c
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 5bda42965f7d1db71b207c0b76e251b8fff64e31
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74606227"
---
# <a name="querying-data-with-the-sqldatasource-control-c"></a>Consultar datos con el control SqlDataSource (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_CS.exe) de la aplicación de ejemplo o [descarga de PDF](querying-data-with-the-sqldatasource-control-cs/_static/datatutorial47cs1.pdf)

> En los tutoriales anteriores usamos el control ObjectDataSource para separar completamente el nivel de presentación de la capa de acceso a datos. A partir de este tutorial, se explica cómo se puede usar el control SqlDataSource para aplicaciones sencillas que no requieren una separación estricta de la presentación y el acceso a los datos.

## <a name="introduction"></a>Introducción

Todos los tutoriales que hemos examinado hasta ahora han usado una arquitectura en capas que consta de la presentación, la lógica empresarial y las capas de acceso a los datos. La capa de acceso a datos (DAL) se diseñó en el primer tutorial ([creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-cs.md)) y el nivel de lógica de negocios en el segundo ([creación de una capa de lógica de negocios](../introduction/creating-a-business-logic-layer-cs.md)). A partir del tutorial de [visualización de datos con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) , vimos cómo usar el nuevo control ObjectDataSource de ASP.net 2,0 s para interactuar mediante declaración con la arquitectura de la capa de presentación.

Aunque todos los tutoriales hasta ahora han usado la arquitectura para trabajar con datos, también es posible obtener acceso, insertar, actualizar y eliminar datos de la base de datos directamente desde una página de ASP.NET, omitiendo la arquitectura. Al hacerlo, se colocan las consultas de base de datos y la lógica de negocios específicas directamente en la Página Web. En el caso de aplicaciones suficientemente grandes o complejas, el diseño, la implementación y el uso de una arquitectura en capas es fundamental para el éxito, la actualización y el mantenimiento de la aplicación. Sin embargo, es posible que el desarrollo de una arquitectura sólida no sea necesario al crear aplicaciones de un solo uso de un solo uso.

ASP.NET 2,0 proporciona cinco controles de origen de datos integrados [SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx), [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx), [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx), [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)y [SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx). SqlDataSource se puede usar para obtener acceso a los datos y modificarlos directamente desde una base de datos relacional, como Microsoft SQL Server, Microsoft Access, Oracle, MySQL y otros. En este tutorial y en los tres siguientes, examinaremos cómo trabajar con el control SqlDataSource, exploraremos Cómo consultar y filtrar datos de la base de datos, y cómo usar SqlDataSource para insertar, actualizar y eliminar datos.

![ASP.NET 2,0 incluye cinco controles de origen de datos integrados](querying-data-with-the-sqldatasource-control-cs/_static/image1.gif)

**Figura 1**: ASP.net 2,0 incluye cinco controles de origen de datos integrados

## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>Comparar ObjectDataSource y SqlDataSource

Conceptualmente, los controles ObjectDataSource y SqlDataSource son simplemente proxies de datos. Como se describe en el tutorial [Mostrar datos con ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-cs.md) , ObjectDataSource tiene propiedades que indican el tipo de objeto que proporciona los datos y los métodos que se van a invocar para seleccionar, insertar, actualizar y eliminar datos del tipo de objeto subyacente. Una vez configuradas las propiedades de ObjectDataSource s, se puede enlazar un control Web de datos como GridView, DetailsView o DataList al control mediante los métodos ObjectDataSource `Select()`, `Insert()`, `Delete()`y `Update()` para interactuar con la arquitectura subyacente.

SqlDataSource proporciona la misma funcionalidad, pero funciona en una base de datos relacional en lugar de en una biblioteca de objetos. Con SqlDataSource, se debe especificar la cadena de conexión de base de datos y las consultas SQL ad hoc o los procedimientos almacenados que se van a ejecutar para insertar, actualizar, eliminar y recuperar datos. Los métodos SqlDataSource s `Select()`, `Insert()`, `Update()`y `Delete()`, cuando se invocan, se conectan a la base de datos especificada y emiten la consulta SQL adecuada. Como se muestra en el siguiente diagrama, estos métodos hacen el trabajo pesado de conectarse a una base de datos, emitir una consulta y devolver los resultados.

![SqlDataSource sirve como proxy para la base de datos](querying-data-with-the-sqldatasource-control-cs/_static/image2.gif)

**Figura 2**: SqlDataSource actúa como proxy en la base de datos

> [!NOTE]
> En este tutorial nos centraremos en la recuperación de datos de la base de datos. En el tutorial [Insertar, actualizar y eliminar datos con el control SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md) , veremos cómo configurar SqlDataSource para que admita la inserción, actualización y eliminación.

## <a name="the-sqldatasource-and-accessdatasource-controls"></a>Los controles SqlDataSource y AccessDataSource

Además del control SqlDataSource, ASP.NET 2,0 también incluye un control AccessDataSource. Estos dos controles diferentes conducen a muchos desarrolladores nuevos de ASP.NET 2,0 para sospechar que el control AccessDataSource está diseñado para trabajar exclusivamente con Microsoft Access con el control SqlDataSource diseñado para trabajar exclusivamente con Microsoft SQL Server. Aunque el AccessDataSource está diseñado para trabajar específicamente con Microsoft Access, el control SqlDataSource funciona con *cualquier* base de datos relacional a la que se pueda tener acceso a través de .net. Esto incluye los almacenes de datos compatibles con OleDb u ODBC, como Microsoft SQL Server, Microsoft Access, Oracle, Informix, MySQL y PostgreSQL, entre muchos otros.

La única diferencia entre los controles AccessDataSource y SqlDataSource es cómo se especifica la información de conexión a la base de datos. El control AccessDataSource solo necesita la ruta de acceso al archivo de base de datos de Access. Por otro lado, SqlDataSource requiere una cadena de conexión completa.

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>Paso 1: creación de las páginas web de SqlDataSource

Antes de empezar a explorar cómo trabajar directamente con los datos de la base de datos mediante el control SqlDataSource, deje que primero dedique un momento a crear las páginas de ASP.NET en nuestro proyecto de sitio web que necesitamos para este tutorial y los tres siguientes. Comience agregando una nueva carpeta denominada `SqlDataSource`. A continuación, agregue las siguientes páginas de ASP.NET a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`

![Agregue las páginas ASP.NET para los tutoriales relacionados con SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image3.gif)

**Figura 3**: agregar las páginas ASP.net para los tutoriales relacionados con SqlDataSource

Al igual que en las otras carpetas, `Default.aspx` en la carpeta `SqlDataSource` mostrará los tutoriales en su sección. Recuerde que el control de usuario `SectionLevelTutorialListing.ascx` proporciona esta funcionalidad. Por lo tanto, agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta la página s Vista de diseño.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](querying-data-with-the-sqldatasource-control-cs/_static/image5.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image4.gif)

**Ilustración 4**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](querying-data-with-the-sqldatasource-control-cs/_static/image6.gif))

Por último, agregue estas cuatro páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después de agregar los botones personalizados a DataList y Repeater `<siteMapNode>`:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample1.sql)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para los tutoriales de edición, inserción y eliminación.

![El mapa del sitio ahora incluye entradas para los tutoriales de SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image7.gif)

**Figura 5**: ahora, el mapa del sitio incluye entradas para los tutoriales de SqlDataSource

## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>Paso 2: agregar y configurar el control SqlDataSource

Para empezar, abra la página `Querying.aspx` en la carpeta `SqlDataSource` y cambie a Vista de diseño. Arrastre un control SqlDataSource desde el cuadro de herramientas hasta el diseñador y establezca su `ID` en `ProductsDataSource`. Como con ObjectDataSource, SqlDataSource no genera ninguna salida representada y, por tanto, aparece como un cuadro gris en la superficie de diseño. Para configurar SqlDataSource, haga clic en el vínculo configurar origen de datos de la etiqueta inteligente SqlDataSource s.

![Haga clic en el vínculo configurar origen de datos de la etiqueta inteligente SqlDataSource s](querying-data-with-the-sqldatasource-control-cs/_static/image8.gif)

**Figura 6**: haga clic en el vínculo configurar origen de datos de la etiqueta inteligente SqlDataSource s

Se abre el Asistente para configurar el origen de datos de control SqlDataSource. Aunque los pasos del asistente se diferencian del control ObjectDataSource s, el objetivo final es el mismo para proporcionar los detalles sobre cómo recuperar, insertar, actualizar y eliminar datos a través del origen de datos. En el caso de SqlDataSource, esto implica especificar la base de datos subyacente que se va a utilizar y proporcionar instrucciones SQL ad hoc o procedimientos almacenados.

En el primer paso del asistente nos solicita la base de datos. La lista desplegable incluye las bases de datos que se encuentran en la carpeta de `App_Data` de la aplicación web y las que se han agregado al nodo conexiones de datos en el Explorador de servidores. Dado que ya hemos agregado una cadena de conexión para la base de datos de `NORTHWIND.MDF` en la carpeta `App_Data` a nuestro archivo `Web.config` de proyectos, la lista desplegable incluye una referencia a esa cadena de conexión, `NORTHWINDConnectionString`. Elija este elemento en la lista desplegable y haga clic en siguiente.

![Elija NORTHWINDConnectionString en la lista desplegable.](querying-data-with-the-sqldatasource-control-cs/_static/image9.gif)

**Figura 7**: elegir el `NORTHWINDConnectionString` de la lista desplegable

Después de elegir la base de datos, el asistente solicita que la consulta devuelva datos. Podemos especificar las columnas de una tabla o vista que se van a devolver o puede escribir una instrucción SQL personalizada o especificar un procedimiento almacenado. Puede alternar entre esta opción a través de los botones de radio especificar una instrucción SQL personalizada o un procedimiento almacenado y especificar columnas de una tabla o vista.

> [!NOTE]
> En este primer ejemplo, vamos a usar la opción especificar columnas de una tabla o vista. Volveremos al asistente más adelante en este tutorial y exploraremos la opción especificar una instrucción SQL o un procedimiento almacenado personalizado.

En la figura 8 se muestra la pantalla configurar la instrucción SELECT cuando se selecciona el botón de radio especificar columnas de una tabla o vista. La lista desplegable contiene el conjunto de tablas y vistas de la base de datos Northwind, con la tabla seleccionada o las columnas View s mostradas en la lista de casillas siguiente. En este ejemplo, vamos a devolver las columnas `ProductID`, `ProductName`y `UnitPrice` de la tabla `Products`. Como se muestra en la figura 8, después de efectuar estas selecciones, el asistente muestra la instrucción SQL resultante `SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`.

![Devolver datos de la tabla Products](querying-data-with-the-sqldatasource-control-cs/_static/image10.gif)

**Figura 8**: datos devueltos de la tabla `Products`

Una vez configurado el Asistente para devolver las columnas `ProductID`, `ProductName`y `UnitPrice` de la tabla `Products`, haga clic en el botón siguiente. Esta última pantalla proporciona una oportunidad para examinar los resultados de la consulta configurada en el paso anterior. Al hacer clic en el botón probar consulta, se ejecuta la instrucción de `SELECT` configurada y se muestran los resultados en una cuadrícula.

![Haga clic en el botón probar consulta para revisar la consulta SELECT.](querying-data-with-the-sqldatasource-control-cs/_static/image11.gif)

**Figura 9**: haga clic en el botón probar consulta para revisar la `SELECT` consulta

Para finalizar el asistente, haga clic en Finalizar.

Al igual que con ObjectDataSource, el Asistente de SqlDataSource s simplemente asigna valores a las propiedades control s, es decir, las propiedades [`ConnectionString`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx) y [`SelectCommand`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx) . Después de completar el asistente, el marcado declarativo del control SqlDataSource debe ser similar al siguiente:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample2.aspx)]

La propiedad `ConnectionString` proporciona información sobre cómo conectarse a la base de datos. A esta propiedad se le puede asignar un valor completo de cadena de conexión codificada de forma rígida o puede apuntar a una cadena de conexión en `Web.config`. Para hacer referencia a un valor de cadena de conexión en Web. config, use la sintaxis `<%$ expressionPrefix:expressionValue %>`. Normalmente, *expressionPrefix* es connectionStrings y *expressionValue* es el nombre de la cadena de conexión en la [sección `Web.config``<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx). Sin embargo, se puede usar la sintaxis para hacer referencia a `<appSettings>` elementos o al contenido de los archivos de recursos. Vea [información general sobre las expresiones ASP.net](https://msdn.microsoft.com/library/d5bd1tad.aspx) para más información sobre esta sintaxis.

La propiedad `SelectCommand` especifica la instrucción SQL ad hoc o el procedimiento almacenado que se va a ejecutar para devolver los datos.

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>Paso 3: agregar un control Web de datos y enlazarlo a SqlDataSource

Una vez que se ha configurado SqlDataSource, se puede enlazar a un control Web de datos, como GridView o DetailsView. En este tutorial, vamos a mostrar los datos en un control GridView. En el cuadro de herramientas, arrastre un control GridView a la página y enlácelo al `ProductsDataSource` SqlDataSource seleccionando el origen de datos en la lista desplegable de la etiqueta inteligente GridView s.

[![agregar un control GridView y enlazarlo al control SqlDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image13.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image12.gif)

**Figura 10**: agregar un control GridView y enlazarlo al control SqlDataSource ([haga clic para ver la imagen de tamaño completo](querying-data-with-the-sqldatasource-control-cs/_static/image14.gif))

Una vez que haya seleccionado el control SqlDataSource en la lista desplegable de la etiqueta inteligente de GridView s, Visual Studio agregará automáticamente un CheckBoxField a GridView para cada una de las columnas devueltas por el control de origen de datos. Dado que SqlDataSource devuelve tres columnas de base de datos `ProductID`, `ProductName`y `UnitPrice` hay tres campos en GridView.

Dedique un momento a configurar los tres BoundFields de GridView. Cambie la propiedad `ProductName` campo s `HeaderText` por nombre de producto y el campo `UnitPrice` a precio. Formatee también el campo de `UnitPrice` como moneda. Después de realizar estas modificaciones, el marcado declarativo de GridView s debe ser similar al siguiente:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample3.aspx)]

Visite esta página a través de un explorador. Como se muestra en la figura 11, el control GridView muestra cada uno de los valores de `ProductID`, `ProductName`y `UnitPrice`.

[![GridView muestra todos los valores Product s ProductID, ProductName y UnitPrice](querying-data-with-the-sqldatasource-control-cs/_static/image16.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image15.gif)

**Figura 11**: GridView muestra cada uno de los valores de `ProductID`, `ProductName`y `UnitPrice` de los productos ([haga clic para ver la imagen de tamaño completo](querying-data-with-the-sqldatasource-control-cs/_static/image17.gif))

Cuando se visita la página, GridView invoca su método de control de origen de datos s `Select()`. Cuando se usaba el control ObjectDataSource, este llamó al método `ProductsBLL` Class s `GetProducts()`. Sin embargo, con SqlDataSource, el método `Select()` establece una conexión con la base de datos especificada y emite el `SelectCommand` (`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`, en este ejemplo). SqlDataSource devuelve los resultados que el control GridView enumera, y crea una fila en GridView para cada registro de base de datos devuelto.

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>Características integradas de control Web de datos y el control SqlDataSource

En general, las características inherentes a los controles Web de datos de paginación, ordenación, edición, eliminación, inserción, etc., son específicas del control Web de datos y no dependen del control de origen de datos utilizado. Es decir, GridView puede utilizar su paginación, ordenación, edición y eliminación integradas, tanto si está enlazada a una clase ObjectDataSource como a SqlDataSource. Sin embargo, algunas características de control Web de datos son sensibles al control de origen de datos que se está utilizando o a la configuración del control de origen de datos.

Por ejemplo, en la página de [paginación eficaz a través de grandes cantidades de datos](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs.md) , se describe cómo, de forma predeterminada, la lógica de paginación de los controles Web de datos devuelve *todos* los registros del origen de datos subyacente y, a continuación, muestra solo el subconjunto de registros adecuado dado el índice de la página actual y el número de registros que se van a mostrar por página. Este modelo es muy ineficaz cuando se pagina a través de conjuntos de resultados suficientemente grandes. Afortunadamente, ObjectDataSource puede configurarse para admitir la paginación personalizada, que solo devuelve el subconjunto preciso de los registros que se van a mostrar. El control SqlDataSource, sin embargo, carece de las propiedades para implementar la paginación personalizada.

Otro detalle con la paginación y la ordenación surgen con SqlDataSource. De forma predeterminada, los datos devueltos de SqlDataSource se pueden paginar o ordenar a través de GridView. Para demostrarlo, active las opciones habilitar paginación y habilitar ordenación en la etiqueta inteligente de GridView s en `Querying.aspx` y compruebe que funciona según lo previsto.

La ordenación y la paginación funcionan porque SqlDataSource recupera los datos de la base de datos en un conjunto de datos con tipo flexible. El número total de registros devueltos por la consulta es un aspecto esencial de la implementación de la paginación que se puede establecer en el conjunto de elementos. Además, los resultados de los conjuntos de resultados se pueden ordenar a través de una DataView. SqlDataSource utiliza estas funciones automáticamente cuando GridView solicita datos paginados o ordenados.

SqlDataSource puede configurarse para devolver un DataReader en lugar de un conjunto de resultados cambiando su [propiedad`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx) de `DataSet` (valor predeterminado) a `DataReader`. Es posible que se prefiera usar un DataReader en situaciones en las que se pasen los resultados de SqlDataSource a un código existente que espera un DataReader. Además, dado que los objetos DataReader son bastante más sencillos que los conjuntos de valores, ofrecen un mejor rendimiento. Sin embargo, si realiza este cambio, el control Web de datos no puede ordenar ni paginar, ya que SqlDataSource no puede determinar el número de registros que devuelve la consulta, ni tampoco ofrece ninguna técnica para ordenar los datos devueltos.

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>Paso 4: usar una instrucción SQL personalizada o un procedimiento almacenado

Al configurar el control SqlDataSource, la consulta utilizada para devolver los datos se puede especificar en uno de los dos enfoques como una instrucción SQL personalizada o un procedimiento almacenado, o como columnas de una tabla o vista existente. En el paso 2, hemos examinado la selección de columnas de la tabla `Products`. Echemos un vistazo al uso de una instrucción SQL personalizada.

Agregue otro control GridView a la página `Querying.aspx` y elija crear un nuevo origen de datos en la lista desplegable de la etiqueta inteligente. A continuación, indique que los datos se extraerán de una base de datos. se creará un nuevo control SqlDataSource. Asigne al control el nombre `ProductsWithCategoryInfoDataSource`.

![Cree un nuevo control SqlDataSource denominado ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-cs/_static/image18.gif)

**Figura 12**: cree un nuevo control SqlDataSource denominado `ProductsWithCategoryInfoDataSource`

En la pantalla siguiente se le pide que especifique la base de datos. Como hemos vuelto en la figura 7, seleccione el `NORTHWINDConnectionString` en la lista desplegable y haga clic en siguiente. En la pantalla configurar la instrucción SELECT, elija el botón de radio especificar una instrucción SQL personalizada o un procedimiento almacenado y haga clic en siguiente. Se abrirá la pantalla definir instrucciones personalizadas o procedimientos almacenados, que ofrece pestañas SELECT, UPDATE, INSERT y DELETE. En cada pestaña puede especificar una instrucción SQL personalizada en el cuadro de texto o elegir un procedimiento almacenado en la lista desplegable. En este tutorial, veremos cómo escribir una instrucción SQL personalizada. en el siguiente tutorial se incluye un ejemplo en el que se usa un procedimiento almacenado.

![Escriba una instrucción SQL personalizada o seleccione un procedimiento almacenado](querying-data-with-the-sqldatasource-control-cs/_static/image19.gif)

**Figura 13**: especificación de una instrucción SQL personalizada o selección de un procedimiento almacenado

La instrucción SQL personalizada se puede escribir manualmente en el cuadro de texto o se puede construir gráficamente haciendo clic en el botón Generador de consultas. En el Generador de consultas o en el cuadro de texto, use la siguiente consulta para devolver los campos `ProductID` y `ProductName` de la tabla de `Products` mediante un `JOIN` para recuperar los `CategoryName` de los productos de la tabla `Categories`:

[!code-sql[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample4.sql)]

![Puede crear gráficamente la consulta mediante el Generador de consultas](querying-data-with-the-sqldatasource-control-cs/_static/image20.gif)

**Figura 14**: puede construir gráficamente la consulta mediante el generador de consultas

Después de especificar la consulta, haga clic en siguiente para pasar a la pantalla de consulta de prueba. Haga clic en finalizar para completar el Asistente de SqlDataSource.

Después de completar el asistente, el control GridView tendrá tres BoundFields agregados que muestran las columnas `ProductID`, `ProductName`y `CategoryName` devueltas desde la consulta y que dan como resultado el siguiente marcado declarativo:

[!code-aspx[Main](querying-data-with-the-sqldatasource-control-cs/samples/sample5.aspx)]

[![GridView muestra cada ID. de producto, nombre y nombre de categoría asociado](querying-data-with-the-sqldatasource-control-cs/_static/image22.gif)](querying-data-with-the-sqldatasource-control-cs/_static/image21.gif)

**Figura 15**: el control GridView muestra cada ID. de producto, nombre y nombre de categoría asociado ([haga clic para ver la imagen de tamaño completo](querying-data-with-the-sqldatasource-control-cs/_static/image23.gif))

## <a name="summary"></a>Resumen

En este tutorial, vimos cómo consultar y Mostrar datos con el control SqlDataSource. Al igual que ObjectDataSource, SqlDataSource actúa como proxy, lo que proporciona un enfoque declarativo para obtener acceso a los datos. Sus propiedades especifican la base de datos a la que se va a conectar y la consulta SQL `SELECT` que se va a ejecutar; se pueden especificar a través del ventana Propiedades o mediante el Asistente para configurar DataSource.

Los ejemplos de consulta de `SELECT` que examinamos en este tutorial devolvieron todos los registros de la consulta especificada. Sin embargo, el control SqlDataSource puede incluir una cláusula `WHERE` con parámetros cuyos valores se asignan mediante programación o se extraen automáticamente de un origen especificado. Veremos cómo crear y usar consultas parametrizadas en el siguiente tutorial.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Obtener acceso a datos de bases de datos relacionales](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [Información general sobre el control SqlDataSource](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [Tutoriales de inicio rápido de ASP.NET: control SqlDataSource](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Elemento `<connectionStrings>` de Web. config](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [Referencia de cadena de conexión de base de datos](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Susan Connery, Bernadette Leigh y David Suru. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](using-parameterized-queries-with-the-sqldatasource-cs.md)
