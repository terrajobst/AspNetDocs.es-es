---
uid: web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
title: Agregar una columna GridView de botones de radioC#() | Microsoft Docs
author: rick-anderson
description: En este tutorial se examina cómo agregar una columna de botones de radio a un control GridView para proporcionar al usuario una forma más intuitiva de seleccionar una sola fila de...
ms.author: riande
ms.date: 03/06/2007
ms.assetid: 32377145-ec25-4715-8370-a1c590a331d5
msc.legacyurl: /web-forms/overview/data-access/enhancing-the-gridview/adding-a-gridview-column-of-radio-buttons-cs
msc.type: authoredcontent
ms.openlocfilehash: b59cc64b14c6414e6558fdb8a281644db8386701
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74593349"
---
# <a name="adding-a-gridview-column-of-radio-buttons-c"></a>Agregar una columna GridView de botones de radio (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_51_CS.exe) de la aplicación de ejemplo o [descarga de PDF](adding-a-gridview-column-of-radio-buttons-cs/_static/datatutorial51cs1.pdf)

> En este tutorial se examina cómo agregar una columna de botones de radio a un control GridView para proporcionar al usuario una forma más intuitiva de seleccionar una sola fila de GridView.

## <a name="introduction"></a>Introducción

El control GridView ofrece una gran cantidad de funcionalidad integrada. Incluye varios campos diferentes para mostrar texto, imágenes, hipervínculos y botones. Admite plantillas para una personalización adicional. Con unos pocos clics del mouse, es posible crear un control GridView en el que se pueda seleccionar cada fila a través de un botón, o bien habilitar la edición o la eliminación de capacidades. A pesar de la gran cantidad de características proporcionadas, a menudo habrá situaciones en las que será necesario agregar características adicionales no compatibles. En este tutorial y en los dos siguientes examinaremos cómo mejorar la funcionalidad de GridView para incluir características adicionales.

Este tutorial y el siguiente se centran en mejorar el proceso de selección de filas. Tal y como se ha examinado en el [maestro y detalles mediante un GridView maestro seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md), podemos agregar un CommandField a GridView que incluya un botón seleccionar. Al hacer clic en él, aparece un postback y la propiedad GridView s `SelectedIndex` se actualiza en el índice de la fila cuyo botón seleccionar se hizo clic. En el tutorial [maestro y detalles mediante un control GridView de la página maestra seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) , vimos cómo usar esta característica para mostrar los detalles de la fila de GridView seleccionada.

Aunque el botón seleccionar funciona en muchas situaciones, puede que no funcione también para otros usuarios. En lugar de usar un botón, se suelen usar otros dos elementos de la interfaz de usuario para la selección: el botón de radio y la casilla. Podemos aumentar el control GridView para que en lugar de un botón seleccionar, cada fila contenga un botón de radio o una casilla. En escenarios en los que el usuario solo puede seleccionar uno de los registros de GridView, es posible que se prefiera el botón de radio para el botón seleccionar. En situaciones en las que el usuario puede seleccionar potencialmente varios registros como en una aplicación de correo electrónico basada en Web, donde un usuario podría querer seleccionar varios mensajes para eliminar la casilla ofrece una funcionalidad que no está disponible en el botón seleccionar o el botón de radio interfaces de usuario.

En este tutorial se examina cómo agregar una columna de botones de radio a GridView. El tutorial de continuación explora el uso de las casillas.

## <a name="step-1-creating-the-enhancing-the-gridview-web-pages"></a>Paso 1: crear la mejora de las páginas web de GridView

Antes de empezar a mejorar GridView para incluir una columna de botones de radio, vamos a crear las páginas de ASP.NET en nuestro proyecto de sitio web que necesitamos para este tutorial y los dos siguientes. Comience agregando una nueva carpeta denominada `EnhancedGridView`. A continuación, agregue las siguientes páginas de ASP.NET a esa carpeta, asegurándose de asociar cada página a la página maestra de `Site.master`:

- `Default.aspx`
- `RadioButtonField.aspx`
- `CheckBoxField.aspx`
- `InsertThroughFooter.aspx`

![Agregue las páginas ASP.NET para los tutoriales relacionados con SqlDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.gif)

**Figura 1**: agregar las páginas ASP.net para los tutoriales relacionados con SqlDataSource

Al igual que en las otras carpetas, `Default.aspx` en la carpeta `EnhancedGridView` mostrará los tutoriales en su sección. Recuerde que el control de usuario `SectionLevelTutorialListing.ascx` proporciona esta funcionalidad. Por lo tanto, agregue este control de usuario a `Default.aspx` arrastrándolo desde el Explorador de soluciones hasta la página s Vista de diseño.

[![agregar el control de usuario SectionLevelTutorialListing. ascx a default. aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image1.png)

**Figura 2**: agregar el control de usuario `SectionLevelTutorialListing.ascx` a `Default.aspx` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image2.png))

Por último, agregue estas cuatro páginas como entradas al archivo `Web.sitemap`. En concreto, agregue el siguiente marcado después de usar el control SqlDataSource `<siteMapNode>`:

[!code-xml[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample1.xml)]

Después de actualizar `Web.sitemap`, dedique un momento a ver el sitio web de tutoriales a través de un explorador. El menú de la izquierda incluye ahora los elementos para los tutoriales de edición, inserción y eliminación.

![El mapa del sitio ahora incluye entradas para mejorar los tutoriales de GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.gif)

**Figura 3**: ahora, el mapa del sitio incluye entradas para mejorar los tutoriales de GridView.

## <a name="step-2-displaying-the-suppliers-in-a-gridview"></a>Paso 2: mostrar los proveedores en un control GridView

En este tutorial, vamos a crear un GridView que enumera los proveedores de EE. UU., con cada fila de GridView que proporciona un botón de radio. Después de seleccionar un proveedor mediante el botón de radio, el usuario puede ver los productos de proveedores haciendo clic en un botón. Aunque esta tarea puede parecer trivial, hay una serie de matices que lo hacen especialmente complicado. Antes de profundizar en estas sutilezas, vamos a obtener primero un control GridView que muestre los proveedores.

Para empezar, abra la página de `RadioButtonField.aspx` de la carpeta `EnhancedGridView` arrastrando un control GridView desde el cuadro de herramientas hasta el diseñador. Establezca el `ID` de GridView en `Suppliers` y, desde su etiqueta inteligente, elija crear un nuevo origen de datos. En concreto, cree un ObjectDataSource denominado `SuppliersDataSource` que extraiga sus datos del objeto `SuppliersBLL`.

[![crear un nuevo ObjectDataSource denominado SuppliersDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image3.png)

**Figura 4**: creación de un nuevo ObjectDataSource denominado `SuppliersDataSource` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image4.png))

[![configurar ObjectDataSource para usar la clase SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image5.png)

**Figura 5**: configuración de ObjectDataSource para usar la clase `SuppliersBLL` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.png))

Puesto que solo queremos enumerar esos proveedores en Estados Unidos, elija el método de `GetSuppliersByCountry(country)` en la lista desplegable de la pestaña seleccionar.

[![configurar ObjectDataSource para usar la clase SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image6.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.png)

**Figura 6**: configuración de ObjectDataSource para usar la clase `SuppliersBLL` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.png))

En la pestaña actualizar, seleccione la opción (ninguno) y haga clic en siguiente.

[![configurar ObjectDataSource para usar la clase SuppliersBLL](adding-a-gridview-column-of-radio-buttons-cs/_static/image7.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.png)

**Figura 7**: configuración de ObjectDataSource para usar la clase `SuppliersBLL` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.png))

Dado que el método `GetSuppliersByCountry(country)` acepta un parámetro, el Asistente para configurar el origen de datos nos solicita el origen de ese parámetro. Para especificar un valor codificado de forma rígida (EE. UU., en este ejemplo), deje la lista desplegable origen de parámetro establecida en ninguno y escriba el valor predeterminado en el cuadro de texto. Haga clic en Finalizar para completar el asistente.

[![usar USA como valor predeterminado para el parámetro Country](adding-a-gridview-column-of-radio-buttons-cs/_static/image8.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.png)

**Figura 8**: Use usa como valor predeterminado para el parámetro `country` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.png))

Después de completar el asistente, GridView incluirá una BoundField para cada uno de los campos de datos de proveedor. Quite todos los `CompanyName`, `City`y `Country` BoundFields, y cambie el nombre de la propiedad `CompanyName` BoundFields `HeaderText` a supplier. Después de hacerlo, la sintaxis declarativa de GridView y ObjectDataSource debe ser similar a la siguiente.

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample2.aspx)]

En este tutorial, permitiría al usuario ver los productos de proveedores seleccionados en la misma página que la lista de proveedores o en otra página. Para ello, agregue dos controles Web de botón a la página. Configure los `ID` s de estos dos botones en `ListProducts` y `SendToProducts`, con la idea de que, cuando se hace clic en `ListProducts`, se producirá un postback y los productos de los proveedores seleccionados se mostrarán en la misma página, pero cuando se haga clic en `SendToProducts`, el usuario se recogerá en otra página que muestre los productos.

En la ilustración 9 se muestra la `Suppliers` GridView y los dos controles Web de botón cuando se ven a través de un explorador.

[![esos proveedores de EE. UU. tienen su nombre, ciudad e información de país en la lista](adding-a-gridview-column-of-radio-buttons-cs/_static/image9.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.png)

**Figura 9**: estos proveedores de EE. UU. tienen su nombre, ciudad e información de país ([haga clic para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.png))

## <a name="step-3-adding-a-column-of-radio-buttons"></a>Paso 3: agregar una columna de botones de radio

En este momento, el `Suppliers` GridView tiene tres BoundFields que muestran el nombre de la empresa, la ciudad y el país de cada proveedor de EE. UU. Sin embargo, todavía falta una columna de botones de radio. Desafortunadamente, el control GridView no incluye un RadioButtonField integrado; de lo contrario, podríamos agregarlo a la cuadrícula y hacerlo. En su lugar, podemos agregar un TemplateField y configurar su `ItemTemplate` para representar un botón de radio, lo que da como resultado un botón de radio para cada fila de GridView.

En principio, podríamos suponer que la interfaz de usuario deseada puede implementarse agregando un control Web RadioButton al `ItemTemplate` de un TemplateField. Aunque esto agregará un solo botón de radio a cada fila de GridView, los botones de radio no se pueden agrupar y, por lo tanto, no son mutuamente excluyentes. Es decir, un usuario final puede seleccionar varios botones de radio simultáneamente desde GridView.

Aunque el uso de un control TemplateField de controles Web RadioButton no ofrece la funcionalidad que necesitamos, vamos a implementar este enfoque, ya que merece la pena examinar por qué los botones de radio resultantes no están agrupados. Empiece agregando un TemplateField al GridView proveedores, convirtiéndolo en el campo de la izquierda. Después, en la etiqueta inteligente de GridView s, haga clic en el vínculo editar plantillas y arrastre un control Web RadioButton desde el cuadro de herramientas hasta el `ItemTemplate` TemplateField s (consulte la figura 10). Establezca la propiedad RadioButton s `ID` en `RowSelector` y la propiedad `GroupName` en `SuppliersGroup`.

[![agregar un control Web RadioButton al ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image10.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.png)

**Figura 10**: agregar un control Web RadioButton al `ItemTemplate` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.png))

Después de hacer estas adiciones a través del diseñador, el marcado de GridView s debe ser similar al siguiente:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample3.aspx)]

La propiedad RadioButton s [`GroupName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.radiobutton.groupname(VS.80).aspx) es lo que se utiliza para agrupar una serie de botones de radio. Todos los controles RadioButton con el mismo valor `GroupName` se consideran agrupados; solo se puede seleccionar un botón de radio de un grupo a la vez. La propiedad `GroupName` especifica el valor para el atributo de `name` de botón de radio representado. El explorador examina los botones de radio `name` los atributos para determinar las agrupaciones de botones de radio.

Con el control Web RadioButton agregado al `ItemTemplate`, visite esta página a través de un explorador y haga clic en los botones de radio de las filas de la cuadrícula. Observe que los botones de radio no están agrupados, lo que permite seleccionar todas las filas, tal y como se muestra en la figura 11.

[![los botones de radio de GridView no están agrupados](adding-a-gridview-column-of-radio-buttons-cs/_static/image11.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.png)

**Figura 11**: los botones de radio de GridView no están agrupados ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.png))

El motivo por el que los botones de radio no están agrupados se debe a que sus atributos de `name` representados son diferentes, a pesar de tener el mismo valor de propiedad `GroupName`. Para ver estas diferencias, haga una vista o código fuente desde el explorador y examine el marcado del botón de radio:

[!code-html[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample4.html)]

Observe cómo los atributos `name` y `id` no son los valores exactos tal y como se especifican en el ventana Propiedades, pero se anteponen a otros valores de `ID`. Los valores de `ID` adicionales agregados al principio de los atributos `id` y `name` que se representan son los `ID` s de los botones de radio que controla los `GridViewRow` `ID` s, GridView s `ID`, el control de contenido `ID`y el formulario Web Forms `ID`. Estos `ID` s se agregan de modo que cada control Web representado en GridView tenga un `id` y valores `name` únicos.

Cada control representado necesita un `name` y `id` diferentes, ya que es el modo en que el explorador identifica de forma única cada control en el lado cliente y cómo identifica al servidor Web qué acción o cambio se ha producido en el PostBack. Por ejemplo, Imagine que desea ejecutar código de servidor cada vez que se cambia un estado RadioButton s Checked. Podríamos lograr esto estableciendo la propiedad RadioButton s `AutoPostBack` en `true` y creando un controlador de eventos para el evento `CheckChanged`. Sin embargo, si los valores de `name` y `id` representados para todos los botones de radio eran los mismos, en el PostBack no se pudo determinar en qué RadioButton se hizo clic.

Lo breve es que no se puede crear una columna de botones de radio en un control GridView mediante el control Web RadioButton. En su lugar, se deben usar técnicas de arcaico para asegurarse de que el marcado adecuado se inserta en cada fila de GridView.

> [!NOTE]
> Al igual que el control Web RadioButton, el control HTML del botón de radio, cuando se agrega a una plantilla, incluirá el atributo `name` único, de modo que los botones de radio de la cuadrícula no estén agrupados. Si no está familiarizado con los controles HTML, no dude en omitir esta nota, ya que los controles HTML rara vez se usan, especialmente en ASP.NET 2,0. Pero si está interesado en obtener más información, consulte la entrada del blog de [Scott Allen](http://odetocode.com/blogs/scott/default.aspx) s [y controles html](http://www.odetocode.com/Articles/348.aspx).

## <a name="using-a-literal-control-to-inject-radio-button-markup"></a>Usar un control literal para insertar el marcado de botón de radio

Para agrupar correctamente todos los botones de radio dentro de GridView, es necesario insertar manualmente el marcado de los botones de radio en el `ItemTemplate`. Cada botón de radio necesita el mismo `name` atributo, pero debe tener un atributo de `id` único (en caso de que desee tener acceso a un botón de radio a través de un script del lado cliente). Después de que un usuario selecciona un botón de radio y vuelve a enviar la página, el explorador devolverá el valor del botón de radio seleccionado s `value` atributo. Por lo tanto, cada botón de radio necesitará un atributo de `value` único. Por último, en el postback, es necesario asegurarse de agregar el atributo `checked` al botón de radio seleccionado; de lo contrario, una vez que el usuario realiza una selección y devuelve datos, los botones de radio volverán a su estado predeterminado (todos no seleccionados).

Existen dos enfoques que se pueden realizar para insertar marcado de bajo nivel en una plantilla. Uno consiste en combinar el marcado y las llamadas a los métodos de formato definidos en la clase de código subyacente. Esta técnica se analizó en primer lugar en el tutorial [uso de TemplateFields en el control GridView](../custom-formatting/using-templatefields-in-the-gridview-control-cs.md) . En nuestro caso, podría ser similar a lo siguiente:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample5.aspx)]

En este caso, `GetUniqueRadioButton` y `GetRadioButtonValue` serían métodos definidos en la clase de código subyacente que devolvieron los valores de atributo `id` y `value` adecuados para cada botón de radio. Este enfoque funciona bien para asignar los atributos `id` y `value`, pero es breve cuando se necesita especificar el valor del atributo `checked` porque la sintaxis de DataBinding solo se ejecuta cuando se enlazan los datos por primera vez a GridView. Por consiguiente, si GridView tiene el estado de vista habilitado, los métodos de formato solo se activarán cuando se cargue la página por primera vez (o cuando GridView se reenlace explícitamente al origen de datos) y, por lo tanto, no se llamará a la función que establece el atributo `checked` en el PostBack. Es un problema bastante sutil y un poco más allá del ámbito de este artículo, por lo que lo dejaré en este caso. Sin embargo, le recomendamos que pruebe a usar el enfoque anterior y que trabaje con el punto en el que se bloqueará. Aunque este ejercicio no le llevará más cerca de una versión de trabajo, le ayudará a fomentar una comprensión más profunda de GridView y el ciclo de vida de los enlaces de la información.

El otro enfoque para insertar el marcado de bajo nivel personalizado en una plantilla y el enfoque que vamos a usar para este tutorial es agregar un [control literal](https://msdn.microsoft.com/library/sz4949ks(VS.80).aspx) a la plantilla. A continuación, en el `RowCreated` de GridView o en el controlador de eventos `RowDataBound`, se puede tener acceso mediante programación al control literal y su propiedad `Text` establecida en el marcado que se va a emitir.

Comience quitando el RadioButton del `ItemTemplate`TemplateField s, reemplazándolo por un control literal. Establezca el `ID` de control literal en `RadioButtonMarkup`.

[![agregar un control literal al ItemTemplate](adding-a-gridview-column-of-radio-buttons-cs/_static/image12.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.png)

**Figura 12**: agregar un control Literal al `ItemTemplate` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.png))

A continuación, cree un controlador de eventos para el evento GridView s `RowCreated`. El evento `RowCreated` se activa una vez por cada fila agregada, tanto si los datos se están reenlazando a GridView como si no. Esto significa que, incluso en un postback cuando los datos se recargan desde el estado de vista, el evento de `RowCreated` se sigue desencadenando y este es el motivo por el que lo usamos en lugar de `RowDataBound` (que solo se activa cuando los datos están enlazados explícitamente al control Web de datos).

En este controlador de eventos, solo queremos continuar si se refiere a una fila de datos. Para cada fila de datos, se desea hacer referencia mediante programación al control literal `RadioButtonMarkup` y establecer su propiedad `Text` en el marcado que se va a emitir. Como se muestra en el código siguiente, el marcado emitido crea un botón de radio cuyo atributo `name` está establecido en `SuppliersGroup`, cuyo atributo `id` está establecido en `RowSelectorX`, donde *X* es el índice de la fila de GridView y cuyo atributo `value` está establecido en el índice de la fila de GridView.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample6.cs)]

Cuando se selecciona una fila de GridView y se produce un postback, estamos interesados en el `SupplierID` del proveedor seleccionado. Por lo tanto, se podría pensar que el valor de cada botón de radio debe ser el `SupplierID` real (en lugar del índice de la fila de GridView). Aunque esto puede funcionar en determinadas circunstancias, sería un riesgo de seguridad para aceptar y procesar una `SupplierID`. Por ejemplo, nuestro GridView muestra solo los proveedores de EE. UU. Sin embargo, si el `SupplierID` se pasa directamente desde el botón de radio, ¿qué se debe hacer para que un usuario de este manipule el `SupplierID` valor que se devuelve en el PostBack? Utilizando el índice de fila como `value`y, a continuación, obteniendo el `SupplierID` en el PostBack de la colección de `DataKeys`, podemos asegurarnos de que el usuario solo usa uno de los valores de `SupplierID` asociados a una de las filas de GridView.

Después de agregar este código de controlador de eventos, dedique un minuto a probar la página en un explorador. En primer lugar, tenga en cuenta que solo se puede seleccionar un botón de radio de la cuadrícula a la vez. Sin embargo, al seleccionar un botón de radio y hacer clic en uno de los botones, se produce un postback y los botones de radio se revierten a su estado inicial (es decir, en el postback, el botón de radio seleccionado ya no está seleccionado). Para corregir esto, es necesario aumentar el controlador de eventos `RowCreated` para que inspeccione el índice de botón de radio seleccionado enviado desde el PostBack y agregue el atributo `checked="checked"` al marcado emitido de las coincidencias de índice de fila.

Cuando se produce un postback, el explorador devuelve el `name` y `value` del botón de radio seleccionado. El valor se puede recuperar mediante programación mediante `Request.Form["name"]`. La [propiedad`Request.Form`](https://msdn.microsoft.com/library/system.web.httprequest.form.aspx) proporciona un [`NameValueCollection`](https://msdn.microsoft.com/library/system.collections.specialized.namevaluecollection.aspx) que representa las variables de formulario. Las variables de formulario son los nombres y los valores de los campos de formulario de la página web, y el explorador web los devuelve cada vez que se produce un postback. Dado que el atributo rendered `name` de los botones de radio de GridView es `SuppliersGroup`, cuando se devuelve la página web, el explorador enviará `SuppliersGroup=valueOfSelectedRadioButton` de vuelta al servidor Web (junto con los demás campos de formulario). A continuación, se puede tener acceso a esta información desde la propiedad `Request.Form` mediante: `Request.Form["SuppliersGroup"]`.

Dado que es necesario determinar el índice del botón de radio seleccionado no solo en el controlador de eventos `RowCreated`, pero en los controladores de eventos de `Click` para los controles Web de botón, permite agregar una propiedad `SuppliersSelectedIndex` a la clase de código subyacente que devuelve `-1` si no se ha seleccionado ningún botón de radio y el índice seleccionado si se ha seleccionado uno de los botones de radio.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample7.cs)]

Con esta propiedad agregada, sabemos agregar el marcado `checked="checked"` en el controlador de eventos `RowCreated` cuando `SuppliersSelectedIndex` es igual a `e.Row.RowIndex`. Actualice el controlador de eventos para incluir esta lógica:

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample8.cs)]

Con este cambio, el botón de radio seleccionado permanece seleccionado después de un postback. Ahora que tenemos la capacidad de especificar qué botón de radio está seleccionado, podríamos cambiar el comportamiento de modo que, cuando se Visitó la página por primera vez, se seleccione el primer botón de radio de la fila de GridView (en lugar de no tener botones de radio seleccionados de forma predeterminada, que es el actual. comportamiento). Para que el primer botón de radio esté seleccionado de forma predeterminada, simplemente cambie la instrucción `if (SuppliersSelectedIndex == e.Row.RowIndex)` a lo siguiente: `if (SuppliersSelectedIndex == e.Row.RowIndex || (!Page.IsPostBack && e.Row.RowIndex == 0))`.

En este punto se ha agregado una columna de botones de radio agrupados a GridView que permite seleccionar una sola fila de GridView y recordarla en los postbacks. Los pasos siguientes son para mostrar los productos proporcionados por el proveedor seleccionado. En el paso 4, veremos cómo redirigir al usuario a otra página, enviando a lo largo de la `SupplierID`seleccionada. En el paso 5, veremos cómo mostrar los productos de proveedores seleccionados en un control GridView en la misma página.

> [!NOTE]
> En lugar de usar TemplateField (el enfoque de este largo paso 3), podríamos crear una clase de `DataControlField` personalizada que represente la interfaz de usuario y la funcionalidad adecuadas. La [clase`DataControlField`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.datacontrolfield.aspx) es la clase base de la que derivan BoundField, CheckBoxField, TemplateField y otros campos de GridView y DetailsView integrados. La creación de una clase de `DataControlField` personalizada significa que la columna de botones de radio se podría agregar simplemente mediante la sintaxis declarativa, y también simplificaría considerablemente la replicación de la funcionalidad en otras páginas web y otras aplicaciones Web.

Sin embargo, si ya ha creado controles personalizados y compilados en ASP.NET, sabe que esto requiere una cantidad justa de tareas y que lleva con él un host de matices y casos de borde que deben controlarse cuidadosamente. Por lo tanto, se renunciará a la implementación de una columna de botones de radio como una clase de `DataControlField` personalizada por ahora y se adhiere a la opción TemplateField. Quizás tengamos la oportunidad de explorar la creación, el uso y la implementación de clases de `DataControlField` personalizadas en un tutorial futuro.

## <a name="step-4-displaying-the-selected-supplier-s-products-in-a-separate-page"></a>Paso 4: mostrar los productos de proveedores seleccionados en una página independiente

Una vez que el usuario ha seleccionado una fila de GridView, es necesario mostrar los productos de proveedor seleccionados. En algunas circunstancias, es posible que deseemos mostrar estos productos en una página independiente, en otros, es posible que prefiera hacerlo en la misma página. Vamos a examinar primero cómo mostrar los productos en una página independiente. en el paso 5 veremos cómo agregar un control GridView a `RadioButtonField.aspx` para mostrar los productos de proveedores seleccionados.

Actualmente hay dos controles Web de botón en la página `ListProducts` y `SendToProducts`. Cuando se hace clic en el botón `SendToProducts`, queremos enviar al usuario a `~/Filtering/ProductsForSupplierDetails.aspx`. Esta página se ha creado en el tutorial de [filtrado maestro y detalles en dos páginas](../masterdetail/master-detail-filtering-across-two-pages-cs.md) y muestra los productos del proveedor cuyo `SupplierID` se pasa a través del campo querystring denominado `SupplierID`.

Para proporcionar esta funcionalidad, cree un controlador de eventos para el evento `SendToProducts` botón s `Click`. En el paso 3 se ha agregado la propiedad `SuppliersSelectedIndex`, que devuelve el índice de la fila cuyo botón de radio está seleccionado. El `SupplierID` correspondiente se puede recuperar de la colección de `DataKeys` de GridView y, a continuación, el usuario se puede enviar a `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` mediante `Response.Redirect("url")`.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample9.cs)]

Este código funciona de forma maravillosa siempre que se seleccione uno de los botones de radio de GridView. Si, inicialmente, GridView no tiene ningún botón de radio seleccionado y el usuario hace clic en el botón `SendToProducts`, `SuppliersSelectedIndex` se `-1`, lo que hará que se produzca una excepción, ya que `-1` está fuera del intervalo de índices de la colección `DataKeys`. Sin embargo, esto no es un problema, si decide actualizar el controlador de eventos `RowCreated` como se describe en el paso 3 para que el primer botón de radio de GridView esté seleccionado inicialmente.

Para dar cabida a un valor `SuppliersSelectedIndex` de `-1`, agregue un control Web de etiqueta a la página sobre GridView. Establezca su propiedad `ID` en `ChooseSupplierMsg`, su propiedad `CssClass` en `Warning`, sus propiedades `EnableViewState` y `Visible` en `false`y su propiedad `Text` para elegir un proveedor de la cuadrícula. La clase CSS `Warning` muestra texto en una fuente de color rojo, cursiva, negrita y grande, y se define en `Styles.css`. Al establecer las propiedades `EnableViewState` y `Visible` en `false`, la etiqueta no se representa excepto solo para los postbacks en los que la propiedad control s `Visible` se establece mediante programación en `true`.

[![agregar un control Web de etiqueta sobre GridView](adding-a-gridview-column-of-radio-buttons-cs/_static/image13.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image21.png)

**Figura 13**: agregar un control Web de etiqueta sobre GridView ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image22.png))

A continuación, aumente el controlador de eventos `Click` para mostrar la etiqueta de `ChooseSupplierMsg` si `SuppliersSelectedIndex` es menor que cero y redirija al usuario a `~/Filtering/ProductsForSupplierDetails.aspx?SupplierID=SupplierID` en caso contrario.

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample10.cs)]

Visite la página en un explorador y haga clic en el botón `SendToProducts` antes de seleccionar un proveedor de GridView. Como se muestra en la figura 14, se muestra la etiqueta `ChooseSupplierMsg`. A continuación, seleccione un proveedor y haga clic en el botón `SendToProducts`. Esto le indicará a una página que muestra los productos suministrados por el proveedor seleccionado. En la figura 15 se muestra la página `ProductsForSupplierDetails.aspx` cuando se seleccionó el proveedor de Breweries de Bigfoot.

[![se muestra la etiqueta ChooseSupplierMsg si no se selecciona ningún proveedor](adding-a-gridview-column-of-radio-buttons-cs/_static/image14.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image23.png)

**Figura 14**: el `ChooseSupplierMsg` etiqueta se muestra si no se selecciona ningún proveedor ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image24.png))

[![los productos de proveedores seleccionados se muestran en ProductsForSupplierDetails. aspx](adding-a-gridview-column-of-radio-buttons-cs/_static/image15.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image25.png)

**Figura 15**: los productos de proveedores seleccionados se muestran en `ProductsForSupplierDetails.aspx` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image26.png))

## <a name="step-5-displaying-the-selected-supplier-s-products-on-the-same-page"></a>Paso 5: mostrar los productos de proveedores seleccionados en la misma página

En el paso 4, vimos cómo enviar al usuario a otra página web para mostrar los productos de proveedores seleccionados. Como alternativa, los productos de los proveedores seleccionados se pueden mostrar en la misma página. Para ilustrar esto, agregaremos otro GridView a `RadioButtonField.aspx` para mostrar los productos de proveedor seleccionados.

Puesto que solo queremos que este GridView de productos se muestre una vez que se ha seleccionado un proveedor, agregue un control Web panel debajo del `Suppliers` GridView, estableciendo su `ID` en `ProductsBySupplierPanel` y su propiedad `Visible` en `false`. En el panel, agregue los productos de texto para el proveedor seleccionado, seguido de un control GridView denominado `ProductsBySupplier`. En la etiqueta inteligente de GridView s, elija enlazarla a un nuevo ObjectDataSource denominado `ProductsBySupplierDataSource`.

[![enlazar el control GridView ProductsBySupplier a un nuevo ObjectDataSource](adding-a-gridview-column-of-radio-buttons-cs/_static/image16.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image27.png)

**Figura 16**: enlace de la `ProductsBySupplier` GridView a un nuevo ObjectDataSource ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image28.png))

A continuación, configure ObjectDataSource para usar la clase `ProductsBLL`. Puesto que solo queremos recuperar los productos proporcionados por el proveedor seleccionado, especifique que ObjectDataSource debe invocar el método `GetProductsBySupplierID(supplierID)` para recuperar sus datos. Seleccione (ninguno) en las listas desplegables de las pestañas actualizar, insertar y eliminar.

[![configurar ObjectDataSource para usar el método GetProductsBySupplierID (supplierID)](adding-a-gridview-column-of-radio-buttons-cs/_static/image17.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image29.png)

**Figura 17**: configuración de ObjectDataSource para usar el método `GetProductsBySupplierID(supplierID)` ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image30.png))

[![establecer las listas desplegables en (ninguna) en las pestañas actualizar, insertar y eliminar](adding-a-gridview-column-of-radio-buttons-cs/_static/image18.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image31.png)

**Figura 18**: establezca las listas desplegables en (ninguna) en las pestañas actualizar, insertar y eliminar ([haga clic para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image32.png))

Después de configurar las pestañas seleccionar, actualizar, insertar y eliminar, haga clic en siguiente. Dado que el método `GetProductsBySupplierID(supplierID)` espera un parámetro de entrada, el Asistente para crear orígenes de datos le pide que especifique el origen del valor del parámetro s.

Tenemos un par de opciones aquí para especificar el origen del valor del parámetro s. Podríamos usar el objeto de parámetro predeterminado y asignar mediante programación el valor de la propiedad `SuppliersSelectedIndex` a la propiedad Parameter s `DefaultValue` del controlador de eventos ObjectDataSource s `Selecting`. Vuelva a consultar la [configuración mediante programación del tutorial de valores de parámetro de ObjectDataSource](../basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs.md) para un actualizador en la asignación mediante programación de valores a los parámetros de ObjectDataSource s.

Como alternativa, podemos usar ControlParameter y hacer referencia a la propiedad `Suppliers` GridView s [`SelectedValue`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedvalue.aspx) (consulte la figura 19). La propiedad GridView s `SelectedValue` devuelve el valor de `DataKey` correspondiente a la [propiedad`SelectedIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.selectedindex.aspx). Para que esta opción funcione, es necesario establecer mediante programación la propiedad `SelectedIndex` de GridView en la fila seleccionada cuando se haga clic en el botón `ListProducts`. Como ventaja adicional, si se establece el `SelectedIndex`, el registro seleccionado tomará el `SelectedRowStyle` definido en el tema de `DataWebControls` (un fondo amarillo).

[![usar ControlParameter para especificar GridView s SelectedValue como el origen del parámetro](adding-a-gridview-column-of-radio-buttons-cs/_static/image19.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image33.png)

**Figura 19**: uso de ControlParameter para especificar el control GridView s como el origen del parámetro ([haga clic para ver la imagen de tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image34.png))

Al finalizar el asistente, Visual Studio agregará automáticamente campos para los campos de datos de los productos. Quite todos los `ProductName`, `CategoryName`y `UnitPrice` BoundFields, y cambie las propiedades de `HeaderText` a product, Category y price. Configure el `UnitPrice` BoundField para que su valor tenga el formato de moneda. Después de realizar estos cambios, el marcado declarativo del panel, GridView y ObjectDataSource debe ser similar al siguiente:

[!code-aspx[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample11.aspx)]

Para completar este ejercicio, es necesario establecer la propiedad GridView s `SelectedIndex` en el `SelectedSuppliersIndex` y la propiedad `Visible` del panel `ProductsBySupplierPanel` en `true` al hacer clic en el botón `ListProducts`. Para ello, cree un controlador de eventos para el evento de control Web Button `ListProducts` `Click` y agregue el código siguiente:

[!code-csharp[Main](adding-a-gridview-column-of-radio-buttons-cs/samples/sample12.cs)]

Si no se ha seleccionado un proveedor de GridView, se muestra la etiqueta de `ChooseSupplierMsg` y se oculta el panel de `ProductsBySupplierPanel`. De lo contrario, si se ha seleccionado un proveedor, se muestra el `ProductsBySupplierPanel` y se actualiza la propiedad `SelectedIndex` de GridView.

En la figura 20 se muestran los resultados una vez que se ha seleccionado el proveedor de Bigfoot Breweries y se ha realizado un clic en el botón Mostrar productos en la página.

[![los productos proporcionados por Bigfoot Breweries se muestran en la misma página](adding-a-gridview-column-of-radio-buttons-cs/_static/image20.gif)](adding-a-gridview-column-of-radio-buttons-cs/_static/image35.png)

**Figura 20**: los productos proporcionados por Bigfoot Breweries aparecen en la misma página ([haga clic para ver la imagen a tamaño completo](adding-a-gridview-column-of-radio-buttons-cs/_static/image36.png))

## <a name="summary"></a>Resumen

Tal y como se describe en el tutorial [maestro y detalles con un control GridView de la página maestra seleccionable con un DetailView de detalles](../masterdetail/master-detail-using-a-selectable-master-gridview-with-a-details-detailview-cs.md) , los registros se pueden seleccionar en un control GridView mediante un CommandField cuya propiedad `ShowSelectButton` esté establecida en `true`. Sin embargo, el CommandField de instalación de muestra los botones como botones de envío, vínculos o imágenes normales. Una interfaz de usuario de selección de fila alternativa consiste en proporcionar un botón de radio o una casilla en cada fila de GridView. En este tutorial, hemos examinado cómo agregar una columna de botones de radio.

Desafortunadamente, agregar una columna de botones de radio no es tan sencillo o sencillo como cabría esperar. No hay ningún RadioButtonField integrado que se pueda agregar al hacer clic en un botón y el uso del control Web RadioButton dentro de TemplateField incluye su propio conjunto de problemas. Al final, para proporcionar una interfaz de este tipo, es necesario crear una clase de `DataControlField` personalizada o volver a insertar el HTML adecuado en TemplateField durante el evento `RowCreated`.

Una vez que se ha explorado cómo agregar una columna de botones de radio, preste atención a la adición de una columna de casillas. Con una columna de casillas, un usuario puede seleccionar una o más filas de GridView y, a continuación, realizar alguna operación en todas las filas seleccionadas (por ejemplo, seleccionar un conjunto de correos electrónicos de un cliente de correo electrónico basado en Web y, a continuación, seleccionar eliminar todos los correos electrónicos seleccionados). En el siguiente tutorial, veremos cómo agregar este tipo de columna.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue David Suru. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Siguiente](adding-a-gridview-column-of-checkboxes-cs.md)
