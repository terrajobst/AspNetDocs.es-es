---
uid: web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
title: Permitir solo determinados caracteres en un cuadro de textoC#() | Microsoft Docs
author: wenz
description: Los controles de validación de ASP.NET pueden garantizar que solo se permiten determinados caracteres en los datos proporcionados por el usuario. Sin embargo, esto sigue sin impedir que los usuarios escriban no válidos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: fd2a1c52-d717-44af-8a61-67c8279bb26e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/filteredtextbox/allowing-only-certain-characters-in-a-text-box-cs
msc.type: authoredcontent
ms.openlocfilehash: d1e367becd574e31d24fca8545f76b1ed3c4d85e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2019
ms.locfileid: "74611752"
---
# <a name="allowing-only-certain-characters-in-a-text-box-c"></a>Permitir solo determinados caracteres en un cuadro de texto (C#)

por [Christian Wenz](https://github.com/wenz)

[Descargar código](https://download.microsoft.com/download/4/c/2/4c2def7a-0d23-4055-91f9-1f18504167d7/FilteredTextBox0.cs.zip) o [Descargar PDF](https://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/filteredtextbox0CS.pdf)

> Los controles de validación de ASP.NET pueden garantizar que solo se permiten determinados caracteres en los datos proporcionados por el usuario. Sin embargo, esto sigue sin impedir que los usuarios escriban caracteres no válidos y intenten enviar el formulario.

## <a name="overview"></a>Información general del

Los controles de validación de ASP.NET pueden garantizar que solo se permiten determinados caracteres en los datos proporcionados por el usuario. Sin embargo, esto sigue sin impedir que los usuarios escriban caracteres no válidos y intenten enviar el formulario.

## <a name="steps"></a>Pasos

El kit de herramientas de control de AJAX de ASP.NET contiene el control `FilteredTextBox` que extiende un cuadro de texto. Una vez activada, solo se puede escribir en el campo un determinado conjunto de caracteres.

Para que esto funcione, primero necesitamos como habitual el ASP.NET AJAX `ScriptManager` que carga las bibliotecas de JavaScript que también usa ASP.NET AJAX control Toolkit:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample1.aspx)]

A continuación, necesitamos un cuadro de texto:

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample2.aspx)]

Por último, el control de `FilteredTextBoxExtender` se encarga de restringir los caracteres que el usuario puede escribir. En primer lugar, establezca el atributo `TargetControlID` en el `ID` del control `TextBox`. A continuación, elija uno de los valores de `FilterType` disponibles:

- `Custom` valor predeterminado; debe proporcionar una lista de caracteres válidos.
- `LowercaseLetters` solo letras minúsculas
- solo dígitos de `Numbers`
- solo `UppercaseLetters` letras mayúsculas

Si se utiliza el `Custom FilterType`, se debe establecer la propiedad `ValidChars` y proporcionar una lista de caracteres que se pueden escribir. Por la forma: Si intenta pegar texto en el cuadro de texto, se quitan todos los caracteres no válidos.

Este es el marcado del control de `FilteredTextBoxExtender` que solo permite dígitos (algo que también habría sido posible con `FilterType="Numbers"`):

[!code-aspx[Main](allowing-only-certain-characters-in-a-text-box-cs/samples/sample3.aspx)]

Ejecute la página e intente escribir una letra si JavaScript está habilitado, no funcionará; sin embargo, los dígitos aparecen en la página. Sin embargo, tenga en cuenta que la protección `FilteredTextBox` proporciona no es una prueba de viñetas: si JavaScript está habilitado, es posible que se escriban datos en el cuadro de texto, por lo que tendrá que usar medias de validación adicionales, es decir, ASP. Controles de validación de la red.

[![solo se pueden especificar dígitos](allowing-only-certain-characters-in-a-text-box-cs/_static/image2.png)](allowing-only-certain-characters-in-a-text-box-cs/_static/image1.png)

Solo se pueden escribir dígitos ([haga clic para ver la imagen de tamaño completo](allowing-only-certain-characters-in-a-text-box-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Siguiente](allowing-only-certain-characters-in-a-text-box-vb.md)
