---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: Usar consultas parametrizadas con SqlDataSource (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, continuaremos viendo el control SqlDataSource y veremos cómo definir consultas con parámetros. Los parámetros se pueden especificar tanto declaración...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 241dc8c089d4faa9eb95a63684e8a56756bb302c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611295"
---
# <a name="using-parameterized-queries-with-the-sqldatasource-c"></a>Usar consultas parametrizadas con SqlDataSource (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe) de la aplicación de ejemplo o [descarga de PDF](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> En este tutorial, continuaremos viendo el control SqlDataSource y veremos cómo definir consultas con parámetros. Los parámetros se pueden especificar de forma declarativa y mediante programación, y se pueden extraer de varias ubicaciones, como la cadena QueryString, el estado de la sesión, otros controles, etc.

## <a name="introduction"></a>Introducción

En el tutorial anterior, vimos cómo usar el control SqlDataSource para recuperar datos directamente desde una base de datos. Mediante el Asistente para configurar orígenes de datos, podríamos elegir la base de datos y, a continuación, seleccionar las columnas que se van a devolver en una tabla o vista. Escriba una instrucción SQL personalizada; o bien, use un procedimiento almacenado. Tanto si se seleccionan columnas de una tabla o vista como si se escribe una instrucción SQL personalizada, la propiedad SqlDataSource control s `SelectCommand` se asigna a la instrucción SQL ad hoc de `SELECT` resultante y es esta `SELECT` instrucción que se ejecuta cuando se invoca el método SqlDataSource s `Select()` (ya sea mediante programación o automáticamente desde un control Web de datos).

Las instrucciones de SQL `SELECT` que se usan en el tutorial anterior de demostraciones no tenían `WHERE` cláusulas. En una instrucción `SELECT`, se puede utilizar la cláusula `WHERE` para limitar los resultados devueltos. Por ejemplo, para mostrar los nombres de los productos que cuestan más de $50,00, podríamos usar la siguiente consulta:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

Normalmente, los valores utilizados en una cláusula `WHERE` se determinan por algún origen externo, como un valor QueryString, una variable de sesión o la entrada del usuario desde un control Web en la página. Idealmente, estas entradas se especifican mediante el uso de *parámetros*. Con Microsoft SQL Server, los parámetros se denotan con `@parameterName`, como en:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

SqlDataSource admite las consultas con parámetros, tanto para las instrucciones `SELECT` como para las instrucciones `INSERT`, `UPDATE`y `DELETE`. Además, los valores de los parámetros se pueden extraer automáticamente de una variedad de orígenes de la cadena QueryString, el estado de la sesión, los controles de la página, etc. o bien, se pueden asignar mediante programación. En este tutorial, veremos cómo definir consultas con parámetros y cómo especificar los valores de los parámetros de forma declarativa y mediante programación.

> [!NOTE]
> En el tutorial anterior, comparamos el origen ObjectDataSource, que ha sido nuestra herramienta de elección en los primeros 46 tutoriales con SqlDataSource, anotando sus similitudes conceptuales. Estas similitudes también se extienden a los parámetros. Los parámetros de ObjectDataSource asignados a los parámetros de entrada de los métodos en el nivel de lógica de negocios. Con SqlDataSource, los parámetros se definen directamente dentro de la consulta SQL. Ambos controles tienen colecciones de parámetros para sus métodos `Select()`, `Insert()`, `Update()`y `Delete()`, y ambos pueden tener estos valores de parámetro rellenados a partir de orígenes predefinidos (valores QueryString, variables de sesión, etc.) o asignados mediante programación.

## <a name="creating-a-parameterized-query"></a>Crear una consulta parametrizada

El Asistente para configurar el origen de datos de control SqlDataSource ofrece tres vías para definir el comando que se va a ejecutar para recuperar los registros de base de datos:

- Al seleccionar las columnas de una tabla o vista existente,
- Mediante la especificación de una instrucción SQL personalizada, o bien
- Mediante la elección de un procedimiento almacenado

Al seleccionar columnas de una tabla o vista existente, los parámetros de la cláusula `WHERE` se deben especificar mediante el cuadro de diálogo Agregar cláusula de `WHERE`. Sin embargo, al crear una instrucción SQL personalizada, puede especificar los parámetros directamente en la cláusula `WHERE` (mediante `@parameterName` para indicar cada parámetro). Un [procedimiento almacenado](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1) se compone de una o varias instrucciones SQL, y estas instrucciones se pueden parametrizar. Los parámetros utilizados en las instrucciones SQL, sin embargo, se deben pasar como parámetros de entrada al procedimiento almacenado.

Dado que la creación de una consulta parametrizada depende de cómo se especifique el `SelectCommand` SqlDataSource, eche un vistazo a los tres enfoques. Para empezar, abra la página `ParameterizedQueries.aspx` de la carpeta `SqlDataSource`, arrastre un control SqlDataSource desde el cuadro de herramientas hasta el diseñador y establezca su `ID` en `Products25BucksAndUnderDataSource`. A continuación, haga clic en el vínculo configurar origen de datos de la etiqueta inteligente control s. Seleccione la base de datos que se va a usar (`NORTHWINDConnectionString`) y haga clic en siguiente.

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>Paso 1: agregar una cláusula`WHERE`al seleccionar las columnas de una tabla o vista

Al seleccionar los datos que se van a devolver desde la base de datos con el control SqlDataSource, el Asistente para configurar orígenes de datos nos permite elegir simplemente las columnas que se van a devolver de una tabla o vista existente (vea la ilustración 1). Al hacerlo, se crea automáticamente una instrucción de `SELECT` de SQL, que es lo que se envía a la base de datos cuando se invoca el método SqlDataSource s `Select()`. Como hicimos en el tutorial anterior, seleccione la tabla Products en la lista desplegable y active las columnas `ProductID`, `ProductName`y `UnitPrice`.

[![seleccionar las columnas que se van a devolver desde una tabla o vista](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**Figura 1**: selección de las columnas que se van a devolver desde una tabla o vista ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))

Para incluir una cláusula `WHERE` en la instrucción `SELECT`, haga clic en el botón `WHERE`, que abre el cuadro de diálogo Agregar cláusula de `WHERE` (vea la figura 2). Para agregar un parámetro para limitar los resultados devueltos por la consulta de `SELECT`, elija primero la columna por la que desea filtrar los datos. A continuación, elija el operador que se va a usar para filtrar (=, &lt;, &lt;=, &gt;, etc.). Por último, elija el origen del valor del parámetro s, como, por ejemplo, la cadena de tipo QueryString o el estado de sesión. Después de configurar el parámetro, haga clic en el botón Agregar para incluirlo en la consulta `SELECT`.

En este ejemplo, solo se devuelven los resultados en los que el valor de `UnitPrice` es menor o igual que $25,00. Por lo tanto, seleccione `UnitPrice` de la lista desplegable columna y &lt;= en la lista desplegable operador. Al usar un valor de parámetro codificado de forma rígida (como $25,00) o si el valor del parámetro se va a especificar mediante programación, seleccione ninguno en la lista desplegable origen. A continuación, escriba el valor del parámetro codificado de forma rígida en el cuadro de texto valor 25,00 y complete el proceso haciendo clic en el botón Agregar.

[![limitar los resultados devueltos desde el cuadro de diálogo Agregar cláusula WHERE](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**Figura 2**: limitar los resultados devueltos desde el cuadro de diálogo Agregar cláusula de `WHERE` ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))

Después de agregar el parámetro, haga clic en Aceptar para volver al Asistente para configurar orígenes de datos. La instrucción `SELECT` en la parte inferior del asistente debe incluir ahora una cláusula `WHERE` con un parámetro denominado `@UnitPrice`:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> Si especifica varias condiciones en la cláusula `WHERE` en el cuadro de diálogo Agregar cláusula de `WHERE`, el asistente las combina con el operador de `AND`. Si necesita incluir un `OR` en la cláusula `WHERE` (como `WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`), tendrá que generar la instrucción `SELECT` a través de la pantalla instrucción SQL personalizada.

Complete la configuración de SqlDataSource (haga clic en siguiente y luego en finalizar) y, a continuación, inspeccione el marcado declarativo de SqlDataSource. El marcado incluye ahora una colección `<SelectParameters>`, que escribe los orígenes de los parámetros en el `SelectCommand`.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

Cuando se invoca el método SqlDataSource s `Select()`, el valor del parámetro `UnitPrice` (25,00) se aplica al parámetro `@UnitPrice` en el `SelectCommand` antes de que se envíe a la base de datos. El resultado neto es que solo se devuelven de la tabla `Products` los productos que son menores o iguales que $25,00. Para confirmarlo, agregue un control GridView a la página, enlácelo a este origen de datos y, a continuación, vea la página a través de un explorador. Solo debería ver los productos enumerados que son menores o iguales que $25,00, como confirma la figura 3.

[![solo se muestran los productos con un valor inferior o igual a $25,00](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**Figura 3**: solo se muestran los productos que son menores o iguales que $25,00 ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))

## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>Paso 2: agregar parámetros a una instrucción SQL personalizada

Al agregar una instrucción SQL personalizada, puede escribir la cláusula `WHERE` explícitamente o especificar un valor en la celda de filtro del Generador de consultas. Para demostrarlo, deje que se muestren solo los productos en un control GridView cuyos precios sean menores que un umbral determinado. Comience agregando un cuadro de texto a la página `ParameterizedQueries.aspx` para recopilar este valor de umbral del usuario. Establezca la propiedad TextBox s `ID` en `MaxPrice`. Agregue un control Web Button y establezca su propiedad `Text` para mostrar productos coincidentes.

A continuación, arrastre un control GridView a la página y desde su etiqueta inteligente elija crear un nuevo SqlDataSource denominado `ProductsFilteredByPriceDataSource`. En el Asistente para configurar orígenes de datos, vaya a la pantalla especificar una instrucción SQL personalizada o un procedimiento almacenado (consulte la figura 4) y escriba la siguiente consulta:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

Después de escribir la consulta (ya sea de forma manual o a través de la Generador de consultas), haga clic en siguiente.

[![devolver solo los productos que son menores o iguales que un valor de parámetro](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**Figura 4**: devolver solo los productos inferiores o iguales a un valor de parámetro ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))

Dado que la consulta incluye parámetros, la siguiente pantalla del asistente nos solicitará el origen de los valores de los parámetros. Elija control en la lista desplegable origen de parámetros y `MaxPrice` (el control de cuadro de texto s `ID` valor) en la lista desplegable ControlID. También puede especificar un valor predeterminado opcional para usarlo en caso de que el usuario no haya escrito ningún texto en el cuadro de texto `MaxPrice`. En el momento, no escriba un valor predeterminado.

[![la propiedad MaxPrice TextBox s text se usa como el origen del parámetro](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**Figura 5**: la propiedad `MaxPrice` TextBox s `Text` se usa como el origen del parámetro ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))

Para completar el Asistente para configurar orígenes de datos, haga clic en siguiente y luego en finalizar. El marcado declarativo para GridView, TextBox, Button y SqlDataSource es el siguiente:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

Tenga en cuenta que el parámetro dentro de la sección SqlDataSource `<SelectParameters>` es un `ControlParameter`, que incluye propiedades adicionales como `ControlID` y `PropertyName`. Cuando se invoca el método SqlDataSource s `Select()`, el `ControlParameter` obtiene el valor de la propiedad de control Web especificada y lo asigna al parámetro correspondiente de la `SelectCommand`. En este ejemplo, se usa la propiedad `MaxPrice` s text como valor del parámetro `@MaxPrice`.

Tómese un minuto para ver esta página a través de un explorador. Al visitar la página por primera vez o cuando el cuadro de texto `MaxPrice` no tiene ningún valor, no se muestra ningún registro en GridView.

[![no se muestran registros cuando el cuadro de texto MaxPrice está vacío](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**Figura 6**: no se muestra ningún registro cuando el cuadro de texto `MaxPrice` está vacío ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))

La razón por la que no se muestran los productos es que, de forma predeterminada, una cadena vacía para un valor de parámetro se convierte en una base de datos `NULL` valor. Dado que la comparación de `[UnitPrice] <= NULL` siempre se evalúa como false, no se devuelve ningún resultado.

Escriba un valor en el cuadro de texto, como 5,00, y haga clic en el botón Mostrar productos coincidentes. En el postback, SqlDataSource informa a GridView de que uno de sus orígenes de parámetro ha cambiado. Por consiguiente, GridView se vuelve a enlazar a SqlDataSource y muestra los productos que son menores o iguales que $5,00.

[se muestran ![productos menores o iguales a $5,00](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**Figura 7**: se muestran los productos que son menores o iguales que $5,00 ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))

## <a name="initially-displaying-all-products"></a>Mostrando inicialmente todos los productos

En lugar de no mostrar ningún producto cuando la página se carga por primera vez, es posible que desee mostrar *todos* los productos. Una manera de enumerar todos los productos siempre que el cuadro de texto `MaxPrice` esté vacío es establecer el valor predeterminado del parámetro en un valor alto de increíblemente, como 1 millón, ya que es poco probable que Northwind Traders tenga un inventario cuyo precio unitario sea superior a $1 millón. Sin embargo, este enfoque es shortsighted y podría no funcionar en otras situaciones.

En los tutoriales anteriores: [los parámetros declarativos](../basic-reporting/declarative-parameters-cs.md) y el [filtrado de maestro y detalles con DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) , se enfrentó a un problema similar. Nuestra solución era colocar esta lógica en el nivel de lógica de negocios. En concreto, el BLL examinó el valor de entrada y, si se `NULL` o algún valor reservado, la llamada se enrutó al método DAL que devolvió todos los registros. Si el valor de entrada era un valor de filtrado normal, se realizó una llamada al método DAL que ejecutó una instrucción SQL que usaba una cláusula de `WHERE` con parámetros con el valor proporcionado.

Desafortunadamente, omitimos la arquitectura al usar SqlDataSource. En su lugar, es necesario personalizar la instrucción SQL para capturar todos los registros de forma inteligente si el parámetro `@MaximumPrice` es `NULL` o algún valor reservado. Para este ejercicio, deje que lo haga para que, si el parámetro `@MaximumPrice` es igual a `-1.0`, se devuelvan *todos* los registros (`-1.0` funciona como un valor reservado, ya que ningún producto puede tener un valor `UnitPrice` negativo). Para ello, podemos usar la siguiente instrucción SQL:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

Esta cláusula `WHERE` devuelve *todos* los registros si el parámetro `@MaximumPrice` es igual a `-1.0`. Si el valor del parámetro no es `-1.0`, solo se devuelven los productos cuyo `UnitPrice` sea menor o igual que el valor del parámetro `@MaximumPrice`. Al establecer el valor predeterminado del parámetro `@MaximumPrice` en `-1.0`, en la primera carga de página (o siempre que el cuadro de texto `MaxPrice` esté vacío), `@MaximumPrice` tendrá un valor de `-1.0` y se mostrarán todos los productos.

[![ahora se muestran todos los productos cuando el cuadro de texto MaxPrice está vacío.](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**Figura 8**: ahora todos los productos se muestran cuando el cuadro de texto `MaxPrice` está vacío ([haga clic para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))

Hay un par de advertencias que se deben tener en cuenta con este enfoque. En primer lugar, observe que el tipo de datos del parámetro s se deduce por el uso de TI en la consulta SQL. Si cambia la cláusula `WHERE` de `@MaximumPrice = -1.0` a `@MaximumPrice = -1`, el tiempo de ejecución trata el parámetro como un entero. Si después intenta asignar el cuadro de texto `MaxPrice` a un valor decimal (como 5,00), se producirá un error porque no puede convertir 5,00 en un entero. Para solucionarlo, asegúrese de usar `@MaximumPrice = -1.0` en la cláusula `WHERE` o, mejor aún, establezca la propiedad `ControlParameter` Object s `Type` en decimal.

En segundo lugar, al agregar el `OR @MaximumPrice = -1.0` a la cláusula `WHERE`, el motor de consultas no puede usar un índice en `UnitPrice` (suponiendo que existe uno), lo que da como resultado un recorrido de la tabla. Esto puede afectar al rendimiento si hay un número suficientemente grande de registros en la tabla `Products`. Un mejor enfoque sería trasladar esta lógica a un procedimiento almacenado en el que una instrucción `IF` realizaría una consulta `SELECT` de la tabla `Products` sin una cláusula `WHERE` cuando todos los registros tuvieran que devolverse o uno cuya cláusula `WHERE` contenga solo los criterios de `UnitPrice`, de modo que se pueda usar un índice.

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>Paso 3: crear y usar procedimientos almacenados parametrizados

Los procedimientos almacenados pueden incluir un conjunto de parámetros de entrada que se pueden usar en las instrucciones SQL definidas en el procedimiento almacenado. Al configurar SqlDataSource para utilizar un procedimiento almacenado que acepta parámetros de entrada, estos valores de parámetro se pueden especificar utilizando las mismas técnicas que con las instrucciones SQL ad hoc.

Para ilustrar el uso de procedimientos almacenados en SqlDataSource, cree un nuevo procedimiento almacenado en la base de datos Northwind denominada `GetProductsByCategory`, que acepta un parámetro denominado `@CategoryID` y devuelve todas las columnas de los productos cuya columna `CategoryID` coincide con `@CategoryID`. Para crear un procedimiento almacenado, vaya al Explorador de servidores y explore en profundidad la base de datos de `NORTHWND.MDF`. (Si no ve el Explorador de servidores, póngalo en el menú Ver y seleccione la opción Explorador de servidores).

En la base de datos de `NORTHWND.MDF`, haga clic con el botón secundario en la carpeta procedimientos almacenados, elija Agregar nuevo procedimiento almacenado y escriba la siguiente sintaxis:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

Haga clic en el icono guardar (o Ctrl + S) para guardar el procedimiento almacenado. Para probar el procedimiento almacenado, haga clic en él con el botón secundario en la carpeta procedimientos almacenados y elija Ejecutar. Esto le pedirá los parámetros del procedimiento almacenado (`@CategoryID`, en esta instancia), después de los cuales se mostrarán los resultados en la ventana de salida.

[![el procedimiento almacenado GetProductsByCategory cuando se ejecuta con un @CategoryID de 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**Figura 9**: el `GetProductsByCategory` procedimiento almacenado cuando se ejecuta con un `@CategoryID` de 1 ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))

Permita que use este procedimiento almacenado para mostrar todos los productos de la categoría Beverages en un control GridView. Agregue un nuevo GridView a la página y enlácelo a un nuevo SqlDataSource denominado `BeverageProductsDataSource`. Continúe con la pantalla especificar una instrucción SQL personalizada o un procedimiento almacenado, seleccione el botón de radio procedimiento almacenado y elija el `GetProductsByCategory` procedimiento almacenado en la lista desplegable.

[![seleccionar el procedimiento almacenado GetProductsByCategory en la lista desplegable](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**Figura 10**: seleccione el `GetProductsByCategory` procedimiento almacenado en la lista desplegable ([haga clic para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))

Dado que el procedimiento almacenado acepta un parámetro de entrada (`@CategoryID`), al hacer clic en siguiente se le pide que especifique el origen para este valor de parámetro. El `CategoryID` bebidas es 1, por lo que debe dejar la lista desplegable origen del parámetro en ninguno y escribir 1 en el cuadro de texto DefaultValue.

[![usar un valor codificado de forma rígida de 1 para devolver los productos de la categoría bebidas](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**Figura 11**: Use un valor codificado de forma rígida de 1 para devolver los productos de la categoría Beverages ([haga clic para ver la imagen a tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))

Como se muestra en el siguiente marcado declarativo, cuando se utiliza un procedimiento almacenado, la propiedad SqlDataSource s `SelectCommand` se establece en el nombre del procedimiento almacenado y la [propiedad`SelectCommandType`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx) se establece en `StoredProcedure`, lo que indica que el `SelectCommand` es el nombre de un procedimiento almacenado en lugar de una instrucción SQL ad hoc.

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

Pruebe la página en un explorador. Solo se muestran los productos que pertenecen a la categoría Beverages, aunque *todos* los campos Product se muestran, ya que el `GetProductsByCategory` procedimiento almacenado devuelve todas las columnas de la tabla `Products`. Por supuesto, podríamos limitar o personalizar los campos que se muestran en GridView en el cuadro de diálogo Editar columnas de GridView.

[![se muestran todas las bebidas](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**Figura 12**: se muestran todas las bebidas ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))

## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>Paso 4: invocar una instrucción SqlDataSource s`Select()`mediante programación

Los ejemplos que se muestran en el tutorial anterior y este tutorial hasta ahora tienen controles SqlDataSource enlazados directamente a GridView. Sin embargo, se puede obtener acceso mediante programación a los datos del control SqlDataSource y enumerarlos en el código. Esto puede ser especialmente útil cuando se necesita consultar datos para inspeccionarlos, pero no es necesario mostrarlos. En lugar de tener que escribir todo el código reutilizable de ADO.NET para conectarse a la base de datos, especificar el comando y recuperar los resultados, puede permitir que SqlDataSource controle este código de más monótono.

Para ilustrar el trabajo con los datos de SqlDataSource mediante programación, Imagine que su jefe le ha acercado con una solicitud para crear una página web que muestra el nombre de una categoría seleccionada aleatoriamente y sus productos asociados. Es decir, cuando un usuario visita esta página, queremos elegir aleatoriamente una categoría en la tabla `Categories`, mostrar el nombre de la categoría y, a continuación, enumerar los productos que pertenecen a esa categoría.

Para lograr esto, se necesitan dos controles SqlDataSource para obtener una categoría aleatoria de la tabla `Categories` y otra para obtener los productos de la categoría. Compilaremos el SqlDataSource que recupera un registro de categoría aleatorio en este paso. En el paso 5 se examina cómo diseñar el SqlDataSource que recupera los productos de la categoría.

Comience agregando SqlDataSource a `ParameterizedQueries.aspx` y establezca su `ID` en `RandomCategoryDataSource`. Configúrela para que use la siguiente consulta SQL:

[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()` devuelve los registros ordenados en orden aleatorio (consulte [uso de `NEWID()` para ordenar registros aleatoriamente](http://www.sqlteam.com/item.asp?ItemID=8747)). `SELECT TOP 1` devuelve el primer registro del conjunto de resultados. En conjunto, esta consulta devuelve los valores de las columnas `CategoryID` y `CategoryName` de una única categoría seleccionada aleatoriamente.

Para mostrar el valor de la categoría `CategoryName`, agregue un control Web de etiqueta a la página, establezca su propiedad `ID` en `CategoryNameLabel`y borre la propiedad `Text`. Para recuperar los datos de un control SqlDataSource mediante programación, es necesario invocar su método `Select()`. El [método`Select()`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx) espera un único parámetro de entrada de tipo [`DataSourceSelectArguments`](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx), que especifica cómo se deben enviar los datos para que se devuelvan. Esto puede incluir instrucciones sobre la ordenación y el filtrado de los datos, y la usan los controles Web de datos al ordenar o paginar a través de los datos de un control SqlDataSource. Sin embargo, en nuestro ejemplo, no es necesario que los datos se modifiquen antes de que se devuelvan y, por tanto, se pasa el objeto `DataSourceSelectArguments.Empty`.

El método `Select()` devuelve un objeto que implementa `IEnumerable`. El tipo preciso devuelto depende del valor de la propiedad SqlDataSource control s [`DataSourceMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx). Como se explicó en el tutorial anterior, esta propiedad se puede establecer en un valor de `DataSet` o `DataReader`. Si se establece en `DataSet`, el método `Select()` devuelve un objeto [DataView](https://msdn.microsoft.com/library/01s96x0z.aspx) ; Si se establece en `DataReader`, devuelve un objeto que implementa [`IDataReader`](https://msdn.microsoft.com/library/system.data.idatareader.aspx). Dado que el `RandomCategoryDataSource` SqlDataSource tiene su propiedad `DataSourceMode` establecida en `DataSet` (el valor predeterminado), se va a trabajar con un objeto DataView.

En el código siguiente se muestra cómo recuperar los registros del `RandomCategoryDataSource` SqlDataSource como DataView y cómo leer el valor de la columna `CategoryName` de la primera fila DataView:

[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]` devuelve el primer `DataRowView` de DataView. `randomCategoryView[0]["CategoryName"]` devuelve el valor de la columna `CategoryName` de la primera fila. Tenga en cuenta que la DataView tiene un tipo flexible. Para hacer referencia a un valor de columna determinado, necesitamos pasar el nombre de la columna como una cadena (NombreCategoría, en este caso). En la figura 13 se muestra el mensaje mostrado en el `CategoryNameLabel` al ver la página. Por supuesto, el nombre de categoría real que se muestra se selecciona aleatoriamente por el `RandomCategoryDataSource` SqlDataSource en cada visita a la página (incluidos los postbacks).

[![se muestra el nombre de la categoría seleccionada aleatoriamente](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**Figura 13**: se muestra el nombre de la categoría seleccionada aleatoriamente ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))

> [!NOTE]
> Si la propiedad SqlDataSource control s `DataSourceMode` se hubiera establecido en `DataReader`, el valor devuelto del método `Select()` habría tenido que convertirse en `IDataReader`. Para leer el valor de `CategoryName` columna de la primera fila, se usa código como el siguiente:

[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

Con el uso de SqlDataSource al seleccionar una categoría aleatoriamente, se vuelve a preparar para agregar el control GridView que enumera los productos de la categoría.

> [!NOTE]
> En lugar de usar un control Web de etiqueta para mostrar el nombre de la categoría, podríamos haber agregado FormView o DetailsView a la página, enlazándolo a SqlDataSource. Sin embargo, el uso de la etiqueta nos permitió explorar cómo invocar la instrucción SqlDataSource s `Select()` mediante programación y trabajar con los datos resultantes en el código.

## <a name="step-5-assigning-parameter-values-programmatically"></a>Paso 5: asignar valores de parámetro mediante programación

Todos los ejemplos que vimos hasta ahora en este tutorial han utilizado un valor de parámetro codificado de forma rígida o uno tomado de uno de los orígenes de parámetros predefinidos (un valor QueryString, un control Web en la página, etc.). Sin embargo, los parámetros de control SqlDataSource también se pueden establecer mediante programación. Para completar nuestro ejemplo actual, necesitamos un SqlDataSource que devuelva todos los productos que pertenecen a una categoría especificada. Este SqlDataSource tendrá un parámetro `CategoryID` cuyo valor debe establecerse en función del valor de `CategoryID` columna devuelto por el `RandomCategoryDataSource` SqlDataSource en el controlador de eventos `Page_Load`.

Empiece agregando un control GridView a la página y enlácelo a un nuevo SqlDataSource denominado `ProductsByCategoryDataSource`. Como hicimos en el paso 3, configure SqlDataSource para que invoque el `GetProductsByCategory` procedimiento almacenado. Deje la lista desplegable origen de parámetro establecida en ninguno, pero no escriba un valor predeterminado, ya que estableceremos este valor predeterminado mediante programación.

[![no especifique un origen de parámetro o un valor predeterminado](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**Figura 14**: no especifique un origen de parámetro o un valor predeterminado ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))

Después de completar el Asistente de SqlDataSource, el marcado declarativo resultante debe ser similar al siguiente:

[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

Podemos asignar el `DefaultValue` del parámetro `CategoryID` mediante programación en el controlador de eventos `Page_Load`:

[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

Con esta adición, la página incluye un control GridView que muestra los productos asociados a la categoría seleccionada aleatoriamente.

[![no especifique un origen de parámetro o un valor predeterminado](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**Figura 15**: no especifique un origen de parámetro o un valor predeterminado ([haga clic para ver la imagen de tamaño completo](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))

## <a name="summary"></a>Resumen

SqlDataSource permite a los desarrolladores de páginas definir consultas con parámetros cuyos valores de parámetro se pueden codificar de forma rígida, extraer de orígenes de parámetros predefinidos o asignar mediante programación. En este tutorial, hemos visto cómo crear una consulta con parámetros desde el Asistente para configurar orígenes de datos para consultas SQL ad hoc y procedimientos almacenados. También analizamos el uso de orígenes de parámetros codificados de forma rígida, un control Web como origen de parámetro y la especificación mediante programación del valor del parámetro.

Como con ObjectDataSource, SqlDataSource también proporciona funciones para modificar sus datos subyacentes. En el siguiente tutorial veremos cómo definir `INSERT`, `UPDATE`y `DELETE` instrucciones con SqlDataSource. Una vez agregadas estas instrucciones, podemos usar las características integradas de inserción, edición y eliminación inherentes a los controles GridView, DetailsView y FormView.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Scott Clyde, Randell Schmidt y Ken Pespisa. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](querying-data-with-the-sqldatasource-control-cs.md)
> [Siguiente](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
