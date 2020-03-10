---
uid: web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
title: Especificar la página maestra mediante programación (VB) | Microsoft Docs
author: rick-anderson
description: Examina la configuración de la página maestra de la página de contenido mediante programación a través del controlador de eventos PreInit.
ms.author: riande
ms.date: 07/28/2008
ms.assetid: 0edcd653-f24a-41aa-aef4-75f868fe5ac2
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/specifying-the-master-page-programmatically-vb
msc.type: authoredcontent
ms.openlocfilehash: 3b039b22bef38ae6ebf80be070820dc1638f87f4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78457309"
---
# <a name="specifying-the-master-page-programmatically-vb"></a>Especificar la página maestra mediante programación (VB)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.zip) o [Descargar PDF](https://download.microsoft.com/download/d/6/6/d66ad554-afdd-409e-a5c3-201b774fbb31/ASPNET_MasterPages_Tutorial_09_VB.pdf)

> Examina la configuración de la página maestra de la página de contenido mediante programación a través del controlador de eventos PreInit.

## <a name="introduction"></a>Introducción

Como en el ejemplo inaugural de la [*creación de un diseño de todo el sitio mediante páginas maestras*](creating-a-site-wide-layout-using-master-pages-vb.md), todas las páginas de contenido hacían referencia a la página maestra mediante declaración a través del atributo `MasterPageFile` en la Directiva `@Page`. Por ejemplo, la siguiente directiva de `@Page` vincula la página de contenido a la página maestra `Site.master`:

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample1.aspx)]

La [clase`Page`](https://msdn.microsoft.com/library/system.web.ui.page.aspx) del espacio de nombres `System.Web.UI` incluye una [propiedad`MasterPageFile`](https://msdn.microsoft.com/library/system.web.ui.page.masterpagefile.aspx) que devuelve la ruta de acceso a la página maestra de la página de contenido; Esta propiedad se establece mediante la Directiva `@Page`. Esta propiedad también se puede usar para especificar mediante programación la página maestra de la página de contenido. Este enfoque es útil si desea asignar dinámicamente la página maestra en función de factores externos, como el usuario que visita la página.

En este tutorial se agregará una segunda página maestra a nuestro sitio web y se decidirá dinámicamente qué página maestra se va a usar en tiempo de ejecución.

## <a name="step-1-a-look-at-the-page-lifecycle"></a>Paso 1: echar un vistazo al ciclo de vida de la página

Siempre que una solicitud llega al servidor web para una página de ASP.NET que es una página de contenido, el motor de ASP.NET debe confundir los controles de contenido de la página en los controles ContentPlaceHolder correspondientes de la página maestra. Esta fusión crea una jerarquía de control única que puede continuar a lo largo del ciclo de vida de la página habitual.

En la ilustración 1 se muestra esta fusión. En el paso 1 de la figura 1 se muestran las jerarquías de control de contenido inicial y página maestra. En el final de la fase de preinicialización, los controles de contenido de la página se agregan al ContentPlaceHolders correspondiente en la página maestra (paso 2). Después de esta fusión, la página maestra actúa como la raíz de la jerarquía de controles fusionados. A continuación, esta jerarquía de control fusionada se agrega a la página para generar la jerarquía de controles finalizada (paso 3). El resultado neto es que la jerarquía de control de la página incluye la jerarquía de controles fusionados.

[![la página maestra y las jerarquías de control de la página de contenido se fusionan en conjunto durante la fase de PreInit](specifying-the-master-page-programmatically-vb/_static/image2.png)](specifying-the-master-page-programmatically-vb/_static/image1.png)

**Figura 01**: la página maestra y las jerarquías de control de la página de contenido se fusionan conjuntamente durante la fase de PreInit ([haga clic para ver la imagen de tamaño completo](specifying-the-master-page-programmatically-vb/_static/image3.png))

## <a name="step-2-setting-themasterpagefileproperty-from-code"></a>Paso 2: establecer la propiedad`MasterPageFile`desde el código

La partakes de la página maestra de esta fusión depende del valor de la propiedad `MasterPageFile` del objeto `Page`. Establecer el atributo `MasterPageFile` en la Directiva `@Page` tiene el efecto neto de asignar la propiedad `MasterPageFile` del `Page`durante la fase de inicialización, que es la primera fase del ciclo de vida de la página. También podemos establecer esta propiedad mediante programación. Sin embargo, es imperativo que esta propiedad se establezca antes de que tenga lugar la fusión de la figura 1.

Al principio de la fase de PreInit, el objeto `Page` genera su [evento`PreInit`](https://msdn.microsoft.com/library/system.web.ui.page.preinit.aspx) y llama a su [método`OnPreInit`](https://msdn.microsoft.com/library/system.web.ui.page.onpreinit.aspx). Para establecer la página maestra mediante programación, podemos crear un controlador de eventos para el evento `PreInit` o invalidar el método `OnPreInit`. Echemos un vistazo a ambos enfoques.

Comience abriendo `Default.aspx.vb`, el archivo de clase de código subyacente para la Página principal de nuestro sitio. Agregue un controlador de eventos para el evento de `PreInit` de la página escribiendo en el código siguiente:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample2.vb)]

Aquí se puede establecer la propiedad `MasterPageFile`. Actualice el código para que asigne el valor "~/site.Master" a la propiedad `MasterPageFile`.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample3.vb)]

Si establece un punto de interrupción y comienza con la depuración, verá que cada vez que se visita la página `Default.aspx` o cuando hay un postback en esta página, se ejecuta el controlador de eventos `Page_PreInit` y se asigna la propiedad `MasterPageFile` a "~/site.Master".

Como alternativa, puede invalidar el método `OnPreInit` de la clase `Page` y establecer la propiedad `MasterPageFile` aquí. En este ejemplo, vamos a establecer la página maestra en una página determinada, pero en lugar de `BasePage`. Recuerde que hemos creado una clase de página base personalizada (`BasePage`) en el tutorial [*especificar el título, las etiquetas meta y otros encabezados HTML en la página maestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-vb.md) . Actualmente `BasePage` invalida el método `OnLoadComplete` de la clase `Page`, donde establece la propiedad `Title` de la página en función de los datos del mapa del sitio. Vamos a actualizar `BasePage` para reemplazar también el método `OnPreInit` con el fin de especificar mediante programación la página maestra.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample4.vb)]

Dado que todas las páginas de contenido se derivan de `BasePage`, todas tienen ahora su página maestra asignada mediante programación. En este momento, el controlador de eventos `PreInit` de `Default.aspx.vb` es superfluo; no dude en quitarlo.

### <a name="what-about-thepagedirective"></a>¿Qué ocurre con la Directiva de`@Page`?

Lo que puede ser un poco confuso es que las propiedades de las páginas de contenido `MasterPageFile` se especifican ahora en dos lugares: mediante programación en el método `OnPreInit` de la clase `BasePage`, así como a través del atributo `MasterPageFile` en la Directiva `@Page` de cada página de contenido.

La primera fase del ciclo de vida de la página es la fase de inicialización. Durante esta fase, se asigna a la propiedad `MasterPageFile` del objeto `Page` el valor del atributo `MasterPageFile` en la Directiva de `@Page` (si se proporciona). La fase de preinicialización sigue a la fase de inicialización y aquí es donde se establece mediante programación la propiedad `MasterPageFile` del objeto `Page`, con lo que se sobrescribe el valor asignado de la Directiva `@Page`. Dado que estamos estableciendo la propiedad `MasterPageFile` del objeto `Page` mediante programación, podríamos quitar el atributo `MasterPageFile` de la Directiva `@Page` sin que ello afecte a la experiencia del usuario final. Para convencerse, continúe y quite el `MasterPageFile` atributo de la Directiva `@Page` en `Default.aspx` y, a continuación, visite la página a través de un explorador. Como cabría esperar, la salida es la misma que antes de que se quitara el atributo.

El hecho de que la propiedad `MasterPageFile` se establezca a través de la Directiva `@Page` o mediante programación es insignificante de la experiencia del usuario final. Sin embargo, Visual Studio usa el atributo `MasterPageFile` de la Directiva `@Page` durante el tiempo de diseño para generar la vista WYSIWYG en el diseñador. Si vuelve a `Default.aspx` en Visual Studio y navega hasta el diseñador, verá el mensaje "error de página maestra: la página tiene controles que requieren una referencia a una página maestra, pero no se especifica ninguno" (vea la figura 2).

En Resumen, debe dejar el atributo `MasterPageFile` en la Directiva `@Page` para disfrutar de una experiencia enriquecida en tiempo de diseño en Visual Studio.

[![Visual Studio usa el atributo MasterPageFile de la Directiva de @Page para presentar la vista de diseño](specifying-the-master-page-programmatically-vb/_static/image5.png)](specifying-the-master-page-programmatically-vb/_static/image4.png)

**Figura 02**: Visual Studio usa el atributo `MasterPageFile` de la directiva de `@Page` para representar la vista de diseño ([haga clic para ver la imagen de tamaño completo](specifying-the-master-page-programmatically-vb/_static/image6.png))

## <a name="step-3-creating-an-alternative-master-page"></a>Paso 3: crear una página maestra alternativa

Dado que la página maestra de una página de contenido se puede establecer mediante programación en tiempo de ejecución, es posible cargar dinámicamente una página maestra determinada basándose en algunos criterios externos. Esta funcionalidad puede ser útil en situaciones en las que el diseño del sitio debe variar en función del usuario. Por ejemplo, una aplicación web del motor de blog puede permitir a los usuarios elegir un diseño para su blog, donde cada diseño está asociado a una página maestra diferente. En tiempo de ejecución, cuando un visitante está viendo el blog de un usuario, la aplicación web necesitaría determinar el diseño del blog y asociar dinámicamente la página maestra correspondiente a la página de contenido.

Vamos a examinar cómo cargar dinámicamente una página maestra en tiempo de ejecución en función de algunos criterios externos. Nuestro sitio web contiene una sola página maestra (`Site.master`). Necesitamos otra página maestra para ilustrar la elección de una página maestra en tiempo de ejecución. Este paso se centra en la creación y configuración de la nueva página maestra. En el paso 4 se examina la determinación de la página maestra que se va a usar en tiempo de ejecución.

Cree una nueva página maestra en la carpeta raíz denominada `Alternate.master`. Agregue también una nueva hoja de estilos al sitio web denominado `AlternateStyles.css`.

[![agregar otra página maestra y un archivo CSS al sitio web](specifying-the-master-page-programmatically-vb/_static/image8.png)](specifying-the-master-page-programmatically-vb/_static/image7.png)

**Figura 03**: agregar otra página maestra y un archivo CSS al sitio web ([haga clic para ver la imagen de tamaño completo](specifying-the-master-page-programmatically-vb/_static/image9.png))

He diseñado la página maestra de `Alternate.master` para que el título se muestre en la parte superior de la página, centrado y en segundo plano. He dispensado de la columna izquierda y he colocado el contenido debajo del `MainContent` control ContentPlaceHolder, que ahora abarca todo el ancho de la página. Además, nixed la lista de lecciones sin ordenar y la reemplazamos por una lista horizontal arriba `MainContent`. También se actualizaron las fuentes y los colores usados por la página maestra (y, por extensión, sus páginas de contenido). La figura 4 muestra `Default.aspx` cuando se usa la página maestra de `Alternate.master`.

> [!NOTE]
> ASP.NET incluye la capacidad de definir *temas*. Un tema es una colección de imágenes, archivos CSS y valores de propiedad de control Web relacionados con el estilo que se pueden aplicar a una página en tiempo de ejecución. Los temas son la manera de ir si los diseños de su sitio difieren solo en las imágenes mostradas y según sus reglas de CSS. Si los diseños difieren de forma más sustancial, como usar controles Web diferentes o tener un diseño radicalmente diferente, tendrá que usar páginas maestras independientes. Consulte la sección lecturas adicionales al final de este tutorial para obtener más información sobre los temas.

[![nuestras páginas de contenido ahora pueden usar una nueva apariencia y funcionamiento](specifying-the-master-page-programmatically-vb/_static/image11.png)](specifying-the-master-page-programmatically-vb/_static/image10.png)

**Figura 04**: las páginas de contenido ahora pueden usar una nueva apariencia y funcionamiento ([haga clic para ver la imagen de tamaño completo](specifying-the-master-page-programmatically-vb/_static/image12.png))

Cuando se fusiona el marcado de las páginas maestra y de contenido, la clase `MasterPage` comprueba para asegurarse de que todos los controles de contenido de la página de contenido hagan referencia a un control ContentPlaceHolder en la página maestra. Se produce una excepción si se encuentra un control de contenido que hace referencia a un ContentPlaceHolder no existente. En otras palabras, es imperativo que la página maestra que se asigna a la página de contenido tenga un ContentPlaceHolder para cada control de contenido en la página de contenido.

La página maestra de `Site.master` incluye cuatro controles ContentPlaceHolder:

- `head`
- `MainContent`
- `QuickLoginUI`
- `LeftColumnContent`

Algunas de las páginas de contenido del sitio web incluyen solo uno o dos controles de contenido. otros incluyen un control de contenido para cada uno de los ContentPlaceHolders disponibles. Si la nueva página maestra (`Alternate.master`) se puede asignar alguna vez a las páginas de contenido que tienen controles de contenido para todos los ContentPlaceHolders en `Site.master`, es esencial que `Alternate.master` incluya también los mismos controles de ContentPlaceHolder que `Site.master`.

Para que la página maestra de `Alternate.master` tenga un aspecto similar al mío (vea la ilustración 4), empiece por definir los estilos de la página maestra en la hoja de estilos de `AlternateStyles.css`. Agregue las siguientes reglas en `AlternateStyles.css`:

[!code-css[Main](specifying-the-master-page-programmatically-vb/samples/sample5.css)]

A continuación, agregue el siguiente marcado declarativo a `Alternate.master`. Como puede ver, `Alternate.master` contiene cuatro controles ContentPlaceHolder con los mismos valores `ID` que los controles ContentPlaceHolder en `Site.master`. Además, incluye un control ScriptManager, que es necesario para las páginas del sitio web que usan el marco de ASP.NET AJAX.

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample6.aspx)]

### <a name="testing-the-new-master-page"></a>Probar la nueva página maestra

Para probar esta nueva página maestra, actualice el método `OnPreInit` de la clase `BasePage` para que se asigne a la propiedad `MasterPageFile` el valor `"~/Alternate.maser"` y, a continuación, visite el sitio Web. Cada página debe funcionar sin errores excepto en dos: `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx`. La adición de un producto a DetailsView en `~/Admin/AddProduct.aspx` da como resultado un `NullReferenceException` de la línea de código que intenta establecer la propiedad `GridMessageText` de la página maestra. Al visitar `~/Admin/Products.aspx` se inicia una `InvalidCastException` en la carga de la página con el mensaje: "no se puede convertir el objeto de tipo ' ASP. alterno\_Master ' al tipo ' ASP. site\_Master '".

Estos errores se producen porque el `Site.master` clase de código subyacente incluye eventos, propiedades y métodos públicos que no están definidos en `Alternate.master`. La parte de marcado de estas dos páginas tiene una directiva de `@MasterType` que hace referencia a la página maestra de `Site.master`.

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample7.aspx)]

Además, el controlador de eventos `ItemInserted` de DetailsView en `~/Admin/AddProduct.aspx` incluye código que convierte la propiedad `Page.Master` con tipo flexible en un objeto de tipo `Site`. La Directiva `@MasterType` (usada de esta manera) y la conversión en el controlador de eventos `ItemInserted` acoplan estrechamente las páginas `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` a la página maestra de `Site.master`.

Para romper este acoplamiento estrecho, podemos tener `Site.master` y `Alternate.master` derivan de una clase base común que contiene las definiciones de los miembros públicos. A continuación, se puede actualizar la Directiva de `@MasterType` para hacer referencia a este tipo base común.

### <a name="creating-a-custom-base-master-page-class"></a>Crear una clase de página maestra base personalizada

Agregue un nuevo archivo de clase a la carpeta `App_Code` denominada `BaseMasterPage.vb` y haga que se derive de `System.Web.UI.MasterPage`. Es necesario definir el método `RefreshRecentProductsGrid` y la propiedad `GridMessageText` en `BaseMasterPage`, pero no se pueden mover directamente desde `Site.master` porque estos miembros trabajan con controles Web específicos de la página maestra de `Site.master` (la `RecentProducts` la etiqueta GridView y `GridMessage`).

Lo que debemos hacer es configurar `BaseMasterPage` de forma que estos miembros se definan allí, pero se implementan en las clases derivadas de `BaseMasterPage`(`Site.master` y `Alternate.master`). Este tipo de herencia es posible marcando la clase como `MustInherit` y sus miembros como `MustOverride`. En Resumen, agregar estas palabras clave a la clase y sus dos miembros anuncian que `BaseMasterPage` no ha implementado `RefreshRecentProductsGrid` y `GridMessageText`, pero que son sus clases derivadas.

También necesitamos definir el evento `PricesDoubled` en `BaseMasterPage` y proporcionar un medio por las clases derivadas para generar el evento. El patrón utilizado en el .NET Framework para facilitar este comportamiento es crear un evento público en la clase base y agregar un método protegido y reemplazable denominado `OnEventName`. Después, las clases derivadas pueden llamar a este método para generar el evento o puede invalidarlo para ejecutar el código inmediatamente antes o después de que se produzca el evento.

Actualice la clase de `BaseMasterPage` para que contenga el código siguiente:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample8.vb)]

A continuación, vaya a la clase de código subyacente `Site.master` y haga que se derive de `BaseMasterPage`. Dado que `BaseMasterPage` contiene miembros marcados `MustOverride` necesitamos invalidar esos miembros aquí en `Site.master`. Agregue la palabra clave `Overrides` a las definiciones de método y propiedad. Actualice también el código que genera el evento `PricesDoubled` en el controlador de eventos `Click` del botón `DoublePrice` con una llamada al método `OnPricesDoubled` de la clase base.

Después de estas modificaciones, la clase de código subyacente `Site.master` debe contener el código siguiente:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample9.vb)]

También necesitamos actualizar la clase de código subyacente de `Alternate.master`para que se derive de `BaseMasterPage` e invalide los dos miembros de `MustOverride`. Pero como `Alternate.master` no contiene un control GridView que Enumere los productos más recientes ni una etiqueta que muestre un mensaje después de agregar un nuevo producto a la base de datos, estos métodos no tienen que hacer nada.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample10.vb)]

### <a name="referencing-the-base-master-page-class"></a>Referencia a la clase de página maestra base

Ahora que hemos completado la clase `BaseMasterPage` y que nuestras dos páginas maestras la extienden, nuestro último paso es actualizar las páginas `~/Admin/AddProduct.aspx` y `~/Admin/Products.aspx` para hacer referencia a este tipo común. Empiece por cambiar la Directiva de `@MasterType` en ambas páginas de:

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample11.aspx)]

A:

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample12.aspx)]

En lugar de hacer referencia a una ruta de acceso de archivo, la propiedad `@MasterType` ahora hace referencia al tipo base (`BaseMasterPage`). Por consiguiente, la propiedad fuertemente tipada `Master` utilizada en las clases de código subyacente de ambas páginas es ahora de tipo `BaseMasterPage` (en lugar de tipo `Site`). Con este cambio en el lugar de la revisita `~/Admin/Products.aspx`. Anteriormente, esto producía un error de conversión porque la página se configuró para usar la página maestra de `Alternate.master`, pero la Directiva de `@MasterType` hacía referencia al archivo `Site.master`. Pero ahora la página se representa sin errores. Esto se debe a que la página maestra de `Alternate.master` se puede convertir en un objeto de tipo `BaseMasterPage` (ya que lo extiende).

Hay un pequeño cambio que se debe realizar en `~/Admin/AddProduct.aspx`. El controlador de eventos `ItemInserted` del control DetailsView utiliza la propiedad fuertemente tipada `Master` y la propiedad `Page.Master` de tipo flexible. Hemos corregido la referencia fuertemente tipada cuando se actualizó la Directiva de `@MasterType`, pero todavía necesitamos actualizar la referencia de tipo flexible. Reemplace la siguiente línea de código:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample13.vb)]

Con lo siguiente, que convierte `Page.Master` al tipo base:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample14.vb)]

## <a name="step-4-determining-what-master-page-to-bind-to-the-content-pages"></a>Paso 4: determinar qué página maestra se va a enlazar a las páginas de contenido

En la clase `BasePage` se establecen actualmente todas las propiedades de `MasterPageFile` de las páginas de contenido en un valor codificado de forma rígida en la fase de preinicialización del ciclo de vida de la página. Podemos actualizar este código para basar la página maestra en un factor externo. Quizás la página maestra que se va a cargar depende de las preferencias del usuario que ha iniciado sesión actualmente. En ese caso, es necesario escribir código en el método `OnPreInit` en `BasePage` que busca las preferencias de la página maestra del usuario que se visita actualmente.

Vamos a crear una página web que permite al usuario elegir la página maestra que se va a usar: `Site.master` o `Alternate.master` y guardar esta opción en una variable de sesión. Empiece por crear una nueva página web en el directorio raíz denominado `ChooseMasterPage.aspx`. Al crear esta página (o cualquier otra página de contenido en adelante), no es necesario enlazarla a una página maestra porque la página maestra se establece mediante programación en `BasePage`. Sin embargo, si no enlaza la nueva página a una página maestra, el marcado declarativo predeterminado de la nueva página contiene un formulario web y otro contenido proporcionado por la página maestra. Deberá reemplazar manualmente este marcado con los controles de contenido adecuados. Por ese motivo, me resulta más fácil enlazar la nueva página de ASP.NET a una página maestra.

> [!NOTE]
> Dado que `Site.master` y `Alternate.master` tienen el mismo conjunto de controles de ContentPlaceHolder, no importa qué página maestra elija al crear la nueva página de contenido. Por coherencia, recomiendo usar `Site.master`.

[![agregar una nueva página de contenido al sitio web](specifying-the-master-page-programmatically-vb/_static/image14.png)](specifying-the-master-page-programmatically-vb/_static/image13.png)

**Figura 05**: agregar una nueva página de contenido al sitio web ([haga clic para ver la imagen de tamaño completo](specifying-the-master-page-programmatically-vb/_static/image15.png))

Actualice el archivo de `Web.sitemap` para incluir una entrada para esta lección. Agregue el siguiente marcado debajo del `<siteMapNode>` para la lección páginas maestras y ASP.NET AJAX:

[!code-xml[Main](specifying-the-master-page-programmatically-vb/samples/sample15.xml)]

Antes de agregar contenido a la página de `ChooseMasterPage.aspx`, dedique un momento a actualizar la clase de código subyacente de la página para que se derive de `BasePage` (en lugar de `System.Web.UI.Page`). Después, agregue un control DropDownList a la página, establezca su propiedad `ID` en `MasterPageChoice`y agregue dos ListItems con los valores `Text` de "~/site.Master" y "~/Alternate.Master".

Agregue un control Web de botón a la página y establezca sus propiedades `ID` y `Text` en `SaveLayout` y "guardar opción de diseño", respectivamente. En este momento, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](specifying-the-master-page-programmatically-vb/samples/sample16.aspx)]

Cuando se visita la página por primera vez, es necesario mostrar la opción de la página maestra seleccionada actualmente por el usuario. Cree un controlador de eventos `Page_Load` y agregue el código siguiente:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample17.vb)]

El código anterior solo se ejecuta en la primera visita de la página (y no en los postbacks posteriores). En primer lugar, comprueba para ver si existe la variable de sesión `MyMasterPage`. Si lo hace, intenta encontrar el ListItem correspondiente en el `MasterPageChoice` DropDownList. Si se encuentra un ListItem coincidente, su propiedad `Selected` se establece en `True`.

También necesitamos código que guarde la elección del usuario en la variable de sesión `MyMasterPage`. Cree un controlador de eventos para el evento de `Click` del botón `SaveLayout` y agregue el código siguiente:

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample18.vb)]

> [!NOTE]
> En el momento en que se ejecuta el controlador de eventos `Click` en el postback, ya se ha seleccionado la página maestra. Por lo tanto, la selección de la lista desplegable del usuario no estará en vigor hasta que visite la siguiente página. El `Response.Redirect` obliga al explorador a volver a solicitar `ChooseMasterPage.aspx`.

Una vez completada la página `ChooseMasterPage.aspx`, nuestra tarea final es tener `BasePage` asignar la propiedad `MasterPageFile` basada en el valor de la variable de sesión `MyMasterPage`. Si la variable de sesión no está establecida, `BasePage` establecer el valor predeterminado en `Site.master`.

[!code-vb[Main](specifying-the-master-page-programmatically-vb/samples/sample19.vb)]

> [!NOTE]
> He cambiado el código que asigna la propiedad `MasterPageFile` del objeto `Page` del controlador de eventos `OnPreInit` y en dos métodos independientes. Este primer método, `SetMasterPageFile`, asigna la propiedad `MasterPageFile` al valor devuelto por el segundo método, `GetMasterPageFileFromSession`. He marcado el método `SetMasterPageFile` `Overridable` para que las clases futuras que amplían `BasePage` puedan reemplazarlo opcionalmente para implementar la lógica personalizada, si es necesario. Veremos un ejemplo de cómo invalidar la propiedad `SetMasterPageFile` de `BasePage`en el siguiente tutorial.

Con este código en su lugar, visite la página `ChooseMasterPage.aspx`. Inicialmente, se selecciona la página maestra de `Site.master` (vea la figura 6), pero el usuario puede elegir una página maestra diferente de la lista desplegable.

[![las páginas de contenido se muestran mediante la página maestra site. Master](specifying-the-master-page-programmatically-vb/_static/image17.png)](specifying-the-master-page-programmatically-vb/_static/image16.png)

**Figura 06**: las páginas de contenido se muestran en la página maestra de `Site.master` ([haga clic para ver la imagen a tamaño completo](specifying-the-master-page-programmatically-vb/_static/image18.png))

[![páginas de contenido se muestran ahora mediante la página maestra alternativa.](specifying-the-master-page-programmatically-vb/_static/image20.png)](specifying-the-master-page-programmatically-vb/_static/image19.png)

**Figura 07**: las páginas de contenido se muestran ahora en la página maestra de `Alternate.master` ([haga clic para ver la imagen a tamaño completo](specifying-the-master-page-programmatically-vb/_static/image21.png))

## <a name="summary"></a>Resumen

Cuando se visita una página de contenido, sus controles de contenido se fusionan con los controles ContentPlaceHolder de la página maestra. La página maestra de la página de contenido se indica mediante la propiedad `MasterPageFile` de la clase `Page`, que se asigna al atributo `MasterPageFile` de la Directiva `@Page` durante la fase de inicialización. Como se ha mostrado en este tutorial, podemos asignar un valor a la propiedad `MasterPageFile`, siempre y cuando lo hagamos antes del final de la fase de PreInit. La posibilidad de especificar mediante programación la página maestra abre la puerta para escenarios más avanzados, como enlazar dinámicamente una página de contenido a una página maestra basada en factores externos.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Diagrama del ciclo de vida de la página ASP.NET](http://emanish.googlepages.com/Asp.Net2.0Lifecycle.PNG)
- [Información general del ciclo de vida de la página ASP.NET](https://msdn.microsoft.com/library/ms178472.aspx)
- [Información general sobre temas y máscaras de ASP.NET](https://msdn.microsoft.com/library/ykzx33wh.aspx)
- [Páginas maestras: sugerencias, trucos y capturas](http://www.odetocode.com/articles/450.aspx)
- [Temas de ASP.NET](http://www.odetocode.com/articles/423.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. El revisor responsable de este tutorial fue Banerjee. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](master-pages-and-asp-net-ajax-vb.md)
> [Siguiente](nested-master-pages-vb.md)
