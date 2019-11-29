---
uid: web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
title: Actualizar y eliminar datos binarios existentes (VB) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores vimos cómo el control GridView facilita la edición y eliminación de los datos de texto. En este tutorial, veremos cómo el control GridView también realiza...
ms.author: riande
ms.date: 03/27/2007
ms.assetid: 3a052ced-9cf5-47b8-a400-934f0b687c26
msc.legacyurl: /web-forms/overview/data-access/working-with-binary-files/updating-and-deleting-existing-binary-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 27ff6941008b4e7bf6d632e4c248fd1d35fb3589
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74621197"
---
# <a name="updating-and-deleting-existing-binary-data-vb"></a>Actualizar y eliminar los datos binarios existentes (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_57_VB.exe) de la aplicación de ejemplo o [descarga de PDF](updating-and-deleting-existing-binary-data-vb/_static/datatutorial57vb1.pdf)

> En los tutoriales anteriores vimos cómo el control GridView facilita la edición y eliminación de los datos de texto. En este tutorial, veremos cómo el control GridView también permite editar y eliminar datos binarios, tanto si los datos binarios se guardan en la base de datos como si se almacenan en el sistema de archivos.

## <a name="introduction"></a>Introducción

En los tres últimos tutoriales, hemos agregado bastante funcionalidad para trabajar con datos binarios. Comenzamos agregando una `BrochurePath` columna a la tabla de `Categories` y actualizamos la arquitectura en consecuencia. También hemos agregado los métodos capa de acceso a datos y capa de lógica de negocios para trabajar con la columna categorías de la tabla s existentes `Picture`, que contiene el contenido binario de un archivo de imagen. Hemos creado páginas web para presentar los datos binarios en un vínculo de descarga de GridView para el folleto, con la categoría s imagen mostrada en un elemento `<img>` y haber agregado DetailsView para permitir que los usuarios agreguen una nueva categoría y carguen sus datos de folletos e imágenes.

Todo lo que queda por implementar es la posibilidad de editar y eliminar las categorías existentes, que se llevarán a cabo en este tutorial con las características integradas de edición y eliminación de GridView. Al editar una categoría, el usuario podrá cargar opcionalmente una nueva imagen o hacer que la categoría siga usando la existente. En el folleto, pueden optar por usar el folleto existente, cargar un nuevo folleto o indicar que la categoría ya no tiene un folleto asociado. Vamos a empezar a trabajar.

## <a name="step-1-updating-the-data-access-layer"></a>Paso 1: actualizar la capa de acceso a datos

La capa DAL tiene métodos `Insert`, `Update`y `Delete` generados automáticamente, pero estos métodos se generaron basándose en la consulta principal `CategoriesTableAdapter` s, que no incluye la columna `Picture`. Por lo tanto, los métodos `Insert` y `Update` no incluyen parámetros para especificar los datos binarios de la categoría s imagen. Como hicimos en el [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-vb.md), es necesario crear un nuevo método TableAdapter para actualizar la tabla `Categories` al especificar datos binarios.

Abra el conjunto de los tipos y, en el diseñador, haga clic con el botón secundario en el encabezado `CategoriesTableAdapter` s y elija Agregar consulta en el menú contextual para iniciar el Asistente para la configuración de consultas de TableAdapter. Este asistente se inicia preguntando cómo debe tener acceso a la base de datos. Elija usar instrucciones SQL y haga clic en siguiente. El paso siguiente solicita el tipo de consulta que se va a generar. Dado que se va a crear una consulta para agregar un nuevo registro a la tabla de `Categories`, elija actualizar y haga clic en siguiente.

[![seleccionar la opción de actualización](updating-and-deleting-existing-binary-data-vb/_static/image2.png)](updating-and-deleting-existing-binary-data-vb/_static/image1.png)

**Figura 1**: seleccione la opción de actualización ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image3.png))

Ahora es necesario especificar el `UPDATE` instrucción SQL. El Asistente sugiere automáticamente una instrucción `UPDATE` correspondiente a la consulta principal de TableAdapter s (una que actualiza los valores `CategoryName`, `Description`y `BrochurePath`). Cambie la instrucción para que la columna `Picture` esté incluida junto con un parámetro `@Picture`, como por ejemplo:

[!code-sql[Main](updating-and-deleting-existing-binary-data-vb/samples/sample1.sql)]

La última pantalla del asistente nos pide asignar un nombre al nuevo método TableAdapter. Escriba `UpdateWithPicture` y haga clic en finalizar.

[![nombre al nuevo método TableAdapter UpdateWithPicture](updating-and-deleting-existing-binary-data-vb/_static/image5.png)](updating-and-deleting-existing-binary-data-vb/_static/image4.png)

**Figura 2**: asigne un nombre al nuevo método TableAdapter `UpdateWithPicture` ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image6.png))

## <a name="step-2-adding-the-business-logic-layer-methods"></a>Paso 2: agregar los métodos del nivel de lógica de negocios

Además de actualizar la capa DAL, es necesario actualizar la capa BLL para incluir los métodos para actualizar y eliminar una categoría. Estos son los métodos que se invocarán desde el nivel de presentación.

Para eliminar una categoría, podemos usar el método `Delete` generado automáticamente `CategoriesTableAdapter` s. Agregue el método siguiente a la clase `CategoriesBLL`:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample2.vb)]

En este tutorial, vamos a crear dos métodos para actualizar una categoría: uno que espera los datos de imagen binaria e invoca el método de `UpdateWithPicture` que se acaba de agregar a la `CategoriesTableAdapter` y otro que solo acepta los valores `CategoryName`, `Description`y `BrochurePath` y utiliza `CategoriesTableAdapter` instrucción `Update` generada automáticamente. La lógica que subyace en el uso de dos métodos es que, en algunas circunstancias, es posible que un usuario desee actualizar la imagen de categoría s junto con sus otros campos, en cuyo caso el usuario tendrá que cargar la nueva imagen. Los datos binarios de imagen s cargados se pueden usar en la instrucción `UPDATE`. En otros casos, es posible que el usuario solo esté interesado en actualizar, por ejemplo, el nombre y la descripción. Pero si la instrucción `UPDATE` espera también los datos binarios de la columna `Picture`, también es necesario proporcionar esa información. Esto requeriría un recorrido adicional a la base de datos para devolver los datos de imagen del registro que se está editando. Por lo tanto, queremos dos métodos `UPDATE`. El nivel de lógica de negocios determinará cuál se debe usar en función de si se proporcionan datos de imagen al actualizar la categoría.

Para facilitar esto, agregue dos métodos a la clase `CategoriesBLL`, ambos denominados `UpdateCategory`. El primero debe aceptar tres `String`, una matriz de `Byte` y una `Integer` como parámetros de entrada; la segunda, solo tres `String` s y una `Integer`. Los parámetros de entrada `String` son para el nombre, la descripción y la ruta de acceso del archivo del folleto, la matriz de `Byte` es para el contenido binario de la imagen de la categoría s y el `Integer` identifica el `CategoryID` del registro que se va a actualizar. Observe que la primera sobrecarga invoca el segundo si la matriz `Byte` pasada es `Nothing`:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample3.vb)]

## <a name="step-3-copying-over-the-insert-and-view-functionality"></a>Paso 3: copia de la funcionalidad de inserción y vista

En el [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-vb.md) , hemos creado una página denominada `UploadInDetailsView.aspx` que enumera todas las categorías de un control GridView y proporcionado DetailsView para agregar nuevas categorías al sistema. En este tutorial se extenderá el control GridView para incluir la compatibilidad con la edición y la eliminación. En lugar de continuar trabajando desde `UploadInDetailsView.aspx`, en lugar de ello, realice los cambios de este tutorial en la página `UpdatingAndDeleting.aspx` de la misma carpeta `~/BinaryData`. Copie y pegue el marcado y el código declarativos de `UploadInDetailsView.aspx` en `UpdatingAndDeleting.aspx`.

Para empezar, abra la página `UploadInDetailsView.aspx`. Copie toda la sintaxis declarativa dentro del elemento `<asp:Content>`, como se muestra en la figura 3. Después, abra `UpdatingAndDeleting.aspx` y pegue este marcado dentro de su elemento `<asp:Content>`. Del mismo modo, copie el código de la clase de código subyacente de la página `UploadInDetailsView.aspx` en `UpdatingAndDeleting.aspx`.

[![copiar el marcado declarativo de UploadInDetailsView. aspx](updating-and-deleting-existing-binary-data-vb/_static/image8.png)](updating-and-deleting-existing-binary-data-vb/_static/image7.png)

**Figura 3**: copia del marcado declarativo de `UploadInDetailsView.aspx` ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image9.png))

Después de copiar sobre el marcado y el código declarativos, visite `UpdatingAndDeleting.aspx`. Debería ver el mismo resultado y tener la misma experiencia del usuario que con `UploadInDetailsView.aspx` página del tutorial anterior.

## <a name="step-4-adding-deleting-support-to-the-objectdatasource-and-gridview"></a>Paso 4: agregar compatibilidad de eliminación a ObjectDataSource y GridView

Como hemos comentado en el tutorial Introducción a la [inserción, actualización y eliminación de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) , el control GridView proporciona capacidades de eliminación integradas y estas funcionalidades se pueden habilitar en el paso de una casilla si el origen de datos subyacente de la cuadrícula admite la eliminación. Actualmente, el ObjectDataSource al que está enlazado GridView (`CategoriesDataSource`) no admite la eliminación.

Para solucionarlo, haga clic en la opción Configurar origen de datos de la etiqueta inteligente de ObjectDataSource s para iniciar el asistente. La primera pantalla muestra que ObjectDataSource está configurado para funcionar con la clase `CategoriesBLL`. Presione siguiente. Actualmente, solo se especifican las propiedades ObjectDataSource `InsertMethod` y `SelectMethod`. Sin embargo, el asistente rellena automáticamente las listas desplegables de las pestañas actualizar y eliminar con los métodos `UpdateCategory` y `DeleteCategory`, respectivamente. Esto se debe a que en la `CategoriesBLL` clase hemos marcado estos métodos con la `DataObjectMethodAttribute` como los métodos predeterminados para actualizar y eliminar.

Por ahora, establezca la lista desplegable actualizar pestaña s en (ninguna), pero deje la lista desplegable eliminar pestaña s establecida en `DeleteCategory`. Volveremos a este asistente en el paso 6 para agregar compatibilidad de actualización.

[![configurar ObjectDataSource para usar el método DeleteCategory](updating-and-deleting-existing-binary-data-vb/_static/image11.png)](updating-and-deleting-existing-binary-data-vb/_static/image10.png)

**Figura 4**: configuración de ObjectDataSource para usar el método `DeleteCategory` ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image12.png))

> [!NOTE]
> Al finalizar el asistente, Visual Studio puede preguntar si desea actualizar los campos y las claves, lo que volverá a generar los campos de controles Web de datos. Elija no, porque si elige sí, se sobrescribirán las personalizaciones de campo que haya realizado.

ObjectDataSource incluirá ahora un valor para su propiedad `DeleteMethod`, así como un `DeleteParameter`. Recuerde que cuando se usa el Asistente para especificar los métodos, Visual Studio establece la propiedad ObjectDataSource s `OldValuesParameterFormatString` en `original_{0}`, lo que causa problemas con las invocaciones del método Update y DELETE. Por lo tanto, borre esta propiedad de forma conjunta o restablezca el valor predeterminado, `{0}`. Si necesita actualizar la memoria en esta propiedad ObjectDataSource, vea el tutorial [información general sobre la inserción, actualización y eliminación de datos](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md) .

Después de completar el asistente y de corregir el `OldValuesParameterFormatString`, el marcado declarativo de ObjectDataSource s debe ser similar al siguiente:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample4.aspx)]

Después de configurar ObjectDataSource, agregue funcionalidades de eliminación a GridView activando la casilla habilitar eliminación de la etiqueta inteligente de GridView s. Esto agregará un CommandField a GridView cuya propiedad `ShowDeleteButton` está establecida en `True`.

[![habilitar la compatibilidad para eliminar en GridView](updating-and-deleting-existing-binary-data-vb/_static/image14.png)](updating-and-deleting-existing-binary-data-vb/_static/image13.png)

**Figura 5**: habilitar la compatibilidad para eliminar en GridView ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image15.png))

Dedique un momento a probar la funcionalidad de eliminación. Hay una clave externa entre el `Products` Table s `CategoryID` y la `Categories` Table s `CategoryID`, por lo que obtendrá una excepción de infracción de restricción FOREIGN KEY si intenta eliminar cualquiera de las ocho primeras categorías. Para probar esta funcionalidad, agregue una nueva categoría y proporcione un folleto y una imagen. Mi categoría de prueba, que se muestra en la figura 6, incluye un archivo de folleto de prueba denominado `Test.pdf` y una imagen de prueba. La figura 7 muestra GridView una vez agregada la categoría de pruebas.

[![agregar una categoría de pruebas con un folleto e imagen](updating-and-deleting-existing-binary-data-vb/_static/image17.png)](updating-and-deleting-existing-binary-data-vb/_static/image16.png)

**Figura 6**: agregar una categoría de pruebas con un folleto e imagen ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image18.png))

[![después de insertar la categoría de pruebas, se muestra en GridView](updating-and-deleting-existing-binary-data-vb/_static/image20.png)](updating-and-deleting-existing-binary-data-vb/_static/image19.png)

**Figura 7**: después de insertar la categoría de pruebas, se muestra en GridView ([haga clic para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image21.png))

En Visual Studio, actualice el Explorador de soluciones. Ahora debería ver un nuevo archivo en la carpeta `~/Brochures`, `Test.pdf` (vea la figura 8).

A continuación, haga clic en el vínculo eliminar de la fila de la categoría de pruebas, lo que hará que la página se devuelva y que el método de `DeleteCategory` de `CategoriesBLL` clase se active. Esto invocará el método de `Delete` de la capa DAL, lo que hará que se envíe la instrucción `DELETE` adecuada a la base de datos. Los datos se vuelven a enlazar a GridView y el marcado se devuelve al cliente con la categoría de pruebas ya no está presente.

Mientras el flujo de trabajo de eliminación eliminó correctamente el registro de la categoría de pruebas de la tabla `Categories`, no quitó su archivo de folleto del sistema de archivos del servidor Web. Actualice el Explorador de soluciones y verá que `Test.pdf` todavía está en la carpeta `~/Brochures`.

![El archivo test. pdf no se eliminó del sistema de archivos del servidor Web](updating-and-deleting-existing-binary-data-vb/_static/image1.gif)

**Figura 8**: el archivo de `Test.pdf` no se eliminó del sistema de archivos del servidor Web

## <a name="step-5-removing-the-deleted-category-s-brochure-file"></a>Paso 5: quitar el archivo de folleto de categoría eliminado

Uno de los inconvenientes de almacenar datos binarios externos a la base de datos es que se deben realizar pasos adicionales para limpiar estos archivos cuando se elimina el registro de base de datos asociado. GridView y ObjectDataSource proporcionan eventos que se activan antes y después de que se haya realizado el comando DELETE. En realidad, es necesario crear controladores de eventos para los eventos anteriores y posteriores a la acción. Antes de que se elimine el registro de `Categories`, es necesario determinar su ruta de acceso de archivo PDF, pero no queremos eliminar el PDF antes de que se elimine la categoría en caso de que haya alguna excepción y la categoría no se elimine.

El evento GridView s [`RowDeleting`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleting.aspx) se desencadena antes de que se haya invocado el comando ObjectDataSource s Delete, mientras que su [evento`RowDeleted`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.rowdeleted.aspx) se activa después de. Cree controladores de eventos para estos dos eventos mediante el código siguiente:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample5.vb)]

En el controlador de eventos `RowDeleting`, el `CategoryID` de la fila que se va a eliminar se obtiene de la colección GridView s `DataKeys`, a la que se puede tener acceso en este controlador de eventos a través de la colección de `e.Keys`. A continuación, se invoca la clase `CategoriesBLL` s `GetCategoryByCategoryID(categoryID)` para devolver información sobre el registro que se va a eliminar. Si el objeto de `CategoriesDataRow` devuelto tiene un valor que no es`NULL``BrochurePath`, se almacena en la variable de página `deletedCategorysPdfPath` de modo que el archivo se pueda eliminar en el controlador de eventos `RowDeleted`.

> [!NOTE]
> En lugar de recuperar los detalles del `BrochurePath` para el registro de `Categories` que se va a eliminar en el controlador de eventos de `RowDeleting`, podríamos haber agregado el `BrochurePath` a la propiedad `DataKeyNames` de GridView y haber tenido acceso al valor record s a través de la colección `e.Keys`. Si lo hace, se aumenta ligeramente el tamaño del estado de vista de GridView, pero se reduce la cantidad de código necesario y se guarda un viaje en la base de datos.

Una vez invocado el comando de eliminación de ObjectDataSource s subyacente, se activa el controlador de eventos GridView s `RowDeleted`. Si no hay ninguna excepción al eliminar los datos y hay un valor para `deletedCategorysPdfPath`, el PDF se elimina del sistema de archivos. Tenga en cuenta que este código adicional no es necesario para limpiar los datos binarios de la categoría asociados a su imagen. Esto se debe a que los datos de la imagen se almacenan directamente en la base de datos, por lo que la eliminación de la fila de `Categories` también elimina los datos de la imagen de la categoría.

Después de agregar los dos controladores de eventos, vuelva a ejecutar este caso de prueba. Al eliminar la categoría, también se elimina su PDF asociado.

La actualización de datos binarios asociados a registros existentes proporciona algunos desafíos interesantes. En el resto de este tutorial se profundiza en agregar funcionalidades de actualización al folleto y a la imagen. En el paso 6 se exploran las técnicas para actualizar la información del folleto mientras el paso 7 examina la actualización de la imagen.

## <a name="step-6-updating-a-category-s-brochure"></a>Paso 6: actualización de un folleto de categoría

Como se describe en el tutorial de introducción a la inserción, actualización y eliminación de datos, GridView ofrece compatibilidad de edición de nivel de fila integrada que se puede implementar mediante el paso de una casilla si su origen de datos subyacente está configurado correctamente. Actualmente, el `CategoriesDataSource` ObjectDataSource todavía no está configurado para incluir soporte de actualización, por lo que debe agregarlo.

Haga clic en el vínculo configurar origen de datos del asistente de ObjectDataSource s y continúe con el segundo paso. Debido a la `DataObjectMethodAttribute` utilizada en `CategoriesBLL`, la lista desplegable actualizar se debe rellenar automáticamente con la sobrecarga de `UpdateCategory` que acepta cuatro parámetros de entrada (para todas las columnas, pero `Picture`). Cámbielo para que use la sobrecarga con cinco parámetros.

[![configurar ObjectDataSource para usar el método UpdateCategory que incluye un parámetro para la imagen](updating-and-deleting-existing-binary-data-vb/_static/image23.png)](updating-and-deleting-existing-binary-data-vb/_static/image22.png)

**Figura 9**: configuración de ObjectDataSource para usar el método de `UpdateCategory` que incluye un parámetro para `Picture` ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image24.png))

ObjectDataSource incluirá ahora un valor para su propiedad `UpdateMethod`, así como los `UpdateParameter` correspondientes. Como se indicó en el paso 4, Visual Studio establece la propiedad ObjectDataSource s `OldValuesParameterFormatString` en `original_{0}` al usar el Asistente para configurar orígenes de datos. Esto causará problemas con las invocaciones del método Update y DELETE. Por lo tanto, borre esta propiedad de forma conjunta o restablezca el valor predeterminado, `{0}`.

Después de completar el asistente y de corregir el `OldValuesParameterFormatString`, el marcado declarativo de ObjectDataSource s debería tener un aspecto similar al siguiente:

[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample6.aspx)]

Para activar las características de edición integradas de GridView, active la opción Habilitar edición de la etiqueta inteligente de GridView s. Esto establecerá la propiedad CommandField s `ShowEditButton` en `True`, lo que dará como resultado la adición de un botón Editar (y los botones actualizar y cancelar de la fila que se está editando).

[![configurar GridView para que admita la edición](updating-and-deleting-existing-binary-data-vb/_static/image26.png)](updating-and-deleting-existing-binary-data-vb/_static/image25.png)

**Figura 10**: configuración de GridView para admitir la edición ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image27.png))

Visite la página a través de un explorador y haga clic en uno de los botones de edición Row s. `CategoryName` y `Description` BoundFields se representan como cuadros de texto. El `BrochurePath` TemplateField carece de un `EditItemTemplate`, por lo que sigue mostrando su `ItemTemplate` un vínculo al folleto. El `Picture` ImageField se representa como un cuadro de texto cuya propiedad `Text` tiene asignado el valor del `DataImageUrlField` ImageField s, en este caso `CategoryID`.

[![GridView carece de una interfaz de edición para BrochurePath](updating-and-deleting-existing-binary-data-vb/_static/image29.png)](updating-and-deleting-existing-binary-data-vb/_static/image28.png)

**Figura 11**: el control GridView carece de una interfaz de edición para `BrochurePath` ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image30.png))

## <a name="customizing-thebrochurepaths-editing-interface"></a>Personalización de la interfaz de edición de`BrochurePath`s

Necesitamos crear una interfaz de edición para la `BrochurePath` TemplateField, una que permite al usuario:

- Dejar la categoría s folleto tal cual,
- Actualice el folleto de la categoría mediante la carga de un nuevo folleto o
- Quite todo el folleto de la categoría (en el caso de que la categoría ya no tenga un folleto asociado).

También necesitamos actualizar la interfaz de edición de `Picture` ImageField s, pero lo haremos en el paso 7.

En la etiqueta inteligente de GridView s, haga clic en el vínculo editar plantillas y seleccione la `BrochurePath` TemplateField s `EditItemTemplate` en la lista desplegable. Agregue un control Web RadioButtonList a esta plantilla, estableciendo su propiedad `ID` en `BrochureOptions` y su propiedad `AutoPostBack` en `True`. En el ventana Propiedades, haga clic en los puntos suspensivos de la propiedad `Items`, que abrirá el editor de la colección `ListItem`. Agregue las tres opciones siguientes con `Value` s 1, 2 y 3, respectivamente:

- Usar el folleto actual
- Quitar el folleto actual
- Cargar nuevo folleto

Establezca la propiedad First `ListItem` s `Selected` en `True`.

![Agregue tres ListItems a RadioButtonList](updating-and-deleting-existing-binary-data-vb/_static/image2.gif)

**Figura 12**: adición de tres `ListItem` s al RadioButtonList

Debajo de RadioButtonList, agregue un control FileUpload denominado `BrochureUpload`. Establezca su propiedad `Visible` en `False`.

[![agregar un control RadioButtonList y FileUpload a la EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image32.png)](updating-and-deleting-existing-binary-data-vb/_static/image31.png)

**Figura 13**: agregar un control RadioButtonList y FileUpload al `EditItemTemplate` ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image33.png))

Este RadioButtonList proporciona las tres opciones para el usuario. La idea es que el control FileUpload solo se mostrará si se selecciona la última opción cargar nuevo folleto. Para ello, cree un controlador de eventos para el evento RadioButtonList s `SelectedIndexChanged` y agregue el código siguiente:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample7.vb)]

Dado que los controles RadioButtonList y FileUpload están dentro de una plantilla, es necesario escribir un poco de código para tener acceso a estos controles mediante programación. Se pasa al controlador de eventos `SelectedIndexChanged` una referencia de RadioButtonList en el parámetro de entrada `sender`. Para obtener el control FileUpload, necesitamos obtener el control primario RadioButtonList y usar el método `FindControl("controlID")` desde allí. Una vez que tenemos una referencia a los controles RadioButtonList y FileUpload, la propiedad FileUpload control s `Visible` está establecida en `True` solo si RadioButtonList s `SelectedValue` es igual a 3, que es el `Value` para el `ListItem`cargar nuevo folleto.

Con este código en su lugar, dedique un momento a probar la interfaz de edición. Haga clic en el botón Editar de una fila. Inicialmente, se debe seleccionar la opción usar folleto actual. Al cambiar el índice seleccionado, se produce un postback. Si se selecciona la tercera opción, se muestra el control FileUpload; de lo contrario, está oculto. La figura 14 muestra la interfaz de edición cuando se hace clic en el botón Editar por primera vez. En la figura 15 se muestra la interfaz después de seleccionar la opción cargar nuevo folleto.

[![inicialmente, se selecciona la opción usar folleto actual.](updating-and-deleting-existing-binary-data-vb/_static/image35.png)](updating-and-deleting-existing-binary-data-vb/_static/image34.png)

**Figura 14**: inicialmente, la opción usar folleto actual está seleccionada ([haga clic para ver la imagen a tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image36.png))

[![elegir la opción cargar nuevo folleto muestra el control FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image38.png)](updating-and-deleting-existing-binary-data-vb/_static/image37.png)

**Figura 15**: selección de la opción cargar nuevo folleto muestra el control FileUpload ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image39.png))

## <a name="saving-the-brochure-file-and-updating-thebrochurepathcolumn"></a>Guardar el archivo del folleto y actualizar el`BrochurePath`columna

Cuando se hace clic en el botón actualizar de GridView, se desencadena su evento `RowUpdating`. Se invoca el comando ObjectDataSource s Update y, a continuación, se desencadena el evento GridView s `RowUpdated`. Al igual que con el flujo de trabajo de eliminación, es necesario crear controladores de eventos para ambos eventos. En el controlador de eventos `RowUpdating`, es necesario determinar qué acción se debe realizar en función del `SelectedValue` de la `BrochureOptions` RadioButtonList:

- Si el `SelectedValue` es 1, queremos seguir usando la misma configuración de `BrochurePath`. Por lo tanto, es necesario establecer el parámetro ObjectDataSource s `brochurePath` en el valor de `BrochurePath` existente del registro que se está actualizando. El parámetro ObjectDataSource s `brochurePath` se puede establecer mediante `e.NewValues["brochurePath"] = value`.
- Si el `SelectedValue` es 2, queremos establecer el valor record s `BrochurePath` en `NULL`. Esto se puede lograr estableciendo el parámetro de `brochurePath` ObjectDataSource en `Nothing`, lo que hace que se use una base de datos `NULL` en la instrucción `UPDATE`. Si hay un archivo de folleto existente que se está quitando, es necesario eliminar el archivo existente. Sin embargo, solo queremos hacer esto si la actualización se completa sin generar una excepción.
- Si el `SelectedValue` es 3, queremos asegurarnos de que el usuario haya cargado un archivo PDF y, a continuación, guardarlo en el sistema de archivos y actualizar el valor de la columna record s `BrochurePath`. Además, si hay un archivo de folleto existente que se está reemplazando, es necesario eliminar el archivo anterior. Sin embargo, solo queremos hacer esto si la actualización se completa sin generar una excepción.

Los pasos necesarios para completarse cuando el `SelectedValue` RadioButtonList es 3 son prácticamente idénticos a los usados por el controlador de eventos `ItemInserting` de DetailsView. Este controlador de eventos se ejecuta cuando se agrega un nuevo registro de categoría desde el control DetailsView que hemos agregado en el [tutorial anterior](including-a-file-upload-option-when-adding-a-new-record-vb.md). Por lo tanto, nos ayuda a refactorizar esta funcionalidad en métodos independientes. En concreto, he sacado la funcionalidad común a dos métodos:

- `ProcessBrochureUpload(FileUpload, out bool)` acepta como entrada una instancia del control FileUpload y un valor booleano de salida que especifica si la operación de eliminación o edición debe continuar o si se debe cancelar debido a un error de validación. Este método devuelve la ruta de acceso al archivo guardado o `null` si no se guardó ningún archivo.
- `DeleteRememberedBrochurePath` elimina el archivo especificado por la ruta de acceso en la variable de página `deletedCategorysPdfPath` si no se `null``deletedCategorysPdfPath`.

A continuación se muestra el código de estos dos métodos. Observe la similitud entre `ProcessBrochureUpload` y el controlador de eventos de `ItemInserting` de DetailsView en el tutorial anterior. En este tutorial, he actualizado los controladores de eventos de DetailsView para usar estos nuevos métodos. Descargue el código asociado a este tutorial para ver las modificaciones de los controladores de eventos de DetailsView.

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample8.vb)]

Los controladores de eventos `RowUpdating` y `RowUpdated` de GridView usan los métodos `ProcessBrochureUpload` y `DeleteRememberedBrochurePath`, como se muestra en el código siguiente:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample9.vb)]

Observe cómo el controlador de eventos `RowUpdating` usa una serie de instrucciones condicionales para realizar la acción adecuada según el valor de la propiedad `BrochureOptions` RadioButtonList s `SelectedValue`.

Con este código en su lugar, puede editar una categoría y hacer que use su folleto actual, no usar ningún folleto o cargar uno nuevo. Continúe y pruébelo. Establezca puntos de interrupción en los controladores de eventos `RowUpdating` y `RowUpdated` para obtener una idea del flujo de trabajo.

## <a name="step-7-uploading-a-new-picture"></a>Paso 7: cargar una nueva imagen

La interfaz de edición de `Picture` ImageField s se representa como un cuadro de texto rellenado con el valor de su propiedad `DataImageUrlField`. Durante el flujo de trabajo de edición, GridView pasa un parámetro a ObjectDataSource con el parámetro s Name, el valor de la propiedad ImageField s `DataImageUrlField` y el parámetro s Value el valor especificado en el cuadro de texto en la interfaz de edición. Este comportamiento es adecuado cuando la imagen se guarda como un archivo en el sistema de archivos y el `DataImageUrlField` contiene la dirección URL completa de la imagen. Con tales circunstancias, la interfaz de edición muestra la dirección URL de la imagen en el cuadro de texto, que el usuario puede cambiar y se ha guardado en la base de datos. Concedidos, esta interfaz predeterminada no permite al usuario cargar una nueva imagen, pero permite cambiar la dirección URL de la imagen del valor actual a otro. En este tutorial, sin embargo, la interfaz de edición predeterminada de ImageField s no es suficiente porque la `Picture` datos binarios se almacenan directamente en la base de datos y la propiedad `DataImageUrlField` solo contiene la `CategoryID`.

Para comprender mejor lo que sucede en nuestro tutorial cuando un usuario edita una fila con un ImageField, considere el ejemplo siguiente: un usuario edita una fila con `CategoryID` 10, lo que hace que el `Picture` ImageField se represente como un cuadro de texto con el valor 10. Imagine que el usuario cambia el valor de este cuadro de texto a 50 y hace clic en el botón actualizar. Se produce un postback y GridView crea inicialmente un parámetro denominado `CategoryID` con el valor 50. Sin embargo, antes de que GridView envíe este parámetro (y los parámetros `CategoryName` y `Description`), agrega los valores de la colección `DataKeys`. Por lo tanto, sobrescribe el parámetro `CategoryID` con el valor de `CategoryID` subyacente de la fila actual, 10. En Resumen, la interfaz de edición de ImageField s no tiene ningún efecto en el flujo de trabajo de edición de este tutorial, ya que los nombres de las propiedades de `DataImageUrlField` ImageField y el valor de Grid s `DataKey` son uno de los mismos.

Aunque ImageField facilita la visualización de una imagen basada en datos de base de datos, no queremos proporcionar un cuadro de texto en la interfaz de edición. En su lugar, queremos ofrecer un control FileUpload que el usuario final pueda usar para cambiar la categoría s Picture. A diferencia del valor de `BrochurePath`, para estos tutoriales, se decidió requerir que cada categoría deba tener una imagen. Por lo tanto, no es necesario dejar que el usuario indique que no hay ninguna imagen asociada, por lo que el usuario puede cargar una nueva imagen o dejar la imagen actual tal cual.

Para personalizar la interfaz de edición de ImageField s, es necesario convertirla en TemplateField. En la etiqueta inteligente de GridView s, haga clic en el vínculo editar columnas, seleccione el ImageField y haga clic en el vínculo convertir este campo en TemplateField.

![Convertir ImageField en TemplateField](updating-and-deleting-existing-binary-data-vb/_static/image3.gif)

**Figura 16**: conversión de ImageField en TemplateField

Al convertir ImageField en TemplateField de esta manera, se genera un TemplateField con dos plantillas. Como se muestra en la sintaxis declarativa siguiente, el `ItemTemplate` contiene un control Web Image cuya propiedad `ImageUrl` se asigna mediante la sintaxis de DataBinding basada en las propiedades `DataImageUrlField` y `DataImageUrlFormatString` de ImageField s. El `EditItemTemplate` contiene un cuadro de texto cuya propiedad `Text` se enlaza al valor especificado por la propiedad `DataImageUrlField`.

[!code-aspx[Main](updating-and-deleting-existing-binary-data-vb/samples/sample10.aspx)]

Necesitamos actualizar el `EditItemTemplate` para utilizar un control FileUpload. En la etiqueta inteligente de GridView s, haga clic en el vínculo editar plantillas y, a continuación, seleccione la `Picture` TemplateField s `EditItemTemplate` en la lista desplegable. En la plantilla debería ver un cuadro de texto para quitarlo. A continuación, arrastre un control FileUpload desde el cuadro de herramientas a la plantilla, estableciendo su `ID` en `PictureUpload`. Agregue también el texto para cambiar la categoría s imagen y especifique una nueva imagen. Para mantener la misma categoría de imagen, deje el campo vacío en la plantilla.

[![agregar un control FileUpload a EditItemTemplate](updating-and-deleting-existing-binary-data-vb/_static/image41.png)](updating-and-deleting-existing-binary-data-vb/_static/image40.png)

**Figura 17**: agregar un control FileUpload al `EditItemTemplate` ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image42.png))

Después de personalizar la interfaz de edición, vea el progreso en un explorador. Al ver una fila en modo de solo lectura, la imagen de la categoría s se muestra como antes, pero al hacer clic en el botón Editar se representa la columna de imagen como texto con un control FileUpload.

[![la interfaz de edición incluye un control FileUpload](updating-and-deleting-existing-binary-data-vb/_static/image44.png)](updating-and-deleting-existing-binary-data-vb/_static/image43.png)

**Figura 18**: la interfaz de edición incluye un control FileUpload ([haga clic para ver la imagen de tamaño completo](updating-and-deleting-existing-binary-data-vb/_static/image45.png))

Recuerde que ObjectDataSource está configurado para llamar al método `CategoriesBLL` Class s `UpdateCategory` que acepta como entrada los datos binarios de la imagen como una matriz de `Byte`. Sin embargo, si esta matriz es `Nothing`, se llama a la sobrecarga de `UpdateCategory` alternativa, que emite la instrucción `UPDATE` SQL que no modifica la columna de `Picture`, lo que deja intacta la imagen de la categoría s Current. Por lo tanto, en el controlador de eventos GridView s `RowUpdating` es necesario hacer referencia mediante programación al control FileUpload `PictureUpload` y determinar si se ha cargado un archivo. Si no se cargó ninguno, *no queremos especificar* un valor para el parámetro `picture`. Por otro lado, si se cargó un archivo en el control FileUpload `PictureUpload`, queremos asegurarnos de que es un archivo JPG. Si es así, podemos enviar su contenido binario a ObjectDataSource a través del parámetro `picture`.

Al igual que con el código que se usa en el paso 6, gran parte del código que se necesita aquí ya existe en el controlador de eventos `ItemInserting` de DetailsView. Por lo tanto, he refactorizado la funcionalidad común en un nuevo método, `ValidPictureUpload`y actualizado el controlador de eventos `ItemInserting` para usar este método.

Agregue el código siguiente al inicio del controlador de eventos de `RowUpdating` GridView s. Es importante que este código venga antes del código que guarda el archivo del folleto, ya que no deseamos guardar el folleto en el sistema de archivos del servidor Web si se carga un archivo de imagen no válido.

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample11.vb)]

El método `ValidPictureUpload(FileUpload)` toma un control FileUpload como su único parámetro de entrada y comprueba la extensión s cargada del archivo para asegurarse de que el archivo cargado es un JPG. solo se llama si se carga un archivo de imagen. Si no se carga ningún archivo, no se establece el parámetro de imagen y, por tanto, se usa el valor predeterminado de `Nothing`. Si se ha cargado una imagen y `ValidPictureUpload` devuelve `True`, al parámetro `picture` se le asignan los datos binarios de la imagen cargada; Si el método devuelve `False`, el flujo de trabajo de actualización se cancela y se sale del controlador de eventos.

El código del método `ValidPictureUpload(FileUpload)`, que se ha refactorizado desde el controlador de eventos de `ItemInserting` DetailsView, sigue:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample12.vb)]

## <a name="step-8-replacing-the-original-categories-pictures-with-jpgs"></a>Paso 8: sustitución de las imágenes de las categorías originales por JPGs

Recuerde que las imágenes de ocho categorías originales son archivos de mapa de bits encapsulados en un encabezado OLE. Ahora que hemos agregado la capacidad de editar una imagen de registro existente, dedique un momento a reemplazar estos mapas de bits por JPGs. Si desea seguir usando las imágenes de la categoría actual, puede convertirlas en JPGs realizando los pasos siguientes:

1. Guarde las imágenes de mapa de bits en el disco duro. Visite la página `UpdatingAndDeleting.aspx` del explorador y, para cada una de las primeras ocho categorías, haga clic con el botón secundario en la imagen y elija guardar la imagen.
2. Abra la imagen en el editor de imágenes que prefiera. Por ejemplo, puede usar Microsoft Paint.
3. Guarde el mapa de bits como imagen JPG.
4. Actualice la imagen de la categoría s a través de la interfaz de edición mediante el archivo JPG.

Después de editar una categoría y cargar la imagen JPG, la imagen no se representará en el explorador porque la página `DisplayCategoryPicture.aspx` está quitando los primeros 78 bytes de las imágenes de las ocho primeras categorías. Para solucionarlo, quite el código que realiza la eliminación del encabezado OLE. Después de hacerlo, el controlador de eventos `DisplayCategoryPicture.aspx``Page_Load` debe tener solo el código siguiente:

[!code-vb[Main](updating-and-deleting-existing-binary-data-vb/samples/sample13.vb)]

> [!NOTE]
> En la página de `UpdatingAndDeleting.aspx`, las interfaces de inserción y edición pueden usar un poco más de trabajo. Los `CategoryName` y `Description` BoundFields en DetailsView y GridView deben convertirse en TemplateFields. Dado que `CategoryName` no permite valores de `NULL`, se debe agregar un RequiredFieldValidator. Y es probable que el cuadro de texto `Description` se convierta en un cuadro de texto de varias líneas. Dejo estos toques finales como un ejercicio.

## <a name="summary"></a>Resumen

En este tutorial se completa el trabajo con datos binarios. En este tutorial y en los tres anteriores, vimos cómo los datos binarios se pueden almacenar en el sistema de archivos o directamente en la base de datos. Un usuario proporciona datos binarios al sistema seleccionando un archivo de su disco duro y cárguelo en el servidor Web, donde puede almacenarse en el sistema de archivos o insertarse en la base de datos. ASP.NET 2,0 incluye un control FileUpload que facilita la tarea de arrastrar y colocar. Sin embargo, como se indicó en el tutorial de [carga de archivos](uploading-files-vb.md) , el control FileUpload solo es adecuado para cargas de archivos relativamente pequeñas, idealmente no superar un megabyte. También se ha explorado cómo asociar datos cargados con el modelo de datos subyacente, así como la forma de editar y eliminar los datos binarios de los registros existentes.

El siguiente conjunto de tutoriales explora diversas técnicas de almacenamiento en caché. El almacenamiento en caché proporciona un medio para mejorar el rendimiento general de una aplicación tomando los resultados de operaciones costosas y almacenándolos en una ubicación a la que se puede obtener acceso con mayor rapidez.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial era Teresa Murphy. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](including-a-file-upload-option-when-adding-a-new-record-vb.md)
