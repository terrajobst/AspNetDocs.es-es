---
uid: web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
title: Mostrar datos binarios en los controles Web de datos (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos las opciones para presentar datos binarios en una página web, incluida la presentación de un archivo de imagen y el aprovisionamiento de un vínculo de descarga.
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 9201656a-e1c2-4020-824b-18fb632d2925
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/displaying-binary-data-in-the-data-web-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 27c901af092aa990f557750dc5d2c42ba2644c02
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78489103"
---
# <a name="displaying-binary-data-in-the-data-web-controls-vb"></a>Mostrar datos binarios en los controles web de datos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_55_VB.exe) de la aplicación de ejemplo o [descarga de PDF](displaying-binary-data-in-the-data-web-controls-vb/_static/datatutorial55vb1.pdf)

> En este tutorial, veremos las opciones para presentar datos binarios en una página web, incluida la presentación de un archivo de imagen y el aprovisionamiento de un vínculo de descarga para un archivo PDF.

## <a name="introduction"></a>Introducción

En el tutorial anterior exploramos las dos técnicas para asociar datos binarios con un modelo de datos subyacente de la aplicación y usaban el control FileUpload para cargar archivos desde un explorador al sistema de archivos del servidor Web. Todavía veremos cómo asociar los datos binarios cargados con el modelo de datos. Es decir, una vez que un archivo se ha cargado y guardado en el sistema de archivos, se debe almacenar una ruta de acceso al archivo en el registro de base de datos correspondiente. Si los datos se almacenan directamente en la base de datos, los datos binarios cargados no se deben guardar en el sistema de archivos, sino que deben insertarse en la base de datos.

Sin embargo, antes de consultar cómo asociar los datos con el modelo de datos, vamos a examinar primero cómo proporcionar los datos binarios al usuario final. Presentar los datos de texto es lo bastante sencillo, pero ¿cómo se deben presentar los datos binarios? Por supuesto, depende del tipo de datos binarios. En el caso de las imágenes, es probable que desee mostrar la imagen. en el caso de PDF, documentos de Microsoft Word, archivos ZIP y otros tipos de datos binarios, es probable que sea más apropiado proporcionar un vínculo de descarga.

En este tutorial, veremos cómo presentar los datos binarios junto con sus datos de texto asociados mediante controles Web de datos como GridView y DetailsView. En el siguiente tutorial se va a activar la atención de la Asociación de un archivo cargado con la base de datos.

## <a name="step-1-providingbrochurepathvalues"></a>Paso 1: proporcionar valores de`BrochurePath`

La columna de `Picture` de la tabla de `Categories` ya contiene datos binarios para las distintas imágenes de categoría. En concreto, la columna de `Picture` para cada registro contiene el contenido binario de una imagen de mapa de bits de 16 colores de baja calidad y grano. Cada imagen de categoría tiene 172 píxeles de ancho y 120 píxeles de alto y consume aproximadamente 11 KB. Lo que es más, el contenido binario de la columna `Picture` incluye un encabezado [OLE](http://en.wikipedia.org/wiki/Object_Linking_and_Embedding) de 78 bytes que se debe quitar antes de mostrar la imagen. Esta información de encabezado está presente porque la base de datos Northwind tiene sus raíces en Microsoft Access. En Access, los datos binarios se almacenan con el tipo de datos del objeto OLE, que se recorta en este encabezado. Por ahora, veremos cómo quitar los encabezados de estas imágenes de baja calidad para mostrar la imagen. En un futuro tutorial, se creará una interfaz para actualizar una columna de `Picture` categoría y se reemplazarán estas imágenes de mapa de bits que usan encabezados OLE con imágenes JPG equivalentes sin los encabezados OLE innecesarios.

En el tutorial anterior, vimos cómo usar el control FileUpload. Por lo tanto, puede continuar y agregar archivos de folletos al sistema de archivos del servidor Web. De este modo, sin embargo, no actualiza la columna `BrochurePath` en la tabla `Categories`. En el siguiente tutorial veremos cómo hacerlo, pero por ahora tenemos que proporcionar manualmente valores para esta columna.

En esta descarga del tutorial, encontrará siete archivos de folletos PDF en la carpeta `~/Brochures`, uno para cada una de las categorías excepto el marisco. He omitido de forma intencionada agregar un folleto de marisco para ilustrar cómo controlar escenarios en los que no todos los registros tienen datos binarios asociados. Para actualizar la tabla `Categories` con estos valores, haga clic con el botón secundario en el nodo `Categories` de Explorador de servidores y elija Mostrar datos de tabla. A continuación, escriba las rutas de acceso virtual a los archivos del folleto para cada categoría que tenga un folleto, como se muestra en la figura 1. Puesto que no hay ningún folleto para la categoría mariscos, deje el valor `BrochurePath` columna s como `NULL`.

[![especificar manualmente los valores de la columna Categories Table s BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image1.png)

**Figura 1**: especifique manualmente los valores de la columna `Categories` Table s `BrochurePath` ([haga clic para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.png))

## <a name="step-2-providing-a-download-link-for-the-brochures-in-a-gridview"></a>Paso 2: proporcionar un vínculo de descarga para los folletos en un control GridView

Con los valores `BrochurePath` proporcionados para la tabla `Categories`, se vuelve a preparar para crear un control GridView que muestre cada categoría junto con un vínculo para descargar el folleto categoría s. En el paso 4 extenderemos este GridView para mostrar también la imagen de la categoría.

Empiece arrastrando un control GridView desde el cuadro de herramientas hasta el diseñador de la página `DisplayOrDownloadData.aspx` en la carpeta `BinaryData`. Establezca el `ID` de GridView en `Categories` y a través de la etiqueta inteligente de GridView s, elija enlazarlo a un nuevo origen de datos. En concreto, enlácelo a un ObjectDataSource denominado `CategoriesDataSource` que recupere los datos mediante el método `CategoriesBLL` Object s `GetCategories()`.

[![crear un nuevo ObjectDataSource denominado CategoriesDataSource](displaying-binary-data-in-the-data-web-controls-vb/_static/image2.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.png)

**Figura 2**: creación de un nuevo ObjectDataSource denominado `CategoriesDataSource` ([haga clic para ver la imagen de tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.png))

[![configurar ObjectDataSource para usar la clase CategoriesBLL](displaying-binary-data-in-the-data-web-controls-vb/_static/image3.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.png)

**Figura 3**: configuración de ObjectDataSource para usar la clase `CategoriesBLL` ([haga clic para ver la imagen de tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.png))

[![recuperar la lista de categorías mediante el método GetCategories ()](displaying-binary-data-in-the-data-web-controls-vb/_static/image4.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.png)

**Figura 4**: recuperación de la lista de categorías con el método `GetCategories()` ([haga clic para ver la imagen de tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.png))

Después de completar el Asistente para configurar orígenes de datos, Visual Studio agregará automáticamente un BoundField al `Categories` GridView para los `CategoryID`, `CategoryName`, `Description`, `NumberOfProducts`y `BrochurePath` `DataColumn` s. Continúe y quite el `NumberOfProducts` BoundField, ya que la consulta `GetCategories()` método s no recupera esta información. Quite también el `CategoryID` BoundField y cambie el nombre de las propiedades `CategoryName` y `BrochurePath` BoundFields `HeaderText` a categoría y folleto, respectivamente. Después de realizar estos cambios, el marcado declarativo de GridView y ObjectDataSource s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample1.aspx)]

Vea esta página a través de un explorador (consulte la figura 5). Se muestra cada una de las ocho categorías. Las siete categorías con valores `BrochurePath` tienen el valor `BrochurePath` que se muestra en el BoundField respectivo. Mariscos, que tiene un valor `NULL` para su `BrochurePath`, muestra una celda vacía.

[![se muestra el nombre, la descripción y el valor BrochurePath de cada categoría.](displaying-binary-data-in-the-data-web-controls-vb/_static/image5.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.png)

**Figura 5**: se enumeran los valores de nombre, descripción y `BrochurePath` de cada categoría ([haga clic para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.png))

En lugar de mostrar el texto de la columna `BrochurePath`, queremos crear un vínculo al folleto. Para ello, quite el `BrochurePath` BoundField y reemplácelo por un HyperLinkField. Establezca la propiedad New HyperLinkField s `HeaderText` en folleto, su propiedad `Text` para ver el folleto y su propiedad `DataNavigateUrlFields` en `BrochurePath`.

![Agregar un HyperLinkField para BrochurePath](displaying-binary-data-in-the-data-web-controls-vb/_static/image6.gif)

**Ilustración 6**: agregar un HyperLinkField para `BrochurePath`

Esto agregará una columna de vínculos a GridView, tal como se muestra en la figura 7. Al hacer clic en un vínculo ver folleto, se mostrará el PDF directamente en el explorador o se solicitará al usuario que descargue el archivo, en función de si está instalado un lector PDF y de la configuración del explorador.

[para ver ![un folleto de categoría, haga clic en el vínculo ver folleto](displaying-binary-data-in-the-data-web-controls-vb/_static/image7.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.png)

**Figura 7**: un folleto de categoría se puede ver haciendo clic en el vínculo ver folleto ([haga clic para ver la imagen a tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.png))

[![se muestra el PDF del folleto de categorías.](displaying-binary-data-in-the-data-web-controls-vb/_static/image8.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.png)

**Figura 8**: se muestra el pdf del folleto de categorías ([haga clic para ver la imagen de tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image14.png))

## <a name="hiding-the-view-brochure-text-for-categories-without-a-brochure"></a>Ocultar el texto del folleto de la vista para las categorías sin un folleto

Como se muestra en la figura 7, en el `BrochurePath` HyperLinkField se muestra el valor de la propiedad `Text` (folleto de vista) de todos los registros, independientemente de si se trata de un valor no`NULL` para `BrochurePath`. Por supuesto, si `BrochurePath` es `NULL`, el vínculo se muestra como solo texto, como es el caso de la categoría mariscos (consulte la figura 7). En lugar de mostrar el folleto de la vista de texto, es posible que las categorías sin un valor `BrochurePath` muestren texto alternativo, como no hay ningún Folleto disponible.

Con el fin de proporcionar este comportamiento, es necesario usar TemplateField cuyo contenido se genera mediante una llamada a un método de página que emite el resultado adecuado en función del valor de `BrochurePath`. En primer lugar, hemos explorado esta técnica de formato en el tutorial [uso de TemplateFields en el control GridView](../custom-formatting/using-templatefields-in-the-gridview-control-vb.md) .

Convierta HyperLinkField en TemplateField; para ello, seleccione la `BrochurePath` HyperLinkField y, a continuación, haga clic en el vínculo convertir este campo en un TemplateField en el cuadro de diálogo Editar columnas.

![Convertir HyperLinkField en TemplateField](displaying-binary-data-in-the-data-web-controls-vb/_static/image9.gif)

**Figura 9**: conversión de HyperLinkField en TemplateField

Esto creará TemplateField con una `ItemTemplate` que contiene un control Web de hipervínculo cuya propiedad `NavigateUrl` está enlazada al valor `BrochurePath`. Reemplace este marcado por una llamada al método `GenerateBrochureLink`, pasando el valor de `BrochurePath`:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample2.aspx)]

A continuación, cree un método `Protected` en la clase de código subyacente de la página ASP.NET denominada `GenerateBrochureLink` que devuelva un `String` y acepte un `Object` como parámetro de entrada.

[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample3.vb)]

Este método determina si el valor de `Object` pasado es una `NULL` de base de datos y, en ese caso, devuelve un mensaje que indica que la categoría carece de un folleto. De lo contrario, si hay un valor `BrochurePath`, se muestra en un hipervínculo. Tenga en cuenta que si el valor `BrochurePath` está presente, se pasa al [método`ResolveUrl(url)`](https://msdn.microsoft.com/library/system.web.ui.control.resolveurl.aspx). Este método resuelve la *dirección URL*pasada, reemplazando el carácter `~` por la ruta de acceso virtual adecuada. Por ejemplo, si la aplicación se basa en `/Tutorial55`, `ResolveUrl("~/Brochures/Meats.pdf")` devolverá `/Tutorial55/Brochures/Meat.pdf`.

En la ilustración 10 se muestra la página después de aplicar estos cambios. Tenga en cuenta que el campo mariscos categoría s `BrochurePath` muestra el texto sin folleto.

[![el texto no hay ningún Folleto disponible para esas categorías sin un folleto](displaying-binary-data-in-the-data-web-controls-vb/_static/image10.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image15.png)

**Figura 10**: el texto no hay ningún Folleto disponible para las categorías sin un folleto ([haga clic para ver la imagen de tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image16.png))

## <a name="step-3-adding-a-web-page-to-display-a-category-s-picture"></a>Paso 3: agregar una página web para mostrar una categoría de imagen

Cuando un usuario visita una página de ASP.NET, recibe el código HTML de la página de ASP.NET. El HTML recibido es simplemente texto y no contiene datos binarios. Los datos binarios adicionales, como imágenes, archivos de sonido, aplicaciones Macromedia Flash, vídeos de Windows Media Player incrustados, etc., existen como recursos independientes en el servidor Web. El código HTML contiene referencias a estos archivos, pero no incluye el contenido real de los archivos.

Por ejemplo, en HTML, el elemento `<img>` se usa para hacer referencia a una imagen, con el atributo `src` que apunta al archivo de imagen como se indica a continuación:

[!code-html[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample4.html)]

Cuando un explorador recibe este código HTML, realiza otra solicitud al servidor web para recuperar el contenido binario del archivo de imagen, que luego se muestra en el explorador. El mismo concepto se aplica a los datos binarios. En el paso 2, el folleto no se envió al explorador como parte del marcado HTML de la página. En su lugar, el HTML representado proporcionó hipervínculos que, al hacer clic en él, hacía que el explorador solicitara el documento PDF directamente.

Para mostrar o permitir que los usuarios descarguen datos binarios que residen en la base de datos, es necesario crear una página web independiente que devuelva los datos. En nuestra aplicación, solo hay un campo de datos binarios almacenado directamente en la base de datos de la categoría de la imagen. Por lo tanto, se necesita una página que, cuando se llama, devuelve los datos de imagen para una categoría determinada.

Agregue una nueva página ASP.NET a la carpeta `BinaryData` denominada `DisplayCategoryPicture.aspx`. Al hacerlo, deje la casilla seleccionar página maestra desactivada. Esta página espera un valor `CategoryID` en QueryString y devuelve los datos binarios de esa categoría `Picture` columna. Puesto que esta página devuelve datos binarios y nada más, no necesita ningún marcado en la sección HTML. Por lo tanto, haga clic en la pestaña origen en la esquina inferior izquierda y quite todo el marcado de la página, excepto la Directiva `<%@ Page %>`. Es decir, el marcado declarativo de `DisplayCategoryPicture.aspx` s debe constar de una sola línea:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample5.aspx)]

Si ve el atributo `MasterPageFile` en la Directiva `<%@ Page %>`, quítelo.

En la clase de código subyacente de la página, agregue el código siguiente al controlador de eventos `Page_Load`:

[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample6.vb)]

Este código comienza leyendo el valor de la cadena de `CategoryID` QueryString en una variable denominada `categoryID`. A continuación, los datos de la imagen se recuperan a través de una llamada al método `CategoriesBLL` de clase `GetCategoryWithBinaryDataByCategoryID(categoryID)`. Estos datos se devuelven al cliente mediante el método `Response.BinaryWrite(data)`, pero antes de que se llame a, se debe quitar el encabezado OLE del valor de la columna `Picture`. Esto se logra creando una matriz de `Byte` denominada `strippedImageData` que contendrá exactamente 78 caracteres menos de lo que se encuentra en la columna `Picture`. El [método`Array.Copy`](https://msdn.microsoft.com/library/z50k9bft.aspx) se usa para copiar los datos de `category.Picture` a partir de la posición 78 a `strippedImageData`.

La propiedad `Response.ContentType` especifica el [tipo MIME](http://en.wikipedia.org/wiki/MIME) del contenido que se devuelve para que el explorador sepa cómo representarlo. Dado que la columna `Categories` Table s `Picture` es una imagen de mapa de bits, aquí se usa el tipo MIME de mapa de bits (Image/BMP). Si omite el tipo MIME, la mayoría de los exploradores seguirán mostrando la imagen correctamente, ya que pueden deducir el tipo en función del contenido de los datos binarios del archivo de imagen. Sin embargo, es prudente incluir el tipo MIME siempre que sea posible. Vea el [sitio web asignación de números de Internet](http://www.iana.org/) para obtener una lista completa de [tipos de medios MIME](http://www.iana.org/assignments/media-types/).

Con esta página creada, puede ver una determinada imagen de la categoría en `DisplayCategoryPicture.aspx?CategoryID=categoryID`. En la figura 11 se muestra la imagen de la categoría de Beverages, que se puede ver desde `DisplayCategoryPicture.aspx?CategoryID=1`.

[![se muestra la imagen de la categoría de bebidas](displaying-binary-data-in-the-data-web-controls-vb/_static/image11.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image17.png)

**Figura 11**: se muestra la imagen de la categoría de Beverages ([haga clic para ver la imagen de tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image18.png))

Si, al visitar `DisplayCategoryPicture.aspx?CategoryID=categoryID`, se produce una excepción que indica que no se puede convertir el objeto de tipo ' System. DBNull ' al tipo ' System. Byte [] ', hay dos cosas que podrían estar causando esto. En primer lugar, la columna `Categories` Table s `Picture` permite valores `NULL`. Sin embargo, la página `DisplayCategoryPicture.aspx` supone que hay un valor que no es`NULL`. No se puede tener acceso directamente a la propiedad `Picture` del `CategoriesDataTable` si tiene un valor `NULL`. Si desea permitir `NULL` valores para la columna `Picture`, quiere incluir la siguiente condición:

[!code-vb[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample7.vb)]

En el código anterior se supone que hay algún archivo de imagen denominado `NoPictureAvailable.gif` en la carpeta `Images` que desea mostrar para esas categorías sin una imagen.

Esta excepción también puede deberse a que la instrucción `CategoriesTableAdapter` s `GetCategoryWithBinaryDataByCategoryID` Method s `SELECT` ha vuelto a la lista de columnas de la consulta principal, lo que puede suceder si usa instrucciones SQL ad hoc y vuelve a ejecutar el Asistente para la consulta principal de TableAdapter s. Asegúrese de que la instrucción `GetCategoryWithBinaryDataByCategoryID` Method s `SELECT` aún incluye la columna `Picture`.

> [!NOTE]
> Cada vez que se visita el `DisplayCategoryPicture.aspx`, se obtiene acceso a la base de datos y se devuelven los datos de las imágenes de las categorías especificadas. No obstante, si la categoría s imagen no ha cambiado desde que el usuario la ha visto por última vez, esto es un esfuerzo desperdiciado. Afortunadamente, HTTP permite obtener las *obtieneciones condicionales*. Con una instrucción GET condicional, el cliente que realiza la solicitud HTTP envía a lo largo de un [`If-Modified-Since` encabezado HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) que proporciona la fecha y la hora en que el cliente recuperó por última vez este recurso del servidor Web. Si el contenido no ha cambiado desde la fecha especificada, el servidor web puede responder con un [código de estado no modificado (304)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) y renunciar a devolver el contenido de los recursos solicitados. En Resumen, esta técnica evita que el servidor web tenga que devolver el contenido de un recurso si no se ha modificado desde que el cliente tuvo acceso por última vez a él.

Sin embargo, para implementar este comportamiento, es necesario agregar una columna de `PictureLastModified` a la tabla de `Categories` para capturar Cuándo se actualizó por última vez la columna de `Picture`, así como el código para comprobar el encabezado de `If-Modified-Since`. Para obtener más información sobre el encabezado `If-Modified-Since` y el flujo de trabajo condicional GET, consulte [http Conditional Get for RSS hackers](http://fishbowl.pastiche.org/2002/10/21/http_conditional_get_for_rss_hackers) y [una mirada más detallada sobre la realización de solicitudes HTTP en una página ASP.net](http://aspnet.4guysfromrolla.com/articles/122204-1.aspx).

## <a name="step-4-displaying-the-category-pictures-in-a-gridview"></a>Paso 4: mostrar las imágenes de la categoría en un control GridView

Ahora que tenemos una página web para mostrar una imagen de categoría determinada, podemos mostrarla mediante el [control Web Image](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/image.aspx) o un elemento HTML `<img>` que apunte a `DisplayCategoryPicture.aspx?CategoryID=categoryID`. Las imágenes cuya dirección URL está determinada por los datos de la base de datos se pueden mostrar en GridView o DetailsView mediante ImageField. ImageField contiene `DataImageUrlField` y `DataImageUrlFormatString` propiedades que funcionan como las propiedades `DataNavigateUrlFields` y `DataNavigateUrlFormatString` de HyperLinkField.

Vamos a aumentar el `Categories` GridView en `DisplayOrDownloadData.aspx` agregando un ImageField para mostrar cada imagen de categoría. Simplemente agregue ImageField y establezca sus propiedades `DataImageUrlField` y `DataImageUrlFormatString` en `CategoryID` y `DisplayCategoryPicture.aspx?CategoryID={0}`, respectivamente. Esto creará una columna de GridView que representa un elemento `<img>` cuyo atributo `src` hace referencia a `DisplayCategoryPicture.aspx?CategoryID={0}`, donde {0} se reemplaza con el valor de `CategoryID` de filas de GridView.

![Agregar un ImageField a GridView](displaying-binary-data-in-the-data-web-controls-vb/_static/image12.gif)

**Figura 12**: agregar un ImageField a GridView

Después de agregar ImageField, la sintaxis declarativa s de GridView debe ser similar a SOOTHE siguiente:

[!code-aspx[Main](displaying-binary-data-in-the-data-web-controls-vb/samples/sample8.aspx)]

Tómese un momento para ver esta página a través de un explorador. Observe cómo cada registro incluye ahora una imagen para la categoría.

[![se muestra la categoría de la imagen para cada fila](displaying-binary-data-in-the-data-web-controls-vb/_static/image13.gif)](displaying-binary-data-in-the-data-web-controls-vb/_static/image19.png)

**Figura 13**: la categoría s imagen se muestra para cada fila ([haga clic para ver la imagen de tamaño completo](displaying-binary-data-in-the-data-web-controls-vb/_static/image20.png))

## <a name="summary"></a>Resumen

En este tutorial, hemos examinado cómo presentar datos binarios. La forma en que se presentan los datos depende del tipo de datos. En el caso de los archivos de folletos PDF, se ha ofrecido al usuario un vínculo de vista de folleto que, al hacer clic en él, tomó al usuario directamente al archivo PDF. En la categoría de la imagen, creamos primero una página para recuperar y devolver los datos binarios de la base de datos y, a continuación, usaban esa página para mostrar cada categoría de imagen en un control GridView.

Ahora que hemos visto cómo mostrar los datos binarios, se ha relisto para examinar cómo realizar las operaciones de inserción, actualización y eliminación en la base de datos con los datos binarios. En el siguiente tutorial veremos cómo asociar un archivo cargado a su registro de base de datos correspondiente. En el tutorial, veremos cómo actualizar los datos binarios existentes y cómo eliminar los datos binarios cuando se quita su registro asociado.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Teresa Murphy y Dave Gardner. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](uploading-files-vb.md)
> [Siguiente](including-a-file-upload-option-when-adding-a-new-record-vb.md)
