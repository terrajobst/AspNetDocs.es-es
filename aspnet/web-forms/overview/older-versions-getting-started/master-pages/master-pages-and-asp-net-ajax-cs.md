---
uid: web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
title: Páginas maestras y ASP.NET AJAXC#() | Microsoft Docs
author: rick-anderson
description: Describe las opciones para usar ASP.NET AJAX y páginas maestras. Examina el uso de la clase ScriptManagerProxy; describe la forma en que se cargan los distintos archivos JS dependen de...
ms.author: riande
ms.date: 07/11/2008
ms.assetid: 0c55eb66-ba44-4d49-98e8-5c87fd9b1111
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/master-pages-and-asp-net-ajax-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cd1d57b4d2aa01654da53ab2b1cc01f71ad8a87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78510571"
---
# <a name="master-pages-and-aspnet-ajax-c"></a>Páginas maestras y ASP.NET AJAX (C#)

por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](https://download.microsoft.com/download/1/8/4/184e24fa-fcc8-47fa-ac99-4b6a52d41e97/ASPNET_MasterPages_Tutorial_08_CS.zip) o [Descargar PDF](https://download.microsoft.com/download/e/b/4/eb4abb10-c416-4ba4-9899-32577715b1bd/ASPNET_MasterPages_Tutorial_08_CS.pdf)

> Describe las opciones para usar ASP.NET AJAX y páginas maestras. Examina el uso de la clase ScriptManagerProxy; describe cómo se cargan los distintos archivos JS dependiendo de si se usa el ScriptManager en la página maestra o en la página de contenido.

## <a name="introduction"></a>Introducción

En los últimos años, más y más desarrolladores han creado aplicaciones web habilitadas para [Ajax](http://en.wikipedia.org/wiki/Ajax_(programming)). Un sitio web habilitado para AJAX usa varias tecnologías web relacionadas para ofrecer una experiencia de usuario más dinámica. Crear aplicaciones de ASP.NET con AJAX habilitado es sorprendentemente fácil gracias al [marco de ASP.NET AJAX](../../../../ajax/index.md)de Microsoft. ASP.NET AJAX está integrado en ASP.NET 3,5 y Visual Studio 2008; también está disponible como descarga independiente para las aplicaciones de ASP.NET 2,0.

Al compilar páginas web habilitadas para AJAX con el marco de AJAX de ASP.NET, debe agregar exactamente un [control ScriptManager](https://msdn.microsoft.com/library/bb398863.aspx) a cada una de las páginas que usan el marco de trabajo. Como su nombre implica, el ScriptManager administra el script del lado cliente que se usa en las páginas web habilitadas para AJAX. Como mínimo, el ScriptManager emite HTML que indica al explorador que descargue los archivos JavaScript que estructuran la biblioteca de cliente de ASP.NET AJAX. También se puede usar para registrar archivos JavaScript personalizados, servicios web habilitados para scripts y funcionalidad de servicio de aplicación personalizada.

Si el sitio usa páginas maestras (como debería), no es necesario agregar un control ScriptManager a cada página de contenido único. en su lugar, puede Agregar un control ScriptManager a la página maestra. En este tutorial se muestra cómo agregar el control ScriptManager a la página maestra. También se examina cómo usar el control ScriptManagerProxy para registrar scripts personalizados y servicios de script en una página de contenido específica.

> [!NOTE]
> En este tutorial no se explora el diseño ni la creación de aplicaciones web habilitadas para AJAX con el marco de ASP.NET AJAX. Para obtener más información sobre el uso de AJAX, consulte los vídeos y [tutoriales](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)de [AJAX de ASP.net](../../../videos/aspnet-ajax/index.md) , así como los recursos que aparecen en la sección de lecturas adicionales al final de este tutorial.

## <a name="examining-the-markup-emitted-by-the-scriptmanager-control"></a>Examinar el marcado emitido por el control ScriptManager

El control ScriptManager emite marcado que indica al explorador que descargue los archivos JavaScript que estructuran la biblioteca de cliente de ASP.NET AJAX. También agrega un bit de JavaScript en línea a la página que inicializa esta biblioteca. El marcado siguiente muestra el contenido que se agrega a la salida representada de una página que incluye un control ScriptManager:

[!code-html[Main](master-pages-and-asp-net-ajax-cs/samples/sample1.html)]

Las etiquetas de `<script src="url"></script>` indican al explorador que descargue y ejecute el archivo JavaScript en la *dirección URL*. El ScriptManager emite tres etiquetas de este tipo: una hace referencia al archivo `WebResource.axd`, mientras que los otros dos hacen referencia al archivo `ScriptResource.axd`. En realidad, estos archivos no existen como archivos en el sitio Web. En su lugar, cuando una solicitud de uno de estos archivos llega al servidor Web, el motor ASP.NET examina la cadena de consulta y devuelve el contenido de JavaScript adecuado. El script proporcionado por estos tres archivos JavaScript externos constituye la biblioteca de cliente del marco de AJAX de ASP.NET. El resto de etiquetas `<script>` emitidas por ScriptManager incluyen el script en línea que inicializa esta biblioteca.

Las referencias de scripts externos y el script en línea emitidos por ScriptManager son esenciales para una página que usa el marco de AJAX de ASP.NET, pero no es necesaria para las páginas que no usan el marco de trabajo. Por lo tanto, puede que le resulte ideal que solo agregue un ScriptManager a las páginas que usan el marco de AJAX de ASP.NET. Y esto es suficiente, pero si tiene muchas páginas que usan el marco, terminará agregando el control ScriptManager a todas las páginas, una tarea repetitiva, para decir el menor. Como alternativa, puede Agregar un ScriptManager a la página maestra, que luego inserta este script necesario en todas las páginas de contenido. Con este enfoque, no es necesario recordar agregar un ScriptManager a una nueva página que use el marco de AJAX de ASP.NET porque ya está incluido en la página maestra. En el paso 1 se explica cómo agregar un ScriptManager a la página maestra.

> [!NOTE]
> Si piensa incluir la funcionalidad de AJAX en la interfaz de usuario de la página maestra, no tiene ninguna opción en la materia; debe incluir el ScriptManager en la página maestra.

Una desventaja de agregar el ScriptManager a la página maestra es que el script anterior se emite en *todas* las páginas, independientemente de si es necesario. Esto conduce claramente al ancho de banda desperdiciado para las páginas que tienen el ScriptManager incluido (a través de la página maestra), pero no usan las características del marco de ASP.NET AJAX. Pero ¿cuánto ancho de banda se desperdicia?

- El contenido real emitido por el ScriptManager (mostrado arriba) totaliza un poco más de 1 KB.
- Los tres archivos de script externos a los que hace referencia el elemento `<script>`, sin embargo, se componen aproximadamente 450 KB será de datos sin comprimir. en un sitio web que usa la compresión gzip, este ancho de banda total puede reducirse cerca de 100 KB. Sin embargo, estos archivos de script los almacena el explorador durante un año, lo que significa que solo deben descargarse una vez y después se pueden volver a usar en otras páginas del sitio.

En el mejor de los casos, cuando se almacenan en caché los archivos de script, el costo total es 1 KB, que es insignificante. En el peor de los casos, sin embargo, es cuando todavía no se han descargado los archivos de script y el servidor Web no está usando ninguna forma de compresión (el acierto de ancho de banda está en torno a 450 KB será, que puede Agregar en cualquier parte desde una segunda o dos a través de una conexión de banda ancha hasta un minuto para  usuarios a través de módems de acceso telefónico. La buena noticia es que, dado que los archivos de script externos se almacenan en caché por el explorador, este peor escenario se produce con poca frecuencia.

> [!NOTE]
> Si todavía se siente incómodo colocando el control ScriptManager en la página maestra, tenga en cuenta el formulario web (el `<form runat="server">` marcado en la página maestra). Cada página de ASP.NET que usa el modelo de postback debe incluir precisamente un formulario Web Forms. Agregar un formulario Web Forms agrega contenido adicional: un número de campos de formulario ocultos, el `<form>` etiqueta en sí y, si es necesario, una función de JavaScript para iniciar un postback desde el script. Este marcado no es necesario para las páginas que no son de postback. Este marcado superfluo podría eliminarse quitando el formulario web de la página maestra y agregándolo manualmente a cada página de contenido que necesite una. Sin embargo, las ventajas de tener el formulario web en la página maestra compensan las desventajas de tener que agregarse innecesariamente a determinadas páginas de contenido.

## <a name="step-1-adding-a-scriptmanager-control-to-the-master-page"></a>Paso 1: agregar un control ScriptManager a la página maestra

Cada página web que usa el marco de AJAX de ASP.NET debe contener exactamente un control ScriptManager. Debido a este requisito, normalmente tiene sentido colocar un único control ScriptManager en la página maestra para que todas las páginas de contenido tengan el control ScriptManager incluido automáticamente. Además, el ScriptManager debe aparecer antes que cualquiera de los controles de servidor de ASP.NET AJAX, como los controles UpdatePanel y UpdateProgress. Por lo tanto, es mejor colocar el ScriptManager antes de cualquier control de ContentPlaceHolder en el formulario Web.

Abra la página maestra de `Site.master` y agregue un control ScriptManager a la página en el formulario web, pero antes del elemento `<div id="topContent">` (vea la ilustración 1). Si usa Visual Web Developer 2008 o Visual Studio 2008, el control ScriptManager se encuentra en el cuadro de herramientas en la pestaña extensiones AJAX. Si usa Visual Studio 2005, deberá instalar primero el marco de AJAX de ASP.NET y agregar los controles al cuadro de herramientas. Visite el [wiki de ASP.NET AJAX](https://github.com/DevExpress/AjaxControlToolkit/wiki) para obtener el marco de ASP.net 2,0.

Después de agregar el ScriptManager a la página, cambie su `ID` de `ScriptManager1` a `MyManager`.

[![agregar el ScriptManager a la página maestra](master-pages-and-asp-net-ajax-cs/_static/image2.png)](master-pages-and-asp-net-ajax-cs/_static/image1.png)

**Figura 01**: agregar el ScriptManager a la página maestra ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image3.png))

## <a name="step-2-using-the-aspnet-ajax-framework-from-a-content-page"></a>Paso 2: usar el marco de AJAX de ASP.NET desde una página de contenido

Con el control ScriptManager agregado a la página maestra, ahora podemos agregar la funcionalidad de ASP.NET AJAX Framework a cualquier página de contenido. Vamos a crear una nueva página de ASP.NET que muestra un producto seleccionado aleatoriamente en la base de datos Northwind. Usaremos el control Timer de ASP.NET AJAX Framework para actualizar esta pantalla cada 15 segundos y mostrar un nuevo producto.

Empiece por crear una nueva página en el directorio raíz denominado `ShowRandomProduct.aspx`. No olvide enlazar esta nueva página a la página maestra de `Site.master`.

[![agregar una nueva página de ASP.NET al sitio web](master-pages-and-asp-net-ajax-cs/_static/image5.png)](master-pages-and-asp-net-ajax-cs/_static/image4.png)

**Figura 02**: agregar una nueva página de ASP.net al sitio web ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image6.png))

Recuerde que en el tutorial para [*especificar el título, las etiquetas meta y otros encabezados HTML en la página maestra*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) se ha creado una clase de página base personalizada denominada `BasePage` que ha generado el título de la página si no se ha establecido explícitamente. Vaya a la clase de código subyacente de la página de `ShowRandomProduct.aspx` y haga que se derive de `BasePage` (en lugar de `System.Web.UI.Page`).

Por último, actualice el archivo de `Web.sitemap` para incluir una entrada para esta lección. Agregue el siguiente marcado debajo del `<siteMapNode>` de la lección de interacción de la página maestra a contenido:

[!code-xml[Main](master-pages-and-asp-net-ajax-cs/samples/sample2.xml)]

La adición de este elemento `<siteMapNode>` se refleja en la lista de lecciones (vea la figura 5).

### <a name="displaying-a-randomly-selected-product"></a>Mostrar un producto seleccionado aleatoriamente

Vuelva a `ShowRandomProduct.aspx`. En el diseñador, arrastre un control UpdatePanel desde el cuadro de herramientas hasta el `MainContent` control de contenido y establezca su propiedad `ID` en `ProductPanel`. UpdatePanel representa una región de la pantalla que se puede actualizar de forma asincrónica a través de un postback de página parcial.

La primera tarea es Mostrar información sobre un producto seleccionado aleatoriamente en el UpdatePanel. Empiece arrastrando un control DetailsView a UpdatePanel. Establezca la propiedad `ID` del control DetailsView en `ProductInfo` y borre sus propiedades `Height` y `Width`. Expanda la etiqueta inteligente de DetailsView y, en la lista desplegable elegir origen de datos, elija enlazar DetailsView a un nuevo control SqlDataSource denominado `RandomProductDataSource`.

[![enlazar DetailsView a un nuevo control SqlDataSource](master-pages-and-asp-net-ajax-cs/_static/image8.png)](master-pages-and-asp-net-ajax-cs/_static/image7.png)

**Figura 03**: enlace de DetailsView a un nuevo control SqlDataSource ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image9.png))

Configure el control SqlDataSource para que se conecte a la base de datos Northwind a través del `NorthwindConnectionString` (que hemos creado en [*interactuar con la página maestra desde el tutorial de la página de contenido*](interacting-with-the-content-page-from-the-master-page-cs.md) ). Al configurar la instrucción SELECT, elija especificar una instrucción SQL personalizada y, a continuación, escriba la siguiente consulta:

[!code-sql[Main](master-pages-and-asp-net-ajax-cs/samples/sample3.sql)]

La palabra clave `TOP 1` de la cláusula `SELECT` devuelve solo el primer registro devuelto por la consulta. La [función`NEWID()`](https://msdn.microsoft.com/library/ms190348.aspx) genera un nuevo [valor de identificador único global (GUID)](http://en.wikipedia.org/wiki/Globally_Unique_Identifier) y se puede usar en una cláusula `ORDER BY` para devolver los registros de la tabla en un orden aleatorio.

[![configurar SqlDataSource para que devuelva un único registro seleccionado aleatoriamente](master-pages-and-asp-net-ajax-cs/_static/image11.png)](master-pages-and-asp-net-ajax-cs/_static/image10.png)

**Figura 04**: configure el SqlDataSource para que devuelva un único registro seleccionado aleatoriamente ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image12.png))

Después de completar el asistente, Visual Studio crea un BoundField para las dos columnas devueltas por la consulta anterior. En este momento, el marcado declarativo de la página debe ser similar al siguiente:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample4.aspx)]

En la figura 5 se muestra la página `ShowRandomProduct.aspx` cuando se ve a través de un explorador. Haga clic en el botón actualizar del explorador para volver a cargar la página. debería ver los valores `ProductName` y `UnitPrice` de un nuevo registro seleccionado aleatoriamente.

[![se muestra el nombre y el precio de un producto aleatorio](master-pages-and-asp-net-ajax-cs/_static/image14.png)](master-pages-and-asp-net-ajax-cs/_static/image13.png)

**Figura 05**: se muestra el nombre y el precio de un producto aleatorio ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image15.png))

### <a name="automatically-displaying-a-new-product-every-15-seconds"></a>Visualización automática de un nuevo producto cada 15 segundos

El marco de AJAX de ASP.NET incluye un control de temporizador que realiza un postback en un momento especificado. en el postback, se genera el evento `Tick` del temporizador. Si el control Timer se coloca dentro de un UpdatePanel, se desencadena un postback de página parcial, durante el cual se pueden volver a enlazar los datos a DetailsView para mostrar un nuevo producto seleccionado aleatoriamente.

Para ello, arrastre un temporizador desde el cuadro de herramientas y colóquelo en el UpdatePanel. Cambie el `ID` del temporizador de `Timer1` a `ProductTimer` y su propiedad `Interval` de 60000 a 15000. La propiedad `Interval` indica el número de milisegundos entre los postbacks; Si se establece en 15000, el temporizador desencadena un postback de página parcial cada 15 segundos. En este momento el marcado declarativo del temporizador debe ser similar al siguiente:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample5.aspx)]

Cree un controlador de eventos para el evento `Tick` del temporizador. En este controlador de eventos, es necesario volver a enlazar los datos a DetailsView llamando al método `DataBind` de DetailsView. Al hacerlo, se indica a DetailsView que vuelva a recuperar los datos de su control de origen de datos, que seleccionará y mostrará un nuevo registro seleccionado aleatoriamente (al igual que al volver a cargar la página haciendo clic en el botón actualizar del explorador).

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample6.cs)]

Así de simple. Vuelva a visitar la página a través de un explorador. Inicialmente, se muestra la información de un producto aleatorio. Si ve la pantalla de forma paciente, observará que, después de 15 segundos, la información sobre un nuevo producto reemplaza mágicamente a la pantalla existente.

Para ver mejor lo que sucede aquí, vamos a agregar un control etiqueta al UpdatePanel que muestra la hora en que se actualizó por última vez la pantalla. Agregue un control Web de etiqueta dentro de UpdatePanel, establezca su `ID` en `LastUpdateTime`y borre su propiedad `Text`. A continuación, cree un controlador de eventos para el evento `Load` de UpdatePanel y muestre la hora actual en la etiqueta. (El evento `Load` de UpdatePanel se desencadena en cada postback de página completa o parcial).

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample7.cs)]

Con este cambio completado, la página incluye la hora en que se cargó el producto que se muestra actualmente. En la ilustración 6 se muestra la página la primera vez que se visita. En la figura 7 se muestra la página 15 segundos más tarde después de que el control Timer se haya "marcado" y se haya actualizado UpdatePanel para mostrar información sobre un nuevo producto.

[![se muestra un producto seleccionado aleatoriamente en la carga de página](master-pages-and-asp-net-ajax-cs/_static/image17.png)](master-pages-and-asp-net-ajax-cs/_static/image16.png)

**Figura 06**: se muestra un producto seleccionado aleatoriamente en la carga de página ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image18.png))

[![cada 15 segundos se muestra un nuevo producto seleccionado aleatoriamente](master-pages-and-asp-net-ajax-cs/_static/image20.png)](master-pages-and-asp-net-ajax-cs/_static/image19.png)

**Figura 07**: cada 15 segundos se muestra un nuevo producto seleccionado aleatoriamente ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image21.png))

## <a name="step-3-using-the-scriptmanagerproxy-control"></a>Paso 3: usar el control ScriptManagerProxy

Además de incluir el script necesario para la biblioteca de cliente de ASP.NET AJAX Framework, el ScriptManager también puede registrar archivos JavaScript personalizados, referencias a servicios web habilitados para scripts y servicios de autenticación, autorización y perfiles personalizados. Normalmente, dichas personalizaciones son específicas de una página determinada. Sin embargo, si se hace referencia a los archivos de scripts personalizados, a las referencias de servicio web o a los servicios de autenticación, autorización o de perfil en el ScriptManager en la página maestra, se incluirán en *todas* las páginas del sitio Web.

Para agregar personalizaciones relacionadas con ScriptManager por página, use el control ScriptManagerProxy. Puede Agregar un ScriptManagerProxy a una página de contenido y, a continuación, registrar el archivo JavaScript personalizado, la referencia de servicio web o el servicio de autenticación, autorización o Perfil de la ScriptManagerProxy; Esto tiene el efecto de registrar estos servicios para la página de contenido en particular.

> [!NOTE]
> Una página ASP.NET no puede tener más de un control ScriptManager presente. Por lo tanto, no se puede Agregar un control ScriptManager a una página de contenido si el control ScriptManager ya está definido en la página maestra. El único propósito de la ScriptManagerProxy es proporcionar a los desarrolladores una forma de definir el ScriptManager en la página maestra, pero seguir teniendo la capacidad de agregar personalizaciones de ScriptManager en cada página.

Para ver el control ScriptManagerProxy en acción, vamos a aumentar el UpdatePanel en `ShowRandomProduct.aspx` para incluir un botón que use un script del lado cliente para pausar o reanudar el control Timer. El control Timer tiene tres métodos de cliente que se pueden usar para lograr esta funcionalidad deseada:

- `_startTimer()`: inicia el control Timer
- `_raiseTick()`: hace que el control de temporizador "tick", por tanto, se devuelve y se genera su evento `Tick` en el servidor.
- `_stopTimer()`: detiene el control Timer

Vamos a crear un archivo de JavaScript con una variable denominada `timerEnabled` y una función llamada `ToggleTimer`. La variable `timerEnabled` indica si el control de temporizador está habilitado o deshabilitado actualmente. su valor predeterminado es true. La función `ToggleTimer` acepta dos parámetros de entrada: una referencia al botón pausar/reanudar y el valor de `id` del lado cliente del control Timer. Esta función alterna el valor de `timerEnabled`, obtiene una referencia al control Timer, inicia o detiene el temporizador (dependiendo del valor de `timerEnabled`) y actualiza el texto que se muestra en el botón a "PAUSE" o "resume". Se llamará a esta función cada vez que se haga clic en el botón pausar o reanudar.

Empiece por crear una nueva carpeta en el sitio web denominado `Scripts`. Después, agregue un nuevo archivo a la carpeta scripts denominada `TimerScript.js` del tipo archivo JScript.

[![agregar un nuevo archivo JavaScript a la carpeta scripts](master-pages-and-asp-net-ajax-cs/_static/image23.png)](master-pages-and-asp-net-ajax-cs/_static/image22.png)

**Figura 08**: agregar un nuevo archivo JavaScript a la carpeta `Scripts` ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image24.png))

[![se ha agregado un nuevo archivo JavaScript al sitio web](master-pages-and-asp-net-ajax-cs/_static/image26.png)](master-pages-and-asp-net-ajax-cs/_static/image25.png)

**Figura 09**: se ha agregado un nuevo archivo JavaScript al sitio web ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image27.png))

Después, agregue el siguiente scripts al archivo TimerScript. js:

[!code-csharp[Main](master-pages-and-asp-net-ajax-cs/samples/sample8.cs)]

Ahora tenemos que registrar este archivo JavaScript personalizado en `ShowRandomProduct.aspx`. Vuelva a `ShowRandomProduct.aspx` y agregue un control ScriptManagerProxy a la página; establezca su `ID` en `MyManagerProxy`. Para registrar un archivo JavaScript personalizado, seleccione el control ScriptManagerProxy en el diseñador y, a continuación, vaya a la ventana Propiedades. Una de las propiedades es titulada scripts. Al seleccionar esta propiedad, se muestra el editor de la colección ScriptReference que se muestra en la figura 10. Haga clic en el botón Agregar para incluir una nueva referencia de script y, a continuación, escriba la ruta de acceso al archivo de script en la propiedad ruta de acceso: `~/Scripts/TimerScript.js`.

[![agregar una referencia de script al control ScriptManagerProxy](master-pages-and-asp-net-ajax-cs/_static/image29.png)](master-pages-and-asp-net-ajax-cs/_static/image28.png)

**Figura 10**: agregar una referencia de script al control ScriptManagerProxy ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image30.png))

Después de agregar la referencia de script, el marcado declarativo del control ScriptManagerProxy se actualiza para incluir una colección `<Scripts>` con una sola entrada `ScriptReference`, como se muestra en el siguiente fragmento de código de marcado:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample9.aspx)]

La entrada `ScriptReference` indica a ScriptManagerProxy que incluya una referencia al archivo JavaScript en su marcado representado. Es decir, al registrar el script personalizado en el ScriptManagerProxy, la salida representada de la página `ShowRandomProduct.aspx` ahora incluye otra etiqueta de `<script src="url"></script>`: `<script src="Scripts/TimerScript.js" type="text/javascript"></script>`.

Ahora se puede llamar a la función `ToggleTimer` definida en `TimerScript.js` desde el script de cliente en la página `ShowRandomProduct.aspx`. Agregue el siguiente código HTML dentro de UpdatePanel:

[!code-aspx[Main](master-pages-and-asp-net-ajax-cs/samples/sample10.aspx)]

Esto muestra un botón con el texto "PAUSE". Siempre que se hace clic en él, se llama a la función de JavaScript `ToggleTimer`, pasando una referencia al botón y el valor de identificador del control Timer (`ProductTimer`). Tenga en cuenta la sintaxis para obtener el valor `id` del control Timer. `<%=ProductTimer.ClientID%>` emite el valor de la propiedad `ClientID` del control `ProductTimer` Timer. En el tutorial [*nombre de identificador de control en las páginas de contenido*](control-id-naming-in-content-pages-cs.md) se describen las diferencias entre el valor de `ID` del servidor y el valor de `id` del lado cliente resultante, y cómo `ClientID` devuelve el `id`del lado cliente.

En la figura 11 se muestra esta página cuando se visita por primera vez a través de un explorador. El temporizador se está ejecutando actualmente y actualiza la información de producto mostrada cada 15 segundos. En la ilustración 12 se muestra la pantalla después de hacer clic en el botón pausar. Al hacer clic en el botón pausar se detiene el temporizador y se actualiza el texto del botón a "resume". La información del producto se actualizará (y continuará con la actualización cada 15 segundos) cuando el usuario haga clic en reanudar.

[![haga clic en el botón pausar para detener el control Timer](master-pages-and-asp-net-ajax-cs/_static/image32.png)](master-pages-and-asp-net-ajax-cs/_static/image31.png)

**Figura 11**: haga clic en el botón pausar para detener el control Timer ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image33.png))

[![haga clic en el botón Reanudar para reiniciar el temporizador](master-pages-and-asp-net-ajax-cs/_static/image35.png)](master-pages-and-asp-net-ajax-cs/_static/image34.png)

**Figura 12**: haga clic en el botón Reanudar para reiniciar el temporizador ([haga clic para ver la imagen de tamaño completo](master-pages-and-asp-net-ajax-cs/_static/image36.png))

## <a name="summary"></a>Resumen

Al compilar aplicaciones web habilitadas para AJAX mediante el marco de AJAX de ASP.NET, es imperativo que cada página web habilitada para AJAX incluya un control ScriptManager. Para facilitar este proceso, podemos agregar un ScriptManager a la página maestra en lugar de tener que recordar agregar un ScriptManager a cada página de contenido. En el paso 1 se mostró cómo agregar el ScriptManager a la página maestra mientras el paso 2 examinaba la implementación de la funcionalidad de AJAX en una página de contenido.

Si necesita agregar scripts personalizados, referencias a servicios web habilitados para scripts o servicios de autenticación, autorización o perfiles personalizados en una página de contenido en particular, agregue un control ScriptManagerProxy a la página de contenido y, a continuación, configure el personalizaciones allí. En el paso 3 se examinó cómo usar el ScriptManagerProxy para registrar un archivo JavaScript personalizado en una página de contenido específica.

¡ Feliz programación!

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas descritos en este tutorial, consulte los siguientes recursos:

- [Marco de AJAX de ASP.NET](../../../../ajax/index.md)
- [Tutoriales de AJAX de ASP.NET](../aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax.md)
- [Vídeos de ASP.NET AJAX](../../../videos/aspnet-ajax/index.md)
- [Creación de una interfaz de usuario interactiva con Microsoft ASP.NET AJAX](http://aspnet.4guysfromrolla.com/articles/101007-1.aspx)
- [Usar NEWID para ordenar registros aleatoriamente](http://www.sqlteam.com/article/using-newid-to-randomly-sort-records)
- [Usar el control Timer](http://aspnet.4guysfromrolla.com/articles/061808-1.aspx)

### <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de varios libros de ASP/ASP. net y fundador de 4GuysFromRolla.com, ha estado trabajando con las tecnologías Web de Microsoft desde 1998. Scott funciona como consultor, profesor y redactor independiente. Su último libro se [*enseña a ASP.NET 3,5 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672329972/4guysfromrollaco). Se puede acceder a Scott en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o a través de su blog en [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Agradecimiento especial a

Muchos revisores útiles revisaron esta serie de tutoriales. ¿Está interesado en revisar los próximos artículos de MSDN? Si es así, coloque una línea en [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](interacting-with-the-content-page-from-the-master-page-cs.md)
> [Siguiente](specifying-the-master-page-programmatically-cs.md)
