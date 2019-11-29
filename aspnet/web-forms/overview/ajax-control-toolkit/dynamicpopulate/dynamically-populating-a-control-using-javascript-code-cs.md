---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
title: Rellenar dinámicamente un control mediante códigoC#JavaScript () | Microsoft Docs
author: wenz
description: El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino en t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: cc4c2def-e88c-4456-ae8b-a6ae0ff8cc2d
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/dynamically-populating-a-control-using-javascript-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 24dc358427dec3ffcba16d00041c9a2db657e7e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599261"
---
# <a name="dynamically-populating-a-control-using-javascript-code-c"></a>Rellenar dinámicamente un control mediante el código de JavaScript (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate1.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate1CS.pdf)

> El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página. También se puede desencadenar el rellenado mediante código JavaScript del lado cliente personalizado.

## <a name="overview"></a>Información general del

El control `DynamicPopulate` en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página. También se puede desencadenar el rellenado mediante código JavaScript del lado cliente personalizado.

## <a name="steps"></a>Pasos

En primer lugar, necesita un servicio Web ASP.NET que implemente el método al que llamará el control `DynamicPopulateExtender`. El servicio Web implementa el método `getDate()` que espera un argumento de tipo cadena, denominado `contextKey`, ya que el control `DynamicPopulate` envía un fragmento de información de contexto con cada llamada de servicio Web. Este es el código (`DynamicPopulate.cs.asmx`de archivos), que recupera la fecha actual en uno de los tres formatos siguientes:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample1.aspx)]

En el paso siguiente, cree un nuevo sitio de ASP.NET y comience con el control ScriptManager de AJAX de ASP.NET:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample2.aspx)]

A continuación, agregue un control etiqueta (por ejemplo, mediante el control HTML del mismo nombre, o el control Web `<asp:Label />`) que posteriormente mostrará el resultado de la llamada al servicio Web.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample3.aspx)]

A continuación, incluya un control de `DynamicPopulateExtender` y proporcione información del servicio Web, control de destino, pero no el nombre del control que desencadena el rellenado que se realizará más adelante, con JavaScript personalizado.

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample4.aspx)]

Ahora a la parte de JavaScript. La función `$find()`, definida por la biblioteca ASP.NET AJAX, devuelve una referencia a los objetos del lado servidor del kit de herramientas de control AJAX de ASP.NET, como `DynamicPopulateExtender`. En el archivo actual, `$find("dpe")` devuelve una referencia al control `DynamicPopulateExtender` de la página. Expone un método denominado `populate()` que desencadena el proceso de rellenado dinámico. El método `populate()` requiere un argumento: la clave de contexto que actuará como argumento para el método Web `getDate()`. Por ejemplo, `$find("dpe").populate("format1")` rellenaría la etiqueta con la fecha actual en formato de mes-día-año.

Para que el ejemplo sea un poco más flexible, el usuario puede elegir entre varios formatos de fecha. Para cada uno de ellos, se muestra un botón de radio. Una vez que el usuario hace clic en un botón de radio, el código JavaScript rellena dinámicamente la etiqueta con el formato de fecha seleccionado. Estos son los botones de radio siguientes:

[!code-aspx[Main](dynamically-populating-a-control-using-javascript-code-cs/samples/sample5.aspx)]

Tenga en cuenta que, en el contexto de un botón de radio, la expresión de JavaScript `this.value` hace referencia al valor del botón actual, que es exactamente la misma información con la que puede trabajar el método `getDate()`.

[![un clic en el botón recupera la fecha del servidor, en el formato especificado](dynamically-populating-a-control-using-javascript-code-cs/_static/image2.png)](dynamically-populating-a-control-using-javascript-code-cs/_static/image1.png)

Un clic en el botón recupera la fecha del servidor con el formato especificado ([haga clic para ver la imagen a tamaño completo](dynamically-populating-a-control-using-javascript-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-cs.md)
> [Siguiente](using-dynamicpopulate-with-a-user-control-and-javascript-cs.md)
