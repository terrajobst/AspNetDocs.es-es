---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
title: Descripción de los desencadenadores UpdatePanel de ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Al trabajar en el editor de marcado de Visual Studio, es posible que observe (desde IntelliSense) que hay dos elementos secundarios de un control UpdatePanel. Uno de qu...
ms.author: riande
ms.date: 03/12/2008
ms.assetid: faab8503-2984-48a9-8a40-7728461abc50
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-updatepanel-triggers
msc.type: authoredcontent
ms.openlocfilehash: b1cc869f373d4f8283b4d92af74707c3f11fef61
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74588799"
---
# <a name="understanding-aspnet-ajax-updatepanel-triggers"></a>Descripción de los desencadenadores UpdatePanel de ASP.NET AJAX

por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial02_Triggers_cs.pdf)

> Al trabajar en el editor de marcado de Visual Studio, es posible que observe (desde IntelliSense) que hay dos elementos secundarios de un control UpdatePanel. Uno de los cuales es el elemento triggers, que especifica los controles de la página (o el control de usuario, si está usando uno) que desencadenará una representación parcial del control UpdatePanel en el que reside el elemento.

## <a name="introduction"></a>Introducción

La tecnología ASP.NET de Microsoft aporta un modelo de programación orientado a objetos y basado en eventos y lo agrupa con las ventajas del código compilado. Sin embargo, su modelo de procesamiento del lado servidor tiene varias desventajas inherentes a la tecnología, muchas de las cuales se pueden solucionar con las nuevas características incluidas en las extensiones de AJAX Microsoft ASP.NET 3,5. Estas extensiones permiten muchas características nuevas de cliente enriquecidas, incluida la representación parcial de páginas sin necesidad de una actualización de página completa, la capacidad de obtener acceso a los servicios web a través de scripts de cliente (incluida la API de generación de perfiles de ASP.NET) y una amplia API del lado cliente. diseñado para reflejar muchos de los esquemas de control que se han detectado en el conjunto de controles del servidor de ASP.NET.

En estas notas del producto se examina la funcionalidad de los desencadenadores XML del componente `UpdatePanel` de ASP.NET AJAX. Los desencadenadores XML proporcionan un control granular sobre los componentes que pueden provocar una representación parcial de controles UpdatePanel específicos.

Estas notas del producto se basan en la versión beta 2 del .NET Framework 3,5 y Visual Studio 2008. Las extensiones de AJAX de ASP.NET, anteriormente un ensamblado de complementos destinado a ASP.NET 2,0, ahora se integran en la biblioteca de clases base .NET Framework. En estas notas del producto también se da por hecho que va a trabajar con Visual Studio 2008, no con Visual Web Developer Express, y proporcionará tutoriales de acuerdo con la interfaz de usuario de Visual Studio (aunque los listados de código serán totalmente compatibles con independencia de entorno de desarrollo).

## <a name="triggers"></a>*Desencadenadores*

De forma predeterminada, los desencadenadores de un UpdatePanel determinado incluyen automáticamente los controles secundarios que invocan un postback, incluidos los controles de cuadro de texto (por ejemplo) que tienen su propiedad `AutoPostBack` establecida en **true**. Sin embargo, los desencadenadores también se pueden incluir mediante declaración con el marcado; Esto se hace en la sección `<triggers>` de la declaración del control UpdatePanel. Aunque se puede tener acceso a los desencadenadores a través de la propiedad de colección `Triggers`, se recomienda registrar los desencadenadores de representación parciales en tiempo de ejecución (por ejemplo, si un control no está disponible en tiempo de diseño) mediante el método `RegisterAsyncPostBackControl(Control)` del objeto ScriptManager de la página, dentro del evento `Page_Load`. Recuerde que las páginas no tienen estado, por lo que debe volver a registrar estos controles cada vez que se crean.

También se puede deshabilitar la inclusión automática del desencadenador secundario (para que los controles secundarios que crean postbacks no desencadenen automáticamente representaciones parciales) estableciendo la propiedad `ChildrenAsTriggers` en **false**. Esto le permite la máxima flexibilidad a la hora de asignar los controles específicos que pueden invocar una representación de página y se recomienda, de modo que un desarrollador pueda optar por responder a un evento, en lugar de controlar los eventos que puedan surgir.

Tenga en cuenta que cuando los controles UpdatePanel están anidados, cuando UpdateMode está establecido en **Conditional**, si se desencadena el UpdatePanel secundario, pero el elemento primario no lo es, solo se actualizará el UpdatePanel secundario. Sin embargo, si se actualiza el UpdatePanel primario, también se actualizará el UpdatePanel secundario.

## <a name="the-lttriggersgt-element"></a>*&lt;desencadena&gt; elemento*

Al trabajar en el editor de marcado de Visual Studio, es posible que observe (desde IntelliSense) que hay dos elementos secundarios de un control de `UpdatePanel`. El elemento más frecuente es el elemento `<ContentTemplate>`, que básicamente encapsula el contenido que se conservará en el panel de actualización (el contenido para el que se habilita la representación parcial). El otro elemento es el elemento `<Triggers>`, que especifica los controles de la página (o el control de usuario, si está usando uno) que desencadenará una representación parcial del control UpdatePanel en el que residen los desencadenadores de &lt;&gt; elemento.

El elemento `<Triggers>` puede contener cualquier número cada uno de los dos nodos secundarios: `<asp:AsyncPostBackTrigger>` y `<asp:PostBackTrigger>`. Ambos aceptan dos atributos, `ControlID` y `EventName`, y pueden especificar cualquier control dentro de la unidad de encapsulación actual (por ejemplo, si el control UpdatePanel reside dentro de un control de usuario Web, no debe intentar hacer referencia a un control de la página en la que residirá el control de usuario).

El elemento `<asp:AsyncPostBackTrigger>` es especialmente útil para que pueda tener como destino cualquier evento de un control que exista como elemento secundario de *cualquier* control UpdatePanel en la unidad de encapsulación, no solo el UpdatePanel bajo el que este desencadenador es un elemento secundario. Por lo tanto, se puede realizar cualquier control para desencadenar una actualización de página parcial.

Del mismo modo, el elemento `<asp:PostBackTrigger>` se puede usar para desencadenar una representación de página parcial, pero una que requiere un recorrido de ida y vuelta completo al servidor. Este elemento de desencadenador también se puede usar para forzar una representación de página completa cuando un control desencadenaría de otro modo una representación de página parcial (por ejemplo, cuando un control de `Button` existe en el elemento `<ContentTemplate>` de un control UpdatePanel). De nuevo, el elemento PostBackTrigger puede especificar cualquier control que sea un elemento secundario de cualquier control UpdatePanel en la unidad de encapsulación actual.

## <a name="lttriggersgt-element-reference"></a>*&lt;desencadenadores&gt; referencia de elemento*

*Descendientes de marcado:*

| **Etiqueta** | **Descripción** |
| --- | --- |
| &lt;ASP: AsyncPostBackTrigger&gt; | Especifica un control y evento que producirá una actualización de página parcial para el UpdatePanel que contiene esta referencia de desencadenador. |
| &lt;ASP: PostBackTrigger&gt; | Especifica un control y un evento que producirán una actualización de página completa (una actualización de página completa). Esta etiqueta se puede usar para forzar una actualización completa cuando un control desencadenaría de otro modo la representación parcial. |

## <a name="walkthrough-cross-updatepanel-triggers"></a>*Tutorial: desencadenadores Cross-UpdatePanel*

1. Cree una nueva página de ASP.NET con un conjunto de objetos ScriptManager para habilitar la representación parcial. Agregue dos UpdatePanel a esta página: en el primero, incluya un control de etiqueta (Label1) y dos controles de botón (button1 y BUTTON2). Button1 debe indicar hacer clic para actualizar y BUTTON2 debe indicar hacer clic para actualizar esto, o algo a lo largo de estas líneas. En el segundo UpdatePanel, incluya solo un control de etiqueta (Label2), pero establezca su propiedad ForeColor en un valor distinto del predeterminado para diferenciarlo.
2. Establezca la propiedad UpdateMode de ambas etiquetas UpdatePanel en **Conditional**.

**Lista 1: marcado para default. aspx:** 

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample1.aspx)]

1. En el controlador de eventos de clic para Button1, establezca Label1. Text y Label2. Text en algo dependiente del tiempo (por ejemplo, DateTime. Now. ToLongTimeString ()). En el caso del controlador de eventos click para BUTTON2, establezca solo Label1. Text en el valor dependiente del tiempo.

**Lista 2: Codebehind (recortado) en default.aspx.cs:** 

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample2.cs)]

1. Presione F5 para compilar y ejecutar el proyecto. Tenga en cuenta que, al hacer clic en actualizar ambos paneles, ambas etiquetas cambian el texto; sin embargo, al hacer clic en actualizar este panel, solo se actualiza Label1.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image2.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image1.png)

([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image3.png))

## <a name="under-the-hood"></a>*Aspectos técnicos*

Al utilizar el ejemplo que acabamos de crear, podemos echar un vistazo a lo que está haciendo ASP.NET AJAX y cómo funcionan los desencadenadores multipanel de UpdatePanel. Para ello, trabajaremos con el código HTML de origen de la página generado, así como con la extensión Mozilla Firefox denominada FireBug, con él, podemos examinar fácilmente las devoluciones de AJAX. También usaremos la herramienta de reflector de .NET de Lutz Roeder. Ambas herramientas están disponibles de forma gratuita y se pueden encontrar con una búsqueda en Internet.

Un examen del código fuente de la página muestra casi nada de lo normal; los controles UpdatePanel se representan como contenedores `<div>` y podemos ver que los recursos del script incluyen los que proporciona el `<asp:ScriptManager>`. También hay algunas nuevas llamadas específicas de AJAX a PageRequestManager que son internas a la biblioteca de scripts de cliente de AJAX. Por último, vemos los dos contenedores UpdatePanel: uno con los botones `<input>` representados con los dos controles `<asp:Label>` que se representan como contenedores `<span>`. (Si inspecciona el árbol DOM en FireBug, observará que las etiquetas están atenuadas para indicar que no producen contenido visible).

Haga clic en el botón actualizar este panel y observe que el UpdatePanel superior se actualizará con la hora actual del servidor. En FireBug, elija la pestaña consola para que pueda examinar la solicitud. Examine primero los parámetros de la solicitud POST:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image5.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image4.png)

([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image6.png))

Tenga en cuenta que UpdatePanel ha indicado exactamente el código AJAX del servidor que se ha desencadenado mediante el parámetro ScriptManager1: `Button1` del control `UpdatePanel1`. Ahora, haga clic en el botón actualizar ambos paneles. A continuación, examinando la respuesta, vemos una serie de variables delimitadas por canalización establecidas en una cadena. en concreto, vemos que el UpdatePanel superior, `UpdatePanel1`, tiene el código HTML que se envía al explorador. La biblioteca de scripts de cliente de AJAX sustituye el contenido HTML original de UpdatePanel por el nuevo contenido a través de la propiedad `.innerHTML`, por lo que el servidor envía el contenido cambiado del servidor como HTML.

Ahora, haga clic en el botón actualizar ambos paneles y examine los resultados del servidor. Los resultados son muy similares: ambos UpdatePanel reciben un nuevo HTML del servidor. Al igual que con la devolución de llamada anterior, se envía un estado de página adicional.

Como podemos ver, dado que no se utiliza ningún código especial para realizar un postback de AJAX, la biblioteca de scripts de cliente de AJAX es capaz de interceptar postbacks de formulario sin ningún código adicional. Los controles de servidor usan automáticamente JavaScript para que no envíen automáticamente el formulario-ASP.NET inserta automáticamente el código para la validación del formulario y el estado ya, que se consigue principalmente mediante la inclusión automática de recursos de script, la clase PostBackOptions y la clase ClientScriptManager.

Por ejemplo, considere un control CheckBox; Examine el desensamblado de la clase en .NET reflector. Para ello, asegúrese de que el ensamblado System. Web está abierto y navegue hasta la clase `System.Web.UI.WebControls.CheckBox`, abriendo el método `RenderInputTag`. Busque un condicional que Compruebe la propiedad `AutoPostBack`:

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image8.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image7.png)

([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image9.png))

Cuando el PostBack automático está habilitado en un control de `CheckBox` (a través de la propiedad AutoPostBack es true), por lo tanto, la etiqueta de `<input>` resultante se representa con un script de control de eventos ASP.NET en su atributo `onclick`. La interceptación del envío del formulario y, a continuación, permite que ASP.NET AJAX se inserte en la página nonintrusively, lo que ayuda a evitar posibles cambios importantes que podrían producirse al usar un reemplazo de cadena posiblemente imprecisa. Además, esto permite que *cualquier* control ASP.net personalizado use la eficacia de ASP.NET AJAX sin código adicional para admitir su uso dentro de un contenedor UpdatePanel.

La funcionalidad de `<triggers>` corresponde a los valores inicializados en la llamada PageRequestManager a \_updateControls (tenga en cuenta que la biblioteca de scripts de cliente de ASP.NET AJAX usa la Convención de que los métodos, eventos y nombres de campo que comienzan con un carácter de subrayado se marcan como internos y no están diseñados para usarse fuera de la propia biblioteca). Con él, podemos observar qué controles están diseñados para hacer retrovoluciones de AJAX.

Por ejemplo, vamos a agregar dos controles adicionales a la página, quedando un control fuera del todo el UpdatePanel y saliendo de uno dentro de un UpdatePanel. Agregaremos un control CheckBox en el UpdatePanel superior y colocará un DropDownList con varios colores definidos en la lista. Este es el nuevo marcado:

**Lista 3: nuevo marcado**

[!code-aspx[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample3.aspx)]

Y este es el nuevo código subyacente:

**Lista 4: Codebehind**

[!code-csharp[Main](understanding-asp-net-ajax-updatepanel-triggers/samples/sample4.cs)]

La idea que subyace en esta página es que la lista desplegable selecciona uno de tres colores para mostrar la segunda etiqueta, que la casilla determina si está en negrita y si las etiquetas muestran la fecha y la hora. La casilla no debe producir una actualización de AJAX, pero la lista desplegable debería, aunque no esté hospedada en un UpdatePanel.

[![](understanding-asp-net-ajax-updatepanel-triggers/_static/image11.png)](understanding-asp-net-ajax-updatepanel-triggers/_static/image10.png)

([Haga clic para ver la imagen de tamaño completo](understanding-asp-net-ajax-updatepanel-triggers/_static/image12.png))

Como se aprecia en la captura de pantalla anterior, el botón más reciente en el que se hace clic fue el botón derecho actualizar este panel, que actualizó el tiempo superior independiente del tiempo inferior. La fecha también se desactivó entre los clics, ya que la fecha es visible en la etiqueta inferior. Por último, es el color de la etiqueta inferior: se actualizó más recientemente que el texto de la etiqueta, lo que demuestra que el estado del control es importante y los usuarios esperan que se conserven a través de los postbacks de AJAX. *Sin embargo*, no se actualizó la hora. El tiempo se rellenó automáticamente a través de la persistencia de la \_\_campo VIEWSTATE de la página que está interpretando el tiempo de ejecución de ASP.NET cuando el control se representó en el servidor. El código del servidor de ASP.NET AJAX no reconoce en qué métodos están cambiando el estado de los controles. simplemente se vuelve a rellenar desde el estado de vista y, a continuación, se ejecutan los eventos adecuados.

Sin embargo, debe señalarse que había inicializado el tiempo dentro de la página\_evento de carga, el tiempo se habría incrementado correctamente. Por lo tanto, los desarrolladores deben tener cuidado de que el código adecuado se ejecute durante los controladores de eventos adecuados y evitar el uso de la carga de la página\_cuando un controlador de eventos de control sea adecuado.

## <a name="summary"></a>Resumen

El control UpdatePanel de ASP.NET AJAX Extensions es versátil y puede utilizar una serie de métodos para identificar los eventos de control que deben hacer que se actualice. Permite que los controles secundarios los actualicen automáticamente, pero también pueden responder a los eventos de control en cualquier parte de la página.

Para reducir la posibilidad de la carga de procesamiento del servidor, se recomienda que la propiedad `ChildrenAsTriggers` de un UpdatePanel se establezca en `false`y que se opte por los eventos en lugar de incluirlos de forma predeterminada. Esto también evita que los eventos innecesarios provoquen efectos potencialmente no deseados, incluida la validación, y cambios en los campos de entrada. Estos tipos de errores pueden ser difíciles de aislar, ya que la página se actualiza de forma transparente al usuario y, por tanto, la causa puede no ser obvia de inmediato.

Mediante el examen de los trabajos internos del modelo de interceptación post de formulario de ASP.NET AJAX, pudimos determinar que utiliza el marco de trabajo ya proporcionado por ASP.NET. Al hacerlo, se conserva la compatibilidad máxima con los controles diseñados con el mismo marco de trabajo y se realiza una intrusión mínima en cualquier JavaScript adicional escrito para la página.

## <a name="bio"></a>Disponibilidad

Rob Paveza es un desarrollador de aplicaciones .NET Senior en Terralever ([www.terralever.com](http://www.terralever.com)), una principal empresa de marketing interactiva en Tempe, AZ. Puede ponerse en contacto con usted en [robpaveza@gmail.com](mailto:robpaveza@gmail.com)y su blog se encuentra en [http://geekswithblogs.net/robp/](http://geekswithblogs.net/robp/).

Scott Cate ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el Presidente de myKB.com ([www.myKB.com](http://www.myKB.com)), donde se especializa en escribir aplicaciones basadas en ASP.net centradas en las soluciones de software de la base de conocimiento. Puede ponerse en contacto con Scott por correo electrónico en [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-partial-page-updates-with-asp-net-ajax.md)
> [Siguiente](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
