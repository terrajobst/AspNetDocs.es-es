---
uid: web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
title: Almacenar datos en caché con ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: El almacenamiento en caché puede significar la diferencia entre una aplicación web lenta y rápida. Este tutorial es el primero de los cuatro que tienen una visión detallada del almacenamiento en caché en ASP.NET...
ms.author: riande
ms.date: 05/30/2007
ms.assetid: 2e56a733-5512-48a6-9276-70a65bbe4d5d
msc.legacyurl: /web-forms/overview/data-access/caching-data/caching-data-with-the-objectdatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: 16f20d9a0f4f677073174d680418b278dba40b07
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74612217"
---
# <a name="caching-data-with-the-objectdatasource-vb"></a>Almacenar datos en caché con ObjectDataSource (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_58_VB.exe) de la aplicación de ejemplo o [descarga de PDF](caching-data-with-the-objectdatasource-vb/_static/datatutorial58vb1.pdf)

> El almacenamiento en caché puede significar la diferencia entre una aplicación web lenta y rápida. Este tutorial es el primero de los cuatro que tienen una visión detallada del almacenamiento en caché en ASP.NET. Conozca los conceptos clave del almacenamiento en caché y cómo aplicar el almacenamiento en caché a la capa de presentación a través del control ObjectDataSource.

## <a name="introduction"></a>Introducción

En informática, el *almacenamiento en caché* es el proceso de tomar datos o información que son caros de obtener y almacenar una copia de él en una ubicación que es más rápida de acceder. En el caso de las aplicaciones controladas por datos, las consultas grandes y complejas normalmente consumen la mayor parte del tiempo de ejecución de la aplicación. Este tipo de rendimiento de la aplicación, a menudo, se puede mejorar mediante el almacenamiento de los resultados de consultas de base de datos caras en la memoria de la aplicación.

ASP.NET 2,0 ofrece una variedad de opciones de almacenamiento en caché. Se puede almacenar en caché una página web completa o el marcado representado de control de usuario en la memoria caché de *resultados*. Los controles ObjectDataSource y SqlDataSource también proporcionan capacidades de almacenamiento en caché, lo que permite almacenar datos en caché en el nivel de control. Y ASP.NET s *Data cache* proporciona una API de almacenamiento en caché enriquecida que permite a los desarrolladores de páginas almacenar en caché objetos mediante programación. En este tutorial y en los tres siguientes examinaremos el uso de las características de almacenamiento en caché de ObjectDataSource s y la memoria caché de datos. También exploraremos cómo almacenar en caché los datos de la aplicación en el inicio y cómo mantener actualizados los datos almacenados en caché mediante el uso de dependencias de caché de SQL. Estos tutoriales no exploran el almacenamiento en caché de resultados. Para obtener una visión detallada del almacenamiento en caché de resultados, vea [almacenamiento en caché de resultados en ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx).

El almacenamiento en caché puede aplicarse en cualquier lugar de la arquitectura, desde la capa de acceso a datos hasta el nivel de presentación. En este tutorial veremos cómo aplicar el almacenamiento en caché a la capa de presentación a través del control ObjectDataSource. En el siguiente tutorial, examinaremos los datos de almacenamiento en caché en el nivel de lógica de negocios.

## <a name="key-caching-concepts"></a>Conceptos de Key Caching

El almacenamiento en caché puede mejorar considerablemente el rendimiento general y la escalabilidad de las aplicaciones, ya que toma datos que son caros de generar y almacenar una copia en una ubicación a la que se puede tener acceso de forma más eficaz. Dado que la memoria caché solo contiene una copia de los datos subyacentes reales, puede quedar obsoleta o *obsoleta*si cambian los datos subyacentes. Para combatir esto, un desarrollador de páginas puede indicar los criterios por los que se *expulsará* el elemento de caché de la memoria caché, mediante:

- **Criterios basados en el tiempo** que un elemento se puede Agregar a la memoria caché para una duración absoluta o variable. Por ejemplo, un desarrollador de páginas puede indicar una duración de, por ejemplo, 60 segundos. Con una duración absoluta, el elemento almacenado en caché se expulsa 60 segundos después de agregarse a la memoria caché, independientemente de la frecuencia con la que se obtuvo acceso a él. Con una duración variable, el elemento almacenado en caché se expulsa 60 segundos después del último acceso.
- **Criterios basados en dependencia:** una dependencia puede asociarse a un elemento cuando se agrega a la memoria caché. Cuando el elemento s Dependency cambia, se expulsa de la memoria caché. La dependencia puede ser un archivo, otro elemento de caché o una combinación de ambos. ASP.NET 2,0 también permite las dependencias de caché de SQL, que permiten a los desarrolladores agregar un elemento a la memoria caché y desalojarlos cuando cambian los datos de la base de datos subyacente. Examinaremos las dependencias de caché de SQL en el tutorial próximo [uso de dependencias de caché de SQL](using-sql-cache-dependencies-vb.md) .

Independientemente de los criterios de expulsión especificados, es *posible que un* elemento de la memoria caché se elimine antes de que se cumplan los criterios basados en el tiempo o en la dependencia. Si la memoria caché ha alcanzado su capacidad, los elementos existentes deben quitarse antes de que se puedan agregar otros nuevos. Por consiguiente, al trabajar mediante programación con datos almacenados en caché, es fundamental que siempre asuma que los datos almacenados en caché pueden no estar presentes. Veremos el patrón que se va a usar al obtener acceso a los datos de la memoria caché mediante programación en el siguiente tutorial, en *el que se almacenan en caché los datos de la arquitectura*.

El almacenamiento en caché proporciona un medio económico para obtener más rendimiento de una aplicación. Como [Steven Smith](http://aspadvice.com/blogs/ssmith/) articula en su artículo [ASP.net Caching: técnicas y procedimientos recomendados](https://msdn.microsoft.com/library/aa478965.aspx):

El almacenamiento en caché puede ser una buena manera de obtener un rendimiento suficientemente bueno sin requerir mucho tiempo y análisis. La memoria es barata, por lo que si puede obtener el rendimiento que necesita almacenando en caché la salida durante 30 segundos en lugar de dedicar un día o una semana intentando optimizar el código o la base de datos, realice la solución de almacenamiento en caché (suponiendo que los datos antiguos de 30 segundos sean correctos) y continúen. Finalmente, es probable que el diseño sea deficiente, por lo que, por supuesto, debe intentar diseñar las aplicaciones correctamente. Pero si solo necesita obtener un buen rendimiento suficiente hoy, el almacenamiento en caché puede ser un excelente [enfoque], lo que le permite refactorizar su aplicación en una fecha posterior cuando tenga tiempo para hacerlo.

Aunque el almacenamiento en caché puede proporcionar mejoras de rendimiento apreciables, no es aplicable en todas las situaciones, como en las aplicaciones que usan datos de actualización frecuente en tiempo real o en los que los datos obsoletos de corta duración no sean aceptables. Pero para la mayoría de las aplicaciones, se debe usar el almacenamiento en caché. Para obtener más información sobre el almacenamiento en caché en ASP.NET 2,0, consulte la sección [almacenamiento en caché para el rendimiento](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/default.aspx) de los [tutoriales de inicio rápido de ASP.net 2,0](https://quickstarts.asp.net/QuickStartv20/aspnet/).

## <a name="step-1-creating-the-caching-web-pages"></a>Paso 1: creación de las páginas web de almacenamiento en caché

Antes de comenzar con la exploración de las características de almacenamiento en caché de ObjectDataSource, vamos a crear las páginas de ASP.NET en nuestro proyecto de sitio web que necesitamos para este tutorial y los tres siguientes. Comience agregando una nueva carpeta denominada `Caching`. A continuación, agregue las siguientes páginas de ASP.NET a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `ObjectDataSource.aspx`
- `FromTheArchitecture.aspx`
- `AtApplicationStartup.aspx`
- `SqlCacheDependencies.aspx`

![Agregar las páginas ASP.NET para los tutoriales relacionados con el almacenamiento en caché](caching-data-with-the-objectdatasource-vb/_static/image1.png)

**Figura 1**: incorporación de las páginas de ASP.net para los tutoriales relacionados con el almacenamiento en caché

Al igual que en las otras carpetas, `Default.aspx` en la carpeta `Caching` mostrará los tutoriales en su sección. Recuerde que el control de usuario `SectionLevelTutorialListing.ascx` proporciona esta funcionalidad. Por lo tanto, agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta la página s Vista de diseño.

[![ilustración 2: agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](caching-data-with-the-objectdatasource-vb/_static/image3.png)](caching-data-with-the-objectdatasource-vb/_static/image2.png)

**Figura 2**: Figura 2: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](caching-data-with-the-objectdatasource-vb/_static/image4.png))

Por último, agregue estas páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después del `<siteMapNode>`de trabajo con datos binarios:

[!code-xml[Main](caching-data-with-the-objectdatasource-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para los tutoriales de almacenamiento en caché.

![El mapa del sitio ahora incluye entradas para los tutoriales de almacenamiento en caché](caching-data-with-the-objectdatasource-vb/_static/image5.png)

**Figura 3**: ahora, el mapa del sitio incluye entradas para los tutoriales de almacenamiento en caché

## <a name="step-2-displaying-a-list-of-products-in-a-web-page"></a>Paso 2: mostrar una lista de productos en una página web

En este tutorial se explora el uso de las características de almacenamiento en caché integradas de control ObjectDataSource. Sin embargo, antes de que podamos ver estas características, primero necesitamos una página de la que trabajar. Permita crear una página web que use GridView para mostrar la información de producto recuperada por un ObjectDataSource de la clase `ProductsBLL`.

Para empezar, abra la página `ObjectDataSource.aspx` de la carpeta `Caching`. Arrastre un control GridView desde el cuadro de herramientas hasta el diseñador, establezca su propiedad `ID` en `Products`y, desde su etiqueta inteligente, elija enlazarlo a un nuevo control ObjectDataSource denominado `ProductsDataSource`. Configure ObjectDataSource para trabajar con la clase `ProductsBLL`.

[![configurar ObjectDataSource para usar la clase ProductsBLL](caching-data-with-the-objectdatasource-vb/_static/image7.png)](caching-data-with-the-objectdatasource-vb/_static/image6.png)

**Figura 4**: configuración de ObjectDataSource para usar la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](caching-data-with-the-objectdatasource-vb/_static/image8.png))

En esta página, vamos a crear un GridView modificable para que podamos examinar lo que ocurre cuando se modifican los datos almacenados en caché en ObjectDataSource a través de la interfaz de GridView. Deje la lista desplegable de la pestaña seleccionar establecida en su valor predeterminado, `GetProducts()`, pero cambie el elemento seleccionado en la pestaña actualizar a la `UpdateProduct` sobrecarga que acepte `productName`, `unitPrice`y `productID` como sus parámetros de entrada.

[![establecer la lista desplegable actualizar pestaña s en la sobrecarga UpdateProduct adecuada](caching-data-with-the-objectdatasource-vb/_static/image10.png)](caching-data-with-the-objectdatasource-vb/_static/image9.png)

**Figura 5**: establezca la lista desplegable actualizar pestaña s en la sobrecarga de `UpdateProduct` adecuada ([haga clic para ver la imagen a tamaño completo](caching-data-with-the-objectdatasource-vb/_static/image11.png))

Por último, establezca las listas desplegables de las pestañas insertar y eliminar en (ninguna) y haga clic en finalizar. Tras completar el Asistente para configurar orígenes de datos, Visual Studio establece la propiedad ObjectDataSource s `OldValuesParameterFormatString` en `original_{0}`. Como se describe en el tutorial [información general sobre la inserción, actualización y eliminación de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , esta propiedad debe quitarse de la sintaxis declarativa o volver a establecerse en su valor predeterminado, `{0}`, para que el flujo de trabajo de actualización continúe sin errores.

Además, al finalizar el asistente, Visual Studio agrega un campo a GridView para cada uno de los campos de datos del producto. Quite todos los `ProductName`, `CategoryName`y `UnitPrice` BoundFields. A continuación, actualice las propiedades de `HeaderText` de cada uno de estos BoundFields con productos, categorías y precios, respectivamente. Dado que el campo `ProductName` es obligatorio, convierta el BoundField en TemplateField y agregue un RequiredFieldValidator al `EditItemTemplate`. Del mismo modo, convierta el `UnitPrice` BoundField en TemplateField y agregue un CompareValidator para asegurarse de que el valor especificado por el usuario es un valor de moneda válido que es mayor o igual que cero. Además de estas modificaciones, no dude en realizar cualquier cambio estético, como alinear a la derecha el valor de `UnitPrice`, o especificar el formato del texto de la `UnitPrice` en sus interfaces de solo lectura y edición.

Haga que el control GridView sea editable activando la casilla habilitar edición en la etiqueta inteligente de GridView s. Active también las casillas habilitar paginación y habilitar ordenación.

> [!NOTE]
> ¿Necesita una revisión de cómo personalizar la interfaz de edición de GridView? Si es así, consulte el tutorial [Personalización de la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) .

[![habilitar la compatibilidad con GridView para editar, ordenar y paginar](caching-data-with-the-objectdatasource-vb/_static/image13.png)](caching-data-with-the-objectdatasource-vb/_static/image12.png)

**Ilustración 6**: habilitar la compatibilidad con GridView para editar, ordenar y paginar ([haga clic para ver la imagen de tamaño completo](caching-data-with-the-objectdatasource-vb/_static/image14.png))

Después de realizar estas modificaciones de GridView, el marcado declarativo de GridView y ObjectDataSource s debe ser similar al siguiente:

[!code-aspx[Main](caching-data-with-the-objectdatasource-vb/samples/sample2.aspx)]

Como se muestra en la figura 7, el control GridView modificable muestra el nombre, la categoría y el precio de cada uno de los productos de la base de datos. Dedique un momento a probar la funcionalidad de la página para ordenar los resultados, paginarlos y editar un registro.

[![el nombre, la categoría y el precio de cada producto se muestran en un control GridView que se puede ordenar y paginable.](caching-data-with-the-objectdatasource-vb/_static/image16.png)](caching-data-with-the-objectdatasource-vb/_static/image15.png)

**Figura 7**: cada nombre de producto, categoría y precio se muestran en un control GridView que se puede ordenar, paginable y editable ([haga clic para ver la imagen de tamaño completo](caching-data-with-the-objectdatasource-vb/_static/image17.png))

## <a name="step-3-examining-when-the-objectdatasource-is-requesting-data"></a>Paso 3: examinar Cuándo ObjectDataSource solicita datos

El `Products` GridView recupera los datos que se van a mostrar invocando el método `Select` de la `ProductsDataSource` ObjectDataSource. Este origen de datos crea una instancia de la clase `ProductsBLL` de la capa de lógica de negocios y llama a su método `GetProducts()`, que a su vez llama a la capa de acceso a datos s `ProductsTableAdapter` s `GetProducts()` método. El método DAL se conecta a la base de datos Northwind y emite la consulta de `SELECT` configurada. Después, estos datos se devuelven a la capa DAL, que los empaqueta en un `NorthwindDataTable`. El objeto DataTable se devuelve a la capa BLL, que lo devuelve a ObjectDataSource, que lo devuelve a GridView. A continuación, GridView crea un objeto de `GridViewRow` para cada `DataRow` de la DataTable, y cada `GridViewRow` se representa finalmente en el código HTML que se devuelve al cliente y se muestra en el explorador del visitante.

Esta secuencia de eventos se produce cada vez que GridView debe enlazarse a sus datos subyacentes. Esto sucede cuando se visita la página por primera vez, cuando se mueve de una página de datos a otra, cuando se ordena GridView o cuando se modifican los datos de GridView a través de las interfaces de edición o eliminación integradas. Si el estado de vista de GridView es deshabilitado, el control GridView se volverá a enlazar también en cada postback. GridView también se puede reenlazar explícitamente a sus datos llamando a su método `DataBind()`.

Para apreciar totalmente la frecuencia con la que se recuperan los datos de la base de datos, deje que se muestre un mensaje que indique cuándo se vuelven a recuperar los datos. Agregue un control Web de etiqueta encima de GridView denominado `ODSEvents`. Desactive su propiedad `Text` y establezca su propiedad `EnableViewState` en `False`. Debajo de la etiqueta, agregue un control Web Button y establezca su propiedad `Text` en postback.

[![agregar una etiqueta y un botón a la página sobre GridView](caching-data-with-the-objectdatasource-vb/_static/image19.png)](caching-data-with-the-objectdatasource-vb/_static/image18.png)

**Figura 8**: agregar una etiqueta y un botón a la página sobre GridView ([haga clic para ver la imagen de tamaño completo](caching-data-with-the-objectdatasource-vb/_static/image20.png))

Durante el flujo de trabajo de acceso a datos, el evento ObjectDataSource s `Selecting` se desencadena antes de que se cree el objeto subyacente y se invoque su método configurado. Cree un controlador de eventos para este evento y agregue el código siguiente:

[!code-vb[Main](caching-data-with-the-objectdatasource-vb/samples/sample3.vb)]

Cada vez que el origen ObjectDataSource realiza una solicitud a la arquitectura de los datos, la etiqueta mostrará el texto seleccionado evento desencadenado.

Visite esta página en un explorador. Cuando se visita la página por primera vez, se muestra el texto selección de eventos desencadenados. Haga clic en el botón postback y observe que el texto desaparece (suponiendo que la propiedad GridView s `EnableViewState` esté establecida en `True`, el valor predeterminado). Esto se debe a que, en el postback, GridView se reconstruye a partir de su estado de vista y, por tanto, no se convierte en el origen de datos. La ordenación, la paginación o la edición de los datos, sin embargo, hace que el control GridView se vuelva a enlazar a su origen de datos y, por tanto, vuelve a aparecer el texto desencadenador de selección.

[![cada vez que se reenlaza GridView a su origen de datos, se muestra la selección de eventos desencadenados.](caching-data-with-the-objectdatasource-vb/_static/image22.png)](caching-data-with-the-objectdatasource-vb/_static/image21.png)

**Figura 9**: cada vez que se reenlaza el control GridView a su origen de datos, se muestra la selección de eventos desencadenados ([haga clic para ver la imagen de tamaño completo](caching-data-with-the-objectdatasource-vb/_static/image23.png))

[![hacer clic en el botón de postback hace que GridView se reconstruya a partir de su estado de vista](caching-data-with-the-objectdatasource-vb/_static/image25.png)](caching-data-with-the-objectdatasource-vb/_static/image24.png)

**Figura 10**: hacer clic en el botón de postback hace que GridView se reconstruya a partir de su estado de vista ([haga clic para ver la imagen de tamaño completo](caching-data-with-the-objectdatasource-vb/_static/image26.png))

Puede parecer que no se recuperan los datos de la base de datos cada vez que se paginan o se ordenan los datos. Después de todo, puesto que se vuelve a usar la paginación predeterminada, ObjectDataSource ha recuperado todos los registros al mostrar la primera página. Aunque GridView no proporcione compatibilidad de ordenación y paginación, los datos deben recuperarse de la base de datos cada vez que cualquier usuario visite la página por primera vez (y en cada postback, si el estado de vista está deshabilitado). Pero si GridView muestra los mismos datos a todos los usuarios, estas solicitudes de bases de datos adicionales son superfluas. ¿Por qué no almacenar en caché los resultados devueltos por el método `GetProducts()` y enlazar GridView a esos resultados almacenados en caché?

## <a name="step-4-caching-the-data-using-the-objectdatasource"></a>Paso 4: almacenamiento en caché de los datos mediante ObjectDataSource

Con solo establecer algunas propiedades, ObjectDataSource puede configurarse para almacenar en caché automáticamente los datos recuperados en la memoria caché de datos ASP.NET. En la lista siguiente se resumen las propiedades relacionadas con la memoria caché de ObjectDataSource:

- [EnableCaching](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.enablecaching.aspx) debe establecerse en `True` para habilitar el almacenamiento en caché. De manera predeterminada, es `False`.
- [CacheDuration](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheduration.aspx) la cantidad de tiempo, en segundos, que los datos se almacenan en caché. El valor predeterminado es 0. ObjectDataSource solo almacenará en caché los datos si se `True` `EnableCaching` y `CacheDuration` se establece en un valor mayor que cero.
- [CacheExpirationPolicy](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cacheexpirationpolicy.aspx) se puede establecer en `Absolute` o `Sliding`. Si `Absolute`, ObjectDataSource almacena en memoria caché los datos recuperados durante `CacheDuration` segundos; Si `Sliding`, los datos expiran solo después de que no se haya tenido acceso a ellos durante `CacheDuration` segundos. De manera predeterminada, es `Absolute`.
- [CacheKeyDependency](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.cachekeydependency.aspx) utilice esta propiedad para asociar las entradas de caché de ObjectDataSource con una dependencia de caché existente. Las entradas de datos de ObjectDataSource s se pueden desalojar prematuramente de la memoria caché al expirar la `CacheKeyDependency`asociada. Normalmente, esta propiedad se usa para asociar una dependencia de caché de SQL con la memoria caché de ObjectDataSource, un tema que exploraremos en el futuro con el tutorial sobre las [dependencias de caché de SQL](using-sql-cache-dependencies-vb.md) .

Permita que s configure el `ProductsDataSource` ObjectDataSource para almacenar en caché sus datos durante 30 segundos en una escala absoluta. Establezca la propiedad ObjectDataSource s `EnableCaching` en `True` y su propiedad `CacheDuration` en 30. Deje la propiedad `CacheExpirationPolicy` establecida en su valor predeterminado, `Absolute`.

[![configurar ObjectDataSource para almacenar en caché sus datos durante 30 segundos](caching-data-with-the-objectdatasource-vb/_static/image28.png)](caching-data-with-the-objectdatasource-vb/_static/image27.png)

**Figura 11**: configuración de ObjectDataSource para almacenar en caché los datos durante 30 segundos ([haga clic para ver la imagen de tamaño completo](caching-data-with-the-objectdatasource-vb/_static/image29.png))

Guarde los cambios y vuelva a visitar esta página en un explorador. El texto desencadenado por el evento de selección aparecerá al visitar la página por primera vez, ya que los datos no están en la memoria caché. Pero los postbacks subsiguientes desencadenados al hacer clic en el botón de postback, ordenar, paginar o hacer clic en los botones editar o *Cancelar no vuelven* a mostrar el texto desencadenado por el evento. Esto se debe a que el evento `Selecting` solo se desencadena cuando ObjectDataSource obtiene sus datos de su objeto subyacente; el evento `Selecting` no se activa si los datos se extraen de la caché de datos.

Después de 30 segundos, los datos se expulsarán de la memoria caché. Los datos también se expulsarán de la memoria caché si se invocan los métodos ObjectDataSource `Insert`, `Update`o `Delete`. Por lo tanto, después de que transcurran 30 segundos o se haga clic en el botón actualizar, la ordenación, la paginación o la selección de los botones editar o cancelar harán que ObjectDataSource obtenga sus datos de su objeto subyacente, lo que muestra el texto que se ha desencadenado al desencadenar el evento `Selecting`. Estos resultados devueltos se vuelven a colocar en la memoria caché de datos.

> [!NOTE]
> Si ve el texto que se ha desencadenado con frecuencia, incluso cuando espera que ObjectDataSource trabaje con los datos en caché, puede deberse a restricciones de memoria. Si no hay suficiente memoria libre, es posible que se hayan eliminado los datos agregados a la memoria caché por ObjectDataSource. Si ObjectDataSource no parece tener correctamente el almacenamiento en caché de los datos o solo almacena los datos de forma esporádica, cierre algunas aplicaciones para liberar memoria e inténtelo de nuevo.

En la figura 12 se muestra el flujo de trabajo de almacenamiento en caché de ObjectDataSource. Cuando aparece el texto de selección del evento desencadenado en la pantalla, se debe a que los datos no estaban en la memoria caché y tenían que recuperarse del objeto subyacente. Sin embargo, cuando falta este texto, se debe a que los datos estaban disponibles desde la memoria caché. Cuando los datos se devuelven desde la memoria caché, no hay ninguna llamada al objeto subyacente y, por lo tanto, no se ejecuta ninguna consulta de base de datos.

![ObjectDataSource almacena y recupera sus datos de la memoria caché de datos](caching-data-with-the-objectdatasource-vb/_static/image30.png)

**Figura 12**: el ObjectDataSource almacena y recupera sus datos de la memoria caché de datos

Cada aplicación de ASP.NET tiene su propia instancia de caché de datos que se comparte entre todas las páginas y visitantes. Esto significa que los datos almacenados en la memoria caché de datos por parte de ObjectDataSource también se comparten entre todos los usuarios que visitan la página. Para comprobarlo, abra la página `ObjectDataSource.aspx` en un explorador. Al visitar la página por primera vez, aparecerá el texto desencadenado por el evento seleccionado (suponiendo que los datos agregados a la memoria caché por las pruebas anteriores ya se hayan desalojado). Abra una segunda instancia del explorador y copie y pegue la dirección URL de la primera instancia del explorador en la segunda. En la segunda instancia del explorador, el texto desencadenado para el evento no se muestra porque usa los mismos datos almacenados en caché que el primero.

Al insertar los datos recuperados en la memoria caché, ObjectDataSource usa un valor de clave de caché que incluye: los valores de las propiedades `CacheDuration` y `CacheExpirationPolicy`; el tipo del objeto comercial subyacente utilizado por ObjectDataSource, que se especifica mediante la [propiedad`TypeName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.typename.aspx) (`ProductsBLL`, en este ejemplo); el valor de la propiedad `SelectMethod` y el nombre y los valores de los parámetros de la colección `SelectParameters`; y los valores de sus propiedades `StartRowIndex` y `MaximumRows`, que se usan al implementar la [paginación personalizada](../paging-and-sorting/paging-and-sorting-report-data-vb.md).

La creación del valor de la clave de caché como una combinación de estas propiedades garantiza una entrada de caché única a medida que estos valores cambian. Por ejemplo, en los tutoriales anteriores, hemos examinado el uso de la `ProductsBLL` clase s `GetProductsByCategoryID(categoryID)`, que devuelve todos los productos de una categoría especificada. Un usuario podría aparecer en la página y ver las bebidas, que tiene una `CategoryID` de 1. Si ObjectDataSource almacenó en memoria caché sus resultados sin tener en cuenta los valores de `SelectParameters`, cuando otro usuario llegó a la página para ver los condimentos mientras los productos de bebidas estaban en la memoria caché, estos pueden ver los productos de bebidas en caché en lugar de los condimentos. Al modificar la clave de caché mediante estas propiedades, que incluyen los valores de la `SelectParameters`, ObjectDataSource mantiene una entrada de caché independiente para las bebidas y los condimentos.

## <a name="stale-data-concerns"></a>Problemas de datos obsoletos

ObjectDataSource expulsa automáticamente sus elementos de la memoria caché cuando se invoca uno de sus métodos `Insert`, `Update`o `Delete`. Esto ayuda a protegerse contra datos obsoletos mediante la eliminación de las entradas de caché cuando se modifican los datos a través de la página. Sin embargo, es posible que un ObjectDataSource que use el almacenamiento en caché siga mostrando datos obsoletos. En el caso más simple, puede deberse a que los datos cambian directamente dentro de la base de datos. Quizás un administrador de base de datos acaba de ejecutar un script que modifica algunos de los registros de la base de datos.

Este escenario también podría desdoblarse de una manera más sutil. Mientras que ObjectDataSource expulsa sus elementos de la memoria caché cuando se llama a uno de sus métodos de modificación de datos, los elementos almacenados en memoria caché se quitan para la combinación de valores de propiedades de ObjectDataSource s determinada (`CacheDuration`, `TypeName`, `SelectMethod`, etc.). Si tiene dos ObjectDataSource que usan `SelectMethods` o `SelectParameters`diferentes, pero todavía pueden actualizar los mismos datos, un ObjectDataSource puede actualizar una fila e invalidar sus propias entradas de caché, pero la fila correspondiente del segundo ObjectDataSource se seguirá atendiendo desde la memoria caché. Recomiendo crear páginas para presentar esta funcionalidad. Cree una página que muestre un GridView modificable que extraiga sus datos de un ObjectDataSource que use el almacenamiento en caché y que esté configurado para obtener datos del método de `GetProducts()` de `ProductsBLL` Class s. Agregue otro GridView y ObjectDataSource editable a esta página (u otra), pero para este segundo ObjectDataSource, use el método `GetProductsByCategoryID(categoryID)`. Dado que las dos propiedades de `SelectMethod` de ObjectDataSource difieren, cada una tiene sus propios valores en caché. Si edita un producto en una cuadrícula, la próxima vez que enlace los datos a la otra cuadrícula (mediante la paginación, la ordenación, etc.), seguirá atendiendo a los datos antiguos almacenados en caché y no reflejará el cambio realizado desde la otra cuadrícula.

En Resumen, use solo Expires basados en el tiempo si está dispuesto a tener el potencial de los datos obsoletos y use expiraciones más cortas para los escenarios en los que la actualización de los datos sea importante. Si no se aceptan datos obsoletos, se renuncia al almacenamiento en caché o se utilizan dependencias de caché de SQL (suponiendo que se trata de datos de base de datos que se vuelven a almacenar en caché). Exploraremos las dependencias de caché de SQL en un tutorial futuro.

## <a name="summary"></a>Resumen

En este tutorial se han examinado las capacidades de almacenamiento en caché integradas de ObjectDataSource. Con solo establecer algunas propiedades, podemos indicar a ObjectDataSource que almacene en caché los resultados devueltos desde el `SelectMethod` especificado en la memoria caché de datos ASP.NET. Las propiedades `CacheDuration` y `CacheExpirationPolicy` indican la duración del elemento que se almacena en caché y si es una expiración absoluta o variable. La propiedad `CacheKeyDependency` asocia todas las entradas de caché de ObjectDataSource con una dependencia de caché existente. Se puede usar para desalojar las entradas de ObjectDataSource s de la memoria caché antes de que se alcance la expiración basada en el tiempo y se usa normalmente con las dependencias de caché de SQL.

Dado que ObjectDataSource simplemente almacena en caché sus valores en la memoria caché de datos, podríamos replicar la funcionalidad integrada de ObjectDataSource s mediante programación. No tiene sentido hacerlo en el nivel de presentación, ya que ObjectDataSource ofrece esta funcionalidad de forma integrada, pero se pueden implementar funcionalidades de almacenamiento en caché en una capa independiente de la arquitectura. Para ello, es necesario repetir la misma lógica usada por ObjectDataSource. Exploraremos cómo trabajar con la memoria caché de datos mediante programación en el siguiente tutorial.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Caching de ASP.NET: técnicas y procedimientos recomendados](https://msdn.microsoft.com/library/aa478965.aspx)
- [Guía de arquitectura de almacenamiento en caché para aplicaciones .NET Framework](https://msdn.microsoft.com/library/ee817645.aspx)
- [Almacenamiento en caché de resultados en ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/121306-1.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial era Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-sql-cache-dependencies-cs.md)
> [Siguiente](caching-data-in-the-architecture-vb.md)
