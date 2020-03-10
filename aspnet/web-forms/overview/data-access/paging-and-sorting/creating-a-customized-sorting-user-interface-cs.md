---
uid: web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
title: Crear una interfaz de usuario de ordenaciónC#personalizada () | Microsoft Docs
author: rick-anderson
description: Al mostrar una larga lista de datos ordenados, puede resultar muy útil agrupar los datos relacionados mediante la introducción de filas de separadores. En este tutorial veremos cómo CRE...
ms.author: riande
ms.date: 08/15/2006
ms.assetid: 6f81b633-9d01-4e52-ae4a-2ea6bc109475
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/creating-a-customized-sorting-user-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: 93ec07a13de80e4c874ff46b5dfa626b60b632c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78502711"
---
# <a name="creating-a-customized-sorting-user-interface-c"></a>Crear una interfaz de usuario de ordenación personalizada (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_27_CS.exe) de la aplicación de ejemplo o [descarga de PDF](creating-a-customized-sorting-user-interface-cs/_static/datatutorial27cs1.pdf)

> Al mostrar una larga lista de datos ordenados, puede resultar muy útil agrupar los datos relacionados mediante la introducción de filas de separadores. En este tutorial, veremos cómo crear una interfaz de usuario de ordenación.

## <a name="introduction"></a>Introducción

Cuando se muestra una larga lista de datos ordenados en los que solo hay unos cuantos valores distintos en la columna ordenada, un usuario final puede resultarle difícil discernir dónde se encuentran exactamente los límites de diferencia. Por ejemplo, hay 81 productos en la base de datos, pero solo nueve opciones de categoría diferentes (ocho categorías únicas más la opción `NULL`). Considere el caso de un usuario que esté interesado en examinar los productos que se encuentran en la categoría mariscos. En una página en la que se enumeran *todos* los productos de un solo GridView, el usuario puede decidir que su mejor apuesta es ordenar los resultados por categoría, que agrupará todos los productos de mariscos juntos. Después de ordenar por categoría, el usuario tiene que buscar en la lista, buscando dónde empiezan y acaban los productos con grupos de mariscos. Puesto que los resultados se ordenan alfabéticamente por el nombre de la categoría, la búsqueda de los productos de mariscos no es difícil, pero sigue siendo necesario examinar atentamente la lista de elementos de la cuadrícula.

Para ayudar a resaltar los límites entre los grupos ordenados, muchos sitios web emplean una interfaz de usuario que agrega un separador entre estos grupos. Los separadores como los que se muestran en la ilustración 1 permiten a los usuarios encontrar más rápidamente un grupo determinado e identificar sus límites, así como determinar qué grupos distintos existen en los datos.

[![cada grupo de categorías se identifica claramente](creating-a-customized-sorting-user-interface-cs/_static/image2.png)](creating-a-customized-sorting-user-interface-cs/_static/image1.png)

**Figura 1**: cada grupo de categorías se identifica claramente ([haga clic para ver la imagen de tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image3.png))

En este tutorial, veremos cómo crear una interfaz de usuario de ordenación.

## <a name="step-1-creating-a-standard-sortable-gridview"></a>Paso 1: crear un GridView estándar, que se ordene

Antes de explorar cómo aumentar el control GridView para proporcionar la interfaz de ordenación mejorada, vamos a crear primero un GridView estándar, que se pueda ordenar y que muestre los productos. Para empezar, abra la página `CustomSortingUI.aspx` de la carpeta `PagingAndSorting`. Agregue un control GridView a la página, establezca su propiedad `ID` en `ProductList`y enlácelo a un nuevo ObjectDataSource. Configure ObjectDataSource para usar el método `ProductsBLL` Class s `GetProducts()` para seleccionar registros.

A continuación, configure el control GridView de modo que solo contenga los `ProductName`, `CategoryName`, `SupplierName`y `UnitPrice` BoundFields y el CheckBoxField discontinuo. Por último, configure GridView para admitir la ordenación activando la casilla habilitar ordenación en la etiqueta inteligente de GridView s (o estableciendo su propiedad `AllowSorting` en `true`). Después de hacer estas adiciones a la página `CustomSortingUI.aspx`, el marcado declarativo debe ser similar al siguiente:

[!code-aspx[Main](creating-a-customized-sorting-user-interface-cs/samples/sample1.aspx)]

Tómese un momento para ver el progreso hasta ahora en un explorador. En la ilustración 2 se muestra el GridView ordenable cuando sus datos se ordenan por Categoría en orden alfabético.

[![los datos de GridView ordenables se ordenan por categoría](creating-a-customized-sorting-user-interface-cs/_static/image5.png)](creating-a-customized-sorting-user-interface-cs/_static/image4.png)

**Figura 2**: los datos de GridView que se ordenan se ordenan por categoría ([haga clic para ver la imagen de tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image6.png))

## <a name="step-2-exploring-techniques-for-adding-the-separator-rows"></a>Paso 2: explorar técnicas para agregar las filas de separadores

Con el GridView genérico, que se puede ordenar, todo lo que queda es poder agregar las filas del separador en GridView antes de cada grupo ordenado único. Pero ¿cómo se pueden insertar esas filas en el control GridView? En esencia, es necesario recorrer en iteración las filas de GridView, determinar dónde se producen las diferencias entre los valores de la columna ordenada y, a continuación, agregar la fila de separador adecuada. Al pensar en este problema, parece natural que la solución se encuentra en algún lugar del controlador de eventos GridView s `RowDataBound`. Como se explicó en el tutorial [sobre el formato personalizado basado en datos](../custom-formatting/custom-formatting-based-upon-data-cs.md) , este controlador de eventos se usa normalmente al aplicar el formato de nivel de fila en función de los datos de las filas. Sin embargo, el controlador de eventos `RowDataBound` no es la solución aquí, ya que las filas no se pueden agregar a GridView mediante programación desde este controlador de eventos. De hecho, la colección GridView s `Rows` es de solo lectura.

Para agregar más filas a GridView tenemos tres opciones:

- Agregue estas filas de separadores de metadatos a los datos reales enlazados a GridView.
- Una vez enlazado GridView a los datos, agregue instancias de `TableRow` adicionales a la colección de controles GridView s.
- Crear un control de servidor personalizado que extienda el control GridView e invalide los métodos responsables de construir la estructura GridView s

La creación de un control de servidor personalizado sería el mejor enfoque si esta funcionalidad fuera necesaria en muchas páginas web o en varios sitios Web. Sin embargo, implicaría bastante código y una exploración exhaustiva de las profundidades de los trabajos internos de GridView. Por lo tanto, no tendremos en cuenta esta opción para este tutorial.

Las otras dos opciones son Agregar filas de separadores a los datos reales que se están enlazando a GridView y manipular la colección de controles de GridView después de que se haya enlazado el problema de forma diferente y merecen un debate.

## <a name="adding-rows-to-the-data-bound-to-the-gridview"></a>Agregar filas a los datos enlazados a GridView

Cuando GridView se enlaza a un origen de datos, crea un `GridViewRow` para cada registro devuelto por el origen de datos. Por lo tanto, podemos insertar las filas de separadores necesarias agregando los registros del separador al origen de datos antes de enlazarlas a GridView. En la figura 3 se muestra este concepto.

![Una técnica implica Agregar filas de separador al origen de datos](creating-a-customized-sorting-user-interface-cs/_static/image7.png)

**Figura 3**: una técnica implica Agregar filas de separador al origen de datos

Utilizo los registros de separadores de términos entre comillas porque no hay ningún registro de separador especial; en su lugar, debemos marcar de algún modo que un registro determinado en el origen de datos sirve como separador en lugar de una fila de datos normal. En nuestros ejemplos, se vuelve a enlazar una instancia de `ProductsDataTable` a GridView, que se compone de `ProductRows`. Podríamos marcar un registro como una fila de separador estableciendo su propiedad `CategoryID` en `-1` (puesto que dicho valor no podía existir con normalidad).

Para usar esta técnica, es necesario realizar los pasos siguientes:

1. Recuperar mediante programación los datos para enlazar a GridView (una instancia `ProductsDataTable`)
2. Ordenar los datos en función de las propiedades `SortExpression` y `SortDirection` de GridView
3. Recorrer en iteración el `ProductsRows` en el `ProductsDataTable`, buscando dónde se encuentran las diferencias en la columna ordenada
4. En cada límite de grupo, inserte un registro de separador `ProductsRow` instancia en la DataTable, una que tenga `CategoryID` establecido en `-1` (o cualquier designación en la que se haya decidido marcar un registro como un registro de separador).
5. Después de insertar las filas de separadores, enlace mediante programación los datos a GridView

Además de estos cinco pasos, también es necesario proporcionar un controlador de eventos para el evento `RowDataBound` de GridView. Aquí, se comprueba cada `DataRow` y se determina si se trata de una fila de separador, una cuya configuración de `CategoryID` se `-1`. Si es así, es posible que desee ajustar su formato o el texto que se muestra en las celdas.

El uso de esta técnica para insertar los límites de los grupos de ordenación requiere un poco más de trabajo que el descrito anteriormente, ya que también debe proporcionar un controlador de eventos para el evento GridView s `Sorting` y realizar un seguimiento de los valores `SortExpression` y `SortDirection`.

## <a name="manipulating-the-gridview-s-control-collection-after-it-s-been-databound"></a>Manipular la colección de controles GridView s una vez que se ha enlazado a un enlace de objetos

En lugar de enviar mensajes a los datos antes de enlazarlos a GridView, podemos agregar las filas de separadores *después* de que los datos se hayan enlazado a GridView. El proceso de enlace de datos crea la jerarquía de controles GridView s, que en realidad es simplemente una instancia de `Table` formada por una colección de filas, cada una de las cuales se compone de una colección de celdas. En concreto, la colección de controles GridView s contiene un objeto `Table` en su raíz, una `GridViewRow` (que se deriva de la clase `TableRow`) para cada registro del `DataSource` enlazado a GridView y un objeto `TableCell` en cada instancia `GridViewRow` para cada campo de datos de la `DataSource`.

Para agregar filas de separadores entre cada grupo de ordenación, podemos manipular directamente esta jerarquía de controles una vez creada. Podemos estar seguros de que se ha creado la jerarquía de controles GridView s por última vez en el momento en que se represente la página. Por lo tanto, este enfoque invalida el método de `Render` de clase `Page`, momento en el que la jerarquía de control final de GridView s se actualiza para incluir las filas de separadores necesarias. En la figura 4 se ilustra este proceso.

[![una técnica alternativa manipula la jerarquía de controles GridView s](creating-a-customized-sorting-user-interface-cs/_static/image9.png)](creating-a-customized-sorting-user-interface-cs/_static/image8.png)

**Figura 4**: una técnica alternativa manipula la jerarquía de controles de GridView ([haga clic para ver la imagen de tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image10.png))

En este tutorial, usaremos este último enfoque para personalizar la experiencia del usuario de ordenación.

> [!NOTE]
> El código que se presenta en este tutorial se basa en el ejemplo que se proporciona en la entrada de blog de [Teemu Keiski](http://aspadvice.com/blogs/joteke/default.aspx) s, lo que [reproduce un poco con la agrupación de ordenación de GridView](http://aspadvice.com/blogs/joteke/archive/2006/02/11/15130.aspx).

## <a name="step-3-adding-the-separator-rows-to-the-gridview-s-control-hierarchy"></a>Paso 3: agregar las filas del separador a la jerarquía de controles GridView s

Puesto que solo queremos agregar las filas de separador a la jerarquía de controles GridView s después de que su jerarquía de control se haya creado y creado por última vez en esa visita de página, queremos realizar esta adición al final del ciclo de vida de la página, pero antes del GridView real la jerarquía de altrol se ha representado en HTML. El último punto posible en el que podemos lograr esto es el evento `Page` Class s `Render`, que podemos reemplazar en nuestra clase de código subyacente con la siguiente firma de método:

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample2.cs)]

Cuando se invoca el método de `Render` de clase s original de `Page` `base.Render(writer)` se representará cada uno de los controles de la página, lo que generará el marcado en función de su jerarquía de controles. Por lo tanto, es imperativo llamar a `base.Render(writer)`, de modo que la página se represente y que se manipule la jerarquía de controles GridView s antes de llamar a `base.Render(writer)`, de modo que las filas del separador se hayan agregado a la jerarquía de controles GridView s antes de que se representen.

Para insertar los encabezados de grupo de ordenación, primero debemos asegurarnos de que el usuario ha solicitado que se ordenen los datos. De forma predeterminada, el contenido de GridView no está ordenado y, por lo tanto, no es necesario escribir ningún encabezado de ordenación de grupo.

> [!NOTE]
> Si desea que GridView se ordene por una columna determinada cuando se cargue la página por primera vez, llame al método GridView s `Sort` en la primera página visite (pero no en postbacks posteriores). Para ello, agregue esta llamada en el controlador de eventos `Page_Load` dentro de un `if (!Page.IsPostBack)` condicional. Consulte la información del tutorial sobre la [paginación y la ordenación de datos de informe](paging-and-sorting-report-data-cs.md) para obtener más información sobre el método `Sort`.

Suponiendo que los datos se han ordenado, la siguiente tarea consiste en determinar la columna por la que se ordenan los datos y, a continuación, examinar las filas para buscar las diferencias en los valores de esa columna. El código siguiente garantiza que los datos se han ordenado y busca la columna por la que se han ordenado los datos:

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample3.cs)]

Si el control GridView todavía está ordenado, no se habrá establecido la propiedad GridView s `SortExpression`. Por lo tanto, solo queremos agregar las filas de separador si esta propiedad tiene algún valor. Si lo hace, es necesario determinar el índice de la columna por la que se ordenan los datos. Esto se logra recorriendo en bucle la colección de `Columns` de GridView, buscando la columna cuya propiedad `SortExpression` es igual a la propiedad `SortExpression` de GridView. Además de la columna s index, también se obtiene la propiedad `HeaderText`, que se usa al mostrar las filas del separador.

Con el índice de la columna por la que se ordenan los datos, el último paso es enumerar las filas de GridView. Para cada fila, es necesario determinar si el valor de la columna ordenada es distinto del valor de la fila anterior de la columna ordenada s. Si es así, es necesario insertar una nueva instancia de `GridViewRow` en la jerarquía de controles. Esto se logra con el código siguiente:

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample4.cs)]

Este código comienza mediante programación para hacer referencia al objeto `Table` que se encuentra en la raíz de la jerarquía de controles GridView s y para crear una variable de cadena denominada `lastValue`. `lastValue` se usa para comparar el valor de la columna ordenada de la fila actual con el valor de la fila anterior. A continuación, se enumera la colección de `Rows` GridView s y, para cada fila, el valor de la columna ordenada se almacena en la variable `currentValue`.

> [!NOTE]
> Para determinar el valor de la columna de filas determinada, use la propiedad celda s `Text`. Esto funciona bien para BoundFields, pero no funcionará como se desea en TemplateFields, CheckBoxFields, etc. Veremos cómo tener en cuenta los campos de GridView alternativos en breve.

A continuación, se comparan las variables `currentValue` y `lastValue`. Si difieren, es necesario agregar una nueva fila de separador a la jerarquía de controles. Esto se logra determinando el índice del `GridViewRow` en la colección `Table` objetos `Rows`, creando nuevas instancias de `GridViewRow` y `TableCell` y, a continuación, agregando `TableCell` y `GridViewRow` a la jerarquía de controles.

Tenga en cuenta que la fila del separador `TableCell` tiene el formato que abarca todo el ancho del control GridView, se le da formato mediante la clase CSS `SortHeaderRowStyle` y tiene su propiedad `Text` de modo que muestra tanto el nombre del grupo de ordenación (como la categoría) como el valor del grupo s (por ejemplo, bebidas). Por último, `lastValue` se actualiza al valor de `currentValue`.

La clase CSS utilizada para dar formato a la fila de encabezado de grupo de ordenación `SortHeaderRowStyle` debe especificarse en el archivo de `Styles.css`. No dude en usar cualquier aspecto de configuración de estilo para usted. He usado lo siguiente:

[!code-css[Main](creating-a-customized-sorting-user-interface-cs/samples/sample5.css)]

Con el código actual, la interfaz de ordenación agrega encabezados de grupo de ordenación al ordenar por cualquier BoundField (consulte la figura 5, que muestra una captura de pantalla al ordenar por proveedor). Sin embargo, al ordenar por cualquier otro tipo de campo (por ejemplo, CheckBoxField o TemplateField), los encabezados de grupo de ordenación no se encuentran en ninguna parte (vea la figura 6).

[![la interfaz de ordenación incluye encabezados de grupo de ordenación al ordenar por BoundFields](creating-a-customized-sorting-user-interface-cs/_static/image12.png)](creating-a-customized-sorting-user-interface-cs/_static/image11.png)

**Figura 5**: la interfaz de ordenación incluye encabezados de grupo de ordenación al ordenar por BoundFields ([haga clic para ver la imagen de tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image13.png))

[![faltan los encabezados de grupo de ordenación al ordenar un CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image15.png)](creating-a-customized-sorting-user-interface-cs/_static/image14.png)

**Figura 6**: faltan los encabezados de grupo de ordenación al ordenar un CheckBoxField ([haga clic para ver la imagen de tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image16.png))

La razón por la que faltan los encabezados de grupo de ordenación cuando se ordena por un CheckBoxField es porque el código usa actualmente únicamente la propiedad `TableCell` s `Text` para determinar el valor de la columna ordenada para cada fila. En el caso de CheckBoxFields, la propiedad `TableCell` s `Text` es una cadena vacía. en su lugar, el valor está disponible a través de un control Web CheckBox que se encuentra dentro de la colección `TableCell` s `Controls`.

Para controlar los tipos de campo distintos de BoundFields, es necesario aumentar el código donde se asigna la variable `currentValue` para comprobar la existencia de una casilla en la colección `TableCell` s `Controls`. En lugar de usar `currentValue = gvr.Cells[sortColumnIndex].Text`, reemplace este código por lo siguiente:

[!code-csharp[Main](creating-a-customized-sorting-user-interface-cs/samples/sample6.cs)]

Este código examina la columna ordenada `TableCell` de la fila actual para determinar si hay algún control en la colección `Controls`. Si hay, y el primer control es una casilla, la variable `currentValue` se establece en sí o no, dependiendo de la propiedad CheckBox `Checked`. De lo contrario, el valor se toma de la propiedad `TableCell` s `Text`. Esta lógica se puede replicar para controlar la ordenación de cualquier TemplateFields que pueda existir en GridView.

Con la adición de código anterior, los encabezados de grupo de ordenación ahora están presentes cuando se ordena por el CheckBoxField discontinuo (consulte la figura 7).

[![los encabezados del grupo de ordenación ahora están presentes al ordenar un CheckBoxField](creating-a-customized-sorting-user-interface-cs/_static/image18.png)](creating-a-customized-sorting-user-interface-cs/_static/image17.png)

**Figura 7**: los encabezados del grupo de ordenación ahora están presentes al ordenar una CheckBoxField ([haga clic para ver la imagen de tamaño completo](creating-a-customized-sorting-user-interface-cs/_static/image19.png))

> [!NOTE]
> Si tiene productos con `NULL` valores de la base de datos para los campos `CategoryID`, `SupplierID`o `UnitPrice`, esos valores aparecerán como cadenas vacías en GridView de forma predeterminada, lo que significa que el texto de la fila del separador de los productos con valores `NULL` se leerá como categoría: (es decir, no hay ningún nombre después de la categoría: como con la categoría: bebidas). Si desea que se muestre un valor aquí, puede establecer la [propiedad BoundFields`NullDisplayText`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.boundfield.nulldisplaytext.aspx) en el texto que desea mostrar, o bien puede Agregar una instrucción condicional en el método Render al asignar el `currentValue` a la propiedad separador row s `Text`.

## <a name="summary"></a>Resumen

GridView no incluye muchas opciones integradas para personalizar la interfaz de ordenación. Sin embargo, con un poco de código de bajo nivel, es posible ajustar la jerarquía de controles GridView s para crear una interfaz más personalizada. En este tutorial vimos cómo agregar una fila de separador de grupos de ordenación para un control GridView que se pueda ordenar, lo que identifica más fácilmente los límites de los grupos distintos y los grupos. Para obtener más ejemplos de interfaces de ordenación personalizadas, consulte la entrada de blog sobre [Scott Guthrie](https://weblogs.asp.net/scottgu/) s de [ASP.net 2,0 GridView](https://weblogs.asp.net/scottgu/archive/2006/02/11/437995.aspx) .

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](sorting-custom-paged-data-cs.md)
> [Siguiente](paging-and-sorting-report-data-vb.md)
