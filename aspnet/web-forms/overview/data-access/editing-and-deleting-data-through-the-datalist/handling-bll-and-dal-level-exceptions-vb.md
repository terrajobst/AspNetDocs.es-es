---
uid: web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
title: Controlar las excepciones de nivel BLL y DAL (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo tactfully controlar las excepciones que se producen durante un flujo de trabajo de actualización de DataList editable.
ms.author: riande
ms.date: 10/30/2006
ms.assetid: ca665073-b379-4239-9404-f597663ca65e
msc.legacyurl: /web-forms/overview/data-access/editing-and-deleting-data-through-the-datalist/handling-bll-and-dal-level-exceptions-vb
msc.type: authoredcontent
ms.openlocfilehash: 319ab44f2e65afc77f6f89ca8aa58c529f40d05c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74624259"
---
# <a name="handling-bll--and-dal-level-exceptions-vb"></a>Controlar las excepciones de nivel BLL y DAL (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_38_VB.exe) de la aplicación de ejemplo o [descarga de PDF](handling-bll-and-dal-level-exceptions-vb/_static/datatutorial38vb1.pdf)

> En este tutorial, veremos cómo tactfully controlar las excepciones que se producen durante un flujo de trabajo de actualización de DataList editable.

## <a name="introduction"></a>Introducción

En la [información general sobre la edición y eliminación de datos en el](an-overview-of-editing-and-deleting-data-in-the-datalist-cs.md) tutorial de DataList, creamos una lista de datos que ofrecía capacidades sencillas de edición y eliminación. Aunque es totalmente funcional, apenas es fácil de utilizar, ya que los errores que se produjeron durante el proceso de edición o eliminación dieron lugar a una excepción no controlada. Por ejemplo, si se omite el nombre del producto o, cuando se edita un producto, al escribir un valor de precio de muy asequible, se produce una excepción. Puesto que esta excepción no se detecta en el código, se propaga hasta el tiempo de ejecución de ASP.NET, que muestra los detalles de la excepción en la Página Web.

Como vimos en la administración de las [excepciones de nivel BLL y Dal en un tutorial de página de ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs.md) , si se genera una excepción a partir de las profundidades de las capas de lógica de negocios o de acceso a datos, los detalles de la excepción se devuelven a ObjectDataSource y luego a GridView. Vimos cómo controlar correctamente estas excepciones creando `Updated` o `RowUpdated` controladores de eventos para ObjectDataSource o GridView, comprobando una excepción y, a continuación, indicando que se controló la excepción.

Sin embargo, nuestros tutoriales de DataList no usan ObjectDataSource para actualizar y eliminar datos. En su lugar, estamos trabajando directamente con el BLL. Para detectar las excepciones que se originan en BLL o DAL, es necesario implementar el código de control de excepciones en el código subyacente de la página de ASP.NET. En este tutorial, veremos cómo más tactfully controlar las excepciones que se producen durante un flujo de trabajo de actualización de DataList editable.

> [!NOTE]
> En la *información general de la edición y eliminación de datos en el* tutorial de DataList se han analizado diferentes técnicas para editar y eliminar datos de la lista de datos, algunas técnicas implicadas en el uso de ObjectDataSource para actualizar y eliminar. Si emplea estas técnicas, puede controlar las excepciones de BLL o DAL a través de los controladores de eventos `Updated` o `Deleted` de ObjectDataSource.

## <a name="step-1-creating-an-editable-datalist"></a>Paso 1: crear una lista de DataList modificable

Antes de preocuparnos por el control de las excepciones que se producen durante el flujo de trabajo de actualización, primero debe crear una lista de controles editable. Abra la página `ErrorHandling.aspx` de la carpeta `EditDeleteDataList`, agregue una lista de propiedades al diseñador, establezca su propiedad `ID` en `Products`y agregue un nuevo ObjectDataSource denominado `ProductsDataSource`. Configure ObjectDataSource para usar el método `ProductsBLL` Class s `GetProducts()` para seleccionar registros; establezca las listas desplegables de las pestañas insertar, actualizar y eliminar en (ninguna).

[![devolver la información del producto mediante el método GetProducts ()](handling-bll-and-dal-level-exceptions-vb/_static/image2.png)](handling-bll-and-dal-level-exceptions-vb/_static/image1.png)

**Figura 1**: devolver la información del producto mediante el método `GetProducts()` ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-vb/_static/image3.png))

Después de completar el Asistente de ObjectDataSource, Visual Studio creará automáticamente una `ItemTemplate` para la lista de objetos. Reemplácelo por un `ItemTemplate` que muestre el nombre y el precio de cada producto e incluya un botón Editar. A continuación, cree un `EditItemTemplate` con un control Web de cuadro de texto para los botones nombre y precio, y actualizar y cancelar. Por último, establezca la propiedad DataList s `RepeatColumns` en 2.

Después de estos cambios, el marcado declarativo de la página debe ser similar al siguiente. Haga doble comprobación para asegurarse de que los botones editar, cancelar y actualizar tienen sus propiedades `CommandName` establecidas en editar, cancelar y actualizar, respectivamente.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample1.aspx)]

> [!NOTE]
> En este tutorial, se debe habilitar el estado de vista DataList s.

Dedique un momento a ver nuestro progreso a través de un explorador (consulte la figura 2).

[![cada producto incluye un botón Editar](handling-bll-and-dal-level-exceptions-vb/_static/image5.png)](handling-bll-and-dal-level-exceptions-vb/_static/image4.png)

**Figura 2**: cada producto incluye un botón Editar ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-vb/_static/image6.png))

En la actualidad, el botón Editar solo produce un postback, pero el producto no se puede modificar. Para habilitar la edición, es necesario crear controladores de eventos para los eventos `EditCommand`, `CancelCommand`y `UpdateCommand` de DataList. Los eventos `EditCommand` y `CancelCommand` simplemente actualizan la propiedad DataList s `EditItemIndex` y vuelve a enlazar los datos a DataList:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample2.vb)]

El controlador de eventos `UpdateCommand` es un poco más complicado. Debe leer en los `ProductID` de productos editados de la colección de `DataKeys` junto con el nombre y el precio del producto en los cuadros de texto de la `EditItemTemplate`y, a continuación, llamar al método de la clase `ProductsBLL` s `UpdateProduct` antes de devolver el estado de la lista de objetos a su estado anterior a la edición.

Por ahora, vamos a usar el mismo código exacto del controlador de eventos `UpdateCommand` en la *información general sobre la edición y eliminación de datos en el* tutorial de DataList. Agregaremos el código para controlar correctamente las excepciones en el paso 2.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample3.vb)]

En el caso de una entrada no válida que puede tener el formato de un precio por unidad con un formato incorrecto, un valor de precio unitario no válido como-$5,00 o la omisión del nombre del producto se generará una excepción. Dado que el controlador de eventos `UpdateCommand` no incluye ningún código de control de excepciones en este momento, la excepción se propagará hasta el tiempo de ejecución de ASP.NET, donde se mostrará al usuario final (vea la figura 3).

![Cuando se produce una excepción no controlada, el usuario final ve una página de error](handling-bll-and-dal-level-exceptions-vb/_static/image7.png)

**Figura 3**: cuando se produce una excepción no controlada, el usuario final ve una página de error

## <a name="step-2-gracefully-handling-exceptions-in-the-updatecommand-event-handler"></a>Paso 2: controlar las excepciones correctamente en el controlador de eventos UpdateCommand

Durante el flujo de trabajo de actualización, pueden producirse excepciones en el controlador de eventos `UpdateCommand`, el BLL o el DAL. Por ejemplo, si un usuario especifica un precio de demasiado caro, la instrucción `Decimal.Parse` del controlador de eventos `UpdateCommand` producirá una excepción `FormatException`. Si el usuario omite el nombre del producto o si el precio tiene un valor negativo, el DAL producirá una excepción.

Cuando se produce una excepción, queremos mostrar un mensaje informativo dentro de la propia página. Agregue un control Web de etiqueta a la página cuya `ID` esté establecida en `ExceptionDetails`. Configure el texto de la etiqueta para que se muestre en una fuente roja, extra grande, negrita y cursiva asignando su `CssClass` propiedad a la clase `Warning` CSS, que se define en el archivo `Styles.css`.

Cuando se produce un error, solo queremos que la etiqueta se muestre una vez. Es decir, en postbacks posteriores, el mensaje de advertencia etiqueta s debería desaparecer. Esto puede realizarse mediante la eliminación de la propiedad etiqueta s `Text` o la configuración de su propiedad `Visible` en `False` en el controlador de eventos `Page_Load` (como hemos vuelto en el control de las [excepciones de nivel BLL y Dal en un tutorial de página de ASP.net](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) ) o deshabilitando la etiqueta compatibilidad con el estado de vista. Dejar que use la última opción.

[!code-aspx[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample4.aspx)]

Cuando se produzca una excepción, asignaremos los detalles de la excepción a la propiedad `ExceptionDetails` Label control s `Text`. Puesto que su estado de vista está deshabilitado, en los postbacks subsiguientes se perderán los cambios de programación de la propiedad `Text` s y se revertirá al texto predeterminado (una cadena vacía), lo que ocultará el mensaje de advertencia.

Para determinar cuándo se ha producido un error con el fin de mostrar un mensaje útil en la página, es necesario agregar un bloque `Try ... Catch` al controlador de eventos `UpdateCommand`. La parte `Try` contiene código que puede conducir a una excepción, mientras que el bloque de `Catch` contiene código que se ejecuta en el caso de una excepción. Consulte la sección [aspectos básicos del control de excepciones](https://msdn.microsoft.com/library/2w8f0bss.aspx) en la documentación de .NET Framework para obtener más información sobre el bloque de `Try ... Catch`.

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample5.vb)]

Cuando se produce una excepción de cualquier tipo mediante el código dentro del bloque `Try`, se iniciará la ejecución del código del `Catch` bloque s. El tipo de excepción que se produce `DbException`, `NoNullAllowedException`, `ArgumentException`, etc. depende de lo que, exactamente, ha precipitado el error en primer lugar. Si hay un problema en el nivel de base de datos, se producirá una `DbException`. Si se especifica un valor no válido para los campos `UnitPrice`, `UnitsInStock`, `UnitsOnOrder`o `ReorderLevel`, se producirá una `ArgumentException`, como hemos agregado código para validar estos valores de campo en la clase `ProductsDataTable` (consulte el tutorial [creación de una capa de lógica de negocios](../introduction/creating-a-business-logic-layer-vb.md) ).

Podemos proporcionar una explicación más útil al usuario final al basar el texto del mensaje en el tipo de excepción detectada. El siguiente código que se usó en un formulario prácticamente idéntico en el [control de excepciones de nivel BLL y Dal en un](../editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-vb.md) tutorial de página de ASP.net proporciona este nivel de detalle:

[!code-vb[Main](handling-bll-and-dal-level-exceptions-vb/samples/sample6.vb)]

Para completar este tutorial, basta con llamar al método `DisplayExceptionDetails` desde el bloque `Catch` pasando la instancia de `Exception` detectada (`ex`).

Con el bloque `Try ... Catch` en su lugar, a los usuarios se les presenta un mensaje de error más informativo, como se muestra en las figuras 4 y 5. Tenga en cuenta que, en el caso de una excepción, el DataList permanece en modo de edición. Esto se debe a que, una vez que se produce la excepción, el flujo de control se redirige inmediatamente al bloque `Catch`, omitiendo el código que devuelve la lista de controles de bits a su estado de edición previa.

[![se muestra un mensaje de error si un usuario omite un campo obligatorio](handling-bll-and-dal-level-exceptions-vb/_static/image9.png)](handling-bll-and-dal-level-exceptions-vb/_static/image8.png)

**Figura 4**: se muestra un mensaje de error si un usuario omite un campo obligatorio ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-vb/_static/image10.png))

[![se muestra un mensaje de error al especificar un precio negativo](handling-bll-and-dal-level-exceptions-vb/_static/image12.png)](handling-bll-and-dal-level-exceptions-vb/_static/image11.png)

**Figura 5**: se muestra un mensaje de error al escribir un precio negativo ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-vb/_static/image13.png))

## <a name="summary"></a>Resumen

GridView y ObjectDataSource proporcionan controladores de eventos de nivel posterior que incluyen información sobre las excepciones que se generaron durante el flujo de trabajo de actualización y eliminación, así como las propiedades que se pueden establecer para indicar si la excepción se ha reciben. Sin embargo, estas características no están disponibles cuando se trabaja con DataList y se usa la capa BLL directamente. En su lugar, somos responsables de implementar el control de excepciones.

En este tutorial, vimos cómo agregar el control de excepciones a un flujo de trabajo de actualización de DataList editable agregando un bloque `Try ... Catch` al controlador de eventos `UpdateCommand`. Si se produce una excepción durante el flujo de trabajo de actualización, el `Catch` bloquear s código se ejecuta y muestra información útil en la etiqueta de `ExceptionDetails`.

En este momento, la lista de DataList no realiza ningún esfuerzo para impedir que se produzcan excepciones en el primer lugar. Aunque sabemos que un precio negativo producirá una excepción, todavía no hemos agregado ninguna funcionalidad para impedir proactivamente que un usuario escriba dicha entrada no válida. En el siguiente tutorial, veremos cómo ayudar a reducir las excepciones producidas por la entrada de usuario no válida agregando controles de validación en el `EditItemTemplate`.

¡ Feliz programación!

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Instrucciones de diseño de excepciones](https://msdn.microsoft.com/library/ms298399.aspx)
- [Módulos y controladores de registro de errores (ELMAH)](http://workspaces.gotdotnet.com/elmah) (una biblioteca de código abierto para registrar errores)
- [Enterprise Library para .NET Framework 2,0](https://www.microsoft.com/downloads/details.aspx?familyid=5A14E870-406B-4F2A-B723-97BA84AE80B5&amp;displaylang=en) (incluye el bloque de aplicaciones de administración de excepciones)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Ken Pespisa. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](performing-batch-updates-vb.md)
> [Siguiente](adding-validation-controls-to-the-datalist-s-editing-interface-vb.md)
