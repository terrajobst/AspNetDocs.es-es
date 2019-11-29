---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
title: Rellenar una lista con CascadingDropDown (VB) | Microsoft Docs
author: wenz
description: El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en Anoth...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 5236695e-5c70-4887-baee-0bfb0afb3448
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/filling-a-list-using-cascadingdropdown-vb
msc.type: authoredcontent
ms.openlocfilehash: 8dd9ef8a4bdf705ba4451b7fd240e4de8618221c
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74599593"
---
# <a name="filling-a-list-using-cascadingdropdown-vb"></a>Rellenar una lista con CascadingDropDown (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown0.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown0VB.pdf)

> El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList. (Por ejemplo, una lista proporciona una lista de Estados de EE. UU. y la siguiente lista se rellena con las ciudades principales en ese estado). El primer desafío que resolver es realmente rellenar una lista desplegable con este control.

## <a name="overview"></a>Información general del

El control CascadingDropDown en el kit de herramientas de control de AJAX extiende un control DropDownList para que los cambios en un objeto DropDownList carguen los valores asociados en otro DropDownList. (Por ejemplo, una lista proporciona una lista de Estados de EE. UU. y la siguiente lista se rellena con las ciudades principales en ese estado). El primer desafío que resolver es realmente rellenar una lista desplegable con este control.

## <a name="steps"></a>Pasos

Para activar la funcionalidad de ASP.NET AJAX y control Toolkit, el control `ScriptManager` debe colocarse en cualquier parte de la página (pero dentro del elemento `<form>`):

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample1.aspx)]

A continuación, se requiere un control DropDownList:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample2.aspx)]

En esta lista, se agrega un extensor CascadingDropDown. Enviará una solicitud asincrónica a un servicio Web que, a continuación, devolverá una lista de entradas que se mostrarán en la lista. Para que esto funcione, es necesario establecer los siguientes atributos de CascadingDropDown:

- `ServicePath`: dirección URL de un servicio Web que entrega las entradas de la lista
- `ServiceMethod`: método Web que entrega las entradas de la lista
- `TargetControlID`: ID. de la lista desplegable
- `Category`: información de categoría que se envía al método Web cuando se llama
- `PromptText`: texto que se muestra cuando se cargan datos de lista de forma asincrónica desde el servidor

Este es el marcado del elemento `CascadingDropDown`. La única diferencia entre C# y VB es el nombre del servicio web asociado:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample3.aspx)]

El código JavaScript procedente del extensor `CascadingDropDown` llama a un método de servicio Web con la siguiente firma:

[!code-vb[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample4.vb)]

Por lo tanto, el aspecto importante es que el método debe devolver una matriz de tipo `CascadingDropDownNameValue` (definida por el kit de herramientas de control de AJAX de ASP.NET). En el constructor `CascadingDropDownNameValue`, primero se debe proporcionar el texto de la entrada de la lista y, a continuación, su valor, tal como lo haría `<option value="VALUE">NAME</option>` en HTML. Estos son algunos datos de ejemplo:

[!code-aspx[Main](filling-a-list-using-cascadingdropdown-vb/samples/sample5.aspx)]

Si carga la página en el explorador, la lista se rellenará con tres proveedores.

[![la lista se rellena automáticamente](filling-a-list-using-cascadingdropdown-vb/_static/image2.png)](filling-a-list-using-cascadingdropdown-vb/_static/image1.png)

La lista se rellena automáticamente ([haga clic para ver la imagen de tamaño completo](filling-a-list-using-cascadingdropdown-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](using-auto-postback-with-cascadingdropdown-cs.md)
> [Siguiente](using-cascadingdropdown-with-a-database-vb.md)
