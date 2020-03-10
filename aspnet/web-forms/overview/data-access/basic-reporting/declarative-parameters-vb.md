---
uid: web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
title: Parámetros declarativos (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial se muestra cómo usar un parámetro establecido en un valor codificado de forma rígida para seleccionar los datos que se van a mostrar en un control DetailsView.
ms.author: riande
ms.date: 03/31/2010
ms.assetid: dc1234a3-114f-4c9a-8d25-50ca03cc8e8e
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/declarative-parameters-vb
msc.type: authoredcontent
ms.openlocfilehash: cdc42752fc78d18366af037a81fe4ebe5a1646ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78483133"
---
# <a name="declarative-parameters-vb"></a>Parámetros declarativos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_5_VB.exe) de la aplicación de ejemplo o [descarga de PDF](declarative-parameters-vb/_static/datatutorial05vb1.pdf)

> En este tutorial se muestra cómo usar un parámetro establecido en un valor codificado de forma rígida para seleccionar los datos que se van a mostrar en un control DetailsView.

## <a name="introduction"></a>Introducción

En el [último tutorial](displaying-data-with-the-objectdatasource-vb.md) , hemos examinado la visualización de datos con los controles GridView, DetailsView y FormView enlazados a un control ObjectDataSource que invocó el método `GetProducts()` desde la clase `ProductsBLL`. El método `GetProducts()` devuelve un DataTable fuertemente tipado rellenado con todos los registros de la tabla de `Products` de la base de datos Northwind. La clase `ProductsBLL` contiene métodos adicionales para devolver solo subconjuntos de los productos: `GetProductByProductID(productID)`, `GetProductsByCategoryID(categoryID)`y `GetProductsBySupplierID(supplierID)`. Estos tres métodos esperan un parámetro de entrada que indica cómo filtrar la información de producto devuelta.

ObjectDataSource se puede usar para invocar métodos que esperan parámetros de entrada, pero para ello debemos especificar de dónde proceden los valores de estos parámetros. Los valores de los parámetros pueden estar codificados de forma rígida o pueden provienen de diversos orígenes dinámicos, entre los que se incluyen valores QueryString, variables de sesión, el valor de propiedad de un control Web en la página u otros.

En este tutorial, vamos a empezar por ilustrar cómo usar un parámetro establecido en un valor codificado de forma rígida. En concreto, veremos cómo agregar un DetailsView a la página que muestra información sobre un producto específico, es decir, la combinación de gumbo de chef Anton, que tiene un `ProductID` de 5. A continuación, veremos cómo establecer el valor del parámetro basándose en un control Web. En concreto, usaremos un cuadro de texto para permitir que el usuario escriba un país, después del cual puede hacer clic en un botón para ver la lista de proveedores que residen en ese país.

## <a name="using-a-hard-coded-parameter-value"></a>Usar un valor de parámetro codificado de forma rígida

En el primer ejemplo, empiece agregando un control DetailsView a la página `DeclarativeParams.aspx` de la carpeta `BasicReporting`. En la etiqueta inteligente del DetailsView, seleccione &lt;nuevo origen de datos&gt; en la lista desplegable y elija Agregar un ObjectDataSource.

[![agregar un ObjectDataSource a la página](declarative-parameters-vb/_static/image2.png)](declarative-parameters-vb/_static/image1.png)

**Figura 1**: agregar un ObjectDataSource a la página ([haga clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image3.png))

Se iniciará automáticamente el Asistente para elegir el origen de datos del control ObjectDataSource. Seleccione la clase `ProductsBLL` de la primera pantalla del asistente.

[![seleccionar la clase ProductsBLL](declarative-parameters-vb/_static/image5.png)](declarative-parameters-vb/_static/image4.png)

**Figura 2**: seleccione la clase `ProductsBLL` ([haga clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image6.png))

Como queremos mostrar información acerca de un producto determinado, queremos usar el método `GetProductByProductID(productID)`.

[![elegir el método GetProductByProductID (productID)](declarative-parameters-vb/_static/image8.png)](declarative-parameters-vb/_static/image7.png)

**Figura 3**: elegir el método de `GetProductByProductID(productID)` ([hacer clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image9.png))

Dado que el método seleccionado incluye un parámetro, hay una pantalla más para el asistente, en la que se le pide que defina el valor que se va a usar para el parámetro. La lista de la izquierda muestra todos los parámetros para el método seleccionado. Por `GetProductByProductID(productID)` solo hay un `productID`. A la derecha podemos especificar el valor del parámetro seleccionado. La lista desplegable origen del parámetro enumera los distintos orígenes posibles para el valor del parámetro. Dado que queremos especificar un valor codificado de forma rígida de 5 para el parámetro `productID`, deje el origen del parámetro como ninguno y escriba 5 en el cuadro de texto DefaultValue.

[![se usará un parámetro codificado de forma rígida de 5 para el parámetro productID](declarative-parameters-vb/_static/image11.png)](declarative-parameters-vb/_static/image10.png)

**Figura 4**: se utilizará un valor de parámetro codificado de forma rígida de 5 para el parámetro `productID` ([haga clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image12.png))

Después de completar el Asistente para configurar origen de datos, el marcado declarativo del control ObjectDataSource incluye un objeto `Parameter` en la colección `SelectParameters` para cada uno de los parámetros de entrada esperados por el método definido en la propiedad `SelectMethod`. Dado que el método que usamos en este ejemplo espera solo un parámetro de entrada único, `parameterID`, aquí solo hay una entrada. La colección de `SelectParameters` puede contener cualquier clase que derive de la clase `Parameter` en el espacio de nombres `System.Web.UI.WebControls`. En el caso de los valores de parámetros codificados de forma rígida, se usa la clase base `Parameter`, pero para las demás opciones de origen de parámetro se usa una clase de `Parameter` derivada; también puede crear sus propios [tipos de parámetro personalizados](http://www.leftslipper.com/ShowFaq.aspx?FaqId=11), si es necesario.

[!code-aspx[Main](declarative-parameters-vb/samples/sample1.aspx)]

> [!NOTE]
> Si está siguiendo su propio equipo, el marcado declarativo que ve en este punto puede incluir valores para las propiedades `InsertMethod`, `UpdateMethod`y `DeleteMethod`, así como `DeleteParameters`. El Asistente para elegir origen de datos de ObjectDataSource especifica automáticamente los métodos del `ProductBLL` que se van a usar para insertar, actualizar y eliminar, por lo que, a menos que los borre explícitamente, se incluirán en el marcado anterior.

Al visitar esta página, el control Web de datos invocará el método `Select` de ObjectDataSource, que llamará al método `GetProductByProductID(productID)` de la clase `ProductsBLL` utilizando el valor codificado de forma rígida de 5 para el parámetro de entrada `productID`. El método devolverá un objeto `ProductDataTable` fuertemente tipado que contiene una sola fila con información sobre la combinación de gumbo de chef Anton (el producto con `ProductID` 5).

[![se muestra información sobre la combinación de gumbo de chef Anton](declarative-parameters-vb/_static/image14.png)](declarative-parameters-vb/_static/image13.png)

**Figura 5**: se muestra información sobre la combinación de gumbo de chef Anton ([haga clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image15.png))

## <a name="setting-the-parameter-value-to-the-property-value-of-a-web-control"></a>Establecer el valor del parámetro en el valor de propiedad de un control Web

Los valores de parámetro de ObjectDataSource también se pueden establecer en función del valor de un control Web en la página. Para ilustrar esto, vamos a tener un control GridView que enumera todos los proveedores que se encuentran en un país especificado por el usuario. Para ello, agregue un cuadro de texto a la página en la que el usuario puede escribir un nombre de país. Establezca la propiedad `ID` de este control de cuadro de texto en `CountryName`. Agregue también un control Web de botón.

[![agregar un cuadro de texto a la página con el identificador CountryName](declarative-parameters-vb/_static/image17.png)](declarative-parameters-vb/_static/image16.png)

**Figura 6**: agregar un cuadro de texto a la página con `ID` `CountryName` ([haga clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image18.png))

Después, agregue un control GridView a la página y, en la etiqueta inteligente, elija Agregar un nuevo ObjectDataSource. Puesto que queremos mostrar información de proveedor, seleccione la clase `SuppliersBLL` de la primera pantalla del asistente. En la segunda pantalla, seleccione el método `GetSuppliersByCountry(country)`.

[![elegir el método GetSuppliersByCountry (Country)](declarative-parameters-vb/_static/image20.png)](declarative-parameters-vb/_static/image19.png)

**Figura 7**: elija el método `GetSuppliersByCountry(country)` ([haga clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image21.png))

Dado que el método `GetSuppliersByCountry(country)` tiene un parámetro de entrada, el asistente incluye una vez más la pantalla final para elegir el valor del parámetro. Esta vez, establezca el origen del parámetro en control. Esto rellenará la lista desplegable ControlID con los nombres de los controles de la página. Seleccione el control `CountryName` de la lista. Cuando se visite la página por primera vez, el cuadro de texto `CountryName` estará en blanco, por lo que no se devolverá ningún resultado y no se mostrará nada. Si desea mostrar algunos resultados de forma predeterminada, establezca el cuadro de texto DefaultValue en consecuencia.

[![establecer el valor del parámetro en el valor del control CountryName](declarative-parameters-vb/_static/image23.png)](declarative-parameters-vb/_static/image22.png)

**Figura 8**: establezca el valor del parámetro en el valor del control `CountryName` ([haga clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image24.png))

El marcado declarativo de ObjectDataSource difiere ligeramente del primer ejemplo, mediante el uso de [ControlParameter](https://msdn.microsoft.com/library/system.web.ui.webcontrols.controlparameter.aspx) en lugar del objeto de `Parameter` estándar. Una `ControlParameter` tiene propiedades adicionales para especificar el `ID` del control Web y el valor de propiedad que se va a usar para el parámetro (`PropertyName`). El Asistente para configurar el origen de datos era lo suficientemente inteligente como para determinar que, para un cuadro de texto, probablemente querrámos usar la propiedad `Text` para el valor del parámetro. Sin embargo, si desea utilizar un valor de propiedad diferente del control Web, puede cambiar el valor de `PropertyName` aquí o haciendo clic en el vínculo "Mostrar propiedades avanzadas" en el asistente.

[!code-aspx[Main](declarative-parameters-vb/samples/sample2.aspx)]

Al visitar la página por primera vez, el cuadro de texto `CountryName` está vacío. El control GridView sigue invocando el método `Select` de ObjectDataSource, pero se pasa un valor de `Nothing` al método `GetSuppliersByCountry(country)`. El TableAdapter convierte el `Nothing` en un valor de `NULL` de base de datos (`DBNull.Value`), pero la consulta utilizada por el método `GetSuppliersByCountry(country)` se escribe de forma que no devuelve ningún valor cuando se especifica un valor `NULL` para el parámetro `@CategoryID`. En Resumen, no se devuelve ningún proveedor.

Sin embargo, cuando el visitante escribe en un país y hace clic en el botón Mostrar proveedores para producir un postback, se vuelve a consultar el método de `Select` de ObjectDataSource, pasando el valor de `Text` del control de cuadro de texto como el parámetro `country`.

[![se muestran los proveedores de Canadá](declarative-parameters-vb/_static/image26.png)](declarative-parameters-vb/_static/image25.png)

**Figura 9**: se muestran los proveedores de Canadá ([haga clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image27.png))

## <a name="showing-all-suppliers-by-default"></a>Mostrar todos los proveedores de forma predeterminada

En lugar de mostrar ninguno de los proveedores al ver la página por primera vez, es posible que desee mostrar *todos los* proveedores al principio, lo que permite al usuario reducir la lista escribiendo un nombre de país en el cuadro de texto. Cuando el cuadro de texto está vacío, el método de `GetSuppliersByCountry(country)` de la clase `SuppliersBLL` se pasa `Nothing` para su *`country`* parámetro de entrada. Este valor `Nothing` se pasa entonces al método `GetSupplierByCountry(country)` de la capa DAL, donde se convierte en un valor `NULL` de la base de datos para el parámetro `@Country` en la siguiente consulta:

[!code-sql[Main](declarative-parameters-vb/samples/sample3.sql)]

La expresión `Country = NULL` siempre devuelve false, incluso para los registros cuya columna `Country` tenga un valor `NULL`; por lo tanto, no se devuelve ningún registro.

Para devolver *todos los* proveedores cuando el cuadro de texto Country está vacío, podemos aumentar el método `GetSuppliersByCountry(country)` en BLL para invocar el método `GetSuppliers()` cuando se `Nothing` su parámetro Country y llamar al método `GetSuppliersByCountry(country)` de la capa Dal en caso contrario. Esto tendrá el efecto de devolver todos los proveedores cuando no se especifica ningún país y el subconjunto de proveedores adecuado cuando se incluye el parámetro Country.

Cambie el método `GetSuppliersByCountry(country)` de la clase `SuppliersBLL` por lo siguiente:

[!code-vb[Main](declarative-parameters-vb/samples/sample4.vb)]

Con este cambio, en la página `DeclarativeParams.aspx` se muestran todos los proveedores cuando se visitan por primera vez (o cada vez que el cuadro de texto `CountryName` está vacío).

[![todos los proveedores se muestran ahora de forma predeterminada](declarative-parameters-vb/_static/image29.png)](declarative-parameters-vb/_static/image28.png)

**Figura 10**: todos los proveedores se muestran ahora de forma predeterminada ([haga clic para ver la imagen de tamaño completo](declarative-parameters-vb/_static/image30.png))

## <a name="summary"></a>Resumen

Para usar métodos con parámetros de entrada, es necesario especificar los valores de los parámetros en la colección de `SelectParameters` de ObjectDataSource. Los diferentes tipos de parámetros permiten obtener el valor del parámetro de orígenes diferentes. El tipo de parámetro predeterminado usa un valor codificado de forma rígida, pero con la misma facilidad (y sin una línea de código), los valores de parámetro se pueden obtener de la cadena QueryString, las variables de sesión, las cookies e incluso los valores especificados por el usuario de los controles Web en la página.

En los ejemplos que hemos visto en este tutorial se ha mostrado cómo usar valores de parámetros declarativos. Sin embargo, puede haber ocasiones en las que sea necesario usar un origen de parámetro que no esté disponible, como la fecha y la hora actuales, o bien, si el sitio usa la pertenencia, el ID. de usuario del visitante. En estos escenarios, podemos establecer los valores de parámetro mediante programación antes de que ObjectDataSource invoque el método de su objeto subyacente. Veremos cómo hacerlo en el [siguiente tutorial](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md).

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-data-with-the-objectdatasource-vb.md)
> [Siguiente](programmatically-setting-the-objectdatasource-s-parameter-values-vb.md)
