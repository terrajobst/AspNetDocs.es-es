---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: Modelo de página de ASP.NET 2,0 | Microsoft Docs
author: microsoft
description: En ASP.NET 1. x, los desarrolladores tenían la opción de elegir entre un modelo de código en línea y un modelo de código subyacente. El código subyacente se puede implementar mediante el uso del atributo src attr...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: bcb71b2b5a484e8756406867e08e8aa699a9024d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78440713"
---
# <a name="the-aspnet-20-page-model"></a>El modelo de página de ASP.NET 2,0

por [Microsoft](https://github.com/microsoft)

> En ASP.NET 1. x, los desarrolladores tenían la opción de elegir entre un modelo de código en línea y un modelo de código subyacente. El código subyacente se puede implementar mediante el atributo src o el atributo CodeBehind de la Directiva @Page. En ASP.NET 2,0, los desarrolladores todavía tienen una opción entre el código en línea y el código subyacente, pero se han realizado mejoras significativas en el modelo de código subyacente.

En ASP.NET 1. x, los desarrolladores tenían la opción de elegir entre un modelo de código en línea y un modelo de código subyacente. El código subyacente se puede implementar mediante el atributo src o el atributo CodeBehind de la Directiva @Page. En ASP.NET 2,0, los desarrolladores todavía tienen una opción entre el código en línea y el código subyacente, pero se han realizado mejoras significativas en el modelo de código subyacente.

## <a name="improvements-in-the-code-behind-model"></a>Mejoras en el modelo de código subyacente

Con el fin de comprender por completo los cambios en el modelo de código subyacente en ASP.NET 2,0, lo mejor es revisar rápidamente el modelo tal y como existía en ASP.NET 1. x.

## <a name="the-code-behind-model-in-aspnet-1x"></a>El modelo de código subyacente en ASP.NET 1. x

En ASP.NET 1. x, el modelo de código subyacente constaba de un archivo ASPX (WebForm) y un archivo de código subyacente que contiene código de programación. Los dos archivos se conectaron mediante la Directiva @Page del archivo ASPX. Cada control de la página ASPX tenía una declaración correspondiente en el archivo de código subyacente como una variable de instancia. El archivo de código subyacente también contenía código para el enlace de eventos y código generado necesario para el diseñador de Visual Studio. Este modelo funcionó bastante bien, pero como todos los elementos de ASP.NET de la página ASPX requerían el código correspondiente en el archivo de código subyacente, no había ninguna separación real de código y contenido. Por ejemplo, si un diseñador agregó un nuevo control de servidor a un archivo ASPX fuera del IDE de Visual Studio, la aplicación se interrumpirá debido a la ausencia de una declaración para ese control en el archivo de código subyacente.

## <a name="the-code-behind-model-in-aspnet-20"></a>El modelo de código subyacente en ASP.NET 2,0

ASP.NET 2,0 mejora considerablemente en este modelo. En ASP.NET 2,0, el código subyacente se implementa mediante las nuevas *clases parciales* proporcionadas en ASP.net 2,0. La clase de código subyacente de ASP.NET 2,0 se define como una clase parcial, lo que significa que solo contiene parte de la definición de clase. La parte restante de la definición de clase se genera dinámicamente mediante ASP.NET 2,0 mediante la página ASPX en tiempo de ejecución o cuando el sitio web está precompilado. El vínculo entre el archivo de código subyacente y la página ASPX todavía se establece mediante la Directiva @ page. Sin embargo, en lugar de un atributo CodeBehind o src, ASP.NET 2,0 ahora usa el atributo Codefile. El atributo Inherits también se utiliza para especificar el nombre de clase de la página.

Una directiva @ Page típica podría tener el siguiente aspecto:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

Una definición de clase típica en un archivo de código subyacente de ASP.NET 2,0 podría tener el siguiente aspecto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> C#y Visual Basic son los únicos lenguajes administrados que admiten actualmente clases parciales. Por lo tanto, los programadores que usen J# no podrán usar el modelo de código subyacente en ASP.NET 2,0.

El nuevo modelo mejora el modelo de código subyacente, ya que ahora los desarrolladores tendrán archivos de código que contienen solo el código que han creado. También proporciona una separación real del código y el contenido, ya que no hay declaraciones de variables de instancia en el archivo de código subyacente.

> [!NOTE]
> Dado que la clase parcial de la página ASPX es donde tiene lugar el enlace de eventos, Visual Basic los desarrolladores pueden realizar un pequeño aumento del rendimiento mediante el uso de la palabra clave handles en el código subyacente para enlazar eventos. C#no tiene ninguna palabra clave equivalente.

## <a name="new--page-directive-attributes"></a>Nuevos atributos de Directiva @ page

ASP.NET 2,0 agrega muchos atributos nuevos a la Directiva @ page. Los atributos siguientes son nuevos en ASP.NET 2,0.

## <a name="async"></a>Async

El atributo Async permite configurar la página para que se ejecute de forma asincrónica. Cubra las páginas asincrónicas más adelante en este módulo.

## <a name="asynctimeout"></a>AsyncTimeout

Especificó el tiempo de espera para las páginas asincrónicas. El valor predeterminado es 45 segundos.

## <a name="codefile"></a>CodeFile

El atributo Codefile es el sustituto del atributo CodeBehind en Visual Studio 2002/2003.

### <a name="codefilebaseclass"></a>CodeFileBaseClass

El atributo CodeFileBaseClass se usa en los casos en los que desea que varias páginas se deriven de una sola clase base. Debido a la implementación de clases parciales en ASP.NET, sin este atributo, una clase base que utiliza campos comunes compartidos para hacer referencia a los controles declarados en una página ASPX no funcionaría correctamente porque el motor de compilación de ASP. NETs creará automáticamente nuevos miembros basados en los controles de la página. Por lo tanto, si desea una clase base común para dos o más páginas de ASP.NET, deberá definir especificar la clase base en el atributo CodeFileBaseClass y, a continuación, derivar cada clase de páginas de esa clase base. El atributo Codefile también es necesario cuando se utiliza este atributo.

## <a name="compilationmode"></a>CompilationMode

Este atributo le permite establecer la propiedad CompilationMode de la página ASPX. La propiedad CompilationMode es una enumeración que contiene los valores **Always**, **auto**y **Never**. El valor predeterminado es **siempre**. La configuración **auto** impedirá que ASP.net compile dinámicamente la página, si es posible. La exclusión de las páginas de la compilación dinámica aumenta el rendimiento. Sin embargo, si una página que se excluye contiene el código que se debe compilar, se producirá un error al examinar la página.

## <a name="enableeventvalidation"></a>EnableEventValidation

Este atributo especifica si los eventos de devolución de llamada y postback se validan. Cuando esta opción está habilitada, se comprueban los argumentos de los eventos de devolución de llamada o devolución de llamada para asegurarse de que se originaron en el control de servidor que los representaba originalmente.

## <a name="enabletheming"></a>EnableTheming

Este atributo especifica si se usan o no temas ASP.NET en una página. El valor predeterminado es **false**. Los temas de ASP.NET se describen en el [módulo 10](profiles-themes-and-web-parts.md).

## <a name="linepragmas"></a>LinePragmas

Este atributo especifica si se deben agregar pragmas de línea durante la compilación. Las pragmas de línea son opciones usadas por los depuradores para marcar secciones específicas de código.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostback

Este atributo especifica si se inserta o no JavaScript en la página para mantener la posición de desplazamiento entre los postbacks. Este atributo es **false** de forma predeterminada.

Cuando este atributo es **true**, ASP.net agregará una &lt;script&gt; bloque en el PostBack que tiene este aspecto:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

Tenga en cuenta que el archivo src de este bloque de script es WebResource. axd. Este recurso no es una ruta de acceso física. Cuando se solicita este script, ASP.NET crea dinámicamente el script.

### <a name="masterpagefile"></a>MasterPageFile

Este atributo especifica el archivo de la página maestra de la página actual. La ruta de acceso puede ser relativa o absoluta. Las páginas maestras se describen en el [módulo 4](master-pages.md).

## <a name="stylesheettheme"></a>StyleSheetTheme

Este atributo permite invalidar las propiedades de apariencia de la interfaz de usuario definidas por un tema de ASP.NET 2,0. Los temas se describen en el [módulo 10](profiles-themes-and-web-parts.md).

## <a name="theme"></a>Tema

Especifica el tema de la página. Si no se especifica un valor para el atributo StyleSheetTheme, el atributo Theme invalida todos los estilos aplicados a los controles de la página.

## <a name="title"></a>Título

Establece el título de la página. El valor especificado aquí aparecerá en el elemento &lt;título&gt; de la página representada.

### <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Establece el valor de la enumeración ViewStateEncryptionMode. Los valores disponibles son **siempre**, **auto**y **Never**. El valor predeterminado es **automático**. Cuando este atributo se establece en el valor **auto**, ViewState está cifrado es un control que lo solicita llamando al método **RegisterRequiresViewStateEncryption** .

## <a name="setting-public-property-values-via-the--page-directive"></a>Establecer valores de propiedades públicas mediante la Directiva @ page

Otra nueva funcionalidad de la Directiva @ page en ASP.NET 2,0 es la capacidad de establecer el valor inicial de las propiedades públicas de una clase base. Supongamos, por ejemplo, que tiene una propiedad pública denominada **SomeText** en la clase base y que desea que se inicialice como **Hello** cuando se carga una página. Para ello, solo tiene que establecer el valor en la Directiva @ page de la siguiente manera:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

El atributo **SomeText** de la Directiva @ page establece el valor inicial de la propiedad SomeText de la clase base en *Hello!* . El vídeo siguiente es un tutorial sobre cómo establecer el valor inicial de una propiedad pública en una clase base mediante la Directiva @ page.

![](the-asp-net-2-0-page-model/_static/image1.png)

[Abrir vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/setprop1.wmv)

## <a name="new-public-properties-of-the-page-class"></a>Nuevas propiedades públicas de la clase Page

Las siguientes propiedades públicas son nuevas en ASP.NET 2,0.

## <a name="apprelativetemplatesourcedirectory"></a>AppRelativeTemplateSourceDirectory

Devuelve la ruta de acceso relativa de la aplicación a la página o el control. Por ejemplo, para una página ubicada en http://app/folder/page.aspx, la propiedad devuelve ~/Folder/.

## <a name="apprelativevirtualpath"></a>AppRelativeVirtualPath

Devuelve la ruta de acceso relativa al directorio virtual de la página o el control. Por ejemplo, en el caso de una página ubicada en http://app/folder/page.aspx, la propiedad devuelve ~/Folder/Page.aspx.

## <a name="asynctimeout"></a>AsyncTimeout

Obtiene o establece el tiempo de espera usado para el control asincrónico de páginas. (Las páginas asincrónicas se tratarán más adelante en este módulo).

## <a name="clientquerystring"></a>ClientQueryString

Propiedad de solo lectura que devuelve la parte de la cadena de consulta de la dirección URL solicitada. Este valor es una dirección URL codificada. Puede usar el método UrlDecode de la clase HttpServerUtility para descodificarlo.

## <a name="clientscript"></a>ClientScript

Esta propiedad devuelve un objeto ClientScriptManager que se puede utilizar para administrar la emisión de ASP. NETs del script del lado cliente. (La clase ClientScriptManager se trata más adelante en este módulo).

## <a name="enableeventvalidation"></a>EnableEventValidation

Esta propiedad controla si se habilita o no la validación de eventos para los eventos de devolución de llamada y postback. Cuando está habilitada, se comprueban los argumentos de los eventos de devolución de llamada o devolución de llamada para asegurarse de que se originaron en el control de servidor que los representaba originalmente.

## <a name="enabletheming"></a>EnableTheming

Esta propiedad obtiene o establece un valor booleano que especifica si un tema ASP.NET 2,0 se aplica a la página.

## <a name="form"></a>Form

Esta propiedad devuelve el formulario HTML en la página ASPX como un objeto HtmlForm.

## <a name="header"></a>Encabezado

Esta propiedad devuelve una referencia a un objeto HtmlHead que contiene el encabezado de página. Puede usar el objeto HtmlHead devuelto para obtener o establecer hojas de estilos, etiquetas meta, etc.

## <a name="idseparator"></a>IdSeparator

Esta propiedad de solo lectura obtiene el carácter que se usa para separar los identificadores de control cuando ASP.NET está compilando un identificador único para los controles de una página. No debe usarse directamente desde el código.

## <a name="isasync"></a>IsAsync

Esta propiedad permite las páginas asincrónicas. Las páginas asincrónicas se tratan más adelante en este módulo.

## <a name="iscallback"></a>IsCallback

Esta propiedad de solo lectura devuelve **true** si la página es el resultado de una devolución de llamada. Las devoluciones de llamada se describen más adelante en este módulo.

## <a name="iscrosspagepostback"></a>IsCrossPagePostBack

Esta propiedad de solo lectura devuelve **true** si la página forma parte de un postback-página. Los postbacks entre páginas se describen más adelante en este módulo.

## <a name="items"></a>Elementos

Devuelve una referencia a una instancia de IDictionary que contiene todos los objetos almacenados en el contexto de las páginas. Puede agregar elementos a este objeto IDictionary y estarán disponibles a lo largo de la duración del contexto.

## <a name="maintainscrollpositiononpostback"></a>MaintainScrollPositionOnPostBack

Esta propiedad controla si ASP.NET emite JavaScript que mantiene la posición de desplazamiento de las páginas en el explorador después de que se produzca un postback. (Los detalles de esta propiedad se han explicado anteriormente en este módulo).

## <a name="master"></a>Master

Esta propiedad de solo lectura devuelve una referencia a la instancia de MasterPage para una página a la que se ha aplicado una página maestra.

## <a name="masterpagefile"></a>MasterPageFile

Obtiene o establece el nombre de archivo de la página maestra de la página. Esta propiedad solo se puede establecer en el método PreInit.

## <a name="maxpagestatefieldlength"></a>MaxPageStateFieldLength

Esta propiedad obtiene o establece la longitud máxima del estado de las páginas en bytes. Si la propiedad se establece en un número positivo, el estado de vista de las páginas se dividirá en varios campos ocultos para que no supere el número de bytes especificado. Si la propiedad es un número negativo, el estado de vista no se dividirá en fragmentos.

## <a name="pageadapter"></a>PageAdapter

Devuelve una referencia al objeto PageAdapter que modifica la página del explorador que lo solicita.

## <a name="previouspage"></a>PreviousPage

Devuelve una referencia a la página anterior en los casos de un servidor. Transfer o un postback-Page.

## <a name="skinid"></a>SkinID

Especifica la máscara de ASP.NET 2,0 que se va a aplicar a la página.

## <a name="stylesheettheme"></a>StyleSheetTheme

Esta propiedad obtiene o establece la hoja de estilos que se aplica a una página.

## <a name="templatecontrol"></a>TemplateControl

Devuelve una referencia al control contenedor de la página.

## <a name="theme"></a>Tema

Obtiene o establece el nombre del tema ASP.NET 2,0 que se aplica a la página. Este valor se debe establecer antes del método PreInit.

## <a name="title"></a>Título

Esta propiedad obtiene o establece el título de la página tal y como se obtiene del encabezado de páginas.

## <a name="viewstateencryptionmode"></a>ViewStateEncryptionMode

Obtiene o establece el ViewStateEncryptionMode de la página. Vea una explicación detallada de esta propiedad anteriormente en este módulo.

## <a name="new-protected-properties-of-the-page-class"></a>Nuevas propiedades protegidas de la clase Page

A continuación se muestran las nuevas propiedades protegidas de la clase Page en ASP.NET 2,0.

## <a name="adapter"></a>Adaptador

Devuelve una referencia a ControlAdapter que representa la página en el dispositivo que la solicitó.

## <a name="asyncmode"></a>AsyncMode

Esta propiedad indica si la página se procesa de forma asincrónica o no. Está diseñado para su uso en tiempo de ejecución y no directamente en el código.

## <a name="clientidseparator"></a>ClientIDSeparator

Esta propiedad devuelve el carácter que se usa como separador al crear identificadores de cliente únicos para los controles. Está diseñado para su uso en tiempo de ejecución y no directamente en el código.

## <a name="pagestatepersister"></a>PageStatePersister

Esta propiedad devuelve el objeto PageStatePersister de la página. Esta propiedad la usan principalmente los desarrolladores de controles de ASP.NET.

## <a name="uniquefilepathsuffix"></a>UniqueFilePathSuffix

Esta propiedad devuelve un sufijo único que se anexa a la ruta de acceso del archivo para los exploradores de almacenamiento en caché. El valor predeterminado es \_\_ufps = y un número de 6 dígitos.

## <a name="new-public-methods-for-the-page-class"></a>Nuevos métodos públicos para la clase Page

Los métodos públicos siguientes son nuevos en la clase Page en ASP.NET 2,0.

## <a name="addonprerendercompleteasync"></a>AddOnPreRenderCompleteAsync

Este método registra los delegados de controlador de eventos para la ejecución asincrónica de la página. Las páginas asincrónicas se tratan más adelante en este módulo.

## <a name="applystylesheetskin"></a>ApplyStyleSheetSkin

Aplica las propiedades de una hoja de estilos de páginas a la página.

## <a name="executeregisteredasynctasks"></a>ExecuteRegisteredAsyncTasks

Este método constituye una tarea asincrónica.

### <a name="getvalidators"></a>GetValidators

Devuelve una colección de validadores para el grupo de validación especificado o el grupo de validación predeterminado si no se especifica ninguno.

## <a name="registerasynctask"></a>RegisterAsyncTask

Este método registra una nueva tarea asincrónica. Las páginas asincrónicas se describen más adelante en este módulo.

## <a name="registerrequirescontrolstate"></a>RegisterRequiresControlState

Este método indica a ASP.NET que el estado del control de páginas debe ser persistente.

## <a name="registerrequiresviewstateencryption"></a>RegisterRequiresViewStateEncryption

Este método indica a ASP.NET que el ViewState de las páginas requiere cifrado.

## <a name="resolveclienturl"></a>ResolveClientUrl

Devuelve una dirección URL relativa que se puede utilizar para las solicitudes de cliente para imágenes, etc.

## <a name="setfocus"></a>SetFocus

Este método establecerá el foco en el control que se especifica cuando la página se carga inicialmente.

## <a name="unregisterrequirescontrolstate"></a>UnregisterRequiresControlState

Este método anulará el registro del control que se le pasa como si ya no requiera la persistencia del estado de control.

## <a name="changes-to-the-page-lifecycle"></a>Cambios en el ciclo de vida de la página

El ciclo de vida de la página en ASP.NET 2,0 no ha cambiado drásticamente, pero hay algunos métodos nuevos que debe tener en cuenta. A continuación se describe el ciclo de vida de la página de ASP.NET 2,0.

## <a name="preinit-new-in-aspnet-20"></a>PreInit (nuevo en ASP.NET 2,0)

El evento PreInit es la primera fase del ciclo de vida a la que puede tener acceso un desarrollador. La adición de este evento permite cambiar mediante programación los temas de ASP.NET 2,0, las páginas maestras, las propiedades de acceso para un perfil de ASP.NET 2,0, etc. Si se encuentra en un estado de postback, es importante tener en cuenta que ViewState todavía no se ha aplicado a los controles en este punto del ciclo de vida. Por lo tanto, si un desarrollador cambia una propiedad de un control en esta fase, es probable que se sobrescriba más adelante en el ciclo de vida de las páginas.

## <a name="init"></a>Init

El evento init no ha cambiado de ASP.NET 1. x. Aquí es donde desea leer o inicializar las propiedades de los controles de la página. En esta fase, las páginas maestras, los temas, etc. ya se aplican a la página.

## <a name="initcomplete-new-in-20"></a>InitComplete (novedad en 2,0)

El evento InitComplete se llama al final de la fase de inicialización de las páginas. En este punto del ciclo de vida, puede tener acceso a los controles de la página, pero su estado todavía no se ha rellenado.

## <a name="preload-new-in-20"></a>Carga previa (novedad en 2,0)

Se llama a este evento una vez aplicados todos los datos de postback y justo antes de la carga de la página\_.

## <a name="load"></a>Load

El evento Load no ha cambiado de ASP.NET 1. x.

## <a name="loadcomplete-new-in-20"></a>LoadComplete (novedad en 2,0)

El evento LoadComplete es el último evento en la fase de carga de páginas. En esta fase, todos los datos de postback y ViewState se han aplicado a la página.

## <a name="prerender"></a>PreRender

Si desea que ViewState se mantenga correctamente para los controles que se agregan a la página dinámicamente, el evento de preprocesamiento es la última oportunidad para agregarlos.

## <a name="prerendercomplete-new-in-20"></a>PreRenderComplete (novedad en 2,0)

En la fase PreRenderComplete, todos los controles se han agregado a la página y la página está lista para ser representada. El evento PreRenderComplete es el último evento generado antes de que se guarden las páginas ViewState.

## <a name="savestatecomplete-new-in-20"></a>SaveStateComplete (novedad en 2,0)

Se llama al evento SaveStateComplete inmediatamente después de que se haya guardado todo el estado de control y ViewState de la página. Este es el último evento antes de que la página se represente realmente en el explorador.

## <a name="render"></a>Representar

El método Render no ha cambiado desde ASP.NET 1. x. Aquí es donde se inicializa el HtmlTextWriter y la página se representa en el explorador.

## <a name="cross-page-postback-in-aspnet-20"></a>Postback-página en ASP.NET 2,0

En ASP.NET 1. x, se requirieron postbacks para publicar en la misma página. No se permiten postbacks entre páginas. ASP.NET 2,0 agrega la capacidad de devolver datos a una página diferente a través de la interfaz IButtonControl. Cualquier control que implemente la nueva interfaz IButtonControl (Button, LinkButton e ImageButton además de controles personalizados de terceros) puede aprovechar esta nueva funcionalidad mediante el uso del atributo PostBackUrl. En el código siguiente se muestra un control de botón que devuelve datos a una segunda página.

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

Cuando se devuelve la página, la página que inicia el PostBack es accesible a través de la propiedad PreviousPage de la segunda página. Esta funcionalidad se implementa a través de la nueva función de cliente de WebForm\_DoPostBackWithOptions que ASP.NET 2,0 representa en la página cuando un control se devuelve a una página diferente. Esta función de JavaScript se proporciona mediante el nuevo controlador WebResource. axd que emite el script al cliente.

El vídeo siguiente es un tutorial de postback-página.

![](the-asp-net-2-0-page-model/_static/image2.png)

[Abrir vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/xpage1.wmv)

## <a name="more-details-on-cross-page-postbacks"></a>Más detalles sobre las devoluciones entre páginas

### <a name="viewstate"></a>ViewState

Es posible que ya se le haya preguntado lo que sucede con ViewState en la primera página de un escenario de postback-página. Después de todo, cualquier control que no implemente IPostBackDataHandler mantendrá su estado a través de ViewState, de modo que para tener acceso a las propiedades de ese control en la segunda página de un postback-página, debe tener acceso a ViewState para la página. ASP.NET 2,0 se encarga de este escenario con un nuevo campo oculto en la segunda página denominada \_\_PREVIOUSPAGE. El campo de formulario de \_\_PREVIOUSPAGE contiene el ViewState de la primera página para poder tener acceso a las propiedades de todos los controles de la segunda página.

### <a name="circumventing-findcontrol"></a>Eludir FindControl

En el tutorial de vídeo de un postback-Page, usé el método FindControl para obtener una referencia al control TextBox en la primera página. Ese método funciona bien para ese propósito, pero FindControl es costoso y requiere escribir código adicional. Afortunadamente, ASP.NET 2,0 proporciona una alternativa a FindControl para este propósito que funcionará en muchos escenarios. La Directiva PreviousPageType permite tener una referencia fuertemente tipada a la página anterior mediante el uso del atributo TypeName o VirtualPath. El atributo TypeName permite especificar el tipo de la página anterior, mientras que el atributo VirtualPath permite hacer referencia a la página anterior mediante una ruta de acceso virtual. Después de establecer la Directiva PreviousPageType, debe exponer los controles, etc. a los que desea permitir el acceso mediante propiedades públicas.

## <a name="lab-1-cross-page-postback"></a>Devoluciones entre páginas del laboratorio 1

En este laboratorio, creará una aplicación que utiliza la nueva funcionalidad de postback-página de postback de ASP.NET 2,0.

1. Abra Visual Studio 2005 y cree un nuevo sitio Web ASP.NET.
2. Agregue un nuevo WebForm denominado página2. aspx.
3. Abra el. aspx predeterminado en Vista de diseño y agregue un control de botón y un control de cuadro de texto. 

    1. Asigne al control de botón un identificador de **SubmitButton** y al control de cuadro de texto un identificador de **nombre de usuario**.
    2. Establezca la propiedad PostBackUrl del botón en página2. aspx.
4. Abra Page2. aspx en la vista Código fuente.
5. Agregue una directiva @ PreviousPageType como se muestra a continuación:
6. Agregue el código siguiente a la página\_carga del código subyacente de página2. aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. Para compilar el proyecto, haga clic en compilar en el menú compilar.
8. Agregue el código siguiente al código subyacente para default. aspx: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. Cambie la página\_cargar en página2. aspx a lo siguiente: 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. Compile el proyecto.
11. Ejecute el proyecto.
12. Escriba su nombre en el cuadro de texto y haga clic en el botón.
13. ¿Cuál es el resultado?

## <a name="asynchronous-pages-in-aspnet-20"></a>Páginas asincrónicas en ASP.NET 2,0

Muchos problemas de contención de ASP.NET se deben a la latencia de llamadas externas (como llamadas a servicios web o de bases de datos), latencia de e/s de archivos, etc. Cuando se realiza una solicitud en una aplicación ASP.NET, ASP.NET usa uno de sus subprocesos de trabajo para atender esa solicitud. Esa solicitud es propietaria del subproceso hasta que se completa la solicitud y se envía la respuesta. ASP.NET 2,0 busca resolver problemas de latencia con estos tipos de problemas agregando la capacidad de ejecutar páginas de forma asincrónica. Esto significa que un subproceso de trabajo puede iniciar la solicitud y luego entregar la ejecución adicional a otro subproceso, con lo que se devuelve rápidamente al grupo de subprocesos disponibles. Cuando se ha completado la e/s de archivo, la llamada de base de datos, etc., se obtiene un nuevo subproceso del grupo de subprocesos para finalizar la solicitud.

El primer paso para que una página se ejecute de forma asincrónica es establecer el atributo **Async** de la Directiva de página de la siguiente manera:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

Este atributo indica a ASP.NET que implemente el IHttpAsyncHandler para la página.

El siguiente paso consiste en llamar al método AddOnPreRenderCompleteAsync en un punto del ciclo de vida de la página antes de la representación previa. (Este método suele llamarse en la página\_carga). El método AddOnPreRenderCompleteAsync toma dos parámetros: BeginEventHandler y EndEventHandler. El BeginEventHandler devuelve un IAsyncResult que se pasa a continuación como un parámetro a EndEventHandler.

El vídeo siguiente es un tutorial de una solicitud de página asincrónica.

![](the-asp-net-2-0-page-model/_static/image3.png)

[Abrir vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/async1.wmv)

> [!NOTE]
> Una página asincrónica no se representa en el explorador hasta que se haya completado el EndEventHandler. Sin duda, pero algunos desarrolladores considerarán que las solicitudes asincrónicas son similares a las devoluciones de llamada asincrónicas. Es importante saber que no son. La ventaja de las solicitudes asincrónicas es que el primer subproceso de trabajo se puede devolver al grupo de subprocesos para atender las nuevas solicitudes, lo que reduce la contención debido a que está enlazado a la e/s, etc.

## <a name="script-callbacks-in-aspnet-20"></a>Incluir devoluciones de llamada en ASP.NET 2,0

Los desarrolladores web siempre han buscado formas de evitar el parpadeo asociado a una devolución de llamada. En ASP.NET 1. x, SmartNavigation era el método más común para evitar el parpadeo, pero SmartNavigation producía problemas para algunos desarrolladores debido a la complejidad de su implementación en el cliente. ASP.NET 2,0 soluciona este problema con las devoluciones de llamada de script. Las devoluciones de llamada de script usan XMLHttp para realizar solicitudes en el servidor Web a través de JavaScript. La solicitud XMLHttp devuelve datos XML que se pueden manipular a continuación a través del DOM del explorador. El nuevo controlador WebResource. axd oculta el código XMLHttp al usuario.

Hay varios pasos que son necesarios para configurar una devolución de llamada de script en ASP.NET 2,0.

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a>Paso 1: implementar la interfaz ICallbackEventHandler

Para que ASP.NET reconozca su página como participante en una devolución de llamada de script, debe implementar la interfaz ICallbackEventHandler. Puede hacerlo en el archivo de código subyacente, como por ejemplo:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

También puede hacerlo mediante la Directiva @ Implements, como la siguiente:

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

Normalmente, se usaría la Directiva @ Implements al usar el código ASP.NET insertado.

## <a name="step-2--call-getcallbackeventreference"></a>Paso 2: llamar a GetCallbackEventReference

Como se mencionó anteriormente, la llamada XMLHttp se encapsula en el controlador WebResource. axd. Cuando se representa la página, ASP.NET agregará una llamada a WebForm\_DoCallBack, un script de cliente proporcionado por WebResource. axd. La función de devolución de llamada de\_WebForm reemplaza el \_\_función de postback para una devolución de llamada. Recuerde que \_\_el PostBack envía mediante programación el formulario en la página. En un escenario de devolución de llamada, desea evitar un postback, por lo que \_\_no será suficiente.

> [!NOTE]
> en un escenario de devolución de llamada de script de cliente todavía se representa \_\_DoCallBack en la página. Sin embargo, no se utiliza para la devolución de llamada.

Los argumentos para el WebForm\_función del lado cliente de la devolución de llamada se proporcionan a través de la función del lado servidor GetCallbackEventReference, que normalmente se llamaría en la carga de la página\_. Una llamada típica a GetCallbackEventReference podría tener el siguiente aspecto:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> En este caso, cm es una instancia de ClientScriptManager. La clase ClientScriptManager se tratará más adelante en este módulo.

Hay varias versiones sobrecargadas de GetCallbackEventReference. En este caso, los argumentos son los siguientes:

`this`

Referencia al control en el que se llama a GetCallbackEventReference. En este caso, es la propia página.

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

Un argumento de cadena que se pasará desde el código del lado cliente al evento del servidor. En este caso, IM pasando el valor de una lista desplegable denominada ddlCompany.

`ShowCompanyName`

El nombre de la función del lado cliente que aceptará el valor devuelto (como cadena) del evento de devolución de llamada del servidor. Solo se llamará a esta función cuando la devolución de llamada del servidor sea correcta. Por lo tanto, por motivos de solidez, generalmente se recomienda usar la versión sobrecargada de GetCallbackEventReference que toma un argumento de cadena adicional que especifica el nombre de una función del lado cliente que se va a ejecutar en caso de error.

`null`

Una cadena que representa una función del lado cliente que inició antes de la devolución de llamada al servidor. En este caso, no hay ningún script de este tipo, por lo que el argumento es NULL.

`true`

Valor booleano que especifica si la devolución de llamada se realizará de forma asincrónica o no.

La llamada a WebForm\_DoCallBack en el cliente pasará estos argumentos. Por lo tanto, cuando esta página se represente en el cliente, ese código tendrá el siguiente aspecto:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

Tenga en cuenta que la firma de la función en el cliente es un poco diferente. La función del lado cliente pasa 5 cadenas y un valor booleano. La cadena adicional (que es null en el ejemplo anterior) contiene la función del lado cliente que controlará los errores de la devolución de llamada del servidor.

## <a name="step-3--hook-the-client-side-control-event"></a>Paso 3: enlazar el evento de control del lado cliente

Observe que el valor devuelto de GetCallbackEventReference anterior se asignó a una variable de cadena. Esa cadena se utiliza para enlazar un evento del lado cliente para el control que inicia la devolución de llamada. En este ejemplo, la devolución de llamada se inicia mediante una lista desplegable en la página, por lo que deseo enlazar el evento *onchange* .

Para enlazar el evento del lado cliente, basta con agregar un controlador al marcado del lado cliente de la manera siguiente:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

Recuerde que *cbRef* es el valor devuelto de la llamada a GetCallbackEventReference. Contiene la llamada a WebForm\_DoCallBack que se mostró anteriormente.

## <a name="step-4--register-the-client-side-script"></a>Paso 4: registrar el script del lado cliente

Recuerde que la llamada a GetCallbackEventReference especificó que un script del lado cliente denominado **ShowCompanyName** se ejecutaría cuando la devolución de llamada del servidor se realizase correctamente. Ese script debe agregarse a la página mediante una instancia de ClientScriptManager. (La clase ClientScriptManager se tratará más adelante en este módulo). Para ello, haga lo siguiente:

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a>Paso 5: llamar a los métodos de la interfaz ICallbackEventHandler

ICallbackEventHandler contiene dos métodos que debe implementar en el código. Son **RaiseCallbackEvent** y **GetCallbackEvent**.

**RaiseCallbackEvent** toma una cadena como argumento y no devuelve nada. El argumento de cadena se pasa desde la llamada del lado cliente a WebForm\_DoCallBack. En este caso, ese valor es el atributo de *valor* de la lista desplegable denominada ddlCompany. El código del lado servidor debe colocarse en el método RaiseCallbackEvent. Por ejemplo, si la devolución de llamada realiza una solicitud WebRequest en un recurso externo, ese código debe colocarse en RaiseCallbackEvent.

**GetCallbackEvent** es responsable de procesar el valor devuelto por la devolución de llamada al cliente. No toma ningún argumento y devuelve una cadena. La cadena que devuelve se pasará como argumento a la función del lado cliente, en este caso *ShowCompanyName*.

Una vez que haya completado los pasos anteriores, está listo para realizar una devolución de llamada de script en ASP.NET 2,0.

![](the-asp-net-2-0-page-model/_static/image4.png)

[Abrir vídeo de pantalla completa](the-asp-net-2-0-page-model/_static/callback1.wmv)

Las devoluciones de llamada de script en ASP.NET se admiten en cualquier explorador que admita la realización de llamadas XMLHttp. Esto incluye todos los exploradores modernos en uso hoy en día. Internet Explorer utiliza el objeto ActiveX XMLHttp mientras otros exploradores modernos (incluido el próximo IE 7) usan un objeto XMLHttp intrínseco. Para determinar mediante programación si un explorador admite las devoluciones de llamada, puede usar la propiedad **request. Browser. SupportCallback** . Esta propiedad devolverá **true** si el cliente que realiza la solicitud admite devoluciones de llamada de script.

## <a name="working-with-client-script-in-aspnet-20"></a>Trabajar con scripts de cliente en ASP.NET 2,0

Los scripts de cliente en ASP.NET 2,0 se administran mediante el uso de la clase ClientScriptManager. La clase ClientScriptManager realiza un seguimiento de los scripts de cliente mediante un tipo y un nombre. Esto evita que el mismo script se inserte mediante programación en una página más de una vez.

> [!NOTE]
> Una vez que un script se ha registrado correctamente en una página, cualquier intento posterior de registrar el mismo script simplemente hará que el script no se registre por segunda vez. No se agregan scripts duplicados y no se produce ninguna excepción. Para evitar cálculos innecesarios, hay métodos que puede usar para determinar si un script ya está registrado, de modo que no intente registrarlo más de una vez.

Los métodos de ClientScriptManager deben estar familiarizados con todos los desarrolladores de ASP.NET actuales:

## <a name="registerclientscriptblock"></a>RegisterClientScriptBlock

Este método agrega un script a la parte superior de la página representada. Esto resulta útil para agregar funciones que se llamarán explícitamente en el cliente.

Hay dos versiones sobrecargadas de este método. Tres de los cuatro argumentos son comunes entre ellos. Son estos:

`type (string)`

El argumento de ***tipo*** identifica un tipo para el script. Por lo general, se recomienda usar el tipo de la página (esto. GetType ()) para el tipo.

`key (string)`

El argumento ***key*** es una clave definida por el usuario para el script. Debe ser único para cada script. Si intenta agregar un script con la misma clave y el mismo tipo de un script ya agregado, no se agregará.

`script (string)`

El argumento ***script*** es una cadena que contiene el script real que se va a agregar. Se recomienda usar StringBuilder para crear el script y, a continuación, usar el método ToString () en StringBuilder para asignar el argumento de ***script*** .

Si usa la RegisterClientScriptBlock sobrecargada que solo toma tres argumentos, debe incluir elementos de script (&lt;&gt; de script y &lt;/script&gt;) en el script.

Puede optar por usar la sobrecarga de RegisterClientScriptBlock que toma un cuarto argumento. El cuarto argumento es un valor booleano que especifica si ASP.NET debe agregar elementos de script. Si este argumento es **true**, el script no debe incluir los elementos de script explícitamente.

Use el método IsClientScriptBlockRegistered para determinar si ya se ha registrado un script. Esto le permite evitar un intento de volver a registrar un script que ya se ha registrado.

### <a name="registerclientscriptinclude-new-in-20"></a>RegisterClientScriptInclude (novedad en 2,0)

La etiqueta RegisterClientScriptInclude crea un bloque de script que se vincula a un archivo de script externo. Tiene dos sobrecargas. Uno toma una clave y una dirección URL. La segunda agrega un tercer argumento que especifica el tipo.

Por ejemplo, el código siguiente genera un bloque de script que se vincula a jsfunctions. js en la raíz de la carpeta scripts de la aplicación:

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

Este código genera el código siguiente en la página representada:

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> El bloque de script se representa en la parte inferior de la página.

Use el método IsClientScriptIncludeRegistered para determinar si ya se ha registrado un script. Esto le permite evitar un intento de volver a registrar un script.

## <a name="registerstartupscript"></a>RegisterStartupScript

El método RegisterStartupScript toma los mismos argumentos que el método RegisterClientScriptBlock. Un script registrado con RegisterStartupScript se ejecuta después de que se cargue la página, pero antes del evento OnLoad del lado cliente. En 1. X, los scripts registrados con RegisterStartupScript se colocaban justo antes de la etiqueta de cierre &lt;/Form.&gt; mientras que los scripts registrados con RegisterClientScriptBlock se colocaban inmediatamente después del formulario de apertura &lt;&gt; etiqueta. En ASP.NET 2,0, ambos se colocan inmediatamente antes de la etiqueta de cierre de la&gt; &lt;/Form.

> [!NOTE]
> Si registra una función con RegisterStartupScript, esa función no se ejecutará hasta que la llame explícitamente en el código del lado cliente.

Use el método IsStartupScriptRegistered para determinar si ya se ha registrado un script y evitar un intento de volver a registrar un script.

## <a name="other-clientscriptmanager-methods"></a>Otros métodos de ClientScriptManager

Estos son algunos de los otros métodos útiles de la clase ClientScriptManager.

|  <strong>GetCallbackEventReference</strong>   |                                                 Vea las devoluciones de llamada de script anteriores en este módulo.                                                 |
|-----------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
|  <strong>GetPostBackClientHyperlink</strong>  |                Obtiene una referencia de JavaScript (JavaScript:&lt;llamada a&gt;) que se puede usar para devolver datos de un evento del lado cliente.                 |
|  <strong>GetPostBackEventReference</strong>   |                                   Obtiene una cadena que se puede utilizar para iniciar una devolución desde el cliente.                                    |
|      <strong>GetWebResourceUrl</strong>       | Devuelve una dirección URL a un recurso que está incrustado en un ensamblado. Debe usarse junto con <strong>RegisterClientScriptResource</strong>. |
| <strong>RegisterClientScriptResource</strong> |     Registra un recurso Web en la página. Se trata de recursos incrustados en un ensamblado y controlados por el nuevo controlador WebResource. axd.      |
|     <strong>RegisterHiddenField</strong>      |                                                 Registra un campo de formulario oculto en la página.                                                 |
|  <strong>RegisterOnSubmitStatement</strong>   |                                  Registra el código del lado cliente que se ejecuta cuando se envía el formulario HTML.                                   |
