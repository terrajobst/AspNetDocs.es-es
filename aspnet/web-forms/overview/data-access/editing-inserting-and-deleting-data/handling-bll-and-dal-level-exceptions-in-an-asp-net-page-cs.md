---
uid: web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
title: Controlar las excepciones de nivel BLL y DAL en una página ASP.NET (C#) | Microsoft Docs
author: rick-anderson
description: En este tutorial, veremos cómo mostrar un mensaje de error informativo y descriptivo en caso de que se produzca una excepción durante una operación de inserción, actualización o eliminación de...
ms.author: riande
ms.date: 07/17/2006
ms.assetid: 49d8a66c-3ea8-4087-839f-179d1d94512a
msc.legacyurl: /web-forms/overview/data-access/editing-inserting-and-deleting-data/handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b9cdb5af6f33171b191d5a80473c7796eb098d9
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74589327"
---
# <a name="handling-bll--and-dal-level-exceptions-in-an-aspnet-page-c"></a>Controlar las excepciones de nivel BLL y DAL en una página de ASP.NET (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_18_CS.exe) de la aplicación de ejemplo o [descarga de PDF](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/datatutorial18cs1.pdf)

> En este tutorial, veremos cómo mostrar un mensaje de error descriptivo e informativo en caso de que se produzca una excepción durante una operación de inserción, actualización o eliminación de un control Web de datos ASP.NET.

## <a name="introduction"></a>Introducción

El trabajo con datos de una aplicación Web de ASP.NET mediante una arquitectura de aplicación en capas implica los tres pasos generales siguientes:

1. Determine qué método de la capa de lógica de negocios se debe invocar y los valores de parámetro que se van a pasar. Los valores de parámetro pueden estar codificados de forma rígida, asignados mediante programación o entradas especificadas por el usuario.
2. Invoque al método.
3. Procesar los resultados. Cuando se llama a un método BLL que devuelve datos, esto puede implicar enlazar los datos a un control Web de datos. En el caso de los métodos BLL que modifican datos, esto puede incluir la realización de alguna acción basada en un valor devuelto o el control correcto de cualquier excepción que surgiera en el paso 2.

Como vimos en el [tutorial anterior](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md), tanto ObjectDataSource como los controles Web de datos proporcionan puntos de extensibilidad para los pasos 1 y 3. Por ejemplo, GridView desencadena su evento `RowUpdating` antes de asignar sus valores de campo a la colección `UpdateParameters` de ObjectDataSource; su evento `RowUpdated` se genera después de que ObjectDataSource haya completado la operación.

Ya hemos examinado los eventos que se activan durante el paso 1 y han visto cómo se pueden usar para personalizar los parámetros de entrada o cancelar la operación. En este tutorial, haremos la atención a los eventos que se activan una vez completada la operación. Con estos controladores de eventos de nivel posterior podemos, entre otras cosas, determinar si se produjo una excepción durante la operación y controlarla correctamente, mostrando un mensaje de error descriptivo e informativo en la pantalla en lugar de establecer el valor predeterminado en el ASP.NET estándar Página de excepción.

Para ilustrar el trabajo con estos eventos de nivel posterior, vamos a crear una página que muestra los productos en un control GridView modificable. Al actualizar un producto, si se genera una excepción, la página ASP.NET mostrará un mensaje breve sobre el control GridView que explica que se ha producido un problema. Comencemos.

## <a name="step-1-creating-an-editable-gridview-of-products"></a>Paso 1: crear un GridView modificable de productos

En el tutorial anterior, creamos un control GridView modificable con solo dos campos, `ProductName` y `UnitPrice`. Esto requiere crear una sobrecarga adicional para el método `UpdateProduct` de la clase `ProductsBLL`, uno que solo aceptó tres parámetros de entrada (el nombre del producto, el precio por unidad y el ID.) en lugar de un parámetro para cada campo de producto. En este tutorial, vamos a practicar de nuevo esta técnica, creando un GridView modificable que muestra el nombre del producto, la cantidad por unidad, el precio por unidad y las unidades en existencias, pero solo permite editar el nombre, el precio por unidad y las unidades en existencias.

Para dar cabida a este escenario, se necesitará otra sobrecarga del método `UpdateProduct`, uno que acepte cuatro parámetros: el nombre del producto, el precio unitario, las unidades en existencias y el identificador. Agregue el método siguiente a la clase `ProductsBLL`:

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample1.cs)]

Con este método completo, estamos listos para crear la página ASP.NET que permite editar estos cuatro campos de producto concretos. Abra la página `ErrorHandling.aspx` en la carpeta `EditInsertDelete` y agregue un control GridView a la página a través del diseñador. Enlace GridView a un nuevo ObjectDataSource, asignando el método `Select()` al método `GetProducts()` de la clase `ProductsBLL` y el método `Update()` a la sobrecarga `UpdateProduct` que acaba de crear.

[![usar la sobrecarga del método UpdateProduct que acepta cuatro parámetros de entrada](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image2.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image1.png)

**Figura 1**: usar la sobrecarga del método `UpdateProduct` que acepta cuatro parámetros de entrada ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image3.png))

Se creará un ObjectDataSource con una colección de `UpdateParameters` con cuatro parámetros y un control GridView con un campo para cada uno de los campos product. El marcado declarativo de ObjectDataSource asigna a la propiedad `OldValuesParameterFormatString` el valor `original_{0}`, lo que producirá una excepción, ya que la clase BLL no espera que se pase un parámetro de entrada denominado `original_productID`. No olvide quitar esta configuración por completo de la sintaxis declarativa (o establecerla en el valor predeterminado, `{0}`).

A continuación, reduzca el GridView para incluir solo los `ProductName`, `QuantityPerUnit`, `UnitPrice`y `UnitsInStock` BoundFields. También puede aplicar cualquier formato de nivel de campo que considere necesario (como cambiar las propiedades de `HeaderText`).

En el tutorial anterior, hemos visto cómo dar formato al `UnitPrice` BoundField como moneda en modo de solo lectura y en modo de edición. Vamos a hacer lo mismo aquí. Recuerde que esto requiere establecer la propiedad `DataFormatString` de BoundField en `{0:c}`, su propiedad `HtmlEncode` en `false`y su `ApplyFormatInEditMode` en `true`, como se muestra en la figura 2.

[![configurar el modo UnitPrice BoundField para que se muestre como moneda](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image5.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image4.png)

**Figura 2**: configuración de la `UnitPrice` BoundField para que se muestre como moneda ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image6.png))

Dar formato al `UnitPrice` como moneda en la interfaz de edición requiere la creación de un controlador de eventos para el evento `RowUpdating` de GridView que analiza la cadena con formato de moneda en un valor `decimal`. Recuerde que el controlador de eventos `RowUpdating` del último tutorial también se ha comprobado para asegurarse de que el usuario proporcionó un valor `UnitPrice`. Sin embargo, para este tutorial, vamos a permitir que el usuario omita el precio.

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample2.cs)]

El control GridView incluye un `QuantityPerUnit` BoundField, pero este BoundField debe ser solo para fines de presentación y no debe ser editable por el usuario. Para organizar esto, basta con establecer la propiedad BoundFields ' `ReadOnly` en `true`.

[![hacer que el QuantityPerUnit no sea de solo lectura](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image8.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image7.png)

**Figura 3**: convertir el `QuantityPerUnit` BoundField en modo de solo lectura ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image9.png))

Por último, active la casilla habilitar edición de la etiqueta inteligente de GridView. Después de completar estos pasos, el diseñador de la página `ErrorHandling.aspx` debe tener un aspecto similar al de la figura 4.

[![quitar todos los BoundFields necesarios y activar la casilla habilitar edición](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image11.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image10.png)

**Figura 4**: Quite todas las BoundFields, excepto las necesarias, y active la casilla habilitar edición ([haga clic para ver la imagen a tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image12.png))

En este momento, tenemos una lista de todos los campos de `ProductName`, `QuantityPerUnit`, `UnitPrice`y `UnitsInStock` de los productos. sin embargo, solo se pueden editar los campos `ProductName`, `UnitPrice`y `UnitsInStock`.

[Los usuarios de ![ahora pueden editar fácilmente los nombres, precios y unidades de los productos en los campos de acciones.](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image14.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image13.png)

**Figura 5**: ahora, los usuarios pueden editar fácilmente los nombres, precios y unidades de los productos en los campos de existencias ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image15.png))

## <a name="step-2-gracefully-handling-dal-level-exceptions"></a>Paso 2: controlar correctamente las excepciones de nivel DAL

Aunque el GridView editable funciona bien cuando los usuarios escriben valores válidos para el nombre, el precio y las unidades de los productos editados en existencias, la entrada de valores no válidos produce una excepción. Por ejemplo, si se omite el valor de `ProductName`, se produce una [NoNullAllowedException](https://msdn.microsoft.com/library/default.asp?url=/library/cpref/html/frlrfsystemdatanonullallowedexceptionclasstopic.asp) , ya que la propiedad `ProductName` de la clase `ProductsRow` tiene su propiedad `AllowDBNull` establecida en `false`; Si la base de datos está inactiva, el TableAdapter producirá una `SqlException` al intentar conectarse a la base de datos. Sin realizar ninguna acción, estas excepciones se propagan desde la capa de acceso a datos hasta la capa de lógica de negocios, luego hasta la página ASP.NET y, finalmente, hasta el tiempo de ejecución de ASP.NET.

En función de cómo esté configurada la aplicación web y de si está visitando o no la aplicación desde `localhost`, una excepción no controlada puede dar lugar a una página de error de servidor genérico, un informe de error detallado o una página web fácil de ver. Vea [control de errores de aplicaciones web en ASP.net](http://www.15seconds.com/issue/030102.htm) y el [elemento customErrors](https://msdn.microsoft.com/library/h0hfz6fc(VS.80).aspx) para obtener más información sobre cómo responde el tiempo de ejecución de ASP.net a una excepción no detectada.

La figura 6 muestra la pantalla que se encuentra al intentar actualizar un producto sin especificar el valor de `ProductName`. Este es el informe de errores detallado predeterminado que se muestra al llegar `localhost`.

[![si se omite el nombre del producto, se mostrarán los detalles de la excepción](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image17.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image16.png)

**Figura 6**: si se omite el nombre del producto, se mostrarán los detalles de la excepción ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image18.png))

Aunque estos detalles de la excepción son útiles cuando se prueba una aplicación, la presentación de un usuario final con una pantalla de este tipo en el caso de una excepción es menor que la ideal. Es probable que un usuario final no sepa qué es un `NoNullAllowedException` o por qué se produjo. Un mejor enfoque consiste en presentar al usuario un mensaje más descriptivo que explique que hubo problemas al intentar actualizar el producto.

Si se produce una excepción al realizar la operación, los eventos de nivel posterior tanto en ObjectDataSource como en el control Web de datos proporcionan un medio para detectarlo y cancelar la excepción de la propagación hasta el tiempo de ejecución de ASP.NET. En nuestro ejemplo, vamos a crear un controlador de eventos para el evento `RowUpdated` de GridView que determina si se ha desencadenado una excepción y, en caso afirmativo, muestra los detalles de la excepción en un control Web de etiqueta.

Comience agregando una etiqueta a la página ASP.NET, estableciendo su propiedad `ID` en `ExceptionDetails` y borrando su propiedad `Text`. Para dibujar el ojo del usuario en este mensaje, establezca su propiedad `CssClass` en `Warning`, que es una clase CSS que se ha agregado al archivo `Styles.css` en el tutorial anterior. Recuerde que esta clase CSS hace que el texto de la etiqueta se muestre en una fuente roja, cursiva, negrita, extra grande.

[![agregar un control Web de etiqueta a la página](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image20.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image19.png)

**Figura 7**: agregar un control Web de etiqueta a la página ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image21.png))

Como queremos que este control Web de etiqueta solo esté visible inmediatamente después de que se produzca una excepción, establezca su propiedad `Visible` en false en el controlador de eventos `Page_Load`:

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample3.cs)]

Con este código, en la primera página visita y postbacks posteriores, el control `ExceptionDetails` tendrá su propiedad `Visible` establecida en `false`. En el caso de una excepción de nivel DAL o BLL, que se puede detectar en el controlador de eventos de `RowUpdated` de GridView, se establecerá la propiedad `Visible` del control `ExceptionDetails` en true. Dado que los controladores de eventos de control Web se producen después de la `Page_Load` controlador de eventos en el ciclo de vida de la página, se muestra la etiqueta. Sin embargo, en el siguiente postback, el controlador de eventos `Page_Load` revertirá la propiedad `Visible` a `false`y la ocultará de nuevo.

> [!NOTE]
> Como alternativa, podríamos quitar la necesidad de establecer la propiedad `Visible` del control `ExceptionDetails` en `Page_Load` asignando su propiedad `Visible` `false` en la sintaxis declarativa y deshabilitando su estado de vista (estableciendo su propiedad `EnableViewState` en `false`). Usaremos este enfoque alternativo en un tutorial futuro.

Con el control etiqueta agregado, el siguiente paso es crear el controlador de eventos para el evento `RowUpdated` de GridView. Seleccione GridView en el diseñador, vaya al ventana Propiedades y haga clic en el icono de rayo, donde se muestran los eventos de GridView. Ya debe haber una entrada para el evento `RowUpdating` de GridView, ya que hemos creado un controlador de eventos para este evento anteriormente en este tutorial. Cree también un controlador de eventos para el evento `RowUpdated`.

![Crear un controlador de eventos para el evento RowUpdated de GridView](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image22.png)

**Figura 8**: creación de un controlador de eventos para el evento `RowUpdated` de GridView

> [!NOTE]
> También puede crear el controlador de eventos a través de las listas desplegables en la parte superior del archivo de clase de código subyacente. Seleccione GridView en la lista desplegable de la izquierda y el evento `RowUpdated` de la derecha.

Al crear este controlador de eventos, se agregará el código siguiente a la clase de código subyacente de la página ASP.NET:

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample4.cs)]

Este segundo parámetro de entrada del controlador de eventos es un objeto de tipo [GridViewUpdatedEventArgs](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridviewupdatedeventargs.aspx), que tiene tres propiedades de interés para controlar las excepciones:

- `Exception` una referencia a la excepción producida; Si no se ha producido ninguna excepción, esta propiedad tendrá un valor de `null`
- `ExceptionHandled` un valor booleano que indica si la excepción se ha controlado o no en el controlador de eventos `RowUpdated`; Si `false` (valor predeterminado), se vuelve a producir la excepción, percolating hasta el tiempo de ejecución de ASP.NET
- `KeepInEditMode` si se establece en `true` la fila de GridView modificada permanece en modo de edición; Si `false` (valor predeterminado), la fila de GridView vuelve a su modo de solo lectura.

Nuestro código, a continuación, debe comprobar si `Exception` no está `null`, lo que significa que se produjo una excepción al realizar la operación. Si este es el caso, queremos:

- Mostrar un mensaje descriptivo en la etiqueta de `ExceptionDetails`
- Indicar que se ha controlado la excepción
- Mantener la fila de GridView en modo de edición

En el código siguiente se logran estos objetivos:

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample5.cs)]

Este controlador de eventos comienza comprobando si `e.Exception` se `null`. Si no es así, la propiedad `Visible` de la etiqueta de `ExceptionDetails` se establece en `true` y su propiedad `Text` en "se produjo un problema al actualizar el producto". Los detalles de la excepción real que se produjo residen en la propiedad `InnerException` del objeto `e.Exception`. Se examina esta excepción interna y, si es de un tipo determinado, se anexa un mensaje adicional útil a la propiedad `Text` de la etiqueta de `ExceptionDetails`. Por último, las propiedades `ExceptionHandled` y `KeepInEditMode` se establecen en `true`.

En la ilustración 9 se muestra una captura de pantalla de esta página cuando se omite el nombre del producto; En la figura 10 se muestran los resultados al especificar un valor de `UnitPrice` no válido (-50).

[![ProductName BoundField debe contener un valor](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image24.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image23.png)

**Figura 9**: el `ProductName` BoundField debe contener un valor ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image25.png))

[No se permiten ![valores de UnitPrice negativos](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image27.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image26.png)

**Figura 10**: no se permiten valores de `UnitPrice` negativos ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image28.png))

Al establecer la propiedad `e.ExceptionHandled` en `true`, el controlador de eventos `RowUpdated` ha indicado que ha controlado la excepción. Por lo tanto, la excepción no se propagará hasta el tiempo de ejecución de ASP.NET.

> [!NOTE]
> Las figuras 9 y 10 muestran una forma estable de controlar las excepciones que se producen debido a una entrada de usuario no válida. Idealmente, sin embargo, una entrada no válida nunca alcanzará la capa de lógica de negocios en primer lugar, ya que la página ASP.NET debe asegurarse de que las entradas del usuario son válidas antes de invocar el método `UpdateProduct` de la clase `ProductsBLL`. En el tutorial siguiente, veremos cómo agregar controles de validación a las interfaces de edición e inserción para asegurarse de que los datos enviados a la capa de lógica de negocios se ajustan a las reglas de negocios. Los controles de validación no solo impiden la invocación del método `UpdateProduct` hasta que los datos proporcionados por el usuario son válidos, pero también proporcionan una experiencia de usuario más informativa para identificar problemas de entrada de datos.

## <a name="step-3-gracefully-handling-bll-level-exceptions"></a>Paso 3: controlar correctamente las excepciones de nivel BLL

Al insertar, actualizar o eliminar datos, la capa de acceso a datos puede producir una excepción en caso de que se produzca un error relacionado con datos. Es posible que la base de datos esté sin conexión, que no se haya especificado un valor en una columna de tabla de base de datos necesaria o que se haya infringido una restricción de nivel de tabla. Además de las excepciones relacionadas estrictamente con datos, el nivel de lógica de negocios puede utilizar excepciones para indicar cuándo se han infringido las reglas de negocios. En el tutorial [creación de una capa de lógica de negocios](../introduction/creating-a-business-logic-layer-cs.md) , por ejemplo, se ha agregado una comprobación de regla de negocios a la sobrecarga de `UpdateProduct` original. En concreto, si el usuario estaba marcando un producto como discontinuo, es necesario que el producto no sea el único proporcionado por su proveedor. Si se infringió esta condición, se produjo una `ApplicationException`.

Para la `UpdateProduct` sobrecarga creada en este tutorial, vamos a agregar una regla de negocios que prohibe que el campo `UnitPrice` se establezca en un valor nuevo que sea más del doble del valor de la `UnitPrice` original. Para ello, ajuste la sobrecarga de `UpdateProduct` de modo que realice esta comprobación y produzca una `ApplicationException` si se infringe la regla. El método actualizado es el siguiente:

[!code-csharp[Main](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/samples/sample6.cs)]

Con este cambio, cualquier actualización de precio que sea más del doble del precio existente hará que se produzca una `ApplicationException`. Al igual que ocurre con la excepción que se genera desde la capa DAL, esta `ApplicationException` de BLL-elevada se puede detectar y controlar en el controlador de eventos `RowUpdated` de GridView. De hecho, el código del controlador de eventos `RowUpdated`, tal como se escribe, detectará correctamente esta excepción y mostrará el valor de la propiedad `Message` del `ApplicationException`. En la figura 11 se muestra una captura de pantalla cuando un usuario intenta actualizar el precio de Chai a $50,00, que es más que el doble del precio actual de $19,95.

[![las reglas de negocios no permiten un aumento del precio más del doble del precio de un producto](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image30.png)](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image29.png)

**Figura 11**: el precio de las reglas de negocio no permite aumentar el precio de un producto más del doble ([haga clic para ver la imagen de tamaño completo](handling-bll-and-dal-level-exceptions-in-an-asp-net-page-cs/_static/image31.png))

> [!NOTE]
> Idealmente, nuestras reglas de lógica de negocios se refactorizarán de las sobrecargas del método `UpdateProduct` y en un método común. Esto se deja como un ejercicio para el lector.

## <a name="summary"></a>Resumen

Durante las operaciones de inserción, actualización y eliminación, tanto el control Web de datos como ObjectDataSource implicaban desencadenar eventos previos y posteriores que devuelven la operación real. Como vimos en este tutorial y en el anterior, al trabajar con un objeto GridView editable, el evento `RowUpdating` de GridView se activa, seguido del evento `Updating` de ObjectDataSource, en el que el comando de actualización se realiza en el objeto subyacente de ObjectDataSource. Una vez finalizada la operación, se desencadena el evento `Updated` de ObjectDataSource, seguido del evento `RowUpdated` de GridView.

Podemos crear controladores de eventos para los eventos de nivel anterior con el fin de personalizar los parámetros de entrada o de los eventos de nivel posterior para poder inspeccionar y responder a los resultados de la operación. Los controladores de eventos de nivel posterior suelen usarse para detectar si se produjo una excepción durante la operación. En el caso de una excepción, estos controladores de eventos de nivel posterior pueden controlar opcionalmente la excepción por su cuenta. En este tutorial, hemos visto cómo controlar este tipo de excepción al mostrar un mensaje de error descriptivo.

En el siguiente tutorial, veremos cómo reducir la probabilidad de que surjan problemas de formato de datos (por ejemplo, al escribir una `UnitPrice`negativa). En concreto, veremos cómo agregar controles de validación a las interfaces de edición e inserción.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor del cliente potencial de este tutorial fue Liz Shulok. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)
> [Siguiente](adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)
