---
uid: web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
title: Incluir una opción de carga de archivos al agregar un nuevo registro (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial se muestra cómo crear una interfaz web que permite al usuario escribir datos de texto y cargar archivos binarios. Para ilustrar las opciones disponibles...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 5776281d-4637-4d1e-a65b-2621d2cade44
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/including-a-file-upload-option-when-adding-a-new-record-vb
msc.type: authoredcontent
ms.openlocfilehash: 8edaf1754eddd7b03f1c323d1bee13238582fc99
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78475345"
---
# <a name="including-a-file-upload-option-when-adding-a-new-record-vb"></a>Incluir una opción de carga de archivos al agregar un nuevo registro (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_56_VB.exe) de la aplicación de ejemplo o [descarga de PDF](including-a-file-upload-option-when-adding-a-new-record-vb/_static/datatutorial56vb1.pdf)

> En este tutorial se muestra cómo crear una interfaz web que permite al usuario escribir datos de texto y cargar archivos binarios. Para ilustrar las opciones disponibles para almacenar datos binarios, se guardará un archivo en la base de datos mientras el otro está almacenado en el sistema de archivos.

## <a name="introduction"></a>Introducción

En los dos tutoriales anteriores, hemos explorado técnicas para almacenar datos binarios que están asociados con el modelo de datos de la aplicación, hemos examinado cómo usar el control FileUpload para enviar archivos desde el cliente al servidor Web y hemos visto cómo presentar estos datos binarios en un Data W control EB. Sin embargo, aún hablaremos sobre cómo asociar datos cargados con el modelo de datos.

En este tutorial, se creará una página web para agregar una nueva categoría. Además de los cuadros de texto del nombre y la descripción de la categoría s, esta página deberá incluir dos controles FileUpload uno para la nueva imagen categoría s y otro para el folleto. La imagen cargada se almacenará directamente en la columna nuevo registro s `Picture`, mientras que el folleto se guardará en la carpeta `~/Brochures` con la ruta de acceso al archivo guardado en la columna nuevo registro s `BrochurePath`.

Antes de crear esta nueva página web, es necesario actualizar la arquitectura. La consulta principal `CategoriesTableAdapter` s no recupera la columna `Picture`. Por consiguiente, el método de `Insert` generado automáticamente solo tiene entradas para los campos `CategoryName`, `Description`y `BrochurePath`. Por lo tanto, es necesario crear un método adicional en el TableAdapter que solicite los cuatro campos de `Categories`. También será necesario actualizar la clase `CategoriesBLL` en el nivel de lógica de negocios.

## <a name="step-1-adding-aninsertwithpicturemethod-to-thecategoriestableadapter"></a>Paso 1: agregar un método`InsertWithPicture`a la`CategoriesTableAdapter`

Cuando creamos el `CategoriesTableAdapter` en el tutorial [creación de una capa de acceso a datos](../introduction/creating-a-data-access-layer-vb.md) , la configuramos para generar automáticamente las instrucciones `INSERT`, `UPDATE`y `DELETE` basadas en la consulta principal. Además, se indicó al TableAdapter que empleara el enfoque de DB Direct, que creaba los métodos `Insert`, `Update`y `Delete`. Estos métodos ejecutan las instrucciones `INSERT`, `UPDATE`y `DELETE` generadas automáticamente y, por consiguiente, aceptan parámetros de entrada basados en las columnas devueltas por la consulta principal. En el tutorial de [carga de archivos](uploading-files-vb.md) , se ha ampliado la consulta principal `CategoriesTableAdapter` s para usar la columna `BrochurePath`.

Dado que la consulta principal `CategoriesTableAdapter` s no hace referencia a la columna `Picture`, no se puede Agregar un registro nuevo ni actualizar un registro existente con un valor para la columna `Picture`. Para capturar esta información, podemos crear un nuevo método en el TableAdapter que se usa específicamente para insertar un registro con datos binarios o podemos personalizar la instrucción de `INSERT` generada automáticamente. El problema con la personalización de la instrucción `INSERT` generada automáticamente es que nos arriesgamos a que las personalizaciones se sobrescriban mediante el asistente. Por ejemplo, Imagine que hemos personalizado la instrucción `INSERT` para incluir el uso de la columna `Picture`. Esto actualizaría el método TableAdapter s `Insert` para incluir un parámetro de entrada adicional para los datos binarios de la categoría s Picture s. A continuación, podríamos crear un método en el nivel de lógica de negocios para usar este método DAL e invocar este método BLL a través de la capa de presentación y todo funcionaría bien. Es decir, hasta la próxima vez que se configure el TableAdapter mediante el Asistente para configuración de TableAdapter. En cuanto se complete el asistente, se sobrescribirán las personalizaciones de la instrucción `INSERT`, el método `Insert` volvería a su forma antigua y nuestro código ya no se compilaría.

> [!NOTE]
> Esta molestia es un problema no relacionado con el uso de procedimientos almacenados en lugar de instrucciones SQL ad hoc. En un futuro tutorial se explorará el uso de procedimientos almacenados en lugar de instrucciones SQL ad hoc en la capa de acceso a datos.

Para evitar este posible dolor de cabeza, en lugar de personalizar las instrucciones SQL generadas automáticamente, en su lugar, cree un nuevo método para el TableAdapter. Este método, denominado `InsertWithPicture`, aceptará valores para las columnas `CategoryName`, `Description`, `BrochurePath`y `Picture` y ejecutará una instrucción `INSERT` que almacena los cuatro valores en un nuevo registro.

Abra el conjunto de los tipos y, en el diseñador, haga clic con el botón derecho en el encabezado `CategoriesTableAdapter` s y elija Agregar consulta en el menú contextual. Esto inicia el Asistente para la configuración de consultas de TableAdapter, que comienza preguntando cómo debe tener acceso la consulta de TableAdapter a la base de datos. Elija usar instrucciones SQL y haga clic en siguiente. El paso siguiente solicita el tipo de consulta que se va a generar. Dado que se va a crear una consulta para agregar un nuevo registro a la tabla de `Categories`, elija Insertar y haga clic en siguiente.

[![seleccionar la opción insertar](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image1.png)

**Figura 1**: seleccione la opción Insertar ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.png))

Ahora es necesario especificar el `INSERT` instrucción SQL. El Asistente sugiere automáticamente una instrucción `INSERT` correspondiente a la consulta principal de TableAdapter s. En este caso, es una instrucción `INSERT` que inserta los valores `CategoryName`, `Description`y `BrochurePath`. Actualice la instrucción para que la columna `Picture` esté incluida junto con un parámetro `@Picture`, como por ejemplo:

[!code-sql[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample1.sql)]

La última pantalla del asistente nos pide asignar un nombre al nuevo método TableAdapter. Escriba `InsertWithPicture` y haga clic en finalizar.

[![nombre al nuevo método TableAdapter InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image2.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.png)

**Figura 2**: asigne un nombre al nuevo método TableAdapter `InsertWithPicture` ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.png))

## <a name="step-2-updating-the-business-logic-layer"></a>Paso 2: actualización del nivel de lógica de negocios

Puesto que el nivel de presentación solo debe interactuar con el nivel de lógica de negocios en lugar de omitirlo para ir directamente a la capa de acceso a datos, es necesario crear un método BLL que invoque el método DAL que se acaba de crear (`InsertWithPicture`). Para este tutorial, cree un método en la clase `CategoriesBLL` denominada `InsertWithPicture` que acepte como entrada tres `String` s y una matriz de `Byte`. Los parámetros de entrada `String` son para el nombre, la descripción y la ruta de acceso del archivo del folleto, mientras que la matriz de `Byte` es para el contenido binario de la categoría s imagen. Como se muestra en el código siguiente, este método BLL invoca el método DAL correspondiente:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample2.vb)]

> [!NOTE]
> Asegúrese de que ha guardado el conjunto de DataSet con tipo antes de agregar el método `InsertWithPicture` a la capa BLL. Dado que el código de la clase `CategoriesTableAdapter` se genera automáticamente basándose en el conjunto de DataSet con tipo, si no guarda primero los cambios en el conjunto de tipos, la propiedad `Adapter` no conocerá el método `InsertWithPicture`.

## <a name="step-3-listing-the-existing-categories-and-their-binary-data"></a>Paso 3: enumerar las categorías existentes y sus datos binarios

En este tutorial, se creará una página que permite a un usuario final agregar una nueva categoría al sistema, lo que proporciona una imagen y un folleto para la nueva categoría. En el [tutorial anterior](displaying-binary-data-in-the-data-web-controls-vb.md) usamos un control GridView con TemplateField y ImageField para mostrar el nombre de cada categoría, la descripción, la imagen y un vínculo para descargar su folleto. Vamos a replicar esa funcionalidad para este tutorial, creando una página que enumera todas las categorías existentes y permite que se creen nuevas.

Para empezar, abra la página de `DisplayOrDownload.aspx` de la carpeta `BinaryData`. Vaya a la vista de código fuente y copie la sintaxis declarativa de GridView y ObjectDataSource s, pegándola en el elemento `<asp:Content>` de `UploadInDetailsView.aspx`. Además, no olvide copiar el método `GenerateBrochureLink` desde la clase de código subyacente de `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx`.

[![copiar y pegar la sintaxis declarativa de DisplayOrDownload. aspx en UploadInDetailsView. aspx](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image3.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.png)

**Figura 3**: copiar y pegar la sintaxis declarativa de `DisplayOrDownload.aspx` a `UploadInDetailsView.aspx` ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.png))

Después de copiar la sintaxis declarativa y `GenerateBrochureLink` método en la página `UploadInDetailsView.aspx`, vea la página a través de un explorador para asegurarse de que todo se ha copiado correctamente. Debería ver un control GridView que muestre las ocho categorías que incluye un vínculo para descargar el folleto, así como la categoría s imagen.

[![debería ver ahora cada categoría junto con sus datos binarios](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image4.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.png)

**Figura 4**: ahora debería ver cada categoría junto con sus datos binarios ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.png))

## <a name="step-4-configuring-thecategoriesdatasourceto-support-inserting"></a>Paso 4: configurar la`CategoriesDataSource`para admitir la inserción

El `CategoriesDataSource` ObjectDataSource que usa el `Categories` GridView actualmente no proporciona la capacidad de insertar datos. Para permitir la inserción a través de este control de origen de datos, es necesario asignar su método de `Insert` a un método en su objeto subyacente, `CategoriesBLL`. En particular, queremos asignarlo al método `CategoriesBLL` que hemos agregado en el paso 2, `InsertWithPicture`.

Para empezar, haga clic en el vínculo configurar origen de datos de la etiqueta inteligente de ObjectDataSource s. La primera pantalla muestra el objeto con el que se configura el origen de datos, `CategoriesBLL`. Deje esta configuración tal cual y haga clic en siguiente para avanzar a la pantalla definir métodos de datos. Desplácese a la pestaña insertar y seleccione el método `InsertWithPicture` de la lista desplegable. Haga clic en Finalizar para completar el asistente.

[![configurar ObjectDataSource para usar el método InsertWithPicture](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image5.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.png)

**Figura 5**: configuración de ObjectDataSource para usar el método `InsertWithPicture` ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.png))

> [!NOTE]
> Al finalizar el asistente, Visual Studio puede preguntar si desea actualizar los campos y las claves, lo que volverá a generar los campos de controles Web de datos. Elija no, porque si elige sí, se sobrescribirán las personalizaciones de campo que haya realizado.

Después de completar el asistente, ObjectDataSource incluirá ahora un valor para su propiedad `InsertMethod`, así como `InsertParameters` para las cuatro columnas de categoría, como se muestra en el siguiente marcado declarativo:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample3.aspx)]

## <a name="step-5-creating-the-inserting-interface"></a>Paso 5: creación de la interfaz de inserción

Como se describe primero en [información general sobre la inserción, actualización y eliminación de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md), el control DetailsView proporciona una interfaz de inserción integrada que se puede utilizar cuando se trabaja con un control de origen de datos que admite la inserción. Vamos a agregar un control DetailsView a esta página encima de GridView que representará permanentemente su interfaz de inserción, lo que permite al usuario agregar rápidamente una nueva categoría. Al agregar una nueva categoría en DetailsView, el control GridView debajo se actualizará automáticamente y mostrará la nueva categoría.

Empiece arrastrando un control DetailsView desde el cuadro de herramientas hasta el diseñador situado encima de GridView, estableciendo su propiedad `ID` en `NewCategory` y borrando los valores de las propiedades `Height` y `Width`. En la etiqueta inteligente de DetailsView s, enlácelo al `CategoriesDataSource` existente y, a continuación, active la casilla habilitar inserción.

[![enlazar DetailsView a CategoriesDataSource y habilitar la inserción](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image6.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.png)

**Figura 6**: enlace de DetailsView al `CategoriesDataSource` y habilitación de la inserción ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image12.png))

Para representar de forma permanente DetailsView en la interfaz de inserción, establezca su propiedad `DefaultMode` en `Insert`.

Tenga en cuenta que DetailsView tiene cinco `CategoryID`BoundFields, `CategoryName`, `Description`, `NumberOfProducts`y `BrochurePath` aunque el `CategoryID` BoundField no se representa en la interfaz de inserción porque su propiedad `InsertVisible` está establecida en `False`. Estos BoundFields existen porque son las columnas devueltas por el método `GetCategories()`, que es lo que invoca ObjectDataSource para recuperar sus datos. Sin embargo, para la inserción, no queremos permitir que el usuario especifique un valor para `NumberOfProducts`. Además, es necesario permitirles cargar una imagen para la nueva categoría, así como cargar un PDF para el folleto.

Quite todos los `NumberOfProducts` BoundField de DetailsView y, a continuación, actualice las propiedades `HeaderText` de los `CategoryName` y `BrochurePath` BoundFields en categoría y folleto, respectivamente. A continuación, convierta el `BrochurePath` BoundField en TemplateField y agregue una nueva TemplateField para la imagen, y asigne a esta nueva TemplateField un valor `HeaderText` de Picture. Mueva el `Picture` TemplateField para que esté entre el `BrochurePath` TemplateField y CommandField.

![Enlazar DetailsView a CategoriesDataSource y habilitar la inserción](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image7.gif)

**Figura 7**: enlace de DetailsView al `CategoriesDataSource` y habilitación de la inserción

Si convirtió el `BrochurePath` BoundField en TemplateField a través del cuadro de diálogo Editar campos, TemplateField incluye un `ItemTemplate`, `EditItemTemplate`y `InsertItemTemplate`. Sin embargo, solo se necesita el `InsertItemTemplate`, así que no dude en quitar las otras dos plantillas. En este momento, la sintaxis declarativa s de DetailsView debe ser similar a la siguiente:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample4.aspx)]

## <a name="adding-fileupload-controls-for-the-brochure-and-picture-fields"></a>Agregar controles FileUpload para los campos folleto e imagen

En la actualidad, el `BrochurePath` TemplateField s `InsertItemTemplate` contiene un cuadro de texto, mientras que el `Picture` TemplateField no contiene ninguna plantilla. Necesitamos actualizar estos dos `InsertItemTemplate` s de TemplateField para usar los controles FileUpload.

En la etiqueta inteligente de DetailsView s, elija la opción editar plantillas y, a continuación, seleccione la `BrochurePath` TemplateField s `InsertItemTemplate` en la lista desplegable. Quite el cuadro de texto y, a continuación, arrastre un control FileUpload desde el cuadro de herramientas a la plantilla. Establezca el `ID` del control FileUpload en `BrochureUpload`. Del mismo modo, agregue un control FileUpload al `Picture` TemplateField s `InsertItemTemplate`. Establezca esta `ID` de control FileUpload en `PictureUpload`.

[![agregar un control FileUpload a InsertItemTemplate](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image8.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image13.png)

**Figura 8**: agregar un control FileUpload al `InsertItemTemplate` ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image14.png))

Después de realizar estas adiciones, las dos sintaxis declarativa de TemplateField s serán:

[!code-aspx[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample5.aspx)]

Cuando un usuario agrega una nueva categoría, queremos asegurarnos de que el folleto y la imagen tienen el tipo de archivo correcto. En el folleto, el usuario debe proporcionar un PDF. En el caso de la imagen, necesitamos que el usuario cargue un archivo de imagen, pero *se permite el archivo de imagen o* solo los archivos de imagen de un tipo determinado, como gifs o jpgs. Para permitir diferentes tipos de archivo, es necesario extender el esquema de `Categories` para incluir una columna que capture el tipo de archivo para que este tipo se pueda enviar al cliente a través de `Response.ContentType` en `DisplayCategoryPicture.aspx`. Dado que no disponemos de una columna de este tipo, sería prudente restringir a los usuarios para que solo proporcionen un tipo de archivo de imagen específico. Las imágenes existentes de `Categories` tabla son mapas de bits, pero los JPGs son un formato de archivo más adecuado para las imágenes atendidas en Internet.

Si un usuario carga un tipo de archivo incorrecto, es necesario cancelar la inserción y mostrar un mensaje que indica el problema. Agregue un control Web de etiqueta debajo de DetailsView. Establezca su propiedad `ID` en `UploadWarning`, borre su `Text` propiedad, establezca la propiedad `CssClass` en Warning y las propiedades `Visible` y `EnableViewState` en `False`. La clase `Warning` CSS se define en `Styles.css` y representa el texto en una fuente grande, roja, cursiva y negrita.

> [!NOTE]
> Idealmente, el `CategoryName` y `Description` BoundFields se convertiría en TemplateFields y sus interfaces de inserción se personalizaron. Por ejemplo, la interfaz de inserción de `Description` podría ser más adecuada a través de un cuadro de texto de varias líneas. Y como la columna `CategoryName` no acepta valores de `NULL`, se debe agregar un RequiredFieldValidator para asegurarse de que el usuario proporciona un valor para el nuevo nombre de categoría. Estos pasos se dejan como un ejercicio para el lector. Consulte [Personalización de la interfaz de modificación de datos](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-vb.md) para obtener una visión detallada del aumento de las interfaces de modificación de datos.

## <a name="step-6-saving-the-uploaded-brochure-to-the-web-server-s-file-system"></a>Paso 6: guardar el folleto cargado en el sistema de archivos del servidor Web

Cuando el usuario escribe los valores para una nueva categoría y hace clic en el botón Insertar, se produce un postback y se despliega el flujo de trabajo de inserción. En primer lugar, se desencadena el evento DetailsView s [`ItemInserting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserting.aspx) . A continuación, se invoca el método ObjectDataSource s `Insert()`, lo que hace que se agregue un nuevo registro a la tabla `Categories`. Después de eso, se desencadena el evento DetailsView s [`ItemInserted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.iteminserted.aspx) .

Antes de que se invoque el método ObjectDataSource s `Insert()`, primero debemos asegurarnos de que el usuario cargó los tipos de archivo adecuados y, a continuación, guardar el PDF del folleto en el sistema de archivos del servidor Web. Cree un controlador de eventos para el evento de `ItemInserting` DetailsView s y agregue el código siguiente:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample6.vb)]

El controlador de eventos se inicia haciendo referencia al control de FileUpload `BrochureUpload` a partir de las plantillas de DetailsView s. Después, si se ha cargado un folleto, se examina la extensión de archivo cargada. Si la extensión no es. PDF, se muestra una advertencia, se cancela la inserción y finaliza la ejecución del controlador de eventos.

> [!NOTE]
> Confiar en la extensión de archivos cargados no es una técnica de prevención para asegurarse de que el archivo cargado es un documento PDF. El usuario podría tener un documento PDF válido con la extensión `.Brochure`o podría haber tomado un documento no PDF y le ha dado una extensión `.pdf`. El contenido binario del archivo s debe examinarse mediante programación para comprobar de forma más concluyente el tipo de archivo. Sin embargo, estos enfoques exhaustivos suelen ser exagerados; la comprobación de la extensión es suficiente para la mayoría de los escenarios.

Como se describe en el tutorial de [carga de archivos](uploading-files-vb.md) , se debe tener cuidado al guardar archivos en el sistema de archivos para que una carga de usuarios no sobrescriba otros. En este tutorial se intentará usar el mismo nombre que el archivo cargado. Sin embargo, si ya existe un archivo en el directorio `~/Brochures` con el mismo nombre de archivo, se anexará un número al final hasta que se encuentre un nombre único. Por ejemplo, si el usuario carga un archivo de folleto denominado `Meats.pdf`, pero ya existe un archivo denominado `Meats.pdf` en la carpeta `~/Brochures`, cambiaremos el nombre del archivo guardado a `Meats-1.pdf`. Si es así, intentaremos `Meats-2.pdf`, y así sucesivamente, hasta que se encuentre un nombre de archivo único.

En el código siguiente se usa el [método`File.Exists(path)`](https://msdn.microsoft.com/library/system.io.file.exists.aspx) para determinar si ya existe un archivo con el nombre de archivo especificado. Si es así, continúa intentando usar nuevos nombres de archivo para el folleto hasta que no se encuentre ningún conflicto.

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample7.vb)]

Una vez que se ha encontrado un nombre de archivo válido, el archivo debe guardarse en el sistema de archivos y el valor de ObjectDataSource s `brochurePath``InsertParameter` debe actualizarse para que este nombre de archivo se escriba en la base de datos. Como vimos en el tutorial de *carga de archivos* , el archivo se puede guardar con el método FileUpload control s `SaveAs(path)`. Para actualizar el parámetro ObjectDataSource s `brochurePath`, use la colección `e.Values`.

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample8.vb)]

## <a name="step-7-saving-the-uploaded-picture-to-the-database"></a>Paso 7: guardar la imagen cargada en la base de datos

Para almacenar la imagen cargada en el nuevo registro de `Categories`, es necesario asignar el contenido binario cargado al parámetro ObjectDataSource s `picture` en el evento `ItemInserting` de DetailsView. Sin embargo, antes de llevar a cabo esta asignación, debemos asegurarnos de que la imagen cargada sea un JPG y no otro tipo de imagen. Como en el paso 6, permita que use la extensión de archivo de imagen s cargada para determinar su tipo.

Mientras que la tabla `Categories` permite `NULL` valores de la columna de `Picture`, todas las categorías tienen una imagen actualmente. Permita que el usuario proporcione una imagen al agregar una nueva categoría a través de esta página. El código siguiente realiza una comprobación para asegurarse de que se ha cargado una imagen y de que tiene una extensión adecuada.

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample9.vb)]

Este código debe colocarse *antes* del código del paso 6 para que, si hay un problema con la carga de la imagen, el controlador de eventos finalice antes de que el archivo del folleto se guarde en el sistema de archivos.

Suponiendo que se haya cargado un archivo adecuado, asigne el contenido binario cargado al valor de parámetro de imagen con la siguiente línea de código:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample10.vb)]

## <a name="the-completeiteminsertingevent-handler"></a>El controlador de eventos`ItemInserting`completo

Por integridad, este es el controlador de eventos `ItemInserting` en su totalidad:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample11.vb)]

## <a name="step-8-fixing-thedisplaycategorypictureaspxpage"></a>Paso 8: corrección de la página`DisplayCategoryPicture.aspx`

Tómese un momento para probar la interfaz de inserción y `ItemInserting` controlador de eventos que se creó en los últimos pasos. Visite la página `UploadInDetailsView.aspx` a través de un explorador e intente agregar una categoría, pero omita la imagen o especifique una imagen no JPG o un folleto que no sea PDF. En cualquiera de estos casos, se mostrará un mensaje de error y se cancelará el flujo de trabajo de inserción.

[![se muestra un mensaje de advertencia si se carga un tipo de archivo no válido](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image9.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image15.png)

**Figura 9**: se muestra un mensaje de advertencia si se carga un tipo de archivo no válido ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image16.png))

Una vez que haya comprobado que la página requiere la carga de una imagen y no acepte archivos no PDF o no JPG, agregue una nueva categoría con una imagen JPG válida, y quede vacío el campo del folleto. Después de hacer clic en el botón Insertar, la página se devolverá y se agregará un nuevo registro a la tabla `Categories` con el contenido binario de imágenes cargadas que se almacenó directamente en la base de datos. GridView se actualiza y muestra una fila para la categoría recién agregada, pero, tal y como se muestra en la figura 10, la nueva imagen de la categoría s no se representa correctamente.

[![no se muestra la nueva imagen de la categoría s](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image10.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image17.png)

**Figura 10**: no se muestra la nueva imagen de la categoría s ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image18.png))

La razón por la que no se muestra la nueva imagen es que la página de `DisplayCategoryPicture.aspx` que devuelve la categoría s especificada se configura para procesar mapas de bits que tienen un encabezado OLE. Este encabezado de 78 bytes se elimina del contenido binario de la columna de `Picture` antes de que se devuelvan al cliente. Pero el archivo JPG que acaba de cargar para la nueva categoría no tiene este encabezado OLE. por lo tanto, los bytes necesarios y válidos se quitan de los datos binarios de la imagen.

Dado que ahora hay dos mapas de bits con encabezados OLE y JPGs en la tabla `Categories`, es necesario actualizar `DisplayCategoryPicture.aspx` para que se elimine el encabezado OLE de las ocho categorías originales y se omita esta eliminación para los registros de categoría más recientes. En el siguiente tutorial, veremos cómo actualizar una imagen de registro existente y actualizaremos todas las imágenes de la categoría anterior para que se JPGs. Por el momento, sin embargo, use el siguiente código en `DisplayCategoryPicture.aspx` para quitar los encabezados OLE solo de las ocho categorías originales:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample12.vb)]

Con este cambio, la imagen JPG se representa ahora correctamente en GridView.

[![las imágenes JPG de las nuevas categorías se representan correctamente](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image11.gif)](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image19.png)

**Figura 11**: las imágenes JPG de las nuevas categorías se representan correctamente ([haga clic para ver la imagen de tamaño completo](including-a-file-upload-option-when-adding-a-new-record-vb/_static/image20.png))

## <a name="step-9-deleting-the-brochure-in-the-face-of-an-exception"></a>Paso 9: eliminar el folleto en la superficie de una excepción

Uno de los desafíos de almacenar datos binarios en el sistema de archivos del servidor web es que presenta una desconexión entre el modelo de datos y sus datos binarios. Por lo tanto, cada vez que se elimina un registro, también se deben quitar los datos binarios correspondientes del sistema de archivos. Esto puede entrar en juego también al insertar. Considere el siguiente escenario: un usuario agrega una nueva categoría, especificando una imagen y un folleto válidos. Al hacer clic en el botón Insertar, se produce un postback y se desencadena el evento DetailsView s `ItemInserting`, guardando el folleto en el sistema de archivos del servidor Web. A continuación, se invoca el método ObjectDataSource s `Insert()`, que llama al método `CategoriesBLL` de clase `InsertWithPicture`, que llama al método `InsertWithPicture` `CategoriesTableAdapter` s.

Ahora, ¿qué ocurre si la base de datos está sin conexión o si hay un error en la instrucción SQL `INSERT`? Claramente se producirá un error en la inserción, por lo que no se agregará ninguna fila de categoría nueva a la base de datos. Pero todavía tenemos el archivo del folleto cargado en el sistema de archivos del servidor Web. Este archivo debe eliminarse ante una excepción durante el flujo de trabajo de inserción.

Como se explicó anteriormente en el tratamiento de las [excepciones de nivel BLL y Dal en un](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial de página de ASP.net, cuando se produce una excepción dentro de las profundidades de la arquitectura, se propaga a través de las distintas capas. En el nivel de presentación, podemos determinar si se ha producido una excepción desde el evento de `ItemInserted` DetailsView s. Este controlador de eventos también proporciona los valores de la `InsertParameters`de ObjectDataSource. Por lo tanto, se puede crear un controlador de eventos para el evento `ItemInserted` que comprueba si se ha producido una excepción y, en ese caso, elimina el archivo especificado por el parámetro de `brochurePath` de ObjectDataSource:

[!code-vb[Main](including-a-file-upload-option-when-adding-a-new-record-vb/samples/sample13.vb)]

## <a name="summary"></a>Resumen

Hay una serie de pasos que se deben realizar para proporcionar una interfaz basada en web para agregar registros que incluyan datos binarios. Si los datos binarios se almacenan directamente en la base de datos, lo más probable es que necesite actualizar la arquitectura, agregando métodos específicos para controlar el caso en el que se insertan los datos binarios. Una vez actualizada la arquitectura, el paso siguiente consiste en crear la interfaz de inserción, que se puede realizar mediante un control DetailsView que se ha personalizado para incluir un control FileUpload para cada campo de datos binarios. Los datos cargados se pueden guardar en el sistema de archivos del servidor web o asignarse a un parámetro de origen de datos en el controlador de eventos `ItemInserting` de DetailsView.

Guardar los datos binarios en el sistema de archivos requiere más planeación que guardar datos directamente en la base de datos. Se debe elegir un esquema de nomenclatura para evitar que una carga de usuarios sobrescriba otras. Además, se deben realizar pasos adicionales para eliminar el archivo cargado si se produce un error en la inserción de la base de datos.

Ahora tenemos la posibilidad de agregar nuevas categorías al sistema con un folleto e imagen, pero aún veremos cómo actualizar los datos binarios de una categoría existente o cómo quitar correctamente los datos binarios de una categoría eliminada. Exploraremos estos dos temas en el siguiente tutorial.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Dave Gardner, Teresa Murphy y Bernadette Leigh. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](displaying-binary-data-in-the-data-web-controls-vb.md)
> [Siguiente](updating-and-deleting-existing-binary-data-vb.md)
