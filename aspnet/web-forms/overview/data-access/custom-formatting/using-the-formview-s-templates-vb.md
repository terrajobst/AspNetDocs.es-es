---
uid: web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
title: Usar las plantillas de FormView (VB) | Microsoft Docs
author: rick-anderson
description: A diferencia de DetailsView, FormView no se compone de campos. En su lugar, el FormView se representa mediante plantillas. En este tutorial se examinará el uso de la
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 67b25f4c-2823-42b6-b07d-1d650b3fd711
msc.legacyurl: /web-forms/overview/data-access/custom-formatting/using-the-formview-s-templates-vb
msc.type: authoredcontent
ms.openlocfilehash: cafe47cf5766bb14503852ec6e9f305d1e6d426f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78426583"
---
# <a name="using-the-formviews-templates-vb"></a>Usar las plantillas de FormView (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/5/7/0/57084608-dfb3-4781-991c-407d086e2adc/ASPNET_Data_Tutorial_14_VB.exe) de la aplicación de ejemplo o [descarga de PDF](using-the-formview-s-templates-vb/_static/datatutorial14vb1.pdf)

> A diferencia de DetailsView, FormView no se compone de campos. En su lugar, el FormView se representa mediante plantillas. En este tutorial se examinará el uso del control FormView para presentar una presentación menos rígida de los datos.

## <a name="introduction"></a>Introducción

En los dos últimos tutoriales, vimos cómo personalizar las salidas de los controles GridView y DetailsView mediante TemplateFields. Los TemplateFields permiten que el contenido de un campo específico sea muy personalizado, pero en el extremo tanto GridView como DetailsView tienen una apariencia en lugar de boxy, similar a la cuadrícula. En el caso de muchos escenarios, como el diseño de una cuadrícula es ideal, pero a veces se necesita una pantalla menos rígida. Cuando se muestra un solo registro, es posible un diseño fluido mediante el control FormView.

A diferencia de DetailsView, FormView no se compone de campos. No se puede Agregar BoundField o TemplateField a FormView. En su lugar, el FormView se representa mediante plantillas. Piense en FormView como un control DetailsView que contiene una única TemplateField. FormView admite las siguientes plantillas:

- `ItemTemplate` usa para representar el registro determinado que se muestra en FormView
- `HeaderTemplate` utiliza para especificar una fila de encabezado opcional
- `FooterTemplate` se usa para especificar una fila de pie de página opcional
- `EmptyDataTemplate` cuando la `DataSource` de FormView no tiene ningún registro, se utiliza el `EmptyDataTemplate` en lugar de la `ItemTemplate` para representar el marcado del control.
- `PagerTemplate` se puede usar para personalizar la interfaz de paginación para FormViews que tienen habilitada la paginación
- `EditItemTemplate` / `InsertItemTemplate` usa para personalizar la interfaz de edición o la interfaz de inserción para FormViews que admiten esta funcionalidad

En este tutorial se examinará el uso del control FormView para presentar una presentación menos rígida de los productos. En lugar de tener campos para el nombre, la categoría, el proveedor, etc., el `ItemTemplate` de FormView mostrará estos valores mediante una combinación de un elemento de encabezado y un `<table>` (consulte la figura 1).

[![el FormView se descompone del diseño similar a la cuadrícula que se ha detectado en DetailsView](using-the-formview-s-templates-vb/_static/image2.png)](using-the-formview-s-templates-vb/_static/image1.png)

**Figura 1**: el FormView se rompe del diseño similar a la cuadrícula que se ve en DetailsView ([haga clic para ver la imagen de tamaño completo](using-the-formview-s-templates-vb/_static/image3.png))

## <a name="step-1-binding-the-data-to-the-formview"></a>Paso 1: enlazar los datos a FormView

Abra la página `FormView.aspx` y arrastre un FormView desde el cuadro de herramientas hasta el diseñador. La primera vez que se agrega el FormView aparece como un cuadro gris que indica que se necesita una `ItemTemplate`.

[![el FormView no se puede representar en el diseñador hasta que se proporciona un ItemTemplate](using-the-formview-s-templates-vb/_static/image5.png)](using-the-formview-s-templates-vb/_static/image4.png)

**Figura 2**: no se puede representar FormView en el diseñador hasta que se proporcione un `ItemTemplate` ([haga clic para ver la imagen de tamaño completo](using-the-formview-s-templates-vb/_static/image6.png))

El `ItemTemplate` se puede crear a mano (a través de la sintaxis declarativa) o se puede crear de forma automática enlazando FormView a un control de origen de datos a través del diseñador. Este `ItemTemplate` creado automáticamente contiene código HTML que muestra el nombre de cada campo y un control etiqueta cuya propiedad `Text` se enlaza al valor del campo. Este enfoque también crea automáticamente una `InsertItemTemplate` y `EditItemTemplate`, que se rellenan con controles de entrada para cada uno de los campos de datos devueltos por el control de origen de datos.

Si desea crear automáticamente la plantilla, en la etiqueta inteligente de FormView agregue un nuevo control ObjectDataSource que invoque el método de `GetProducts()` de la clase `ProductsBLL`. Esto creará un FormView con un `ItemTemplate`, `InsertItemTemplate`y `EditItemTemplate`. En la vista Código fuente, quite el `InsertItemTemplate` y `EditItemTemplate` ya que no le interesa crear un FormView que admita la edición o la inserción todavía. Después, borre el marcado dentro del `ItemTemplate` para que tengamos una pizarra limpia desde la que trabajar.

Si prefiere compilar el `ItemTemplate` manualmente, puede Agregar y configurar el origen de propiedades arrastrándolo desde el cuadro de herramientas hasta el diseñador. Sin embargo, no establezca el origen de datos de FormView desde el diseñador. En su lugar, vaya a la vista de código fuente y establezca manualmente la propiedad `DataSourceID` de FormView en el valor `ID` de ObjectDataSource. A continuación, agregue manualmente el `ItemTemplate`.

Independientemente del enfoque que decida realizar, en este momento el marcado declarativo de FormView debe ser similar al siguiente:

[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample1.aspx)]

Dedique un momento a activar la casilla habilitar paginación en la etiqueta inteligente de FormView. Esto agregará el atributo `AllowPaging="True"` a la sintaxis declarativa de FormView. Además, establezca la propiedad `EnableViewState` en false.

## <a name="step-2-defining-theitemtemplates-markup"></a>Paso 2: definir el marcado del`ItemTemplate`

Con FormView enlazado al control ObjectDataSource y configurado para admitir la paginación, estamos preparados para especificar el contenido de la `ItemTemplate`. En este tutorial, vamos a mostrar el nombre del producto en un encabezado de `<h3>`. A continuación, vamos a usar un `<table>` HTML para mostrar las propiedades de producto restantes en una tabla de cuatro columnas en la que la primera y la tercera columnas enumeran los nombres de propiedad y la segunda y cuarta lista de sus valores.

Este marcado se puede escribir en a través de la interfaz de edición de plantillas de FormView en el diseñador o escribirse manualmente mediante la sintaxis declarativa. Al trabajar con plantillas, normalmente resulta más rápido trabajar directamente con la sintaxis declarativa, pero no dude en usar cualquier técnica con la que esté más familiarizado.

El marcado siguiente muestra el marcado declarativo de FormView una vez completada la estructura del `ItemTemplate`:

[!code-aspx[Main](using-the-formview-s-templates-vb/samples/sample2.aspx)]

Observe que la sintaxis de DataBinding-`<%# Eval("ProductName") %>`, por ejemplo, se puede insertar directamente en la salida de la plantilla. Es decir, no es necesario asignar a la propiedad `Text` de un control de etiqueta. Por ejemplo, tenemos el valor `ProductName` mostrado en un elemento `<h3>` mediante `<h3><%# Eval("ProductName") %></h3>`, que para las Chai del producto se representará como `<h3>Chai</h3>`.

Las clases `ProductPropertyLabel` y `ProductPropertyValue` CSS se utilizan para especificar el estilo de los nombres y valores de las propiedades del producto en el `<table>`. Estas clases CSS se definen en `Styles.css` y hacen que los nombres de propiedad estén en negrita y alineados a la derecha y agreguen un relleno derecho a los valores de propiedad.

Dado que no hay ningún CheckBoxFields disponible con FormView, para mostrar el valor `Discontinued` como una casilla, debemos agregar nuestro propio control CheckBox. La propiedad `Enabled` está establecida en false, convirtiéndola en solo lectura y la propiedad `Checked` de la casilla está enlazada al valor del campo de datos `Discontinued`.

Una vez completado el `ItemTemplate`, la información del producto se muestra de manera mucho más fluida. Compare el resultado de DetailsView del último tutorial (Figura 3) con la salida generada por FormView en este tutorial (Figura 4).

[![la salida de DetailsView rígida](using-the-formview-s-templates-vb/_static/image8.png)](using-the-formview-s-templates-vb/_static/image7.png)

**Ilustración 3**: salida de DetailsView rígida ([haga clic para ver la imagen de tamaño completo](using-the-formview-s-templates-vb/_static/image9.png))

[![la salida de FormView de fluidos](using-the-formview-s-templates-vb/_static/image11.png)](using-the-formview-s-templates-vb/_static/image10.png)

**Ilustración 4**: salida de FormView de fluidos ([haga clic para ver la imagen de tamaño completo](using-the-formview-s-templates-vb/_static/image12.png))

## <a name="summary"></a>Resumen

Aunque los controles GridView y DetailsView pueden personalizar su salida mediante TemplateFields, ambos siguen presentando sus datos en un formato boxy de cuadrícula. Para aquellas ocasiones en las que es necesario mostrar un único registro con un diseño menos rígido, el FormView es una opción ideal. Como DetailsView, FormView representa un único registro de su `DataSource`, pero a diferencia de DetailsView, se compone solo de plantillas y no admite campos.

Como vimos en este tutorial, FormView permite un diseño más flexible al mostrar un único registro. En los tutoriales futuros, examinaremos los controles DataList y Repeater, que proporcionan el mismo nivel de flexibilidad que FormsView, pero pueden mostrar varios registros (por ejemplo, GridView).

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor del cliente potencial de este tutorial fue E.R. Gilmore. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](using-templatefields-in-the-detailsview-control-vb.md)
> [Siguiente](displaying-summary-information-in-the-gridview-s-footer-vb.md)
