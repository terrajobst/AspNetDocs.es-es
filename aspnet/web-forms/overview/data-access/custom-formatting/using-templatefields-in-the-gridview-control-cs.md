---
uid: web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
title: Usar TemplateFields en el control GridView (C#) | Microsoft Docs
author: rick-anderson
description: Para proporcionar flexibilidad, GridView ofrece TemplateField, que se representa mediante una plantilla. Una plantilla puede incluir una combinación de HTML estático, controles Web y...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 11de31e8-a78a-4f96-bd75-66e994175902
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-templatefields-in-the-gridview-control-cs
msc.type: authoredcontent
ms.openlocfilehash: ec17a16d7bb487d1c5cacf2d5971bbeffc1ba031
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78481729"
---
# <a name="using-templatefields-in-the-gridview-control-c"></a>Usar TemplateFields en el control GridView (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/9/6/9/969e5c94-dfb6-4e47-9570-d6d9e704c3c1/ASPNET_Data_Tutorial_12_CS.exe) de la aplicación de ejemplo o [descarga de PDF](using-templatefields-in-the-gridview-control-cs/_static/datatutorial12cs1.pdf)

> Para proporcionar flexibilidad, GridView ofrece TemplateField, que se representa mediante una plantilla. Una plantilla puede incluir una combinación de HTML estático, controles Web y sintaxis de enlace de los enlaces. En este tutorial, examinaremos cómo usar TemplateField para lograr un mayor grado de personalización con el control GridView.

## <a name="introduction"></a>Introducción

GridView se compone de un conjunto de campos que indican qué propiedades de la `DataSource` se van a incluir en la salida representada junto con cómo se mostrarán los datos. El tipo de campo más sencillo es BoundField, que muestra un valor de datos como texto. Otros tipos de campo muestran los datos mediante elementos HTML alternativos. El CheckBoxField, por ejemplo, se representa como una casilla cuyo estado activado depende del valor de un campo de datos especificado. ImageField representa una imagen cuyo origen de la imagen se basa en un campo de datos especificado. Los hipervínculos y los botones cuyo estado depende de un valor de campo de datos subyacente se pueden representar mediante los tipos de campo HyperLinkField y ButtonField.

Aunque los tipos de campos CheckBoxField, ImageField, HyperLinkField y ButtonField permiten una vista alternativa de los datos, siguen siendo bastante limitados con respecto al formato. Un CheckBoxField solo puede mostrar una única casilla, mientras que un ImageField solo puede mostrar una sola imagen. ¿Qué ocurre si un campo determinado necesita mostrar texto, una casilla *y* una imagen, todos basados en valores de campo de datos diferentes? O bien, ¿qué ocurre si desea mostrar los datos con un control Web distinto de la casilla, la imagen, el hipervínculo o el botón? Además, la BoundField limita su presentación a un único campo de datos. ¿Qué ocurre si deseáramos mostrar dos o más valores de campo de datos en una sola columna de GridView?

Para dar cabida a este nivel de flexibilidad, GridView ofrece TemplateField, que se representa mediante una *plantilla*. Una plantilla puede incluir una combinación de HTML estático, controles Web y sintaxis de enlace de los enlaces. Además, TemplateField tiene una variedad de plantillas que se pueden usar para personalizar la representación de distintas situaciones. Por ejemplo, el `ItemTemplate` se utiliza de forma predeterminada para representar la celda para cada fila, pero la plantilla de `EditItemTemplate` se puede usar para personalizar la interfaz al editar datos.

En este tutorial, examinaremos cómo usar TemplateField para lograr un mayor grado de personalización con el control GridView. En el [tutorial anterior](custom-formatting-based-upon-data-cs.md) , vimos cómo personalizar el formato basado en los datos subyacentes mediante los controladores de eventos `DataBound` y `RowDataBound`. Otra manera de personalizar el formato en función de los datos subyacentes es llamando a métodos de formato desde una plantilla. También veremos esta técnica en este tutorial.

En este tutorial, usaremos TemplateFields para personalizar la apariencia de una lista de empleados. En concreto, se enumerarán todos los empleados, pero se mostrarán el nombre y los apellidos del empleado en una columna, su fecha de contratación en un control de calendario y una columna de estado que indica cuántos días se han empleado en la empresa.

[![se usan tres TemplateFields para personalizar la pantalla](using-templatefields-in-the-gridview-control-cs/_static/image2.png)](using-templatefields-in-the-gridview-control-cs/_static/image1.png)

**Figura 1**: se usan tres TemplateFields para personalizar la pantalla ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-gridview"></a>Paso 1: enlazar los datos a GridView

En los escenarios en los que es necesario usar TemplateFields para personalizar la apariencia, es más fácil empezar por crear un control GridView que contenga solo BoundFields primero y, a continuación, agregar un nuevo TemplateFields o convertir el BoundFields existente en TemplateFields según sea necesario. Por lo tanto, vamos a iniciar este tutorial agregando un control GridView a la página a través del diseñador y enlazándolo a un ObjectDataSource que devuelve la lista de empleados. En estos pasos se creará un control GridView con BoundFields para cada uno de los campos Employee.

Abra la página `GridViewTemplateField.aspx` y arrastre un control GridView desde el cuadro de herramientas hasta el diseñador. En la etiqueta inteligente de GridView, elija Agregar un nuevo control ObjectDataSource que invoque el método `GetEmployees()` de la clase `EmployeesBLL`.

[![agregar un nuevo control ObjectDataSource que invoca el método GetEmployees ()](using-templatefields-in-the-gridview-control-cs/_static/image5.png)](using-templatefields-in-the-gridview-control-cs/_static/image4.png)

**Figura 2**: agregar un nuevo control ObjectDataSource que invoca el método `GetEmployees()` ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image6.png))

Al enlazar GridView de esta manera, se agregará automáticamente un BoundField para cada una de las propiedades de empleado: `EmployeeID`, `LastName`, `FirstName`, `Title`, `HireDate`, `ReportsTo`y `Country`. En este informe no vamos a mostrar las propiedades `EmployeeID`, `ReportsTo`o `Country`. Para quitar estos BoundFields, puede:

- Usar el cuadro de diálogo campos haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView para abrir este cuadro de diálogo. Después, seleccione BoundFields en la lista de la parte inferior izquierda y haga clic en el botón X de color rojo para quitar la BoundField.
- Edite la sintaxis declarativa de GridView a mano desde la vista de código fuente, elimine el elemento `<asp:BoundField>` de las BoundField que desea quitar.

Una vez que haya quitado el `EmployeeID`, `ReportsTo`y `Country` BoundFields, el marcado de GridView debería tener el siguiente aspecto:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample1.aspx)]

Tómese un momento para ver el progreso en un explorador. En este momento debería ver una tabla con un registro para cada empleado y cuatro columnas: una para el apellido del empleado, una para su nombre, una para su título y otra para su fecha de contratación.

[![se muestran los campos LastName, FirstName, title y HireDate para cada empleado](using-templatefields-in-the-gridview-control-cs/_static/image8.png)](using-templatefields-in-the-gridview-control-cs/_static/image7.png)

**Figura 3**: los campos `LastName`, `FirstName`, `Title`y `HireDate` se muestran para cada empleado ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image9.png))

## <a name="step-2-displaying-the-first-and-last-names-in-a-single-column"></a>Paso 2: mostrar el nombre y los apellidos de una sola columna

Actualmente, el nombre y los apellidos de cada empleado se muestran en una columna independiente. Puede combinarlos en una sola columna en su lugar. Para ello, es necesario usar TemplateField. Podemos agregar una nueva TemplateField, agregarle el marcado y la sintaxis de enlace de los enlaces necesarios y, a continuación, eliminar el `FirstName` y `LastName` BoundFields, o bien convertir el `FirstName` BoundField en TemplateField, editar el TemplateField para incluir el valor de `LastName` y, a continuación, quitar el `LastName` BoundField.

Ambos enfoques tienen el mismo resultado, pero personalmente me gusta convertir BoundFields en TemplateFields cuando sea posible, ya que la conversión agrega automáticamente un `ItemTemplate` y `EditItemTemplate` con controles Web y sintaxis de enlace de los enlaces para imitar la apariencia y la funcionalidad de BoundField. La ventaja es que tendremos que hacer menos trabajo con TemplateField, ya que el proceso de conversión habrá realizado parte del trabajo.

Para convertir un BoundField existente en TemplateField, haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView y muestre el cuadro de diálogo campos. Seleccione BoundField para convertir en la lista de la esquina inferior izquierda y, a continuación, haga clic en el vínculo "convertir este campo en TemplateField" en la esquina inferior derecha.

[![convertir un BoundField en TemplateField desde el cuadro de diálogo campos](using-templatefields-in-the-gridview-control-cs/_static/image11.png)](using-templatefields-in-the-gridview-control-cs/_static/image10.png)

**Figura 4**: conversión de BoundField en TemplateField desde el cuadro de diálogo campos ([haga clic para ver la imagen a tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image12.png))

Continúe y convierta el `FirstName` BoundField en TemplateField. Después de este cambio, no hay ninguna diferencia perceptive en el diseñador. Esto se debe a que la conversión de BoundField en TemplateField crea un TemplateField que mantiene la apariencia y el funcionamiento de BoundField. A pesar de que no haya ninguna diferencia visual en este punto del diseñador, este proceso de conversión ha reemplazado la sintaxis declarativa de BoundField `<asp:BoundField DataField="FirstName" HeaderText="FirstName" SortExpression="FirstName" />`-con la siguiente sintaxis de TemplateField:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample2.aspx)]

Como puede ver, TemplateField consta de dos plantillas, una `ItemTemplate` que tiene una etiqueta cuya propiedad `Text` está establecida en el valor del campo de datos `FirstName` y un `EditItemTemplate` con un control TextBox cuya propiedad `Text` también se establece en el campo `FirstName` Data. La sintaxis de DataBinding-`<%# Bind("fieldName") %>`-indica que el campo de datos *`fieldName`* se enlaza a la propiedad de control Web especificada.

Para agregar el valor del campo de datos `LastName` a este TemplateField, es necesario agregar otro control Web de etiqueta en el `ItemTemplate` y enlazar su propiedad `Text` a `LastName`. Esto puede realizarse a mano o a través del diseñador. Para hacerlo manualmente, basta con agregar la sintaxis declarativa adecuada al `ItemTemplate`:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample3.aspx)]

Para agregarlo a través del diseñador, haga clic en el vínculo editar plantillas de la etiqueta inteligente de GridView. Se mostrará la interfaz de edición de plantillas de GridView. En la etiqueta inteligente de la interfaz, se muestra una lista de las plantillas de GridView. Puesto que solo tenemos un TemplateField en este momento, las únicas plantillas que aparecen en la lista desplegable son las plantillas para el `FirstName` TemplateField junto con las `EmptyDataTemplate` y `PagerTemplate`. La plantilla de `EmptyDataTemplate`, si se especifica, se usa para representar la salida de GridView si no hay ningún resultado en los datos enlazados a GridView; el `PagerTemplate`, si se especifica, se usa para representar la interfaz de paginación de un control GridView que admita la paginación.

[![las plantillas de GridView se pueden editar a través del diseñador](using-templatefields-in-the-gridview-control-cs/_static/image14.png)](using-templatefields-in-the-gridview-control-cs/_static/image13.png)

**Figura 5**: las plantillas de GridView se pueden editar a través del diseñador ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image15.png))

Para mostrar también el `LastName` en el `FirstName` TemplateField, arrastre el control etiqueta desde el cuadro de herramientas hasta el `FirstName` `ItemTemplate` de la interfaz de edición de plantillas de GridView.

[![agregar un control Web Label al ItemTemplate de FirstName](using-templatefields-in-the-gridview-control-cs/_static/image17.png)](using-templatefields-in-the-gridview-control-cs/_static/image16.png)

**Figura 6**: agregar un control Web de etiqueta al `FirstName` ItemTemplate de TemplateField ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image18.png))

En este punto, el control Web Label agregado a TemplateField tiene su propiedad `Text` establecida en "Label". Necesitamos cambiar esto para que esta propiedad esté enlazada al valor del campo de datos `LastName` en su lugar. Para ello, haga clic en la etiqueta inteligente del control etiqueta y elija la opción Editar DataBindings.

[![elegir la opción Editar DataBindings de la etiqueta inteligente de la etiqueta](using-templatefields-in-the-gridview-control-cs/_static/image20.png)](using-templatefields-in-the-gridview-control-cs/_static/image19.png)

**Figura 7**: elija la opción Editar DataBindings de la etiqueta inteligente de la etiqueta ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image21.png))

Se abrirá el cuadro de diálogo DataBindings. Aquí puede seleccionar la propiedad para participar en el enlace de datos de la lista de la izquierda y elegir el campo al que desea enlazar los datos en la lista desplegable de la derecha. Elija la propiedad `Text` de la izquierda y el campo `LastName` de la derecha y haga clic en Aceptar.

[![enlazar la propiedad de texto al campo de datos LastName](using-templatefields-in-the-gridview-control-cs/_static/image23.png)](using-templatefields-in-the-gridview-control-cs/_static/image22.png)

**Figura 8**: enlace de la propiedad `Text` al campo de datos `LastName` ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image24.png))

> [!NOTE]
> El cuadro de diálogo DataBindings le permite indicar si desea realizar un enlace de los dos sentidos. Si deja esta desactivada, se utilizará la sintaxis de DataBinding `<%# Eval("LastName")%>` en lugar de `<%# Bind("LastName")%>`. Cualquier enfoque es adecuado para este tutorial. El enlace de datos bidireccional se convierte en importante al insertar y editar datos. Sin embargo, para mostrar solo los datos, cualquier enfoque funcionará igualmente bien. Veremos el enlace de los dos sentidos en detalle en futuros tutoriales.

Tómese un momento para ver esta página a través de un explorador. Como puede ver, GridView todavía incluye cuatro columnas; sin embargo, la columna `FirstName` ahora *muestra los valores de los campos* de datos `FirstName` y `LastName`.

[![los valores FirstName y LastName se muestran en una sola columna](using-templatefields-in-the-gridview-control-cs/_static/image26.png)](using-templatefields-in-the-gridview-control-cs/_static/image25.png)

**Figura 9**: los valores `FirstName` y `LastName` se muestran en una sola columna ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image27.png))

Para completar este primer paso, quite el `LastName` BoundField y cambie el nombre de la propiedad `HeaderText` de la `FirstName` TemplateField a "Name". Después de estos cambios, el marcado declarativo de GridView debería tener un aspecto similar al siguiente:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample4.aspx)]

[![el nombre y los apellidos de cada empleado se muestran en una columna](using-templatefields-in-the-gridview-control-cs/_static/image29.png)](using-templatefields-in-the-gridview-control-cs/_static/image28.png)

**Figura 10**: el nombre y los apellidos de cada empleado se muestran en una columna ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image30.png))

## <a name="step-3-using-the-calendar-control-to-display-thehireddatefield"></a>Paso 3: usar el control de calendario para mostrar el campo de`HiredDate`

Mostrar un valor de campo de datos como texto en GridView es tan sencillo como usar BoundField. En algunos escenarios, sin embargo, los datos se expresan mejor con un control Web determinado en lugar de solo texto. Esta personalización de la presentación de los datos es posible con TemplateFields. Por ejemplo, en lugar de mostrar la fecha de contratación del empleado como texto, podríamos mostrar un calendario (con [el control de calendario](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar(VS.80).aspx)) con su fecha de contratación resaltada.

Para ello, empiece por convertir el `HiredDate` BoundField en TemplateField. Solo tiene que ir a la etiqueta inteligente de GridView y hacer clic en el vínculo editar columnas y abrir el cuadro de diálogo campos. Seleccione la `HiredDate` BoundField y haga clic en "convertir este campo en TemplateField".

[![convertir HiredDate BoundField en TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image32.png)](using-templatefields-in-the-gridview-control-cs/_static/image31.png)

**Figura 11**: conversión del `HiredDate` BoundField en TemplateField ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image33.png))

Como vimos en el paso 2, esto reemplazará a BoundField con TemplateField que contenga una `ItemTemplate` y `EditItemTemplate` con una etiqueta y un cuadro de texto cuyas propiedades `Text` estén enlazadas al valor `HiredDate` mediante la sintaxis de DataBinding `<%# Bind("HiredDate")%>`.

Para reemplazar el texto por un control de calendario, edite la plantilla quitando la etiqueta y agregando un control de calendario. En el diseñador, seleccione Editar plantillas de la etiqueta inteligente de GridView y elija el `HireDate` `ItemTemplate` de TemplateField en la lista desplegable. A continuación, elimine el control etiqueta y arrastre un control Calendar desde el cuadro de herramientas a la interfaz de edición de plantillas.

[![agregar un control de calendario al ItemTemplate de HireDate TemplateField](using-templatefields-in-the-gridview-control-cs/_static/image35.png)](using-templatefields-in-the-gridview-control-cs/_static/image34.png)

**Figura 12**: agregar un control de calendario al `ItemTemplate` de `HireDate` TemplateField ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image36.png))

En este punto, cada fila de GridView contendrá un control de calendario en su `HiredDate` TemplateField. Sin embargo, el valor real `HiredDate` del empleado no se establece en ningún lugar del control de calendario, lo que hace que cada control de calendario muestre de forma predeterminada el mes y la fecha actuales. Para solucionarlo, es necesario asignar los `HiredDate` de cada empleado a las propiedades [SelectedDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selecteddate(VS.80).aspx) y [VisibleDate](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.visibledate(VS.80).aspx) del control de calendario.

En la etiqueta inteligente del control de calendario, elija Editar DataBindings. A continuación, enlace las propiedades `SelectedDate` y `VisibleDate` al campo de datos `HiredDate`.

[![enlazar las propiedades SelectedDate y VisibleDate al campo de datos HiredDate](using-templatefields-in-the-gridview-control-cs/_static/image38.png)](using-templatefields-in-the-gridview-control-cs/_static/image37.png)

**Figura 13**: enlace de las propiedades `SelectedDate` y `VisibleDate` al campo de datos `HiredDate` ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image39.png))

> [!NOTE]
> No es necesario que la fecha seleccionada del control de calendario esté siempre visible. Por ejemplo, un calendario puede tener<sup>el 1 de</sup>agosto de 1999 como la fecha seleccionada, pero mostrar el mes y el año actuales. La fecha seleccionada y la fecha visible se especifican mediante las propiedades `SelectedDate` y `VisibleDate` del control de calendario. Como queremos seleccionar el `HiredDate` del empleado y asegurarse de que se muestra, es necesario enlazar ambas propiedades al campo de datos `HireDate`.

Al ver la página en un explorador, el calendario muestra ahora el mes de la fecha de contratación del empleado y selecciona esa fecha concreta.

[![se muestra el HiredDate del empleado en el control de calendario](using-templatefields-in-the-gridview-control-cs/_static/image41.png)](using-templatefields-in-the-gridview-control-cs/_static/image40.png)

**Figura 14**: el `HiredDate` del empleado se muestra en el control de calendario ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image42.png))

> [!NOTE]
> Contrariamente a todos los ejemplos que hemos visto hasta ahora, en este tutorial *no* hemos establecido `EnableViewState` propiedad en `false` para este GridView. La razón de esta decisión es que, al hacer clic en las fechas del control de calendario, se produce un postback, estableciendo la fecha seleccionada del calendario en la fecha en la que acaba de hacer clic. Sin embargo, si el estado de vista de GridView está deshabilitado, en cada postback se vuelven a enlazar los datos de GridView a su origen de datos subyacente, lo que hace que la fecha seleccionada del calendario se *vuelva* a establecer en el `HireDate`del empleado, sobrescribiendo la fecha elegida por el usuario.

En este tutorial se trata de una discusión de Moot, ya que el usuario no puede actualizar el `HireDate`del empleado. Probablemente sea mejor configurar el control de calendario para que sus fechas no se puedan seleccionar. En cualquier caso, en este tutorial se muestra que, en algunas circunstancias, el estado de vista debe estar habilitado para proporcionar determinada funcionalidad.

## <a name="step-4-showing-the-number-of-days-the-employee-has-worked-for-the-company"></a>Paso 4: mostrar el número de días que el empleado ha trabajado para la empresa

Hasta ahora hemos detectado dos aplicaciones de TemplateFields:

- Combinar dos o más valores de campo de datos en una columna y
- Expresar un valor de campo de datos mediante un control Web en lugar de texto

Un tercer uso de TemplateFields es Mostrar metadatos sobre los datos subyacentes de GridView. Además de mostrar las fechas de contratación de los empleados, por ejemplo, es posible que también desee tener una columna que muestre el número total de días que han estado en el trabajo.

Sin embargo, otro uso de TemplateFields surge en escenarios en los que los datos subyacentes deben mostrarse de manera diferente en el informe de la página web que en el formato que se almacena en la base de datos. Imagine que la tabla `Employees` tenía un campo `Gender` que almacenó el carácter `M` o `F` para indicar el sexo del empleado. Al mostrar esta información en una página web, es posible que quiera mostrar el sexo como "hombre" o "hembra", en lugar de solo "M" o "F".

Ambos escenarios se pueden controlar mediante la creación de un *método de formato* en la clase de código subyacente de la página ASP.net (o en una biblioteca de clases independiente, implementada como un método `static`) que se invoca desde la plantilla. Este método de formato se invoca desde la plantilla con la misma sintaxis de DataBinding que se ha mencionado anteriormente. El método de formato puede tomar cualquier número de parámetros, pero debe devolver una cadena. Esta cadena devuelta es el código HTML que se inserta en la plantilla.

Para ilustrar este concepto, vamos a ampliar nuestro tutorial para mostrar una columna que muestra el número total de días que un empleado ha estado en el trabajo. Este método de formato tomará un objeto de `Northwind.EmployeesRow` y devolverá el número de días que el empleado se ha empleado como una cadena. Este método se puede Agregar a la clase de código subyacente de la página ASP.NET, pero se *debe* marcar como `protected` o `public` para que sea accesible desde la plantilla.

[!code-csharp[Main](using-templatefields-in-the-gridview-control-cs/samples/sample5.cs)]

Dado que el campo de `HiredDate` puede contener `NULL` valores de base de datos, primero debemos asegurarnos de que el valor no se `NULL` antes de continuar con el cálculo. Si el valor `HiredDate` es `NULL`, simplemente se devuelve la cadena "Unknown"; Si no se `NULL`, calculamos la diferencia entre la hora actual y el valor de `HiredDate` y devuelven el número de días.

Para usar este método, es necesario invocarlo desde TemplateField en GridView mediante la sintaxis de DataBinding. Para empezar, agregue una nueva TemplateField a GridView; para ello, haga clic en el vínculo editar columnas de la etiqueta inteligente de GridView y agregue una nueva TemplateField.

[![agregar un nuevo TemplateField a GridView](using-templatefields-in-the-gridview-control-cs/_static/image44.png)](using-templatefields-in-the-gridview-control-cs/_static/image43.png)

**Figura 15**: agregar una nueva TemplateField a GridView ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image45.png))

Establezca la propiedad `HeaderText` de la nueva TemplateField en "días del trabajo" y la propiedad `HorizontalAlign` de su `ItemStyle`en `Center`. Para llamar al método `DisplayDaysOnJob` desde la plantilla, agregue un `ItemTemplate` y use la siguiente sintaxis de enlace de enlaces:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample6.aspx)]

`Container.DataItem` devuelve un objeto `DataRowView` correspondiente al registro `DataSource` enlazado al `GridViewRow`. Su propiedad `Row` devuelve el `Northwind.EmployeesRow`fuertemente tipado, que se pasa al método `DisplayDaysOnJob`. Esta sintaxis de enlace de los enlaces puede aparecer directamente en el `ItemTemplate` (como se muestra en la sintaxis declarativa siguiente) o se puede asignar a la propiedad `Text` de un control Web de etiqueta.

> [!NOTE]
> Como alternativa, en lugar de pasar una instancia de `EmployeesRow`, podríamos pasar el valor de `HireDate` mediante `<%# DisplayDaysOnJob(Eval("HireDate")) %>`. Sin embargo, el método `Eval` devuelve un `object`, por lo que tenemos que cambiar la firma del método `DisplayDaysOnJob` para aceptar un parámetro de entrada de tipo `object`, en su lugar. No se puede convertir ciegamente la llamada de `Eval("HireDate")` a una `DateTime` porque la columna de `HireDate` de la tabla de `Employees` puede contener valores `NULL`. Por lo tanto, es necesario aceptar un `object` como parámetro de entrada para el método `DisplayDaysOnJob`, comprobar si tiene un valor de `NULL` de base de datos (que se puede realizar mediante `Convert.IsDBNull(objectToCheck)`) y, a continuación, continuar.

Debido a estos matices, he optado por pasar toda la instancia de `EmployeesRow`. En el tutorial siguiente, veremos un ejemplo de conexión más para usar la sintaxis de `Eval("columnName")` para pasar un parámetro de entrada a un método de formato.

A continuación se muestra la sintaxis declarativa de GridView una vez que se ha agregado TemplateField y el método `DisplayDaysOnJob` llamado desde el `ItemTemplate`:

[!code-aspx[Main](using-templatefields-in-the-gridview-control-cs/samples/sample7.aspx)]

En la figura 16 se muestra el tutorial completado, cuando se ve a través de un explorador.

[![el número de días que se muestra el empleado en el trabajo](using-templatefields-in-the-gridview-control-cs/_static/image47.png)](using-templatefields-in-the-gridview-control-cs/_static/image46.png)

**Figura 16**: se muestra el número de días que ha estado el empleado en el trabajo ([haga clic para ver la imagen de tamaño completo](using-templatefields-in-the-gridview-control-cs/_static/image48.png))

## <a name="summary"></a>Resumen

TemplateField en el control GridView permite un mayor grado de flexibilidad a la hora de mostrar los datos que están disponibles con los demás controles de campo. TemplateFields son ideales en situaciones en las que:

- Los campos de datos múltiples deben mostrarse en una columna de GridView
- Los datos se expresan mejor con un control Web en lugar de texto sin formato
- El resultado depende de los datos subyacentes, como Mostrar metadatos o volver a formatear los datos.

Además de personalizar la presentación de los datos, TemplateFields también se usan para personalizar las interfaces de usuario que se usan para editar e insertar datos, como veremos en futuros tutoriales.

Los dos tutoriales siguientes continúan explorando plantillas, empezando por una mirada al uso de TemplateFields en DetailsView. Después, pasaremos a FormView, que usa plantillas en lugar de campos para proporcionar mayor flexibilidad en el diseño y la estructura de los datos.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue dan Jagers. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](custom-formatting-based-upon-data-cs.md)
> [Siguiente](using-templatefields-in-the-detailsview-control-cs.md)
