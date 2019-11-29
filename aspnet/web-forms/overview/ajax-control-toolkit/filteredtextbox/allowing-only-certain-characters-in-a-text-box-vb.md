---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
title: Permitir solo determinados caracteres en un cuadro de texto (VB) | Microsoft Docs
author: wenz
description: Los controles de validación de ASP.NET pueden garantizar que solo se permiten determinados caracteres en los datos proporcionados por el usuario. Sin embargo, esto sigue sin impedir que los usuarios escriban no válidos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 33af23f1-4016-4740-8fb2-37d1773452cd
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-vb
msc.type: authoredcontent
ms.openlocfilehash: 895708ebecc30c5f35e6ecd0349604bb777cbd93
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74573930"
---
# <a name="allowing-only-certain-characters-in-a-text-box-vb"></a>Permitir solo determinados caracteres en un cuadro de texto (VB)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.vb.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0VB.pdf)

> Los controles de validación de ASP.NET pueden garantizar que solo se permiten determinados caracteres en los datos proporcionados por el usuario. Sin embargo, esto sigue sin impedir que los usuarios escriban caracteres no válidos y intenten enviar el formulario.

## <a name="overview"></a>Información general del

Los controles de validación de ASP.NET pueden garantizar que solo se permiten determinados caracteres en los datos proporcionados por el usuario. Sin embargo, esto sigue sin impedir que los usuarios escriban caracteres no válidos y intenten enviar el formulario.

## <a name="steps"></a>Pasos

El kit de herramientas de control de AJAX de ASP.NET contiene el control `FilteredTextBox` que extiende un cuadro de texto. Una vez activada, solo se puede escribir en el campo un determinado conjunto de caracteres.

Para que esto funcione, primero necesitamos como habitual el ASP.NET AJAX `ScriptManager` que carga las bibliotecas de JavaScript que también usa ASP.NET AJAX control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample1.aspx)]

A continuación, necesitamos un cuadro de texto:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample2.aspx)]

Por último, el control de `FilteredTextBoxExtender` se encarga de restringir los caracteres que el usuario puede escribir. En primer lugar, establezca el atributo `TargetControlID` en el `ID` del control `TextBox`. A continuación, elija uno de los valores de `FilterType` disponibles:

- `Custom` valor predeterminado; debe proporcionar una lista de caracteres válidos.
- `LowercaseLetters` solo letras minúsculas
- solo dígitos de `Numbers`
- solo `UppercaseLetters` letras mayúsculas

Si se utiliza el `Custom FilterType`, se debe establecer la propiedad `ValidChars` y proporcionar una lista de caracteres que se pueden escribir. Por la forma: Si intenta pegar texto en el cuadro de texto, se quitan todos los caracteres no válidos.

Este es el marcado del control de `FilteredTextBoxExtender` que solo permite dígitos (algo que también habría sido posible con `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-vb/samples/sample3.aspx)]

Ejecute la página e intente escribir una letra si JavaScript está habilitado, no funcionará; sin embargo, los dígitos aparecen en la página. Sin embargo, tenga en cuenta que la protección `FilteredTextBox` proporciona no es una prueba de viñetas: si JavaScript está habilitado, es posible que se escriban datos en el cuadro de texto, por lo que tendrá que usar medias de validación adicionales, es decir, ASP. Controles de validación de la red.

[![solo se pueden especificar dígitos](allowing-only-certain-characters-in-a-text-box-vb/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-vb/_static/image1.png)

Solo se pueden escribir dígitos ([haga clic para ver la imagen de tamaño completo](allowing-only-certain-characters-in-a-text-box-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Anterior](allowing-only-certain-characters-in-a-text-box-cs.md)
