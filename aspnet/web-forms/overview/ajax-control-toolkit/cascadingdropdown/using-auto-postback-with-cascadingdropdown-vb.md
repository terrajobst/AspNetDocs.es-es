---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
title: Usar postback automático con CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en Anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0b34f7f6-a0cc-4b9f-9761-643fb0bb3ece
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-auto-postback-with-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 5dea23a20aba00af5109f05f18365b89e409a131
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74574453"
---
# <a name="using-auto-postback-with-cascadingdropdown-vb"></a>Usar el postback automático con CascadingDropDown (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown3.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown3VB.pdf)

> El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList. Sin embargo, cuando se usa el control CascadingDropDown, ASP. La característica de postback del control DropDownList de la red no funciona, puesto que la carga de datos de forma asincrónica en la lista genera un postback (innecesario). Con algún código JavaScript, se puede evitar este efecto.

## <a name="overview"></a>Información general del

El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList. (Por ejemplo, una lista proporciona una lista de Estados de EE. UU. y la siguiente lista se rellena con las ciudades principales en ese estado). Sin embargo, cuando se usa el control CascadingDropDown, ASP. La característica de postback del control DropDownList de la red no funciona, puesto que la carga de datos de forma asincrónica en la lista genera un postback (innecesario). Con algún código JavaScript, se puede evitar este efecto.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de ASP.NET AJAX y el kit de herramientas de control, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del &lt;`form`elemento &gt;):

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample1.aspx)]

A continuación, se requiere un control DropDownList:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample2.aspx)]

En esta lista, se agrega un extensor CascadingDropDown, que proporciona la dirección URL del servicio Web y la información del método:

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample3.aspx)]

A continuación, el extensor CascadingDropDown llama de forma asincrónica a un servicio Web con la siguiente firma de método:

[!code-vb[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample4.vb)]

El método devuelve una matriz de tipo CascadingDropDown Value. El constructor del tipo espera primero el título de la entrada de la lista y, a continuación, el valor (atributo de `value` HTML).

[!code-aspx[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample5.aspx)]

La carga de la página en el explorador rellenará la lista desplegable con tres proveedores y la segunda se preseleccionará. Además, ASP.NET define el método JavaScript `__doPostBack()`. Una vez cargada la página, esta llamada de JavaScript se agrega a la lista desplegable, pero solo si hay elementos en ella. Si no hay ningún elemento en la lista, el kit de herramientas de control lo está cargando actualmente, por lo que el código de JavaScript usa un tiempo de espera y vuelve a intentarlo en un medio segundo.

[!code-html[Main](using-auto-postback-with-cascadingdropdown-vb/samples/sample6.html)]

De este modo, solo se ejecuta un postback cuando realmente hay elementos en la lista y el usuario selecciona una entrada.

[![seleccionar un elemento de lista provoca un postback](using-auto-postback-with-cascadingdropdown-vb/_static/image2.png)](using-auto-postback-with-cascadingdropdown-vb/_static/image1.png)

Al seleccionar un elemento de lista, se produce un postback ([hacer clic para ver la imagen de tamaño completo](using-auto-postback-with-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](presetting-list-entries-with-cascadingdropdown-vb.md)
