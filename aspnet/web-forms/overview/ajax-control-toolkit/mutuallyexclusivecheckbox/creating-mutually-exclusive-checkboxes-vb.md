---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
title: Crear casillas de verificación mutuamente excluyentes (VB) | Microsoft Docs
author: wenz
description: 'Cuando solo se puede seleccionar uno de los conjuntos de opciones, se suelen usar los botones de radio. Sin embargo, hay un inconveniente: cuando se selecciona un botón de radio en un grupo,...'
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e9dd1d5a-a1db-4114-981d-6a91acb1d709
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-vb
msc.type: authoredcontent
ms.openlocfilehash: f33936dd4d71f6bbf08f02966eefe44c8c152eba
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78446167"
---
# <a name="creating-mutually-exclusive-checkboxes-vb"></a>Crear casillas de verificación mutuamente excluyentes (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0VB.pdf)

> Cuando solo se puede seleccionar uno de los conjuntos de opciones, se suelen usar los botones de radio. Sin embargo, hay un inconveniente: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio. Las casillas pueden desactivarse en cualquier momento; sin embargo, no se excluyen mutuamente. En este tutorial se ofrece lo mejor de ambos enfoques: las casillas que son mutuamente excluyentes.

## <a name="overview"></a>Información general

Cuando solo se puede seleccionar uno de los conjuntos de opciones, se suelen usar los botones de radio. Sin embargo, hay un inconveniente: una vez que se selecciona un botón de radio en un grupo, no es posible desactivar todos los botones de radio. Las casillas pueden desactivarse en cualquier momento; sin embargo, no se excluyen mutuamente. En este tutorial se ofrece lo mejor de ambos enfoques: las casillas que son mutuamente excluyentes.

## <a name="steps"></a>Pasos

El kit de herramientas de control de AJAX de ASP.NET contiene el extensor MutuallyExclusiveCheckBox. Esto permite a los programadores asignar cualquier casilla a un nombre de grupo (`Key` atributo). En todas las casillas del mismo grupo, solo se puede seleccionar una al mismo tiempo.

Comencemos con la colocación de dos casillas en una nueva página de ASP.NET. Puede haber más, pero dos de ellos bastan para demostrar el principio:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample1.aspx)]

En ambas casillas, se debe colocar un control MutuallyExclusiveCheckBoxExtender en la página. Ambos atributos de clave deben tener el mismo valor, al igual que los atributos de valor de los elementos de botón de radio HTML deben ser idénticos para indicar el grupo al que pertenecen. La propiedad TargetControlID del objeto extender apunta al identificador de la casilla.

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample2.aspx)]

Por último, incluya el `ScriptManager` de AJAX de ASP.NET que requieran todos los elementos de ASP.NET AJAX control Toolkit:

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-vb/samples/sample3.aspx)]

Guardar y ejecutar la página: puede activar y desactivar ambas casillas; sin embargo, en ningún momento puede activar ambas casillas.

[![solo se puede comprobar una casilla a la vez](creating-mutually-exclusive-checkboxes-vb/_static/image2.png)](creating-mutually-exclusive-checkboxes-vb/_static/image1.png)

Solo se puede comprobar una casilla a la vez ([haga clic para ver la imagen de tamaño completo](creating-mutually-exclusive-checkboxes-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](creating-mutually-exclusive-checkboxes-cs.md)
