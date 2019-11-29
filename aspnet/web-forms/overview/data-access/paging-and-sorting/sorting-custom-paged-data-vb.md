---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: Ordenar datos paginados personalizados (VB) | Microsoft Docs
author: rick-anderson
description: En el tutorial anterior, aprendió a implementar la paginación personalizada al presentar datos en una página web. En este tutorial, veremos cómo extender el anterior...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 934c7558d907611732ae6f04c553bc9e295c569b
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74618495"
---
# <a name="sorting-custom-paged-data-vb"></a>Ordenar datos paginados personalizados (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe) de la aplicación de ejemplo o [descarga de PDF](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> En el tutorial anterior, aprendió a implementar la paginación personalizada al presentar datos en una página web. En este tutorial, veremos cómo ampliar el ejemplo anterior para incluir la compatibilidad con la ordenación de la paginación personalizada.

## <a name="introduction"></a>Introducción

En comparación con la paginación predeterminada, la paginación personalizada puede mejorar el rendimiento de la paginación a través de los datos en varios órdenes de magnitud, lo que hace que la paginación personalizada sea la opción de implementación de paginación de hecho al paginar a través de grandes cantidades La implementación de la paginación personalizada es más complicada que la implementación de la paginación predeterminada, en especial al agregar ordenación a la combinación. En este tutorial extenderemos el ejemplo de la anterior para incluir la compatibilidad con la ordenación *y* la paginación personalizada.

> [!NOTE]
> Como este tutorial se basa en el anterior, antes de empezar, dedique un momento a copiar la sintaxis declarativa dentro del elemento `<asp:Content>` de la página web del tutorial anterior (`EfficientPaging.aspx`) y péguela entre el elemento `<asp:Content>` en la página de `SortParameter.aspx`. Consulte el paso 1 del tutorial [incorporación de controles de validación al editor e inserción de interfaces](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md) para obtener una explicación más detallada sobre la replicación de la funcionalidad de una página ASP.net en otra.

## <a name="step-1-reexamining-the-custom-paging-technique"></a>Paso 1: reexaminar la técnica de paginación personalizada

Para que la paginación personalizada funcione correctamente, debemos implementar alguna técnica que pueda tomar eficazmente un subconjunto determinado de registros, dados los parámetros de índice de fila inicial y máximo de filas. Hay una serie de técnicas que se pueden usar para lograr este objetivo. En el tutorial anterior, hemos examinado el uso de Microsoft SQL Server nueva función de categoría de `ROW_NUMBER()` de 2005 s. En Resumen, la función de categoría `ROW_NUMBER()` asigna un número de fila a cada fila devuelta por una consulta clasificada por un criterio de ordenación especificado. El subconjunto de registros adecuado se obtiene a continuación devolviendo una sección concreta de los resultados numerados. En la consulta siguiente se muestra cómo usar esta técnica para devolver los productos numerados entre 11 y 20 cuando se clasifican los resultados ordenados alfabéticamente por el `ProductName`:

[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

Esta técnica funciona bien para la paginación mediante un criterio de ordenación específico (`ProductName` ordenado alfabéticamente, en este caso), pero es necesario modificar la consulta para mostrar los resultados ordenados por una expresión de ordenación diferente. Idealmente, la consulta anterior podría volver a escribirse para usar un parámetro en la cláusula `OVER`, como por ejemplo:

[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

Desafortunadamente, no se admiten las cláusulas de `ORDER BY` con parámetros. En su lugar, se debe crear un procedimiento almacenado que acepte un `@sortExpression` parámetro de entrada, pero usa una de las siguientes soluciones alternativas:

- Escribir consultas codificadas de forma rígida para cada una de las expresiones de ordenación que se pueden usar; a continuación, use `IF/ELSE` instrucciones T-SQL para determinar la consulta que se va a ejecutar.
- Use una instrucción `CASE` para proporcionar expresiones de `ORDER BY` dinámicas basadas en el parámetro de entrada `@sortExpressio` n; Vea la sección uso para ordenar dinámicamente los resultados de una consulta en [las instrucciones SQL `CASE`](http://www.4guysfromrolla.com/webtech/102704-1.shtml) para obtener más información.
- Cree la consulta adecuada como una cadena en el procedimiento almacenado y, a continuación, use [el `sp_executesql` procedimiento almacenado del sistema](https://msdn.microsoft.com/library/ms188001.aspx) para ejecutar la consulta dinámica.

Cada una de estas soluciones alternativas tiene algunas desventajas. La primera opción no es tan fácil de mantener como la otra, ya que requiere que se cree una consulta para cada expresión de ordenación posible. Por lo tanto, si posteriormente decide agregar nuevos campos que se pueden ordenar a GridView, también necesitará volver atrás y actualizar el procedimiento almacenado. El segundo enfoque tiene algunos matices que presentan problemas de rendimiento al ordenar por columnas de bases de datos que no son de cadena y que también sufren los mismos problemas de mantenimiento que el primero. Y la tercera opción, que usa SQL dinámico, introduce el riesgo de un ataque por inyección de SQL si un atacante puede ejecutar el procedimiento almacenado pasando los valores de parámetro de entrada de su elección.

Aunque ninguno de estos enfoques es perfecto, creo que la tercera opción es la mejor de las tres. Con el uso de SQL dinámico, ofrece un nivel de flexibilidad, mientras que los otros dos no. Además, un ataque por inyección de SQL solo se puede aprovechar si un atacante puede ejecutar el procedimiento almacenado pasando los parámetros de entrada de su elección. Dado que la capa DAL utiliza consultas con parámetros, ADO.NET protegerá los parámetros que se envían a la base de datos a través de la arquitectura, lo que significa que la vulnerabilidad de ataque por inyección de SQL solo existe si el atacante puede ejecutar directamente el procedimiento almacenado.

Para implementar esta funcionalidad, cree un nuevo procedimiento almacenado en la base de datos Northwind denominada `GetProductsPagedAndSorted`. Este procedimiento almacenado debe aceptar tres parámetros de entrada: `@sortExpression`, un parámetro de entrada de tipo `nvarchar(100`) que especifica cómo se deben ordenar los resultados y se insertan directamente después del texto `ORDER BY` en la cláusula `OVER`; y `@startRowIndex` y `@maximumRows`, los mismos dos parámetros de entrada enteros del procedimiento almacenado `GetProductsPaged` examinados en el tutorial anterior. Cree el `GetProductsPagedAndSorted` procedimiento almacenado mediante el siguiente script:

[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

El procedimiento almacenado se inicia asegurándose de que se ha especificado un valor para el parámetro `@sortExpression`. Si faltan, los resultados se clasifican por `ProductID`. A continuación, se crea la consulta SQL dinámica. Tenga en cuenta que la consulta SQL dinámica se diferencia ligeramente de las consultas anteriores usadas para recuperar todas las filas de la tabla Products. En ejemplos anteriores, hemos obtenido los nombres de las categorías y los proveedores asociados de cada producto mediante una subconsulta. Esta decisión se tomó en el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) y se hizo en lugar de usar `JOIN` s porque el TableAdapter no puede crear automáticamente los métodos de inserción, actualización y eliminación asociados para dichas consultas. Sin embargo, el procedimiento almacenado de `GetProductsPagedAndSorted` debe usar `JOIN` s para que los resultados se ordenen por la categoría o los nombres de proveedor.

Esta consulta dinámica se crea concatenando las partes de la consulta estática y los parámetros `@sortExpression`, `@startRowIndex`y `@maximumRows`. Como `@startRowIndex` y `@maximumRows` son parámetros de entero, se deben convertir en nvarchars para que se concatene correctamente. Una vez construida esta consulta SQL dinámica, se ejecuta a través de `sp_executesql`.

Dedique un momento a probar este procedimiento almacenado con valores diferentes para los parámetros `@sortExpression`, `@startRowIndex`y `@maximumRows`. En el Explorador de servidores, haga clic con el botón secundario en el nombre del procedimiento almacenado y elija Ejecutar. Se abrirá el cuadro de diálogo Ejecutar procedimiento almacenado en el que puede escribir los parámetros de entrada (vea la figura 1). Para ordenar los resultados por el nombre de categoría, use CategoryName para el valor del parámetro `@sortExpression`; para ordenar por el nombre de la compañía del proveedor, use CompanyName. Después de proporcionar los valores de los parámetros, haga clic en Aceptar. Los resultados se muestran en la ventana de salida. En la figura 2 se muestran los resultados al devolver los productos clasificados entre 11 y 20 al ordenar por el `UnitPrice` en orden descendente.

![Pruebe valores diferentes para los tres parámetros de entrada del procedimiento almacenado.](sorting-custom-paged-data-vb/_static/image1.png)

**Figura 1**: Pruebe valores diferentes para los tres parámetros de entrada del procedimiento almacenado.

[![los resultados del procedimiento almacenado se muestran en el Ventana de salida](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**Figura 2**: los resultados del procedimiento almacenado se muestran en el ventana de salida ([haga clic para ver la imagen de tamaño completo](sorting-custom-paged-data-vb/_static/image4.png))

> [!NOTE]
> Al clasificar los resultados por la columna de `ORDER BY` especificada en la cláusula `OVER`, SQL Server debe ordenar los resultados. Se trata de una operación rápida si hay un índice clúster en las columnas en las que se ordenan los resultados, o si hay un índice de cobertura, pero puede ser más costoso en caso contrario. Para mejorar el rendimiento de las consultas suficientemente grandes, considere la posibilidad de agregar un índice no clúster para la columna por la que se ordenan los resultados. Consulte [funciones de clasificación y rendimiento en SQL Server 2005](http://www.sql-server-performance.com/ak_ranking_functions.asp) para obtener más información.

## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>Paso 2: aumentar el acceso a datos y las capas de lógica de negocios

Una vez creado el `GetProductsPagedAndSorted` procedimiento almacenado, el siguiente paso es proporcionar un medio para ejecutar ese procedimiento almacenado a través de la arquitectura de la aplicación. Esto implica agregar un método adecuado a la capa DAL y a la capa BLL. Permita que s Comience agregando un método a la capa DAL. Abra el `Northwind.xsd` conjunto de bits con tipo, haga clic con el botón derecho en el `ProductsTableAdapter`y elija la opción Agregar consulta en el menú contextual. Como hicimos en el tutorial anterior, queremos configurar este nuevo método DAL para usar un procedimiento almacenado `GetProductsPagedAndSorted`existente, en este caso. Empiece por indicar que desea que el nuevo método TableAdapter use un procedimiento almacenado existente.

![Elija usar un procedimiento almacenado existente](sorting-custom-paged-data-vb/_static/image5.png)

**Figura 3**: elegir usar un procedimiento almacenado existente

Para especificar el procedimiento almacenado que se va a usar, seleccione el `GetProductsPagedAndSorted` procedimiento almacenado en la lista desplegable de la pantalla siguiente.

![Usar el procedimiento almacenado GetProductsPagedAndSorted](sorting-custom-paged-data-vb/_static/image6.png)

**Figura 4**: uso del procedimiento almacenado GetProductsPagedAndSorted

Este procedimiento almacenado devuelve un conjunto de registros como resultado, por lo que en la siguiente pantalla, indica que devuelve datos tabulares.

![Indicar que el procedimiento almacenado devuelve datos tabulares](sorting-custom-paged-data-vb/_static/image7.png)

**Figura 5**: indicar que el procedimiento almacenado devuelve datos tabulares

Por último, cree métodos DAL que usen los patrones rellenar un DataTable y devolver un objeto DataTable, denominando los métodos `FillPagedAndSorted` y `GetProductsPagedAndSorted`, respectivamente.

![Elegir los nombres de los métodos](sorting-custom-paged-data-vb/_static/image8.png)

**Figura 6**: elegir los nombres de los métodos

Ahora que hemos ampliado la capa DAL, se vuelve a preparar para pasar a la capa BLL. Abra el archivo de clase `ProductsBLL` y agregue un nuevo método, `GetProductsPagedAndSorted`. Este método debe aceptar tres parámetros de entrada `sortExpression`, `startRowIndex`y `maximumRows` y simplemente debe llamar al método de `GetProductsPagedAndSorted` de la capa DAL, de la siguiente manera:

[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>Paso 3: configuración de ObjectDataSource para pasar el parámetro SortExpression

Al haber aumentado la capa DAL y la capa BLL para incluir métodos que usan el `GetProductsPagedAndSorted` procedimiento almacenado, todo lo que queda es configurar el origen ObjectDataSource en la página `SortParameter.aspx` para usar el nuevo método BLL y pasar el parámetro `SortExpression` en función de la columna que el usuario haya solicitado para ordenar los resultados.

Comience cambiando el `SelectMethod` de ObjectDataSource de `GetProductsPaged` a `GetProductsPagedAndSorted`. Esto puede realizarse a través del Asistente para configurar orígenes de datos, desde el ventana Propiedades o directamente a través de la sintaxis declarativa. A continuación, es necesario proporcionar un valor para la [propiedad`SortParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx)de ObjectDataSource. Si se establece esta propiedad, el ObjectDataSource intenta pasar la propiedad `SortExpression` de GridView al `SelectMethod`. En concreto, ObjectDataSource busca un parámetro de entrada cuyo nombre es igual al valor de la propiedad `SortParameterName`. Dado que el método de `GetProductsPagedAndSorted` BLL s tiene el parámetro de entrada expresión de ordenación denominado `sortExpression`, establezca la propiedad ObjectDataSource s `SortExpression` en sortExpression.

Después de realizar estos dos cambios, la sintaxis declarativa s de ObjectDataSource debe ser similar a la siguiente:

[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> Como en el tutorial anterior, asegúrese de que ObjectDataSource no *incluya los* parámetros de entrada SortExpression, StartRowIndex o maximumRows en su colección SelectParameters.

Para habilitar la ordenación en GridView, active la casilla habilitar ordenación en la etiqueta inteligente GridView s, que establece la propiedad GridView s `AllowSorting` en `true` y que hace que el texto del encabezado de cada columna se represente como LinkButton. Cuando el usuario final hace clic en uno de los LinkButtons de encabezado, se recorre un postback y se siguen los pasos siguientes:

1. GridView actualiza su [propiedad`SortExpression`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx) al valor de la `SortExpression` del campo en cuyo vínculo de encabezado se hizo clic.
2. ObjectDataSource invoca el método BLL s `GetProductsPagedAndSorted`, pasando la propiedad `SortExpression` de GridView como el valor para el parámetro de entrada Method s `sortExpression` (junto con los valores adecuados `startRowIndex` y `maximumRows` parámetro de entrada)
3. La capa BLL invoca el método `GetProductsPagedAndSorted` de DAL.
4. La capa DAL ejecuta el `GetProductsPagedAndSorted` procedimiento almacenado, pasando el parámetro `@sortExpression` (junto con los valores de los parámetros de entrada `@startRowIndex` y `@maximumRows`)
5. El procedimiento almacenado devuelve el subconjunto de datos adecuado a la capa BLL, que lo devuelve a ObjectDataSource; después, estos datos se enlazan a GridView, se representan en HTML y se envían al usuario final.

La figura 7 muestra la primera página de resultados cuando se ordena por el `UnitPrice` en orden ascendente.

[![los resultados se ordenan por UnitPrice](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**Figura 7**: los resultados se ordenan por UnitPrice ([haga clic para ver la imagen de tamaño completo](sorting-custom-paged-data-vb/_static/image11.png))

Aunque la implementación actual puede ordenar correctamente los resultados por nombre de producto, nombre de categoría, cantidad por unidad y precio unitario, si intenta ordenar los resultados por el nombre de proveedor, se produce una excepción en tiempo de ejecución (vea la figura 8).

![Al intentar ordenar los resultados por el proveedor, se produce la siguiente excepción en tiempo de ejecución](sorting-custom-paged-data-vb/_static/image12.png)

**Figura 8**: el intento de ordenar los resultados por el proveedor produce la siguiente excepción en tiempo de ejecución

Esta excepción se produce porque la `SortExpression` de GridView s `SupplierName` BoundField está establecida en `SupplierName`. Sin embargo, en realidad se llama al nombre del proveedor en la tabla `Suppliers` `CompanyName` se ha asignado un alias a este nombre de columna como `SupplierName`. Sin embargo, la cláusula `OVER` utilizada por la función `ROW_NUMBER()` no puede usar el alias y debe utilizar el nombre de columna real. Por lo tanto, cambie el `SupplierName` BoundField s `SortExpression` de NombreProveedor a CompanyName (consulte la figura 9). Como se muestra en la figura 10, después de este cambio, los resultados se pueden ordenar por el proveedor.

![Cambiar el NombreProveedor BoundField s SortExpression a CompanyName](sorting-custom-paged-data-vb/_static/image13.png)

**Figura 9**: cambio del NombreProveedor BoundField s SortExpression a CompanyName

[![los resultados se pueden ordenar por proveedor](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**Figura 10**: los resultados se pueden ordenar por proveedor ([haga clic para ver la imagen de tamaño completo](sorting-custom-paged-data-vb/_static/image16.png))

## <a name="summary"></a>Resumen

La implementación de paginación personalizada que examinamos en el tutorial anterior requería que el orden en el que se ordenaran los resultados se especificara en tiempo de diseño. En Resumen, esto significaba que la implementación de paginación personalizada que hemos implementado no podía, al mismo tiempo, proporcionar funciones de ordenación. En este tutorial se superaron esta limitación extendiendo el procedimiento almacenado desde el primero para incluir un `@sortExpression` parámetro de entrada mediante el cual se pueden ordenar los resultados.

Después de crear este procedimiento almacenado y crear nuevos métodos en la capa DAL y BLL, pudimos implementar un control GridView que ofrecía tanto la ordenación como la paginación personalizada mediante la configuración de ObjectDataSource para pasar la propiedad `SortExpression` actual de GridView a la `SelectMethod`BLL.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Carlos Santos. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](efficiently-paging-through-large-amounts-of-data-vb.md)
> [Siguiente](creating-a-customized-sorting-user-interface-vb.md)
