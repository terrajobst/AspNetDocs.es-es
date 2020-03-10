---
uid: web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
title: Cargar archivos (VB) | Microsoft Docs
author: rick-anderson
description: Obtenga información acerca de cómo permitir que los usuarios carguen archivos binarios (por ejemplo, documentos de Word o PDF) en el sitio web, donde pueden almacenarse en el sistema de archivos del servidor...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: f7c00fbd-652c-433d-8ed3-0e5168a4d4df
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/uploading-files-vb
msc.type: authoredcontent
ms.openlocfilehash: 6e0d57ef2f1e8132f19777a7d14e94611c68adcd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78441499"
---
# <a name="uploading-files-vb"></a>Carga de archivos (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_54_VB.exe) de la aplicación de ejemplo o [descarga de PDF](uploading-files-vb/_static/datatutorial54vb1.pdf)

> Obtenga información acerca de cómo permitir que los usuarios carguen archivos binarios (por ejemplo, documentos de Word o PDF) en el sitio web, donde pueden almacenarse en el sistema de archivos del servidor o en la base de datos.

## <a name="introduction"></a>Introducción

Todos los tutoriales que hemos examinado hasta ahora han trabajado exclusivamente con datos de texto. Sin embargo, muchas aplicaciones tienen modelos de datos que capturan datos de texto y binarios. Un sitio de citas en línea podría permitir que los usuarios carguen una imagen para asociarla a su perfil. Un sitio web de contratación podría permitir que los usuarios carguen su currículo como un documento de Microsoft Word o PDF.

Trabajar con datos binarios agrega un nuevo conjunto de desafíos. Es necesario decidir cómo se almacenan los datos binarios en la aplicación. La interfaz que se usa para insertar nuevos registros debe actualizarse para permitir al usuario cargar un archivo desde su equipo y se deben realizar pasos adicionales para mostrar o proporcionar un medio para descargar los datos binarios asociados a registros. En este tutorial y en los tres siguientes, exploraremos cómo superar estos desafíos. Al final de estos tutoriales, habrá creado una aplicación totalmente funcional que asocia una imagen y un folleto PDF con cada categoría. En este tutorial concreto, veremos distintas técnicas para almacenar datos binarios y exploraremos cómo permitir a los usuarios cargar un archivo desde su equipo y guardarlo en el sistema de archivos del servidor Web.

> [!NOTE]
> Los datos binarios que forman parte de un modelo de datos de la aplicación a veces se denominan [BLOB](http://en.wikipedia.org/wiki/Binary_large_object), un acrónimo del objeto binario grande. En estos tutoriales, he elegido usar los datos binarios terminológicos, aunque el término BLOB es sinónimo.

## <a name="step-1-creating-the-working-with-binary-data-web-pages"></a>Paso 1: crear páginas web de trabajo con datos binarios

Antes de empezar a explorar los desafíos asociados con la adición de compatibilidad con datos binarios, vamos a crear las páginas de ASP.NET en nuestro proyecto de sitio web que necesitamos para este tutorial y los tres siguientes. Comience agregando una nueva carpeta denominada `BinaryData`. A continuación, agregue las siguientes páginas de ASP.NET a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `FileUpload.aspx`
- `DisplayOrDownloadData.aspx`
- `UploadInDetailsView.aspx`
- `UpdatingAndDeleting.aspx`

![Agregar las páginas ASP.NET para los tutoriales relacionados con datos binarios](uploading-files-vb/_static/image1.gif)

**Figura 1**: agregar las páginas ASP.net para los tutoriales relacionados con datos binarios

Al igual que en las otras carpetas, `Default.aspx` en la carpeta `BinaryData` mostrará los tutoriales en su sección. Recuerde que el control de usuario `SectionLevelTutorialListing.ascx` proporciona esta funcionalidad. Por lo tanto, agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta la página s Vista de diseño.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](uploading-files-vb/_static/image2.gif)](uploading-files-vb/_static/image1.png)

**Figura 2**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](uploading-files-vb/_static/image2.png))

Por último, agregue estas páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después de mejorar el `<siteMapNode>`GridView:

[!code-xml[Main](uploading-files-vb/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para los tutoriales sobre cómo trabajar con datos binarios.

![El mapa del sitio ahora incluye entradas para los tutoriales sobre cómo trabajar con datos binarios](uploading-files-vb/_static/image3.gif)

**Figura 3**: ahora, el mapa del sitio incluye entradas para los tutoriales sobre cómo trabajar con datos binarios.

## <a name="step-2-deciding-where-to-store-the-binary-data"></a>Paso 2: decidir dónde almacenar los datos binarios

Los datos binarios asociados con el modelo de datos de la aplicación se pueden almacenar en uno de dos lugares: en el sistema de archivos del servidor Web con una referencia al archivo almacenado en la base de datos; o directamente dentro de la propia base de datos (consulte la figura 4). Cada enfoque tiene su propio conjunto de ventajas e inconvenientes, y merece una explicación más detallada.

[![datos binarios se pueden almacenar en el sistema de archivos o directamente en la base de datos](uploading-files-vb/_static/image4.gif)](uploading-files-vb/_static/image3.png)

**Figura 4**: los datos binarios se pueden almacenar en el sistema de archivos o directamente en la base de datos ([haga clic para ver la imagen de tamaño completo](uploading-files-vb/_static/image4.png))

Imagine que deseamos extender la base de datos Northwind para asociar una imagen a cada producto. Una opción sería almacenar estos archivos de imagen en el sistema de archivos del servidor Web y registrar la ruta de acceso en la tabla `Products`. Con este enfoque, se agrega una columna `ImagePath` a la tabla `Products` de tipo `varchar(200)`, quizás. Cuando un usuario carga una imagen para Chai, esa imagen podría estar almacenada en el sistema de archivos del servidor Web en `~/Images/Tea.jpg`, donde `~` representa la ruta de acceso física de la aplicación. Es decir, si el sitio web tiene la raíz de la ruta de acceso física `C:\Websites\Northwind\`, `~/Images/Tea.jpg` sería equivalente a `C:\Websites\Northwind\Images\Tea.jpg`. Después de cargar el archivo de imagen, es d actualizar el registro de Chai en la tabla de `Products` de forma que su `ImagePath` columna haga referencia a la ruta de acceso de la nueva imagen. Podríamos usar `~/Images/Tea.jpg` o simplemente `Tea.jpg` si decidimos que todas las imágenes de producto se colocarían en la carpeta Application s `Images`.

Las principales ventajas de almacenar los datos binarios en el sistema de archivos son:

- **Facilidad de implementación** como veremos en breve, el almacenamiento y la recuperación de datos binarios almacenados directamente en la base de datos implica un poco más de código que al trabajar con datos a través del sistema de archivos. Además, para que un usuario pueda ver o descargar datos binarios, se les debe presentar una dirección URL a los datos. Si los datos residen en el sistema de archivos del servidor Web, la dirección URL es sencilla. Sin embargo, si los datos están almacenados en la base de datos, es necesario crear una página web que recuperará y devolverá los datos de la base de datos.
- **Acceso más amplio a los datos binarios** . es posible que los datos binarios deban ser accesibles a otros servicios o aplicaciones, que no pueden extraer los datos de la base de datos. Por ejemplo, es posible que las imágenes asociadas a cada producto también deban estar disponibles para los usuarios a través de [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol), en cuyo caso se desea almacenar los datos binarios en el sistema de archivos.
- **Rendimiento** si los datos binarios se almacenan en el sistema de archivos, la congestión de la demanda y la red entre el servidor de base de datos y el servidor Web será menor que si los datos binarios se almacenan directamente en la base de datos.

La principal desventaja de almacenar los datos binarios en el sistema de archivos es que desacopla los datos de la base de datos. Si se elimina un registro de la tabla de `Products`, el archivo asociado en el sistema de archivos del servidor Web no se elimina automáticamente. Debemos escribir código adicional para eliminar el archivo o el sistema de archivos se llenará con archivos huérfanos no usados. Además, al realizar una copia de seguridad de la base de datos, debemos asegurarnos de que las copias de seguridad de los datos binarios asociados estén también en el sistema de archivos. Mover la base de datos a otro sitio o servidor plantea desafíos similares.

Como alternativa, los datos binarios se pueden almacenar directamente en una base de datos Microsoft SQL Server 2005 mediante la creación de una columna de tipo `varbinary`. Al igual que con otros tipos de datos de longitud variable, puede especificar una longitud máxima de los datos binarios que se pueden mantener en esta columna. Por ejemplo, para reservar como máximo 5.000 bytes, utilice `varbinary(5000)`; `varbinary(MAX)` permite el tamaño de almacenamiento máximo, aproximadamente 2 GB.

La principal ventaja de almacenar datos binarios directamente en la base de datos es el acoplamiento estrecho entre los datos binarios y el registro de base de datos. Esto simplifica considerablemente las tareas de administración de bases de datos, como las copias de seguridad o el traslado de la base de datos a otro sitio o servidor. Además, al eliminar un registro se eliminan automáticamente los datos binarios correspondientes. También hay ventajas más sutiles de almacenar los datos binarios en la base de datos. Consulte [almacenamiento de archivos binarios directamente en la base de datos mediante ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/120606-1.aspx) para obtener información más detallada.

> [!NOTE]
> En Microsoft SQL Server 2000 y versiones anteriores, el tipo de datos `varbinary` tenía un límite máximo de 8.000 bytes. Para almacenar hasta 2 GB de datos binarios, se debe usar en su lugar [`image` tipo de datos](https://msdn.microsoft.com/library/ms187993.aspx) . Sin embargo, con la adición de `MAX` en SQL Server 2005, el tipo de datos `image` está en desuso. Todavía se admite por compatibilidad con versiones anteriores, pero Microsoft ha anunciado que el tipo de datos `image` se quitará en una versión futura de SQL Server.

Si está trabajando con un modelo de datos más antiguo, puede ver el tipo de datos `image`. La tabla de `Categories` de datos Northwind tiene una columna `Picture` que se puede utilizar para almacenar los datos binarios de un archivo de imagen para la categoría. Como la base de datos Northwind tiene sus raíces en Microsoft Access y versiones anteriores de SQL Server, esta columna es de tipo `image`.

En este tutorial y en los tres siguientes, usaremos ambos enfoques. La tabla `Categories` ya tiene una columna `Picture` para almacenar el contenido binario de una imagen para la categoría. Agregaremos una columna adicional, `BrochurePath`, para almacenar una ruta de acceso a un archivo PDF en el sistema de archivos del servidor Web que se puede usar para proporcionar una visión general de calidad de impresión de la categoría.

## <a name="step-3-adding-thebrochurepathcolumn-to-thecategoriestable"></a>Paso 3: agregar la columna`BrochurePath`a la tabla`Categories`

Actualmente, la tabla Categories solo tiene cuatro columnas: `CategoryID`, `CategoryName`, `Description`y `Picture`. Además de estos campos, es necesario agregar uno nuevo que apunte al folleto de categoría s (si existe). Para agregar esta columna, vaya al Explorador de servidores, explore en profundidad las tablas, haga clic con el botón derecho en la tabla `Categories` y elija Abrir definición de tabla (vea la figura 5). Si no ve el Explorador de servidores, póngalo seleccionando la opción Explorador de servidores en el menú Ver o presione Ctrl + Alt + S.

Agregue una nueva columna de `varchar(200)` a la tabla de `Categories` denominada `BrochurePath` y permita `NULL` s y haga clic en el icono de guardar (o presione Ctrl + S).

[![agregar una columna BrochurePath a la tabla Categories](uploading-files-vb/_static/image5.gif)](uploading-files-vb/_static/image5.png)

**Figura 5**: agregar una columna `BrochurePath` a la tabla `Categories` ([haga clic para ver la imagen de tamaño completo](uploading-files-vb/_static/image6.png))

## <a name="step-4-updating-the-architecture-to-use-thepictureandbrochurepathcolumns"></a>Paso 4: actualización de la arquitectura para usar las columnas`Picture`y`BrochurePath`

Actualmente, el `CategoriesDataTable` de la capa de acceso a datos (DAL) tiene cuatro `DataColumn` s definidas: `CategoryID`, `CategoryName`, `Description`y `NumberOfProducts`. Cuando se diseñó originalmente este DataTable en el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) , el `CategoriesDataTable` solo tenía las tres primeras columnas; la columna de `NumberOfProducts` se agregó en el [maestro y detalles mediante una lista con viñetas de registros maestros con un tutorial de DataList de detalles](../filtering-scenarios-with-the-datalist-and-repeater/master-detail-using-a-bulleted-list-of-master-records-with-a-details-datalist-vb.md) .

Como se describe en *crear una capa de acceso a datos*, las tablas de datos del conjunto de datos con tipo componen los objetos comerciales. Los TableAdapters son responsables de comunicarse con la base de datos y de rellenar los objetos de negocio con los resultados de la consulta. El `CategoriesDataTable` rellena el `CategoriesTableAdapter`, que tiene tres métodos de recuperación de datos:

- `GetCategories()` ejecuta la consulta principal de TableAdapter s y devuelve los campos `CategoryID`, `CategoryName`y `Description` de todos los registros de la tabla `Categories`. La consulta principal es la que usan los métodos `Insert` y `Update` generados automáticamente.
- `GetCategoryByCategoryID(categoryID)` devuelve los campos `CategoryID`, `CategoryName`y `Description` de la categoría cuya `CategoryID` es igual a *CategoryID*.
- `GetCategoriesAndNumberOfProducts()`: devuelve los campos `CategoryID`, `CategoryName`y `Description` de todos los registros de la tabla `Categories`. También utiliza una subconsulta para devolver el número de productos asociados a cada categoría.

Tenga en cuenta que ninguna de estas consultas devuelve las columnas `Categories` Table s `Picture` o `BrochurePath`; ni el `CategoriesDataTable` proporcionan `DataColumn` s para estos campos. Para trabajar con las propiedades Picture y `BrochurePath`, es necesario agregarlas primero al `CategoriesDataTable` y, después, actualizar la clase `CategoriesTableAdapter` para devolver estas columnas.

## <a name="adding-thepictureandbrochurepathdatacolumn-s"></a>Agregar el`Picture`y`BrochurePath``DataColumn` s

Comience agregando estas dos columnas a la `CategoriesDataTable`. Haga clic con el botón derecho en el encabezado `CategoriesDataTable` s, seleccione Agregar en el menú contextual y, a continuación, elija la opción columna. Esto creará un nuevo `DataColumn` en DataTable denominado `Column1`. Cambie el nombre de esta columna a `Picture`. En el ventana Propiedades, establezca la propiedad `DataColumn` s `DataType` en `System.Byte[]` (no es una opción de la lista desplegable; debe escribirla en).

[![crear un DataColumn denominado Picture cuyo DataType sea System. Byte []](uploading-files-vb/_static/image6.gif)](uploading-files-vb/_static/image7.png)

**Figura 6**: crear una `DataColumn` denominada `Picture` cuya `DataType` sea `System.Byte[]` ([haga clic para ver la imagen de tamaño completo](uploading-files-vb/_static/image8.png))

Agregue otro `DataColumn` a la DataTable y asígnele el nombre `BrochurePath` mediante el valor predeterminado de `DataType` (`System.String`).

## <a name="returning-thepictureandbrochurepathvalues-from-the-tableadapter"></a>Devolver los valores de`Picture`y`BrochurePath`de TableAdapter

Con estos dos `DataColumn` s agregados al `CategoriesDataTable`, se vuelve a preparar para actualizar el `CategoriesTableAdapter`. Podríamos tener ambos valores de columna devueltos en la consulta de TableAdapter principal, pero esto volvería a recuperar los datos binarios cada vez que se invocó el método `GetCategories()`. En su lugar, permita que s actualice la consulta de TableAdapter principal para volver `BrochurePath` y crear un método de recuperación de datos adicional que devuelva una determinada categoría de `Picture` columna.

Para actualizar la consulta de TableAdapter principal, haga clic con el botón derecho en el encabezado `CategoriesTableAdapter` s y elija la opción configurar en el menú contextual. Se abre el Asistente para configuración del adaptador de tabla, que se ve en una serie de tutoriales anteriores. Actualice la consulta para devolver el `BrochurePath` y haga clic en finalizar.

[![actualizar la lista de columnas de la instrucción SELECT para devolver también BrochurePath](uploading-files-vb/_static/image7.gif)](uploading-files-vb/_static/image9.png)

**Figura 7**: actualización de la lista de columnas en la instrucción `SELECT` para devolver también `BrochurePath` ([haga clic para ver la imagen de tamaño completo](uploading-files-vb/_static/image10.png))

Cuando se usan instrucciones SQL ad hoc para el TableAdapter, la actualización de la lista de columnas en la consulta principal actualiza la lista de columnas para todos los métodos de consulta de `SELECT` en el TableAdapter. Esto significa que el método de `GetCategoryByCategoryID(categoryID)` se ha actualizado para devolver la columna `BrochurePath`, que podría ser la que pretendía. Sin embargo, también se actualizó la lista de columnas en el método `GetCategoriesAndNumberOfProducts()`, quitando la subconsulta que devuelve el número de productos de cada categoría. Por lo tanto, es necesario actualizar este método `SELECT` consulta. Haga clic con el botón derecho en el método `GetCategoriesAndNumberOfProducts()`, elija configurar y revierta el `SELECT` consulta a su valor original:

[!code-sql[Main](uploading-files-vb/samples/sample2.sql)]

A continuación, cree un nuevo método TableAdapter que devuelva una categoría determinada `Picture` valor de la columna. Haga clic con el botón derecho en el encabezado `CategoriesTableAdapter` s y elija la opción Agregar consulta para iniciar el Asistente para configuración de consultas de TableAdapter. El primer paso de este asistente nos pregunta si queremos consultar los datos mediante una instrucción SQL ad hoc, un nuevo procedimiento almacenado o uno existente. Seleccione usar instrucciones SQL y haga clic en siguiente. Como se devolverá una fila, elija la opción seleccionar qué devuelve filas en el segundo paso.

[![seleccionar la opción usar instrucciones SQL](uploading-files-vb/_static/image8.gif)](uploading-files-vb/_static/image11.png)

**Figura 8**: seleccione la opción usar instrucciones SQL ([haga clic para ver la imagen de tamaño completo](uploading-files-vb/_static/image12.png))

[![debido a que la consulta devolverá un registro de la tabla Categories, elija SELECT que devuelve las filas](uploading-files-vb/_static/image9.gif)](uploading-files-vb/_static/image13.png)

**Figura 9**: dado que la consulta devolverá un registro de la tabla Categories, elija Select, que devuelve las filas ([haga clic para ver la imagen a tamaño completo](uploading-files-vb/_static/image14.png))

En el tercer paso, escriba la siguiente consulta SQL y haga clic en siguiente:

[!code-sql[Main](uploading-files-vb/samples/sample3.sql)]

El último paso es elegir el nombre del nuevo método. Utilice `FillCategoryWithBinaryDataByCategoryID` y `GetCategoryWithBinaryDataByCategoryID` para los patrones rellenar un DataTable y devolver un objeto DataTable, respectivamente. Haga clic en Finalizar para completar el asistente.

[![elegir los nombres de los métodos de TableAdapter s](uploading-files-vb/_static/image10.gif)](uploading-files-vb/_static/image15.png)

**Figura 10**: elija los nombres de los métodos de TableAdapter ([haga clic para ver la imagen de tamaño completo](uploading-files-vb/_static/image16.png))

> [!NOTE]
> Después de completar el Asistente para configuración de consultas del adaptador de tabla, puede ver un cuadro de diálogo que le informa de que el nuevo texto de comando devuelve datos con un esquema diferente del esquema de la consulta principal. En Resumen, el asistente está teniendo en cuenta que la consulta principal de TableAdapter s `GetCategories()` devuelve un esquema diferente del que acabamos de crear. Pero esto es lo que queremos, por lo que puede ignorar este mensaje.

Además, tenga en cuenta que si usa instrucciones SQL ad-hoc y usa el Asistente para cambiar la consulta principal de TableAdapter s en un momento posterior, modificará la lista de columnas de la instrucción `GetCategoryWithBinaryDataByCategoryID` Method s `SELECT` s para incluir solo las columnas de la consulta principal (es decir, quitará la `Picture` columna de la consulta). Tendrá que actualizar manualmente la lista de columnas para devolver el `Picture` columna, de forma similar a lo que hicimos con el método `GetCategoriesAndNumberOfProducts()` anteriormente en este paso.

Después de agregar los dos `DataColumn` s al `CategoriesDataTable` y el método `GetCategoryWithBinaryDataByCategoryID` al `CategoriesTableAdapter`, estas clases del diseñador de DataSet con tipo deben tener un aspecto similar a la captura de pantalla de la figura 11.

![El diseñador de DataSet incluye las nuevas columnas y el método](uploading-files-vb/_static/image11.gif)

**Figura 11**: el diseñador de DataSet incluye las nuevas columnas y el método

## <a name="updating-the-business-logic-layer-bll"></a>Actualizar la capa de lógica de negocios (BLL)

Con la capa DAL actualizada, todo lo que queda es aumentar el nivel de lógica de negocios (BLL) para incluir un método para el nuevo método de `CategoriesTableAdapter`. Agregue el siguiente método a la clase `CategoriesBLL`:

[!code-vb[Main](uploading-files-vb/samples/sample4.vb)]

## <a name="step-5-uploading-a-file-from-the-client-to-the-web-server"></a>Paso 5: carga de un archivo desde el cliente al servidor Web

Al recopilar datos binarios, a menudo, estos datos son proporcionados por un usuario final. Para capturar esta información, el usuario debe ser capaz de cargar un archivo desde su equipo al servidor Web. A continuación, los datos cargados deben integrarse con el modelo de datos, lo que puede significar guardar el archivo en el sistema de archivos del servidor Web y agregar una ruta de acceso al archivo en la base de datos, o escribir el contenido binario directamente en la base de datos. En este paso veremos cómo permitir a un usuario cargar archivos desde su equipo al servidor. En el siguiente tutorial, haremos la atención a la integración del archivo cargado con el modelo de datos.

El nuevo [control Web FileUpload](https://msdn.microsoft.com/library/ms227677(VS.80).aspx) de ASP.net 2,0 s proporciona un mecanismo para que los usuarios envíen un archivo desde su equipo al servidor Web. El control FileUpload se representa como un elemento `<input>` cuyo atributo `type` se establece en File, que los exploradores se muestran como un cuadro de texto con un botón examinar. Al hacer clic en el botón examinar, se abre un cuadro de diálogo en el que el usuario puede seleccionar un archivo. Cuando se devuelve el formulario, el contenido del archivo seleccionado se envía junto con el PostBack. En el lado del servidor, se puede acceder a la información sobre el archivo cargado a través de las propiedades del control FileUpload.

Para mostrar los archivos de carga, abra la página `FileUpload.aspx` de la carpeta `BinaryData`, arrastre un control FileUpload desde el cuadro de herramientas hasta el diseñador y establezca la propiedad control s `ID` en `UploadTest`. A continuación, agregue un control Web Button estableciendo sus propiedades `ID` y `Text` para `UploadButton` y cargar el archivo seleccionado, respectivamente. Por último, coloque un control Web de etiqueta debajo del botón, borre su `Text` propiedad y establezca su propiedad `ID` en `UploadDetails`.

[![agregar un control FileUpload a la página ASP.NET](uploading-files-vb/_static/image12.gif)](uploading-files-vb/_static/image17.png)

**Figura 12**: agregar un control FileUpload a la página ASP.net ([haga clic para ver la imagen de tamaño completo](uploading-files-vb/_static/image18.png))

La figura 13 muestra esta página cuando se ve a través de un explorador. Tenga en cuenta que al hacer clic en el botón examinar se abre un cuadro de diálogo de selección de archivos, lo que permite al usuario seleccionar un archivo de su equipo. Una vez que se ha seleccionado un archivo, al hacer clic en el botón Cargar archivo seleccionado, se produce un postback que envía el contenido binario del archivo s seleccionado al servidor Web.

[![el usuario puede seleccionar un archivo para cargarlo desde su equipo al servidor](uploading-files-vb/_static/image13.gif)](uploading-files-vb/_static/image19.png)

**Figura 13**: el usuario puede seleccionar un archivo para cargarlo desde su equipo al servidor ([haga clic para ver la imagen a tamaño completo](uploading-files-vb/_static/image20.png))

En el postback, el archivo cargado se puede guardar en el sistema de archivos o los datos binarios pueden trabajar directamente con un flujo. En este ejemplo, vamos a crear una carpeta `~/Brochures` y guardar allí el archivo cargado. Comience agregando la carpeta `Brochures` al sitio como una subcarpeta del directorio raíz. A continuación, cree un controlador de eventos para el evento `UploadButton` s `Click` y agregue el código siguiente:

[!code-vb[Main](uploading-files-vb/samples/sample5.vb)]

El control FileUpload proporciona diversas propiedades para trabajar con los datos cargados. Por ejemplo, la [propiedad`HasFile`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.hasfile.aspx) indica si el usuario ha cargado un archivo, mientras que la [propiedad`FileBytes`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload.filebytes.aspx) proporciona acceso a los datos binarios cargados como una matriz de bytes. El controlador de eventos `Click` se inicia asegurándose de que se ha cargado un archivo. Si se ha cargado un archivo, la etiqueta muestra el nombre del archivo cargado, su tamaño en bytes y su tipo de contenido.

> [!NOTE]
> Para asegurarse de que el usuario cargue un archivo, puede comprobar la propiedad `HasFile` y mostrar una advertencia si se `False`, o puede usar el [control RequiredFieldValidator](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/validation/default.aspx) en su lugar.

El `SaveAs(filePath)` de FileUpload guarda el archivo cargado en la *ruta*de acceso especificada. *filePath* debe ser una *ruta de acceso física* (`C:\Websites\Brochures\SomeFile.pdf`) en lugar de una *ruta de acceso* *virtual* (`/Brochures/SomeFile.pdf`). El [método`Server.MapPath(virtPath)`](https://msdn.microsoft.com/library/system.web.httpserverutility.mappath.aspx) toma una ruta de acceso virtual y devuelve la ruta de acceso física correspondiente. Aquí, la ruta de acceso virtual es `~/Brochures/fileName`, donde *filename* es el nombre del archivo cargado. Consulte [uso de Server. MapPath](http://www.4guysfromrolla.com/webtech/121799-1.shtml) para obtener más información sobre las rutas de acceso virtuales y físicas y el uso de `Server.MapPath`.

Después de completar el controlador de eventos `Click`, dedique un momento a probar la página en un explorador. Haga clic en el botón examinar y seleccione un archivo de la unidad de disco duro y, a continuación, haga clic en el botón Cargar archivo seleccionado. El PostBack enviará el contenido del archivo seleccionado al servidor Web, que mostrará información sobre el archivo antes de guardarlo en la carpeta `~/Brochures`. Después de cargar el archivo, vuelva a Visual Studio y haga clic en el botón actualizar en el Explorador de soluciones. Debería ver el archivo que acaba de cargar en la carpeta ~/brochures

[![el archivo EvolutionValley. jpg se ha cargado en el servidor Web](uploading-files-vb/_static/image14.gif)](uploading-files-vb/_static/image21.png)

**Figura 14**: el archivo `EvolutionValley.jpg` se ha cargado en el servidor Web ([haga clic para ver la imagen de tamaño completo](uploading-files-vb/_static/image22.png))

![EvolutionValley. jpg se guardó en la carpeta ~/brochures](uploading-files-vb/_static/image15.gif)

**Figura 15**: se guardó `EvolutionValley.jpg` en la carpeta `~/Brochures`

## <a name="subtleties-with-saving-uploaded-files-to-the-file-system"></a>Matices con guardar archivos cargados en el sistema de archivos

Hay varios matices que deben abordarse al guardar archivos de carga en el sistema de archivos del servidor Web. En primer lugar, hay el problema de seguridad. Para guardar un archivo en el sistema de archivos, el contexto de seguridad en el que se ejecuta la página ASP.NET debe tener permisos de escritura. El servidor Web de desarrollo de ASP.NET se ejecuta en el contexto de su cuenta de usuario actual. Si usa Microsoft s Internet Information Services (IIS) como servidor Web, el contexto de seguridad depende de la versión de IIS y de su configuración.

Otro desafío de guardar archivos en el sistema de archivos gira en torno a la asignación de nombres a los archivos. Actualmente, nuestra página guarda todos los archivos cargados en el directorio `~/Brochures` con el mismo nombre que el archivo en el equipo cliente. Si el usuario A carga un folleto con el nombre `Brochure.pdf`, el archivo se guardará como `~/Brochure/Brochure.pdf`. Pero, ¿qué ocurre si más tarde el usuario B carga un archivo de folleto diferente que tiene el mismo nombre de archivo (`Brochure.pdf`)? Con el código que tenemos ahora, los archivos de usuario A s se sobrescribirán con la carga de usuario B s.

Existen varias técnicas para resolver conflictos de nombres de archivo. Una opción consiste en prohibir la carga de un archivo si ya existe uno con el mismo nombre. Con este enfoque, cuando el usuario B intenta cargar un archivo denominado `Brochure.pdf`, el sistema no guardaría su archivo y, en su lugar, muestra un mensaje que informa al usuario B para cambiar el nombre del archivo e intentarlo de nuevo. Otro enfoque consiste en guardar el archivo con un nombre de archivo único, que podría ser un [identificador único global (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) o el valor de las columnas de clave principal del registro de base de datos correspondiente (suponiendo que la carga esté asociada a una fila determinada del modelo de datos). En el siguiente tutorial Exploraremos estas opciones con más detalle.

## <a name="challenges-involved-with-very-large-amounts-of-binary-data"></a>Desafíos relacionados con cantidades muy grandes de datos binarios

En estos tutoriales se supone que los datos binarios capturados tienen un tamaño modesto. El trabajo con cantidades muy grandes de archivos de datos binarios con varios megabytes o más, presenta nuevos desafíos que van más allá del ámbito de estos tutoriales. Por ejemplo, de forma predeterminada, ASP.NET rechazará las cargas de más de 4 MB, aunque esto se puede configurar mediante el [elemento`<httpRuntime>`](https://msdn.microsoft.com/library/e1f13641.aspx) en `Web.config`. IIS impone también sus propias limitaciones de tamaño de carga de archivos. Para obtener más información, vea [IIS Upload File size](http://vandamme.typepad.com/development/2005/09/iis_upload_file.html) . Además, el tiempo necesario para cargar archivos grandes puede superar los 110 segundos predeterminados ASP.NET esperará una solicitud. También hay problemas de memoria y rendimiento que surgen cuando se trabaja con archivos de gran tamaño.

El control FileUpload no es práctico para cargas de archivos grandes. A medida que el contenido de los archivos se está publicando en el servidor, el usuario final debe esperar por paciente sin confirmación de que la carga progresa. Esto no es tan importante cuando se trabaja con archivos más pequeños que se pueden cargar en pocos segundos, pero puede ser un problema cuando se trabaja con archivos más grandes que pueden tardar minutos en cargarse. Hay una gran variedad de controles de carga de archivos de terceros que son más adecuados para administrar grandes cargas y muchos de estos proveedores proporcionan indicadores de progreso y administradores de carga de ActiveX que presentan una experiencia de usuario mucho más pulida.

Si su aplicación necesita controlar archivos de gran tamaño, deberá investigar detenidamente los desafíos y encontrar soluciones adecuadas para sus necesidades particulares.

## <a name="summary"></a>Resumen

La creación de una aplicación que necesite capturar datos binarios presenta una serie de desafíos. En este tutorial se exploran los dos primeros: decidir dónde almacenar los datos binarios y permitir que un usuario cargue contenido binario a través de una página web. En los tres tutoriales siguientes, veremos cómo asociar los datos cargados con un registro en la base de datos y cómo mostrar los datos binarios junto con sus campos de datos de texto.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Usar tipos de datos de valores grandes](https://msdn.microsoft.com/library/ms178158.aspx)
- [Inicios rápidos del control FileUpload](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/standard/fileupload.aspx)
- [Control de servidor ASP.NET 2,0 FileUpload](http://www.wrox.com/WileyCDA/Section/id-292158.html)
- [El lado oscuro de las cargas de archivos](http://www.aspnetresources.com/articles/dark_side_of_file_uploads.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial eran Teresa Murphy y Bernadette Leigh. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](updating-and-deleting-existing-binary-data-cs.md)
> [Siguiente](displaying-binary-data-in-the-data-web-controls-vb.md)
