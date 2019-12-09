---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
title: Paginación eficaz a través de grandes cantidadesC#de datos () | Microsoft Docs
author: rick-anderson
description: La opción de paginación predeterminada de un control de presentación de datos no es adecuada cuando se trabaja con grandes cantidades de datos, como el control de origen de datos subyacente recupera...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 59c01998-9326-4ecb-9392-cb9615962140
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-cs
msc.type: authoredcontent
ms.openlocfilehash: a3e9562035cb24987b01fcdff5fbfb5fa8a1f894
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74629724"
---
# <a name="efficiently-paging-through-large-amounts-of-data-c"></a>Pasar página por grandes cantidades de datos de forma eficaz (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_CS.exe) de la aplicación de ejemplo o [descarga de PDF](efficiently-paging-through-large-amounts-of-data-cs/_static/datatutorial25cs1.pdf)

> La opción de paginación predeterminada de un control de presentación de datos no es adecuada cuando se trabaja con grandes cantidades de datos, ya que el control de origen de datos subyacente recupera todos los registros, aunque solo se muestre un subconjunto de datos. En tales circunstancias, debemos pasar a la paginación personalizada.

## <a name="introduction"></a>Introducción

Como se explicó en el tutorial anterior, la paginación se puede implementar de una de estas dos maneras:

- La **paginación predeterminada** se puede implementar simplemente comprobando la opción Habilitar paginación en la etiqueta inteligente control de Web de datos. sin embargo, cada vez que se ve una página de datos, el ObjectDataSource recupera *todos* los registros, aunque solo se muestre un subconjunto de ellos en la página.
- La **paginación personalizada** mejora el rendimiento de la paginación predeterminada al recuperar solo los registros de la base de datos que se deben mostrar para la página de datos determinada que solicita el usuario. sin embargo, la paginación personalizada implica un poco más de trabajo de implementación que la paginación predeterminada

Debido a la facilidad de implementación, active una casilla y vuelva a realizarla. la paginación predeterminada es una opción atractiva. Sin embargo, su enfoque na en la recuperación de todos los registros lo convierte en una opción improbable al paginar a través de cantidades de datos suficientemente grandes o para sitios con muchos usuarios simultáneos. En tales circunstancias, debemos pasar a la paginación personalizada para proporcionar un sistema con capacidad de respuesta.

El reto de la paginación personalizada es la posibilidad de escribir una consulta que devuelve el conjunto preciso de registros necesarios para una página de datos determinada. Afortunadamente, Microsoft SQL Server 2005 proporciona una nueva palabra clave para clasificar los resultados, lo que nos permite escribir una consulta que pueda recuperar eficazmente el subconjunto de registros adecuado. En este tutorial veremos cómo usar esta nueva SQL Server palabra clave 2005 para implementar la paginación personalizada en un control GridView. Aunque la interfaz de usuario para la paginación personalizada es idéntica a la paginación predeterminada, la ejecución paso a paso de una página a la siguiente mediante la paginación personalizada puede ser de varios pedidos de magnitud más rápido que la paginación predeterminada.

> [!NOTE]
> La ganancia de rendimiento exacta que exhibe la paginación personalizada depende del número total de registros que se van a paginar y de la carga que se va a colocar en el servidor de base de datos. Al final de este tutorial veremos algunas métricas generales que muestran las ventajas del rendimiento obtenido a través de la paginación personalizada.

## <a name="step-1-understanding-the-custom-paging-process"></a>Paso 1: Descripción del proceso de paginación personalizada

Al paginar a través de los datos, los registros precisos que se muestran en una página dependen de la página de datos que se solicita y del número de registros que se muestran por página. Por ejemplo, Imagine que queríamos paginar los productos 81, mostrando 10 productos por página. Al ver la primera página, se desean los productos del 1 al 10. al ver la segunda página, estamos interesados en los productos 11 a 20, etc.

Hay tres variables que indican qué registros deben recuperarse y cómo se debe representar la interfaz de paginación:

- **Índice de fila inicial** : índice de la primera fila de la página de datos que se va a mostrar; Este índice se puede calcular multiplicando el índice de la página por los registros que se van a mostrar por página y agregando uno. Por ejemplo, al paginar los registros 10 a la vez, en la primera página (cuyo índice de página es 0), el índice de la fila inicial es 0 \* 10 + 1 o 1; en la segunda página (cuyo índice de página es 1), el índice de la fila inicial es 1 \* 10 + 1 o 11.
- Número **máximo de filas** que se van a mostrar por página. Esta variable se conoce como número máximo de filas, ya que en la última página puede haber menos registros devueltos que el tamaño de página. Por ejemplo, al paginar a través de los 81 productos 10 registros por página, la novena y la página final tendrán un solo registro. Sin embargo, ninguna página mostrará más registros que el valor máximo de filas.
- **Recuento total** de registros número total de registros que se van a paginar. Aunque esta variable no es necesaria para determinar qué registros se van a recuperar en una página determinada, dicta la interfaz de paginación. Por ejemplo, si hay 81 productos en los que se va a paginar, la interfaz de paginación sabe Mostrar nueve números de página en la interfaz de usuario de paginación.

Con la paginación predeterminada, el índice de la fila inicial se calcula como el producto del índice de la página y el tamaño de página más uno, mientras que el número máximo de filas es simplemente el tamaño de la página. Dado que la paginación predeterminada recupera todos los registros de la base de datos al representar cualquier página de datos, se conoce el índice de cada fila, lo que hace pasar a la fila de índice de la fila inicial una tarea trivial. Además, el número total de registros está disponible fácilmente, ya que es simplemente el número de registros de la tabla de datos (o cualquier objeto que se use para contener los resultados de la base de datos).

Dadas las variables de índice de inicio y máximo de filas, una implementación de paginación personalizada solo debe devolver el subconjunto preciso de registros a partir del índice de la fila inicial y hasta el número máximo de filas de registros después. La paginación personalizada proporciona dos desafíos:

- Es necesario poder asociar de forma eficaz un índice de fila con cada fila de los datos completos que se van a paginar a través de para que podamos empezar a devolver registros en el índice de la fila inicial especificada.
- Es necesario proporcionar el número total de registros que se van a paginar.

En los dos pasos siguientes examinaremos el script SQL necesario para responder a estos dos desafíos. Además del script SQL, también tendremos que implementar métodos en DAL y BLL.

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>Paso 2: devolver el número total de registros que se van a paginar

Antes de examinar cómo recuperar el subconjunto preciso de los registros de la página que se está mostrando, eche un vistazo primero a cómo devolver el número total de registros que se van a paginar. Esta información es necesaria para configurar correctamente la interfaz de usuario de paginación. El número total de registros devueltos por una consulta SQL determinada se puede obtener mediante el [`COUNT` función de agregado](https://msdn.microsoft.com/library/ms175997.aspx). Por ejemplo, para determinar el número total de registros en la tabla `Products`, podemos usar la siguiente consulta:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample1.sql)]

Permite agregar un método a la capa DAL que devuelve esta información. En concreto, vamos a crear un método DAL denominado `TotalNumberOfProducts()` que ejecute la instrucción de `SELECT` mostrada anteriormente.

Para empezar, abra el `Northwind.xsd` archivo de conjunto de archivos con tipo en la carpeta `App_Code/DAL`. A continuación, haga clic con el botón derecho en el `ProductsTableAdapter` en el diseñador y elija Agregar consulta. Como hemos aprendido en los tutoriales anteriores, esto nos permitirá agregar un nuevo método a la capa DAL que, cuando se invoca, ejecutará una instrucción SQL o un procedimiento almacenado determinado. Al igual que con nuestros métodos de TableAdapter en los tutoriales anteriores, para esta opción se opta por usar una instrucción SQL ad hoc.

![Usar una instrucción SQL ad hoc](efficiently-paging-through-large-amounts-of-data-cs/_static/image1.png)

**Figura 1**: uso de una instrucción SQL ad hoc

En la pantalla siguiente, se puede especificar el tipo de consulta que se va a crear. Puesto que esta consulta devolverá un único valor escalar, el número total de registros de la tabla `Products` elige el `SELECT` que devuelve una opción de valor único.

![Configurar la consulta para utilizar una instrucción SELECT que devuelve un valor único](efficiently-paging-through-large-amounts-of-data-cs/_static/image2.png)

**Figura 2**: configuración de la consulta para usar una instrucción SELECT que devuelve un único valor

Después de indicar el tipo de consulta que se va a usar, se debe especificar la consulta.

![Usar la consulta SELECT COUNT (*) FROM Products](efficiently-paging-through-large-amounts-of-data-cs/_static/image3.png)

**Figura 3**: uso de la consulta Select COUNT (\*) from Products

Por último, especifique el nombre del método. Como ya se ha mencionado, deje que use `TotalNumberOfProducts`.

![Asigne al método DAL el nombre TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-cs/_static/image4.png)

**Figura 4**: nombrar el método Dal TotalNumberOfProducts

Después de hacer clic en finalizar, el asistente agregará el método `TotalNumberOfProducts` a la capa DAL. Los métodos que devuelven valores escalares en la capa DAL devuelven tipos que aceptan valores NULL, en caso de que el resultado de la consulta SQL sea `NULL`. Sin embargo, nuestra consulta de `COUNT` siempre devolverá un valor que no sea de`NULL`; sin importar, el método DAL devuelve un entero que acepta valores NULL.

Además del método DAL, también necesitamos un método en la capa BLL. Abra el archivo de clase de `ProductsBLL` y agregue un método `TotalNumberOfProducts` que simplemente llama al método de `TotalNumberOfProducts` DAL s:

[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample2.cs)]

El método DAL s `TotalNumberOfProducts` devuelve un entero que acepta valores NULL; sin embargo, se ha creado el método `ProductsBLL` Class s `TotalNumberOfProducts` para que devuelva un entero estándar. Por lo tanto, es necesario que el método `ProductsBLL` clase s `TotalNumberOfProducts` devuelva la parte del valor del entero que acepta valores NULL que devuelve el método `TotalNumberOfProducts` de DAL. La llamada a `GetValueOrDefault()` devuelve el valor del entero que acepta valores NULL, si existe; sin embargo, si el entero que acepta valores NULL es `null`, devuelve el valor entero predeterminado, 0.

## <a name="step-3-returning-the-precise-subset-of-records"></a>Paso 3: devolver el subconjunto preciso de registros

La siguiente tarea consiste en crear métodos en la capa DAL y BLL que acepten las variables de índice de inicio y máximo de filas que se han descrito anteriormente y devuelven los registros correspondientes. Antes de hacerlo, deje que primero examinemos el script SQL necesario. El reto de nosotros es que debemos poder asignar eficazmente un índice a cada fila de los resultados completos que se van a paginar a través de para que podamos devolver solo los registros que comienzan en el índice de la fila inicial (y hasta el número máximo de registros).

Esto no es un desafío si ya existe una columna en la tabla de base de datos que actúa como índice de fila. A primera vista, podríamos pensar que el campo `Products` tabla s `ProductID` sería suficiente, ya que el primer producto tiene `ProductID` de 1, el segundo a 2, etc. Sin embargo, la eliminación de un producto deja un hueco en la secuencia, anulando este enfoque.

Hay dos técnicas generales que se usan para asociar de forma eficaz un índice de fila con los datos que se van a recorrer, lo que permite recuperar el subconjunto preciso de registros:

- Al **utilizar SQL Server palabra clave de `ROW_NUMBER()` 2005 s** nueva en SQL Server 2005, la palabra clave `ROW_NUMBER()` asocia una clasificación con cada registro devuelto según el orden. Esta clasificación se puede usar como índice de fila para cada fila.
- **Usar una variable de tabla y `SET ROWCOUNT`** Se puede utilizar SQL Server [instrucción`SET ROWCOUNT`](https://msdn.microsoft.com/library/ms188774.aspx) para especificar el número total de registros que debe procesar una consulta antes de finalizar. [las variables de tabla](http://www.sqlteam.com/item.asp?ItemID=9454) son variables de T-SQL locales que pueden contener datos tabulares, similar a [las tablas temporales](http://www.sqlteam.com/item.asp?ItemID=2029). Este enfoque funciona igual de bien con Microsoft SQL Server 2005 y SQL Server 2000 (mientras que el enfoque `ROW_NUMBER()` solo funciona con SQL Server 2005).  
  
  La idea aquí es crear una variable de tabla que tenga una `IDENTITY` columna y columnas para las claves principales de la tabla cuyos datos se van a paginar. Después, el contenido de la tabla cuyos datos se están paginando se vuelca en la variable de tabla, con lo que se asocia un índice de fila secuencial (a través de la columna `IDENTITY`) para cada registro de la tabla. Una vez que se ha rellenado la variable de tabla, se puede ejecutar una instrucción `SELECT` en la variable de tabla, unida a la tabla subyacente, para extraer los registros determinados. La instrucción `SET ROWCOUNT` se utiliza para limitar de forma inteligente el número de registros que se deben volcar en la variable de tabla.  
  
  Este enfoque de eficacia se basa en el número de página que se solicita, ya que al valor `SET ROWCOUNT` se le asigna el valor de inicio de índice de fila más las filas máximas. Al paginar a través de páginas de número bajo, como las primeras páginas de datos, este enfoque es muy eficaz. Sin embargo, exhibe un rendimiento similar a la paginación predeterminada al recuperar una página cerca del final.

En este tutorial se implementa la paginación personalizada mediante la palabra clave `ROW_NUMBER()`. Para obtener más información sobre el uso de la variable de tabla y la técnica de `SET ROWCOUNT`, vea [un método más eficaz para la paginación a través de grandes conjuntos de resultados](http://www.4guysfromrolla.com/webtech/042606-1.shtml).

La palabra clave `ROW_NUMBER()` asociada a una clasificación con cada registro devuelto en una ordenación determinada con la siguiente sintaxis:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample3.sql)]

`ROW_NUMBER()` devuelve un valor numérico que especifica el rango de cada registro con respecto a la ordenación indicada. Por ejemplo, para ver el rango de cada producto, ordenado del más caro al menos, podríamos usar la siguiente consulta:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample4.sql)]

En la ilustración 5 se muestran los resultados de esta consulta cuando se ejecutan en la ventana de consulta de Visual Studio. Tenga en cuenta que los productos se ordenan por precio, junto con un rango de precios para cada fila.

![El rango de precios se incluye para cada registro devuelto](efficiently-paging-through-large-amounts-of-data-cs/_static/image5.png)

**Figura 5**: el rango de precios se incluye para cada registro devuelto

> [!NOTE]
> `ROW_NUMBER()` es solo una de las muchas funciones nuevas de categoría disponibles en SQL Server 2005. Para obtener una explicación más detallada de `ROW_NUMBER()`, junto con las demás funciones de categoría, lea [resultados clasificados devueltos con Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml).

Al clasificar los resultados por la columna de `ORDER BY` especificada en la cláusula `OVER` (`UnitPrice`, en el ejemplo anterior), SQL Server debe ordenar los resultados. Se trata de una operación rápida si hay un índice clúster en las columnas en las que se están ordenando los resultados, o si hay un índice de cobertura, pero puede ser más costoso en caso contrario. Para ayudar a mejorar el rendimiento de las consultas suficientemente grandes, considere la posibilidad de agregar un índice no agrupado para la columna por la que se ordenan los resultados. Consulte [funciones de categoría y rendimiento en SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obtener una visión más detallada de las consideraciones de rendimiento.

La información de clasificación devuelta por `ROW_NUMBER()` no se puede usar directamente en la cláusula `WHERE`. Sin embargo, se puede usar una tabla derivada para devolver el `ROW_NUMBER()` resultado, que puede aparecer en la cláusula `WHERE`. Por ejemplo, la consulta siguiente usa una tabla derivada para devolver las columnas ProductName y UnitPrice, junto con el resultado de la `ROW_NUMBER()` y, a continuación, utiliza una cláusula `WHERE` para devolver solo los productos cuyo rango de precio está entre 11 y 20:

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample5.sql)]

Ampliando este concepto un poco más, podemos usar este enfoque para recuperar una página específica de datos según los valores de índice de fila inicial y filas máximas que desee:

[!code-html[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample6.html)]

> [!NOTE]
> Como veremos más adelante en este tutorial, el *`StartRowIndex`* proporcionado por ObjectDataSource se indexa a partir de cero, mientras que el valor de `ROW_NUMBER()` devuelto por SQL Server 2005 se indexa a partir de 1. Por lo tanto, la cláusula `WHERE` devuelve los registros donde `PriceRank` es estrictamente mayor que *`StartRowIndex`* y menor o igual que`StartRowIndex` * + `MaximumRows`.*

Ahora que hemos analizado cómo se puede usar `ROW_NUMBER()` para recuperar una página de datos determinada, dados los valores de índice de inicio y máximo de filas, ahora necesitamos implementar esta lógica como métodos en DAL y BLL.

Al crear esta consulta, es necesario decidir el orden por el que se clasificarán los resultados; permita ordenar los productos por su nombre por orden alfabético. Esto significa que, con la implementación de paginación personalizada en este tutorial, no se podrá crear un informe paginado personalizado que también se pueda ordenar. Sin embargo, en el siguiente tutorial, veremos cómo se puede proporcionar esta funcionalidad.

En la sección anterior, creamos el método DAL como una instrucción SQL ad hoc. Desafortunadamente, el analizador de T-SQL de Visual Studio que usa el Asistente de TableAdapter no es como la sintaxis de `OVER` usada por la función `ROW_NUMBER()`. Por lo tanto, es necesario crear este método DAL como un procedimiento almacenado. Seleccione el Explorador de servidores en el menú Ver (o presione Ctrl + Alt + S) y expanda el nodo `NORTHWND.MDF`. Para agregar un nuevo procedimiento almacenado, haga clic con el botón secundario en el nodo procedimientos almacenados y elija Agregar un nuevo procedimiento almacenado (vea la figura 6).

![Agregar un nuevo procedimiento almacenado para la paginación a través de los productos](efficiently-paging-through-large-amounts-of-data-cs/_static/image6.png)

**Figura 6**: agregar un nuevo procedimiento almacenado para la paginación a través de los productos

Este procedimiento almacenado debe aceptar dos parámetros de entrada enteros: `@startRowIndex` y `@maximumRows` y usar la función `ROW_NUMBER()` ordenada por el campo `ProductName`, devolviendo solo las filas mayores que el `@startRowIndex` especificado y menor o igual que `@startRowIndex` + `@maximumRow` s. Escriba el siguiente script en el nuevo procedimiento almacenado y, a continuación, haga clic en el icono guardar para agregar el procedimiento almacenado a la base de datos.

[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample7.sql)]

Después de crear el procedimiento almacenado, dedique un momento a probarlo. Haga clic con el botón derecho en el `GetProductsPaged` nombre del procedimiento almacenado en el Explorador de servidores y elija la opción ejecutar. A continuación, Visual Studio le pedirá los parámetros de entrada, `@startRowIndex` y `@maximumRow` s (consulte la figura 7). Pruebe valores diferentes y examine los resultados.

![Escriba un valor para los parámetros @startRowIndex y @maximumRows](efficiently-paging-through-large-amounts-of-data-cs/_static/image7.png)

<strong>Figura 7</strong>: escriba un valor para los parámetros @startRowIndex y @maximumRows

Después de elegir estos valores de parámetros de entrada, en la ventana de salida se mostrarán los resultados. En la figura 8 se muestran los resultados al pasar 10 para los parámetros `@startRowIndex` y `@maximumRows`.

[![se devuelven los registros que aparecen en la segunda página de datos](efficiently-paging-through-large-amounts-of-data-cs/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image8.png)

**Figura 8**: se devuelven los registros que aparecen en la segunda página de datos ([haga clic para ver la imagen de tamaño completo](efficiently-paging-through-large-amounts-of-data-cs/_static/image10.png))

Con este procedimiento almacenado creado, se ha preparado para crear el método de `ProductsTableAdapter`. Abra el `Northwind.xsd` conjunto de bits con tipo, haga clic con el botón derecho en el `ProductsTableAdapter`y elija la opción Agregar consulta. En lugar de crear la consulta mediante una instrucción SQL ad hoc, créela mediante un procedimiento almacenado existente.

![Crear el método DAL mediante un procedimiento almacenado existente](efficiently-paging-through-large-amounts-of-data-cs/_static/image11.png)

**Ilustración 9**: crear el método Dal mediante un procedimiento almacenado existente

A continuación, se le pedirá que seleccione el procedimiento almacenado que se va a invocar. Elija el `GetProductsPaged` procedimiento almacenado en la lista desplegable.

![Elija el procedimiento almacenado GetProductsPaged en la lista desplegable.](efficiently-paging-through-large-amounts-of-data-cs/_static/image12.png)

**Figura 10**: elegir el procedimiento almacenado GetProductsPaged en la lista desplegable

A continuación, en la siguiente pantalla se le pregunta qué tipo de datos devuelve el procedimiento almacenado: datos tabulares, un solo valor o ningún valor. Dado que el `GetProductsPaged` procedimiento almacenado puede devolver varios registros, indique que devuelve datos tabulares.

![Indicar que el procedimiento almacenado devuelve datos tabulares](efficiently-paging-through-large-amounts-of-data-cs/_static/image13.png)

**Figura 11**: indicar que el procedimiento almacenado devuelve datos tabulares

Por último, indique los nombres de los métodos que desea crear. Al igual que en los tutoriales anteriores, continúe y cree métodos con el rellenado de un DataTable y devuelva un DataTable. Asigne al primer método el nombre `FillPaged` y el segundo `GetProductsPaged`.

![Asigne a los métodos el nombre FillPaged y GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image14.png)

**Figura 12**: asigne un nombre a los métodos FillPaged y GetProductsPaged

Además de crear un método DAL para devolver una página determinada de productos, también es necesario proporcionar dicha funcionalidad en la capa BLL. Al igual que el método DAL, el método GetProductsPaged de BLL debe aceptar dos entradas de enteros para especificar el índice de la fila inicial y el número máximo de filas, y debe devolver solo los registros que se encuentran dentro del intervalo especificado. Cree este método BLL en la clase ProductsBLL que simplemente llama al método GetProductsPaged de la capa DAL, de la siguiente manera:

[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample8.cs)]

Puede usar cualquier nombre para los parámetros de entrada del método BLL, pero, como veremos en breve, al elegir usar `startRowIndex` y `maximumRows` nos ahorrará de un bit de trabajo adicional al configurar un ObjectDataSource para usar este método.

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>Paso 4: configuración de ObjectDataSource para usar la paginación personalizada

Con los métodos BLL y DAL para tener acceso a un subconjunto de registros determinado, se vuelve a preparar la creación de un control GridView que recorre los registros subyacentes mediante la paginación personalizada. Para empezar, abra la página `EfficientPaging.aspx` de la carpeta `PagingAndSorting`, agregue un control GridView a la página y configúrelo para utilizar un nuevo control ObjectDataSource. En los tutoriales anteriores, a menudo tuvimos configurado ObjectDataSource para usar el método `ProductsBLL` Class s `GetProducts`. Sin embargo, esta vez queremos usar el método `GetProductsPaged` en su lugar, ya que el método `GetProducts` devuelve *todos* los productos de la base de datos, mientras que `GetProductsPaged` devuelve solo un subconjunto determinado de registros.

![Configurar ObjectDataSource para usar el método ProductsBLL de la clase GetProductsPaged](efficiently-paging-through-large-amounts-of-data-cs/_static/image15.png)

**Figura 13**: configuración de ObjectDataSource para usar el método ProductsBLL de la clase GetProductsPaged

Dado que vamos a crear un control GridView de solo lectura, dedique un momento a establecer la lista desplegable de métodos en las pestañas insertar, actualizar y eliminar en (ninguno).

A continuación, el Asistente para ObjectDataSource nos solicita los orígenes de los valores del método `GetProductsPaged` s `startRowIndex` y `maximumRows` parámetros de entrada. Los parámetros de entrada se establecerán realmente mediante GridView automáticamente, por lo que simplemente deje el origen establecido en ninguno y haga clic en finalizar.

![Dejar los orígenes de parámetro de entrada como ninguno](efficiently-paging-through-large-amounts-of-data-cs/_static/image16.png)

**Figura 14**: dejar los orígenes de parámetro de entrada como ninguno

Después de completar el Asistente de ObjectDataSource, GridView contendrá un BoundField o CheckBoxField para cada uno de los campos de datos de producto. No dude en adaptar la apariencia de GridView como considere oportuno. He optado por mostrar solo las `ProductName`, `CategoryName`, `SupplierName`, `QuantityPerUnit`y `UnitPrice` BoundFields. Además, configure GridView para que admita la paginación. para ello, active la casilla habilitar paginación en su etiqueta inteligente. Después de estos cambios, el marcado declarativo de GridView y ObjectDataSource debe ser similar al siguiente:

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample9.aspx)]

Sin embargo, si visita la página a través de un explorador, el control GridView no es donde se encuentra.

![GridView no se muestra](efficiently-paging-through-large-amounts-of-data-cs/_static/image17.png)

**Figura 15**: no se muestra GridView

Falta GridView porque ObjectDataSource está usando actualmente 0 como valores para los parámetros de entrada y `maximumRows` `startRowIndex` de `GetProductsPaged`. Por lo tanto, la consulta SQL resultante no devuelve ningún registro y, por tanto, no se muestra GridView.

Para solucionarlo, es necesario configurar ObjectDataSource para usar la paginación personalizada. Esto puede realizarse en los pasos siguientes:

1. **Establezca la propiedad ObjectDataSource s `EnablePaging` en `true`** esto indica al origen ObjectDataSource que debe pasar al `SelectMethod` dos parámetros adicionales: uno para especificar el índice de la fila inicial ([`StartRowIndexParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx)) y otro para especificar el número máximo de filas ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx)).
2. **Establezca las propiedades de ObjectDataSource s `StartRowIndexParameterName` y `MaximumRowsParameterName` en consecuencia,** las propiedades `StartRowIndexParameterName` y `MaximumRowsParameterName` indican los nombres de los parámetros de entrada pasados en el `SelectMethod` para fines de paginación personalizada. De forma predeterminada, estos nombres de parámetro se `startIndexRow` y `maximumRows`, que es el motivo por el que al crear el método `GetProductsPaged` en el BLL, usé estos valores para los parámetros de entrada. Si decide usar nombres de parámetros diferentes para el método de `GetProductsPaged` de BLL, como `startIndex` y `maxRows`, por ejemplo, debe establecer las propiedades de los `StartRowIndexParameterName` y `MaximumRowsParameterName` de ObjectDataSource según corresponda (como startIndex para `StartRowIndexParameterName` y maxRows para `MaximumRowsParameterName`).
3. **Establezca la propiedad ObjectDataSource s [`SelectCountMethod`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx) en el nombre del método que devuelve el número total de registros que se van a paginar (`TotalNumberOfProducts`)** . recuerde que el método `ProductsBLL` de la clase `TotalNumberOfProducts` devuelve el número total de registros que se van a paginar mediante un método dal que ejecuta una consulta de `SELECT COUNT(*) FROM Products`. Esta información es necesaria para que ObjectDataSource pueda representar correctamente la interfaz de paginación.
4. **Quitar los elementos `startRowIndex` y `maximumRows` `<asp:Parameter>` del marcado declarativo de ObjectDataSource s** al configurar ObjectDataSource mediante el asistente, Visual Studio agrega automáticamente dos elementos `<asp:Parameter>` para los parámetros de entrada del método `GetProductsPaged`. Al establecer `EnablePaging` en `true`, estos parámetros se pasarán automáticamente. Si también aparecen en la sintaxis declarativa, ObjectDataSource intentará pasar *cuatro* parámetros al método `GetProductsPaged` y dos parámetros al método `TotalNumberOfProducts`. Si olvida quitar estos elementos `<asp:Parameter>`, al visitar la página a través de un explorador aparecerá un mensaje de error similar a: *ObjectDataSource ' ObjectDataSource1 ' no encontró un método no genérico ' TotalNumberOfProducts ' que tenga parámetros: startRowIndex, maximumRows*.

Después de realizar estos cambios, la sintaxis declarativa s de ObjectDataSource debe ser similar a la siguiente:

[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample10.aspx)]

Tenga en cuenta que se han establecido las propiedades `EnablePaging` y `SelectCountMethod` y se han quitado los elementos `<asp:Parameter>`. En la figura 16 se muestra una captura de pantalla de la ventana Propiedades una vez realizados estos cambios.

![Para usar la paginación personalizada, configure el control ObjectDataSource](efficiently-paging-through-large-amounts-of-data-cs/_static/image18.png)

**Figura 16**: para usar la paginación personalizada, configure el control ObjectDataSource

Después de realizar estos cambios, visite esta página a través de un explorador. Debería ver 10 productos enumerados por orden alfabético. Dedique un momento a recorrer los datos una página a la vez. Aunque no hay ninguna diferencia visual de la perspectiva del usuario final entre la paginación predeterminada y la paginación personalizada, la paginación personalizada se pagina de forma más eficaz a través de grandes cantidades de datos, ya que solo recupera los registros que deben mostrarse para una página determinada.

[![los datos, ordenados por el nombre del producto, se paginan mediante la paginación personalizada](efficiently-paging-through-large-amounts-of-data-cs/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image19.png)

**Figura 17**: los datos, ordenados por el nombre del producto, se paginan mediante la paginación personalizada ([haga clic para ver la imagen de tamaño completo](efficiently-paging-through-large-amounts-of-data-cs/_static/image21.png))

> [!NOTE]
> Con la paginación personalizada, el valor de recuento de páginas devuelto por la `SelectCountMethod` de ObjectDataSource s se almacena en el estado de vista de GridView. Otras variables de GridView las `PageIndex`, `EditIndex`, `SelectedIndex`, `DataKeys` colección, etc. se almacenan en el *Estado del control*, que se conserva independientemente del valor de la propiedad `EnableViewState` de GridView. Puesto que el valor `PageCount` se conserva en los postbacks mediante el estado de vista, cuando se usa una interfaz de paginación que incluye un vínculo para ir a la última página, es imperativo que el estado de vista de GridView esté habilitado. (Si la interfaz de paginación no incluye un vínculo directo a la última página, puede deshabilitar el estado de vista).

Al hacer clic en el vínculo de la última página se produce un postback y se indica a GridView que actualice su `PageIndex` propiedad. Si se hace clic en el vínculo de la última página, GridView asigna su propiedad `PageIndex` a un valor inferior a su propiedad `PageCount`. Con el estado de vista deshabilitado, el valor de `PageCount` se pierde en los postbacks y el `PageIndex` tiene asignado el valor entero máximo en su lugar. Después, el control GridView intenta determinar el índice de la fila inicial multiplicando las propiedades `PageSize` y `PageCount`. Esto da como resultado una `OverflowException` porque el producto supera el tamaño máximo permitido.

## <a name="implement-custom-paging-and-sorting"></a>Implementar la paginación y ordenación personalizadas

Nuestra implementación de paginación personalizada actual requiere que el orden en el que se paginan los datos se especifique estáticamente al crear el `GetProductsPaged` procedimiento almacenado. Sin embargo, es posible que haya observado que la etiqueta inteligente GridView s contiene una casilla habilitar ordenación además de la opción Habilitar paginación. Desafortunadamente, agregar compatibilidad de ordenación a GridView con nuestra implementación de paginación personalizada actual solo ordenará los registros de la página de datos que se está viendo actualmente. Por ejemplo, si configura GridView para que también admita la paginación y, a continuación, cuando vea la primera página de datos, ordene por nombre de producto en orden descendente, invertirá el orden de los productos en la Página 1. Como se muestra en la figura 18, se muestra Carnarvon Tigers como primer producto al ordenar en orden alfabético inverso, lo que omite los 71 otros productos que se encuentran después de Carnarvon Tigers, alfabéticamente. en la ordenación solo se tienen en cuenta los registros de la primera página.

[![solo se ordenan los datos que se muestran en la página actual](efficiently-paging-through-large-amounts-of-data-cs/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-cs/_static/image22.png)

**Figura 18**: solo se ordenan los datos mostrados en la página actual ([haga clic para ver la imagen de tamaño completo](efficiently-paging-through-large-amounts-of-data-cs/_static/image24.png))

La ordenación solo se aplica a la página de datos actual, ya que la ordenación se produce después de que los datos se hayan recuperado del método BLL s `GetProductsPaged` y este método solo devuelve los registros de la página específica. Para implementar la ordenación correctamente, es necesario pasar la expresión de ordenación al método `GetProductsPaged` de modo que los datos se puedan clasificar adecuadamente antes de devolver la página de datos específica. Veremos cómo hacerlo en el siguiente tutorial.

## <a name="implementing-custom-paging-and-deleting"></a>Implementar la paginación y eliminación personalizadas

Si habilita la eliminación de la funcionalidad en un control GridView cuyos datos se paginan mediante técnicas de paginación personalizadas, descubrirá que, al eliminar el último registro de la última página, GridView desaparecerá en lugar de disminuir correctamente el `PageIndex`de GridView. Para reproducir este error, habilite la eliminación del tutorial que acabamos de crear. Vaya a la última página (página 9), donde debería ver un solo producto, ya que estamos paginando a través de 81 productos, 10 productos a la vez. Elimine este producto.

Al eliminar el último producto, GridView *debe* ir automáticamente a la octava página y esta funcionalidad se exhibe con paginación predeterminada. Sin embargo, con la paginación personalizada, después de eliminar ese último producto en la última página, GridView simplemente desaparece de la pantalla. El motivo exacto *por* el que ocurre esto es un poco más allá del ámbito de este tutorial. vea [eliminar el último registro de la última página de un control GridView con paginación personalizada](http://scottonwriting.net/sowblog/posts/7326.aspx) para ver los detalles de bajo nivel en lo que se refiere al origen de este problema. En Resumen, se deben a la siguiente secuencia de pasos que GridView realiza al hacer clic en el botón Eliminar:

1. Eliminar el registro
2. Obtiene los registros adecuados que se van a mostrar para el `PageIndex` especificado y `PageSize`
3. Asegúrese de que el `PageIndex` no supera el número de páginas de datos del origen de datos; Si es así, disminuye automáticamente la propiedad `PageIndex` de GridView.
4. Enlazar la página de datos adecuada a GridView usando los registros obtenidos en el paso 2

El problema se debe al hecho de que en el paso 2 el `PageIndex` que se usa al capturar los registros que se van a mostrar sigue siendo el `PageIndex` de la última página cuyo único registro se ha eliminado. Por lo tanto, en el paso 2, no se devuelve *ningún* registro, ya que la última página de datos ya no contiene ningún registro. A continuación, en el paso 3, el control GridView se entiende que su propiedad `PageIndex` es mayor que el número total de páginas del origen de datos (ya que hemos eliminado el último registro de la última página) y, por lo tanto, disminuye su propiedad `PageIndex`. En el paso 4, el control GridView intenta enlazarse a los datos recuperados en el paso 2; sin embargo, en el paso 2 no se devolvieron registros, por lo que se produce un control GridView vacío. Con la paginación predeterminada, este problema no tiene la superficie t porque en el paso 2 se recuperan *todos* los registros del origen de datos.

Para solucionar este comportamiento tenemos dos opciones. El primero es crear un controlador de eventos para el controlador de eventos GridView s `RowDeleted` que determina cuántos registros se han mostrado en la página que se acaba de eliminar. Si solo había un registro, el registro que acaba de eliminar debe haber sido el último y es necesario reducir el `PageIndex`de GridView. Por supuesto, solo queremos actualizar el `PageIndex` si la operación de eliminación se realizó correctamente, lo que se puede determinar asegurándose de que se `null`la propiedad `e.Exception`.

Este enfoque funciona porque actualiza el `PageIndex` después del paso 1, pero antes del paso 2. Por lo tanto, en el paso 2, se devuelve el conjunto de registros adecuado. Para ello, use código similar al siguiente:

[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample11.cs)]

Una solución alternativa consiste en crear un controlador de eventos para el evento ObjectDataSource s `RowDeleted` y establecer la propiedad `AffectedRows` en un valor de 1. Después de eliminar el registro del paso 1 (pero antes de volver a recuperar los datos en el paso 2), GridView actualiza su `PageIndex` propiedad si una o más filas se vieron afectadas por la operación. Sin embargo, ObjectDataSource no establece la propiedad `AffectedRows` y, por tanto, se omite este paso. Una manera de ejecutar este paso consiste en establecer manualmente la propiedad `AffectedRows` si la operación de eliminación se completa correctamente. Esto puede realizarse mediante código como el siguiente:

[!code-csharp[Main](efficiently-paging-through-large-amounts-of-data-cs/samples/sample12.cs)]

El código de ambos controladores de eventos se puede encontrar en la clase de código subyacente del ejemplo `EfficientPaging.aspx`.

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>Comparar el rendimiento de la paginación predeterminada y personalizada

Dado que la paginación personalizada solo recupera los registros necesarios, mientras que la paginación predeterminada devuelve *todos* los registros de cada página que se está viendo, es evidente que la paginación personalizada es más eficaz que la paginación predeterminada. Pero ¿qué más eficaz es la paginación personalizada? ¿Qué tipo de mejoras de rendimiento se pueden observar moviendo de la paginación predeterminada a la paginación personalizada?

Desafortunadamente, no hay un tamaño que se ajuste a todas las respuestas aquí. La ganancia de rendimiento depende de una serie de factores, los dos más destacados, el número de registros que se van a paginar y la carga colocada en el servidor de base de datos y los canales de comunicación entre el servidor Web y el servidor de base de datos. En el caso de las tablas pequeñas con solo unos cuantos registros, la diferencia de rendimiento puede ser insignificante. Sin embargo, en el caso de tablas grandes, con miles de cientos de miles de filas, la diferencia de rendimiento es aguda.

Un artículo de la mina, la [paginación personalizada en ASP.NET 2,0 con SQL Server 2005](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx), contiene algunas pruebas de rendimiento que ejecuto para mostrar las diferencias de rendimiento entre estas dos técnicas de paginación al paginar a través de una tabla de base de datos con registros de 50.000. En estas pruebas, examinamos el tiempo de ejecución de la consulta en el nivel de SQL Server (con [SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) y en la página ASP.net con [las características de seguimiento de ASP.net s](https://msdn.microsoft.com/library/y13fw6we.aspx). Tenga en cuenta que estas pruebas se ejecutaron en el cuadro de desarrollo con un solo usuario activo y, por lo tanto, no son científicas y no imitan los patrones de carga de sitios web típicos. Independientemente de, los resultados muestran las diferencias relativas en el tiempo de ejecución para la paginación predeterminada y personalizada al trabajar con grandes cantidades de datos.

|  | **Promedio de duración (s)** | **Lecturas** |
| --- | --- | --- |
| **SQL Profiler de paginación predeterminada** | 1,411 | 383 |
| **SQL Profiler de paginación personalizada** | 0,002 | 29 |
| **Seguimiento predeterminado de ASP.NET de paginación** | 2,379 | *N/A* |
| **Seguimiento de ASP.NET de paginación personalizada** | 0,029 | *N/A* |

Como puede ver, la recuperación de una página de datos determinada requiere 354 menos lecturas en promedio y completadas en una fracción del tiempo. En la página ASP.NET, se ha personalizado la página para que se representara cerca<sup>del 1/100 de</sup> tiempo que se llevó a cabo cuando se usa la paginación predeterminada. Vea [mi artículo](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx) para obtener más información sobre estos resultados junto con el código y una base de datos que puede descargar para reproducir estas pruebas en su propio entorno.

## <a name="summary"></a>Resumen

La paginación predeterminada es un cinch para implementar simplemente active la casilla habilitar paginación en la etiqueta inteligente control Web de datos, pero esta simplicidad se refiere al costo del rendimiento. Con la paginación predeterminada, cuando un usuario solicita cualquier página de datos, se devuelven *todos* los registros, aunque solo se puede mostrar una pequeña fracción de ellos. Para combatir esta sobrecarga de rendimiento, ObjectDataSource ofrece una opción de paginación personalizada alternativa.

Aunque la paginación personalizada mejora en los problemas de rendimiento de la paginación predeterminada al recuperar solo los registros que deben mostrarse, es más complicado implementar la paginación personalizada. En primer lugar, se debe escribir una consulta que tenga acceso correctamente (y de forma eficaz) al subconjunto específico de registros solicitados. Esto puede realizarse de varias maneras: la que examinamos en este tutorial es usar SQL Server función New `ROW_NUMBER()` de 2005 s para clasificar los resultados y, a continuación, devolver solo los resultados cuya clasificación esté dentro de un intervalo especificado. Además, es necesario agregar un medio para determinar el número total de registros que se van a paginar. Después de crear estos métodos DAL y BLL, también es necesario configurar ObjectDataSource para que pueda determinar el número total de registros que se van a paginar y puede pasar correctamente los valores de índice de la fila inicial y máximo de filas a la capa BLL.

Aunque la implementación de la paginación personalizada requiere una serie de pasos y no es casi tan simple como la paginación predeterminada, la paginación personalizada es una necesidad al paginar a través de cantidades de datos suficientemente grandes. Tal como se ha mostrado en los resultados, la paginación personalizada puede mostrar los segundos en el tiempo de representación de la página ASP.NET y puede aclarar la carga en el servidor de base de datos en uno o varios pedidos de magnitud.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](paging-and-sorting-report-data-cs.md)
> [Siguiente](sorting-custom-paged-data-cs.md)
