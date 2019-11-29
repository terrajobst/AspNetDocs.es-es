---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
title: Realización de actualizaciones porC#lotes () | Microsoft Docs
author: rick-anderson
description: Obtenga información sobre cómo crear una lista de objetos totalmente modificable en la que todos sus elementos están en modo de edición y cuyos valores se pueden guardar haciendo clic en un botón "Actualizar todo" en la...
ms.author: riande
ms.date: 10/30/2006
ms.assetid: 57743ca7-5695-4e07-aed1-44b297f245a9
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/performing-batch-updates-cs
msc.type: authoredcontent
ms.openlocfilehash: cde12a4d24555216adc49dd02818901278932eaa
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74631560"
---
# <a name="performing-batch-updates-c"></a>Realizar actualizaciones por lotes (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_37_CS.exe) de la aplicación de ejemplo o [descarga de PDF](performing-batch-updates-cs/_static/datatutorial37cs1.pdf)

> Obtenga información acerca de cómo crear una lista de objetos totalmente modificable en la que todos sus elementos están en modo de edición y cuyos valores se pueden guardar haciendo clic en el botón "Actualizar todo" de la página.

## <a name="introduction"></a>Introducción

En el [tutorial anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) , hemos examinado cómo crear una lista de elementos de nivel de elemento. Al igual que el GridView modificable estándar, cada elemento de la lista de elementos incluye un botón Editar que, cuando se hace clic en él, haría que el elemento se modificara. Aunque esta edición de nivel de elemento funciona bien para los datos que solo se actualizan ocasionalmente, ciertos escenarios de casos de uso requieren que el usuario modifique muchos registros. Si un usuario necesita editar docenas de registros y se ve obligado a hacer clic en editar, realizar los cambios y hacer clic en actualizar para cada uno, la cantidad de clic puede obstaculizar su productividad. En estas situaciones, una opción mejor es proporcionar una lista de objetos totalmente editable, una donde *todos* sus elementos están en modo de edición y cuyos valores se pueden editar haciendo clic en el botón Actualizar todo de la página (vea la figura 1).

[![se pueden modificar todos los elementos de una lista de DataList totalmente modificable](performing-batch-updates-cs/_static/image2.png)](performing-batch-updates-cs/_static/image1.png)

**Figura 1**: todos los elementos de una lista de DataList totalmente modificable se pueden modificar ([haga clic para ver la imagen de tamaño completo](performing-batch-updates-cs/_static/image3.png))

En este tutorial, veremos cómo permitir a los usuarios actualizar la información de dirección de los proveedores mediante una lista de datos totalmente modificable.

## <a name="step-1-create-the-editable-user-interface-in-the-datalist-s-itemtemplate"></a>Paso 1: crear la interfaz de usuario editable en el ItemTemplate de DataList

En el tutorial anterior, donde creamos una lista de elementos de lista editable estándar de nivel de elemento, usamos dos plantillas:

- `ItemTemplate` contenía la interfaz de usuario de solo lectura (la etiqueta controles Web para mostrar cada nombre de producto y precio).
- `EditItemTemplate` contenía la interfaz de usuario del modo de edición (los dos controles Web TextBox).

La propiedad DataList s `EditItemIndex` dicta qué `DataListItem` (si existe) se representa mediante el `EditItemTemplate`. En concreto, la `DataListItem` cuyo valor `ItemIndex` coincide con la propiedad DataList s `EditItemIndex` se representa utilizando el `EditItemTemplate`. Este modelo funciona bien cuando solo se puede editar un elemento a la vez, pero se separa al crear una lista de elementos de lista totalmente modificable.

En el caso de una lista de objetos totalmente modificable, deseamos que *todas* las `DataListItem` s se representen mediante la interfaz editable. La manera más sencilla de lograrlo es definir la interfaz editable en el `ItemTemplate`. Para modificar la información de dirección de los proveedores, la interfaz editable contiene el nombre del proveedor como texto y, a continuación, los cuadros de texto de los valores de dirección, ciudad y país.

Para empezar, abra la página `BatchUpdate.aspx`, agregue un control DataList y establezca su propiedad `ID` en `Suppliers`. En la etiqueta inteligente DataList s, opte por agregar un nuevo control ObjectDataSource denominado `SuppliersDataSource`.

[![crear un nuevo ObjectDataSource denominado SuppliersDataSource](performing-batch-updates-cs/_static/image5.png)](performing-batch-updates-cs/_static/image4.png)

**Figura 2**: creación de un nuevo ObjectDataSource denominado `SuppliersDataSource` ([haga clic para ver la imagen de tamaño completo](performing-batch-updates-cs/_static/image6.png))

Configure ObjectDataSource para recuperar datos mediante el método `SuppliersBLL` Class s `GetSuppliers()` (consulte la figura 3). Como en el tutorial anterior, en lugar de actualizar la información del proveedor a través de ObjectDataSource, trabajaremos directamente con el nivel de lógica de negocios. Por lo tanto, establezca la lista desplegable en (ninguno) en la pestaña actualizar (consulte la figura 4).

[![recuperar información del proveedor mediante el método GetSuppliers ()](performing-batch-updates-cs/_static/image8.png)](performing-batch-updates-cs/_static/image7.png)

**Figura 3**: recuperación de información del proveedor mediante el método `GetSuppliers()` ([haga clic para ver la imagen de tamaño completo](performing-batch-updates-cs/_static/image9.png))

[![establecer la lista desplegable en (ninguno) en la pestaña actualizar](performing-batch-updates-cs/_static/image11.png)](performing-batch-updates-cs/_static/image10.png)

**Figura 4**: establezca la lista desplegable en (ninguno) en la pestaña actualizar ([haga clic para ver la imagen a tamaño completo](performing-batch-updates-cs/_static/image12.png))

Después de completar el asistente, Visual Studio genera automáticamente el `ItemTemplate` de datos de lista para mostrar cada campo de datos devuelto por el origen de datos en un control Web de etiqueta. Necesitamos modificar esta plantilla para que proporcione la interfaz de edición en su lugar. El `ItemTemplate` se puede personalizar mediante el diseñador mediante la opción editar plantillas de la etiqueta inteligente DataList s o directamente a través de la sintaxis declarativa.

Dedique un momento a crear una interfaz de edición que muestre el nombre del proveedor como texto, pero incluye cuadros de texto para los valores de dirección, ciudad y país del proveedor. Después de realizar estos cambios, la sintaxis declarativa de la página s debe ser similar a la siguiente:

[!code-aspx[Main](performing-batch-updates-cs/samples/sample1.aspx)]

> [!NOTE]
> Al igual que en el tutorial anterior, el DataList de este tutorial debe tener habilitado su estado de vista.

En el `ItemTemplate` I m con dos clases CSS nuevas, `SupplierPropertyLabel` y `SupplierPropertyValue`, que se han agregado a la clase `Styles.css` y se han configurado para usar la misma configuración de estilo que las clases CSS `ProductPropertyLabel` y `ProductPropertyValue`.

[!code-css[Main](performing-batch-updates-cs/samples/sample2.css)]

Después de realizar estos cambios, visite esta página a través de un explorador. Como se muestra en la figura 5, cada elemento de la lista de elementos muestra el nombre del proveedor como texto y usa cuadros de texto para mostrar la dirección, la ciudad y el país.

[![cada proveedor de la lista de DataList es editable](performing-batch-updates-cs/_static/image14.png)](performing-batch-updates-cs/_static/image13.png)

**Figura 5**: cada proveedor de la lista de DataList es editable ([haga clic para ver la imagen de tamaño completo](performing-batch-updates-cs/_static/image15.png))

## <a name="step-2-adding-an-update-all-button"></a>Paso 2: agregar un botón Actualizar todo

Aunque cada proveedor de la figura 5 tiene los campos dirección, ciudad y país mostrados en un cuadro de texto, actualmente no hay ningún botón Actualizar disponible. En lugar de tener un botón actualizar por elemento, con listas de elementos de documentos totalmente modificables, normalmente hay un solo botón Actualizar todo en la página que, cuando se hace clic en él, actualiza *todos* los registros de la lista de elementos. En este tutorial, vamos a agregar dos botones actualizar todos: uno en la parte superior de la página y otro en la parte inferior (aunque hacer clic en cualquiera de los botones tendrá el mismo efecto).

Comience agregando un control Web Button encima de DataList y establezca su propiedad `ID` en `UpdateAll1`. A continuación, agregue el segundo control Web Button debajo de DataList y establezca su `ID` en `UpdateAll2`. Establezca las propiedades del `Text` de los dos botones para actualizar todos. Por último, cree controladores de eventos para los dos botones `Click` eventos. En lugar de duplicar la lógica de actualización en cada uno de los controladores de eventos, se debe refactorizar esa lógica a un tercer método, `UpdateAllSupplierAddresses`, con los controladores de eventos simplemente invocando este tercer método.

[!code-csharp[Main](performing-batch-updates-cs/samples/sample3.cs)]

La figura 6 muestra la página después de agregar los botones actualizar todos.

[![se han agregado dos botones actualizar todos a la página](performing-batch-updates-cs/_static/image17.png)](performing-batch-updates-cs/_static/image16.png)

**Figura 6**: se han agregado dos botones actualizar todos a la página ([haga clic para ver la imagen de tamaño completo](performing-batch-updates-cs/_static/image18.png))

## <a name="step-3-updating-all-of-the-suppliers-address-information"></a>Paso 3: actualización de toda la información de dirección de los proveedores

Con todos los elementos de DataList que muestran la interfaz de edición y con la adición de los botones actualizar todo, todo lo que queda es escribir el código para realizar la actualización por lotes. En concreto, es necesario crear un bucle a través de los elementos de DataList y llamar al método `SuppliersBLL` Class s `UpdateSupplierAddress` para cada uno.

Se puede tener acceso a la colección de instancias de `DataListItem` que composición de DataList a través de la [propiedad](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datalist.items.aspx)datalist s`Items`. Con una referencia a un `DataListItem`, podemos obtener el `SupplierID` correspondiente de la colección `DataKeys` y hacer referencia mediante programación a los controles Web TextBox dentro de la `ItemTemplate` como se muestra en el código siguiente:

[!code-csharp[Main](performing-batch-updates-cs/samples/sample4.cs)]

Cuando el usuario hace clic en uno de los botones actualizar todo, el método `UpdateAllSupplierAddresses` recorre en iteración cada `DataListItem` en el `Suppliers` DataList y llama al método `UpdateSupplierAddress` de la clase `SuppliersBLL`, pasando los valores correspondientes. Un valor no especificado para las pasadas de dirección, ciudad o país es un valor de `Nothing` a `UpdateSupplierAddress` (en lugar de una cadena en blanco), lo que da como resultado una `NULL` de base de datos para los campos registro subyacente.

> [!NOTE]
> Como mejora, puede que desee agregar un control Web de etiqueta de estado a la página que proporciona un mensaje de confirmación después de realizar la actualización por lotes.

## <a name="updating-only-those-addresses-that-have-been-modified"></a>Actualizar solo las direcciones que se han modificado

El algoritmo de actualización por lotes utilizado para este tutorial llama al método `UpdateSupplierAddress` para *cada* proveedor de la lista de datos, independientemente de si se ha cambiado su información de dirección. Aunque estas actualizaciones ciegas no suelen ser un problema de rendimiento, pueden conducir a registros superfluos si se vuelven a auditar los cambios en la tabla de base de datos. Por ejemplo, si utiliza desencadenadores para registrar todos los `UPDATE` s en la tabla de `Suppliers` en una tabla de auditoría, cada vez que un usuario haga clic en el botón Actualizar todo, se creará un nuevo registro de auditoría para cada proveedor del sistema, independientemente de si el usuario realizó algún cambio.

Las clases DataTable y DataAdapter de ADO.NET están diseñadas para admitir actualizaciones por lotes en las que solo los registros modificados, eliminados y nuevos generan cualquier comunicación de base de datos. Cada fila de la DataTable tiene una [propiedad`RowState`](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) que indica si la fila se ha agregado a la DataTable, se ha eliminado de ella, se ha modificado o no se ha cambiado. Cuando un objeto DataTable se rellena inicialmente, todas las filas se marcan como sin cambios. Al cambiar el valor de cualquiera de las columnas de fila, la fila se marca como modificada.

En la clase `SuppliersBLL` se actualiza la información de dirección de los proveedores especificados leyendo primero en el registro de proveedor único en un `SuppliersDataTable` y, después, estableciendo los valores de las columnas `Address`, `City`y `Country` mediante el código siguiente:

[!code-csharp[Main](performing-batch-updates-cs/samples/sample5.cs)]

Este código asigna los valores de dirección, ciudad y país pasados en el `SuppliersRow` en el `SuppliersDataTable` independientemente de si los valores han cambiado o no. Estas modificaciones hacen que la propiedad `SuppliersRow` s `RowState` se marque como modificada. Cuando se llama al método Data Access Layer s `Update`, observa que se ha modificado el `SupplierRow` y, por tanto, envía un comando `UPDATE` a la base de datos.

No obstante, supongamos que agregamos código a este método para asignar solo los valores de dirección, ciudad y país pasados, si difieren de los valores existentes de `SuppliersRow` s. En el caso de que la dirección, la ciudad y el país sean los mismos que los datos existentes, no se realizará ningún cambio y el `SupplierRow` s `RowState` quedará marcado como sin cambios. El resultado neto es que, cuando se llama al método DAL s `Update`, no se realizará ninguna llamada a la base de datos porque no se ha modificado el `SuppliersRow`.

Para realizar este cambio, reemplace las instrucciones que asignan ciegamente los valores de dirección, ciudad y país pasados con el código siguiente:

[!code-csharp[Main](performing-batch-updates-cs/samples/sample6.cs)]

Con este código agregado, el método DAL s `Update` envía una instrucción `UPDATE` a la base de datos solo para aquellos registros cuyos valores relacionados con la dirección han cambiado.

Como alternativa, podríamos realizar un seguimiento de si hay alguna diferencia entre los campos de dirección pasados y los datos de la base de datos y, si no hay ninguno, simplemente omitir la llamada al método de `Update` de DAL. Este enfoque funciona bien si se vuelve a usar el método de DB Direct, dado que el método de DB Direct no pasa una instancia de `SuppliersRow` cuyos `RowState` se pueden comprobar para determinar si realmente se necesita una llamada a la base de datos.

> [!NOTE]
> Cada vez que se invoca el método de `UpdateSupplierAddress`, se realiza una llamada a la base de datos para recuperar información sobre el registro actualizado. A continuación, si hay cambios en los datos, se realiza otra llamada a la base de datos para actualizar la fila de la tabla. Este flujo de trabajo se puede optimizar creando una `UpdateSupplierAddress` sobrecarga del método que acepte una instancia de `EmployeesDataTable` que tenga *todos* los cambios de la página `BatchUpdate.aspx`. A continuación, puede realizar una llamada a la base de datos para obtener todos los registros de la tabla `Suppliers`. A continuación, se pueden enumerar los dos conjuntos de cambios y solo se pueden actualizar los registros en los que se han producido cambios.

## <a name="summary"></a>Resumen

En este tutorial, vimos cómo crear una lista de datos totalmente editable, lo que permite al usuario modificar rápidamente la información de la dirección de varios proveedores. Empezamos por definir la interfaz de edición de un control Web TextBox para los valores de dirección, ciudad y país del proveedor en la `ItemTemplate`de lista de elementos. A continuación, hemos agregado los botones actualizar todos por encima y por debajo de DataList. Una vez que un usuario ha realizado sus cambios y ha hecho clic en uno de los botones actualizar todo, se enumeran los `DataListItem` s y se realiza una llamada al método `UpdateSupplierAddress` de la clase `SuppliersBLL`.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Zack Jones y Ken Pespisa. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md)
> [Siguiente](handling-bll-and-dal-level-exceptions-cs.md)
