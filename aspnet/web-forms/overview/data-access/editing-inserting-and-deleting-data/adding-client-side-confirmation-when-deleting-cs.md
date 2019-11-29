---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
title: Agregando confirmación del lado cliente al eliminar (C#) | Microsoft Docs
author: rick-anderson
description: En las interfaces que hemos creado hasta ahora, un usuario puede eliminar accidentalmente los datos haciendo clic en el botón eliminar cuando destinan a hacer clic en el botón Editar. En este t...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: f6e2a12a-2b5e-48fd-8db3-1e94a500c19a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/adding-client-side-confirmation-when-deleting-cs
msc.type: authoredcontent
ms.openlocfilehash: e7d53bc65fdbbfa9ce9bfa5fbdbfa0dea598eebe
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74623561"
---
# <a name="adding-client-side-confirmation-when-deleting-c"></a>Agregar la confirmación del cliente al eliminar (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_22_CS.exe) de la aplicación de ejemplo o [descarga de PDF](adding-client-side-confirmation-when-deleting-cs/_static/datatutorial22cs1.pdf)

> En las interfaces que hemos creado hasta ahora, un usuario puede eliminar accidentalmente los datos haciendo clic en el botón eliminar cuando destinan a hacer clic en el botón Editar. En este tutorial, vamos a agregar un cuadro de diálogo de confirmación del lado cliente que aparece cuando se hace clic en el botón Eliminar.

## <a name="introduction"></a>Introducción

En los últimos tutoriales, vimos cómo usar la arquitectura de la aplicación, ObjectDataSource y los controles Web de datos en conjunto para proporcionar funcionalidades de inserción, edición y eliminación. Las interfaces de eliminación que hemos examinado hasta ahora se han compuesto por un botón eliminar que, al hacer clic en él, produce un postback e invoca el método ObjectDataSource s `Delete()`. A continuación, el método `Delete()` invoca el método configurado desde el nivel de lógica de negocios, que propaga la llamada a la capa de acceso a datos, emitiendo la instrucción `DELETE` real a la base de datos.

Aunque esta interfaz de usuario permite a los visitantes eliminar registros a través de los controles GridView, DetailsView o FormView, carece de cualquier tipo de confirmación cuando el usuario hace clic en el botón Eliminar. Si un usuario hace clic accidentalmente en el botón eliminar cuando destina a hacer clic en editar, se eliminará en su lugar el registro que desea actualizar. Para ayudar a evitar esto, en este tutorial agregaremos un cuadro de diálogo de confirmación del lado cliente que aparece cuando se hace clic en el botón Eliminar.

La función `confirm(string)` de JavaScript muestra su parámetro de entrada de cadena como texto dentro de un cuadro de diálogo modal que viene equipado con dos botones: aceptar y cancelar (vea la figura 1). La función `confirm(string)` devuelve un valor booleano según el botón en el que se haga clic (`true`, si el usuario hace clic en aceptar y `false` si hace clic en Cancelar).

![El método confirm (String) de JavaScript muestra un cuadro de mensajes modal en el lado cliente](adding-client-side-confirmation-when-deleting-cs/_static/image1.png)

**Figura 1**: el método `confirm(string)` de JavaScript muestra un cuadro de mensajes modal en el lado cliente

Durante un envío de formulario, si se devuelve un valor de `false` desde un controlador de eventos del cliente, se cancela el envío del formulario. Con esta característica, podemos hacer que el controlador de eventos Delete Button s Client `onclick` devuelva el valor de una llamada a `confirm("Are you sure you want to delete this product?")`. Si el usuario hace clic en cancelar, `confirm(string)` devolverá FALSE, con lo que el envío del formulario se cancelará. Sin el postback, el producto cuyo botón de eliminación se hizo clic no se eliminará. No obstante, si el usuario hace clic en aceptar en el cuadro de diálogo de confirmación, el PostBack continuará sin alteraciones y se eliminará el producto. Consulte [usar el método de `confirm()` de JavaScript para controlar el envío de formularios](http://www.webreference.com/programming/javascript/confirm/) para obtener más información sobre esta técnica.

Agregar el script de cliente necesario difiere ligeramente si se usan plantillas que cuando se usa un CommandField. Por lo tanto, en este tutorial veremos un ejemplo de FormView y de GridView.

> [!NOTE]
> El uso de técnicas de confirmación del lado cliente, como las descritas en este tutorial, supone que los usuarios visitan los exploradores que admiten JavaScript y que tienen JavaScript habilitado. Si alguna de estas suposiciones no es verdadera para un usuario determinado, al hacer clic en el botón eliminar se producirá inmediatamente un postback (sin mostrar un cuadro de mensaje de confirmación).

## <a name="step-1-creating-a-formview-that-supports-deletion"></a>Paso 1: crear un FormView que admita la eliminación

Comience agregando un FormView a la página de `ConfirmationOnDelete.aspx` de la carpeta `EditInsertDelete`, enlazándolo a un nuevo ObjectDataSource que recupere la información del producto a través del método de `GetProducts()` clase `ProductsBLL`. Configure también ObjectDataSource para que el método de `DeleteProduct(productID)` de la clase `ProductsBLL` se asigne al método ObjectDataSource s `Delete()`; Asegúrese de que las listas desplegables insertar y actualizar pestañas están establecidas en (ninguno). Por último, active la casilla habilitar paginación en la etiqueta inteligente FormView s.

Después de estos pasos, el nuevo marcado declarativo de ObjectDataSource s tendrá el aspecto siguiente:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample1.aspx)]

Como en los ejemplos anteriores que no usan la simultaneidad optimista, dedique un momento a borrar la propiedad `OldValuesParameterFormatString` de ObjectDataSource.

Dado que se ha enlazado a un control ObjectDataSource que solo admite la eliminación, la `ItemTemplate` FormView s solo ofrece el botón eliminar, que carece de los botones nuevo y actualizar. Sin embargo, el marcado declarativo FormView s incluye un `EditItemTemplate` y `InsertItemTemplate`superfluos, que se pueden quitar. Dedique un momento a personalizar el `ItemTemplate` para que solo se muestre un subconjunto de los campos de datos del producto. He configurado el mío para mostrar el nombre del producto en un encabezado `<h3>` por encima de sus nombres de proveedor y categoría (junto con el botón Eliminar).

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample2.aspx)]

Con estos cambios, disponemos de una página web totalmente funcional que permite a los usuarios alternar los productos de uno en uno, con la posibilidad de eliminar un producto simplemente haciendo clic en el botón Eliminar. En la ilustración 2 se muestra una captura de pantalla de nuestro progreso hasta ahora cuando se ve a través de un explorador.

[![el FormView muestra información sobre un único producto](adding-client-side-confirmation-when-deleting-cs/_static/image3.png)](adding-client-side-confirmation-when-deleting-cs/_static/image2.png)

**Figura 2**: el FormView muestra información sobre un único producto ([haga clic para ver la imagen de tamaño completo](adding-client-side-confirmation-when-deleting-cs/_static/image4.png))

## <a name="step-2-calling-the-confirmstring-function-from-the-delete-buttons-client-side-onclick-event"></a>Paso 2: llamar a la función confirm (String) desde el evento onclick del lado cliente de los botones de eliminación

Con el FormView creado, el paso final consiste en configurar el botón eliminar, de modo que cuando se hace clic en él, se invoca la función `confirm(string)` de JavaScript. La adición de un script de cliente a un botón, LinkButton o un evento de `onclick` del lado cliente de ImageButton s se puede realizar mediante el uso de la `OnClientClick property`, que es nuevo en ASP.NET 2,0. Dado que queremos que se devuelva el valor de la función `confirm(string)`, simplemente establezca esta propiedad en: `return confirm('Are you certain that you want to delete this product?');`

Después de este cambio, la sintaxis declarativa eliminar s de LinkButton debe ser similar a la siguiente:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample3.aspx)]

Eso es todo lo que tiene que hacer. En la ilustración 3 se muestra una captura de pantalla de esta confirmación en acción. Al hacer clic en el botón eliminar, aparece el cuadro de diálogo confirmar. Si el usuario hace clic en cancelar, el PostBack se cancela y el producto no se elimina. Sin embargo, si el usuario hace clic en aceptar, el PostBack continúa y se invoca el método ObjectDataSource s `Delete()`, culminando en el registro de la base de datos que se va a eliminar.

> [!NOTE]
> La cadena que se pasa a la función de JavaScript `confirm(string)` está delimitada por apóstrofos (en lugar de comillas). En JavaScript, las cadenas se pueden delimitar con cualquiera de los dos caracteres. Aquí usamos apóstrofos para que los delimitadores de la cadena pasada en `confirm(string)` no presenten una ambigüedad con los delimitadores que se usan para el valor de la propiedad `OnClientClick`.

[![se muestra una confirmación al hacer clic en el botón eliminar](adding-client-side-confirmation-when-deleting-cs/_static/image6.png)](adding-client-side-confirmation-when-deleting-cs/_static/image5.png)

**Figura 3**: ahora se muestra una confirmación al hacer clic en el botón Eliminar ([haga clic para ver la imagen a tamaño completo](adding-client-side-confirmation-when-deleting-cs/_static/image7.png))

## <a name="step-3-configuring-the-onclientclick-property-for-the-delete-button-in-a-commandfield"></a>Paso 3: configurar la propiedad OnClientClick para el botón eliminar en un CommandField

Al trabajar con un botón, LinkButton o ImageButton directamente en una plantilla, se puede asociar un cuadro de diálogo de confirmación simplemente configurando su propiedad `OnClientClick` para devolver los resultados de la función `confirm(string)` de JavaScript. Sin embargo, CommandField, que agrega un campo de botones de eliminación a GridView o DetailsView, no tiene una propiedad `OnClientClick` que se puede establecer mediante declaración. En su lugar, se debe hacer referencia mediante programación al botón eliminar de GridView o a DetailsView en el controlador de eventos `DataBound` y, a continuación, establecer su propiedad `OnClientClick` allí.

> [!NOTE]
> Al establecer la propiedad Delete Button s `OnClientClick` en el controlador de eventos `DataBound` adecuado, se tiene acceso a los datos enlazados al registro actual. Esto significa que podemos ampliar el mensaje de confirmación para incluir detalles sobre el registro en particular, como "¿está seguro de que desea eliminar el producto de Chai?". Esta personalización también es posible en las plantillas que usan la sintaxis de DataBinding.

Para practicar el establecimiento de la propiedad `OnClientClick` de los botones de eliminación en un CommandField, permita agregar un control GridView a la página. Configure este GridView para usar el mismo control ObjectDataSource que el FormView usa. Limite también el BoundFields de GridView para incluir solo el nombre del producto, la categoría y el proveedor. Por último, active la casilla habilitar eliminación de la etiqueta inteligente de GridView s. Esto agregará un CommandField a la colección GridView s `Columns` con su propiedad `ShowDeleteButton` establecida en `true`.

Después de realizar estos cambios, el marcado declarativo de GridView debe ser similar al siguiente:

[!code-aspx[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample4.aspx)]

CommandField contiene una sola instancia de LinkButton de eliminación a la que se puede tener acceso mediante programación desde el controlador de eventos de `RowDataBound` de GridView. Una vez hecho referencia, podemos establecer su propiedad `OnClientClick` en consecuencia. Cree un controlador de eventos para el evento `RowDataBound` mediante el código siguiente:

[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample5.cs)]

Este controlador de eventos funciona con filas de datos (las que tienen el botón Eliminar) y comienza mediante programación haciendo referencia al botón Eliminar. En general, use el siguiente patrón:

[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample6.cs)]

*ButtonType* es el tipo de botón que usa CommandField-Button, LinkButton o ImageButton. De forma predeterminada, CommandField usa LinkButtons, pero se puede personalizar mediante el `ButtonType property`s. *CommandFieldIndex* es el índice ordinal del CommandField dentro de la colección de `Columns` de GridView s, mientras que *controlIndex* es el índice del botón eliminar de la colección CommandField s `Controls`. El valor de *controlIndex* depende de la posición del botón en relación con otros botones de la CommandField. Por ejemplo, si el único botón que se muestra en CommandField es el botón eliminar, use un índice de 0. Sin embargo, si hay un botón Editar que precede al botón eliminar, use un índice de 2. El motivo por el que se usa un índice de 2 es porque el CommandField agrega dos controles antes del botón Eliminar: el botón Editar y un LiteralControl que se usan para agregar algo de espacio entre los botones editar y eliminar.

En nuestro ejemplo concreto, CommandField usa LinkButtons y, al ser el campo situado más a la izquierda, tiene un *commandFieldIndex* de 0. Dado que no hay ningún otro botón, sino el botón eliminar en CommandField, usamos un valor de *controlIndex* de 0.

Después de hacer referencia al botón eliminar en CommandField, se tomará información sobre el producto enlazado a la fila actual de GridView. Por último, se establece el valor de la propiedad Delete Button s `OnClientClick` en el código JavaScript adecuado, que incluye el nombre del producto. Dado que la cadena de JavaScript que se pasa a la función `confirm(string)` está delimitada por apóstrofos, se deben escapar los apóstrofos que aparecen en el nombre del producto. En concreto, los apóstrofos del nombre del producto se incluyen en el carácter de escape "`\'`".

Una vez realizados estos cambios, al hacer clic en un botón eliminar de GridView se muestra un cuadro de diálogo de confirmación personalizado (consulte la figura 4). Como sucede con el cuadro de mensajes de confirmación del FormView, si el usuario hace clic en cancelar, se cancela el postback, lo que impide que se produzca la eliminación.

> [!NOTE]
> Esta técnica también se puede utilizar para tener acceso mediante programación al botón eliminar en CommandField en DetailsView. En el caso de DetailsView, sin embargo, puede crear un controlador de eventos para el evento `DataBound`, ya que DetailsView no tiene un evento `RowDataBound`.

[![hacer clic en el botón eliminar de GridView muestra un cuadro de diálogo de confirmación personalizado](adding-client-side-confirmation-when-deleting-cs/_static/image9.png)](adding-client-side-confirmation-when-deleting-cs/_static/image8.png)

**Figura 4**: hacer clic en el botón eliminar de GridView muestra un cuadro de diálogo de confirmación personalizado ([haga clic para ver la imagen de tamaño completo](adding-client-side-confirmation-when-deleting-cs/_static/image10.png))

## <a name="using-templatefields"></a>Usar TemplateFields

Una de las desventajas de CommandField es que se debe tener acceso a sus botones a través de la indización y que el objeto resultante debe convertirse al tipo de botón adecuado (Button, LinkButton o ImageButton). El uso de "números mágicos" y tipos codificados de forma rígida invita a los problemas que no se pueden detectar hasta el tiempo de ejecución. Por ejemplo, si usted u otro desarrollador agrega nuevos botones al CommandField en algún momento en el futuro (por ejemplo, un botón Editar) o cambia la propiedad `ButtonType`, el código existente se compilará sin errores, pero la visita a la página puede producir una excepción o un comportamiento inesperado, en función de cómo se haya escrito el código y los cambios realizados.

Un enfoque alternativo consiste en convertir GridView y DetailsView s CommandFields en TemplateFields. Esto generará TemplateField con un `ItemTemplate` que tiene un LinkButton (o un botón o un ImageButton) para cada botón de CommandField. Estos botones `OnClientClick` propiedades se pueden asignar de forma declarativa, como vimos con FormView, o se puede obtener acceso mediante programación en el controlador de eventos `DataBound` adecuado con el siguiente patrón:

[!code-csharp[Main](adding-client-side-confirmation-when-deleting-cs/samples/sample7.cs)]

Donde *ControlID* es el valor de la propiedad Button `ID`. Aunque este patrón requiere todavía un tipo codificado de forma rígida para la conversión, elimina la necesidad de indexación, lo que permite que el diseño cambie sin que se produzca un error en tiempo de ejecución.

## <a name="summary"></a>Resumen

La función `confirm(string)` de JavaScript es una técnica utilizada comúnmente para controlar el flujo de trabajo de envío del formulario. Cuando se ejecuta, la función muestra un cuadro de diálogo modal, en el lado cliente, que incluye dos botones, aceptar y cancelar. Si el usuario hace clic en aceptar, la función `confirm(string)` devuelve `true`; al hacer clic en Cancelar se devuelve `false`. Esta funcionalidad, junto con un comportamiento del explorador para cancelar un envío de formulario si un controlador de eventos durante el proceso de envío devuelve `false`, se puede usar para mostrar un cuadro de mensajes de confirmación al eliminar un registro.

La función `confirm(string)` se puede asociar con un controlador de eventos del lado cliente de un control Web Button `onclick` a través de la propiedad control s `OnClientClick`. Cuando se trabaja con un botón eliminar en una plantilla, ya sea en una de las plantillas FormView o en TemplateField en DetailsView o GridView, esta propiedad se puede establecer de forma declarativa o mediante programación, como vimos en este tutorial.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](implementing-optimistic-concurrency-cs.md)
> [Siguiente](limiting-data-modification-functionality-based-on-the-user-cs.md)
