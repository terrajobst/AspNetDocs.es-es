---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Establecer mediante programación los valores de parámetro de ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: En este tutorial veremos cómo agregar un método a la capa DAL y a la capa BLL que acepta un único parámetro de entrada y devuelve datos. En el ejemplo se establecerá este parámetro...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f1dd50f46528e8dd51f85e503604d3f0dbc21ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78465919"
---
# <a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Configurar los valores de parámetro de ObjectDataSource mediante programación (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) de la aplicación de ejemplo o [descarga de PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> En este tutorial veremos cómo agregar un método a la capa DAL y a la capa BLL que acepta un único parámetro de entrada y devuelve datos. En el ejemplo se establecerá este parámetro mediante programación.

## <a name="introduction"></a>Introducción

Como vimos en el [tutorial anterior](declarative-parameters-vb.md), hay varias opciones disponibles para pasar de forma declarativa los valores de parámetro a los métodos de ObjectDataSource. Si el valor del parámetro está codificado de forma rígida, procede de un control Web en la página o se encuentra en cualquier otro origen que sea legible para un origen de datos `Parameter` objeto, por ejemplo, ese valor se puede enlazar al parámetro de entrada sin escribir una línea de código.

No obstante, puede haber ocasiones en las que el valor del parámetro proceda de algún origen que aún no tenga en cuenta uno de los objetos de `Parameter` de origen de datos integrados. Si nuestro sitio admite cuentas de usuario, es posible que desee establecer el parámetro basándose en el identificador de usuario del visitante que ha iniciado sesión actualmente. O bien, es posible que necesitemos personalizar el valor del parámetro antes de enviarlo al método del objeto subyacente de ObjectDataSource.

Siempre que se invoca el método `Select` de ObjectDataSource, primero se genera su [evento de selección](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). A continuación, se invoca el método del objeto subyacente de ObjectDataSource. Una vez que complete los desencadenadores de [eventos seleccionados](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) de ObjectDataSource (la figura 1 muestra esta secuencia de eventos). Los valores de parámetro que se pasan al método del objeto subyacente de ObjectDataSource se pueden establecer o personalizar en un controlador de eventos para el evento `Selecting`.

[![se activan y seleccionan los eventos del origen ObjectDataSource antes y después de invocar el método de su objeto subyacente](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figura 1**: los eventos `Selected` y `Selecting` de ObjectDataSource se activan antes y después de invocar el método de su objeto subyacente ([haga clic para ver la imagen de tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))

En este tutorial, veremos cómo agregar un método a la capa DAL y a la capa BLL que acepta un único parámetro de entrada `Month`, de tipo `Integer` y devuelve un objeto `EmployeesDataTable` rellenado con los empleados que tienen su aniversario de contratación en el `Month`especificado. En nuestro ejemplo se establecerá este parámetro mediante programación basándose en el mes actual y se mostrará una lista de "aniversarios de los empleados este mes".

Comencemos.

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Paso 1: agregar un método a`EmployeesTableAdapter`

En el primer ejemplo, es necesario agregar un medio para recuperar los empleados cuyo `HireDate` haya ocurrido en un mes determinado. Para proporcionar esta funcionalidad de acuerdo con la arquitectura, necesitamos crear primero un método en `EmployeesTableAdapter` que se asigne a la instrucción SQL adecuada. Para ello, empiece abriendo el conjunto de DataSet con tipo Northwind. Haga clic con el botón derecho en la etiqueta `EmployeesTableAdapter` y elija Agregar consulta.

[![agregar una nueva consulta a EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figura 2**: agregar una nueva consulta a la `EmployeesTableAdapter` ([haga clic para ver la imagen de tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))

Elija Agregar una instrucción SQL que devuelva filas. Cuando llegue a la pantalla especificar una `SELECT` instrucción, la instrucción de `SELECT` predeterminada para el `EmployeesTableAdapter` ya se cargará. Simplemente agregue en la cláusula `WHERE`: `WHERE DATEPART(m, HireDate) = @Month`. [DatePart](https://msdn.microsoft.com/library/ms174420.aspx) es una función de T-SQL que devuelve una parte de fecha determinada de un tipo de `datetime`; en este caso, vamos a usar `DATEPART` para devolver el mes de la columna de `HireDate`.

[![devolver solo las filas en las que la columna HireDate es menor o igual que el parámetro @HiredBeforeDate](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figura 3**: devolver solo las filas en las que la `HireDate` columna es menor o igual que el parámetro `@HiredBeforeDate` ([haga clic para ver la imagen de tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))

Por último, cambie los nombres de método `FillBy` y `GetDataBy` a `FillByHiredDateMonth` y `GetEmployeesByHiredDateMonth`, respectivamente.

[![elegir nombres de método más adecuados que FillBy y GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Ilustración 4**: elegir nombres de método más adecuados que `FillBy` y `GetDataBy` ([haga clic para ver la imagen de tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))

Haga clic en finalizar para completar el asistente y volver a la superficie de diseño del conjunto de los mismos. El `EmployeesTableAdapter` debe incluir ahora un nuevo conjunto de métodos para tener acceso a los empleados contratados en un mes determinado.

[![los nuevos métodos aparecen en el Superficie de diseño del conjunto de los](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figura 5**: los nuevos métodos aparecen en el superficie de diseño del conjunto de[información (haga clic para ver la imagen de tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))

## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Paso 2: agregar el método`GetEmployeesByHiredDateMonth(month)`a la capa de lógica de negocios

Dado que la arquitectura de la aplicación usa una capa independiente para la lógica de negocios y la lógica de acceso a datos, es necesario agregar un método a la capa BLL que llama a la capa DAL para recuperar los empleados contratados antes de la fecha especificada. Abra el archivo `EmployeesBLL.vb` y agregue el siguiente método:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Al igual que con otros métodos de esta clase, `GetEmployeesByHiredDateMonth(month)` simplemente llama a la capa DAL y devuelve los resultados.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Paso 3: mostrar a los empleados cuyo aniversario de contratación es este mes

Nuestro último paso para este ejemplo es mostrar los empleados cuyo aniversario de contratación es este mes. Comience agregando un control GridView a la página `ProgrammaticParams.aspx` en la carpeta `BasicReporting` y agregue un nuevo ObjectDataSource como origen de datos. Configure ObjectDataSource para usar la clase `EmployeesBLL` con el `SelectMethod` establecido en `GetEmployeesByHiredDateMonth(month)`.

[![usar la clase EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figura 6**: uso de la clase `EmployeesBLL` ([haga clic para ver la imagen de tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))

[![seleccionar en el método GetEmployeesByHiredDateMonth (month)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figura 7**: seleccione en el método `GetEmployeesByHiredDateMonth(month)` ([haga clic para ver la imagen de tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))

La última pantalla nos pide que proporcione el `month` origen del valor del parámetro. Como estableceremos este valor mediante programación, deje el origen del parámetro establecido en la opción None predeterminada y haga clic en finalizar.

[![dejar el origen del parámetro establecido en ninguno](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figura 8**: deje el origen del parámetro establecido en ninguno ([haga clic para ver la imagen de tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))

Esto creará un objeto de `Parameter` en la colección de `SelectParameters` de ObjectDataSource que no tiene un valor especificado.

[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Para establecer este valor mediante programación, es necesario crear un controlador de eventos para el evento `Selecting` de ObjectDataSource. Para ello, vaya al Vista de diseño y haga doble clic en ObjectDataSource. Como alternativa, seleccione ObjectDataSource, vaya a la ventana Propiedades y haga clic en el icono de rayo. A continuación, haga doble clic en el cuadro de texto situado junto al evento `Selecting` o escriba el nombre del controlador de eventos que desea usar. Como tercera opción, puede crear el controlador de eventos seleccionando ObjectDataSource y su evento `Selecting` en las dos listas desplegables en la parte superior de la clase de código subyacente de la página.

![Haga clic en el icono de rayo en la ventana Propiedades para mostrar los eventos de un control Web.](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figura 9**: haga clic en el icono de rayo de la ventana Propiedades para mostrar los eventos de un control Web.

Los tres enfoques agregan un nuevo controlador de eventos para el evento de `Selecting` de ObjectDataSource a la clase de código subyacente de la página. En este controlador de eventos, se pueden leer y escribir en los valores de parámetro mediante `e.InputParameters(parameterName)`, donde *`parameterName`* es el valor del atributo `Name` en la etiqueta `<asp:Parameter>` (la colección `InputParameters` también se puede indizar de forma ordinal, como en `e.InputParameters(index)`). Para establecer el parámetro `month` en el mes actual, agregue lo siguiente al controlador de eventos `Selecting`:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Al visitar esta página a través de un explorador, podemos ver que solo se contrató a un empleado este mes (marzo) Laura Díaz, que se ha puesto en la empresa desde 1994.

[![los empleados cuyo aniversario de este mes se muestran](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figura 10**: los empleados cuyos aniversarios se muestran en este mes ([haga clic para ver la imagen de tamaño completo](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))

## <a name="summary"></a>Resumen

Aunque los valores de los parámetros de ObjectDataSource normalmente se pueden establecer de forma declarativa, sin necesidad de una línea de código, es fácil establecer los valores de parámetro mediante programación. Lo único que debemos hacer es crear un controlador de eventos para el evento `Selecting` de ObjectDataSource, que se activa antes de que se invoque el método del objeto subyacente, y establecer manualmente los valores de uno o varios parámetros a través de la colección `InputParameters`.

Este tutorial concluye la sección de informes básicos. En el [siguiente tutorial](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) se inicia la sección de escenarios de filtrado y detalles principales, en la que veremos técnicas para permitir que el visitante filtre datos y explore en profundidad un informe maestro en un informe de detalles.

¡ Feliz programación!

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros de ASP/ASP. net y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 2,0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en contacto con usted en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que encontrará en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Hilton Giesenow. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](declarative-parameters-vb.md)
