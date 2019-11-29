---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
title: Descripción de las actualizaciones parciales de página con ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Quizás la característica más visible de las extensiones de ASP.NET AJAX es la capacidad de realizar una actualización de página parcial o incremental sin realizar un postback completo a t...
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 54d9df99-1161-4899-b4e8-2679c85915e7
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-partial-page-updates-with-asp-net-ajax
msc.type: authoredcontent
ms.openlocfilehash: 4b87cb8f58dbd7f27b16bcb0d488ff361770d4fe
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74622950"
---
# <a name="understanding-partial-page-updates-with-aspnet-ajax"></a>Descripción de las actualizaciones de página parcial con ASP.NET AJAX

por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial01_Partial_Page_Updates_cs.pdf)

> Quizás la característica más visible de las extensiones de AJAX de ASP.NET es la posibilidad de realizar una actualización de página parcial o incremental sin realizar un postback completo en el servidor, sin cambios de código y cambios de marcado mínimos. Las ventajas son extensas: el estado de los elementos multimedia (como Adobe Flash o Windows Media) no cambia, se reducen los costos de ancho de banda y el cliente no experimenta el parpadeo normalmente asociado a un postback.

## <a name="introduction"></a>Introducción

La tecnología ASP.NET de Microsoft aporta un modelo de programación orientado a objetos y basado en eventos y lo agrupa con las ventajas del código compilado. Sin embargo, su modelo de procesamiento del lado servidor tiene varias desventajas inherentes a la tecnología:

- Las actualizaciones de página requieren un recorrido de ida y vuelta al servidor, lo que requiere una actualización de página.
- Los recorridos de ida y vuelta no conservan los efectos generados por JavaScript u otra tecnología del lado cliente (como Adobe Flash)
- Durante el postback, los exploradores distintos de Microsoft Internet Explorer no admiten la restauración automática de la posición de desplazamiento. E incluso en Internet Explorer, sigue habiendo un parpadeo a medida que se actualiza la página.
- Los postbacks pueden implicar una gran cantidad de ancho de banda, ya que el \_\_campo de formulario VIEWSTATE puede crecer, especialmente cuando se trabaja con controles como el control GridView o los repetidores.
- No hay ningún modelo unificado para obtener acceso a los servicios web a través de JavaScript u otra tecnología del lado cliente.

Escriba extensiones de ASP.NET AJAX de Microsoft. AJAX, que representa **una** **J** AvaScript sincrónica **a** ND **X** ml, es un marco de trabajo integrado para proporcionar actualizaciones de páginas incrementales a través de JavaScript multiplataforma, compuesto por código del lado servidor que incluye el marco de Microsoft Ajax, y un componente de script denominado Microsoft Ajax script Library. Las extensiones de AJAX de ASP.NET también proporcionan compatibilidad multiplataforma para acceder a los servicios Web de ASP.NET a través de JavaScript.

En estas notas del producto se examina la funcionalidad de las actualizaciones parciales de páginas de ASP.NET AJAX Extensions, que incluye el componente ScriptManager, el control UpdatePanel y el control UpdateProgress, y tiene en cuenta los escenarios en los que deben o no deben ser utiliza.

Estas notas del producto se basan en la versión beta 2 de Visual Studio 2008 y el .NET Framework 3,5, que integra las extensiones de AJAX de ASP.NET en la biblioteca de clases base (donde anteriormente era un componente de complemento disponible para ASP.NET 2,0). En estas notas del producto también se supone que usa Visual Studio 2008 y no Visual Web Developer Express Edition; es posible que algunas plantillas de proyecto a las que se hace referencia no estén disponibles para los usuarios de Visual Web Developer Express.

## <a name="partial-page-updates"></a>Actualizaciones parciales de páginas

Quizás la característica más visible de las extensiones de AJAX de ASP.NET es la posibilidad de realizar una actualización de página parcial o incremental sin realizar un postback completo en el servidor, sin cambios de código y cambios de marcado mínimos. Las ventajas son extensas: el estado de los elementos multimedia (como Adobe Flash o Windows Media) no cambia, se reducen los costos de ancho de banda y el cliente no experimenta el parpadeo normalmente asociado a un postback.

La capacidad de integrar la representación parcial de páginas se integra en ASP.NET con unos cambios mínimos en el proyecto.

## <a name="walkthrough-integrating-partial-rendering-into-an-existing-project"></a>Tutorial: integrar la representación parcial en un proyecto existente

1. En Microsoft Visual Studio 2008, cree un nuevo proyecto de sitio web de ASP.NET. para ello, vaya a <em>archivo</em> <em>-&gt; nuevo</em> <em>-&gt; sitio web</em> y seleccione ASP.net sitio web en el cuadro de diálogo. Puede asignarle el nombre que desee, y puede instalarlo en el sistema de archivos o en Internet Information Services (IIS).
2. Se le presentará la página predeterminada en blanco con el marcado ASP.NET básico (un formulario del lado servidor y una directiva `@Page`). Quite una etiqueta llamada `Label1` y un botón denominado `Button1` en la página dentro del elemento de formulario. Puede establecer sus propiedades de texto en el valor que desee.
3. En Vista de diseño, haga doble clic en `Button1` para generar un controlador de eventos de código subyacente. Dentro de este controlador de eventos, establezca `Label1.Text` en el botón. .

**Lista 1: marcado para default. aspx antes de habilitar la representación parcial**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample1.aspx)]

**Lista 2: Codebehind (recortado) en default.aspx.cs**

[!code-csharp[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample2.cs)]

1. Presione F5 para iniciar el sitio Web. Visual Studio le pedirá que agregue un archivo Web. config para habilitar la depuración. lo haga. Al hacer clic en el botón, tenga en cuenta que la página se actualiza para cambiar el texto de la etiqueta, y hay un breve parpadeo a medida que se vuelve a dibujar la página.
2. Después de cerrar la ventana del explorador, vuelva a Visual Studio y a la página de marcado. Desplácese hacia abajo en el cuadro de herramientas de Visual Studio y busque la pestaña extensiones AJAX. (Si no dispone de esta pestaña porque está usando una versión anterior de las extensiones AJAX o Atlas, consulte el tutorial para registrar los elementos del cuadro de herramientas de extensiones AJAX más adelante en esta nota del producto o instale la versión actual con el Windows Installer descargable. en el sitio web).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image2.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image1.png)

([Haga clic para ver la imagen de tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image3.png))

1. <em>Problema conocido:</em> Si instala Visual Studio 2008 en un equipo que ya tiene instalado Visual Studio 2005 con las extensiones de AJAX de ASP.NET 2,0, Visual Studio 2008 importará los elementos del cuadro de herramientas de extensiones AJAX. Puede determinar si este es el caso examinando la información sobre herramientas de los componentes; deben indicar la versión 3.5.0.0. Si dicen la versión 2.0.0.0, ha importado los elementos de cuadro de herramientas antiguos y tendrá que importarlos manualmente mediante el cuadro de diálogo Elegir elementos del cuadro de herramientas de Visual Studio. No podrá agregar controles de la versión 2 a través del diseñador.

2. Antes de que comience la etiqueta de `<asp:Label>`, cree una línea de espacio en blanco y haga doble clic en el control UpdatePanel en el cuadro de herramientas. Tenga en cuenta que una nueva Directiva de `@Register` se incluye en la parte superior de la página, lo que indica que los controles del espacio de nombres System. Web. UI se deben importar utilizando el prefijo `asp:`.
3. Arrastre la etiqueta de cierre `</asp:UpdatePanel>` más allá del final del elemento de botón, de modo que el elemento tenga el formato correcto con los controles de etiqueta y de botón ajustados.
4. Después de la etiqueta de apertura `<asp:UpdatePanel>`, empiece a abrir una nueva etiqueta. Tenga en cuenta que IntelliSense le pide dos opciones. En este caso, cree una etiqueta de `<ContentTemplate>`. Asegúrese de ajustar esta etiqueta alrededor de la etiqueta y el botón para que el marcado tenga el formato correcto.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image5.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image4.png)

([Haga clic para ver la imagen de tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image6.png))

1. En cualquier lugar dentro del elemento `<form>`, incluya un control ScriptManager haciendo doble clic en el elemento `ScriptManager` del cuadro de herramientas.
2. Edite la etiqueta de `<asp:ScriptManager>` para que incluya el atributo `EnablePartialRendering= true`.

**Lista 3: marcado para default. aspx con representación parcial habilitada**

[!code-aspx[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample3.aspx)]

1. Abra el archivo Web. config. Tenga en cuenta que Visual Studio ha agregado automáticamente una referencia de compilación a System. Web. Extensions. dll.

1. Novedades de Visual Studio 2008: el archivo Web. config que se incluye en las plantillas de proyecto de sitio web de ASP.NET incluye automáticamente todas las referencias necesarias a las extensiones de ASP.NET AJAX e incluye secciones comentadas de información de configuración que se pueden Sin comentarios para habilitar funcionalidad adicional. Visual Studio 2005 tenía plantillas similares al instalar las extensiones de AJAX de ASP.NET 2,0. Sin embargo, en Visual Studio 2008, las extensiones de AJAX están desactivadas de forma predeterminada (es decir, se les hace referencia de forma predeterminada, pero se pueden quitar como referencias).

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image8.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image7.png)

([Haga clic para ver la imagen de tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image9.png))

1. Presione F5 para iniciar el sitio Web. Tenga en cuenta que no se requirieron cambios en el código fuente para admitir el marcado de solo representación parcial.

Al iniciar el sitio web, debería ver que la representación parcial ahora está habilitada, porque al hacer clic en el botón no habrá ningún parpadeo, ni se producirá ningún cambio en la posición de desplazamiento de la página (en este ejemplo no se muestra eso). Si fuera a la vista del origen representado de la página después de hacer clic en el botón, se confirmará que, de hecho, no se ha producido una devolución: el texto de la etiqueta original sigue siendo parte del marcado de origen y la etiqueta ha cambiado a través de JavaScript.

Visual Studio 2008 no parece tener una plantilla predefinida para un sitio web habilitado para ASP.NET AJAX. Sin embargo, esta plantilla estaba disponible en Visual Studio 2005 si se instalaron las extensiones de AJAX de Visual Studio 2005 y ASP.NET 2,0. Por lo tanto, es probable que la configuración de un sitio web y a partir de la plantilla de sitio web habilitada para AJAX sea aún más fácil, ya que la plantilla debe incluir un archivo Web. config totalmente configurado (compatible con todas las extensiones de ASP.NET AJAX, incluido el acceso a servicios web). y la serialización de JSON: notación de objetos JavaScript) e incluye de forma predeterminada un UpdatePanel y ContentTemplate en la Página principal de formularios Web Forms. Habilitar la representación parcial con esta página predeterminada es tan sencillo como volver a visitar el paso 10 de este tutorial y colocar los controles en la página.

## <a name="the-scriptmanager-control"></a>El control ScriptManager

## <a name="scriptmanager-control-reference"></a>Referencia de control ScriptManager

Propiedades habilitadas para marcado:

| **Nombre de la propiedad** | **ype** | **Descripción** |
| --- | --- | --- |
| AllowCustomErrors: redireccionamiento | Bool | Especifica si se va a utilizar la sección de errores personalizados del archivo Web. config para controlar los errores. |
| AsyncPostBackError: mensaje | String | Obtiene o establece el mensaje de error enviado al cliente si se produce un error. |
| AsyncPostBack: tiempo de espera | Int32 | Obtiene o establece la cantidad de tiempo predeterminada que un cliente debe esperar para que se complete la solicitud asincrónica. |
| EnableScript-globalización | Bool | Obtiene o establece si la globalización de script está habilitada. |
| EnableScript-localización | Bool | Obtiene o establece si la localización del script está habilitada. |
| ScriptLoadTimeout | Int32 | Determina el número de segundos permitidos para cargar scripts en el cliente. |
| ScriptMode | Enum (auto, Debug, release, inherit) | Obtiene o establece si se van a representar las versiones de lanzamiento de los scripts. |
| ScriptPath | String | Obtiene o establece la ruta de acceso raíz a la ubicación de los archivos de script que se van a enviar al cliente. |

Propiedades de solo código:

| **Nombre de la propiedad** | **ype** | **Descripción** |
| --- | --- | --- |
| AuthenticationService | AuthenticationService-administrador | Obtiene detalles sobre el proxy del servicio de autenticación de ASP.NET que se enviará al cliente. |
| IsDebuggingEnabled | Bool | Obtiene si está habilitada la depuración de scripts y código. |
| IsInAsyncPostback | Bool | Obtiene si la página se encuentra actualmente en una solicitud de devolución de datos asincrónica. |
| ProfileService ( | ProfileService-Manager | Obtiene detalles sobre el proxy del servicio de generación de perfiles de ASP.NET que se enviará al cliente. |
| Scripts | Script de&lt;de recopilación: referencia&gt; | Obtiene una colección de referencias de script que se enviarán al cliente. |
| Servicios | Servicio de colección&lt;-referencia&gt; | Obtiene una colección de referencias de proxy de servicio Web que se enviarán al cliente. |
| SupportsPartialRendering | Bool | Obtiene si el cliente actual admite la representación parcial. Si esta propiedad devuelve **false**, todas las solicitudes de página serán postbacks estándar. |

Métodos de código público:

| **Nombre del método** | **ype** | **Descripción** |
| --- | --- | --- |
| SetFocus (cadena) | Void | Establece el foco del cliente en un control determinado cuando se ha completado la solicitud. |

Descendientes de marcado:

| **Etiqueta** | **Descripción** |
| --- | --- |
| &lt;AuthenticationService&gt; | Proporciona detalles sobre el proxy al servicio de autenticación de ASP.NET. |
| &lt;ProfileService&gt; | Proporciona detalles sobre el proxy para el servicio de generación de perfiles ASP.NET. |
| &lt;Scripts&gt; | Proporciona referencias de script adicionales. |
| &lt;ASP: ScriptReference&gt; | Denota una referencia de script específica. |
| &lt;Servicio&gt; | Proporciona referencias de servicio web adicionales que tendrán las clases de proxy generadas. |
| &lt;ASP: ServiceReference&gt; | Denota una referencia de servicio Web específica. |

El control ScriptManager es el núcleo fundamental para las extensiones de ASP.NET AJAX. Proporciona acceso a la biblioteca de scripts (incluido el amplio sistema de tipos de scripts del lado cliente), admite la representación parcial y proporciona una amplia compatibilidad para servicios adicionales de ASP.NET (como la autenticación y la generación de perfiles, pero también otros servicios web). El control ScriptManager también proporciona compatibilidad con la globalización y la localización para los scripts de cliente.

## <a name="providing-alternative-and-supplemental-scripts"></a>Proporcionar scripts alternativos y adicionales

Aunque las extensiones de AJAX Microsoft ASP.NET 2,0 incluyen el código de script completo en las ediciones Debug y Release como recursos insertados en los ensamblados a los que se hace referencia, los desarrolladores pueden redirigir el ScriptManager a archivos de script personalizados, así como registrar scripts adicionales necesarios.

Para reemplazar el enlace predeterminado para los scripts incluidos normalmente (como los que admiten el espacio de nombres sys. WebForms y el sistema de escritura personalizado), puede registrarse para el evento `ResolveScriptReference` de la clase ScriptManager. Cuando se llama a este método, el controlador de eventos tiene la oportunidad de cambiar la ruta de acceso al archivo de script en cuestión. a continuación, el administrador de scripts enviará una copia diferente o personalizada de los scripts al cliente.

Además, las referencias de script (representadas por la clase `ScriptReference`) se pueden incluir mediante programación o mediante el marcado. Para ello, puede modificar mediante programación la `ScriptManager.Scripts` colección o incluir etiquetas de `<asp:ScriptReference>` bajo la etiqueta `<Scripts>`, que es un elemento secundario de primer nivel del control ScriptManager.

## <a name="custom-error-handling-for-updatepanels"></a>Control de errores personalizado para UpdatePanel

Aunque las actualizaciones se controlan mediante desencadenadores especificados por controles UpdatePanel, la compatibilidad con el control de errores y los mensajes de error personalizados se controla mediante la instancia del control ScriptManager de una página. Para ello, se expone un evento, `AsyncPostBackError`, a la página que puede proporcionar una lógica de control de excepciones personalizada.

Al consumir el evento AsyncPostBackError, puede especificar la propiedad `AsyncPostBackErrorMessage`, que hace que se genere un cuadro de alerta tras la finalización de la devolución de llamada.

También es posible la personalización del cliente en lugar de utilizar el cuadro de alerta predeterminado. por ejemplo, puede que desee mostrar un elemento de `<div>` personalizado en lugar del cuadro de diálogo modal del explorador predeterminado. En este caso, puede controlar el error en el script de cliente:

**Lista 5: script del lado cliente para mostrar los errores personalizados**

[!code-html[Main](understanding-partial-page-updates-with-asp-net-ajax/samples/sample4.html)]

Simplemente, el script anterior registra una devolución de llamada con el tiempo de ejecución de AJAX del cliente para cuando se ha completado la solicitud asincrónica. A continuación, comprueba si se ha producido un error y, en ese caso, procesa los detalles del mismo y, por último, indica al tiempo de ejecución que el error se ha controlado en el script personalizado.

## <a name="globalization-and-localization-support"></a>Compatibilidad con la globalización y la localización

El control ScriptManager proporciona una amplia compatibilidad para la localización de cadenas de scripts y componentes de interfaz de usuario. sin embargo, ese tema está fuera del ámbito de esta nota del producto. Para obtener más información, vea las notas del producto sobre la compatibilidad de globalización en extensiones de AJAX de ASP.NET.

## <a name="the-updatepanel-control"></a>Control UpdatePanel

## <a name="updatepanel-control-reference"></a>Referencia de control UpdatePanel

Propiedades habilitadas para marcado:

| **Nombre de la propiedad** | **ype** | **Descripción** |
| --- | --- | --- |
| ChildrenAsTriggers | bool | Especifica si los controles secundarios invocan automáticamente la actualización en el PostBack. |
| Representación | enum (bloque, alineado) | Especifica la manera en que se presentará visualmente el contenido. |
| UpdateMode | enum (siempre, condicional) | Especifica si UpdatePanel siempre se actualiza durante una representación parcial o si solo se actualiza cuando se alcanza un desencadenador. |

Propiedades de solo código:

| **Nombre de la propiedad** | **ype** | **Descripción** |
| --- | --- | --- |
| IsInPartialRendering | bool | Obtiene si UpdatePanel admite la representación parcial de la solicitud actual. |
| ContentTemplate | ITemplate | Obtiene la plantilla de marcado para la solicitud de actualización. |
| ContentTemplateContainer | Control | Obtiene la plantilla de programación para la solicitud de actualización. |
| Desencadenadores | UpdatePanel-TriggerCollection | Obtiene la lista de desencadenadores asociados al objeto UpdatePanel actual. |

Métodos de código público:

| **Nombre del método** | **ype** | **Descripción** |
| --- | --- | --- |
| Update () | Void | Actualiza el UpdatePanel especificado mediante programación. Permite que una solicitud de servidor desencadene una representación parcial de un UpdatePanel sin desencadenar. |

Descendientes de marcado:

| **Etiqueta** | **Descripción** |
| --- | --- |
| &lt;ContentTemplate&gt; | Especifica el marcado que se va a utilizar para representar el resultado de la representación parcial. Elemento secundario de &lt;ASP:&gt;UpdatePanel. |
| &lt;Desencadenadores&gt; | Especifica una colección de *n* controles asociados con la actualización de este UpdatePanel. Elemento secundario de &lt;ASP:&gt;UpdatePanel. |
| &lt;ASP: AsyncPostBackTrigger&gt; | Especifica un desencadenador que invoca una representación de página parcial para el UpdatePanel determinado. Puede ser o no un control como descendiente del UpdatePanel en cuestión. Granular en el nombre del evento. Secundario de &lt;desencadenadores&gt;. |
| &lt;ASP: PostBackTrigger&gt; | Especifica un control que hace que se actualice toda la página. Puede ser o no un control como descendiente del UpdatePanel en cuestión. Granular en el objeto. Secundario de &lt;desencadenadores&gt;. |

El control `UpdatePanel` es el control que delimita el contenido del lado servidor que participará en la funcionalidad de representación parcial de las extensiones AJAX. No hay ningún límite en cuanto al número de controles UpdatePanel que pueden estar en una página y se pueden anidar. Cada UpdatePanel está aislado, por lo que cada uno puede funcionar de forma independiente (puede tener dos UpdatePanel en ejecución al mismo tiempo, lo que representa diferentes partes de la página, independientemente del postback de la página).

El control UpdatePanel se centra principalmente en los desencadenadores de control; de forma predeterminada, cualquier control contenido en el `ContentTemplate` de UpdatePanel que crea un postback se registra como desencadenador de UpdatePanel. Esto significa que UpdatePanel puede trabajar con los controles enlazados a datos predeterminados (como GridView), con controles de usuario, y que se pueden programar en un script.

De forma predeterminada, cuando se desencadena una representación de página parcial, se actualizan todos los controles UpdatePanel de la página, tanto si los controles UpdatePanel definen los desencadenadores para dicha acción como si no. Por ejemplo, si un UpdatePanel define un control de botón y se hace clic en ese control de botón, todos los controles UpdatePanel de esa página se actualizarán de forma predeterminada. Esto se debe a que, de forma predeterminada, la propiedad `UpdateMode` de UpdatePanel está establecida en `Always`. Como alternativa, puede establecer la propiedad UpdateMode en `Conditional`, lo que significa que el UpdatePanel solo se actualizará si se alcanza un desencadenador específico.

## <a name="custom-control-notes"></a>Notas de control personalizadas

Se puede Agregar un UpdatePanel a cualquier control de usuario o control personalizado; sin embargo, la página en la que se incluyen estos controles también debe incluir un control ScriptManager con la propiedad EnablePartialRendering establecida en **true**.

Una manera en la que se puede tener en cuenta cuando se usan controles Web personalizados es invalidar el método de `CreateChildControls()` protegido de la clase `CompositeControl`. Al hacerlo, puede insertar un UpdatePanel entre los elementos secundarios del control y el mundo exterior si determina que la página admite la representación parcial. de lo contrario, puede simplemente disponer los controles secundarios en un contenedor `Control` instancia.

## <a name="updatepanel-considerations"></a>Consideraciones sobre UpdatePanel

El UpdatePanel funciona como algo de un cuadro negro, ajustando los postASP.NET en el contexto de un XMLHttpRequest de JavaScript. Sin embargo, hay importantes consideraciones de rendimiento que deben tenerse en cuenta, tanto en términos de comportamiento como de velocidad. Para entender cómo funciona el UpdatePanel, de modo que pueda decidir mejor Cuándo es adecuado su uso, debe examinar el intercambio de AJAX. En el ejemplo siguiente se usa un sitio existente y, Mozilla Firefox con la extensión Firebug (Firebug captura datos de XMLHttpRequest).

Considere un formulario que, entre otras cosas, tiene un cuadro de texto código postal que se supone rellenar un campo de ciudad y estado en un formulario o control. En última instancia, este formulario recopila información de pertenencia, incluido el nombre, la dirección y la información de contacto de un usuario. Hay muchas consideraciones de diseño que se deben tener en cuenta, en función de los requisitos de un proyecto específico.

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image11.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image10.png)

([Haga clic para ver la imagen de tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image12.png))

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image14.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image13.png)

([Haga clic para ver la imagen de tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image15.png))

En la iteración original de esta aplicación, se compiló un control que incorporaba la totalidad de los datos de registro del usuario, incluido el código postal, la ciudad y el estado. Se ajustó todo el control dentro de un UpdatePanel y se colocó en un formulario Web. Cuando el usuario escribe el código postal, el UpdatePanel detecta el evento (el evento de TextChanged correspondiente en el back-end, bien especificando desencadenadores o usando la propiedad ChildrenAsTriggers establecida en true). AJAX envía todos los campos en el UpdatePanel, tal y como se captura en FireBug (vea el diagrama a la derecha).

Como indica la captura de pantalla, se entregan los valores de cada control dentro de UpdatePanel (en este caso, todos están vacíos), así como el campo ViewState. Se envían todos los datos que se han indicado, a través de 9Kb, cuando en realidad solo se necesitan cinco bytes de datos para realizar esta solicitud concreta. La respuesta es aún más redimensionada: en total, 57kb se envía al cliente, simplemente para actualizar un campo de texto y un campo desplegable.

También puede interesarle ver cómo ASP.NET AJAX actualiza la presentación. La parte de respuesta de la solicitud de actualización de UpdatePanel se muestra en la pantalla de la consola de Firebug a la izquierda. es una cadena delimitada por canalización especialmente formulada que se divide por el script de cliente y, a continuación, se reensambla en la página. En concreto, ASP.NET AJAX establece la propiedad *innerHTML* del elemento HTML en el cliente que representa el UpdatePanel. Cuando el explorador vuelve a generar el DOM, se produce un pequeño retraso, en función de la cantidad de información que es necesario procesar.

La regeneración del DOM desencadena varios problemas adicionales:

[![](understanding-partial-page-updates-with-asp-net-ajax/_static/image17.png)](understanding-partial-page-updates-with-asp-net-ajax/_static/image16.png)

([Haga clic para ver la imagen de tamaño completo](understanding-partial-page-updates-with-asp-net-ajax/_static/image18.png))

- Si el elemento HTML enfocado está dentro de UpdatePanel, perderá el foco. Por lo tanto, para los usuarios que presionaron la tecla TAB para salir del cuadro de texto código postal, su siguiente destino habría sido el cuadro de texto City. Sin embargo, una vez que el UpdatePanel ha actualizado la pantalla, el formulario ya no habría tenido el foco y presionar la tecla TAB habría empezado a resaltar los elementos de foco (como los vínculos).
- Si se usa cualquier tipo de script de cliente personalizado que tiene acceso a los elementos DOM, las referencias que las funciones conservan pueden quedar inactivas después de un postback parcial.

Los UpdatePanel no están diseñados para ser soluciones de detección. En su lugar, proporcionan una solución rápida para ciertas situaciones, como la creación de prototipos, las actualizaciones de control pequeño, y proporcionan una interfaz conocida para los desarrolladores de ASP.NET que pueden estar familiarizados con el modelo de objetos de .NET pero menos, con el DOM. Hay una serie de alternativas que pueden dar lugar a un mejor rendimiento, dependiendo del escenario de la aplicación:

- Considere la posibilidad de usar PageMethods y JSON (notación de objetos JavaScript) permite al desarrollador invocar métodos estáticos en una página como si se invocara una llamada de servicio Web. Dado que los métodos son estáticos, no es necesario ningún Estado. el autor de la llamada de script proporciona los parámetros y el resultado se devuelve de forma asincrónica.
- Considere la posibilidad de usar un servicio Web y JSON si se debe usar un único control en varios lugares de una aplicación. De nuevo, esto requiere muy poco trabajo especial y funciona de forma asincrónica.

La incorporación de funcionalidad a través de los servicios web o los métodos de página también tiene desventajas. En primer lugar, los desarrolladores de ASP.NET suelen crear componentes pequeños de funcionalidad en los controles de usuario (archivos. ascx). Los métodos de página no se pueden hospedar en estos archivos; deben estar hospedados en la clase de página real. aspx. Los servicios Web, de forma similar, deben estar hospedados en la clase. asmx. En función de la aplicación, esta arquitectura puede infringir el principio de responsabilidad única, ya que la funcionalidad de un componente único ahora está distribuida en dos o más componentes físicos que pueden tener pocos o ningún equivalente cohesivo.

Por último, si una aplicación requiere que se usen UpdatePanel, las siguientes directrices deberían ayudarle con la solución de problemas y el mantenimiento.

- **Anide UpdatePanel lo máximo posible, no solo dentro de las unidades, sino también entre unidades de código.** Por ejemplo, tener un UpdatePanel en una página que contiene un control, mientras que ese control también contiene un UpdatePanel, que contiene otro control que contiene un UpdatePanel, es el anidamiento de unidades cruzadas. Esto ayuda a mantener claros los elementos que deben actualizarse y evita las actualizaciones inesperadas en los UpdatePanel secundarios.
- **Mantenga la propiedad *ChildrenAsTriggers* establecida en false y establezca explícitamente los eventos desencadenadores.** El uso de la colección de `<Triggers>` es una manera mucho más clara de controlar los eventos y puede impedir un comportamiento inesperado, ayudando con las tareas de mantenimiento y forzando a un desarrollador a participar en un evento.
- **Use la unidad más pequeña posible para lograr la funcionalidad.** Como se indicó en la explicación del servicio de código postal, el ajuste exclusivo del mínimo de tiempo para el servidor, el procesamiento total y la superficie del intercambio cliente-servidor, lo que mejora el rendimiento.

## <a name="the-updateprogress-control"></a>El control UpdateProgress

## <a name="updateprogress-control-reference"></a>Referencia del control UpdateProgress

Propiedades habilitadas para marcado:

| **Nombre de la propiedad** | **ype** | **Descripción** |
| --- | --- | --- |
| AssociatedUpdate-PanelID | String | Especifica el identificador del UpdatePanel en el que debería informar este UpdateProgress. |
| DisplayAfter | Valor int. | Especifica el tiempo de espera en milisegundos antes de que se muestre este control después de que comience la solicitud asincrónica. |
| DynamicLayout | bool | Especifica si el progreso se representa dinámicamente. |

Descendientes de marcado:

| **Etiqueta** | **Descripción** |
| --- | --- |
| &lt;ProgressTemplate&gt; | Contiene el conjunto de plantillas de control para el contenido que se mostrará con este control. |

El control UpdateProgress proporciona una medida de los comentarios para mantener el interés de los usuarios mientras realiza el trabajo necesario para transportarlo al servidor. Esto puede ayudar a los usuarios a saber que está haciendo algo aunque es posible que no sea aparente, en especial porque la mayoría de los usuarios se utilizan para actualizar la página y ver el resaltado de la barra de estado.

Como nota, los controles UpdateProgress pueden aparecer en cualquier parte de una jerarquía de páginas. Sin embargo, en los casos en que se inicia un postback parcial desde un UpdatePanel secundario (donde UpdatePanel está anidado dentro de otro UpdatePanel), los postbacks que desencadenan el UpdatePanel secundario provocarán que se muestren las plantillas UpdateProgress para el elemento secundario. UpdatePanel y el UpdatePanel primario. Sin embargo, si el desencadenador es un elemento secundario directo del UpdatePanel primario, solo se mostrarán las plantillas UpdateProgress asociadas con el elemento primario.

## <a name="summary"></a>Resumen

Las extensiones de AJAX Microsoft ASP.NET son productos sofisticados diseñados para ayudar a que el contenido web sea más accesible y ofrecer una experiencia de usuario más enriquecida a las aplicaciones Web. Como parte de las extensiones de AJAX de ASP.NET, los controles de representación de página parcial, incluidos los controles ScriptManager, UpdatePanel y UpdateProgress, son algunos de los componentes más visibles del kit de herramientas.

El componente ScriptManager integra el aprovisionamiento de JavaScript de cliente para las extensiones, y permite que los distintos componentes del lado servidor y cliente funcionen conjuntamente con una inversión de desarrollo mínima.

El control UpdatePanel es el marcado de cuadro mágico aparente en el UpdatePanel puede tener el código subyacente del lado servidor y no desencadenar una actualización de página. Los controles UpdatePanel se pueden anidar y pueden depender de controles de otros UpdatePanel. De forma predeterminada, UpdatePanel controla cualquier postback invocado por sus controles descendientes, aunque esta funcionalidad se puede optimizar con precisión, ya sea mediante declaración o mediante programación.

Cuando se usa el control UpdatePanel, los desarrolladores deben tener en cuenta el impacto en el rendimiento que podría surgir. Entre las alternativas posibles se incluyen los servicios web y los métodos de página, aunque se debe tener en cuenta el diseño de la aplicación.

El control UpdateProgress permite al usuario saber que no se pasa por alto y que la solicitud en segundo plano está en curso mientras que la página no está haciendo nada para responder a la entrada del usuario. También incluye la capacidad de anular los resultados de la representación parcial.

Juntas, estas herramientas ayudan a crear una experiencia de usuario enriquecida y sin problemas al hacer que el servidor funcione menos aparente para el usuario y interrumpa el flujo de trabajo menos.

## <a name="bio"></a>Disponibilidad

Scott Cate ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el Presidente de myKB.com ([www.myKB.com](http://www.myKB.com)), donde se especializa en escribir aplicaciones basadas en ASP.net centradas en las soluciones de software de la base de conocimiento. Puede ponerse en contacto con Scott por correo electrónico en [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Siguiente](understanding-asp-net-ajax-updatepanel-triggers.md)
