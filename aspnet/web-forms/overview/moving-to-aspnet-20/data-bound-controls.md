---
uid: web-forms/overview/moving-to-aspnet-20/data-bound-controls
title: Controles enlazados a datos | Microsoft Docs
author: microsoft
description: La mayoría de las aplicaciones ASP.NET se basan en cierto grado de presentación de datos de un origen de datos back-end. Los controles enlazados a datos han sido una parte dinámica de la interacción con...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 0e23ff32-646d-43f3-8bec-6b2313d3abd6
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-bound-controls
msc.type: authoredcontent
ms.openlocfilehash: 1154b38e0fa3d9d56cb407ae659c3b0d69871fda
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78488875"
---
# <a name="data-bound-controls"></a>Controles enlazados a datos

por [Microsoft](https://github.com/microsoft)

> La mayoría de las aplicaciones ASP.NET se basan en cierto grado de presentación de datos de un origen de datos back-end. Los controles enlazados a datos han sido una parte dinámica de la interacción con los datos en aplicaciones web dinámicas. ASP.NET 2,0 introduce algunas mejoras sustanciales en los controles enlazados a datos, incluida una nueva clase BaseDataBoundControl y una sintaxis declarativa.

La mayoría de las aplicaciones ASP.NET se basan en cierto grado de presentación de datos de un origen de datos back-end. Los controles enlazados a datos han sido una parte dinámica de la interacción con los datos en aplicaciones web dinámicas. ASP.NET 2,0 introduce algunas mejoras sustanciales en los controles enlazados a datos, incluida una nueva clase BaseDataBoundControl y una sintaxis declarativa.

BaseDataBoundControl actúa como la clase base para la clase DataBoundControl y la clase HierarchicalDataBoundControl. En este módulo, trataremos las siguientes clases que se derivan de DataBoundControl:

- AdRotator
- Controles de lista
- GridView
- FormView
- DetailsView

También analizaremos las siguientes clases que derivan de la clase HierarchicalDataBoundControl:

- TreeView
- Menú
- SiteMapPath

## <a name="databoundcontrol-class"></a>Clase DataBoundControl

La clase DataBoundControl es una clase abstracta (marcada como MustInherit en VB) que se usa para interactuar con datos tabulares o de estilo de lista. Los controles siguientes son algunos de los controles que derivan de DataBoundControl.

## <a name="adrotator"></a>AdRotator

El control AdRotator permite mostrar un banner gráfico en una página web que está vinculada a una dirección URL específica. El gráfico que se muestra se gira con las propiedades del control. La frecuencia de una presentación de anuncios determinada en una página se puede configurar mediante la propiedad **impresss** y los anuncios se pueden filtrar mediante el filtrado de palabras clave.

Los controles AdRotator utilizan un archivo XML o una tabla de una base de datos para los datos. Los atributos siguientes se utilizan en los archivos XML para configurar el control AdRotator.

### <a name="imageurl"></a>ImageUrl
Dirección URL de una imagen que se va a mostrar para el anuncio.

### <a name="navigateurl"></a>NavigateUrl
La dirección URL a la que se debe tener al usuario al hacer clic en el anuncio. Debe ser la dirección URL codificada.

### <a name="alternatetext"></a>AlternateText
Texto alternativo que se muestra en una información sobre herramientas y que leen los lectores de pantalla. También se muestra cuando la imagen especificada por ImageUrl no está disponible.

### <a name="keyword"></a>Palabra clave
Define una palabra clave que se puede usar cuando se usa el filtrado de palabras clave. Si se especifica, solo se mostrarán los anuncios con una palabra clave que coincida con el filtro de palabras clave.

### <a name="impressions"></a>Impresiones
Un número de ponderación que determina la frecuencia con la que es probable que aparezca un anuncio determinado. Es relativo a la impresión de otros anuncios en el mismo archivo. El valor máximo de las impresiones colectivas de todos los anuncios en un archivo XML es 2,048 mil millones 1.

### <a name="height"></a>Alto
Alto del anuncio en píxeles.

### <a name="width"></a>Ancho
Ancho del anuncio en píxeles.

> [!NOTE]
> Los atributos de alto y ancho invalidan el alto y el ancho del propio control AdRotator.

Un archivo XML típico podría ser similar al siguiente:

[!code-xml[Main](data-bound-controls/samples/sample1.xml)]

En el ejemplo anterior, el anuncio de Contoso es dos veces el que es probable que aparezca como AD para el sitio Web ASP.NET debido al valor del atributo impresss.

Para mostrar los anuncios del archivo XML anterior, agregue un control AdRotator a una página y establezca la propiedad **AdvertisementFile** para que apunte al archivo XML tal y como se muestra a continuación:

[!code-aspx[Main](data-bound-controls/samples/sample2.aspx)]

Si decide utilizar una tabla de base de datos como origen de datos para el control adrotate, primero deberá configurar una base de datos con el siguiente esquema:

| **Nombre de la columna** | **Tipo de datos** | **Descripción** |
| --- | --- | --- |
| Id. | int | Clave principal. Esta columna puede tener cualquier nombre. |
| ImageUrl | nvarchar (*longitud*) | Dirección URL relativa o absoluta de la imagen que se va a mostrar para el anuncio. |
| NavigateUrl | nvarchar (*longitud*) | La dirección URL de destino para el anuncio. Si no proporciona un valor, el anuncio no es un hipervínculo. |
| AlternateText | nvarchar (*longitud*) | Texto que se muestra si no se puede encontrar la imagen. En algunos exploradores, el texto se muestra como información sobre herramientas. El texto alternativo también se utiliza para la accesibilidad, de modo que los usuarios que no puedan ver el gráfico puedan oír su descripción en voz alta. |
| Palabra clave | nvarchar (*longitud*) | Categoría del anuncio en el que se puede filtrar la página. |
| Impresiones | int (4) | Número que indica la probabilidad de la frecuencia con la que se muestra el anuncio. Cuanto mayor sea el número, más a menudo se mostrará el anuncio. El total de todos los valores de impresiones del archivo XML no puede superar 2,048 mil millones-1. |
| Ancho | int (4) | Ancho de la imagen en píxeles. |
| Alto | int (4) | Alto de la imagen en píxeles. |

En los casos en los que ya tenga una base de datos con un esquema diferente, puede usar las propiedades **AlternateTextField**, **ImageUrlField**y **NavigateUrlField** para asignar los atributos adrotables a la base de datos existente. Para mostrar los datos de la base de datos en el control AdRotator, agregue un control de origen de datos a la página, configure la cadena de conexión para que el control de origen de datos apunte a la base de datos y establezca la propiedad **DataSourceID** del control AdRotator en el identificador del control de origen de datos. En los casos en los que tenga que configurar los anuncios de AdRotator mediante programación, use el evento AdCreated. El evento AdCreated toma dos parámetros: un objeto y el otro, una instancia de AdCreatedEventArgs. AdCreatedEventArgs es una referencia al anuncio que se va a crear.

El fragmento de código siguiente establece ImageUrl, NavigateUrl y AlternateText para un anuncio mediante programación:

[!code-csharp[Main](data-bound-controls/samples/sample3.cs)]

## <a name="list-controls"></a>Controles de lista

Los controles de lista incluyen ListBox, DropDownList, CheckBoxList, RadioButtonList y BulletedList. Cada uno de estos controles puede ser datos enlazados a un origen de datos. Utilizan un campo en el origen de datos como texto para mostrar y, opcionalmente, usan un segundo campo como valor de un elemento. Los elementos también se pueden agregar de forma estática en tiempo de diseño y se pueden mezclar elementos estáticos y elementos dinámicos agregados desde un origen de datos.

Para enlazar datos a un control de lista, agregue un control de origen de datos a la página. Especifique un comando SELECT para el control de origen de datos y, a continuación, establezca la propiedad DataSourceID del control de lista en el identificador del control de origen de datos. Use las propiedades **DataTextField** y **DataValueField** para definir el texto para mostrar y el valor del control. Además, puede usar la propiedad **DataTextFormatString** para controlar la apariencia del texto para mostrar como se indica a continuación:

| **Expresión** | **Descripción** |
| --- | --- |
| Precio: {0:C} | Para datos numéricos o decimales. Muestra el literal "Price:" seguido de los números en formato de divisa. El formato de moneda depende de la configuración de referencia cultural especificada en el atributo Culture de la Directiva de **Página** o en el archivo Web. config. |
| {0:D4} | Para datos enteros. No se puede usar con números decimales. Los enteros se muestran en un campo con relleno cero con un ancho de cuatro caracteres. |
| {0:N2}% | Para datos numéricos. Muestra el número con una precisión de dos decimales seguida del literal "%". |
| {0:000.0} | Para datos numéricos o decimales. Los números se redondean a una posición decimal. Los números de menos de tres dígitos se rellenan con ceros. |
| {0:D} | Para los datos de fecha y hora. Muestra el formato de fecha larga ("jueves, 06 de agosto de 1996"). El formato de fecha depende de la configuración de la referencia cultural de la página o del archivo Web.config. |
| {0:d} | Para los datos de fecha y hora. Muestra el formato de fecha corta ("12/31/99"). |
| {0:yy-MM-dd} | Para los datos de fecha y hora. Muestra la fecha en formato numérico de año-mes-día (96-08-06) |

## <a name="gridview"></a>GridView

El control GridView permite mostrar y editar datos tabulares mediante un enfoque declarativo y es el sucesor del control DataGrid. Las siguientes características están disponibles en el control GridView.

- Enlazar a controles de origen de datos, como SqlDataSource.
- Funcionalidades de ordenación integradas.
- Capacidades integradas de actualización y eliminación.
- Funciones de paginación integradas.
- Capacidades de selección de filas integradas.
- Acceso mediante programación al modelo de objetos de GridView para establecer propiedades dinámicamente, controlar eventos, etc.
- Varios campos de clave.
- Varios campos de datos para las columnas de hipervínculo.
- Apariencia personalizable a través de temas y estilos.

**Campos de columna**

Cada columna del control GridView se representa mediante un objeto DataControlField. De forma predeterminada, la propiedad AutoGenerateColumns está establecida en **true**, lo que crea un objeto AutoGeneratedField para cada campo del origen de datos. Cada campo se representa como una columna en el control GridView en el orden en que aparece cada campo en el origen de datos. También puede controlar manualmente los campos de columna que aparecen en el control **GridView** si establece la propiedad **AutoGenerateColumns** en **false** y después define su propia colección de campos de columna. Los distintos tipos de campos de columna determinan el comportamiento de las columnas del control.

En la tabla siguiente se enumeran los distintos tipos de campos de columna que se pueden usar.

| **Tipo de campo de columna** | **Descripción** |
| --- | --- |
| BoundField | Muestra el valor de un campo en un origen de datos. Es el tipo de columna predeterminado del control GridView. |
| ButtonField | Muestra un botón de comando para cada elemento en el control GridView. Esto le permite crear una columna de controles de botón personalizados, como el botón Agregar o quitar. |
| CheckBoxField | Muestra una casilla para cada elemento en el control GridView. Este tipo de campo de columna se utiliza normalmente para mostrar los campos con un valor booleano. |
| CommandField | Muestra botones de comando predefinidos para realizar operaciones de selección, edición o eliminación. |
| HyperLinkField | Muestra el valor de un campo en un origen de datos como un hipervínculo. Este tipo de campo de columna permite enlazar un segundo campo a la dirección URL del hipervínculo. |
| ImageField | Muestra una imagen para cada elemento en el control GridView. |
| TemplateField | Muestra el contenido definido por el usuario para cada elemento del control GridView según una plantilla especificada. Este tipo de campo de columna permite crear un campo de columna personalizado. |

Para definir una colección de campos de columna mediante declaración, primero agregue las columnas de apertura y cierre **&lt;&gt;** etiquetas entre las etiquetas de apertura y cierre del control GridView. A continuación, enumere los campos de columna que desea incluir entre las columnas de apertura y cierre **&lt;&gt;** etiquetas. Las columnas especificadas se agregan a la colección Columns en el orden mostrado. La colección **Columns** almacena todos los campos de columna en el control y permite administrar mediante programación los campos de columna en el control GridView.

Los campos de columna declarados explícitamente se pueden mostrar en combinación con los campos de columna generados automáticamente. Cuando se usan ambos, los campos de columna declarados explícitamente se representan en primer lugar, seguidos de los campos de columna generados automáticamente.

## <a name="binding-to-data"></a>Enlazar a datos

El control GridView se puede enlazar a un control de origen de datos (como **SqlDataSource**, **ObjectDataSource**, etc.), así como a cualquier origen de datos que implemente la interfaz System. Collections. IEnumerable (como System. Data. DataView, System. Collections. ArrayList o System. Collections. Hashtable). Use uno de los métodos siguientes para enlazar el control GridView al tipo de origen de datos adecuado:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control GridView en el valor de identificador del control de origen de datos. El control GridView se enlaza automáticamente al control de origen de datos especificado y puede aprovechar las capacidades del control de origen de datos para realizar operaciones de ordenación, actualización, eliminación y paginación. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa la interfaz System. Collections. IEnumerable, establezca mediante programación la propiedad DataSource del control GridView en el origen de datos y, a continuación, llame al método DataBind. Cuando se usa este método, el control GridView no proporciona funcionalidad integrada de ordenación, actualización, eliminación y paginación. Debe proporcionar esta funcionalidad usted mismo.

## <a name="data-operations"></a>Operaciones de datos

El control GridView proporciona muchas capacidades integradas que permiten al usuario ordenar, actualizar, eliminar, seleccionar y paginar elementos en el control. Cuando el control GridView se enlaza a un control de origen de datos, el control GridView puede aprovechar las capacidades del control de origen de datos y proporcionar funciones de ordenación, actualización y eliminación automáticas.

> [!NOTE]
> El control GridView puede proporcionar compatibilidad para ordenar, actualizar y eliminar con otros tipos de orígenes de datos. sin embargo, tendrá que proporcionar un controlador de eventos adecuado con la implementación para estas operaciones.

La ordenación permite al usuario ordenar los elementos del control GridView con respecto a una columna concreta haciendo clic en el encabezado de la columna. Para habilitar la ordenación, establezca la propiedad AllowSorting en **true**.

Las funcionalidades de actualización automática, eliminación y selección se habilitan cuando se hace clic en un botón de un campo de columna **ButtonField** o **TemplateField** , con el nombre de comando "Editar", "eliminar" y "seleccionar", respectivamente. El control GridView puede Agregar automáticamente un campo de columna **CommandField** con un botón Editar, eliminar o seleccionar si la propiedad AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateSelectButton está establecida en **true**, respectivamente.

> [!NOTE]
> La inserción de registros en el origen de datos no es compatible directamente con el control GridView. Sin embargo, es posible insertar registros mediante el control GridView junto con el control DetailsView o FormView.

En lugar de Mostrar todos los registros del origen de datos al mismo tiempo, el control GridView puede dividir automáticamente los registros en páginas. Para habilitar la paginación, establezca la propiedad AllowPaging en **true**.

## <a name="customizing-the-user-interface"></a>Personalización de la interfaz de usuario

Puede personalizar la apariencia del control GridView estableciendo las propiedades de estilo para las distintas partes del control. En la tabla siguiente se enumeran las distintas propiedades de estilo.

| **Propiedad de estilo** | **Descripción** |
| --- | --- |
| AlternatingRowStyle | La configuración de estilo para las filas de datos alternas en el control GridView. Cuando se establece esta propiedad, las filas de datos se muestran alternando entre la configuración de RowStyle y la configuración de **AlternatingRowStyle** . |
| EditRowStyle | Configuración de estilo de la fila que se está editando en el control GridView. |
| EmptyDataRowStyle | La configuración de estilo de la fila de datos vacía mostrada en el control GridView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo de la fila de pie de página del control GridView. |
| HeaderStyle | La configuración de estilo para la fila de encabezado del control GridView. |
| PagerStyle | Configuración de estilo de la fila de paginación del control GridView. |
| RowStyle | La configuración de estilo para las filas de datos en el control GridView. Cuando también se establece la propiedad **AlternatingRowStyle** , las filas de datos se muestran alternando entre la configuración de **RowStyle** y la configuración de **AlternatingRowStyle** . |
| SelectedRowStyle | La configuración de estilo de la fila seleccionada en el control GridView. |

También puede mostrar u ocultar diferentes partes del control. En la tabla siguiente se enumeran las propiedades que controlan qué partes se muestran u ocultan.

| **Propiedad** | **Descripción** |
| --- | --- |
| ShowFooter | Muestra u oculta la sección de pie de página del control GridView. |
| ShowHeader | Muestra u oculta la sección de encabezado del control GridView. |

### <a name="events"></a>Events

El control GridView proporciona varios eventos que se pueden programar. Esto le permite ejecutar una rutina personalizada cada vez que se produce un evento. En la tabla siguiente se enumeran los eventos admitidos por el control GridView.

| **Event** | **Descripción** |
| --- | --- |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones de paginación, pero después de que el control GridView se ocupe de la operación de paginación. Este evento se utiliza normalmente cuando es necesario realizar una tarea después de que el usuario navega a una página diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones de paginación, pero antes de que el control GridView administre la operación de paginación. Este evento suele usarse para cancelar la operación de paginación. |
| RowCancelingEdit | Se produce cuando se hace clic en el botón Cancelar de una fila, pero antes de que el control GridView salga del modo de edición. Este evento suele usarse para detener la operación de cancelación. |
| RowCommand | Se produce cuando se hace clic en un botón del control GridView. Este evento suele usarse para realizar una tarea cuando se hace clic en un botón del control. |
| RowCreated | Se produce cuando se crea una nueva fila en el control GridView. Este evento se usa a menudo para modificar el contenido de una fila cuando se crea la fila. |
| RowDataBound | Se produce cuando una fila de datos se enlaza a los datos del control GridView. Este evento se usa a menudo para modificar el contenido de una fila cuando la fila está enlazada a datos. |
| RowDeleted | Se produce cuando se hace clic en el botón eliminar de una fila, pero después de que el control GridView elimine el registro del origen de datos. Este evento se usa a menudo para comprobar los resultados de la operación de eliminación. |
| RowDeleting | Se produce cuando se hace clic en el botón eliminar de una fila, pero antes de que el control GridView elimine el registro del origen de datos. Este evento suele usarse para cancelar la operación de eliminación. |
| RowEditing | Se produce cuando se hace clic en el botón de edición de una fila, pero antes de que el control GridView entre en el modo de edición. Este evento suele usarse para cancelar la operación de edición. |
| RowUpdated | Se produce cuando se hace clic en el botón actualizar de una fila, pero después de que el control GridView actualice la fila. Este evento se usa a menudo para comprobar los resultados de la operación de actualización. |
| RowUpdating | Se produce cuando se hace clic en el botón actualizar de una fila, pero antes de que el control GridView actualice la fila. Este evento suele usarse para cancelar la operación de actualización. |
| SelectedIndexChanged | Se produce cuando se hace clic en el botón seleccionar de una fila, pero después de que el control GridView administre la operación de selección. Este evento suele usarse para realizar una tarea después de seleccionar una fila en el control. |
| SelectedIndexChanging | Se produce cuando se hace clic en el botón seleccionar de una fila, pero antes de que el control GridView administre la operación de selección. Este evento suele usarse para cancelar la operación de selección. |
| Ordenados | Se produce cuando se hace clic en el hipervínculo para ordenar una columna, pero después de que el control GridView se ocupe de la operación de ordenación. Este evento se usa normalmente para realizar una tarea después de que el usuario haga clic en un hipervínculo para ordenar una columna. |
| Ordenar | Se produce cuando se hace clic en el hipervínculo para ordenar una columna, pero antes de que el control GridView administre la operación de ordenación. Este evento suele usarse para cancelar la operación de ordenación o para realizar una rutina de ordenación personalizada. |

## <a name="formview"></a>FormView

El control FormView se utiliza para mostrar un único registro de un origen de datos. Es similar al control DetailsView, excepto en que muestra las plantillas definidas por el usuario en lugar de los campos de fila. La creación de sus propias plantillas proporciona mayor flexibilidad a la hora de controlar cómo se muestran los datos. El control FormView admite las siguientes características:

- Enlazar a controles de origen de datos, como SqlDataSource e ObjectDataSource.
- Funcionalidades de inserción integradas.
- Capacidades integradas de actualización y eliminación.
- Funciones de paginación integradas.
- Acceso mediante programación al modelo de objetos FormView para establecer propiedades dinámicamente, controlar eventos, etc.
- Apariencia personalizable a través de plantillas, temas y estilos definidos por el usuario.

## <a name="templates"></a>Plantillas

Para que el control FormView muestre contenido, debe crear plantillas para las distintas partes del control. La mayoría de las plantillas son opcionales. sin embargo, debe crear una plantilla para el modo en el que está configurado el control. Por ejemplo, un control FormView que admite la inserción de registros debe tener definida una plantilla insertar elemento. En la tabla siguiente se enumeran las distintas plantillas que se pueden crear.

| **Tipo de plantilla** | **Descripción** |
| --- | --- |
| EditItemTemplate | Define el contenido de la fila de datos cuando el control FormView está en modo de edición. Normalmente, esta plantilla contiene controles de entrada y botones de comando con los que el usuario puede editar un registro existente. |
| EmptyDataTemplate | Define el contenido de la fila de datos vacía que se muestra cuando el control FormView se enlaza a un origen de datos que no contiene registros. Normalmente, esta plantilla contiene contenido para avisar al usuario de que el origen de datos no contiene ningún registro. |
| FooterTemplate | Define el contenido de la fila de pie de página. Normalmente, esta plantilla contiene cualquier contenido adicional que le gustaría mostrar en la fila de pie de página. Como alternativa, puede simplemente especificar el texto que se va a mostrar en la fila de pie de página estableciendo la propiedad FooterText. |
| HeaderTemplate | Define el contenido de la fila de encabezado. Normalmente, esta plantilla contiene cualquier contenido adicional que le gustaría mostrar en la fila de encabezado. Como alternativa, puede simplemente especificar el texto que se va a mostrar en la fila de encabezado estableciendo la propiedad HeaderText. |
| ItemTemplate | Define el contenido de la fila de datos cuando el control FormView está en modo de solo lectura. Normalmente, esta plantilla contiene contenido para mostrar los valores de un registro existente. |
| InsertItemTemplate | Define el contenido de la fila de datos cuando el control FormView está en modo de inserción. Normalmente, esta plantilla contiene controles de entrada y botones de comando con los que el usuario puede Agregar un nuevo registro. |
| PagerTemplate | Define el contenido de la fila de paginación que se muestra cuando la característica de paginación está habilitada (cuando la propiedad AllowPaging está establecida en **true**). Normalmente, esta plantilla contiene controles con los que el usuario puede navegar a otro registro. |

Los controles de entrada de la plantilla editar elemento y la plantilla insertar elemento se pueden enlazar a los campos de un origen de datos mediante una expresión de enlace bidireccional. Esto permite que el control FormView Extraiga automáticamente los valores del control de entrada para una operación de actualización o inserción. Las expresiones de enlace bidireccionales también permiten que los controles de entrada de una plantilla de edición de elementos muestren automáticamente los valores de campo originales.

### <a name="binding-to-data"></a>Enlazar a datos

El control FormView se puede enlazar a un control de origen de datos (como **SqlDataSource**, AccessDataSource, **ObjectDataSource** , etc.) o a cualquier origen de datos que implemente la interfaz System. Collections. IEnumerable (como System. Data. DataView, System. Collections. ArrayList y System. Collections. Hashtable). Use uno de los métodos siguientes para enlazar el control FormView al tipo de origen de datos adecuado:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control FormView en el valor de identificador del control de origen de datos. El control FormView se enlaza automáticamente al control de origen de datos especificado y puede aprovechar las capacidades del control de origen de datos para realizar operaciones de inserción, actualización, eliminación y paginación. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa la interfaz **System. Collections. IEnumerable** , establezca mediante programación la propiedad DataSource del control FormView en el origen de datos y, a continuación, llame al método DataBind. Cuando se usa este método, el control FormView no proporciona funciones integradas de inserción, actualización, eliminación y paginación. Debe proporcionar esta funcionalidad mediante el evento adecuado.

## <a name="data-operations"></a>Operaciones de datos

El control FormView proporciona muchas capacidades integradas que permiten al usuario actualizar, eliminar, insertar y paginar elementos en el control. Cuando el control FormView se enlaza a un control de origen de datos, el control FormView puede aprovechar las capacidades del control de origen de datos y proporcionar la funcionalidad de actualización, eliminación, inserción y paginación automática. El control FormView puede proporcionar compatibilidad con las operaciones de actualización, eliminación, inserción y paginación con otros tipos de orígenes de datos. sin embargo, debe proporcionar un controlador de eventos adecuado con la implementación para estas operaciones.

Dado que el control FormView utiliza plantillas, no proporciona una manera de generar automáticamente botones de comando para realizar operaciones de actualización, eliminación o inserción. Debe incluir manualmente estos botones de comando en la plantilla adecuada. El control FormView reconoce ciertos botones que tienen sus propiedades **CommandName** establecidas en valores específicos. En la tabla siguiente se muestran los botones de comando que reconoce el control FormView.

| **Button** | **Valor de CommandName** | **Descripción** |
| --- | --- | --- |
| Cancelar | Ha | Se utiliza en las operaciones de actualización o inserción para cancelar la operación y descartar los valores especificados por el usuario. A continuación, el control FormView vuelve al modo especificado por la propiedad DefaultMode. |
| Eliminar | “Eliminar” | Se utiliza en la eliminación de operaciones para eliminar el registro mostrado del origen de datos. Genera los eventos ItemDeleting y ItemDeleted. |
| Editar | Editar | Se utiliza en las operaciones de actualización para poner el control FormView en modo de edición. El contenido especificado en la propiedad **EditItemTemplate** se muestra para la fila de datos. |
| Insertar | Introducir | Se utiliza en las operaciones de inserción para intentar insertar un nuevo registro en el origen de datos utilizando los valores proporcionados por el usuario. Genera los eventos ItemInserting y ItemInserted. |
| Nuevo | Otro | Se utiliza en las operaciones de inserción para colocar el control FormView en modo de inserción. El contenido especificado en la propiedad **InsertItemTemplate** se muestra para la fila de datos. |
| Página | Del | Se utiliza en las operaciones de paginación para representar un botón en la fila de paginación que realiza la paginación. Para especificar la operación de paginación, establezca la propiedad **CommandArgument** del botón en "Next", "Prev", "First", "Last" o el índice de la página a la que se va a navegar. Genera los eventos PageIndexChanging y PageIndexChanged. |
| Actualizar | Update | Se utiliza en las operaciones de actualización para intentar actualizar el registro mostrado en el origen de datos con los valores proporcionados por el usuario. Genera los eventos ItemUpdating y ItemUpdated. |

A diferencia del botón Eliminar (que elimina el registro mostrado inmediatamente), cuando se hace clic en el botón Editar o nuevo, el control FormView entra en modo de edición o inserción, respectivamente. En el modo de edición, el contenido incluido en la propiedad **EditItemTemplate** se muestra para el elemento de datos actual. Normalmente, la plantilla editar elemento se define de forma que el botón Editar se reemplaza por un botón actualizar y un botón Cancelar. Los controles de entrada adecuados para el tipo de datos del campo (como un cuadro de texto o un control de casilla) también se muestran normalmente con el valor de un campo para que el usuario lo modifique. Al hacer clic en el botón actualizar, se actualiza el registro del origen de datos, mientras que al hacer clic en el botón Cancelar se abandonan los cambios.

Del mismo modo, el contenido incluido en la propiedad **InsertItemTemplate** se muestra para el elemento de datos cuando el control está en modo de inserción. La plantilla insertar elemento normalmente se define de forma que el botón nuevo se reemplace por un botón Insertar y un botón Cancelar, y se muestren los controles de entrada vacíos para que el usuario escriba los valores del nuevo registro. Al hacer clic en el botón Insertar, se inserta el registro en el origen de datos, mientras que al hacer clic en el botón Cancelar se abandonan los cambios.

El control FormView proporciona una característica de paginación, que permite al usuario navegar a otros registros del origen de datos. Cuando está habilitada, se muestra una fila de paginación en el control FormView que contiene los controles de navegación de la página. Para habilitar la paginación, establezca la propiedad **AllowPaging** en **true**. Puede personalizar la fila del buscapersonas estableciendo las propiedades de los objetos incluidos en PagerStyle y la propiedad PagerSettings. En lugar de usar la interfaz de usuario de fila de buscapersonas integrada, puede crear su propia interfaz de usuario mediante la propiedad **PagerTemplate** .

## <a name="customizing-the-user-interface"></a>Personalización de la interfaz de usuario

Puede personalizar la apariencia del control FormView estableciendo las propiedades de estilo para las distintas partes del control. En la tabla siguiente se enumeran las distintas propiedades de estilo.

| **Propiedad de estilo** | **Descripción** |
| --- | --- |
| EditRowStyle | La configuración de estilo de la fila de datos cuando el control FormView está en modo de edición. |
| EmptyDataRowStyle | La configuración de estilo de la fila de datos vacía mostrada en el control FormView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo de la fila de pie de página del control FormView. |
| HeaderStyle | La configuración de estilo para la fila de encabezado del control FormView. |
| InsertRowStyle | La configuración de estilo de la fila de datos cuando el control FormView está en modo de inserción. |
| PagerStyle | La configuración de estilo de la fila de paginación mostrada en el control FormView cuando la característica de paginación está habilitada. |
| RowStyle | La configuración de estilo de la fila de datos cuando el control FormView está en modo de solo lectura. |

## <a name="events"></a>Events

El control FormView proporciona varios eventos que se pueden programar. Esto le permite ejecutar una rutina personalizada cada vez que se produce un evento. En la tabla siguiente se enumeran los eventos admitidos por el control FormView.

| **Event** | **Descripción** |
| --- | --- |
| ItemCommand | Se produce cuando se hace clic en un botón de un control FormView. Este evento suele usarse para realizar una tarea cuando se hace clic en un botón del control. |
| ItemCreated | Se produce después de crear todos los objetos FormViewRow en el control FormView. Este evento suele usarse para modificar los valores de un registro antes de que se muestre. |
| ItemDeleted | Se produce cuando se hace clic en un botón Eliminar (un botón con la propiedad **CommandName** establecida en "Delete"), pero después de que el control FormView elimine el registro del origen de datos. Este evento se usa a menudo para comprobar los resultados de la operación de eliminación. |
| ItemDeleting | Se produce cuando se hace clic en un botón eliminar, pero antes de que el control FormView elimine el registro del origen de datos. Este evento suele usarse para cancelar la operación de eliminación. |
| ItemInserted | Se produce cuando se hace clic en un botón Insertar (un botón con la propiedad **CommandName** establecida en "Insert"), pero después de que el control FormView Inserte el registro. Este evento se usa a menudo para comprobar los resultados de la operación de inserción. |
| ItemInserting | Se produce cuando se hace clic en un botón Insertar, pero antes de que el control FormView Inserte el registro. Este evento suele usarse para cancelar la operación de inserción. |
| ItemUpdated | Se produce cuando se hace clic en un botón Actualizar (un botón con la propiedad **CommandName** establecida en "Update"), pero después de que el control FormView actualice la fila. Este evento se usa a menudo para comprobar los resultados de la operación de actualización. |
| ItemUpdating | Se produce cuando se hace clic en un botón actualizar, pero antes de que el control FormView actualice el registro. Este evento suele usarse para cancelar la operación de actualización. |
| ModeChanged | Se produce después de que el control FormView cambie los modos (para editar, insertar o modo de solo lectura). Este evento suele usarse para realizar una tarea cuando el control FormView cambia de modo. |
| ModeChanging | Se produce antes de que el control FormView cambie los modos (para editar, insertar o modo de solo lectura). Este evento suele usarse para cancelar un cambio de modo. |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones de paginación, pero después de que el control FormView se ocupe de la operación de paginación. Este evento se utiliza normalmente cuando es necesario realizar una tarea después de que el usuario navegue a un registro diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones de paginación, pero antes de que el control FormView administre la operación de paginación. Este evento suele usarse para cancelar la operación de paginación. |

## <a name="detailsview"></a>DetailsView

El control DetailsView se usa para mostrar un único registro de un origen de datos en una tabla, donde cada campo del registro se muestra en una fila de la tabla. Se puede usar en combinación con un control GridView para escenarios maestro-detalle. El control DetailsView admite las siguientes características:

- Enlazar a controles de origen de datos, como SqlDataSource.
- Funcionalidades de inserción integradas.
- Capacidades integradas de actualización y eliminación.
- Funciones de paginación integradas.
- Acceso mediante programación al modelo de objetos de DetailsView para establecer propiedades dinámicamente, controlar eventos, etc.
- Apariencia personalizable a través de temas y estilos.

## <a name="row-fields"></a>Campos de fila

Cada fila de datos del control DetailsView se crea declarando un control de campo. Los distintos tipos de campos de fila determinan el comportamiento de las filas del control. Los controles de campo derivan de DataControlField. En la tabla siguiente se enumeran los distintos tipos de campos de fila que se pueden usar.

| **Tipo de campo de columna** | **Descripción** |
| --- | --- |
| BoundField | Muestra el valor de un campo en un origen de datos como texto. |
| ButtonField | Muestra un botón de comando en el control DetailsView. Esto le permite mostrar una fila con un control de botón personalizado, como un botón Agregar o quitar. |
| CheckBoxField | Muestra una casilla en el control DetailsView. Este tipo de campo de fila se utiliza normalmente para mostrar los campos con un valor booleano. |
| CommandField | Muestra los botones de comando integrados para realizar operaciones de edición, inserción o eliminación en el control DetailsView. |
| HyperLinkField | Muestra el valor de un campo en un origen de datos como un hipervínculo. Este tipo de campo de fila permite enlazar un segundo campo a la dirección URL del hipervínculo. |
| ImageField | Muestra una imagen en el control DetailsView. |
| TemplateField | Muestra el contenido definido por el usuario para una fila en el control DetailsView según una plantilla especificada. Este tipo de campo de fila permite crear un campo de fila personalizado. |

De forma predeterminada, la propiedad AutoGenerateRows se establece en **true**, que genera automáticamente un objeto de campo de fila enlazado para cada campo de un tipo enlazable en el origen de datos. Los tipos enlazables válidos son String, DateTime, decimal, GUID y el conjunto de tipos primitivos. Cada campo se muestra en una fila como texto, en el orden en que aparece cada campo en el origen de datos.

La generación automática de filas proporciona una manera rápida y sencilla de Mostrar todos los campos del registro. Sin embargo, para hacer uso de las funciones avanzadas del control DetailsView, debe declarar explícitamente los campos de fila que se van a incluir en el control DetailsView. Para declarar los campos de fila, primero establezca la propiedad **AutoGenerateRows** en **false**. A continuación, agregue **los campos de&lt;** de apertura y cierre&gt;etiquetas entre las etiquetas de apertura y cierre del control DetailsView. Por último, enumere los campos de fila que desea incluir entre los **campos&lt;** de apertura y cierre&gt;etiquetas. Los campos de fila especificados se agregan a la colección Fields en el orden mostrado. La colección **Fields** permite administrar mediante programación los campos de fila en el control DetailsView.

> [!NOTE]
> Los campos de fila generados automáticamente no se agregan a la colección Fields.

## <a name="binding-to-data"></a>Enlazar a datos

El control DetailsView se puede enlazar a un control de origen de datos, como **SqlDataSource** o AccessDataSource, o a cualquier origen de datos que implemente la interfaz System. Collections. IEnumerable, como System. Data. DataView, System. Collections. ArrayList y System. Collections. Hashtable.

Utilice uno de los métodos siguientes para enlazar el control DetailsView al tipo de origen de datos adecuado:

- Para enlazar a un control de origen de datos, establezca la propiedad DataSourceID del control DetailsView en el valor de identificador del control de origen de datos. El control DetailsView se enlaza automáticamente al control de origen de datos especificado. Este es el método preferido para enlazar a datos.
- Para enlazar a un origen de datos que implementa la interfaz **System. Collections. IEnumerable** , establezca mediante programación la propiedad DataSource del control DetailsView en el origen de datos y, a continuación, llame al método DataBind.

## <a name="security"></a>Seguridad

Este control se puede usar para mostrar los datos proporcionados por el usuario, que pueden incluir scripts de cliente malintencionados. Compruebe la información que se envía desde un cliente para el script ejecutable, las instrucciones SQL u otro código antes de mostrarlo en la aplicación. ASP.NET proporciona una característica de validación de solicitudes de entrada para bloquear el script y HTML en los datos proporcionados por el usuario.

## <a name="data-operations"></a>Operaciones de datos

El control DetailsView proporciona funciones integradas que permiten al usuario actualizar, eliminar, insertar y paginar elementos en el control. Cuando el control DetailsView se enlaza a un control de origen de datos, el control DetailsView puede aprovechar las capacidades del control de origen de datos y proporcionar la funcionalidad de actualización, eliminación, inserción y paginación automática.

El control DetailsView puede proporcionar compatibilidad con las operaciones de actualización, eliminación, inserción y paginación con otros tipos de orígenes de datos. sin embargo, debe proporcionar la implementación para estas operaciones en un controlador de eventos adecuado.

El control DetailsView puede Agregar automáticamente un campo de fila **CommandField** con un botón Editar, eliminar o nuevo estableciendo las propiedades AutoGenerateEditButton, AutoGenerateDeleteButton o AutoGenerateInsertButton en **true**, respectivamente. A diferencia del botón Eliminar (que elimina el registro seleccionado inmediatamente), cuando se hace clic en el botón Editar o nuevo, el control DetailsView entra en modo de edición o inserción, respectivamente. En el modo de edición, el botón Editar se sustituye por un botón actualizar y cancelar. Los controles de entrada adecuados para el tipo de datos del campo (como un cuadro de texto o un control de casilla) se muestran con el valor de un campo para que el usuario lo modifique. Al hacer clic en el botón actualizar, se actualiza el registro del origen de datos, mientras que al hacer clic en el botón Cancelar se abandonan los cambios. Del mismo modo, en el modo de inserción, el botón nuevo se reemplaza por un botón de inserción y de cancelación, y se muestran controles de entrada vacíos para que el usuario escriba los valores del nuevo registro.

El control DetailsView proporciona una característica de paginación, que permite al usuario navegar a otros registros del origen de datos. Cuando está habilitada, los controles de navegación en páginas se muestran en una fila de buscapersonas. Para habilitar la paginación, establezca la propiedad AllowPaging en **true**. La fila de paginación se puede personalizar mediante las propiedades PagerStyle y PagerSettings.

## <a name="customizing-the-user-interface"></a>Personalización de la interfaz de usuario

Puede personalizar la apariencia del control DetailsView estableciendo las propiedades de estilo para las distintas partes del control. En la tabla siguiente se enumeran las distintas propiedades de estilo.

| **Propiedad de estilo** | **Descripción** |
| --- | --- |
| AlternatingRowStyle | La configuración de estilo para las filas de datos alternas en el control DetailsView. Cuando se establece esta propiedad, las filas de datos se muestran alternando entre la configuración de RowStyle y la configuración de **AlternatingRowStyle** . |
| CommandRowStyle | Configuración de estilo de la fila que contiene los botones de comando integrados en el control DetailsView. |
| EditRowStyle | La configuración de estilo de las filas de datos cuando el control DetailsView está en modo de edición. |
| EmptyDataRowStyle | La configuración de estilo de la fila de datos vacía mostrada en el control DetailsView cuando el origen de datos no contiene ningún registro. |
| FooterStyle | La configuración de estilo de la fila de pie de página del control DetailsView. |
| HeaderStyle | La configuración de estilo para la fila de encabezado del control DetailsView. |
| InsertRowStyle | La configuración de estilo de las filas de datos cuando el control DetailsView está en modo de inserción. |
| PagerStyle | La configuración de estilo para la fila de paginación del control DetailsView. |
| RowStyle | La configuración de estilo para las filas de datos en el control DetailsView. Cuando también se establece la propiedad **AlternatingRowStyle** , las filas de datos se muestran alternando entre la configuración de **RowStyle** y la configuración de **AlternatingRowStyle** . |
| FieldHeaderStyle | La configuración de estilo para la columna de encabezado del control DetailsView. |

## <a name="events"></a>Events

El control DetailsView proporciona varios eventos que se pueden programar. Esto le permite ejecutar una rutina personalizada cada vez que se produce un evento. En la tabla siguiente se enumeran los eventos admitidos por el control DetailsView. El control DetailsView también hereda estos eventos de sus clases base: DataBinding, DataBound, disposed, init, Load, PreRender y Render.

| **Event** | **Descripción** |
| --- | --- |
| ItemCommand | Se produce cuando se hace clic en un botón en el control DetailsView. |
| ItemCreated | Se produce después de crear todos los objetos DetailsViewRow en el control DetailsView. Este evento suele usarse para modificar los valores de un registro antes de que se muestre. |
| ItemDeleted | Se produce cuando se hace clic en un botón eliminar, pero después de que el control DetailsView elimine el registro del origen de datos. Este evento se usa a menudo para comprobar los resultados de la operación de eliminación. |
| ItemDeleting | Se produce cuando se hace clic en un botón eliminar, pero antes de que el control DetailsView elimine el registro del origen de datos. Este evento suele usarse para cancelar la operación de eliminación. |
| ItemInserted | Se produce cuando se hace clic en un botón Insertar, pero después de que el control DetailsView Inserte el registro. Este evento se usa a menudo para comprobar los resultados de la operación de inserción. |
| ItemInserting | Se produce cuando se hace clic en un botón Insertar, pero antes de que el control DetailsView Inserte el registro. Este evento suele usarse para cancelar la operación de inserción. |
| ItemUpdated | Se produce cuando se hace clic en un botón actualizar, pero después de que el control DetailsView actualice la fila. Este evento se usa a menudo para comprobar los resultados de la operación de actualización. |
| ItemUpdating | Se produce cuando se hace clic en un botón actualizar, pero antes de que el control DetailsView actualice el registro. Este evento suele usarse para cancelar la operación de actualización. |
| ModeChanged | Se produce después de que el control DetailsView cambie los modos (modo de edición, inserción o solo lectura). Este evento suele usarse para realizar una tarea cuando el control DetailsView cambia de modo. |
| ModeChanging | Se produce antes de que el control DetailsView cambie los modos (modo de edición, inserción o solo lectura). Este evento suele usarse para cancelar un cambio de modo. |
| PageIndexChanged | Se produce cuando se hace clic en uno de los botones de paginación, pero después de que el control DetailsView se ocupe de la operación de paginación. Este evento se utiliza normalmente cuando es necesario realizar una tarea después de que el usuario navegue a un registro diferente en el control. |
| PageIndexChanging | Se produce cuando se hace clic en uno de los botones de paginación, pero antes de que el control DetailsView administre la operación de paginación. Este evento suele usarse para cancelar la operación de paginación. |

## <a name="the-menu-control"></a>Control de menú.

El control de menú de ASP.NET 2,0 está diseñado para ser un sistema de navegación con todas las características. Puede ser un enlace de datos fácilmente a los orígenes de datos jerárquicos, como el SiteMapDataSource.

Una estructura de controles de menú se puede definir de forma declarativa o dinámica y consta de un único nodo raíz y cualquier número de subnodos. El código siguiente define mediante declaración un menú para el control de menú.

[!code-aspx[Main](data-bound-controls/samples/sample4.aspx)]

En el ejemplo anterior, el nodo Home. aspx es el nodo raíz. Todos los demás nodos están anidados dentro del nodo raíz en varios niveles.

Hay dos tipos de menús que el control de menú puede representar; menús estáticos y menús dinámicos. Los menús estáticos se componen de elementos de menú que siempre están visibles. Los menús dinámicos se componen de elementos de menú que solo están visibles cuando el usuario mantiene el mouse sobre ellos. Los clientes a menudo pueden confundir los menús estáticos con los menús definidos de forma declarativa y dinámico con los menús que están enlazados a los enlaces de tiempo de ejecución. De hecho, los menús dinámicos y estáticos no están relacionados con el método de rellenado. Los términos *estático* y *dinámico* solo hacen referencia a si el menú se muestra o no de forma estática de forma predeterminada o solo se muestra cuando el usuario realiza alguna acción.

La propiedad **StaticDisplayLevels** se usa para configurar el número de niveles del menú que son estáticos y, por tanto, se muestran de forma predeterminada. En el ejemplo anterior, el establecimiento de la propiedad **StaticDisplayLevels** en un valor de 2 haría que el menú mostrara estáticamente el nodo principal, el nodo música y el nodo películas. Todos los demás nodos se muestran dinámicamente cuando el usuario mantiene el mouse sobre el nodo primario.

La propiedad **MaximumDynamicDisplayLevels** configura el número máximo de niveles dinámicos que puede mostrar el menú. Los menús dinámicos en un nivel superior al valor especificado por la propiedad **MaximumDynamicDisplayLevels** se descartan.

> [!NOTE]
> Es casi seguro que podría encontrarse con situaciones en las que no parece que los menús se representen debido a la propiedad MaximumDynamicDisplayLevels. En esos casos, asegúrese de que la propiedad está establecida de forma suficiente para permitir la visualización de los menús de los clientes.

## <a name="data-binding-the-menu-control"></a>Enlazar datos al control de menú

El control de menú se puede enlazar a cualquier origen de datos jerárquico, como SiteMapDataSource o XMLDataSource. El SiteMapDataSource es el método que se usa con más frecuencia para el enlace de datos a un control de menú porque se suministra fuera del archivo Web. sitemap y su esquema proporciona una API conocida al control de menú. La siguiente lista muestra un simple archivo Web. sitemap.

[!code-xml[Main](data-bound-controls/samples/sample5.xml)]

Observe que solo hay un elemento siteMapNode raíz, en este caso, el elemento Home. Se pueden configurar varios atributos para cada siteMapNode. Los atributos que se usan con más frecuencia son:

- **dirección URL** Especifica la dirección URL que se va a mostrar cuando un usuario haga clic en el elemento de menú. Si este atributo no está presente, el nodo simplemente volverá a enviar cuando se haga clic en él.
- **título** de Especifica el texto que se muestra en el elemento de menú.
- **Descripción** de Se usa como documentación para el nodo. También se muestra como información sobre herramientas cuando se mantiene el mouse sobre el nodo.
- **siteMapFile** Permite mapas anidadas. Este atributo debe apuntar a un archivo ASP.NET Sitemap con formato correcto.
- **roles** de Permite que el recorte de seguridad ASP.NET controle el aspecto de un nodo.

Tenga en cuenta que, aunque todos estos atributos son opcionales, el comportamiento del menú puede no ser el esperado si no se especifican. Por ejemplo, si se especifica el atributo *URL* pero el atributo *Description* no es, el nodo no será visible y no habrá forma de desplazarse a la dirección URL especificada.

## <a name="controlling-a-menus-operation"></a>Controlar una operación de menús

Hay varias propiedades que afectan al funcionamiento de un control de menú ASP.NET: la propiedad **Orientation** , la propiedad **DisappearAfter** , la propiedad **StaticItemFormatString** y la propiedad **StaticPopoutImageUrl** son solo algunas de ellas.

- La **orientación** se puede establecer en *horizontal* o *vertical* y controla si los elementos de menú estáticos se colocan horizontalmente en una fila o verticalmente y se apilan unos tras otros. Esta propiedad no afecta a los menús dinámicos.
- La propiedad **DisappearAfter** configura cuánto tiempo debe permanecer visible un menú dinámico después de que se haya desplazado el mouse. El valor se especifica en milisegundos y su valor predeterminado es 500. Si establece esta propiedad en un valor de-1, el menú nunca desaparecerá automáticamente. En ese caso, el menú solo desaparecerá cuando el usuario haga clic fuera del menú.
- La propiedad **StaticItemFormatString** facilita el mantenimiento de palabras coherentes en el sistema de menús. Al especificar esta propiedad, se debe especificar *{0}* en lugar de la descripción que aparece en el origen de datos. Por ejemplo, para que el elemento de menú del ejercicio 1 visite nuestra página de productos, etc., debe especificar visite nuestra página de {0} de StaticItemFormatString. En tiempo de ejecución, ASP.NET reemplazará cualquier aparición de {0} con la descripción correcta para el elemento de menú.
- La propiedad **StaticPopoutImageUrl** especifica la imagen que se usa para indicar que un nodo de menú determinado tiene nodos secundarios a los que se puede tener acceso al mantener el mouse sobre él. Los menús dinámicos seguirán usando la imagen predeterminada.

## <a name="templated-menu-controls"></a>Controles de menú con plantilla

El control de menú es un control con plantilla y permite dos ItemTemplates diferentes; StaticItemTemplate y DynamicItemTemplate. Con estas plantillas, puede agregar fácilmente controles de servidor o controles de usuario a los menús.

Para editar las plantillas en Visual Studio .NET, haga clic en el botón de etiqueta inteligente en el menú y elija editar plantillas. Después, puede elegir entre editar StaticItemTemplate o DynamicItemTemplate.

Los controles agregados a StaticItemTemplate aparecerán en el menú estático cuando se cargue la página. Los controles agregados a DynamicItemTemplate aparecerán en todos los menús emergentes.

## <a name="menu-events"></a>Eventos de menú

El control de menú tiene dos eventos que son exclusivos de él; el **MenuItemClicked** y el evento **MenuItemDatabound** .

El evento MenuItemClicked se genera cuando se hace clic en un elemento de menú. El evento MenuItemDatabound se genera cuando un elemento de menú está enlazado a un objeto. Los **MenuEventArgs** que se pasan al controlador de eventos proporcionan acceso al elemento de menú a través de la propiedad Item.

## <a name="controlling-a-menus-appearance"></a>Controlar el aspecto de un menú

También puede afectar a la apariencia de un control de menú mediante uno o varios de los muchos estilos disponibles para dar formato a los menús. Entre ellas se encuentran **StaticMenuStyle**, **DynamicMenuStyle**, **DynamicMenuItemStyle**, **DynamicSelectedStyle**y **DynamicHoverStyle**. Estas propiedades se configuran mediante una cadena de estilo HTML estándar. Por ejemplo, lo siguiente afectará al estilo de los menús dinámicos.

[!code-aspx[Main](data-bound-controls/samples/sample6.aspx)]

> [!NOTE]
> Si usa cualquiera de los estilos de desplazamiento, tendrá que agregar un elemento &lt;Head&gt; en a la página con el elemento *runat* establecido en *Server*.

Los controles de menú también admiten el uso de temas de ASP.NET 2,0.

## <a name="the-treeview-control"></a>El control TreeView

El control TreeView muestra los datos en una estructura similar a un árbol. Al igual que con el control de menú, se pueden enlazar fácilmente datos a cualquier origen de datos jerárquico, como el SiteMapDataSource.

La primera pregunta que los clientes suelen formular sobre el control TreeView en ASP.NET 2,0 es si está relacionada con el WebControl de vista de árbol de Internet Explorer, que estaba disponible para ASP.NET 1. x. No lo es. El control TreeView de ASP.NET 2,0 se escribió desde el principio y ofrece una mejora significativa sobre el WebControl webtreeview de Internet Explorer que estaba disponible anteriormente.

No entraremos en detalles sobre cómo enlazar un control TreeView a un mapa del sitio, ya que se realiza exactamente de la misma manera que el control de menú. Sin embargo, el control TreeView tiene algunas diferencias diferenciales en la forma en que funciona.

De forma predeterminada, un control TreeView aparece totalmente expandido. Para cambiar el nivel de expansión después de la carga inicial, modifique la propiedad **ExpandDepth** del control. Esto es especialmente importante en los casos en los que la vista de árbol está enlazada a datos al expandir nodos concretos.

## <a name="databinding-the-treeview-control"></a>DataBinding del control TreeView

A diferencia del control de menú, TreeView se presta bien a administrar grandes cantidades de datos. Por lo tanto, además de enlazar datos a un SiteMapDataSource o XMLDataSource, la vista de árbol suele estar enlazada a un conjunto de datos u otros datos relacionales. En los casos en los que el control TreeView se enlaza a grandes cantidades de datos, es mejor enlazar solo a los datos que realmente están visibles en el control. Después, puede enlazar datos a datos adicionales a medida que se expanden los nodos de TreeView.

En estos casos, la propiedad **PopulateOnDemand** de TreeView debe establecerse en *true*. A continuación, tendrá que proporcionar una implementación para el método **TreeNodePopulate** .

## <a name="data-binding-without-postback"></a>Enlace de datos sin postback

Tenga en cuenta que al expandir un nodo en el ejemplo anterior por primera vez, la página se devuelve y se actualiza. Esto no es un problema en este ejemplo, pero puede imaginar que podría estar en un entorno de producción con una gran cantidad de datos. Un escenario mejor sería aquél en el que la vista de árbol seguiría rellenando dinámicamente sus nodos, pero sin devolución de datos al servidor.

Al establecer las propiedades **PopulateNodesFromClient** y **PopulateOnDemand** en true, el control TreeView de ASP.net rellenará dinámicamente los nodos sin devolución de datos. Cuando se expande el nodo primario, se realiza una solicitud XMLHttp desde el cliente y se desencadena el evento OnTreeNodePopulate. El servidor responde con una isla de datos XML que se usa para enlazar los nodos secundarios a los datos.

ASP.NET crea dinámicamente el código de cliente que implementa esta funcionalidad. El script &lt;&gt; etiquetas que contienen el script se generan al apuntar a un archivo AXD. Por ejemplo, en la siguiente lista se muestran los vínculos de script para el código de script que genera la solicitud XMLHttp.

[!code-html[Main](data-bound-controls/samples/sample7.html)]

Si examina el archivo AXD anterior en el explorador y lo abre, verá el código que implementa la solicitud XMLHttp. Este método evita que los clientes modifiquen el archivo de script.

## <a name="controlling-the-operation-of-the-treeview-control"></a>Controlar el funcionamiento del control TreeView

El control TreeView tiene varias propiedades que afectan al funcionamiento del control. Las propiedades más obvias son **ShowCheckBoxes**, **ShowExpandCollapse**y **ShowLines**.

La propiedad **ShowCheckBoxes** afecta a si los nodos muestran o no una casilla cuando se representan. Los valores válidos para esta propiedad son **ninguno**, **raíz**, **primario**, **hoja**y **todos**. Esto afecta al control TreeView como se indica a continuación:

| **Valor de propiedad** | **Efecto** |
| --- | --- |
| Ninguna | Las casillas no se muestran en ningún nodo. Ésta es la configuración predeterminada. |
| Root | Una casilla solo se muestra en el nodo raíz. |
| Primario | Una casilla solo se muestra en los nodos que tienen nodos secundarios. Esos nodos secundarios pueden ser nodos primarios o nodos de hoja. |
| Hoja | Una casilla solo se muestra en los nodos que no tienen nodos secundarios. |
| Todas | Se muestra una casilla en todos los nodos. |

Cuando se usan casillas, la propiedad **CheckedNodes** devolverá una colección de nodos de TreeView que se comprueban en el PostBack.

La propiedad **ShowExpandCollapse** controla la apariencia de la imagen de expandir o contraer junto a los nodos raíz y primarios. Si esta propiedad se establece en **false**, los nodos de TreeView se representan como hipervínculos y se expanden o contraen al hacer clic en el vínculo.

La propiedad **ShowLines** controla si se muestran o no las líneas que conectan los nodos primarios a los nodos secundarios. Si **es false** (valor predeterminado), no se muestra ninguna línea. Si **es true**, el control TreeView usará imágenes de líneas en la carpeta especificada por la propiedad **LineImagesFolder** .

Para personalizar la apariencia de las líneas de TreeView, Visual Studio .NET 2005 incluye una herramienta de diseño de línea. Puede tener acceso a esta herramienta mediante el botón de etiqueta inteligente del control TreeView como se indica a continuación.

![](data-bound-controls/_static/image1.jpg)

**Ilustración 1**

Al seleccionar la opción de menú **personalizar imágenes de línea** , se iniciará la herramienta diseñador de líneas, que le permitirá configurar la apariencia de las líneas de TreeView.

## <a name="treeview-events"></a>Eventos TreeView

El control TreeView tiene los siguientes eventos únicos:

- SelectedNodeChanged se produce cuando se selecciona un nodo basándose en la propiedad **SelectAction** .
- TreeNodeCheckChanged se produce cuando cambia el estado de las casillas de los nodos.
- TreeNodeExpanded se produce cuando se expande un nodo basado en la propiedad **SelectAction** .
- TreeNodeCollapsed se produce cuando se contrae un nodo.
- TreeNodeDataBound se produce cuando un nodo está enlazado a datos.
- TreeNodePopulate se produce cuando se rellena un nodo.

La propiedad **SelectAction** permite configurar el evento que se desencadena cuando se selecciona un nodo. La propiedad SelectAction proporciona las siguientes acciones:

- TreeNodeSelectAction. Expand genera TreeNodeExpanded cuando se selecciona el nodo.
- TreeNodeSelectAction. None no genera ningún evento cuando se selecciona el nodo.
- TreeNodeSelectAction. Select provoca el evento SelectedNodeChanged cuando se selecciona el nodo.
- TreeNodeSelectAction. SelectExpand genera el evento SelectedNodeChanged y el evento TreeNodeExpanded cuando se selecciona el nodo.

## <a name="controlling-appearance-with-styles"></a>Controlar el aspecto de los estilos

El control TreeView proporciona muchas propiedades para controlar la apariencia del control con estilos. Están disponibles las propiedades siguientes:

| **Nombre de la propiedad** | **Controles** |
| --- | --- |
| HoverNodeStyle | Controla el estilo de los nodos cuando se mantiene el mouse sobre ellos. |
| LeafNodeStyle | Controla el estilo de los nodos hoja. |
| NodeStyle | Controla el estilo de todos los nodos. Los estilos de nodo específicos (como LeafNodeStyle) invalidan este estilo. |
| ParentNodeStyle | Controla el estilo de todos los nodos primarios. |
| RootNodeStyle | Controla el estilo del nodo raíz. |
| SelectedNodeStyle | Controla el estilo del nodo seleccionado. |

Cada una de estas propiedades es de solo lectura. Sin embargo, cada uno de ellos devolverá un objeto **TreeNodeStyle** y las propiedades de ese objeto se pueden modificar con el formato *de subpropiedad de propiedad* . Por ejemplo, para establecer la propiedad **ForeColor** del **SelectedNodeStyle**, usaría la siguiente sintaxis:

[!code-aspx[Main](data-bound-controls/samples/sample8.aspx)]

Observe que la etiqueta anterior no está cerrada. Esto se debe a que cuando se usa la sintaxis declarativa que se muestra aquí, también se incluyen los nodos controles TreeView simples en el código HTML.

Las propiedades de estilo también se pueden especificar en el código mediante el formato *Property. Subproperty* . Por ejemplo, para establecer la propiedad **ForeColor** de **RootNodeStyle** en el código, usaría la siguiente sintaxis:

[!code-csharp[Main](data-bound-controls/samples/sample9.cs)]

> [!NOTE]
> Para obtener una lista completa de las distintas propiedades de estilo, vea la documentación de MSDN sobre el objeto TreeNodeStyle.

## <a name="the-sitemappath-control"></a>Control SiteMapPath

El control SiteMapPath proporciona un control de navegación de migas de pan para desarrolladores de ASP.NET. Al igual que los demás controles de navegación, se pueden enlazar fácilmente datos a orígenes de datos jerárquicos como SiteMapDataSource o XmlDataSource.

Un control SiteMapPath se compone de objetos SiteMapNodeItem. Hay tres tipos de nodos: el nodo raíz, los nodos primarios y el nodo actual. El nodo raíz es el nodo situado en la parte superior de la estructura jerárquica. El nodo actual representa la página actual. Todos los demás nodos son nodos primarios.

## <a name="controlling-the-operation-of-the-sitemappath-control"></a>Controlar el funcionamiento del control SiteMapPath

Las propiedades que controlan el funcionamiento del control SiteMapPath son las siguientes:

| **Propiedad** | **Descripción de la propiedad** |
| --- | --- |
| ParentLevelsDisplayed | Controla el número de nodos primarios que se muestran. El valor predeterminado es-1, que no impone ninguna restricción en el número de nodos primarios mostrados. |
| PathDirection | Controla la dirección de SiteMapPath. Los valores válidos son RootToCurrent (default) y CurrentToRoot. |
| PathSeparator | Cadena que controla el carácter que separa los nodos en un control SiteMapPath. El valor predeterminado es:. |
| RenderCurrentNodeAsLink | Valor booleano que controla si el nodo actual se representa o no como un vínculo. El valor predeterminado es False. |
| SkipLinkText | Ayuda con la accesibilidad cuando los lectores de pantalla ven la página. Esta propiedad permite que los lectores de pantalla omitan el control SiteMapPath. Para deshabilitar esta característica, establezca la propiedad en String. Empty. |

## <a name="templated-sitemappath-controls"></a>Controles SiteMapPath con plantilla

SiteMapControl es un control con plantilla y, como tal, puede definir plantillas diferentes para su uso en la visualización del control. Para editar plantillas en un control SiteMapPath, haga clic en el botón de etiqueta inteligente del control y elija editar plantillas en el menú. Esto muestra el menú SiteMapTasks, tal y como se muestra a continuación, donde puede elegir entre las distintas plantillas disponibles.

![](data-bound-controls/_static/image2.jpg)

**Ilustración 2**

La plantilla **NodeTemplate** hace referencia a cualquier nodo del SiteMapPath. Si el nodo es un nodo raíz o el nodo actual y se configura **RootNodeTemplate** o **CurrentNodeTemplate** , se invalida NodeTemplate.

## <a name="sitemappath-events"></a>SiteMapPath (eventos)

El control SiteMapPath tiene dos eventos que no se derivan de la clase control. el evento **ItemCreated** y el evento **ItemDataBound** . El evento ItemCreated se genera cuando se crea un elemento SiteMapPath. ItemDataBound se genera cuando se llama al método DataBind durante el enlace de datos de un nodo SiteMapPath. Un objeto **SiteMapNodeItemEventArgs** proporciona acceso al SiteMapNodeItem específico a través de la propiedad Item.

## <a name="controlling-appearance-with-styles"></a>Controlar el aspecto de los estilos

Los estilos siguientes están disponibles para dar formato a un control SiteMapPath.

| **Nombre de la propiedad** | **Controles** |
| --- | --- |
| CurrentNodeStyle | Controla el estilo del texto del nodo actual. |
| RootNodeStyle | Controla el estilo del texto del nodo raíz. |
| NodeStyle | Controla el estilo del texto de todos los nodos suponiendo que no se aplica un CurrentNodeStyle o RootNodeStyle. |

La propiedad NodeStyle se reemplaza por CurrentNodeStyle o RootNodeStyle. Cada una de estas propiedades es de solo lectura y devuelve un objeto de **estilo** . Para afectar a la apariencia de un nodo mediante una de estas propiedades, debe establecer las propiedades del objeto de estilo que se devuelve. Por ejemplo, el código siguiente cambia la propiedad ForeColor del nodo actual.

[!code-aspx[Main](data-bound-controls/samples/sample10.aspx)]

La propiedad también se puede aplicar mediante programación de la siguiente manera:

[!code-csharp[Main](data-bound-controls/samples/sample11.cs)]

> [!NOTE]
> Si se aplica una plantilla, no se aplicará el estilo.

## <a name="lab-1-configuring-an-aspnet-menu-control"></a>Práctica 1: configurar un control de menú de ASP.NET

1. Crear un sitio web nuevo.
2. Agregue un archivo de mapa del sitio seleccionando archivo, nuevo, archivo y eligiendo mapa del sitio en la lista de plantillas de archivo.
3. Abra el mapa del sitio (Web. sitemap de forma predeterminada) y modifíquelo de modo que se parezca a la lista siguiente. Las páginas a las que está vinculando en el archivo del mapa del sitio no existen realmente, pero eso no será un problema para este ejercicio.

    [!code-xml[Main](data-bound-controls/samples/sample12.xml)]
4. Abra el formulario web predeterminado en Vista de diseño.
5. En la sección navegación del cuadro de herramientas, agregue un nuevo control de menú a la página.
6. En la sección datos del cuadro de herramientas, agregue un nuevo SiteMapDataSource. El SiteMapDataSource usará automáticamente el archivo Web. sitemap en el sitio. (El archivo Web. sitemap *debe* estar en la carpeta raíz del sitio).
7. Haga clic en el control de menú y, a continuación, haga clic en el botón etiqueta inteligente para mostrar el cuadro de diálogo tareas de menú.
8. En la lista desplegable elegir origen de datos, seleccione SiteMapDataSource1.
9. Haga clic en el vínculo Autoformato y elija un formato para el menú.
10. En el panel Propiedades, establezca la propiedad **StaticDisplayLevels** en 2. Ahora, el control de menú debería mostrar el nodo Inicio, productos y servicios en el diseñador.
11. Examine la página en el explorador para usar el menú. (Dado que las páginas que se agregan al mapa del sitio no existen realmente, verá un error al intentar ir a ellas).

Experimente con el cambio de las propiedades StaticDisplayLevels y MaximumDynamicDisplayLevels y vea cómo afectan al modo en que se representa el menú.

## <a name="lab-2-dynamically-binding-a-treeview-control"></a>Práctica 2: enlazar dinámicamente un control TreeView

En este ejercicio se supone que tiene SQL Server en ejecución localmente y que la base de datos Northwind está presente en la instancia de SQL Server. Si no se cumplen estas condiciones, cambie la cadena de conexión en el ejemplo. Tenga en cuenta que es posible que también tenga que especificar SQL Server autenticación en lugar de una conexión de confianza.

1. Crear un sitio web nuevo.
2. Cambie a la vista de código para default. aspx y reemplace todo el código por el código que se muestra a continuación. 

    [!code-aspx[Main](data-bound-controls/samples/sample13.aspx)]
3. Guarde la página como TreeView. aspx.
4. Examine la página.
5. Cuando se muestre la página por primera vez, vea el origen de la página en el explorador. Tenga en cuenta que solo los nodos visibles se enviaron al cliente.
6. Haga clic en el signo más junto a cualquier nodo.
7. Vuelva a ver el código fuente de la página. Observe que los nodos recién mostrados ahora están presentes.

## <a name="lab-3-details-view-and-editing-data-using-a-gridview-and-detailsview"></a>Práctica 3: ver detalles y editar datos mediante GridView y DetailsView

1. Crear un sitio web nuevo.
2. Agregue un nuevo archivo Web. config al sitio Web.
3. Agregue una cadena de conexión al archivo Web. config como se muestra a continuación: 

    [!code-xml[Main](data-bound-controls/samples/sample14.xml)]

    > [!NOTE]
    > Es posible que deba cambiar la cadena de conexión en función de su entorno.
4. Guarde y cierre el archivo web.config.
5. Abra default. aspx y agregue un nuevo control SqlDataSource.
6. Cambie el identificador del control SqlDataSource a **Products**.
7. En el menú **tareas de SqlDataSource** , haga clic en **Configurar origen de datos**.
8. Seleccione **Northwind** en la lista desplegable conexión y haga clic en siguiente.
9. Seleccione **productos** en la lista desplegable **nombre** y active las casillas **ProductID**, **ProductName**, **UnitPrice**y **UnidadesEnExistencias** , como se muestra a continuación. 

![](data-bound-controls/_static/image3.jpg)

    **Figure 3**
10. Haga clic en **Siguiente**.
11. Haga clic en **Finalizar**.
12. Cambie a la vista Código fuente y examine el código que se generó. Observe **SelectCommand**, **DeleteCommand**, **InsertCommand**y **UpdateCommand** que se agregaron al control SqlDataSource. Observe también los parámetros que se agregaron.
13. Cambie a Vista de diseño y agregue un nuevo control GridView a la página.
14. Seleccione **productos** en la lista desplegable **elegir origen de datos** .
15. Active **Habilitar paginación** y **Habilitar selección** como se muestra a continuación. 

![](data-bound-controls/_static/image4.jpg)

    **Figure 4**
16. Haga clic en el vínculo **Editar columnas** y asegúrese de que la opción **generar campos automáticamente** está activada.
17. Haga clic en **Aceptar**.
18. Con el control GridView seleccionado, haga clic en el botón situado junto a la propiedad **DataKeyNames** en el panel Propiedades.
19. Seleccione **ProductID** en la lista **campos de datos disponibles** y haga clic en el botón **&gt;** para agregarlo.
20. Haga clic en Aceptar.
21. Agregue un nuevo control SqlDataSource a la página.
22. Cambie el identificador del control SqlDataSource a **detalles**.
23. En el menú tareas de SqlDataSource, elija **Configurar origen de datos**.
24. Elija **Northwind** en la lista desplegable y haga clic en **siguiente**.
25. Seleccione <strong>productos</strong> en la lista desplegable <strong>nombre</strong> y active la casilla <strong>\</strong > * en el cuadro de lista <strong>columnas</strong> .
26. Haga clic en el botón **dónde** .
27. Seleccione **ProductID** en la lista desplegable **columna** .
28. Seleccione **=** en la lista desplegable operador.
29. Seleccione el **control** en la lista desplegable **origen** .
30. Seleccione **GridView1** en la lista desplegable ID. de **control** .
31. Haga clic en el botón **Agregar** para agregar la cláusula WHERE.
32. Haga clic en **Aceptar**.
33. Haga clic en el botón **Opciones avanzadas** y active la casilla **generar instrucciones INSERT, Update y DELETE** .
34. Haga clic en **Aceptar**.
35. Haga clic en **siguiente** y en **Finalizar**.
36. Agregue un control DetailsView a la página.
37. En la lista desplegable **elegir origen de datos** , elija **detalles**.
38. Active la casilla **Habilitar edición** como se muestra a continuación. 

![](data-bound-controls/_static/image1.gif)

    **Figure 5**
39. Guarde la página y examine default. aspx.
40. Haga clic en el vínculo **seleccionar** junto a registros diferentes para ver la actualización de DetailsView automáticamente.
41. Haga clic en el vínculo **Editar** en el control DetailsView.
42. Realice un cambio en el registro y haga clic en **Actualizar**.
