---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
title: Rellenar dinámicamente un control (VB) | Microsoft Docs
author: wenz
description: El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino en t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 27305347-7b5d-4519-97b7-197a357e7f6e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d11320f1f89bb69afe5f62751574079716124da0
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599402"
---
# <a name="dynamically-populating-a-control-vb"></a>Rellenar un control dinámicamente (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate0.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate0VB.pdf)

> El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página.

## <a name="overview"></a>Información general del

El control `DynamicPopulate` en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página. En este tutorial se muestra cómo configurarlo.

## <a name="steps"></a>Pasos

En primer lugar, necesita un servicio Web ASP.NET que implemente el método al que va a llamar `DynamicPopulate`. La clase de servicio Web requiere el atributo `ScriptService` que se define en `Microsoft.Web.Script.Services`; de lo contrario, ASP.NET AJAX no puede crear el proxy de JavaScript del lado cliente para el servicio Web que, a su vez, es necesario para `DynamicPopulate`.

El método Web debe esperar un argumento de tipo cadena, denominado `contextKey`, ya que el control `DynamicPopulate` envía un fragmento de información de contexto con cada llamada de servicio Web. El siguiente servicio Web devuelve la fecha actual en un formato representado por el argumento `contextKey`:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample1.aspx)]

El servicio Web se guarda entonces como `DynamicPopulate.vb.asmx`. Como alternativa, puede implementar el método `getDate()` como un método de página dentro de la página ASP.NET real con el control `DynamicPopulate`.

En el paso siguiente, cree un nuevo archivo ASP.NET. Como siempre, el primer paso consiste en incluir el `ScriptManager` en la página actual para cargar la biblioteca de AJAX de ASP.NET y para que el kit de herramientas de control funcione:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample2.aspx)]

A continuación, agregue un control etiqueta (por ejemplo, con el control HTML del mismo nombre, o la &lt;`asp:Label` /&gt; control Web) que posteriormente mostrará el resultado de la llamada al servicio Web.

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample3.aspx)]

Un botón HTML (como control HTML, ya que no se requiere un postback en el servidor) se usará para desencadenar el rellenado dinámico:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample4.aspx)]

Por último, necesitamos el control `DynamicPopulateExtender` para conectar las cosas. Se establecerán los siguientes atributos (además de los obvios, `ID` y `runat`=`"server"`):

- `TargetControlID` dónde colocar el resultado de la llamada al servicio Web
- `ServicePath` ruta de acceso al servicio Web (omita si desea utilizar un método de página)
- `ServiceMethod` nombre del método de la página o método Web
- `ContextKey` la información de contexto que se va a enviar al servicio Web
- `PopulateTriggerControlID` elemento que desencadena la llamada de servicio Web
- `ClearContentsDuringUpdate` si se va a vaciar el elemento de destino durante la llamada al servicio Web

Como puede ver, el control requiere cierta información pero poner todo en marcha es bastante sencillo. Este es el marcado del control `DynamicPopulateExtender` en el escenario actual:

[!code-aspx[Main](dynamically-populating-a-control-vb/samples/sample5.aspx)]

Ejecute la página ASP.NET en el explorador y haga clic en el botón. recibirá la fecha actual en formato de mes-día-año.

[![un clic en el botón recupera la fecha del servidor](dynamically-populating-a-control-vb/_static/image2.png)](dynamically-populating-a-control-vb/_static/image1.png)

Un clic en el botón recupera la fecha del servidor ([haga clic para ver la imagen de tamaño completo](dynamically-populating-a-control-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
> [Siguiente](dynamically-populating-a-control-using-javascript-code-vb.md)
