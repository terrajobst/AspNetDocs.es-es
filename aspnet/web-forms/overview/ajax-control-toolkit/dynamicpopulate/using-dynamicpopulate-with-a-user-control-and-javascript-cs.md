---
uid: web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
title: Usar DynamicPopulate con un control de usuario y JavaScriptC#() | Microsoft Docs
author: wenz
description: El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino en t...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 38ac8250-8854-444c-b9ab-8998faa41c5a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dynamicpopulate/using-dynamicpopulate-with-a-user-control-and-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: a0e6d04a5f62ab558aceb8302d94d3bf2dc8a39f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78497335"
---
# <a name="using-dynamicpopulate-with-a-user-control-and-javascript-c"></a>Usar DynamicPopulate con un control de usuario y JavaScript (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/d/8/f/d8f2f6f9-1b7c-46ad-9252-e1fc81bdea3e/dynamicpopulate2.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dynamicpopulate2CS.pdf)

> El control DynamicPopulate en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página. También se puede desencadenar el rellenado mediante código JavaScript del lado cliente personalizado. Sin embargo, se debe tener especial cuidado cuando el extensor resida en un control de usuario.

## <a name="overview"></a>Información general

El control `DynamicPopulate` en el kit de herramientas de control de AJAX de ASP.NET llama a un servicio Web (o método de página) y rellena el valor resultante en un control de destino de la página, sin necesidad de actualizar la página. También se puede desencadenar el rellenado mediante código JavaScript del lado cliente personalizado. Sin embargo, se debe tener especial cuidado cuando el extensor resida en un control de usuario.

## <a name="steps"></a>Pasos

En primer lugar, necesita un servicio Web ASP.NET que implemente el método al que llamará el control `DynamicPopulateExtender`. El servicio Web implementa el método `getDate()` que espera un argumento de tipo cadena, denominado `contextKey`, ya que el control `DynamicPopulate` envía un fragmento de información de contexto con cada llamada de servicio Web. Este es el código (`DynamicPopulate.cs.asmx`de archivos), que recupera la fecha actual en uno de los tres formatos siguientes:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample1.aspx)]

En el paso siguiente, cree un nuevo control de usuario (archivo`.ascx`), indicado por la siguiente declaración en la primera línea:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample2.aspx)]

Se usará un &lt;`label`elemento &gt; para mostrar los datos procedentes del servidor.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample3.aspx)]

Además, en el archivo de control de usuario, usaremos tres botones de radio, cada uno de los cuales representa uno de los tres formatos de fecha posibles admitidos por el servicio Web. Cuando el usuario hace clic en uno de los botones de radio, el explorador ejecutará código JavaScript que tiene este aspecto:

[!code-powershell[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample4.ps1)]

Este código tiene acceso a la `DynamicPopulateExtender` (no se preocupe todavía por el identificador extraño, esto se tratará más adelante) y desencadenará el rellenado dinámico con datos. En el contexto del botón de radio actual, `this.value` hace referencia a su valor que es `format1`, `format2` o `format3` exactamente lo que espera el método Web.

Lo único que falta en el control de usuario es el `DynamicPopulateExtender` control que vincula los botones de radio al servicio Web.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample5.aspx)]

También puede anotar el identificador extraño usado en el control: `mcd1$myDate` en lugar de `myDate`. Anteriormente, el código de JavaScript usaba `mcd1_dpe1` para tener acceso al `DynamicPopulateExtender` en lugar de `dpe1`. Esta estrategia de nomenclatura es un requisito especial al usar `DynamicPopulateExtender` dentro de un control de usuario. Además, tiene que incrustar el control de usuario de una manera específica para que todo funcione. Cree una nueva página de ASP.NET y registre un prefijo de etiqueta para el control de usuario que acaba de implementar:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample6.aspx)]

Después, incluya el control ASP.NET de `ScriptManager` AJAX en la página nueva:

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample7.aspx)]

Por último, agregue el control de usuario a la página. Solo tiene que establecer su `ID` atributo (y `runat="server"`, por supuesto), pero también tiene que establecerlo en un nombre específico: `mcd1` ya que este es el prefijo que se usa en el control de usuario para tener acceso a él mediante JavaScript.

[!code-aspx[Main](using-dynamicpopulate-with-a-user-control-and-javascript-cs/samples/sample8.aspx)]

Y listo. La página se comporta de la manera esperada: cuando un usuario hace clic en uno de los botones de radio, el control del kit de herramientas llama al servicio Web y muestra la fecha actual en el formato deseado.

[![de los botones de radio de un control de usuario](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image2.png)](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image1.png)

Los botones de radio residen en un control de usuario ([haga clic para ver la imagen de tamaño completo](using-dynamicpopulate-with-a-user-control-and-javascript-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](dynamically-populating-a-control-using-javascript-code-cs.md)
> [Siguiente](dynamically-populating-a-control-vb.md)
