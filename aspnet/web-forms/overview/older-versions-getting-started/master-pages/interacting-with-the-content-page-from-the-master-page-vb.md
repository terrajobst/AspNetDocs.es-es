---
uid: web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
title: Interactuar con la página de contenido desde la página maestra (VB) | Microsoft Docs
author: rick-anderson
description: Examina cómo llamar a métodos, establecer propiedades, etc. de la página de contenido desde el código de la página maestra.
ms.author: riande
ms.date: 07/11/2008
ms.assetid: a6e2e1a0-c925-43e9-b711-1f178fdd72d7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/interacting-with-the-content-page-from-the-master-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 5367ad1b7f2fa11c635ad95754c9bcc1edcb6c1d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78464539"
---
# <a name="interacting-with-the-content-page-from-the-master-page-vb"></a>Interactuar con la página de contenido desde la página maestra (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_07_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_07_VB.pdf)

> Examina cómo llamar a métodos, establecer propiedades, etc. de la página de contenido desde el código de la página maestra.

## <a name="introduction"></a>Introducción

En el tutorial anterior se examinó cómo interactuar con la página de contenido mediante programación con su página maestra. Recuerde que hemos actualizado la página maestra para incluir un control GridView que enumera los cinco productos agregados más recientemente. A continuación, creamos una página de contenido desde la que el usuario podría agregar un nuevo producto. Al agregar un nuevo producto, la página de contenido necesitaba indicar a la página maestra que actualice su GridView para incluir el producto recién agregado. Esta funcionalidad se logra mediante la adición de un método público a la página maestra que actualizó los datos enlazados a GridView y, a continuación, la invocación de ese método desde la página de contenido.

La forma más común de la interacción de contenido y página maestra se origina en la página de contenido. Sin embargo, es posible que la página maestra Rouse la página de contenido actual en acción, por lo que es posible que se necesite esta funcionalidad si la página maestra contiene elementos de interfaz de usuario que permiten a los usuarios modificar datos que también se muestran en la página de contenido. Considere una página de contenido que muestra la información de los productos en un control GridView y una página maestra que incluye un control de botón que, cuando se hace clic en él, duplica los precios de todos los productos. Al igual que en el ejemplo del tutorial anterior, el control GridView debe actualizarse después de hacer clic en el botón de doble precio para que muestre los nuevos precios, pero en este escenario se trata de la página maestra que necesita Rouse la página de contenido en acción.

En este tutorial se explica cómo se define la funcionalidad de invocación de la página maestra en la página contenido.

### <a name="instigating-programmatic-interaction-via-an-event-and-event-handlers"></a>Instigación de interacción mediante programación a través de un evento y controladores de eventos

La invocación de la funcionalidad de la página de contenido desde una página maestra es más desafiante que la otra. Dado que una página de contenido tiene una sola página maestra, al crear la interacción mediante programación desde la página de contenido sabemos qué métodos públicos y propiedades se encuentran en nuestra cancelación. Sin embargo, una página maestra puede tener muchas páginas de contenido diferentes, cada una con su propio conjunto de propiedades y métodos. ¿Cómo se puede escribir código en la página maestra para realizar alguna acción en su página de contenido cuando no se sabe qué página de contenido se invocará hasta el tiempo de ejecución?

Considere la posibilidad de usar un control Web ASP.NET, como el control de botón. Un control de botón puede aparecer en cualquier número de páginas de ASP.NET y necesita un mecanismo por el que puede alertar a la página en la que se hizo clic. Esto se logra mediante *eventos*. En concreto, el control de botón genera su evento `Click` cuando se hace clic en él. la página ASP.NET que contiene el botón puede responder opcionalmente a esa notificación a través de un *controlador de eventos*.

Este mismo patrón se puede usar para tener una funcionalidad de desencadenador de página maestra en sus páginas de contenido:

1. Agregue un evento a la página maestra.
2. Genere el evento cada vez que la página maestra necesite comunicarse con su página de contenido. Por ejemplo, si la página maestra necesita enviar una alerta a su página de contenido que el usuario ha duplicado los precios, su evento se generará inmediatamente después de que se dupliquen los precios.
3. Cree un controlador de eventos en las páginas de contenido que necesiten realizar alguna acción.

En el resto de este tutorial se implementa el ejemplo que se describe en la introducción; es decir, una página de contenido en la que se enumeran los productos de la base de datos y una página maestra que incluye un control de botón para duplicar los precios.

## <a name="step-1-displaying-products-in-a-content-page"></a>Paso 1: visualización de productos en una página de contenido

Nuestro primer orden de negocio es crear una página de contenido que muestre los productos de la base de datos Northwind. (Hemos agregado la base de datos Northwind al proyecto en el tutorial anterior, [*interactuando con la página maestra desde la página de contenido*](interacting-with-the-master-page-from-the-content-page-vb.md)). Comience agregando una nueva página ASP.NET a la carpeta `~/Admin` denominada `Products.aspx`, asegurándose de enlazarla a la página maestra `Site.master`. La figura 1 muestra el Explorador de soluciones después de que esta página se haya agregado al sitio Web.

[![agregar una nueva página ASP.NET a la carpeta admin](interacting-with-the-content-page-from-the-master-page-vb/_static/image2.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image1.png)

**Figura 01**: agregar una nueva página ASP.net a la carpeta `Admin` ([haga clic para ver la imagen de tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image3.png))

Recuerde que en el tutorial para [*especificar el título, las etiquetas meta y otros encabezados HTML en la página maestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) se ha creado una clase de página base personalizada denominada `BasePage` que genera el título de la página si no se ha establecido explícitamente. Vaya a la clase de código subyacente de la página de `Products.aspx` y haga que se derive de `BasePage` (en lugar de `System.Web.UI.Page`).

Por último, actualice el archivo de `Web.sitemap` para incluir una entrada para esta lección. Agregue el siguiente marcado debajo del `<siteMapNode>` para la lección de interacción de contenido a página maestra:

[!code-xml[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample1.xml)]

La adición de este elemento `<siteMapNode>` se refleja en la lista de lecciones (vea la figura 5).

Vuelva a `Products.aspx`. En el control de contenido de `MainContent`, agregue un control GridView y asígnele el nombre `ProductsGrid`. Enlace GridView a un nuevo control SqlDataSource denominado `ProductsDataSource`.

[![enlazar GridView a un nuevo control SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image5.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image4.png)

**Figura 02**: enlace de GridView a un nuevo control SqlDataSource ([haga clic para ver la imagen de tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image6.png))

Configure el Asistente para que use la base de datos Northwind. Si ha trabajado en el tutorial anterior, debe tener ya una cadena de conexión denominada `NorthwindConnectionString` en `Web.config`. Elija esta cadena de conexión en la lista desplegable, como se muestra en la figura 3.

[![configurar SqlDataSource para usar la base de datos Northwind](interacting-with-the-content-page-from-the-master-page-vb/_static/image8.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image7.png)

**Figura 03**: configuración de SqlDataSource para usar la base de datos Northwind ([haga clic para ver la imagen de tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image9.png))

A continuación, especifique la instrucción `SELECT` del control de origen de datos; para ello, elija la tabla Products en la lista desplegable y devuelva las columnas `ProductName` y `UnitPrice` (consulte la figura 4). Haga clic en siguiente y en finalizar para completar el Asistente para configurar orígenes de datos.

[![devolver los campos ProductName y UnitPrice de la tabla Products](interacting-with-the-content-page-from-the-master-page-vb/_static/image11.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image10.png)

**Figura 04**: devuelva los campos `ProductName` y `UnitPrice` de la tabla `Products` ([haga clic para ver la imagen de tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image12.png))

Así de simple. Después de completar el asistente, Visual Studio agrega dos BoundFields a GridView para reflejar los dos campos devueltos por el control SqlDataSource. A continuación se muestra el marcado de los controles GridView y SqlDataSource. En la ilustración 5 se muestran los resultados cuando se ven a través de un explorador.

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample2.aspx)]

[![cada producto y su precio aparecen en la lista de GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image14.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image13.png)

**Figura 05**: cada producto y su precio se muestran en GridView ([haga clic para ver la imagen de tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image15.png))

> [!NOTE]
> No dude en limpiar la apariencia de GridView. Algunas sugerencias incluyen dar formato al valor UnitPrice mostrado como moneda y usar colores de fondo y fuentes para mejorar la apariencia de la cuadrícula. Para obtener más información sobre cómo mostrar y dar formato a los datos en ASP.NET, consulte la [serie de tutoriales sobre cómo trabajar con datos](../../data-access/index.md).

## <a name="step-2-adding-a-double-prices-button-to-the-master-page"></a>Paso 2: agregar un botón de precios dobles a la página maestra

La siguiente tarea consiste en agregar un control Web de botón a la página maestra en el que, al hacer clic en él, se duplicará el precio de todos los productos de la base de datos. Abra la página maestra de `Site.master` y arrastre un botón desde el cuadro de herramientas hasta el diseñador, colocándolo debajo del control SqlDataSource `RecentProductsDataSource` que hemos agregado en el tutorial anterior. Establezca la propiedad `ID` del botón en `DoublePrice` y su propiedad `Text` en "Double Product prices".

Después, agregue un control SqlDataSource a la página maestra, asignándole un nombre `DoublePricesDataSource`. Este SqlDataSource se usará para ejecutar la instrucción `UPDATE` para duplicar todos los precios. En concreto, es necesario establecer sus propiedades `ConnectionString` y `UpdateCommand` en la cadena de conexión adecuada y `UPDATE` instrucción. A continuación, es necesario llamar al método `Update` del control SqlDataSource al hacer clic en el botón `DoublePrice`. Para establecer las propiedades `ConnectionString` y `UpdateCommand`, seleccione el control SqlDataSource y, a continuación, vaya a la ventana Propiedades. La propiedad `ConnectionString` muestra las cadenas de conexión que ya están almacenadas en `Web.config` en una lista desplegable. Elija la opción `NorthwindConnectionString` como se muestra en la figura 6.

[![configurar SqlDataSource para usar NorthwindConnectionString](interacting-with-the-content-page-from-the-master-page-vb/_static/image17.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image16.png)

**Figura 06**: configuración de SqlDataSource para usar el `NorthwindConnectionString` ([haga clic para ver la imagen de tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image18.png))

Para establecer la propiedad `UpdateCommand`, busque la opción UpdateQuery en el ventana Propiedades. Esta propiedad, cuando se selecciona, muestra un botón con puntos suspensivos; Haga clic en este botón para mostrar el cuadro de diálogo Editor de parámetros y comandos que se muestra en la figura 7. Escriba la siguiente instrucción `UPDATE` en el cuadro de texto del cuadro de diálogo:

[!code-sql[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample3.sql)]

Cuando se ejecuta esta instrucción, duplicará el valor `UnitPrice` de cada registro de la tabla `Products`.

[![establecer la propiedad UpdateCommand de SqlDataSource](interacting-with-the-content-page-from-the-master-page-vb/_static/image20.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image19.png)

**Figura 07**: establecer la propiedad `UpdateCommand` de SqlDataSource ([haga clic para ver la imagen de tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image21.png))

Después de establecer estas propiedades, el marcado declarativo de los controles Button y SqlDataSource debe ser similar al siguiente:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample4.aspx)]

Todo lo que queda es llamar a su método `Update` cuando se hace clic en el botón `DoublePrice`. Cree un controlador de eventos `Click` para el botón `DoublePrice` y agregue el código siguiente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample5.vb)]

Para probar esta funcionalidad, visite la página `~/Admin/Products.aspx` que creamos en el paso 1 y haga clic en el botón "dos precios de productos". Al hacer clic en el botón, se produce un postback y se ejecuta el controlador de eventos `Click` del botón `DoublePrice`, doblando los precios de todos los productos. La página se vuelve a representar y se devuelve el marcado y se vuelve a mostrar en el explorador. El control GridView en la página de contenido, sin embargo, muestra los mismos precios que antes de que se haga clic en el botón "Double Product prices". Esto se debe a que los datos cargados inicialmente en GridView tenían su estado almacenado en el estado de vista, por lo que no se vuelven a cargar en los postbacks a menos que se indique lo contrario. Si visita una página diferente y, a continuación, vuelve a la página `~/Admin/Products.aspx` verá los precios actualizados.

## <a name="step-3-raising-an-event-when-the-prices-are-doubled"></a>Paso 3: generar un evento cuando se duplican los precios

Dado que GridView en la página de `~/Admin/Products.aspx` no refleja inmediatamente el duplicado de precio, es posible que un usuario piense que no hizo clic en el botón "precios de productos dobles" o que no funcionaba. Pueden intentar hacer clic en el botón varias veces más, doblando los precios de nuevo y de nuevo. Para solucionar este comportamiento, es necesario que la cuadrícula de la página de contenido muestre los nuevos precios inmediatamente después de que se dupliquen.

Como se explicó anteriormente en este tutorial, es necesario generar un evento en la página maestra siempre que el usuario haga clic en el botón `DoublePrice`. Un evento es una forma de que una clase (un publicador de eventos) notifique a otro un conjunto de otras clases (los suscriptores de eventos) que se ha producido algo interesante. En este ejemplo, la página maestra es el publicador de eventos; las páginas de contenido que le interesan al hacer clic en el botón `DoublePrice` son los suscriptores.

Una clase se suscribe a un evento mediante la creación de un *controlador de eventos*, que es un método que se ejecuta en respuesta al evento que se está generando. El publicador define los eventos que genera definiendo un *delegado de eventos*. El delegado de eventos especifica qué parámetros de entrada debe aceptar el controlador de eventos. En el .NET Framework, los delegados de eventos no devuelven ningún valor y aceptan dos parámetros de entrada:

- `Object`, que identifica el origen del evento y
- Una clase derivada de `System.EventArgs`

El segundo parámetro pasado a un controlador de eventos puede incluir información adicional sobre el evento. Aunque la clase base `EventArgs` no pasa a lo largo de la información, el .NET Framework incluye una serie de clases que extienden `EventArgs` y abarcan propiedades adicionales. Por ejemplo, se pasa una instancia de `CommandEventArgs` a los controladores de eventos que responden al evento de `Command` e incluye dos propiedades informativas: `CommandArgument` y `CommandName`.

> [!NOTE]
> Para obtener más información sobre la creación, la generación y el control de eventos, vea [eventos y delegados](https://msdn.microsoft.com/library/17sde2xt.aspx) y [delegados de eventos en inglés simple](http://www.codeproject.com/KB/cs/eventdelegates.aspx).

Para definir un evento, use la sintaxis siguiente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample6.vb)]

Dado que solo necesitamos enviar una alerta a la página de contenido cuando el usuario hace clic en el botón `DoublePrice` y no es necesario pasar más información adicional, podemos usar el delegado de eventos `EventHandler`, que define un controlador de eventos que acepta como el segundo parámetro un objeto de tipo `System.EventArgs`. Para crear el evento en la página maestra, agregue la siguiente línea de código a la clase de código subyacente de la página maestra:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample7.vb)]

El código anterior agrega un evento público a la página maestra denominada `PricesDoubled`. Ahora necesitamos generar este evento una vez que se han duplicado los precios. Para generar un evento, use la sintaxis siguiente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample8.vb)]

Donde *Sender* y *EventArgs* son los valores que desea pasar al controlador de eventos del suscriptor.

Actualice el controlador de eventos `DoublePrice` `Click` con el código siguiente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample9.vb)]

Como antes, el controlador de eventos `Click` se inicia llamando al método `Update` del control `DoublePricesDataSource` SqlDataSource para duplicar los precios de todos los productos. A continuación se indican dos adiciones al controlador de eventos. En primer lugar, se actualizan los datos del `RecentProducts` GridView. Este GridView se agregó a la página maestra en el tutorial anterior y muestra los cinco productos agregados más recientemente. Es necesario actualizar esta cuadrícula para que muestre los precios Just-by dobles de estos cinco productos. A continuación, se genera el evento `PricesDoubled`. Una referencia a la propia página maestra (`Me`) se envía al controlador de eventos como el origen del evento y se envía un objeto `EventArgs` vacío como argumentos del evento.

## <a name="step-4-handling-the-event-in-the-content-page"></a>Paso 4: control del evento en la página de contenido

En este momento, la página maestra genera su evento `PricesDoubled` cada vez que se hace clic en el control de botón `DoublePrice`. Sin embargo, esta es solo la mitad de la batalla: todavía necesitamos controlar el evento en el suscriptor. Esto implica dos pasos: crear el controlador de eventos y agregar código de conexión de eventos para que cuando se produzca el evento, se ejecute el controlador de eventos.

Empiece por crear un controlador de eventos denominado `Master_PricesDoubled`. Debido a cómo definimos el evento `PricesDoubled` en la página maestra, los dos parámetros de entrada del controlador de eventos deben ser de tipos `Object` y `EventArgs`, respectivamente. En el controlador de eventos, llame al método `DataBind` de `ProductsGrid` GridView para volver a enlazar los datos a la cuadrícula.

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample10.vb)]

El código del controlador de eventos está completo, pero aún no se ha conectado el evento de `PricesDoubled` de la página maestra a este controlador de eventos. El suscriptor conecta un evento a un controlador de eventos a través de la sintaxis siguiente:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample11.vb)]

*Publisher* es una referencia al objeto que ofrece el *eventName*de evento y *MethodName* es el nombre del controlador de eventos definido en el suscriptor.

Este código de conexión de eventos debe ejecutarse en la primera visita de la página y los postbacks subsiguientes, y se debe producir en un punto del ciclo de vida de la página que precede al momento en que se puede generar el evento. Un buen momento para agregar el código de conexión de eventos se encuentra en la fase de PreInit, que se produce al principio del ciclo de vida de la página.

Abra `~/Admin/Products.aspx` y cree un controlador de eventos de `Page_PreInit`:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample12.vb)]

Para completar este código de cableado, necesitamos una referencia mediante programación a la página maestra desde la página de contenido. Como se indicó en el tutorial anterior, hay dos maneras de hacerlo:

- Convirtiendo la propiedad de `Page.Master` con tipo flexible en el tipo de página maestra adecuado, o bien
- Agregando una directiva de `@MasterType` en la página `.aspx` y usando después la propiedad fuertemente tipada `Master`.

Vamos a usar el último enfoque. Agregue la siguiente directiva de `@MasterType` a la parte superior del marcado declarativo de la página:

[!code-aspx[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample13.aspx)]

Después, agregue el siguiente código de conexión de eventos en el controlador de eventos `Page_PreInit`:

[!code-vb[Main](interacting-with-the-content-page-from-the-master-page-vb/samples/sample14.vb)]

Con este código en su lugar, el control GridView en la página de contenido se actualiza cada vez que se hace clic en el botón `DoublePrice`.

En las figuras 8 y 9 se muestra este comportamiento. En la ilustración 8 se muestra la página cuando se visita por primera vez. Tenga en cuenta que los valores de precio en el `RecentProducts` GridView (en la columna izquierda de la página maestra) y el `ProductsGrid` GridView (en la página de contenido). En la ilustración 9 se muestra la misma pantalla inmediatamente después de hacer clic en el botón `DoublePrice`. Como puede ver, los nuevos precios se reflejan de forma instantánea en ambos GridView.

[![los valores de precio iniciales](interacting-with-the-content-page-from-the-master-page-vb/_static/image23.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image22.png)

**Figura 08**: valores de precio iniciales ([haga clic para ver la imagen de tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image24.png))

[![los precios Just-by dobles se muestran en el GridView](interacting-with-the-content-page-from-the-master-page-vb/_static/image26.png)](interacting-with-the-content-page-from-the-master-page-vb/_static/image25.png)

**Figura 09**: los precios Just-by dobles se muestran en GridView ([haga clic para ver la imagen a tamaño completo](interacting-with-the-content-page-from-the-master-page-vb/_static/image27.png))

## <a name="summary"></a>Resumen

Idealmente, una página maestra y sus páginas de contenido son completamente independientes entre sí y no requieren ningún nivel de interacción. Sin embargo, si tiene una página maestra o una página de contenido que muestra datos que se pueden modificar desde la página maestra o la página de contenido, es posible que deba hacer que la página maestra avise a la página de contenido (o viceversa) cuando se modifiquen los datos para que se pueda actualizar la pantalla. En el tutorial anterior, vimos cómo hacer que una página de contenido interactúe mediante programación con su página maestra; en este tutorial, hemos visto cómo hacer que una página maestra iniciara la interacción.

Aunque la interacción mediante programación entre un contenido y una página maestra puede originarse en el contenido o en la página maestra, el patrón de interacción utilizado depende del origen. Las diferencias se deben al hecho de que una página de contenido tiene una sola página maestra, pero una página maestra puede tener muchas páginas de contenido diferentes. En lugar de hacer que una página maestra interactúe directamente con una página de contenido, un mejor enfoque es hacer que la página maestra provoque un evento para indicar que se ha producido alguna acción. Las páginas de contenido que le interesan a la acción pueden crear controladores de eventos.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Obtener acceso a los datos y actualizarlos en ASP.NET](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Eventos y delegados](https://msdn.microsoft.com/library/17sde2xt.aspx)
- [Pasar información entre el contenido y las páginas maestras](http://aspnet.4guysfromrolla.com/articles/013107-1.aspx)
- [Trabajar con datos en tutoriales de ASP.NET](../../data-access/index.md)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Banerjee. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-master-page-from-the-content-page-vb.md)
> [Siguiente](master-pages-and-asp-net-ajax-vb.md)
