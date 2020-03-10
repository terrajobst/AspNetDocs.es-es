---
uid: web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
title: Nomenclatura de los IDENTIFICADOres de control en las páginas de contenido (VB) | Microsoft Docs
author: rick-anderson
description: Muestra cómo actúan los controles de ContentPlaceHolder como un contenedor de nomenclatura y, por tanto, hacer trabajar mediante programación con un control difícil (a través de FindControl)...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: dbb024a6-f043-4fc5-ad66-56556711875b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/control-id-naming-in-content-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 3cb8dec47040bc65f1a024325c91590729ffbdb7
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78519241"
---
# <a name="control-id-naming-in-content-pages-vb"></a>Nomenclatura de los identificadores de control en las páginas de contenido (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_05_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_05_VB.pdf)

> Muestra cómo los controles de ContentPlaceHolder actúan como un contenedor de nomenclatura y, por consiguiente, se puede trabajar mediante programación con un control difícil (a través de FindControl). Observa este problema y soluciones alternativas. También describe cómo obtener acceso mediante programación al valor de ClientID resultante.

## <a name="introduction"></a>Introducción

Todos los controles de servidor ASP.NET incluyen una propiedad `ID` que identifica de forma única el control y es el medio por el que se tiene acceso mediante programación al control en la clase de código subyacente. Del mismo modo, los elementos de un documento HTML pueden incluir un `id` atributo que identifica de forma única el elemento; Estos valores `id` se usan a menudo en el script del lado cliente para hacer referencia mediante programación a un elemento HTML determinado. Dado esto, puede suponer que cuando un control de servidor ASP.NET se representa en HTML, su valor `ID` se usa como el valor `id` del elemento HTML representado. Esto no es necesariamente así porque, en determinadas circunstancias, un único control con un solo valor de `ID` puede aparecer varias veces en el marcado representado. Considere un control GridView que incluye una propiedad TemplateField con un control Web Label con un valor `ID` de `ProductName`. Cuando GridView se enlaza a su origen de datos en tiempo de ejecución, esta etiqueta se repite una vez para cada fila de GridView. Cada etiqueta representada necesita un valor de `id` único.

Para controlar estos escenarios, ASP.NET permite que determinados controles se denotan como contenedores de nomenclatura. Un contenedor de nomenclatura actúa como nuevo espacio de nombres de `ID`. Los controles de servidor que aparecen en el contenedor de nomenclatura tienen el valor `id` representado con el prefijo `ID` del control contenedor de nomenclatura. Por ejemplo, las clases `GridView` y `GridViewRow` son contenedores de nomenclatura. Por consiguiente, un control de etiqueta definido en una propiedad de la propiedad GridView con `ID` `ProductName` recibe un valor `id` representado de `GridViewID_GridViewRowID_ProductName`. Dado que *GridViewRowID* es único para cada fila de GridView, los valores de `id` resultantes son únicos.

> [!NOTE]
> La [interfaz de`INamingContainer`](https://msdn.microsoft.com/library/system.web.ui.inamingcontainer.aspx) se utiliza para indicar que un control de servidor ASP.net determinado debe funcionar como un contenedor de nomenclatura. La interfaz `INamingContainer` no escribe ningún método que el control de servidor debe implementar. en su lugar, se usa como un marcador. Al generar el marcado representado, si un control implementa esta interfaz, el motor ASP.NET antepone automáticamente su valor `ID` a sus valores de atributo `id` representados de los descendientes. Este proceso se describe con más detalle en el paso 2.

Los contenedores de nomenclatura no solo cambian el valor de atributo representado `id`, sino que también afectan al modo en que se puede hacer referencia al control mediante programación desde la clase de código subyacente de la página ASP.NET. El método `FindControl("controlID")` se utiliza normalmente para hacer referencia a un control Web mediante programación. Sin embargo, `FindControl` no penetra a través de contenedores de nomenclatura. Por consiguiente, no puede usar directamente el método `Page.FindControl` para hacer referencia a los controles de un control GridView u otro contenedor de nomenclatura.

Como puede tener surmised, las páginas maestras y ContentPlaceHolders se implementan como contenedores de nomenclatura. En este tutorial se examina cómo afectan las páginas maestras a los valores de `id` de elementos HTML y cómo hacer referencia mediante programación a los controles web dentro de una página de contenido mediante `FindControl`.

## <a name="step-1-adding-a-new-aspnet-page"></a>Paso 1: agregar una nueva página de ASP.NET

Para demostrar los conceptos descritos en este tutorial, vamos a agregar una nueva página de ASP.NET a nuestro sitio Web. Cree una nueva página de contenido denominada `IDIssues.aspx` en la carpeta raíz, enlazándolo a la página maestra de `Site.master`.

![Agregue la página de contenido IDIssues. aspx a la carpeta raíz.](control-id-naming-in-content-pages-vb/_static/image1.png)

**Figura 01**: agregar la página de contenido `IDIssues.aspx` a la carpeta raíz

Visual Studio crea automáticamente un control de contenido para cada uno de los cuatro ContentPlaceHolders de la página maestra. Como se indicó en el tutorial de [*varios ContentPlaceHolders y contenido predeterminado*](multiple-contentplaceholders-and-default-content-vb.md) , si no hay un control de contenido, se genera en su lugar el contenido predeterminado de ContentPlaceHolder de la página maestra. Dado que los `QuickLoginUI` y `LeftColumnContent` ContentPlaceHolders contienen el marcado predeterminado adecuado para esta página, continúe y quite los controles de contenido correspondientes de `IDIssues.aspx`. En este momento, el marcado declarativo de la página de contenido debe ser similar al siguiente:

[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample1.aspx)]

En el tutorial sobre cómo [*especificar el título, las etiquetas meta y otros encabezados HTML en la página maestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) se ha creado una clase de página base personalizada (`BasePage`) que configura automáticamente el título de la página si no se establece explícitamente. Para que la página `IDIssues.aspx` emplee esta funcionalidad, la clase de código subyacente de la página debe derivarse de la clase `BasePage` (en lugar de `System.Web.UI.Page`). Modifique la definición de la clase de código subyacente para que tenga el aspecto siguiente:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample2.vb)]

Por último, actualice el archivo de `Web.sitemap` para incluir una entrada para esta nueva lección. Agregue un elemento `<siteMapNode>` y establezca sus atributos `title` y `url` en "controlar los problemas de nomenclatura de IDENTIFICADOres" y `~/IDIssues.aspx`, respectivamente. Después de hacer esta adición, el marcado del archivo `Web.sitemap` debe ser similar al siguiente:

[!code-xml[Main](control-id-naming-in-content-pages-vb/samples/sample3.xml)]

Como se muestra en la figura 2, la nueva entrada del mapa del sitio en `Web.sitemap` se refleja inmediatamente en la sección lecciones de la columna izquierda.

![La sección lecciones incluye ahora un vínculo a los problemas de nomenclatura de los IDENTIFICADOres de &quot;control&quot;](control-id-naming-in-content-pages-vb/_static/image2.png)

**Figura 02**: ahora la sección lecciones incluye un vínculo a "controlar los problemas de nomenclatura de identificadores"

## <a name="step-2-examining-the-renderedidchanges"></a>Paso 2: examinar los cambios de`ID`representados

Para comprender mejor las modificaciones que el motor ASP.NET realiza en los valores de `id` representados de los controles de servidor, vamos a agregar algunos controles Web a la página `IDIssues.aspx` y, a continuación, ver el marcado representado que se envía al explorador. En concreto, escriba el texto "Ingrese su edad:" seguido de un control Web TextBox. Más abajo en la página agregue un control Web Button y un control Web Label. Establezca las propiedades `ID` y `Columns` del cuadro de texto en `Age` y 3, respectivamente. Establezca las propiedades `Text` y `ID` del botón en "Submit" y `SubmitButton`. Borre la propiedad `Text` de la etiqueta y establezca su `ID` en `Results`.

En este momento, el marcado declarativo del control de contenido debe ser similar al siguiente:

[!code-aspx[Main](control-id-naming-in-content-pages-vb/samples/sample4.aspx)]

En la ilustración 3 se muestra la página cuando se ve a través del diseñador de Visual Studio.

[![la página incluye tres controles Web: un cuadro de texto, un botón y una etiqueta](control-id-naming-in-content-pages-vb/_static/image4.png)](control-id-naming-in-content-pages-vb/_static/image3.png)

**Figura 03**: la página incluye tres controles Web: un cuadro de texto, un botón y una etiqueta ([haga clic para ver la imagen de tamaño completo](control-id-naming-in-content-pages-vb/_static/image5.png))

Visite la página a través de un explorador y, a continuación, vea el código fuente HTML. Como se muestra en el siguiente marcado, los valores de `id` de los elementos HTML de los controles Web de cuadro de texto, botón y etiqueta son una combinación de los valores `ID` de los controles Web y los valores `ID` de los contenedores de nomenclatura de la página.

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample5.html)]

Como se indicó anteriormente en este tutorial, tanto la página maestra como su ContentPlaceHolders sirven como contenedores de nomenclatura. Por consiguiente, ambos contribuyen a los valores de `ID` representados de sus controles anidados. Tome el atributo `id` del cuadro de texto, por ejemplo: `ctl00_MainContent_Age`. Recuerde que se `Age`el valor de `ID` del control de cuadro de texto. Tiene como prefijo el valor `ID` del control ContentPlaceHolder, `MainContent`. Además, este valor tiene como prefijo el valor `ID` de la página maestra, `ctl00`. El efecto neto es un valor de atributo `id` que consta de los valores `ID` de la página maestra, el control ContentPlaceHolder y el propio cuadro de texto.

En la figura 4 se muestra este comportamiento. Para determinar el `id` representado del cuadro de texto `Age`, empiece con el valor `ID` del control TextBox, `Age`. A continuación, trabaje hasta la jerarquía de controles. En cada contenedor de nomenclatura (los nodos con un color melocotón), Prefije el `id` representado actual con el `id`del contenedor de nomenclatura.

![Los atributos de identificador representado se basan en los valores de ID. de los contenedores de nomenclatura](control-id-naming-in-content-pages-vb/_static/image6.png)

**Figura 04**: los atributos de `id` representados se basan en los valores `ID` de los contenedores de nomenclatura

> [!NOTE]
> Como hemos comentado, la parte `ctl00` del atributo `id` representado constituye el valor de `ID` de la página maestra, pero es posible que se pregunte cómo llegó este valor de `ID`. No se especifica en ninguna parte de la página maestra o de contenido. La mayoría de los controles de servidor de una página ASP.NET se agregan explícitamente a través del marcado declarativo de la página. El control ContentPlaceHolder `MainContent` se especificó explícitamente en el marcado de `Site.master`; se definió el `Age` cuadro de texto `IDIssues.aspx`marcado de. Podemos especificar los valores de `ID` para estos tipos de controles a través de la ventana Propiedades o de la sintaxis declarativa. Otros controles, como la propia página maestra, no se definen en el marcado declarativo. Por consiguiente, los valores de `ID` se deben generar automáticamente. El motor ASP.NET establece los valores de `ID` en tiempo de ejecución para los controles cuyos identificadores no se han establecido explícitamente. Usa el patrón de nomenclatura `ctlXX`, donde *XX* es un valor entero que aumenta secuencialmente.

Dado que la propia página maestra sirve como contenedor de nomenclatura, los controles Web definidos en la página maestra también han modificado los valores de atributo de `id` representados. Por ejemplo, la etiqueta de `DisplayDate` que agregamos a la página maestra en el tutorial [*crear un diseño de todo el sitio con páginas maestras*](creating-a-site-wide-layout-using-master-pages-vb.md) tiene el siguiente marcado representado:

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample6.html)]

Tenga en cuenta que el atributo `id` incluye el valor `ID` (`ctl00`) de la página maestra y el valor `ID` del control Web Label (`DateDisplay`).

## <a name="step-3-programmatically-referencing-web-controls-viafindcontrol"></a>Paso 3: hacer referencia mediante programación a controles Web a través de`FindControl`

Cada control de servidor ASP.NET incluye un método `FindControl("controlID")` que busca en los descendientes del control un control denominado *ControlID*. Si se encuentra este tipo de control, se devuelve; Si no se encuentra ningún control que coincida, `FindControl` devuelve `Nothing`.

`FindControl` es útil en escenarios en los que es necesario tener acceso a un control, pero no tiene una referencia directa a él. Al trabajar con controles Web de datos como GridView, por ejemplo, los controles de los campos de GridView se definen una vez en la sintaxis declarativa, pero en tiempo de ejecución se crea una instancia del control para cada fila de GridView. Por consiguiente, los controles generados en tiempo de ejecución existen, pero no tenemos una referencia directa disponible desde la clase de código subyacente. Como resultado, es necesario usar `FindControl` para trabajar mediante programación con un control específico dentro de los campos de GridView. (Para obtener más información sobre el uso de `FindControl` para tener acceso a los controles dentro de las plantillas de un control Web de datos, vea [formato personalizado basado en datos](../../data-access/custom-formatting/custom-formatting-based-upon-data-vb.md)). Este mismo escenario se produce al agregar dinámicamente controles Web a un formulario Web Forms, un tema que se describe en [creación de datos dinámicos interfaces de usuario de entrada](https://msdn.microsoft.com/library/aa479330.aspx).

Para ilustrar el uso del método `FindControl` para buscar controles en una página de contenido, cree un controlador de eventos para el evento `Click` del `SubmitButton`. En el controlador de eventos, agregue el código siguiente, que hace referencia mediante programación al cuadro de texto `Age` y `Results` etiqueta mediante el método `FindControl` y, a continuación, muestra un mensaje en `Results` en función de la entrada del usuario.

> [!NOTE]
> Por supuesto, no es necesario usar `FindControl` para hacer referencia a los controles de etiqueta y de cuadro de texto para este ejemplo. Podríamos hacer referencia a ellas directamente a través de sus valores de propiedad `ID`. Utilizo `FindControl` aquí para ilustrar lo que sucede cuando se usa `FindControl` desde una página de contenido.

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample7.vb)]

Aunque la sintaxis que se usa para llamar al método `FindControl` difiere ligeramente en las dos primeras líneas de `SubmitButton_Click`, son semánticamente equivalentes. Recuerde que todos los controles de servidor de ASP.NET incluyen un método `FindControl`. Esto incluye la clase `Page`, de la que se deben derivar todas las clases de código subyacente de ASP.NET. Por lo tanto, llamar a `FindControl("controlID")` es equivalente a llamar a `Page.FindControl("controlID")`, suponiendo que no se ha invalidado el método `FindControl` en la clase de código subyacente o en una clase base personalizada.

Después de escribir este código, visite la página `IDIssues.aspx` a través de un explorador, escriba su edad y haga clic en el botón "enviar". Al hacer clic en el botón "enviar", se genera un `NullReferenceException` (vea la figura 5).

[![se genera una excepción NullReferenceException](control-id-naming-in-content-pages-vb/_static/image8.png)](control-id-naming-in-content-pages-vb/_static/image7.png)

**Figura 05**: se genera un `NullReferenceException` ([haga clic para ver la imagen de tamaño completo](control-id-naming-in-content-pages-vb/_static/image9.png))

Si establece un punto de interrupción en el controlador de eventos `SubmitButton_Click` verá que las dos llamadas a `FindControl` devuelven `Nothing`. El `NullReferenceException` se genera cuando se intenta tener acceso a la propiedad `Text` del cuadro de texto `Age`.

El problema es que `Control.FindControl` solo busca descendientes del *control*que estén en el mismo contenedor de nomenclatura. Dado que la página maestra constituye un nuevo contenedor de nomenclatura, una llamada a `Page.FindControl("controlID")` nunca recupera el objeto de la página maestra `ctl00`. (Consulte la figura 4 para ver la jerarquía de controles, que muestra el objeto `Page` como el elemento primario del objeto de página principal `ctl00`). Por lo tanto, no se encuentran los `Results` etiqueta y `Age` cuadro de texto y `ResultsLabel` y `AgeTextBox` tienen asignados valores de `Nothing`.

Hay dos soluciones alternativas a este desafío: podemos explorar en profundidad, un contenedor de nomenclatura a la vez, en el control adecuado. o bien, podemos crear nuestro propio método `FindControl` que contenga la propiedad de los contenedores de nomenclatura. Vamos a examinar cada una de estas opciones.

### <a name="drilling-into-the-appropriate-naming-container"></a>Obtención de detalles del contenedor de nomenclatura adecuado

Para usar `FindControl` para hacer referencia a la etiqueta de `Results` o `Age` cuadro de texto, es necesario llamar a `FindControl` desde un control antecesor en el mismo contenedor de nomenclatura. Como se mostró en la figura 4, el `MainContent` control ContentPlaceHolder es el único antecesor de `Results` o `Age` que se encuentra en el mismo contenedor de nomenclatura. En otras palabras, al llamar al método `FindControl` desde el control `MainContent`, tal como se muestra en el siguiente fragmento de código, se devuelve correctamente una referencia a los controles `Results` o `Age`.

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample8.vb)]

Sin embargo, no podemos trabajar con la `MainContent` ContentPlaceHolder de nuestra clase de código subyacente de la página de contenido con la sintaxis anterior, ya que el ContentPlaceHolder se define en la página maestra. En su lugar, tenemos que usar `FindControl` para obtener una referencia a `MainContent`. Reemplace el código del controlador de eventos `SubmitButton_Click` por las modificaciones siguientes:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample9.vb)]

Si visita la página a través de un explorador, escribe su edad y hace clic en el botón "enviar", se genera un `NullReferenceException`. Si establece un punto de interrupción en el controlador de eventos `SubmitButton_Click` verá que esta excepción se produce al intentar llamar al método `FindControl` del objeto `MainContent`. El objeto `MainContent` es igual a `Nothing` porque el método `FindControl` no encuentra un objeto denominado "MainContent". La razón subyacente es la misma que con la etiqueta de `Results` y `Age` controles de cuadro de texto: `FindControl` inicia su búsqueda desde la parte superior de la jerarquía de controles y no penetra en los contenedores de nomenclatura, pero el `MainContent` ContentPlaceHolder está dentro de la página maestra, que es un contenedor de nomenclatura.

Antes de poder usar `FindControl` para obtener una referencia a `MainContent`, primero necesitamos una referencia al control de página maestra. Una vez que tenemos una referencia a la página maestra, podemos obtener una referencia al `MainContent` ContentPlaceHolder a través de `FindControl` y, desde allí, referencias a la etiqueta de `Results` y al cuadro de texto `Age` (de nuevo, mediante `FindControl`). Pero, ¿cómo se obtiene una referencia a la página maestra? Al inspeccionar los atributos de `id` en el marcado representado, es evidente que el valor de `ID` de la página maestra es `ctl00`. Por lo tanto, podríamos usar `Page.FindControl("ctl00")` para obtener una referencia a la página maestra y, a continuación, usar ese objeto para obtener una referencia a `MainContent`, etc. En el fragmento de código siguiente se muestra esta lógica:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample10.vb)]

Aunque este código funcionará ciertamente, se supone que la `ID` generada automáticamente de la página maestra siempre se `ctl00`. Nunca es una buena idea crear suposiciones sobre valores generados automáticamente.

Afortunadamente, se puede tener acceso a una referencia a la página maestra a través de la propiedad `Master` de la clase `Page`. Por lo tanto, en lugar de tener que usar `FindControl("ctl00")` para obtener una referencia de la página maestra con el fin de tener acceso a la `MainContent` ContentPlaceHolder, en su lugar podemos usar `Page.Master.FindControl("MainContent")`. Actualice el controlador de eventos `SubmitButton_Click` con el código siguiente:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample11.vb)]

Esta vez, visite la página a través de un explorador, escriba su edad y haga clic en el botón "enviar" para mostrar el mensaje en la etiqueta de `Results`, según lo previsto.

[![la edad del usuario se muestra en la etiqueta](control-id-naming-in-content-pages-vb/_static/image11.png)](control-id-naming-in-content-pages-vb/_static/image10.png)

**Figura 06**: la edad del usuario se muestra en la etiqueta ([haga clic para ver la imagen de tamaño completo](control-id-naming-in-content-pages-vb/_static/image12.png))

### <a name="recursively-searching-through-naming-containers"></a>Buscar de forma recursiva en contenedores de nomenclatura

La razón por la que el ejemplo de código anterior hacía referencia al control ContentPlaceHolder `MainContent` desde la página maestra y, a continuación, la etiqueta `Results` y `Age` controles TextBox de `MainContent`, se debe a que el método `Control.FindControl` solo busca en el contenedor de nomenclatura del *control*. Tener `FindControl` permanecer dentro del contenedor de nomenclatura tiene sentido en la mayoría de los escenarios porque dos controles de dos contenedores de nomenclatura diferentes pueden tener los mismos valores `ID`. Considere el caso de un GridView que defina un control Web Label denominado `ProductName` dentro de uno de sus TemplateFields. Cuando los datos están enlazados a GridView en tiempo de ejecución, se crea una etiqueta de `ProductName` para cada fila de GridView. Si `FindControl` buscar en todos los contenedores de nomenclatura y llamamos a `Page.FindControl("ProductName")`, ¿qué instancia de etiqueta debe devolver el `FindControl`? ¿`ProductName` etiqueta de la primera fila de GridView? ¿El de la última fila?

Por tanto, tener `Control.FindControl` contenedor de nomenclatura del *control*solo tiene sentido en la mayoría de los casos. Pero hay otros casos, como el que nos enfrenta, donde tenemos una `ID` única en todos los contenedores de nomenclatura y quieren evitar tener que hacer referencia minuciosamente a cada contenedor de nomenclatura de la jerarquía de controles para tener acceso a un control. Tener una variante `FindControl` que busque de forma recursiva todos los contenedores de nomenclatura también tiene sentido. Desafortunadamente, el .NET Framework no incluye este tipo de método.

La buena noticia es que podemos crear nuestro propio método `FindControl` que busque de forma recursiva todos los contenedores de nomenclatura. De hecho, el uso de *métodos de extensión* podemos recortar un método `FindControlRecursive` a la clase `Control` para acompañar a su método `FindControl` existente.

> [!NOTE]
> Los métodos de extensión son una característica C# que es nueva en 3,0 y Visual Basic 9, que son los lenguajes que se incluyen con la .NET Framework versión 3,5 y Visual Studio 2008. En Resumen, los métodos de extensión permiten a los desarrolladores crear un nuevo método para un tipo de clase existente mediante una sintaxis especial. Para obtener más información sobre esta característica útil, consulte el artículo sobre cómo [extender la funcionalidad de tipo base con métodos de extensión](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx).

Para crear el método de extensión, agregue un nuevo archivo a la carpeta `App_Code` denominada `PageExtensionMethods.vb`. Agregue un método de extensión denominado `FindControlRecursive` que tome como entrada un parámetro `String` denominado `controlID`. Para que los métodos de extensión funcionen correctamente, es fundamental que la clase se marque como `Module` y que los métodos de extensión tengan como prefijo el `<Extension()>` atributo. Además, todos los métodos de extensión deben aceptar como su primer parámetro un objeto del tipo al que se aplica el método de extensión.

Agregue el código siguiente al archivo `PageExtensionMethods.vb` para definir este `Module` y el método de extensión `FindControlRecursive`:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample12.vb)]

Con este código en su lugar, vuelva a la clase de código subyacente de la página `IDIssues.aspx` y comente las llamadas al método de `FindControl` actual. Reemplácelos por llamadas a `Page.FindControlRecursive("controlID")`. Lo que se refiere a los métodos de extensión es que aparecen directamente en las listas desplegables de IntelliSense. Como se muestra en la figura 7, cuando se escribe `Page` y, a continuación, se alcanza el período, el método de `FindControlRecursive` se incluye en la lista desplegable de IntelliSense junto con los demás métodos de clase `Control`.

[![métodos de extensión se incluyen en las listas desplegables de IntelliSense](control-id-naming-in-content-pages-vb/_static/image14.png)](control-id-naming-in-content-pages-vb/_static/image13.png)

**Figura 07**: los métodos de extensión se incluyen en las listas desplegables de IntelliSense ([haga clic para ver la imagen de tamaño completo](control-id-naming-in-content-pages-vb/_static/image15.png))

Escriba el código siguiente en el controlador de eventos `SubmitButton_Click` y, a continuación, pruébelo; para ello, visite la página, escriba su edad y haga clic en el botón "enviar". Tal y como se muestra en la figura 6, la salida resultante será el mensaje "¡ es edad Age years!"

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample13.vb)]

> [!NOTE]
> Dado que los métodos de extensión C# son nuevos en 3,0 y Visual Basic 9, si usa Visual Studio 2005 no puede usar métodos de extensión. En su lugar, debe implementar el método `FindControlRecursive` en una clase auxiliar. [Rick Strahl](http://www.west-wind.com/WebLog/default.aspx) tiene este ejemplo en su entrada de blog, [ASP.net maestra pages and `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx).

## <a name="step-4-using-the-correctidattribute-value-in-client-side-script"></a>Paso 4: usar el valor de atributo`id`correcto en el script del lado cliente

Como se indicó en la introducción de este tutorial, a menudo se usa un atributo `id` representado del control Web en el script del lado cliente para hacer referencia mediante programación a un elemento HTML determinado. Por ejemplo, el siguiente código JavaScript hace referencia a un elemento HTML por su `id` y, a continuación, muestra su valor en un cuadro de mensaje modal:

[!code-csharp[Main](control-id-naming-in-content-pages-vb/samples/sample14.cs)]

Recuerde que en las páginas de ASP.NET que no incluyen un contenedor de nomenclatura, el atributo `id` del elemento HTML representado es idéntico al valor de la propiedad `ID` del control Web. Por este motivo, es tentador codificar de forma rígida los valores de los atributos de `id` en el código JavaScript. Es decir, si sabe que desea tener acceso al control Web TextBox `Age` a través de un script del lado cliente, hágalo a través de una llamada a `document.getElementById("Age")`.

El problema de este enfoque es que al usar páginas maestras (u otros controles de contenedor de nomenclatura), el `id` HTML representado no es sinónimo de la propiedad `ID` del control Web. La primera inclinación puede ser visitar la página a través de un explorador y ver el origen para determinar el atributo de `id` real. Una vez que conozca el valor de `id` representado, puede pegarlo en la llamada a `getElementById` para tener acceso al elemento HTML con el que necesita trabajar a través de un script del lado cliente. Este enfoque es menor que el ideal porque ciertos cambios en la jerarquía de control de la página o los cambios en las propiedades de `ID` de los controles de nomenclatura modificarán el atributo de `id` resultante, con lo que se interrumpirá el código de JavaScript.

La buena noticia es que el valor del atributo `id` que se representa es accesible en el código del lado servidor a través de la [propiedad`ClientID`](https://msdn.microsoft.com/library/system.web.ui.control.clientid.aspx)del control Web. Debe utilizar esta propiedad para determinar el valor de atributo `id` utilizado en el script del lado cliente. Por ejemplo, para agregar una función de JavaScript a la página que, cuando se llama, muestra el valor del cuadro de texto `Age` en un cuadro de mensaje modal, agregue el código siguiente al controlador de eventos de `Page_Load`:

[!code-vb[Main](control-id-naming-in-content-pages-vb/samples/sample15.vb)]

En el código anterior se inserta el valor de la propiedad `ClientID` del cuadro de texto `Age` en la llamada de JavaScript a `getElementById`. Si visita esta página a través de un explorador y ve el código fuente HTML, encontrará el siguiente código JavaScript:

[!code-html[Main](control-id-naming-in-content-pages-vb/samples/sample16.html)]

Observe cómo el valor de atributo `id` correcto, `ctl00_MainContent_Age`, aparece dentro de la llamada a `getElementById`. Dado que este valor se calcula en tiempo de ejecución, funciona independientemente de los cambios posteriores en la jerarquía de controles de página.

> [!NOTE]
> En este ejemplo de JavaScript solo se muestra cómo agregar una función de JavaScript que haga referencia correctamente al elemento HTML representado por un control de servidor. Para usar esta función, tendría que crear JavaScript adicional para llamar a la función cuando se carga el documento o cuando se realiza alguna acción del usuario específica. Para obtener más información sobre estos y temas relacionados, consulte [trabajar con scripts de cliente](https://msdn.microsoft.com/library/aa479302.aspx).

## <a name="summary"></a>Resumen

Algunos controles de servidor ASP.NET actúan como contenedores de nomenclatura, lo que afecta a los valores de atributo representados `id` de sus controles descendientes, así como al ámbito de los controles que utiliza el método `FindControl`. En lo que respecta a las páginas maestras, tanto la propia página maestra como sus controles ContentPlaceHolder son contenedores de nomenclatura. Por lo tanto, es necesario hacer un poco más trabajo para hacer referencia mediante programación a los controles dentro de la página de contenido mediante `FindControl`. En este tutorial se han examinado dos técnicas: profundizar en el control ContentPlaceHolder y llamar a su método `FindControl`; y implantamos nuestra propia `FindControl` implementación que busca de forma recursiva en todos los contenedores de nomenclatura.

Además de los problemas de servidor que presentan los contenedores de nomenclatura con respecto a la referencia a controles Web, también hay problemas en el lado cliente. En ausencia de contenedores de nomenclatura, el valor de propiedad `ID` del control Web y el valor del atributo `id` representado son uno en el mismo. Pero con la adición del contenedor de nomenclatura, el atributo rendered `id` incluye tanto los valores de `ID` del control Web como los contenedores de nomenclatura de la ascendencia de la jerarquía de control. Estos problemas de nomenclatura son un problema, siempre que use la propiedad `ClientID` del control Web para determinar el valor de atributo de `id` representado en el script del lado cliente.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [ASP.NET páginas maestras y `FindControl`](http://www.west-wind.com/WebLog/posts/5127.aspx)
- [Creación de interfaces de usuario de entrada datos dinámicos](https://msdn.microsoft.com/library/aa479330.aspx)
- [Extender la funcionalidad de tipo base con métodos de extensión](http://aspnet.4guysfromrolla.com/articles/120507-1.aspx)
- [Cómo: hacer referencia al contenido de la página maestra de ASP.NET](https://msdn.microsoft.com/library/xxwa0ff0.aspx)
- [Páginas de maestro: sugerencias, trucos y capturas](http://www.odetocode.com/articles/450.aspx)
- [Trabajar con scripts del lado cliente](https://msdn.microsoft.com/library/aa479302.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. Los revisores responsables de este tutorial fueron Zack Jones y este Barnerjee. ¿Está interesado en revisar los próximos artículos de MSDN? En caso afirmativo, suéltelo en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Anterior](urls-in-master-pages-vb.md)
> [Siguiente](interacting-with-the-master-page-from-the-content-page-vb.md)
